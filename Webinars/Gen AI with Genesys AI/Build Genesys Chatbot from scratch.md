# Gen-AI with Genesys AI - Free Webinar (29/June/2024 08:30PM IST)
Are you ready to dive into the world of Artificial Intelligence and revolutionize your Contact Center? Join our exclusive free webinar on **"Gen-AI with Genesys AI - Building a Complete Genesys AI Chatbot from Scratch"**

In this webinar, you'll learn how to leverage the power of **Genesys AI** and **Gen-AI** to create a sophisticated chatbot that can enhance customer interactions, streamline operations, and provide exceptional user experiences. Our expert speaker will guide you through the entire process, from initial setup to deployment, ensuring you gain practical knowledge and hands-on skills.
<p align="center">
<img src="https://github.com/vpjaseem/collaboration/assets/67306692/e7fce97b-b7af-4a83-a0a8-e0d50726170b" width="50%"  height="50%">
</p>


## Prerequisites
- Install Zoom App: [Windows](https://zoom.us/download?os=win) | [MAC](https://zoom.us/download?os=mac) and try to join from PC (mobile app will have limited experience)
- Basic understanding of Contact Center, IVR, Call Queue
- Basic Knowledge of Web applications (HTML, Web Hosting, etc.)
- Though the session is Advanced, there are no prerequisites. Anyone can attend!

## Genesys Chatbot Flow Chart
![image](https://github.com/vpjaseem/collaboration/assets/67306692/9a11e155-ee0b-4653-8ac9-ac89e62e22e6)

## Gen-AI Communication Flow
![image](https://github.com/vpjaseem/collaboration/assets/67306692/93d93d3f-0cf7-471c-a0ee-284c4c090f00)

## Configuration Steps
### 1. Basic Genesys Configurations (Agents, Agent Profiles, ACD Queue)
- Admin > People > Select the Agent > Role > Assign User Role User
- Admin > People > Select the Agent > Person Details > Add New Section > Agent > Name: Aggent 01
- Admin > Contact Center > Wrap-up Codes > Add > GENERAL ENQUIRY, DEBIT CARD ISSUE, TRANSACTION ISSUE
- Admin > Contact Center > Queue > Add a Queue > AJ_BANK_CUSTOMER_SUPPORT, Associate wrap-up codes and members to it (Rest everything default)

#### 2. Inbound Message Flow

#### 3. Digital Bot Flow Configuration
#### 4. Digital Bot Flow Deployment
#### 5. Deploying the Javascript code in production website [ajcollab.com](https://ajcollab.com)

### 6. Salesforce basic configuration
#### 1. Contact Page layouts Customization
While adding external contact, the Account Name is mandatory, we can disable that.
Advanced Setup > Object Manager > Select Contact > Page layouts > Contact Layout > Account Name > Uncheck Required checkbox

#### 2. Add an External Contact
Cases > New > Add a New Contact with Phone and Email

#### 3. Add an Case
Cases > New > Add a case (You must associate the case with Contact Name created in the previous step)

#### 4. API User for Genesys Integration
While integrating with Genesys, we need a user to authenticate with Salesforce API.
Advanced Setup > Users > New > Email: apiuser2@ajcollab.com > Profile: System Administrator

Note: In production scenarios, make sure you get this from the Salesforce Administrator and also, this account should have access to resources where it is intented to access (Implement RBA in Salesforce)

### 5. Generate Security Token for apiuser2@ajcollab.com
Login to Salesforce as apiuser2@ajcollab.com > Settings > Reset My Security Token
Token will be emailed, note the Token for future use

### 6. Genesys Integration with Salesforce CRM APIs
Integrations > Salesforce Data Actions > Provide apiuser2@ajcollab.com, Password and Security Token
Activate the integration

### 7. Salesforce CRM APIs & Genesys Data Action 
#### 1. GET_RECENT_CASE_BY_NUMBER - Colelct the last opened case by external user with contact number
##### Postman URL
```
{{_endpoint}}/services/data/v{{version}}/query/?q=SELECT Id, CaseNumber, Subject, Description FROM Case WHERE ContactPhone='%2B12146605526' AND IsClosed = false ORDER BY LastModifiedDate DESC LIMIT 1
```
##### Genesys Integration Data Action URL
```
/services/data/v60.0/query/?q=$esc.url("SELECT Id, CaseNumber, Subject, Description FROM Case WHERE ContactPhone='${input.PHONE_NUMBER}' AND IsClosed = false ORDER BY LastModifiedDate DESC LIMIT 1")
```
##### Genesys Data Action Translations (JSONPath Expression)
```
{
  "translationMap": 
  {
    "CaseNumber": "$.records[0].CaseNumber",
    "CaseId": "$.records[0].Id",
    "Description": "$.records[0].Description",
    "Subject": "$.records[0].Subject"
  },

  "translationMapDefaults": 
  {
    "CaseNumber": "\"UNKNOWN\"",
    "CaseId": "\"UNKNOWN\"",
    "Description": "\"UNKNOWN\"",
    "Subject": "\"UNKNOWN\""
  },

  "successTemplate": "{\"CaseId\": ${CaseId}, \"CaseNumber\": ${CaseNumber}, \"Subject\": ${Subject}, \"Description\": ${Description}}"
}

```
#### 2. GET_CASE_COMMENT_BY_CASE_ID - Collect the last case comment by Case ID
##### Postman URL
```
{{_endpoint}}/services/data/v{{version}}/query/?q=SELECT CommentBody, CreatedDate FROM CaseComment WHERE ParentId='500IS0000044q1CYAQ' ORDER BY LastModifiedDate DESC LIMIT 1
```
##### Genesys Integration Data Action URL
```
/services/data/v60.0/query/?q=$esc.url("SELECT CommentBody, CreatedDate FROM CaseComment WHERE ParentId='${input.caseId}' ORDER BY LastModifiedDate DESC LIMIT 1")
```

##### Genesys Data Action Translations (JSONPath Expression)
```
{
  "translationMap": 
  {
    "CreatedDate": "$.records[0].CreatedDate",
    "CommentBody": "$.records[0].CommentBody"
  },
  
  "translationMapDefaults": 
  {
    "CreatedDate": "\"UNKNOWN\"",
    "CommentBody": "\"UNKNOWN\""
  },

  "successTemplate": "{\"CreatedDate\": ${CreatedDate}, \"CommentBody\": ${CommentBody}}"

}

```

#### 10. Experince the fully functional Gen-AI Chatbot

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
- [Webex Calling with Local Gateway](https://youtu.be/L6A-XFuMSgA?si=NaVT9KJOBrodYTZk)
- [Install SSL Certificates in Cisco Router / CUBE](https://youtu.be/8pUtDOTw-HM?si=9z0fIDQJraC-qvgG)
- [Salesforce CRM Free Subscription](https://www.salesforce.com/in/form/signup/freetrial-sales/)
- [Salesforce Platform APIs for Postman](https://www.postman.com/salesforce-developers/workspace/salesforce-developers/collection/12721794-67cb9baa-e0da-4986-957e-88d8734647e2)

## Courses offered by AJ Labs
Below are the Training courses offered by AJ Labs.

1. **Advanced Genesys Cloud Contact Center Training [Live Training]** (Genesys Cloud CX, Advanced Architect Flows, CRM Integrations, APIs, Middleware Development, Genesys AI, Google Dilogflow CX, Advanced Agent Scripting) - [Syllabus]() | Start Date **05/August/2024** | Join the [WhatsApp Group](https://chat.whatsapp.com/FjXHwk5QnPVCy0Oa2F7bS5)

2. **Advanced Cloud Collaboration [On demand Video Recordings]** (vmware, Windows AD, DNS, Azure, Azure AD, MS Teams, AudioCodes SBC, Direct Routing, Webex, Webex Calling with CUBE) - [Topic Summary](https://drive.google.com/file/d/1ezFuS6ulvLfyg6PQZXBnnH3McsTaqMWH/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/19zRydHD4n0drATeoFcNbjbfUphrA-IzJ/view?usp=sharing)
   
3. **Advanced Cisco Collaboration Video Training [On demand Video Recordings]** (vmware, Windows, CUCM, CUC, IMP, Expressway, CUBE, CUBE HA, SIP Deep Dive, Log Analysis, UC Upgrade, MRA, B2B, Webex) - [Topic Summary](https://drive.google.com/file/d/12KOZ5IOxI58vu2E_Vi0uYr6Y2Rc4LF6J/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/12xrl_SdC4XMCGJe8yB5m778TorUhtChM/view?usp=sharing) | [Free eBook](https://drive.google.com/file/d/1dPrYSzh5ymVe5Zadum3wW9adl3x7tLBB/view?usp=sharing)

## Webinar Unanswered Q&A
  
