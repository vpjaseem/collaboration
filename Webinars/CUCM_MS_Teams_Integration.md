# CUCM - MS Teams Integration Webinar (03/Nov/2023 09:00PM IST)
Unlocking Seamless Communication in the Modern Workplace
In today's fast-paced business world, effective communication and collaboration are the cornerstones of success. As organizations seek to empower their teams and connect with customers across the globe, the intersection of technologies becomes pivotal. That's where this webinar comes in – a deep dive into the integration of two giants in the communication and collaboration space, **Cisco Unified Communications Manager (CUCM)** and **Microsoft Teams**, facilitated by **Cisco CUBE** with **Direct Routing feature**...!

<p align="center" width="100%">
  <img src="https://github.com/vpjaseem/collaboration/assets/67306692/0ab235b3-62ba-4c54-b1ff-8ff9bfbfd655">
</p>

Join us for a comprehensive exploration of how to bridge the gap between these two industry leaders, enhancing your ability to connect, communicate, and collaborate with unparalleled efficiency. Our webinar will cover everything you need to know about bringing the power of Microsoft Teams into the Cisco Unified Communications environment, leveraging the robust capabilities of Cisco CUBE as the bridge.

The session will be conducted via Zoom Meeting Platform, I Strongly recommend to **install Zoom App** for the smooth experience.

## Class Whiteboard Link
The contents and drawings we discussed in the session will be available here.
- Whiteboard [Link](https://notability.com/n/2ENMinaEW4jAfuCk2Ig4vS)

## Prerequisites
- Basic understanding of CUCM, IP Phone Registration, SIP Trunking
- Knowledge of CUBE (Dial-Peers, SIP)
- Basic understanding of SSL Certificates and how to use them in Cisco Routers - [Reference](https://youtu.be/8pUtDOTw-HM)
- Knowledge of Microsoft 365 Admin Center and MS Teams Admin Center
- Basic understanding of Microsoft Direct Routing (Theoretical knowledge is enough)
- Basic knowledge of Regular Expressions (RegEx) - [Regex Tool](https://regex101.com/)
- Strongly recommend to install Zoom App for the smooth experience 

## Lab Topology
![image](https://github.com/vpjaseem/collaboration/assets/67306692/d813790e-c69d-47d7-a05b-2edf63fa0837)

- CUCM 14 SU3 (14.0.1.13900-155)
- CUBE - Cisco 2901 IOS Version 15.7(3)M8
- Teams App
- Cisco IP Phones 8865, 9971
- [Godaddy Domain](https://www.godaddy.com/en-in) '**ajcollab.com**'
- Free SSl Certificate from [ZeroSSL](https://zerossl.com) or [NameCheap SSL](https://www.namecheap.com/security/ssl-certificates/comodo/positivessl/)
- [Microsoft 365 Admin Center](https://admin.microsoft.com/)
- [Teams Admin Center](https://admin.teams.microsoft.com/)
- MS Teams Root Certificate Authorities - [Baltimore CyberTrust Root](https://github.com/vpjaseem/collaboration/blob/main/YouTube/BaltimoreCyberTrustRoot.cer) | [DigiCert Global Root G2](https://github.com/vpjaseem/collaboration/blob/main/YouTube/DigiCertGlobalRootG2.cer) 

## Build Your Own Lab
You can build exactly similar lab setup by following below links.
- How to build your own Cisco UC Lab - Complete guide to setup Cisco UC in your laptop. [Step by Step Guide](https://github.com/vpjaseem/collaboration/blob/main/Webinars/Build%20Your%20Own%20Home%20UC%20Lab%20in%20vmware%20Workstation.pdf) | [ISO and OVA Files](https://drive.google.com/drive/folders/1y48f4B0yjkxXxRnu92a1jAAPajKTeshK?usp=sharing)
- How to build your own Microsoft UC Lab - Complete guide to Developer Cloud tenant [Step by Step Guide](https://youtu.be/l4VdFLPXr40)

## Call Routing Architecture
![image](https://github.com/vpjaseem/collaboration/assets/67306692/cd42438f-543a-43b9-8ac4-9f770610be37)

## Dial Plan Design
![image](https://github.com/vpjaseem/collaboration/assets/67306692/973840bf-3cab-4659-957e-2ee3208a22d6)

**CUCM to MS Teams** 
- Same DID +12176411001 assigned to IP Phone and MS Teams
- MS Teams require proper E.164 Pattern to route the incoming call
- CUCM Uses 8XXXX to dial Teams Extension, this will be translated to 881217641XXXX from Route Pattern level itself
- At the CUBE level, remove the 88 and add + then the number becomes +1217641XXXX (e.g. +1217641-1001)
- Teams will handle the proper E.164 Number and extend the call to Teams App

**MS Teams to CUCM**
- User is assigned with a dial-pan ^11(\d{3})$ to select proper 5 digit extension
- User assigned with a Voice Routing Policy that is having a PSTN Usage Record
- ^11(\d{3})$ Voice route having PSTN Usage Record will send the call to CUBE
- CUBE route same call to to CUCM
- Extension rings

## Configuration Steps
1. Overview of Lab Topology and final goal
2. Review of CUCM Current Setup (Registered IP Phones with CTL, Tomcat MSAN Certificates, [CallManager MSAN Certificate](https://youtu.be/d6gZiEG2bMw), Device Pools, Regions, Partitions, CSS)
3. *[CUCM]* Configure SIP Profile, SIP Trunk Security Profile, SIP Trunk
4. *[CUCM]* Configure Route Group, Route List and Route Pattern (8XXXX > 881217641XXXX)
5. *[CUBE]* voice class uri, voice class server-group 1, voice class codec, ip address trusted list, Translation Profile, Dial-Peers, e164 pattern maps
6. *[CUBE]* Installing Public SSL Certificate in CUBE (crypto key commands, crypto pki trustpoint)
7. *[Firewall]* Public IP Address and NAT Configurations / Port Forwarding
8. *[CUCM]* Migrating CUCM - CUBE SIP trunk to Secure SIP Trunk (TLS 5061 Port, Root Certificate Upload)
9. *[MS Teams]* License Assigment, Adding SBC, Voice Route, PSTN Usage Record, Voice Routing Policy, Dial-Plan with Normalization
10. *[CUBE]* SIP Profiles, Options Ping, voice service voip, voice class tenant 200, sip-ua
11. Test calls between CUCM and MS Teams
12. Simultaneous Ring Feature (CUCM "SNR" and SM Teams "Ring also Ring" feature)

## Firewall / NAT Configurations
The CUBE must have a Public IP for interconnecting Microsoft Teams and CUBE. There are multiple ways to get Public IP to CUBE, either directly assign Public IP in one of the CUBE interfaces or configure NAT, so that it can translate Private IP of CUBE to Public IP. For Enterprise deployments, have meeting with your network and firewall team to get this completed. In my case, I do have a Public IP and will be doing the NAT. This will make sure any SIP, HTTP, RTP Traffic to my public IP will be forwarded to Private IP of CUBE which is 192.168.0.11

![image](https://github.com/vpjaseem/collaboration/assets/67306692/7766ef3a-a9a7-4dec-abc8-be87eee80357)

## CUBE Configuration Commands
```
!!!!!!!!!! PART 1 !!!!!!!!!!
!! Manipulations for outbound messages to MS Teams
!
voice class sip-profiles 200
 rule 10 request ANY sip-header Contact modify "@.*:" "@sbc.ajcollab.com:" 
 rule 20 response ANY sip-header Contact modify "@.*:" "@sbc.ajcollab.com:" 
 rule 30 request ANY sip-header SIP-Req-URI modify "sip:(.*):5061 (.*)" "sip:\1:5061;user=phone \2" 
 rule 40 request ANY sip-header User-Agent modify "(IOS.*)" "\1\x0D\x0AX-MS-SBC: Cisco UBE/ISR4321/\1" 
 rule 50 response ANY sip-header Server modify "(IOS.*)" "\1\x0D\x0AX-MS-SBC: Cisco UBE/ISR4321/\1" 
 rule 60 request ANY sdp-header Audio-Attribute modify "a=sendonly" "a=inactive" 
 rule 70 response 200 sdp-header Audio-Connection-Info modify "0.0.0.0" "103.94.138.238" 
 rule 71 response ANY sdp-header Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
 rule 72 response ANY sdp-header Audio-Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
 rule 73 request ANY sdp-header Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
 rule 74 request ANY sdp-header Audio-Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
 rule 80 request ANY sdp-header Audio-Attribute modify "(a=crypto:.*inline:[A-Za-z0-9+/=]+)" "\1|2^31" 
 rule 90 response ANY sdp-header Audio-Attribute modify "(a=crypto:.*inline:[A-Za-z0-9+/=]+)" "\1|2^31" 
 rule 100 request ANY sdp-header Audio-Attribute modify "a=candidate.*" "a=label:main-audio" 
 rule 110 response ANY sdp-header Audio-Attribute modify "a=candidate.*" "a=label:main-audio" 
 rule 120 response 486 sip-header Reason modify "cause=34;" "cause=17;" 
 rule 300 response ANY sdp-header Audio-Attribute modify "a=rtcp:(.*) IN IP4 192.168.0.11" "a=rtcp:\1 IN IP4 103.94.138.238" 
 rule 310 request ANY sdp-header Audio-Attribute modify "a=rtcp:(.*) IN IP4 192.168.0.11" "a=rtcp:\1 IN IP4 103.94.138.238" 
 rule 320 response ANY sdp-header Audio-Attribute modify "a=candidate:1 1(.*) 192.168.0.11 (.*)" "a=candidate:1 1\1 103.94.138.238 \2" 
 rule 330 request ANY sdp-header Audio-Attribute modify "a=candidate:1 1(.*) 192.168.0.11 (.*)" "a=candidate:1 1\1 103.94.138.238 \2" 
 rule 340 response ANY sdp-header Audio-Attribute modify "a=candidate:1 2(.*) 192.168.0.11 (.*)" "a=candidate:1 2\1 103.94.138.238 \2" 
 rule 350 request ANY sdp-header Audio-Attribute modify "a=candidate:1 2(.*) 192.168.0.11 (.*)" "a=candidate:1 2\1 103.94.138.238 \2" 
!

!! Manipulations for inbound messages from MS Teams
!
voice class sip-profiles 290
 rule 10 request REFER sip-header From copy "@(.*com)" u05 
 rule 15 request REFER sip-header From copy "sip:(sip.*com)" u05 
 rule 20 request REFER sip-header Refer-To modify "sip:\+(.*)@.*:5061" "sip:+AAA\1@\u05:5061" 
 rule 30 request REFER sip-header Refer-To modify "<sip:sip.*:5061" "<sip:+AAA@\u05:5061" 
 rule 40 response ANY sip-header Server modify "(IOS.*)" "\1\x0D\x0AX-MS-SBC: Cisco UBE/ISR4321/\1" 
 rule 50 request ANY sdp-header Audio-Attribute modify "a=ice-.*" "a=label:main-audio" 
 rule 60 request ANY sdp-header Attribute modify "a=ice-.*" "a=label:main-audio" 
 rule 70 response ANY sdp-header Audio-Attribute modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 80 request ANY sdp-header Connection-Info modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 90 response ANY sdp-header Audio-Attribute modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 100 response ANY sdp-header Connection-Info modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 110 request ANY sdp-header mline-index 1 c= modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 120 response ANY sdp-header mline-index 1 c= modify "IN IP4 103.94.138.238" "IN IP4 192.168.0.11" 
 rule 130 request ANY sdp-header Audio-Attribute modify "a=candidate:1 1 (.*) 103.94.138.238" "a=candidate:1 1 \1 192.168.0.11" 
 rule 140 request ANY sdp-header Audio-Attribute modify "a=candidate:1 2 (.*) 103.94.138.238" "a=candidate:1 2 \1 192.168.0.11" 
 rule 150 response ANY sdp-header Audio-Attribute modify "a=candidate:1 1 (.*) 103.94.138.238" "a=candidate:1 1 \1 192.168.0.11" 
 rule 160 response ANY sdp-header Audio-Attribute modify "a=candidate:1 2 (.*) 103.94.138.238" "a=candidate:1 2 \1 192.168.0.11" 
 rule 170 request ANY sdp-header Audio-Attribute modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
!

!! Manipulations for OPTIONS Keepalive to MS Teams
!
voice class sip-profiles 299
 rule 9 request ANY sip-header Via modify "SIP(.*) 192.168.0.11(.*)" "SIP\1 103.94.138.238\2" 
 rule 10 request OPTIONS sip-header From modify "<sip:192.168.0.11" "<sip:sbc.ajcollab.com" 
 rule 20 request OPTIONS sip-header Contact modify "<sip:192.168.0.11" "<sip:sbc.ajcollab.com" 
 rule 30 request OPTIONS sip-header User-Agent modify "(IOS.*)" "\1\x0D\x0AX-MS-SBC: Cisco UBE/ISR4321/\1" 
 rule 40 response ANY sdp-header Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
 rule 50 response ANY sdp-header Audio-Connection-Info modify "IN IP4 192.168.0.11" "IN IP4 103.94.138.238" 
!

!!!!!!!!!! PART 2 !!!!!!!!!!
!! For addressing the call transfers
!
voice class sip-hdr-passthrulist 290
 passthru-hdr Referred-By
!

!! Used to tag and accept call from CUCM
!
voice class uri CUCM sip
 host ipv4:172.16.11.12
 host ipv4:172.16.11.13
!

!! Used to tag and accept call from MS Teams
!
voice class uri 290 sip
 host sbc.ajcollab.com
!

!! Used to extend the call to CUCM
!
voice class server-group 1
 ipv4 172.16.11.12
 ipv4 172.16.11.13
 description CUCM-SERVERS
!

!! List of Supported Codecs
!
voice class codec 1
 codec preference 1 g711ulaw
 codec preference 2 g711alaw
 codec preference 3 g729r8
 codec preference 4 g729br8
!

!! Configuring OPTIONS Keepalive
!
voice class sip-options-keepalive 200
 transport tcp tls
 sip-profiles 299
!

!! MS Teams Numbers, same DIDs with 88 as a prefix
!
voice class e164-pattern-map 200
  e164 8812176411...
!

!! CUCM Extension Numbers
!
voice class e164-pattern-map 1
  e164 11...
 !
!

!!!!!!!!!! PART 3 !!!!!!!!!!
!! CUBE Global Settings
!
voice service voip
 ip address trusted list
  ipv4 52.112.0.0 255.252.0.0 ! Microsoft Teams cloud services
  ipv4 52.120.0.0 255.252.0.0
  
  ipv4 172.16.11.11 ! CUCM IPs
  ipv4 172.16.11.12
  ipv4 172.16.11.13
  
  ipv4 192.168.0.2 ! NAT Device

 rtcp keepalive
 rtp-port range 16384 16386 ! Only 2 Media Ports. Use only in lab, In production, consult with firewall team.
 address-hiding
 mode border-element 
 allow-connections sip to sip
 no supplementary-service sip refer
 supplementary-service media-renegotiate
 fax protocol t38 version 0 ls-redundancy 0 hs-redundancy 0 fallback none
 sip
  session refresh
  header-passing
  error-passthru
  srtp-auth sha1-80
  pass-thru headers 290
  sip-profiles inbound
!

!! MS Teams Tenant Settings
!
voice class tenant 200
  handle-replaces
  localhost dns:sbc.ajcollab.com
  session transport tcp tls
  no referto-passing
  bind control source-interface GigabitEthernet0/0
  bind media source-interface GigabitEthernet0/0
  pass-thru headers 290
  no pass-thru content custom-sdp
  sip-profiles 200
  sip-profiles 290 inbound
  early-offer forced
  block 183 sdp present
!

!! SIP UA Configurations
!
sip-ua 
 nat symmetric check-media-src
 no remote-party-id
 retry invite 2
 connection-reuse
 crypto signaling default trustpoint SBC-CERT-STORE 
 handle-replaces

!!!!!!!!!! PART 4 !!!!!!!!!!
!! Remove 88 from Teams Number and add +
!
voice translation-rule 1
 rule 1 /\(88\)\(1217641....\)/ /+\2/
!
voice translation-profile TEAMS-OUT
 translate called 1
!


!!!!!!!!!! PART 5 !!!!!!!!!!
!! Dial-Peer to accept call from CUCM
!
dial-peer voice 1 voip
 description INBOUND-FROM-CUCM
 session protocol sipv2
 incoming uri via CUCM
 voice-class codec 1  
 voice-class sip bind control source-interface GigabitEthernet0/0
 voice-class sip bind media source-interface GigabitEthernet0/0
 dtmf-relay rtp-nte
 srtp
 no vad
 session transport tcp tls
!

!! Dial-Peer to Send the call TO MS Teams
!
dial-peer voice 2 voip
 description OUTBOUND-TO-MS-TEAMS
 translation-profile outgoing TEAMS-OUT
 preference 1
 rtp payload-type comfort-noise 13
 session protocol sipv2
 session target dns:sip.pstnhub.microsoft.com:5061
 destination e164-pattern-map 200
 voice-class codec 1  
 voice-class sip tenant 200
 voice-class sip options-keepalive profile 200
 dtmf-relay rtp-nte
 srtp
 fax protocol none
 no vad
!

!! Dial-Peer to accept call from MS Teams
!
dial-peer voice 3 voip
 description inbound INBOUND-FROM-MS-TEAMS
 rtp payload-type comfort-noise 13
 session protocol sipv2
 !!destination dpg 4
 incoming uri to 290
 voice-class codec 1  
 voice-class sip tenant 200
 dtmf-relay rtp-nte
 srtp
 no vad
!

!! Dial-Peer to Send the call to CUCM
!
dial-peer voice 4 voip
 description OUTBOUND-TO-CUCM
 rtp payload-type comfort-noise 13
 session protocol sipv2
 session transport tcp tls
 session server-group 1
 destination e164-pattern-map 1
 voice-class codec 1  
 voice-class sip bind control source-interface GigabitEthernet0/0
 voice-class sip bind media source-interface GigabitEthernet0/0
 dtmf-relay rtp-nte
 srtp
 no vad
!
!!!!!!!!!! PART 6 !!!!!!!!!!
!
ip domain name ajcollab.com
!
!! Cleaning old Key Pairs, Only use if you wanted to clean the keys !!
!
crypto key zeroize rsa
!

!! Generate Key Pair!! 
!
crypto key generate rsa general-keys label SBC-RSA-KEY modulus 2048 exportable
!
show crypto key mypubkey rsa
!
ip ssh rsa keypair-name SBC-RSA-KEY

!! Create Trust Store, this will store Identity certificate and bundle of Intermediate and Root CAs !!
!
crypto pki trustpoint SBC-CERT-STORE
 enrollment terminal
 fqdn sbc.ajcollab.com 
 subject-name cn=sbc.ajcollab.com,OU=Collab,O=AJ Collab
 subject-alt-name sbc.ajcollab.com
 serial-number none
 ip-address non
 revocation-check none
 rsakeypair SBC-RSA-KEY
!

!! Generate CSR, will display the CSR in the terminal. Copy and send to CA!!
!
crypto pki enroll SBC-CERT-STORE
!

!! Authenticate the Identity certificate with bundle of Intermediate and Root CAs !!
!
crypto pki authenticate SBC-CERT-STORE
!
-----BEGIN CERTIFICATE-----
Paste the base 64 encoded bundle of Intermediate and Root CAs as single.
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
-----END CERTIFICATE-----
!

!! Import identity certificate !!
!
crypto pki import SBC-CERT-STORE certificate
!
-----BEGIN CERTIFICATE-----
Paste the base 64 encoded Identity certificate  content here
-----END CERTIFICATE-----
!

!! View the Certificates !!
!
show crypto pki certificates
!

!! Point Trust Store for SIP Traffic !!
sip-ua 
crypto signaling default trustpoint SBC-CERT-STORE
!

!! Point Trust Store for HTTPS Traffic !!
!
ip http authentication local
ip http secure-server
ip http secure-trustpoint SBC-CERT-STORE
!
!! Microsoft Root CAs for Direct Routing !!
crypto pki trustpoint BALTOMORE-MICROSOFT-CA
enrollment terminal
revocation-check none
!
!
crypto pki authenticate BALTOMORE-MICROSOFT-CA
!
-----BEGIN CERTIFICATE-----
Paste the base 64 encoded BALTOMORE certificate.
-----END CERTIFICATE-----
!!You can download BALTOMORE certificate from here: https://github.com/vpjaseem/collaboration/blob/main/YouTube/BaltimoreCyberTrustRoot.cer


crypto pki trustpoint DIGICERT-MICROSOFT-CA
enrollment terminal
revocation-check none
!
crypto pki authenticate DIGICERT-MICROSOFT-CA
!
!!You can download DIGICERT certificate from here: https://github.com/vpjaseem/collaboration/blob/main/YouTube/DigiCertGlobalRootG2.cer

!! Enterprise CA Certificate Trust Store, Used to store the Internal CA Trust Relation !!
!
crypto pki trustpoint AJCOLLAB-ENTERPRISE-CA
enrollment terminal
revocation-check none
!
crypto pki authenticate AJCOLLAB-ENTERPRISE-CA
!
-----BEGIN CERTIFICATE-----
Paste the base 64 encoded BLK Collab CA certificate  content here
-----END CERTIFICATE-----
!


!!! Show Commands !!!
show crypto pki certificates
show crypto key mypubkey rsa
show sip-ua calls
```
## About Me
**Abdul Jaseem**, UC Architect and Corporate Trainer
- Got in to Networking & Collaboration due to not being able to get a job in Embedded Systems :)
- Learn at least a new thing everyday
- From Karuvarakundu, Kerala, India
- Passionate about Technology, cars and driving
- Skills: Cisco UC, Microsoft UC, Genesys Cloud CX, Amazon Connect, Webex CC, Azure, AWS, Windows, vmware, Python, Automation
- Certs: CCIE Collab #59174, Teams Voice Engineer, DevNet, CCNP DC, CCNP Ent, CCNP Security, AWS, Azure, CKA
- Mob: +91-859-0101-859 ([WhatsApp Preferred](https://wa.me/+918590101859))<br>
- [LinkedIn](https://in.linkedin.com/in/abdul-jaseem)


## Courses offered by AJ Labs
- **[Video Training] Cloud Collaboration** (vmware, Windows AD, DNS, Azure, Azure AD, MS Teams, AudioCodes SBC, Direct Routing, Webex, Webex Calling with CUBE) - [Topic Summary](https://github.com/vpjaseem/collaboration/blob/main/Webinars/Cloud%20Collaboration%20Training%20AJ%20Labs%20Ad.pdf) | [Syllabus](https://github.com/vpjaseem/collaboration/blob/main/Webinars/AJ%20Labs%20Cloud%20Collaboration%20Syllabus.pdf) 
- **[Video Training] Advanced Cisco Collaboration Video Training** (vmware, Windows, CUCM, CUC, IMP, Expressway, UC Upgrade, MRA, B2B, Webex) - [Topic Summary](https://github.com/vpjaseem/collaboration/blob/main/Webinars/Adv.%20Cisco%20Collaboration%20Training.pdf) | [Syllabus](https://github.com/vpjaseem/collaboration/blob/main/Webinars/Advanced%20Cisco%20Collaboration%20Syllabus.pdf) | [Free eBook](https://drive.google.com/file/d/15pI_tyxAFgSUHW8Qb9PuwGxCKUsSR4EM/view?usp=sharing)
- **[Live Training] Cloud Contact Center Training Live Training** (Basics of AWS, Genesys Cloud CX, Amazon Connect, Webex Contact Center) - [Topic Summary](https://github.com/vpjaseem/collaboration/blob/main/Webinars/Cloud%20Contact%20Center%20Training.pdf) | Start Date **15/April/2023** | Join the [WhatsApp Group](https://chat.whatsapp.com/FjXHwk5QnPVCy0Oa2F7bS5) if interested, or contact **+91-859-0101-859**

## Helpful Links
These are some helpful links for your references. 
- [CUCM - CUBE Secure SIP Trunking, Mixed Mode, CTL CAPF](https://youtu.be/d6gZiEG2bMw)
- [CUBE SSL Certificate Installation Process](https://youtu.be/8pUtDOTw-HM)

## Webinar Unanswered Q&A
1. 
2. 

