# Zero to Snowflake: Admin - Speaker Notes

Copy each slide's speaker notes into Google Slides. Demo callouts use format: **[DEMO: Part X, Step Y]** or **[DEMO: Part X, Steps Y-Z]**

---

## Slide 1: Title Slide

Welcome everyone to Zero to Snowflake for Administrators. My name is Dylan and I'm a Solutions Engineer at Snowflake.

Before we dive in, a quick word on who this session is for. If you're a Snowflake administrator, this is your practical walkthrough of the core admin building blocks -- setting up warehouses and cost controls, designing roles and permissions, and applying governance and security patterns properly.

If you're a Snowflake user, this will make access and governance feel less mysterious. You'll understand how roles and policies impact what you can see and do, and what options exist to protect and share the work you create.

Either way, you'll leave here feeling confident about how Snowflake works under the hood.

---

## Slide 2: Safe Harbor

Quick legal housekeeping. Let's get into it.

---

## Slide 3: Agenda

Here's our roadmap for the next 90 minutes. We'll start with a high-level overview of the Snowflake platform, then walk through the Snowsight UI, role-based access control, data governance, managing compute, cost governance, Trust Center, and Cortex Code.

I'd love questions throughout. The goal isn't to memorize SQL commands -- the code is in the notebook and the docs. The goal is to understand why these features matter and when to reach for them.

---

## Slide 4: What is Snowflake (Easy, Connected, Trusted)

What is Snowflake at the highest level? Our 3 core principles are Easy, Connected, Trusted.

Easy means fully managed. Think of it like a modern apartment building -- you don't maintain the plumbing, the electrical, the elevators. Building management handles that. You just move in and live your life. Same idea here: no servers to provision, no storage to tune. You focus on what moves the business forward.

Connected means data sharing is built in. Share live data across teams or organizations without copying it.

Trusted means governance from day one. Role-based access control, encryption, masking policies -- not bolted on as an afterthought. We'll spend a good chunk of today on this.

---

## Slide 5: Single Unified Platform (Data, Compute, AI)

Same idea with one addition: AI. Snowflake isn't just storage and compute anymore -- it's a platform where you bring data, run compute against it, and layer AI on top, all in one place.

Why does that matter for administrators? Because you don't need to move your data somewhere else to do machine learning or run a large language model against it. The data stays put, the governance you set up still applies, and the AI runs where the data already lives.

Using AI inside Snowflake gives you -- no data movement, no extra security headaches, no separate platform to govern.

We'll see this in action later when I show you Cortex Code.

Takeaway: One platform for data, compute, and AI. Governance follows your data everywhere, even into AI workloads.

---

## Slide 6: All Types of Data

Snowflake handles all types of data in one place. Structured data like your traditional rows and columns. Semi-structured like JSON and XML. And unstructured -- PDFs, images, audio, video.

Why does this matter? Most organizations have their structured data in one system, their documents in another, their images somewhere else. That creates silos, and silos create governance headaches. Snowflake lets you bring it all together under one roof with one set of security policies.

---

## Slide 7: Comprehensive Compute

This slide shows the compute side. Snowflake gives you two types of compute: virtual warehouses and containers.

A simple way to think about it -- warehouses are your everyday workhorses. They run SQL queries, handle your ETL, power your dashboards. Containers are for when you need to bring your own code or run custom applications, like a Docker container running a Python model.

On the hardware side, CPUs handle general-purpose analytical workloads. GPUs are available when you need to run AI and ML workloads -- things like training models or running inference at scale.

AND you can write in SQL, Python, Java, or Scala -- whatever your team is comfortable with.

Takeaway: Warehouses for SQL workloads, containers for custom code. CPUs for general work, GPUs for AI.

---

## Slide 8: Leading Models in Snowflake

At the foundation, Snowflake gives you access to leading AI models from providers like OpenAI, Anthropic and others -- all running inside Snowflake.

The key point for administrators: all of this runs inside the same governance boundary you already manage. The roles, the masking policies, the access controls -- they all still apply. AI doesn't get a back door around your security.

---

## Slide 9: Cortex AI Architecture

This architecture diagram shows how all of those AI capabilities fit together inside Snowflake. At the bottom you have data ingestion -- documents, audio, images, structured data -- all landing in Snowflake's open formats. In the middle layer, the models we just talked about power everything from text processing to embeddings.

On top of that, you get purpose-built services. Cortex Analyst lets business users ask questions of structured data in natural language. Cortex Search does the same for unstructured documents.

And wrapping around all of it -- governance. Same RBAC, same masking policies, same audit trail. AI doesn't get a free pass on security.

For administrators, the key takeaway is that you don't need to stand up separate AI infrastructure. It's all governed under the same framework I'll soon show you in Snowsight.

---

## Slide 10: Snowsight UI Overview

Alright, let's shift gears from what Snowflake can do to where you'll actually do it.

**[DEMO: Open Snowsight in browser]**

Snowsight is Snowflake's web interface -- this is where most of your day-to-day administrative work happens. It's home base for Snowflake. You'll write queries, manage users, monitor warehouses, review costs, and set up governance all from this one place. Let's walk through it.

---

## Slide 11: Snowsight - Discover, Analyze, Manage

Snowsight is organized around three core workflows: Discover, Analyze and Manage.

Discover is about finding what already exists -- searching across databases, tables, queries, and results. Instead of asking a colleague "where's that revenue table?", you just search for it.

Analyze is the SQL editor -- a workspace with smart autocomplete, automatic column stats, and built-in charting. Your analysts live here.

Manage is where administrators spend their time. This is your control panel for warehouses, users, roles, resource monitors, and cost tracking.

**[Point out the left navigation panel in Snowsight as you mention each section]**

Takeaway: One UI, three workflows.

---

## Slide 12

**SKIP**

---

## Slide 13: Object Hierarchy

This is one of the most important mental models for a Snowflake admin. Think of it like Russian nesting dolls. At the outermost layer, you can optionally have an Organization -- that's for companies managing multiple Snowflake accounts across regions or cloud providers. Inside that, you have Accounts. Inside each account, you have Databases. Inside each database, Schemas. And inside each schema, your actual objects -- tables, views, stages, stored procedures, and so on.

The key rule: every object lives inside its container and can't exist outside it. A table can't float around without a schema. A schema can't exist without a database. This is important for access control because to reach a table, a user's role needs privileges on the database AND the schema AND the table itself. You have to unlock each nesting doll to get to the one inside it.

You'll also notice account-level objects that live outside the database hierarchy -- users, roles, warehouses, resource monitors. These are the things you manage at the account level, not tied to any one database.

As an admin, this hierarchy is your mental map for everything -- setting up access, troubleshooting permissions, organizing teams. When someone says "I can't see that table," you already know where to look: do they have privileges on the schema? The database? That's the nesting-doll chain at work. We're about to build this out hands-on, so keep this picture in your head.

**[DEMO: Part 1, Step 1 - Create foundation (database, schemas, tables)]**

Now that we have the structure down, let's talk about how we actually control who gets to open each of those nesting dolls. That's role-based access control.

---

## Slide 14: RBAC Section Header

This is the section where things get real for admins. Role-based access control -- RBAC -- is the foundation of security in Snowflake. It's how you decide who can see what, who can change what, and who can create what.

We're going to cover the concepts here on the slides, and then jump into a live notebook where we'll build out a full RBAC model from scratch -- database roles, schema-level roles, functional roles, users, the whole thing.

---

## Slide 15: Snowflake Layered Security

Before we zoom into RBAC specifically, let's see where it fits in the bigger security picture. Snowflake has four layers of security, and they stack on top of each other.

Layer one is Network Access -- network policies that restrict connections to specific IP addresses or route traffic through your cloud provider's private network. Think of this as the bouncer at the door.

Layer two is Authentication -- verifying identity. Username and password, SSO, key pair, OAuth, plus MFA. This is checking your ID at the door.

Layer three is Authorization -- and this is RBAC. Once you're in the building, what rooms are you allowed to enter? What can you do in those rooms? That's what roles and privileges control, and it's where we're going to spend most of our time today.

Layer four is Continuous Data Protection -- encryption at rest and in transit, Time Travel, Fail-safe, undrop. This is the safety net that protects data even after it's been accessed or accidentally deleted.

All four layers work together, but layer three -- Authorization -- is where admins have the most day-to-day control.

---

## Slide 16: Access Control Methods (RBAC, DAC, MAS)

Now let's get specific about how access control works in Snowflake. There are three methods, and understanding the difference matters.

First is RBAC -- Role-Based Access Control. Privileges get assigned to roles, and roles get assigned to users. Users never get direct access to objects. This is the primary model and the one you should be using.

Second is DAC -- Discretionary Access Control. This is what most legacy databases use. The person who creates an object owns it and can grant access to whoever they want. It works, but it's hard to govern at scale because ownership is scattered.

Third -- and this is the best practice -- is Managed Access Schemas. This takes the grant power away from individual object owners and gives it to the schema owner. So even if someone creates a table in a production schema, they can't hand out access on their own. Only the schema owner or security admin can do that. This is how you enforce separation of duties.

We'll see this in action in the notebook. The schemas we build will follow the managed access pattern so you can see the difference firsthand.

---

## Slide 17: System Roles

Snowflake comes with four built-in system roles, and they form a hierarchy. At the top is ACCOUNTADMIN -- the superuser. It combines the powers of the two roles below it. You should have very few people using this role, and never for daily work.

Below that is SECURITYADMIN -- this role creates and manages users, roles, and grants. If you're setting up access, this is the role you're using.

Next is SYSADMIN -- this role creates and manages objects like databases, schemas, and warehouses. If you're building infrastructure, this is your role.

And at the bottom is PUBLIC -- a pseudo-role that every single user and role automatically inherits. Anything granted to PUBLIC is visible to everyone on the account, so be very intentional about what you put there.

In the notebook, we'll be creating custom roles that sit between SYSADMIN and PUBLIC -- that's where your real-world functional and access roles live.

---

## Slide 18: Access Control Concepts

This slide ties the vocabulary together. There are four key concepts you need to know.

A Securable Object is anything you can grant access to -- a database, a schema, a table, a warehouse, a stored procedure. If it exists in Snowflake, it's a securable object.

A Privilege is a specific permission on that object -- SELECT, INSERT, USAGE, CREATE TABLE. Privileges are granular, which is what gives you fine-grained control.

A Role is the container you put privileges into. And here's the important distinction: we split roles into two types. An Access Role is a bundle of object privileges -- like "read access to everything in the Finance schema." A Functional Role is a bundle of access roles -- like "Finance Analyst gets Finance read, Marketing read, and warehouse usage." Access roles describe what you can do. Functional roles describe who you are.

A User is either a person or a service account. Users get assigned functional roles, never access roles directly. That's the convention that keeps everything clean -- if someone changes jobs, you swap their functional role instead of re-granting dozens of individual privileges.

This is exactly what we're about to build in the notebook. We'll create access roles at the database and schema level, bundle them into functional roles, and assign those to users like Warren Buffet and a Marketing Automation service account.

---

## Slide 19: Access Control Concepts - Code Example

You don't grant access roles to users by convention to keep things easy. Nothing prevents you, but it's the standard convention that everyone should be using.

**[DEMO: Part 1, Steps 2-6 - Create database roles, schema roles, functional roles]**

---

## Slide 20

**SKIP**

---

## Slide 21: Database Roles

Here's the concept that ties everything we just covered together. Database roles are the recommended way to build your access roles in Snowflake. The idea is simple: the people who own the data should be the ones defining access to it.

A database owner creates database roles and grants them privileges on objects inside that database. Then those database roles get granted up to functional account roles -- the ones your users actually use. This keeps data access decisions close to the data itself instead of centralized with one overloaded admin.

Think of it like department managers handing out building key cards for their own floors. They know who needs access to what. They bundle the right doors into a key card, and then HR assigns that card to employees. The department manager doesn't need to know every employee in the company -- they just define what access looks like for their floor.

**[DEMO: Part 1, Steps 2-4 - Create DB_READ, DB_WRITE, DB_CREATE database roles, then Finance and Marketing schema roles]**

In the notebook, we're building exactly this pattern: database roles for the COMPANY database with tiered access -- READ, WRITE, and CREATE -- at both the database and schema level.

Takeaway: Database roles keep access decisions close to the data. Define access at the database level, bundle into functional roles at the account level.

---

## Slide 22: Creating Users - Code Example

Now let's create the users who will use these roles.

**[DEMO: Part 1, Step 7 - Create Warren Buffet (person) and Marketing Automation (service account)]**

Two types of users: people and service accounts. Warren Buffet is a person -- he logs in interactively, uses MFA, takes coffee breaks. The Marketing Automation account is a service -- it runs pipelines 24/7, authenticates with a key pair, no human ever logs in as this user.

The key difference for admins: person users need MFA and password policies. Service accounts need key rotation and should be locked to specific IPs via network policies. Different security postures for different use cases.

Takeaway: Person users need MFA and passwords. Service accounts need key pairs and IP restrictions. Different postures for different use cases.

---

## Slide 23: Creating Users (continued)

**[DEMO: Part 1, Steps 8-9 - Assign roles to users and test access]**

After creating users, we assign them their functional roles. Warren gets FINANCE_ANALYST. The service account gets MARKETING_MANAGER. Then we test -- switch to FINANCE_ANALYST, query the Finance schema, confirm it works. Try to query Marketing -- access denied. That's the RBAC model doing its job.

Takeaway: Swap the functional role, not dozens of individual grants. That's the payoff of this model.

---

## Slide 24: MFA / Trust Center Preview

MFA is table stakes for security. Snowflake now supports passkeys -- Touch ID, Windows Hello, hardware security keys. More secure than SMS codes, easier to use than authenticator apps.

As an admin, you should be pushing for MFA enrollment across all person users. Trust Center helps you track who's enrolled and who isn't. We'll look at Trust Center in more detail later, but know that it's your dashboard for security compliance.

Takeaway: Push for MFA on every person user. Passkeys make it easier, Trust Center tracks who's enrolled.

---

## Slide 25: Passkeys

Passkeys are the newest MFA option. Users enroll via their Snowsight settings or get prompted after login. It leverages biometrics they already use -- fingerprint, face recognition -- which means adoption is higher because it's actually easier than typing a 6-digit code.

For admins, this means fewer "I lost my authenticator app" support tickets and stronger security at the same time. Win-win.

Takeaway: Passkeys mean stronger security with less friction -- fewer support tickets, higher adoption.

---

## Slide 26: Trust Center Overview

Trust Center is your security command center. It scans your account against industry benchmarks -- CIS Snowflake Foundations, security essentials, threat intelligence -- and tells you where you're compliant and where you have gaps.

The free Security Essentials scanner checks MFA compliance and network policy usage. CIS Benchmarks goes deeper. Threat Intelligence looks for risky user behavior.

We'll do a quick walkthrough of Trust Center later. For now, just know it exists and it's where you go to answer "how secure is my Snowflake account?"

Takeaway: Trust Center answers "how secure is my account?" in one dashboard.

---

## Slide 27: Data Governance Section Header

Now that we control WHO can access data, let's control WHAT they see when they do. This is data governance -- the policies that determine what data looks like to different users.

We have three main tools: tags to classify data, masking policies to hide or transform sensitive columns, and row access policies to filter which rows a user can see.

---

## Slide 28: Connected Catalog

Before we dive into policies, quick note on the governance catalog. Snowflake provides a unified view of all your data assets, their classifications, and protection status. You can see at a glance which tables have PII, which columns are masked, which schemas have row access policies.

This matters because governance at scale requires visibility. You can't protect what you can't see.

---

## Slide 29: Row Access Policies

Row access policies filter data at query time based on who's asking. Same table, different users, different rows.

Think of it like a building directory that only shows floors you have access to. Jordan is the Europe Sales Manager -- when she queries the customer table, she only sees European customers. Alex is North America -- he sees North American customers. Morgan is an admin -- she sees everyone.

The policy is defined once and applied to the table. No duplicate tables, no views per region, no application-level filtering that someone could bypass. The database enforces it.

**[DEMO: Part 2, Steps 1 and 6-8 - Create sensitive tables, create row access policy lookup table, create and apply row access policy]**

*Presenter note: The notebook runs masking (Steps 2-5) before row access (Steps 6-8). You'll demo masking on slide 30 first, then come back to row access. Run Part 2 Steps 1-5 before switching to Steps 6-8.*

Takeaway: One table, one policy, different rows per role. No duplicate tables, no views per region.

---

## Slide 30: Dynamic Data Masking

Column-level security. Masking policies transform data at query time based on the user's role.

Full SSN visible to ACCOUNTADMIN. Fully masked for everyone else. The actual data in the table never changes -- the masking happens on read. This is critical for audit and compliance because you're not degrading your source data.

One policy can apply to many columns. Tag a column as "SSN", attach a masking policy to that tag, and every SSN column across every table gets the same treatment automatically.

**[DEMO: Part 2, Steps 2-5 - Create PII tag, apply to columns, create masking policies, attach to tags]**

Takeaway: Masking transforms data on read, never degrades the source. Tag it once, mask it everywhere.

---

## Slide 31: Tag-Based Masking

This is the automation layer. Instead of attaching masking policies column-by-column, you attach them to tags. Then you tag your columns. It's the difference between putting a lock on every door individually versus installing a master key system. Tag the data, and the policy follows automatically.

Tag a column as PII_LEVEL = 'HIGH', and the high-sensitivity masking policy kicks in automatically. New table with an SSN column? Tag it, and masking follows. No additional policy work.

This scales governance. When you have hundreds of tables, you're not writing hundreds of policy attachments -- you're maintaining a handful of tags and their associated policies.

Takeaway: Tags scale governance. Maintain a handful of tags, not hundreds of policy attachments.

---

## Slide 32: Conditional Masking

Sometimes the masking rule depends on another column's value. Conditional masking lets you express that.

Here's a concrete example: a contacts table where some people opted in to be public. You don't want to mask everyone's phone number -- just the ones who said "private." The policy checks the "visible" column: if it says "public", show the number.

ACCOUNTADMIN always sees everything. Finance role sees rows marked "admin" or "public". Everyone else sees asterisks. This handles the edge cases where simple role-based masking isn't enough.

---

## Slide 33: Conditional Masking - Code Example

The key pattern for conditional masking: a CASE statement checks CURRENT_ROLE() and can reference other columns in the same row to decide what to return.

No change to stored data. The transformation happens at query time. Authorized users unmask automatically.

---

## Slide 34: Data Governance UI

Finally, Snowsight gives you a UI to manage all of this without writing SQL. At-a-glance summary of tagged assets, protection status, drill-down by database or schema, and the ability to apply tags and policies right from the interface.

For compliance reporting, you can generate reports showing which objects are tagged and which have policies applied. Auditors love this.

**[DEMO: Part 2, Step 9 - Switch roles and query EMPLOYEES_SENSITIVE to see masking and row access in action]**

Let me show you the payoff. We'll switch between ACCOUNTADMIN, MARKETING_MANAGER, and FINANCE_ANALYST and query the same table. Watch how the data changes based on who's asking.

---

## Slide 35: Managing Compute / Warehouse Overview

Now let's talk about the engine that runs your queries: virtual warehouses. This is where administrators have the most direct impact on both performance and cost.

This diagram shows the typical setup. Different workloads get different warehouses. Data loading gets a dedicated warehouse sized for bulk operations. Transformation gets another. Reporting and BI get a multi-cluster warehouse that can scale out when lots of users hit it at once.

The key principle: isolate workloads so they don't compete for resources. Your data scientists shouldn't slow down your dashboards. Your ETL jobs shouldn't block your analysts.

**[DEMO: Part 3, Step 1 - Create ANALYTICS_WH warehouse]**

---

## Slide 36: Multi-Cluster Scale-Out

Here's a common situation: you have 5 users during normal hours, and you've sized your warehouse as a Medium. Works great. But at month-end, you have 50 users running reports. Performance crashes because the warehouse saturates.

The data volume didn't change. The queries didn't get more complex. You just have more of them hitting at the same time.

Multi-cluster warehouses solve this by scaling out -- spinning up additional clusters and spreading queries across them. Think of it like a grocery store opening more checkout lanes when the lines get long. The store didn't get bigger -- they just opened more lanes to handle the rush. Instead of one 4-node cluster struggling, you have four 4-node clusters sharing the load.

**[DEMO: Part 3, Step 4 - Create REPORTING_MULTICLUSTER warehouse with MIN/MAX clusters]**

*Presenter note: The notebook runs QAS (Step 3) before multi-cluster (Step 4). Run Part 3 Step 3 before this demo if following notebook order.*

Takeaway: Scale out for concurrency, scale up for complexity. Multi-cluster handles the crowd.

---

## Slide 37: Query Acceleration Service

Query Acceleration is Snowflake's way of turbocharging specific queries without resizing your warehouse. It helps with scan-heavy, parallelizable queries -- think large aggregations or filters over huge tables.

You enable it on the warehouse, and Snowflake automatically applies burst compute when it detects eligible queries. You pay for what you use, but it can dramatically speed up outlier queries that would otherwise need a bigger warehouse.

**[DEMO: Part 3, Step 3 - Enable QAS on ANALYTICS_WH, show eligible queries view]**

Takeaway: QAS turbocharges outlier queries without resizing your warehouse. Enable it, and Snowflake handles the rest.

---

## Slide 38: Built-in Cost Controls

Three levers to control warehouse costs:

Auto-suspend: warehouse turns off after N seconds of idle time. Default is 10 minutes, but for interactive workloads you might go as low as 1 minute. No running warehouse means no credits burning.

Auto-resume: warehouse turns back on automatically when a query arrives. Users don't notice any downtime -- just a few seconds of startup.

Resource monitors: hard limits and alerts. We'll dig into these next.

Takeaway: Auto-suspend stops the meter. Auto-resume keeps it invisible. Resource monitors set the hard limit.

---

## Slide 39: Resource Monitors

Resource monitors are your safety net for controlling credit consumption. Let's look at how they work.

---

## Slide 40: Resource Monitors (Detail)

Resource monitors are your safety net. They track credit consumption and take action when thresholds are hit.

You can set them at the account level or per-warehouse. Tiered triggers let you escalate -- notify at 50%, notify again at 75%, suspend at 95%, suspend immediately at 100%.

Limitations to know: storage and serverless costs aren't covered by resource monitors. And each warehouse can only have one monitor, though one monitor can cover multiple warehouses.

**[DEMO: Part 3, Step 6 - Create CREDIT_WATCHDOG resource monitor with tiered triggers]**

Takeaway: Resource monitors are your automated spending guardrails -- notify, suspend, hard stop.

---

## Slide 41: Resource Monitor Code Example

Let's walk through the SQL. CREDIT_QUOTA is your monthly budget -- like setting a spending limit on a corporate card. TRIGGERS are the escalation ladder: notify at 50%, notify again at 75%, suspend at 95%, hard stop at 100%. NOTIFY sends an email to ACCOUNTADMIN. SUSPEND stops queued queries gracefully. SUSPEND_IMMEDIATE kills running queries -- use sparingly.

Assign the monitor to a warehouse with ALTER WAREHOUSE SET RESOURCE_MONITOR.

---

## Slide 42: Caching Section Header

Let's talk about one of Snowflake's best features for performance: automatic caching. Three layers work together to speed up repeated queries without any configuration on your part.

---

## Slide 43: Three Layers of Caching

Layer 1: Metadata cache. Snowflake stores metadata about your tables -- row counts, min/max values, other table statistics. Queries like COUNT(*) or simple aggregations often hit this cache and return instantly without scanning any data.

Layer 2: Results cache. If you run the exact same query again and the underlying data hasn't changed, Snowflake returns the cached result. No warehouse needed, no credits consumed.

Layer 3: Warehouse cache. Each warehouse caches data it reads from storage. If a different query needs the same partitions, they're already in memory.

**[DEMO: Part 4 - Run COUNT(*) (metadata cache), then SELECT * twice (results cache), then filtered query (warehouse cache)]**

Watch the query profile as we run these. You'll see "QUERY RESULT REUSE" when the results cache kicks in.

Takeaway: Caching is automatic. Understand it to explain performance wins to your stakeholders.

---

## Slide 44: Cost Governance Section Header

Now let's talk about keeping your Snowflake spend under control. Cost governance.

---

## Slide 45: Cost Governance Pillars

Visibility, Control, Optimization. Those are the three pillars of cost governance in Snowflake.

Visibility means understanding where your credits are going -- which warehouses, which users, which workloads.

Control means setting limits so you don't get surprised -- resource monitors, budgets, alerts.

Optimization means finding inefficiencies and fixing them -- right-sizing warehouses, eliminating waste, improving query performance.

We'll touch on all three.

Takeaway: As an admin, Visibility tells you where the money goes, Control keeps it in bounds, and Optimization stretches every credit further.

---

## Slide 46: Cost Breakdown

Overall cost breaks down into three buckets:

Compute: warehouses, serverless features (Snowpipe, tasks, etc.), cloud services. This is usually the biggest portion.

Storage: monthly flat rate per terabyte. Compressed, so your actual storage is often much smaller than raw data size.

Data transfer: egress charges when data leaves your cloud region. Usually minimal unless you're doing cross-region replication.

For administrators, compute is where you have the most control and the most opportunity to optimize.

Takeaway: Compute is the biggest lever. Focus your optimization energy there.

---

## Slide 47: Information Schema vs Account Usage

Two ways to get metadata and usage information: Information Schema and Account Usage.

Information Schema is built into every database. Zero latency, but results are limited -- current state only, only objects your role can see, no historical data.

Account Usage views are delivered via the SNOWFLAKE database share. They have 45-180 minute latency, but give you a full year of history and cover everything in the account.

For cost governance, Account Usage is what you want. Key views:
- METERING_DAILY_HISTORY: credit consumption by service type
- WAREHOUSE_METERING_HISTORY: credit consumption by warehouse
- QUERY_HISTORY: query-level detail including credits used

By default, only ACCOUNTADMIN can access Account Usage. You can grant access to other roles if needed.

**[DEMO: Part 5, Steps 1-2 - Run credit consumption by service type, then weekly trends]**

Takeaway: Information Schema for real-time spot checks. Account Usage for historical analysis and cost governance.

---

## Slide 48: Cost Management Dashboard

Snowsight has a built-in Cost Management dashboard. You can see spend over time, drill into specific warehouses, compare month-over-month, and identify trends.

For admins who prefer SQL, the Account Usage views give you the same data in a queryable format. For executives who want a chart, the dashboard delivers.

---

## Slide 49: Trust Center Section Header

We mentioned Trust Center earlier during the MFA discussion. Now let's take a closer look at what it can do for you.

---

## Slide 50: Trust Center Detail

Trust Center scans your Snowflake account to detect security violations based on the CIS Snowflake Foundations Benchmark. Think of it like a home security inspection -- someone walks through your house and says "this window doesn't lock, that door needs a deadbolt, your smoke detector battery is dead." That's what Trust Center does for your Snowflake account.

You see findings, severity levels, and remediation recommendations all in one place. For admins responsible for security, this is your single pane of glass.

**[DEMO: Open Trust Center in Snowsight, show scanner results and recommendations]**

Takeaway: Trust Center is your single pane of glass for security compliance. Run the free scanner today.

---

## Slide 51: Cortex Code Section Header

Last topic: Cortex Code. This is Snowflake's AI assistant built right into Snowsight. It knows your data catalog, respects your RBAC, and helps you work faster.

---

## Slide 52: Cortex Code

Cortex Code is an AI-powered conversational interface for querying enterprise data using natural language. It sits directly on top of your existing Snowflake data, which means all your governance and access controls are automatically honored.

For administrators, the key points:
- It uses Snowflake RBAC, so it only sees and acts on data the user has access to
- It understands Snowflake -- databases, schemas, tables, semantic models
- It's context-aware -- knows where you are in the UI, remembers your conversation
- It automates complex tasks -- not just answering questions, but building applications, managing cost, governing data

**[DEMO: Open Cortex Code in Snowsight, ask "show me credit usage by warehouse for the last 30 days", watch it generate and run SQL]**

Think of it as a junior analyst who never sleeps, never forgets the schema, and always respects your security policies.

Takeaway: Cortex Code respects your RBAC, knows your schema, and saves you hours of manual SQL.

---

## Slide 53: Next Steps

**SKIP**

---

## Slide 54: Professional Services

**SKIP**

---

## Slide 55: Summary

We covered a lot today: RBAC keeps the wrong people out. Governance controls what they see. Compute management keeps things fast and affordable. Cost governance keeps you in budget. And Cortex Code makes it all easier.

---

## Slide 56: Thank You

Thank you for joining Zero to Snowflake for Administrators. Reach out if you want to go deeper on any topic. Good luck building secure, well-governed Snowflake environments.

---

## Slide 57: Speaker Info / Closing

**[Your contact info, social links, etc.]**

---

# Quick Reference: Notebook Demo Points

| Slide | Demo Callout | Notebook Location |
|-------|--------------|-------------------|
| 10 | Open Snowsight | Browser |
| 13 | Create foundation | Part 1, Step 1 |
| 19 | RBAC setup | Part 1, Steps 2-6 |
| 21 | Database roles | Part 1, Steps 2-4 |
| 22 | Create users | Part 1, Step 7 |
| 23 | Assign roles, test | Part 1, Steps 8-9 |
| 29 | Row access policies | Part 2, Steps 1 and 6-8 |
| 30 | Tags and masking | Part 2, Steps 2-5 |
| 34 | See policies in action | Part 2, Step 9 |
| 35 | Create warehouse | Part 3, Step 1 |
| 36 | Multi-cluster | Part 3, Step 4 |
| 37 | QAS | Part 3, Step 3 |
| 40 | Resource monitor | Part 3, Step 6 |
| 43 | Caching demo | Part 4 |
| 47 | Cost queries | Part 5, Steps 1-2 |
| 50 | Trust Center | Snowsight UI |
| 52 | Cortex Code | Snowsight UI |

---

# Notebook Part/Step Summary

**Part 1: RBAC**
- Step 1: Create foundation (database, schemas, tables)
- Step 2: Create database roles (DB_READ, DB_WRITE, DB_CREATE)
- Step 3: Create Finance schema roles (FINANCE_READ, FINANCE_WRITE, FINANCE_CREATE)
- Step 4: Create Marketing schema roles (MARKETING_READ, MARKETING_WRITE, MARKETING_CREATE)
- Step 5: Create Governance schema and roles
- Step 6: Create functional (account) roles
- Step 7: Create users (Warren Buffet + Marketing Automation)
- Step 8: Assign roles to users
- Step 9: Test access (query Finance, then access denial tests)
- Step 10: Monitor grants and privileges

**Part 2: Data Governance**
- Step 1: Create sensitive data tables
- Step 2: Create tags (PII_LEVEL, DEPARTMENT)
- Step 3: Apply tags to columns
- Step 4: Create masking policies
- Step 5: Attach masking policies to tags
- Step 6: Create row access policy lookup tables
- Step 7: Create row access policies
- Step 8: Apply row access policies to tables
- Step 9: See policies in action (role switching)

**Part 3: Compute Management**
- Step 1: Create warehouse (ANALYTICS_WH)
- Step 2: Create warehouse access roles
- Step 3: Resize and enable QAS
- Step 4: Multi-cluster warehouse (REPORTING_MULTICLUSTER_WH)
- Step 5: Gen2 warehouse (NEXT_GEN_WH)
- Step 6: Resource monitor (CREDIT_WATCHDOG)

**Part 4: Caching**
- Metadata cache demo (COUNT(*))
- Results cache demo (SELECT * repeated)
- Warehouse cache demo (filtered queries)

**Part 5: Cost Management**
- Step 1: Credit consumption by service type
- Step 2: Weekly usage trends

**Part 6: DMFs** (Bonus content, not covered in slides)
- Data metric functions setup and testing
