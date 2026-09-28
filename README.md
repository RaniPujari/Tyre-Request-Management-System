🚗 Tyre Request Management System
An enterprise-grade custom ServiceNow application designed to streamline, automate, and secure the end-to-end lifecycle of tyre procurement and replacement requests.

📌 Executive Summary
The Tyre Request Management application replaces manual tracking with a centralized, role-based workflow inside ServiceNow. Built with security and operational efficiency at its core, it leverages custom data models, granular Access Control Lists (ACLs), automated server-side business logic, and deployment-ready Update Sets to deliver a robust request fulfillment engine.

🎯 Core Objectives
Centralized Administration: Single source of truth for all organizational tyre requests.

Role-Based Security: Strict record and field-level access control via custom ACLs and roles.

Automated Processing: Server-side logic via Business Rules to enforce validation and routing.

Seamless Deployment: Fully packaged into ServiceNow Update Sets for smooth migration across DEV, TEST, and PROD environments.

🗂️ Data Architecture & Schema
Custom Table: u_tyre_request
Field Label	Element Name	Data Type	Description / Notes
Request Number	u_number	Auto-Number	Unique identifier (e.g., TYR0001001)
Requested By	u_requested_by	Reference	Points to sys_user
Tyre Type	u_tyre_type	Choice	All-Terrain, Performance, Winter, Commercial
Tyre Size	u_tyre_size	String	Standard tire dimensions (e.g., 225/65R17)
Quantity	u_quantity	Integer	Units requested
Assignment Group	u_assignment_group	Reference	Points to sys_user_group
Assigned To	u_assigned_to	Reference	Dependent on u_assignment_group
State	u_state	Choice	Draft ➔ Pending Approval ➔ Work in Progress ➔ Closed Complete
Priority	u_priority	Choice	Critical, High, Moderate, Low
👥 Access Management Matrix
To maintain data integrity and satisfy audit compliance, security is enforced across four distinct user personas:

                  ┌─────────────────────────┐
                  │    Tyre Request Admin   │
                  └────────────┬────────────┘
                               │
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
┌───────────────────────┐             ┌───────────────────────┐
│ Tyre Request Manager  │             │  Tyre Request Agent   │
└───────────────────────┘             └───────────────────────┘
            │                                     │
            └──────────────────┬──────────────────┘
                               ▼
                  ┌─────────────────────────┐
                  │    Tyre Request User    │
                  └─────────────────────────┘
Persona / Role	Role Name	Create	Read	Write	Delete	Scope & Responsibilities
User	x_tyre_req.user	🟢	🟡	🟡	🔴	Creates requests; views and edits own records only
Agent	x_tyre_req.agent	🟢	🟢	🟢	🔴	Works assigned tickets; updates state, notes, and fulfillment details
Manager	x_tyre_req.manager	🟢	🟢	🟢	🟡	Oversees team requests, approves high-priority orders, cancels requests
Admin	x_tyre_req.admin	🟢	🟢	🟢	🟢	Full administrative access, schema management, and hard deletion rights
🟢 Allowed | 🟡 Conditional / Restricted | 🔴 Denied

⚙️ Automated Business Logic
The platform uses server-side Business Rules (before and after) to maintain data hygiene and automate routing:

Data Validation (before insert / update): Validates that u_quantity is greater than 0 and formats u_tyre_size strings prior to committing to the database.

Auto-Routing (before insert): Automatically populates u_assignment_group based on the requested location or vehicle type if left empty.

Deletion Guard (before delete): Blocks non-admin users from deleting active or completed requests, preserving audit trails.

State Cascading (before update): Enforces mandatory fields (e.g., Work Notes or Closure Code) before transitioning to Closed Complete.

🔄 End-to-End Workflow
 ┌──────────────┐     ┌──────────────────┐     ┌──────────────────┐
 │ Request      │ ──> │ Validation &     │ ──> │ Group & Agent    │
 │ Creation     │     │ Auto-Assignment  │     │ Assignment       │
 └──────────────┘     └──────────────────┘     └──────────────────┘
                                                        │
                                                        ▼
 ┌──────────────┐     ┌──────────────────┐     ┌──────────────────┐
 │ Request      │ <── │ Fulfillment &    │ <── │ Work In          │
 │ Closure      │     │ Verification     │     │ Progress         │
 └──────────────┘     └──────────────────┘     └──────────────────┘
📦 Deployment & Installation Guide
This application is packaged as a scoped/global ServiceNow Update Set.

Deployment Steps
Log in to the target instance as an Administrator.

Navigate to System Update Sets ➔ Retrieved Update Sets.

Click Import Update Set from XML and select the .xml payload from this repository.

Open the retrieved Update Set record and click Preview Update Set.

Review the preview logs and resolve any reported conflicts or missing dependencies.

Click Commit Update Set to deploy all schema, ACLs, Business Rules, and Form Layouts.

Verify installation by searching for Tyre Requests in the Application Navigator.

🧠 Core Technical Competencies Demonstrated
Data Modeling: Extended application architecture, dictionary overrides, and field dependencies.

Platform Security: High-assurance ACL implementation (table-level, field-level, and script-based conditions).

Scripting & Logic: Server-side JavaScript (GlideRecord, g_scratchpad, Business Rules).

UI/UX Design: Form layout optimizations, view rules, list mechanics, and related lists.

ALM & DevOps: Application lifecycle management using Update Set strategies and conflict resolution.

🚀 Roadmap & Future Enhancements
[ ] Service Catalog Integration: Move end-user submission to Service Portal / Employee Center via Catalog Items & MRVS.

[ ] Flow Designer & Approvals: Replace basic logic with visual Flows, integrating multi-tier manager approvals.

[ ] SLA Engine: Attach Service Level Agreements to monitor fulfillment response and resolution times.

[ ] Inventory Hub Integration: Connect via REST Web Services to external ERP/Inventory software for real-time stock checks.

👩‍💻 Author
Rani Mahadev Pujari

ServiceNow Developer | Data & AI Enthusiast
