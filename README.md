
```
!  ConfMerge OneAccess
banner_extension exec sequence 30 Easywan OneAccess Confmerge Update


! ------------------------------------------------
! Template Routage IPv4 / IPv6 pour site simple ONE ACCESS
!
! Creation Ph. Jounin 2020/11
!
! ------------------------------------------------
! Static routes
! ------------------------------------------------

! ------------------------------------------------
! ROUTAGE VRF MASTER
! ------------------------------------------------

ip extcommunity-list 24 permit soo 1724:100016


! ---
! Route-map out
! ---
route-map SooCT610 permit 10
 set community add 65500:610
 set extcommunity soo 1724:100016
 exit
!
route-map SooCT590 permit 10
 set community add 65500:590
 set extcommunity soo 1724:100016
 exit
! ---
! Route-map in
! ---
route-map SooLP610 deny 5
 match extcommunity 24
 exit
route-map SooLP610 permit 10
 set local-preference 610
 exit
!
route-map SooLP590 deny 5
 match extcommunity 24
 exit
route-map SooLP590 permit 10
 set local-preference 590
 exit
!
!


! Routage Statique des peer IPv6 vers leur PE
ipv6 route 2001:660:2009:1015::21/128 Dialer 1

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
! SET-COMM ROUTE-MAP 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
! LAN-IN LAN-OUT ROUTE-MAP           
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

!
!!!!!!!!!!!!!!!!!!!!! BGP CONFIGURATION !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
router bgp 65500  vrf CRARA 
        no bgp default ipv4-unicast

        peer-group PE1
                disable-connected-check
                route-map SooLP610 in
                route-map SooCT610 out
                remote-as 1724
                timers 15 45
        exit
        peer-group PE2
                disable-connected-check
                route-map SooLP590 in
                route-map SooCT590 out
                remote-as 1724
                timers 15 45
        exit
        address-family ipv4
                peer-group PE1 activate
                peer-group PE2 activate
        exit
! BGP neighbor to nominal PE Ipv6
        exit
        address-family ipv6
                peer-group PE1 activate
                peer-group PE2 activate
        exit
        neighbor 100.72.200.255 
                peer-group PE1
                exit
        neighbor 2001:660:2009:1003::21 
                peer-group PE1
                exit
        ! BGP neighbor to nominal PE Ipv6
router bgp 65500 
        no bgp default ipv4-unicast

        peer-group PE1
                disable-connected-check
                route-map SooLP610 in
                route-map SooCT610 out
                remote-as 1724
                timers 15 45
        exit
        peer-group PE2
                disable-connected-check
                route-map SooLP590 in
                route-map SooCT590 out
                remote-as 1724
                timers 15 45
        exit
        address-family ipv4
                peer-group PE1 activate
                peer-group PE2 activate
        exit
! BGP neighbor to nominal PE Ipv6
        exit
        address-family ipv6
                peer-group PE1 activate
                peer-group PE2 activate
        exit
        neighbor 100.72.202.255 
                peer-group PE1
                exit
        neighbor 2001:660:2009:1015::21 
                peer-group PE1
                exit
        ! BGP neighbor to nominal PE Ipv6
router bgp 65500  vrf COLLEGE-69 
        no bgp default ipv4-unicast

        peer-group PE1
                disable-connected-check
                route-map SooLP610 in
                route-map SooCT610 out
                remote-as 1724
                timers 15 45
        exit
        peer-group PE2
                disable-connected-check
                route-map SooLP590 in
                route-map SooCT590 out
                remote-as 1724
                timers 15 45
        exit
        address-family ipv4
                peer-group PE1 activate
                peer-group PE2 activate
        exit
! BGP neighbor to nominal PE Ipv6
        exit
        address-family ipv6
                peer-group PE1 activate
                peer-group PE2 activate
        exit
        neighbor 100.72.201.255 
                peer-group PE1
                exit
        neighbor 2001:660:2009:1006::21 
                peer-group PE1
                exit
        ! BGP neighbor to nominal PE Ipv6

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
! Routes advertisement MONOVRF and MultiVRF
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
router bgp 

        network 10.77.77.0/255.255.255.0
        
        exit
        address-family ipv6
        exit
exit



!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
! ROUTAGE MULTIVRF
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!


 
! tmpl_QoS-LAN-Standard.txt A ECRIRE
! tmpl_QoS-WAN-Standard.txt A ECRIRE
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTM3MDAwMTQsODA1NjQwMjc3LC0xNTEzND
U5NTk1XX0=
-->