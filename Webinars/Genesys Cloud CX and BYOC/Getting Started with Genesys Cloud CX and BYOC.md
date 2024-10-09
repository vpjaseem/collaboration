Join me for an exclusive webinar where we'll dive deep into telephony integration with Genesys Cloud CX using AudioCodes SBC. Learn how to configure and secure SIP trunking in a Bring Your Own Carrier (BYOC) environment! Webinar is going to be 100% hands-on lab with live demostrations and Q&A.
# Topic Summary
1. Genesys Cloud CX Interface Walkthrough
2. Understanding User Permissions
3. User Management in Genesys Cloud CX
4. Genesys Cloud CX Edge Server
5. Understanding Location and Sites
6. Configuring Genesys Location & Site
7. Configuring BYOC Trunk
8. WebRTC Phone Configuration
9. Creating Queue, Wrap-up Codes
10. Build a Basic Genesys Architect Inbound Call Flow
11. DID Configuration
12. Call Route
13. Tesing the IVR
14. Configuring Salesforce CRM Integration and Data Action
15. Building Additional Logic in Architect Flow

# 1. Genesys Cloud CX Interface Walkthrough

- The Genesys cloud access URL would differ depending on the base region chosen while signing up.
- [Login URL for each region](https://help.mypurecloud.com/articles/aws-regions-for-genesys-cloud-deployment/)
- Genesys Web portal is a role-driven application, we could see options in the portal based on the Roles.

![1](https://github.com/user-attachments/assets/5ca2e4ef-6131-41fc-9652-99ebad3b0daa)


# 2. Understanding User Permissions
- Genesys Cloud is a Role Driven Application
- The role is a combination of multiple Permissions
- A permission decides what exactly a user can do in Genesys Cloud (Basic User, Agent, Supervisor, Master Administrator, Telephony Admin, Integration Admin, etc.)
- The role assigned to a user decides which license to use (Collaborate, Communicate, CX1, CX2, CX3)
![1](https://github.com/user-attachments/assets/1a45f3cb-4333-461f-9051-234fd3ad1f96)
![2](https://github.com/user-attachments/assets/49b2cd47-6a62-4831-bac9-dbd5ef319139)
![3](https://github.com/user-attachments/assets/43ff10f9-9745-48ad-8294-5aa292cc4baf)

# 3. User Management in Genesys Cloud CX
- Users can be created manually, bulk-imported with CSV, APIs, or SCIM (Cloud IdP Push)
## 3.1. Creating New User in Genesys Cloud CX
![4](https://github.com/user-attachments/assets/4efab5bd-0a22-4a8e-ac86-e1d0b53cab86)
![5](https://github.com/user-attachments/assets/26f89e5a-4b88-4a95-9d79-86e42c394050)


## 3.2. Promoting the new user as a Contact Center Agent
![6](https://github.com/user-attachments/assets/5765f7ad-c85d-48fa-b2cd-7db2f5e582f9)
![7](https://github.com/user-attachments/assets/908ca42e-970d-4441-91e5-a50fd594df06)

# 3. Genesys Cloud CX Edge Server
- Edge is available as Cloud-based and Premises
- The Edge is a cloud-managed device that combines a Media server, SIP registrar, SIP proxy, and call recorder into a single piece of hardware on-premises.
- Phone Trunk and External Trunks talks to Edge Server
- In Genesys Cloud CX Voice and BYOC Cloud, the Edge is in AWS, and Genesys fully manage it
- Edge servers are grouped to Edge Group and made available to Sites
- In a fully cloud solution, the cloud edge group is associated with the core site - PureCloud Voice – AWS, and the rest of the sites share the same
- 
![image](https://github.com/user-attachments/assets/f9787df8-c3fe-4b7c-893a-ec47c038fa08)

# 4. Understanding Location and Sites
## 4.1 Location
- A Genesys Cloud CX location is a geographic area of an organization where users are located.
- For organizations with multiple physical sites, locations are used to configure emergency services (such as E911 in the US), ensuring that emergency calls are routed to the appropriate local authorities.
- Locations can be used in reporting and analytics to understand the performance and operations of different geographical areas.
- Location will be displayed under the user profile can easily identify where the user belongs to
- To create a Site, we should have a Location first

## 4.2 Site
- It is the home of a set of phones and edge devices and defines the Telephony properties (Dial-Plan, Outbound Routes)
- It has RegEx based dial-plan, Trunk Association, etc.
- Every site must be associated with one location. An organization can have multiple locations assigned to it in Genesys Cloud CX.
- In a Fully Cloud Edge Solution, any site that you create obtains Edge details from the core site (PureCloud Voice - AWS)

# Configuring Genesys Location & Sites
![Location 1](https://github.com/user-attachments/assets/4f415759-9656-481c-8dd7-9bf93bfc3313)
![Site 1](https://github.com/user-attachments/assets/bb32bf14-aba0-43c7-aa22-93d834955311)

# Configuring BYOC Trunk
![1](https://github.com/user-attachments/assets/4430a059-bc4d-436f-bbb0-762f22667b27)
![2](https://github.com/user-attachments/assets/34992916-7dda-41a0-8cdf-148dad89de60)
![3](https://github.com/user-attachments/assets/5b689e65-a96b-4256-9458-afd86f8f97ec)
![4](https://github.com/user-attachments/assets/c644872b-909d-450b-a4e0-93dccca49efb)
![5](https://github.com/user-attachments/assets/5250f4b2-7dd7-495c-baee-0d71a3683711)
![6](https://github.com/user-attachments/assets/0675c5b7-a7bc-4e4b-ba39-3b51a50f7f37)
![7](https://github.com/user-attachments/assets/4694f976-8667-40d3-a9d6-b40cef5c34b4)
![8](https://github.com/user-attachments/assets/160d34ef-06f7-414e-b296-7c440b184bb3)
![9](https://github.com/user-attachments/assets/3894aeea-3ea0-452a-8f99-f314db0e0ed2)


# WebRTC Phone Configuration
![1](https://github.com/user-attachments/assets/c21bc71a-597a-4c9c-8b39-890722f13ed7)
![2](https://github.com/user-attachments/assets/d7b787e5-554e-497e-8c2b-7927dffd0a95)
![3](https://github.com/user-attachments/assets/84123f55-f558-4717-8b72-08d00d84cb64)

# Creating Queue, Wrap-up Codes
![1](https://github.com/user-attachments/assets/82cc6648-6dbc-4e70-84af-51cc17c325a4)
![2](https://github.com/user-attachments/assets/97bab25f-4721-4d2a-a26a-5947aa40eb40)
![3](https://github.com/user-attachments/assets/a3bd7ee2-df5c-49f2-a101-34319678043a)
![4](https://github.com/user-attachments/assets/f58c7878-fdc1-4d39-a45f-f53428994220)
![5](https://github.com/user-attachments/assets/713fec66-0466-4b10-9e2c-52645c9d1a17)

# Build a Basic Genesys Architect Inbound Call Flow
![1](https://github.com/user-attachments/assets/17cd2a0c-c39a-4091-9f73-988b5b6ab38e)
![2](https://github.com/user-attachments/assets/4e364680-9170-4c99-a772-28f93cf23e77)
![3](https://github.com/user-attachments/assets/2da1b081-063a-4f6f-8bf7-4619372e9fbb)

# DID Configuration
![5](https://github.com/user-attachments/assets/c56369fb-c6bc-4921-bc09-a6764a0f6c76)


# Call Route
![4](https://github.com/user-attachments/assets/88a3e485-1781-4263-9f42-e49bd25355e6)

# Deploy AudiCodes SBC in Azure Cloud

# SSL Certificate Configuration in AudioCodes

# Integrate AudiCodes SBC with Genesys Cloud CX and PSTN

| SCOPE   | PARAMETER            | VALUE                           | COMMENTS                                          |
|---------|----------------------|---------------------------------|---------------------------------------------------|
| Generic | Primary NTP Server   | 216.239.35.8                    | This is Google NTP Server, you can use any        |
|         | Secondary NTP Server | time.google.com                 | This is Google NTP Server, you can use any        |
|         | Secondary DNS        | 8.8.8.8                         | This is Google Public DNS Server, you can use any |
|         | NAT Public IP        | Public IP of Azure SBC VM       | Get this from your Azure Portal                   |
| Genesys | IP-PBX Address       | aj-genesys.byoc.usw2.pure.cloud | Genesys Trunk Termination identifier              |
|         | IP-PBX SIP Domain    | aj-genesys.byoc.usw2.pure.cloud | Genesys Trunk Termination identifier              |
|         | Keep Alive           | Enabled                         | For OPTIONS Ping status                           |
|         | Transport Type       | TLS                             | Secure Signaling                                  |
|         | Destination Port     | 5061                            | Secure SIP (SSIP) Port                            |
|         | Listening Port       | 5061                            | Secure SIP (SSIP) Port                            |
|         | Media Protocol       | SRTP                            | Secure Media                                      |
| Twilio  | SIP Trunk Address    | aj-twilio.pstn.twilio.com       | Twilio Termination SIP URI                        |
|         | SIP Trunk SIP Domain | aj-twilio.pstn.twilio.com       | Twilio Termination SIP URI                        |
|         | Keep Alive           | Enabled                         | For OPTIONS Ping status                           |
|         | Transport Type       | TLS                             | Secure Signaling                                  |
|         | Destination Port     | 5061                            | Secure SIP (SSIP) Port                            |
|         | Listening Port       | 5061                            | Secure SIP (SSIP) Port                            |
|         | Media Protocol       | SRTP                            | Secure Media                                      |


# Tesing the IVR

# Configuring Salesforce CRM Integration and Data Action

# Building Additional Logic in Architect Flow

