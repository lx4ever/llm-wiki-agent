# AI Agent Prompt for MENTAL LOAD MISSION CONTROL

- Notion page id: `3b17a51c-1360-83e5-9503-814c51e92180`

You are a Notion workspace architect and productivity systems designer.

Your task is to build a complete operational workspace in Notion called:

MENTAL LOAD MISSION CONTROL

The workspace must act like a control center for managing complex professional workloads including quotations, operational work, vendor RFQs, and personal admin.

The system must implement the following structure:

Domains → Containers → Boxes → Tasks

It must also support:

Mental Load Radar

Quote Pipeline

Vendor RFQ tracking

Active Mission Stack

Capture Inbox

The workspace must be visually clean and optimized for low cognitive load.

GLOBAL DESIGN PRINCIPLES

The dashboard must feel like a mission control center.

Design requirements:

- minimal clutter

- clear visual grouping

- strong hierarchy

- large section headers

- consistent icons

- simple color coding

Use icons for key sections:

📥 Capture

🚀 Mission Stack

📊 Mental Load Radar

📦 Containers

📈 Quote Pipeline

🧾 Vendor RFQ

⚙ Tasks

DATABASE 1 — CAPTURE INBOX

Purpose:

Fast capture of thoughts and tasks (Box −1).

Properties:

Task (title)

Source (select)

Work

Personal

Idea

Follow-up

Date Captured (date)

Processed (checkbox)

Notes (text)

Rules:

Items must be processed daily into the Tasks database.

DATABASE 2 — TASKS

Purpose:

Central operational task system.

Properties:

Task (title)

Domain (select)

Work Operations

Quotation Engine

Business Development

Personal Admin

Life Design

Container (relation → Containers)

Box (select)

Box −1 Capture

Box 0 Friction Breaker

Box 1 External Commitment

Box 2 System Execution

Box 3 Deep Work

Box 4 Decision

Box 5 Life Admin

Status (select)

Capture

Queued

Active

Waiting

Completed

Priority (select)

Low

Medium

High

Critical

Due Date (date)

Next Action (text)

DATABASE 3 — CONTAINERS

Purpose:

Projects and workstreams grouping tasks.

Properties:

Container Name (title)

Domain (select)

Work Operations

Quotation Engine

Business Development

Personal Admin

Life Design

Health (select)

🟢 Healthy

🟡 Active

🟠 At Risk

🔴 Urgent

⚪ Waiting

Stage (text)

Blockers (text)

Next Step (text)

Relation:

Tasks

DATABASE 4 — QUOTES

Purpose:

Manage quote pipeline.

Properties:

Quote Name (title)

Customer (text)

Stage (select)

RFQ Received

Costing

Vendor RFQ

Internal Review

Submitted

Won

Lost

Health (select)

🟢 Healthy

🟡 Active

🟠 At Risk

🔴 Urgent

⚪ Waiting

Deadline (date)

Value (number)

Container (relation → Containers)

DATABASE 5 — VENDOR RFQ

Purpose:

Track vendor pricing requests.

Properties:

Quote (relation → Quotes)

Vendor (text)

Part (text)

Status (select)

Waiting

Quoted

Ordered

Follow-up Date (date)

DASHBOARD PAGE

Create a page called:

MENTAL LOAD MISSION CONTROL

Use large visual headers and divide the page into the following sections.

SECTION 1 — 📥 CAPTURE INBOX

Linked view of Capture database.

Filter:

Processed = unchecked

Purpose:

Fast brain dump.

SECTION 2 — 🚀 ACTIVE MISSION STACK

Linked Tasks view.

Filter:

Status = Active

Columns:

Task

Container

Box

Priority

Only 3–4 tasks should appear here.

SECTION 3 — 📊 MENTAL LOAD RADAR

Create grouped view of Tasks by Domain.

Display number of tasks per domain.

Purpose:

Visualize workload pressure.

SECTION 4 — 📈 QUOTE PIPELINE

Board view from Quotes database.

Group by:

Stage

Columns should show:

RFQ Received

Costing

Vendor RFQ

Internal Review

Submitted

Won/Lost

SECTION 5 — 📦 CONTAINER HEALTH BOARD

Table view from Containers database.

Show columns:

Container

Domain

Health

Stage

Blockers

Next Step

SECTION 6 — ⚙ TASK QUEUE

Table view from Tasks.

Filter:

Status = Queued

SECTION 7 — 🧾 VENDOR RFQ TRACKER

Table view from Vendor RFQ database.

Columns:

Quote

Vendor

Part

Status

Follow-up Date

ADDITIONAL TASK VIEWS

Create additional task views:

Deep Work Focus

Filter:

Box = Box 3

External Commitments

Filter:

Box = Box 1

System Execution

Filter:

Box = Box 2

EXAMPLE DATA

Create sample entries.

Containers:

LWCM Batch 3 Quote

De Havilland Quote

Yaw Joint Quote

ERP & Order Management

Arcfield Engagement

Quotes:

LWCM Batch 3 — Costing

De Havilland — Internal Review

Yaw Joint — Vendor RFQ

Tasks:

Build LWCM costing

Send R-bond quote

Enter MDA order in IFS

FINAL GOAL

The final workspace must allow the user to see instantly:

- capture inbox

- active mission tasks

- quote pipeline

- project health

- task queues

The interface must feel like a mission control center that reduces mental load and supports high-complexity work.
