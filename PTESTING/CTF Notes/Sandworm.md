```
atlas@sandworm:/var/www/html/SSA/SSA$ cat /etc/passwd
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
fwupd-refresh:x:113:118:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
mysql:x:114:120:MySQL Server,,,:/nonexistent:/bin/false
silentobserver:x:1001:1001::/home/silentobserver:/bin/bash
atlas:x:1000:1000::/home/atlas:/bin/bash
_laurel:x:997:997::/var/log/laurel:/bin/false
```

```python
from flask import Flask, render_template, Response, flash, request, Blueprint, redirect, flash, url_for, render_template_string, jsonify
from flask_login import login_required, login_user, logout_user
from werkzeug.security import check_password_hash
import hashlib
from . import db
import os
from datetime import datetime
import gnupg
from SSA.models import User

main = Blueprint('main', __name__)

gpg = gnupg.GPG(gnupghome='/home/atlas/.gnupg', options=['--ignore-time-conflict'])

@main.route("/")
def home():
    return render_template("index.html", name="home")

@main.route("/about")
def about():
    return render_template("about.html", name="about")

@main.route("/contact", methods=('GET', 'POST',))
def contact():
    if request.method == 'GET':
        return render_template("contact.html", name="contact")
    tip = request.form['encrypted_text']
    if not validate(tip):
        return render_template("contact.html", error_msg="Message is not PGP-encrypted.")

    msg = gpg.decrypt(tip, passphrase='$M1DGu4rD$')

    if msg.data == b'':
        msg = 'Message was encrypted with an unknown PGP key.'
    else:
        tip = msg.data.decode('utf-8')
        msg = "Thank you for your submission."

    save(tip, request.environ.get('HTTP_X_REAL_IP', request.remote_addr))
    return render_template("contact.html", error_msg=msg)


@main.route("/guide", methods=('GET', 'POST'))
def guide():
    if request.method == 'GET':
        return render_template("study.html", name="guide")

    elif request.method == 'POST':
        encrypted = request.form['encrypted_text']
    
        if not validate(encrypted):
            pass
    
        msg = gpg.decrypt(encrypted, passphrase='$M1DGu4rD$')
    
        if msg.data == b'':
            msg = 'Message was encrypted with an unknown PGP key.'
        else:
            msg = msg.data.decode('utf-8')
     
        return render_template("study.html", name="guide", dec_msg=msg)

@main.route("/guide/encrypt", methods=('GET', 'POST',))
def encrypt():
    if request.method == 'GET':
        return render_template("study.html")

    pubkey = request.form['pub_key']

    import_result = gpg.import_keys(pubkey)

    if import_result.count == 0:
        return render_template("study.html", error_msg_pub="Invalid key format.")

    fp = import_result.fingerprints[0]
    now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") 
    key_uid = ', '.join([key['uids'] for key in gpg.list_keys() if key['fingerprint'] == fp][0])
    message = f"""This is an encrypted message for {key_uid}.\n\nIf you can read this, it means you successfully used your private PGP key to decrypt a message meant for you and only you.\n\nCongratulations! Feel free to keep practicing, and make sure you also know how to encrypt, sign, and verify messages to make your repertoire complete.\n\nSSA: {now}"""
    enc_msg = gpg.encrypt(message, recipients=fp, always_trust=True)

    if not enc_msg.ok:
        return render_template("study.html", error_msg="Something went wrong.")

    return render_template("study.html", enc_msg=enc_msg)

@main.route("/guide/verify", methods=('GET', 'POST',))
def verify():
    if request.method == 'GET':
        return render_template("study.html")

    signed = request.form['signed_text']
    pubkey = request.form['public_key']

    if signed and pubkey:
        import_result = gpg.import_keys(pubkey)
        if import_result.count == 0:
            return render_template("study.html", error_msg_key="Key import failed. Make sure your key is properly formatted.")
        else:
            fp = import_result.fingerprints
            verified = gpg.verify(signed)
            if verified.status == 'signature valid':
                msg = f"Signature is valid!\n\n{verified.stderr}"
            else:
                msg = "Make sure your signed message is properly formatted."

            # Cleanup - delete key
            gpg.delete_keys(fp)
            return render_template("study.html", error_msg_sig=msg)

    return render_template("study.html", error_msg_key="Something went wrong.")

@main.route("/process", methods=("POST",))
def process_form():
    signed = request.form['signed_text']
    pubkey = request.form['public_key']

    if signed and pubkey:
        import_result = gpg.import_keys(pubkey)
        if import_result.count == 0:
            msg = "Key import failed. Make sure your key is properly formatted."
        else:
            fp = import_result.fingerprints
            verified = gpg.verify(signed)
            if verified.status == 'signature valid':
                msg = f"Signature is valid!\n\n{verified.stderr}"
            else:
                msg = "Make sure your signed message is properly formatted."
            # Cleanup - delete key
            gpg.delete_keys(fp)

    return render_template_string(msg)


@main.route("/pgp")
def pgp():
    return render_template("pgp.html", name="pgp")

@main.route("/admin")
@login_required
def admin():
    entries = []
    with open('SSA/submissions/log', 'r') as f:
        for i, line in enumerate(f):
            if i <= 7:
                continue
            ip, fname, dtime = line.strip().split(":")
            entries.append({
                'id': i-7,
                'ip': ip,
                'fname': fname,
                'dtime': dtime
                })

    return render_template("admin.html", name="admin", entries=entries)

@main.route("/view", methods=('GET', 'POST',))
@login_required
def view():
    fname = request.args.get('fname')

    try:
        if not fname.endswith('.txt'):
            flask.abort(400)
        with open(f"SSA/submissions/{fname}", 'r') as f:
            msg = f.read()
    except Exception as _:
        msg = 'Something went wrong.'

    return render_template("view.html", name="view", dec_msg=msg)

@main.route("/login", methods=('GET', 'POST'))
def login():
    if request.method == 'GET':
        return render_template("login.html", name="login")
    
    uname = request.form['username']
    pwd = request.form['password']

    user = User.query.filter_by(username=uname).first()

    if not user or not check_password_hash(user.password, pwd):
        flash('Invalid credentials.')
        return redirect(url_for('main.login'))

    login_user(user, remember=True)

    return redirect(url_for('main.admin'))

@main.route("/logout")
@login_required
def logout():
    logout_user()
    return redirect(url_for('main.home'))

def validate(msg):
    if msg[:27] == '-----BEGIN PGP MESSAGE-----' and msg[-27:].strip() == '-----END PGP MESSAGE-----':
        return True
    return False

def save(msg, ip):
    fname = os.urandom(16).hex() + ".txt"
    now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") 
    with open("SSA/submissions/log", "a") as f:
        f.write(f"{ip}:{fname}:{now}\n")
    with open(f"SSA/submissions/{fname}", "w") as f:
        f.write(msg)
```