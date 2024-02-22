# Webex Calling with CUBE - Free Webinar (24/Feb/2024 09:00PM IST)
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
Image here

## Dial Plan Design

## Configurations Steps
**Azure**
1. Azure Marketplace > Cisco Virtual CUBE Session Border Controller > Create > Select Subscription and follow the VM creation wizard and complete the deployment
2. Configure Network Security Group to allow required ports (SSH 22, SIP 5060 - 5080, Media 16384 - 32787)

**Webex Side**
1. Add Location (I have used this address 2770 E Trinity Mls Rd, Carrollton, TX 75006, USA), Add Trunk, Route Group, Dial Plan, Add Numbers
2. Enable Premise based PSTN
3. Assign License and Location and Number to the user

**CUBE Side**
1. Webex Trunk and CUBE Configuration Mapping
![image](https://github.com/vpjaseem/collaboration/assets/67306692/05e3b699-844b-4f9d-8c09-a14826ef82d5)

2. CUBE Configurations
```
Configurations here!
```
**PSTN Twilio Side**
1. sdzzzz

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

