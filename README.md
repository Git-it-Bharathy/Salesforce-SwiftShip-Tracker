# SwiftShip Tracker

**A Salesforce parcel booking and tracking solution with a conversational AI agent (Agentforce + Prompt Builder + Flow).**

> Salesforce Developer Project | Built in a Salesforce Developer Edition org

**Team Leader:** Bharathy S ([@Git-it-Bharathy](https://github.com/Git-it-Bharathy))
**Team Members:** Saranraj, Somesh, Harristarun, Sriram Kaviyan
**Demo video:** _(paste your YouTube / Google Drive link here)_

---

## 1. Overview

Customers often have to call support or use several apps to book a parcel or find out where it is. SwiftShip Tracker brings parcel information into one Salesforce org. Parcel, Delivery, Sender and Receiver data is stored in custom objects with relationships between them, and an Agentforce agent lets a user get parcel details just by typing a Parcel ID.

## 2. What is implemented

| Area | What was built |
|---|---|
| Data model | Four custom objects: `Parcel__c`, `Delivery__c`, `Sender__c`, `Receiver__c`, linked with lookup relationships |
| Tabs | A custom tab for each object |
| Parcel fields | Parcel ID (auto number `P-{000}`), Status (Booked, In Transit, Out for Delivery, Delivered), Weight, Estimated Delivery Date, Sender (lookup) |
| Delivery fields | Current Location (geolocation), Estimated Delivery Date, Sender (lookup), Parcel (lookup) |
| Sender fields | Sender Address (geolocation), Sender Contact (phone), Sender Email |
| Receiver fields | Receiver Address (geolocation), Receiver Contact (phone), Receiver Email, Sender (lookup), Parcel (lookup) |
| Prompt Builder | A Record Summary prompt template, **Retrieve Parcel Details**, that formats a parcel's details |
| Flow | An Autolaunched Flow, **Parcel Details**: Get Records, then the prompt template action, then an output assignment |
| Security | A **Swift Ship** permission set with Read, Create and Edit on all four objects, assigned to the user |
| AI agent | An Agentforce agent, **SwiftShip Tracker**, with a **Parcel Details Inquiry** subagent that runs the Flow as an action |

## 3. How it works

```
User asks: "Can I get my parcel details?"
        |
        v
Agentforce agent (SwiftShip Tracker)
        |   asks for the Parcel ID, e.g. P-001
        v
Subagent: Parcel Details Inquiry
        |
        v
Agent action -> Flow: Parcel Details
        |   Get Records: Parcel where Parcel_ID__c = input
        v
Prompt Template: Retrieve Parcel Details
        |
        v
Reply: Parcel Name, Parcel ID, Status, Weight, Estimated Delivery Date
```

## 4. Data model

```
Sender__c  <----  Parcel__c  ---->  (Sender lookup)
   ^                  ^
   |                  |
Receiver__c ----------+      Delivery__c ----> Parcel__c, Sender__c
```

- A **Parcel** has a Sender.
- A **Delivery** is linked to a Parcel and a Sender and stores the current location.
- A **Receiver** is linked to a Sender and a Parcel.

## 5. Sample test

| Input | Result |
|---|---|
| `Can I get my parcel details?` then `P-001` | The agent returns the Parcel Tracking Update: Parcel Name: Laptop Parcel, Parcel ID: P-001, Status: Booked, Weight: 10 kg, Estimated Delivery Date |

## 6. Repository structure

```
SwiftShip-Tracker/
├── README.md
├── Phase-Templates/        <- the filled phase-wise templates
├── Screenshots/
│   ├── 01-objects/
│   ├── 02-tabs/
│   ├── 03-fields/
│   ├── 04-sample-data/
│   ├── 05-prompt-builder/
│   ├── 06-flow/
│   ├── 07-permission-set/
│   └── 08-agent-and-testing/
└── Demo/                   <- demo video link
```

## 7. How to recreate this project

1. Create a free Developer Edition org at https://developer.salesforce.com/signup
2. Create the four custom objects and their tabs
3. Add the fields and lookup relationships listed above
4. Create sample records in this order: Sender, Parcel, Receiver, Delivery
5. In Prompt Builder, create a Record Summary template on `Parcel__c` and activate it
6. Build the Autolaunched Flow with the variables `ids` (input) and `output` (output)
7. Create the Swift Ship permission set and assign it to your user
8. Turn on Einstein and Agentforce, create the Parcel Details Inquiry subagent, add the Flow as an agent action, then activate the agent

## 8. Future scope

- Automatic status updates and customer email notifications using record-triggered Flows and Email Alerts
- Apex triggers and Batch Apex for validation and periodic updates
- Reports and dashboards for parcel status and delivery performance
- Experience Cloud site so customers can track parcels themselves
- Agent actions that update parcel status, and integration with external logistics systems

## 9. Technologies

Salesforce Developer Edition, Custom Objects, Prompt Builder, Salesforce Flow, Permission Sets, Agentforce (Agent Builder)

---

_Developed as part of a Salesforce Developer course project._
