# Webex Calling with CUBE - Free Webinar (24/Feb/2024 08:00PM IST)
Unlock the Power of **Webex Calling with CUBE Local Gateway!** Join our free webinar and discover how this integration revolutionizes communication. Whether you're a seasoned IT professional or just embarking on your Unified Communications journey, this webinar aims to provide valuable knowledge and practical advice to help you leverage these cutting-edge technologies effectively. Let's start this journey together as we unlock the full potential of **Webex Calling with vCUBE**.
<p align="center">
  <img src="https://github.com/vpjaseem/collaboration/assets/67306692/2a27ebb3-74bd-44d2-a97f-0360c447d061" width="50%"  height="50%">
</p>

## Prerequisites
- Install [Webex App](https://www.webex.com/downloads.html) and try to join from PC (mobile app will have limited experience)
- Basic understanding of Webex Control Hub
- Basic Knowledge of CUBE (Dial-Peers, SIP)
- Basic understanding of Cloud Computing (not mandatory)

## Lab Topology
![image](https://github.com/vpjaseem/collaboration/assets/67306692/77913cd2-899c-4472-8ab9-16b0ffd41109)
- Users are using Webex App, Mobile App, IP Phones to access Webex Services
- vCUBE deployed in Azure and Network Security Group configured to allow SIP and RTP ports
- Twilio Elastic SIP Trunking acts as PSTN

## Build your own FREE lab
The entire lab can be build absolutely free of cost.
- [Webex SandBox](https://developer.webex.com/docs/developer-sandbox-guide) – Free Webex account for 90 days
- [Azure Free Trial](https://azure.microsoft.com/en-in/pricing/offers/ms-azr-0044p) – Fully functional Azure Free account with $200 credit
- [Twilio](https://www.twilio.com/try-twilio) – Free Twilio Account

## Call Routing Architecture
![image](https://github.com/vpjaseem/collaboration/assets/67306692/cd82d97c-6805-4b33-8dd6-4ca7910e2d3c)

## Dial Plan Design
1. Trunk
2. Route Group,
3. Dial Plan (+91!)
4. Voice Class Tenants at CUBE
5. Dial Peers

## Configurations Steps
**Azure**
1. Azure Marketplace > Cisco Virtual CUBE Session Border Controller > Create > Select Subscription and follow the VM creation wizard and complete the deployment
2. Configure Network Security Group to allow required ports (SSH 22, SIP 5060 - 5080, Media 16384 - 32787)

**Webex Side**
1. Add Location (I have used this address 2770 E Trinity Mls Rd, Carrollton, TX 75006, USA), Add Trunk, Route Group, Dial Plan, Add Numbers
2. Enable Premise based PSTN
3. Assign License and Location and Number to the user

**CUBE Side**
1. CUBE Configurations
```
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!Details copied from Webex Control Hub

Trunk Group OTG/DTG
cube01-trunk2692_lgu

Outbound Proxy Address
dfw08.sipconnect-us.bcld.webex.com

Registrar Domain
98027369.us10.bcld.webex.com

Line/Port
cube01-trunk0683_LGU@98027369.us10.bcld.webex.com

Username: cube01-trunk2692_LGU
Password: P0r40Bbupw
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***

!!License Configuration for vCUBE
!
license boot level network-premier addon dna-premier
do write
exit
reload
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!Take a configuration backup to router flash
!
copy running-config flash:
Destination filename [running-config]? base-config-backup
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***

!!Encrypting the password provided by Control Hub
!
key config-key password-encrypt PASSWORD
password encryption aes
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***

!!Self Signed Certificate Configuration for vCUBE
!
crypto pki trustpoint sampleTP
 revocation-check crl
!
sip-ua
 crypto signaling default trustpoint sampleTP cn-san-validate server
 transport tcp tls v1.2
 tcp-retry 1000
!
crypto pki trustpool import clean url http://www.cisco.com/security/pki/trs/ios_core.p7b
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!Global SIP Configuration
!
voice service voip
 ip address trusted list
  ipv4 128.177.14.0 255.255.255.128 !Webex Calling IPs
  ipv4 128.177.36.0 255.255.255.192
  ipv4 199.59.64.0 255.255.255.128
  ipv4 185.115.196.0 255.255.255.128
  ipv4 139.177.64.0 255.255.255.0
  ipv4 139.177.72.0 255.255.255.0
  ipv4 199.19.199.0 255.255.255.0
  ipv4 199.19.196.0 255.255.254.0
  ipv4 170.133.128.0 255.255.192.0
  ipv4 170.72.0.0 255.255.0.0
  ipv4 150.253.209.128 255.255.255.128
  ipv4 135.84.168.0 255.255.248.0
  ipv4 23.89.0.0 255.255.0.0
  ipv4 85.119.56.0 255.255.254.0

  ipv4 54.171.127.192 255.255.255.192 !Twilio Service Provider IPs
  ipv4 54.171.127.192 255.255.255.192
  ipv4 54.244.51.0 255.255.255.0
  ipv4 54.172.60.0 255.255.254.0
  ipv4 54.172.60.0 255.255.255.252
  ipv4 54.244.51.0 255.255.255.252
  ipv4 54.171.127.192 255.255.255.252
  ipv4 35.156.191.128 255.255.255.252
  ipv4 54.65.63.192 255.255.255.252
  ipv4 54.169.127.128 255.255.255.252
  ipv4 54.252.254.64 255.255.255.252
  ipv4 177.71.206.192 255.255.255.252
  exit
 exit
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!SIP Global Commands
!
voice service voip
  media bulk-stats
  rtp-port range 16384 32766
  allow-connections sip to sip
  no supplementary-service sip refer
  no supplementary-service sip handle-replaces
  fax protocol t38 version 0 ls-redundancy 0 hs-redundancy 0 fallback none
  trace
  media statistics
 stun
  stun flowdata agent-id 1 boot-count 4
  stun flowdata shared-secret 0 Password123$
  exit
 sip
  asymmetric payload full
  early-offer forced
  g729 annexb-all
  exit 
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!SIP Profile for Webex Call
!
voice class sip-profiles 1
 rule 9 request ANY sip-header SIP-Req-URI modify "sips:(.*)" "sip:\1"
 rule 10 request ANY sip-header To modify "<sips:(.*)" "<sip:\1"
 rule 11 request ANY sip-header From modify "<sips:(.*)" "<sip:\1"
 rule 12 request ANY sip-header Contact modify "<sips:(.*)>" "<sip:\1;transport=tls>"
 rule 13 response ANY sip-header To modify "<sips:(.*)" "<sip:\1"
 rule 14 response ANY sip-header From modify "<sips:(.*)" "<sip:\1"
 rule 15 response ANY sip-header Contact modify "<sips:(.*)" "<sip:\1"
 rule 20 request ANY sip-header From modify ">" ";otg=TRUNK_GROUP_OTG>"
 rule 30 request ANY sip-header P-Asserted-Identity modify "sips:(.*)" "sip:\1"
 rule 31 request INVITE sip-header Diversion modify "Diversion:(.*)@twilio.com(.*)" "" 
!
!!SIP Profile for Twilio PSTN Call
!
voice class sip-profiles 2
 request REINVITE sip-header From modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com>"
 request CANCEL sip-header From modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com>"
 request INVITE sip-header To modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com>"
 request REINVITE sip-header To modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com>"
 request INVITE sip-header From modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com;user=phone>"
 request INVITE sip-header P-Asserted-Identity modify "(<.*:.*)(@.*>)" "\1@TERMINATION.pstn.twilio.com;user=phone>"
 request ANY sip-header Contact modify "@.*:" "@CUBE_PUBLIC_IP:"
 response ANY sip-header Contact modify "@.*:" "@CUBE_PUBLIC_IP:"
!
voice class uri 1 sip
 pattern dtg=TRUNK_GROUP_OTG
!
voice class codec 1
 codec preference 1 g711ulaw
 codec preference 2 g711alaw
 codec preference 3 g729r8
!
voice class srtp-crypto 1
 crypto 1 AES_CM_128_HMAC_SHA1_80
!
voice class stun-usage 1
 stun usage firewall-traversal flowdata
 stun usage ice lite
!
voice class e164-pattern-map 1
  description WEBEX DID NUMBERS
  e164 +1..........
!
voice class dpg 2
 description WEBEX TO TWILIO PSTN
!
voice class dpg 4
 description TWILIO PSTN TO WEBEX
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!!Map Control Hub parameters to Local Gateway Tenant configuration
!
voice class tenant 1
  registrar dns:1_REGISTRAR_DOMAIN scheme sips expires 240 refresh-ratio 50 tcp tls
  credentials number 2_LINE_PORT_TILL_LGU username 3_USER_NAME password 0 4_PASSWORD realm BroadWorks
  authentication username 3_USER_NAME password 0 4_PASSWORD realm BroadWorks
  authentication username 3_USER_NAME password 0 4_PASSWORD realm 1_REGISTRAR_DOMAIN
  no remote-party-id
  sip-server dns:1_REGISTRAR_DOMAIN
  connection-reuse
  srtp-crypto 1
  session transport tcp tls
  url sips
  error-passthru
  asserted-id pai
  bind control source-interface GigabitEthernet1
  bind media source-interface GigabitEthernet1
  no pass-thru content custom-sdp
  sip-profiles 1
  outbound-proxy dns:5_OUTBOUND_PROXY
  privacy-policy passthru
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
!! Twilio PSTN Tenant
!
voice class tenant 2
  sip-server dns:TERMINATION.pstn.twilio.com
  session transport udp
  asserted-id pai
  bind control source-interface GigabitEthernet1
  bind media source-interface GigabitEthernet1
  sip-profiles 2
  early-offer forced
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
dial-peer voice 1 voip
 description INCOMING FROM WEBEX
 session protocol sipv2
 destination dpg 2
 incoming uri request 1
 voice-class codec 1
 voice-class stun-usage 1
 voice-class sip tenant 1
 dtmf-relay rtp-nte
 srtp
!
dial-peer voice 2 voip
 description OUTGOING TO TWILIO PSTN
 destination-pattern .T
 rtp payload-type comfort-noise 13
 session protocol sipv2
 session target sip-server
 voice-class codec 1
 voice-class sip tenant 2
 dtmf-relay rtp-nte
!
dial-peer voice 3 voip
 description INCOMING FROM TWILIO PSTN
 session protocol sipv2
 destination dpg 4
 incoming called e164-pattern-map 1
 voice-class codec 1
 voice-class sip tenant 2
 dtmf-relay rtp-nte
!
dial-peer voice 4 voip
 description Outgoing OUTGOING TO WEBEX
 destination-pattern BAD.BAD
 session protocol sipv2
 session target sip-server
 voice-class codec 1
 voice-class stun-usage 1
 voice-class sip tenant 1
 no voice-class sip history-info
 no voice-class sip registration passthrough
 dtmf-relay rtp-nte
 srtp
 no vad
!
voice class dpg 2
 description Webex to PSTN
 dial-peer 2 preference 1
!
voice class dpg 4
 description PSTN to Webex
 dial-peer 4 preference 1
!
***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***---***
```
2. Webex Trunk and CUBE Configuration Mapping - [Map Webex Control Hub Trunk paramters to CUBE](https://drive.google.com/file/d/19ASajylH_8MTQX5PG4nKPCwjBmFA9j9H/view?usp=sharing)
   ![image](https://github.com/vpjaseem/collaboration/assets/67306692/4e1da36b-e40e-4921-8fb6-d6f27af7f0f7)
   
**PSTN Twilio Side**
1. Trunk = WEBEX-CALLING-CUBE-TRUNK, Secure Trunking = Disabled, Symmetric RTP = Enabled, Call Transfer = Enabled
2. Termination = ajwxcall.pstn.twilio.com
3. Origination  = sip:X.X.X.X Where X.X.X.X is the Public IP of CUBE
4. IP Access Control List = WEBEX-CALLING-CUBE-IP = X.X.X.X Where X.X.X.X is the Public IP of CUBE


## Courses offered by AJ Labs
Below are the Training course offered by AJ Labs.

1. **Advanced Cloud Collaboration [On demand Video Recordings]** (vmware, Windows AD, DNS, Azure, Azure AD, MS Teams, AudioCodes SBC, Direct Routing, Webex, Webex Calling with CUBE) - [Topic Summary](https://drive.google.com/file/d/1ezFuS6ulvLfyg6PQZXBnnH3McsTaqMWH/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/19zRydHD4n0drATeoFcNbjbfUphrA-IzJ/view?usp=sharing)
   
3. **Cloud Contact Center Training [Live Training]** (Basics of AWS, Genesys Cloud CX, Amazon Connect, Webex Contact Center) - [Topic Summary](https://drive.google.com/file/d/1gxtexL8OPdGawX1GyDmNBPMoox-GJV-M/view?usp=sharing) | Start Date **15/April/2024** (subjected to slight changes) | Join the [WhatsApp Group](https://chat.whatsapp.com/FjXHwk5QnPVCy0Oa2F7bS5)
   
5. **Advanced Cisco Collaboration Video Training [On demand Video Recordings]** (vmware, Windows, CUCM, CUC, IMP, Expressway, CUBE, CUBE HA, SIP Deep Dive, Log Analysis, UC Upgrade, MRA, B2B, Webex) - [Topic Summary](https://drive.google.com/file/d/12KOZ5IOxI58vu2E_Vi0uYr6Y2Rc4LF6J/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/12xrl_SdC4XMCGJe8yB5m778TorUhtChM/view?usp=sharing) | [Free eBook](https://drive.google.com/file/d/1dPrYSzh5ymVe5Zadum3wW9adl3x7tLBB/view?usp=sharing)


## About Me
**Abdul Jaseem**, UC Architect and Corporate Trainer
- Got in to Networking & Collaboration due to not being able to get a job in Embedded Systems :)
- Learn at least a new thing everyday, Served Cisco TAC Bangalore Team for 5 Years
- From Karuvarakundu, Kerala, India
- Passionate about Technology, cars and driving
- Skills: Cisco UC, Microsoft UC, Genesys Cloud CX, Amazon Connect, Webex CC, Azure, AWS, Windows, vmware, Python, Automation
- Certs: CCIE Collab #59174, Teams Voice Engineer, DevNet, CCNP DC, CCNP Ent, CCNP Security, AWS, Azure, vmware VCP, CKA
- Mob: +91-859-0101-859 ([WhatsApp Preferred](https://wa.me/+918590101859))<br>
- [LinkedIn](https://in.linkedin.com/in/abdul-jaseem)
- [YouTube](https://www.youtube.com/@aj-labs)

## Helpful Links
- **CUCM - MS Teams Integration with CUBE** | [Reference](https://github.com/vpjaseem/collaboration/blob/main/Webinars/CUCM_MS_Teams_Integration.md) | [Part 1](https://youtu.be/3q0DsU73KpI) | [Part 2](https://youtu.be/PB83udFEhFg)
- [Install SSL Certificates in Cisco Router / CUBE](https://youtu.be/8pUtDOTw-HM?si=9z0fIDQJraC-qvgG)
- [Webex Calling IPs](https://help.webex.com/en-us/article/b2exve/Port-Reference-Information-for-Webex-Calling)
- [Webex Calling with Local Gateway official guide](https://help.webex.com/en-us/article/jr1i3r/Configure-Local-Gateway-on-Cisco-IOS-XE-for-Webex-Calling)
- [Webex Dial Plan Configuration](https://help.webex.com/en-us/article/n0xb944/Configure-trunks,-route-groups,-and-dial-plans-for-Webex-Calling#Cisco_Reference.dita_cd257f9a-1936-4b97-a473-64681b672bd3)
- [Webex Dial Plan by Country](https://help.webex.com/en-us/article/757iyo/Dial-plans-by-country#topic_hx1_mct_msb)
- [Twilio Elastic SIP Trunking with CUBE](https://assets.cdn.prod.twilio.com/documents/InteropGuide_Twilio_vCiscoUBE_CiscoUCM_Final_1.1.pdf)

## Webinar Unanswered Q&A
1.      
2.   

