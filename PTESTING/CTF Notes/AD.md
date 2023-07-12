
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


### Windows box tools:

1. [Mimikatz](https://github.com/gentilkiwi/mimikatz) - A very powerful tool for extracting password hashes and other credential information from Windows hosts. It can be used in various AD attacks. 
    
2. [PingCastle](https://www.pingcastle.com/) - An excellent tool for auditing Active Directory security from the top down. Sometimes finds issues that other tools miss. Useful for recommending further AD hardening steps for customers in their pentest report. 
    
3. [Certify](https://github.com/GhostPack/Certify) - A C# tool for enumerating and attacking AD CS, similar to Certipy.
    
4. [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) - A powerful PowerShell tool for enumerating and attacking AD. It can be used to dig very deep into various AD objects and for some attacks (i.e., Keberoasting). This tool is no longer supported by the authors in favor of C# tradecraft but remains a powerful and extremely handy tool to keep in your arsenal. A [fork](https://exploit.ph/powerview.html) of the tool was made to add additional features, and it is worth checking out. Dig deep into this tool with the [Active Directory PowerView](https://academy.hackthebox.com/course/preview/active-directory-powerview) module on HTB Academy. 
    
5. [SharpHound](https://github.com/BloodHoundAD/SharpHound) - The C# data collector for the BloodHound tool. There is also a [PowerShell version](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1) and [AzureHound](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/AzureHound.md) for enumerating Azure environments. Check out the [Active Directory BloodHound](https://academy.hackthebox.com/course/preview/active-directory-bloodhound) module for in-depth training on these collectors and the BloodHound GUI tool. 
    

As mentioned in the descriptions, some of these tools have dedicated modules on HTB Academy; others are covered in-depth in modules such as [Active Directory Enumeration & Attacks](https://academy.hackthebox.com/course/preview/active-directory-enumeration--attacks). Stay tuned for many more AD-focused modules on HTB Academy as well!

We will discuss the “why” behind each of these tools in the next section, where we will see several sample approaches for starting a penetration test in an AD environment with and without being provided credentials by the client. Every environment is different, so certain tools may not be necessary, and everyone has their own style/approach, but these can be thought of as “high percentage” tools and techniques that will be employed more often than not.

## An overview of the Active Directory enumeration and pentesting process

Methodologies for attacking Active Directory will vary from pentester to pentester, but one thing that will be true across all internal assessments is that we will start from either: 

- An uncredentialed standpoint: No AD user account and just an internal network connection.
    
- A credentialed standpoint: Typically a low privileged, standard AD user account. 
    

So depending on how the client wants us to start the assessment, our initial steps will differ. Beginning with an AD user account is generally “easier” because we can move right into enumerating the vast attack surface that is AD. But, lucky for us, there are multiple ways to obtain a valid AD user account.