
### Base Linux machine toolkit: 

1. [BloodHound.py](https://github.com/fox-it/BloodHound.py) & [BloodHound GUI](https://github.com/BloodHoundAD/BloodHound) - Tool for enumerating Active Directory and creating graphical representations of possible attack paths. Check out the [Active Directory BloodHound](https://academy.hackthebox.com/course/preview/active-directory-bloodhound) module for more on these tools.
    
2. [Impacket Toolkit](https://github.com/fortra/impacket) - Various scripts for interacting with Active Directory, from enumeration and attacks to remote access and everything in between.
    
3. [Kerbrute](https://github.com/ropnop/kerbrute) - Tool for enumerating valid Active Directory usernames and performing password spraying attacks.
    
4. [Responder](https://github.com/lgandx/Responder)/[Pretender](https://github.com/RedTeamPentesting/pretender) - Tools for performing poisoning attacks against various network protocols to gather password hashes for relaying attacks or offline password cracking.
    
5. [CrackMapExec](https://github.com/Porchetta-Industries/CrackMapExec) - A multi-use Active Directory enumeration and attack tool that can be used with various protocols, including SMB, WinRM, LDAP, RDP, and more. It contains many modules for enumerating and attacking individual Windows hosts and Active Directory environments. There is an excellent module, [Using CrackMapExec](https://academy.hackthebox.com/course/preview/using-crackmapexec), on HTB Academy for practicing with this tool. 
    
6. [Rubeus](https://github.com/GhostPack/Rubeus) - A C# toolkit for interacting with and abusing the Kerberos protocol.
    
7. [Certipy](https://github.com/ly4k/Certipy) - A tool for enumerating and attacking Active Directory Certificate Services (AD CS).
    
8. [PKINITtools](https://github.com/dirkjanm/PKINITtools/) - Various small tools for working with PKINIT (a pre-authentication mechanism for Kerberos 5). They are handy when performing attacks against AD CS.
    
9. [Hashcat](https://hashcat.net/hashcat/) - A tool for performing offline password cracking. Very useful for the various types of password hashes we will collect during an AD pentesting engagement. Practice this powerful tool via the [Cracking Password with Hashcat](https://academy.hackthebox.com/course/preview/cracking-passwords-with-hashcat) module on HTB Academy.
    
10. [Evil-winrm](https://github.com/Hackplayers/evil-winrm) - A powerful tool for remotely authenticating to hosts via WinRM using an NTLM password hash, cleartext password, or Kerberos authentication.