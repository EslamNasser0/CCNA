# Day 3

## Packet Tracer - Configuring VLANs

![Packet Tracer - Configuring VLANs](https://itexamanswers.net/wp-content/uploads/2020/02/3.4.5-Packet-Tracer-Configure-Trunks.jpg)

### Objectives

 - Part 1: Verify the Default VLAN Configuration
 - Part 2: Configure VLANs
 - Part 3: Assign VLANs to Ports


### SWITCH S1
``` bash
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 2, Step 1: Create and name VLANs on S1.
! --------------------------------------------------------------

!--- Create the following VLANs. Names are case-sensitive:

!--- VLAN 10: Faculty/Staff
!--- VLAN 20: Students
!--- VLAN 30: Guest(Default)
!--- VLAN 99: Management&Native

VLAN 10
 name Faculty/Staff

VLAN 20
 name Students

VLAN 30
 name Guest(Default)

VLAN 99
 name Management&Native
 exit

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
```

### SWITCH S2
``` bash
! ==============================================================
enable
conf t

! --------------------------------------------------------------
!--- Part 2, Step 3: Create the VLANs on S2 and S3.
! --------------------------------------------------------------

VLAN 10
 name Faculty/Staff

VLAN 20
 name Students

VLAN 30
 name Guest(Default)

VLAN 99
 name Management&Native
 exit

! --------------------------------------------------------------
!--- Part 3, Step 1: Assign VLANs to the active ports on S2.
! --------------------------------------------------------------

! --- VLAN 10: Fast Ethernet 0/11
interface FastEthernet0/6
 switchport access vlan 30
 switchport mode access

! --- VLAN 20: Fast Ethernet 0/18
interface FastEthernet0/11
 switchport access vlan 10
 switchport trunk native vlan 99
 switchport mode access

! --- VLAN 30: Fast Ethernet 0/6
interface FastEthernet0/18
 switchport access vlan 20
 switchport mode access
!
end
! ==============================================================
```

### SWITCH S3
``` bash
! ==============================================================
enable
conf t

! --------------------------------------------------------------
!--- Part 2, Step 3: Create the VLANs on S2 and S3.
! --------------------------------------------------------------

VLAN 10
 name Faculty/Staff

VLAN 20
 name Students

VLAN 30
 name Guest(Default)

VLAN 99
 name Management&Native
 exit

! --------------------------------------------------------------
!--- Part 3, Step 2: Assign VLANs to the active ports on S3.
! --------------------------------------------------------------

! --- VLAN 10: Fast Ethernet 0/11
interface FastEthernet0/6
 switchport access vlan 30
 switchport mode access

! --- VLAN 20: Fast Ethernet 0/18
interface FastEthernet0/11
 switchport access vlan 10
 switchport trunk native vlan 99
 switchport mode access

! --- VLAN 30: Fast Ethernet 0/6
interface FastEthernet0/18
 switchport access vlan 20
 switchport mode access
!
end
! ==============================================================
```

## Lab 2

Packet Tracer - Implement Basic Connectivity

![Packet Tracer - Configure Initial Switch Settings](https://itexamanswers.net/wp-content/uploads/2016/10/2.7.6-Packet-Tracer-Implement-Basic-Connectivity.jpg)

### Objectives

 - Part 1: Perform a Basic Configuration on S1 and S2
 - Part 2: Configure the PCs
 - Part 3: Configure the Switch Management Interface

### SWITCH S1
``` bash
! ==============================================================
!--- Usage: copy this whole file and paste it into the S1 terminal (start at the Switch> prompt). Every line beginning with "!" is a comment; IOS ignores it, so pasting a comment by accident is harmless.
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 1: Perform a Basic Configuration on S1 and S2.
! --------------------------------------------------------------

!--- Configure the hostname.
hostname S1

!--- Secure the console line with the password "cisco" and require login.
line console 0
 password cisco
 login
 exit

!--- Set the encrypted privileged EXEC password (enable secret) to "class".
enable secret class

!--- Configure a MOTD banner to warn against unauthorized access.
banner motd $Authorized access only. Violators will be prosecuted to the full extent of the law.$

! --------------------------------------------------------------
!--- Part 3: Configure the Switch Management Interface.
! --------------------------------------------------------------

!--- Configure the VLAN 1 management interface with its IP address from the Addressing Table and activate it.
interface vlan 1
 ip address 192.168.1.253 255.255.255.0
 no shutdown
 exit

end

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification:
!---   show ip interface brief  -> VLAN 1 up/up with its IP address
!---   show running-config      -> hostname, passwords, banner
!---   ping 192.168.1.254       -> reach the other switch
!---   ping 192.168.1.1         -> reach PC1
!---   ping 192.168.1.2         -> reach PC2
!--- Note: the first ping may show 80%; retry and it should reach 100%
!--- (the first packet is lost while ARP resolves the MAC address).
! ==============================================================
```

### SWITCH S2
``` bash
! ==============================================================
!--- 2.7.6 Packet Tracer - Implement Basic Connectivity
!--- ANSWER SCRIPT FOR SWITCH S2
!--- Usage: copy this whole file and paste it into the S2 terminal (start at the Switch> prompt). Every line beginning with "!" is a comment; IOS ignores it, so pasting a comment by accident is harmless.
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 1: Perform a Basic Configuration on S1 and S2.
! --------------------------------------------------------------

!--- Configure the hostname.
hostname S2

!--- Secure the console line with the password "cisco" and require login.
line console 0
 password cisco
 login
 exit

!--- Set the encrypted privileged EXEC password (enable secret) to "class".
enable secret class

!--- Configure a MOTD banner to warn against unauthorized access.
banner motd $Authorized access only. Violators will be prosecuted to the full extent of the law.$

! --------------------------------------------------------------
!--- Part 3: Configure the Switch Management Interface.
! --------------------------------------------------------------

!--- Configure the VLAN 1 management interface with its IP address from the Addressing Table and activate it.
interface vlan 1
 ip address 192.168.1.254 255.255.255.0
 no shutdown
 exit

end

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification:
!---   show ip interface brief  -> VLAN 1 up/up with its IP address
!---   show running-config      -> hostname, passwords, banner
!---   ping 192.168.1.253       -> reach the other switch
!---   ping 192.168.1.1         -> reach PC1
!---   ping 192.168.1.2         -> reach PC2
!--- Note: the first ping may show 80%; retry and it should reach 100%
!--- (the first packet is lost while ARP resolves the MAC address).
! ==============================================================
```