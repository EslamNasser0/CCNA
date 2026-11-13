# Day 2

## Lab 1
Packet Tracer - Configure SSH

![Packet Tracer - Configure SSH](https://itexamanswers.net/wp-content/uploads/2020/02/1.3.6-Packet-Tracer-Configure-SSH.jpg)

### Objectives

 - Part 1: Secure Passwords
 - Part 2: Encrypt Communications
 - Part 3: Verify SSH Implementation

### SWITCH S1
``` bash
! ==============================================================
!--- Usage: from the PC1 Command Prompt, Telnet to S1 (telnet 10.10.10.2) and log in with the password "cisco", type "enable" and enter the privileged EXEC password "cisco", then paste this whole file at the S1# prompt. Every line beginning with "!" is a comment; IOS ignores it, so pasting a comment by accident is harmless.
!--- S1 is already configured with a hostname, a VLAN 1 management address and Telnet access. This script hardens it: it encrypts the passwords and replaces Telnet with SSH.
! ==============================================================

! --------------------------------------------------------------
!--- Part 1, Step b: Save the current configuration FIRST, so any mistake can be undone by power-cycling S1.
! --------------------------------------------------------------

copy running-config startup-config

configure terminal

! --------------------------------------------------------------
!--- Part 1, Step c: Secure the passwords.
! --------------------------------------------------------------

!--- Encrypt all plaintext passwords. Before this command, 'show running-config' displays the passwords in clear text; afterwards they appear as type 7 hashes.
service password-encryption

! --------------------------------------------------------------
!--- Part 2, Step 1: Set the IP domain name and generate the RSA keys.
! --------------------------------------------------------------

!--- Set the domain name. Together with the hostname it forms the RSA key name (S1.netacad.pka), so the domain name MUST be set before the crypto command below.
ip domain-name netacad.pka

!--- Generate the 1024-bit RSA key pair that SSH uses to encrypt the session.
crypto key generate rsa general-keys modulus 1024

! --------------------------------------------------------------
!--- Part 2, Step 2: Create an SSH user and lock the VTY lines down to SSH only.
! --------------------------------------------------------------

!--- Create the local user that SSH will authenticate against.
username administrator secret cisco

!--- On the VTY lines: authenticate against the local user database, accept ONLY SSH (this is what makes the later Telnet attempt fail), and remove the old shared line password, which is no longer used once 'login local' is active.
line vty 0 15
 login local
 transport input ssh
 no password cisco
 exit

end

! --------------------------------------------------------------
!--- Part 3, Step c: Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification (Part 3, from the PC1 Command Prompt):
!---   telnet 10.10.10.2               -> connection opens, then is closed by S1 (transport input ssh)
!---   ssh                             -> shows the command usage; -l is the letter L, not the digit 1
!---   ssh -l administrator 10.10.10.2 -> login succeeds with password cisco
!--- On S1:
!---   show ip ssh                     -> SSH enabled, RSA key present
!---   show running-config             -> passwords now stored as type 7, VTY set to SSH only
! ==============================================================
```

## Lab 2

Packet Tracer - Switch Basic Config-Challenge Lab

![Packet Tracer - Switch Basic Config-Challenge Lab](https://itexamanswers.net/wp-content/uploads/2026/08/Topo-1.png)

### Objectives

 - Configure hostnames and IP addresses on two Cisco Internetwork Operating System (IOS) switches using the command-line interface (CLI).
 - Use Cisco IOS commands to specify or limit access to the device configurations.
 - Use IOS commands to save the running configuration.
 - Configure two host devices with IP addresses.
 - Verify connectivity between the two PC end devices.

### Scenario

As a recently hired LAN technician, your network manager has asked you to demonstrate your ability to configure a small LAN. Your tasks include configuring initial settings on two switches using the Cisco IOS and configuring IP address parameters on host devices to provide end-to-end connectivity. You are to use two switches and two hosts/PCs on a cabled and powered network.

### Requirements

 - Use a console connection to access each switch.
 - Name `Class-A` and `Class-B` switches.
 - Use the **R4Xe3** password for all lines.
 - Use the **C4aJa** secret password.
 - Encrypt all clear text passwords.
 - Include the word **warning** in the message-of-the-day (MOTD) Banner.
 - Configure addressing for all devices according to the Addressing Table.
 - Save your configurations.
 - Verify connectivity between all devices.

ID:0021



### SWITCH Class-A
``` bash
! ===== Device: S1 (switch) =====

enable
configure terminal

! ----- Basic settings -----
!--- Name the switch.
hostname Class-A

!--- Use the secret password for privileged EXEC mode.
enable secret C4aJa

!--- Encrypt all clear text passwords.
service password-encryption

!--- Configure an appropriate MOTD banner.
banner motd $ warning authorized access only. All activity may be monitored and reported. $

! ----- Console line -----
line console 0
!--- Use the line password for the console.
password R4Xe3
login
exit

! ----- VTY lines (remote/Telnet access) -----
line vty 0 15
!--- Use the line password for all vty lines.
password R4Xe3
login
exit

! ----- Management addressing (SVI) -----
interface vlan 1
!--- Configure addressing per the Addressing Table.
ip address 10.10.10.100 255.255.255.0
no shutdown
exit

end

!--- Save the running configuration.
copy running-config startup-config

! ===== Verification (reference only, not part of paste) =====
! show running-config
! show ip interface brief
! ping 10.10.10.150
```

### SWITCH Class-B
``` bash
! ===== 2.9.1 Packet Tracer - Basic Switch and End Device Configuration =====
! ===== Device: S2 (switch) =====

enable
configure terminal

! ----- Basic settings -----
!--- Name the switch.
hostname Class-B

!--- Use the secret password for privileged EXEC mode.
enable secret C4aJa

!--- Encrypt all clear text passwords.
service password-encryption

!--- Configure an appropriate MOTD banner.
banner motd $ warning authorized access only. All activity may be monitored and reported. $

! ----- Console line -----
line console 0
!--- Use the line password for the console.
password R4Xe3
login
exit

! ----- VTY lines (remote/Telnet access) -----
line vty 0 15
!--- Use the line password for all vty lines.
password R4Xe3
login
exit

! ----- Management addressing (SVI) -----
interface vlan 1
!--- Configure addressing per the Addressing Table.
ip address 10.10.10.150 255.255.255.0
no shutdown
exit

end

!--- Save the running configuration.
copy running-config startup-config

! ===== Verification (reference only, not part of paste) =====
! show running-config
! show ip interface brief
! ping 10.10.10.100
```

### PC Student-1
``` bash
ipconfig 10.10.10.4 255.255.255.0
```

### PC Student-2
``` bash
ipconfig 10.10.10.5 255.255.255.0
```

