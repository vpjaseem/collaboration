# Gen-AI with Genesys AI - Free Webinar (29/June/2024 08:00PM IST)
# Build & Deploy Advanced Genesys AI Chatbot
Are you ready to dive into the world of Artificial Intelligence and revolutionize your Contact Center? Join our exclusive free webinar on **"Gen-AI with Genesys AI - Building a Complete Genesys AI Chatbot from Scratch"**

In this webinar, you'll learn how to leverage the power of **Genesys AI** and **Gen-AI** to create a sophisticated chatbot that can enhance customer interactions, streamline operations, and provide exceptional user experiences. I will guide you through the entire process, from initial setup to deployment, ensuring you gain practical knowledge and hands-on skills.
<p align="center">
<img src="https://github.com/vpjaseem/collaboration/assets/67306692/61781bf1-db76-468c-869c-840b56197cbe" width="50%"  height="50%">
</p>

## Webinar Recordings:
- [Part 1](https://youtu.be/F6mbeBygAwU)
- [Part 2](https://youtu.be/pwTmDr9QiMA)

## Prerequisites
- Install [Webex App](https://www.webex.com/downloads.html) and try to join from PC (mobile app will have limited experience)
- Basic understanding of Contact Center, IVR, Call Queue
- Basic Knowledge of Web applications (HTML, Web Hosting, etc.)
- Though the session is Advanced, there are no prerequisites. Anyone can attend!

## Genesys Chatbot Flow Chart
![image](https://github.com/vpjaseem/collaboration/assets/67306692/9a11e155-ee0b-4653-8ac9-ac89e62e22e6)

## Configuration Steps
### 1. Basic Genesys Configurations (Agents, Agent Profiles, ACD Queue)
- Admin > People > Select the Agent > Role > Assign User Role User
- Admin > People > Select the Agent > Person Details > Add New Section > Agent > Name: Agent 01
- Admin > Contact Center > Wrap-up Codes > Add > GENERAL ENQUIRY, DEBIT CARD ISSUE, TRANSACTION ISSUE
- Admin > Contact Center > Queue > Add a Queue > AJ_BANK_CUSTOMER_SUPPORT, Associate wrap-up codes and members to it (Rest everything default)

### 2. Inbound Message Flow
- Architect > Inbound Message > AJ_BANK_MESGR_01_IN_MSG_FLOW >Start and End Nodes only > Publish
- Initial State > Start > Send Response > Call Task (TRANSFER_TO_DIGITAL_BOT_FLOW)
- TRANSFER_TO_DIGITAL_BOT_FLOW > Start > Call Digital Bot Flow > Transfer to ACD
  
### 3. Messenger Configuration
- Message > Messenger Configuration > New Configuration 

|#| CONFIGURATION ITEM | VALUE |
|-|---------------------------------------|-------------------|
1 | Set your Launcher Button Visibility	  | Show              |
2 | User Interface                        | On                |
3	| Messenger Homescreen	                | On                |
4	| Add a logo	                          | *Upload Logo*     |
5	| Select your Messenger Color           |	*Pick a Color*    |
6	| Align to	                            | Auto              |
7	| Select your Supported Languages	      | English           |
8	| Labels	                              | *Enter your own*  |
9	| Default Language	                    | English           |
10 | Messaging							| Enabled                        |
11 | Humanize your Conversation			| On                             |
12 | Bot Name							| *Enter your own*               |
13 | Bot Avatar							| *Upload Profile Image for bot* |
14 | Rich Text Formatting				| On                             |
15 | File Attachment					| On                             |
16 | Automatically Start Conversations	| Off                            |
17 | Attachments						| On                             |
18 | Typing Indicators					| On                             |
19 | Authentication						| Off                            |
20 | Co-browse							| Off                            |
21 | Knowledge Articles					| Off                            |
22 | Predictive Engagement				| Off                            |

### 4. Messenger Deployment
- Message > Messenger Deployment > New Deployment > Select Messenger Configuration and Inbound Message Flow
- Copy the Java Script

### 5. Deploying the JavaScript code in production website [ajcollab.com](https://ajcollab.com)
- Access the web application and paste the code in-between \<body\>SCRIPT_HERE\<\/body\>
```
<body>
<script type="text/javascript" charset="utf-8">
  (function (g, e, n, es, ys) {
    g['_genesysJs'] = e;
    g[e] = g[e] || function () {
      (g[e].q = g[e].q || []).push(arguments)
    };
    g[e].t = 1 * new Date();
    g[e].c = es;
    ys = document.createElement('script'); ys.async = 1; ys.src = n; ys.charset = 'utf-8'; document.head.appendChild(ys);
  })(window, 'Genesys', 'https://apps.usw2.pure.cloud/genesys-bootstrap/genesys.min.js', {
    environment: 'prod-usw2',
    deploymentId: 'XXXXXXXX-a18e-XXXX-XXXX-XXXXXXXXb845'
  });
</script>
</body>

```
### 6. Digital Bot Flow Deployment
- Architect > Digital Bot Flows > Add
- Go back to Inbound Message Flow and create new Task to Transfer to the above bot flow and at the end Transfer to ACD (Optional)
- Inbound Message Flow starting task should Greet (Send response) and Call Task to the Transfer Task
![image](https://github.com/vpjaseem/collaboration/assets/67306692/32f53929-c749-4982-978e-2c2a7f45c03d)

### 7. Salesforce basic configuration
#### 7.1. Contact Page layouts Customization
- While adding external contact, the Account Name is mandatory, we can disable that.
- Advanced Setup > Object Manager > Select Contact > Page layouts > Contact Layout > Account Name > Uncheck Required checkbox

#### 7.2. Add an External Contact
- Cases > New > Add a New Contact with Phone and Email

#### 7.3. Add an Case
- Cases > New > Add a case (You must associate the case with Contact Name created in the previous step)

#### 7.4. API User for Genesys Integration
- While integrating with Genesys, we need a user to authenticate with Salesforce API.
- Advanced Setup > Users > New > Email: apiuser2@ajcollab.com > Profile: System Administrator

Note: In production scenarios, make sure you get this from the Salesforce Administrator and also, this account should have access to resources where it is intended to access (Implement RBA in Salesforce)

#### 5. Generate Security Token for apiuser2@ajcollab.com
- Login to Salesforce as apiuser2@ajcollab.com > Settings > Reset My Security Token
- Token will be emailed, note the Token for future use

### 8. Genesys Integration with Salesforce CRM APIs
- Integrations > Salesforce Data Actions > AJ_BANK_SALESFORCE_CRM_INTEGRATION >Provide apiuser2@ajcollab.com, Password and Security Token
- Activate the integration

### 9. Salesforce CRM APIs & Genesys Data Action 
#### 9.1. GET_RECENT_CASE_BY_NUMBER - Collect the last opened case by external user with contact number
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
#### 9.2. GET_CASE_COMMENT_BY_CASE_ID - Collect the last case comment by Case ID
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
### 10. Natural Language Understanding (NLU) and AI Business Logic in Digital Bot Flow
Further customize the bot to interact with customer using Natural Language Understanding (NLU)
- Initial Greeting > Start > Jump to Reusable Task > INITIAL_TASK
- INITIAL_TASK > Wait for Input (This will trigger NLU)
- Add 2 TASKs to address the business use cases (CASE_STATUS_TASK and TRANSFER_TO_AGENT_TASK)
- CASE_STATUS_INTENT --> CASE_STATUS_TASK
- CASE_STATUS_INTENT --> Utterances
```
1
existing ticket
case status
What is the status of my case?
Can you tell me the status of my case?
Check the status of my case.
I want to know the status of my case.
How is my case progressing?
Can you update me on my case?
What's happening with my case?
Provide the status of my case.
Case status update, please.
Is there any update on my case?
How's my case doing?
I need information on my case status.
Can you give me a case status update?
What's the current status of my case?
Where is my case at right now?
```
- TRANSFER_TO_AGENT_INTENT --> TRANSFER_TO_AGENT_TASK
- TRANSFER_TO_AGENT_INTENT --> Utterances
```
2
transfer me
real agent
Start a chat with an agent.
Connect me to a chat agent.
Let me chat with a representative.
Can I chat with an agent?
I want to chat with an agent.
Initiate a chat with a representative.
Chat with a customer service agent.
I need help via chat.
Can you start a chat with an agent?
Get me a chat agent.
Chat with an agent, please.
I need to chat with a customer support agent.
Can I get some help via chat?
Please start a chat with an agent.
I want to chat with a live person.
```
- Slots (customer_number_slot > customer_number_slot_type > RegEx = \+?\d{7,15})
- CASE_STATUS_TASK > Ask for Slot > Ask for Yes/ No >

- No  > Update Data (clear the data) > Communicate > Jump to Reusable Task > CASE_STATUS_TASK
- Yes > Call Data Action > GET_RECENT_CASE_BY_NUMBER >

- Success > Decision (Task.CaseId != "UNKNOWN") > Yes > Communicate > "Your Existing case number is" > Call Data Action (GET_CASE_COMMENT_BY_CASE_ID) > Success > Decision (Task.CommentBody != "UNKNOWN") > Communicate > "Last Update" > Update Data > Jump to INITIAL_TASK
- Failure > Transfer


- TRANSFER_TO_AGENT_TASK > Start > Transfer to ACD
- Configure Intents, Utterances and Slots
![image](https://github.com/vpjaseem/collaboration/assets/67306692/81763795-51af-4084-bba6-090fb66d0f86)

- Build the logical flow (Salesforce API Calls, Data Actions)
![image](https://github.com/vpjaseem/collaboration/assets/67306692/93d93d3f-0cf7-471c-a0ee-284c4c090f00)

### 11. Fine-Tune the Genesys AI Bot
- Settings > Bot Flow, Event Handling
- Optimization Dashboard > Utterance History

### 12. Experience the fully functional Other Gen-AI Chatbot

![image](https://github.com/vpjaseem/collaboration/assets/67306692/3660bc55-65b2-4e49-9783-4e0f588a29e6)

#### AJ Bot 1 (Self Service Customer Support)
- [AJ Bot 1 Experience](https://ajcollab.com/bot1.html)
1. Bot designed to Open New Case
2. Check the case status in ticketing tool (Testing requires a registered number in the ticketing system, as an example you can give +918590101859)
3. Resetting the debit card pin and Chat with live agent
4. Chat with an live agent

#### AJ Bot 2 (With Advanced Authentications)
- [AJ Bot 2 Experience](https://ajcollab.com/bot2.html)
1. Bot designed to Check the case status in ticketing tool after OTP Authentication. Testing requires a registered number in the ticketing system, and OTP will be delivered there only
2. Chat with an live agent
3. **Twilio powered OTP Service** - Experience the authentication flow with OTP Service, you can test with your own number

## About Me
**Abdul Jaseem**, UC Architect and Corporate Trainer
- Got in to Networking & Collaboration due to not being able to get a job in Embedded Systems :)
- Learn at least a new thing everyday, Served Cisco TAC Bangalore Team for 5 Years
- From Karuvarakundu, Kerala, India
- Passionate about Technology, cars and driving
- Skills: Cisco UC, Microsoft UC, Genesys Cloud CX, Amazon Connect, Webex CC, Azure, AWS, Windows, vmware, Python, Automation
- Certs: CCIE Collab #59174, Teams Voice Engineer, DevNet, CCNP DC, CCNP Ent, CCNP Security, AWS, Azure, vmware VCP, CKA
- Mob: +91-859-0101-859 ([WhatsApp Preferred](https://wa.me/+918590101859))<br>
- [LinkedIn](https://in.linkedin.com/in/abdul-jaseem) | [Instagram](https://www.instagram.com/ajlabs.training/)
- [YouTube](https://www.youtube.com/@aj-labs)

## Helpful Links
- **CUCM - MS Teams Integration with CUBE** | [Reference](https://github.com/vpjaseem/collaboration/blob/main/Webinars/CUCM_MS_Teams_Integration.md) | [Part 1](https://youtu.be/3q0DsU73KpI) | [Part 2](https://youtu.be/PB83udFEhFg)
- [Webex Calling with Local Gateway](https://youtu.be/L6A-XFuMSgA?si=NaVT9KJOBrodYTZk)
- [Install SSL Certificates in Cisco Router / CUBE](https://youtu.be/8pUtDOTw-HM?si=9z0fIDQJraC-qvgG)
- [Salesforce CRM Free Subscription](https://www.salesforce.com/in/form/signup/freetrial-sales/)
- Salesforce APIs [Search API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_search.htm) | [Salesforce Object Query Language - SOQL and Salesforce Object Search Language - SOSL](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_sosl_intro.htm)
- [Salesforce Platform APIs for Postman](https://www.postman.com/salesforce-developers/workspace/salesforce-developers/collection/12721794-67cb9baa-e0da-4986-957e-88d8734647e2)

## Courses offered by AJ Labs
Below are the Training courses offered by AJ Labs.

1. **Advanced Genesys Cloud Contact Center Training [Live Training]** (Genesys Cloud CX, Advanced Architect Flows, CRM Integrations, APIs, Middleware Development, Genesys AI, Google Dilogflow CX, Advanced Agent Scripting) - [Syllabus](https://drive.google.com/file/d/10ZrdHtvt12sWcBwN_uirgiLwXnEL8o-S/view?usp=sharing) | Start Date **05/August/2024** | Join the [WhatsApp Group](https://chat.whatsapp.com/FjXHwk5QnPVCy0Oa2F7bS5)

2. **Advanced Cloud Collaboration [On demand Video Recordings]** (vmware, Windows AD, DNS, Azure, Azure AD, MS Teams, AudioCodes SBC, Direct Routing, Webex, Webex Calling with CUBE) - [Topic Summary](https://drive.google.com/file/d/1ezFuS6ulvLfyg6PQZXBnnH3McsTaqMWH/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/19zRydHD4n0drATeoFcNbjbfUphrA-IzJ/view?usp=sharing)

3. **Microsoft Teams Direct Routing with AudioCodes, Ribbon, CUBE, AnyNode SBCs - Fast Track** (Direct Routing implementation, PSTN Integration, RegEx, All Enterprise SBCs, Troubleshooting of each SBCs with logs) - [Udemy](https://www.udemy.com/course/microsoft-teams-direct-routing-with-all-sbcs-part-of-ms-721/?referralCode=43FA284904FB9F8FC80D)

4. **Advanced Cisco Collaboration Video Training [On demand Video Recordings]** (vmware, Windows, CUCM, CUC, IMP, Expressway, CUBE, CUBE HA, SIP Deep Dive, Log Analysis, UC Upgrade, MRA, B2B, Webex) - [Topic Summary](https://drive.google.com/file/d/12KOZ5IOxI58vu2E_Vi0uYr6Y2Rc4LF6J/view?usp=sharing) | [Syllabus](https://drive.google.com/file/d/12xrl_SdC4XMCGJe8yB5m778TorUhtChM/view?usp=sharing) | [Free eBook](https://drive.google.com/file/d/1dPrYSzh5ymVe5Zadum3wW9adl3x7tLBB/view?usp=sharing)

## Feedback
- [Share your Feedback](https://forms.gle/agei1sVg5gZA3FGbA)
## Webinar Unanswered Q&A
1. 
