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

## Packet Tracer - Configure Trunks

![Packet Tracer - Configure Trunks](https://itexamanswers.net/wp-content/uploads/2020/02/3.4.5-Packet-Tracer-Configure-Trunks.jpg)

### Objectives

 - Part 1: Verify VLANs
 - Part 2: Configure Trunks

### SWITCH S1
``` bash
! ==============================================================
!--- The VLANs and the access ports on S2/S3 are preconfigured; this lab only configures the trunk links.
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 2, Step 1: Configure trunking on S1 and use VLAN 99 as the native VLAN.
! --------------------------------------------------------------

!--- a. Configure G0/1 (to S2) and G0/2 (to S3) for trunking.
!--- b. Configure VLAN 99 as the native VLAN on both trunks. CDP reports a native VLAN mismatch until S2 and S3 are also set to VLAN 99.
interface range gigabitethernet 0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 exit

end

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification:
!---   show interfaces trunk  -> Gig0/1 and Gig0/2 trunking, native VLAN 99
!---   From PC1, ping 172.17.10.24 (PC4) -> success across the trunks
! ==============================================================
```

### SWITCH S2
``` bash
! ==============================================================
!--- S2 connects to S1 on G0/1 only.
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 2, Step 3: Correct the native VLAN mismatch on S2 and S3.
! --------------------------------------------------------------

!--- a. Make G0/1 a static trunk (Step 4 expects "Administrative Mode: trunk") and set the native VLAN to 99 to match S1.
interface gigabitethernet 0/1
 switchport mode trunk
 switchport trunk native vlan 99
 exit

end

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification:
!---   show interfaces trunk                             -> Gig0/1 trunking, native VLAN 99, VLANs 1,10,20,30,88,99 active
!---   show interfaces gigabitethernet 0/1 switchport  -> Administrative Mode: trunk, Native VLAN 99
!---   show vlan                                         -> Gig0/1 no longer listed (trunk ports are not shown)
! ==============================================================
```

### SWITCH S3
``` bash
! ==============================================================
!--- S3 connects to S1 on G0/2 only.
! ==============================================================

enable
configure terminal

! --------------------------------------------------------------
!--- Part 2, Step 3: Correct the native VLAN mismatch on S2 and S3.
! --------------------------------------------------------------

!--- a. Make G0/2 a static trunk (Step 4 expects "Administrative Mode: trunk") and set the native VLAN to 99 to match S1.
interface gigabitethernet 0/2
 switchport mode trunk
 switchport trunk native vlan 99
 exit

end

! --------------------------------------------------------------
!--- Save the configuration to NVRAM. (Press Enter when prompted for the destination filename.)
! --------------------------------------------------------------
copy running-config startup-config


! ==============================================================
!--- Verification:
!---   show interfaces trunk                             -> Gig0/2 trunking, native VLAN 99, VLANs 1,10,20,30,88,99 active
!---   show interfaces gigabitethernet 0/2 switchport  -> Administrative Mode: trunk, Native VLAN 99
!---   show vlan                                         -> Gig0/2 no longer listed (trunk ports are not shown)
! ==============================================================
```