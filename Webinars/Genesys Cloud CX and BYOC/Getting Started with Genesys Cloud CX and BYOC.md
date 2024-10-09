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

# Genesys Cloud CX Edge Server
- Edge is available as Cloud-based and Premises
- The Edge is a cloud-managed device that combines a Media server, SIP registrar, SIP proxy, and call recorder into a single piece of hardware on-premises.
- Phone Trunk and External Trunks talks to Edge Server
- In Genesys Cloud CX Voice and BYOC Cloud, the Edge is in AWS, and Genesys fully manage it
- Edge servers are grouped to Edge Group and made available to Sites
- In a fully cloud solution, the cloud edge group is associated with the core site - PureCloud Voice – AWS, and the rest of the sites share the same
- 
![image](https://github.com/user-attachments/assets/f9787df8-c3fe-4b7c-893a-ec47c038fa08)
