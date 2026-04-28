# For work

- Notion page id: `7527a51c-1360-830a-a7f6-81aa06fe5ede`
- URL: https://www.notion.so/For-work-7527a51c1360830aa7f681aa06fe5ede

## Main Page Content

Goal: reduce my admin work time by 80%

- agent to follow up on PO OC and shipping date/tracking

- Agent that checks all my own orders against all communications with customer, summarize latest info

- Agent that checks labour hours in IFS

- Agent that creates customer contact list and records of interactions, reminder for me to follow up

- Create AI skill that takes input from pdf shipping documents, identify free issue parts, and reconcile/verify with the parts list, and then output whether all free issue parts are received based on the PL and shipping docs.

- Agent to review IFS purchase material status - cloud flow then trigger power automate?

- Cloud flow that triggers desktop flow - weekly production schedule update, pull info from IFS, update MS Project

## Boxes Mental Load Model

## AI Agent Prompt for MENTAL LOAD MISSION CONTROL

## Master Prompt — Auto-Update My Notion Mission Control

### RFQ Workflow ###########################

```mermaid
graph TB
    A["RFQ Email Received via Kiteworks/Email"] --> B["Create New RFQ Number in Salesforce, IFS & Excel Tracker"]
    B --> B2["Create Kanata Quote Review Form (AI auto-generates form)\nManual: Upload into CRM"]
    B2 --> C{"Is it for CGP Customer?"}
    C -->|Yes| D["Create New RFQ Folder in Confidential Drive"]
    C -->|No| E["AI: AI Quotation Agent\nAI builds Notion page from RFQ PDF & Excel"]
    D --> E
    E --> G["AI: RFQ Parts List Normalization"]
    G --> H["Manual: Update Cost Sheet Excel"]
    
    
    %% Manual sub-process for cost sheet updates
    H --> H0["AI: Update A/R parts qty based on drawing"]
    H0 --> H1["Parts sourcing"]
    H1 --> H1a["AI: Identify vendors done at normalization step (hardware parts, Glenair, wire & cable, free issue, plastics, labels, heat shrink tubing)"]
    H1 --> H1b["Manual: Source parts (all other items)"]
    H1a --> H2["Manual: Get part pricing, lead time (LT) & MOQ"]
    H1b --> H2
    H2 --> H4["Manual: Update pricing on Cost Sheet Excel"]
    H4 --> I["AI: Cost Sheet Consolidation"]
    I --> I1["AI: Compute vendor freight - consolidation step"]
    I1 --> J["Manual: Update Cost Sheets with Labour Cost & Engineering Notes"]
    J --> K["Manual: Save Final Cost Sheets to Local Folders & CRM"]
    K --> L["Manual: Compile Quotation Document using ERP"]
    L --> M["Send Quotation to Customer"]

    %% Color schema
    %% - AI: green
    %% - Manual: pink
    %% - Decisions: gray
    classDef ai fill:#90EE90,stroke:#2f7a2f,stroke-width:1px,color:#111;
    classDef manual fill:#FFB6C1,stroke:#a33b52,stroke-width:1px,color:#111;
    classDef decision fill:#D9D9D9,stroke:#666,stroke-width:1px,color:#111;

    class E,G,H0,H1a,I,I1 ai;
    class A,B,D,H,H1,H1b,H2,H4,J,K,L,M manual;
    class C decision;s A,B,D,H,H1,H1b,H2,H4,J,K,L,M manual;
    class C decision;
```

![RFQ flowchart visual](https://prod-files-secure.s3.us-west-2.amazonaws.com/bb47a51c-1360-815a-b85e-0003c8c25119/248927dc-27a4-49e0-865b-0fae0d31e9e9/ai-edited-image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJMJQW73%2F20260428%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260428T023331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDvzvkmmhcwx2h%2Flab0vNqgd1GJoBwiLYXM%2FpikP%2F0%2F7gIgaA4pNLK5vhwfgx2CmXtWAtzvw2Oo0J%2FifH4Y5SaEnMkqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP1TXDuYZ81Aibi3OSrcAxuyxN7WQ8vFOK6fSV5CM1d1pgso3oSFV7OdlSTc%2F1laZmzhxVFEKBVx8wLlVo%2B1nDEt3wXb1bXm20l1D%2FfcWHrz3ny7UnSmmf5BwggcIBoX%2FbV3h%2Fxf8eGaBRLjxYzLcUxBSLhIswn8nHKlZiRL54cEgDsGcl4%2Fyxu6Ndt10PJz%2FFh874n0YGWgtfYysQtn5nN26UzhIq0l%2BE8WQB9QCimu2uxAAGC%2BSi1LCQPEoJ%2FQy0QgAfb4yGH2VE9A2pOC503%2Bw6fzMMWdVQ54w6INgCBtp0gAj8ySK4f%2BJvN08xehzlT6KYYIn%2BHPeB%2F20W%2Fr%2FS%2FyRHv5r3g7MmItHhIMebPFaS9V5i3eb5R1UpMOlqQejhjSFQBdlq4SZmbcPv5lfKkI4tCVK%2F%2B9CdmnVyZnCzDos%2FgEFAA%2BMiJADsp2cGN4skIC7NOpwNWppUl2S7xqP65hVbkUlEhPlvq88ezwjYQNwGec2o5LmqxwwIPga4loaJJsJvpm7p42PTTs4weyGEO672z9ppdZ3v%2FE9T4ePcFAvydGsKO04Y3DTdmJKDrrmea2NGxH94nv9aUaEOBfWZQdJa4mAsH5l9Nc9gf44hSVZA50udEk1Bhy0eS169yAQoAClo2ALB1q8lrtMOajwM8GOqUB2LqLbg1F%2F3Yk7dPJjZXK2n0ymkKJ%2FG98ljZ8tQpBNbXMDB6L6y4jSyOdPs629dWjKeu%2F1bqXzWVMGYHaFsd%2FZjhsE4XYgrSrJqR9%2BC6E%2F%2Fo%2B7Gq%2FECI76M4qGRLBHu0G261a0MHJTmeSKNLfFuIhYPTuYwcPxjzzcuarCDMOsYDIgtOGImPTpmwlwCSKx3NqSeavO4FiSzCGO7AHn50C1sWpJQms&X-Amz-Signature=256640a2020c5a9abb60ab5e5c80641e5f04c0ae7bbeede18dd507b13a2ac621&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

#### Visual workflow (swimlanes)

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/bb47a51c-1360-815a-b85e-0003c8c25119/a653f26f-9203-4877-a3a3-864f948641d1/ai-edited-image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJMJQW73%2F20260428%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260428T023331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDvzvkmmhcwx2h%2Flab0vNqgd1GJoBwiLYXM%2FpikP%2F0%2F7gIgaA4pNLK5vhwfgx2CmXtWAtzvw2Oo0J%2FifH4Y5SaEnMkqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP1TXDuYZ81Aibi3OSrcAxuyxN7WQ8vFOK6fSV5CM1d1pgso3oSFV7OdlSTc%2F1laZmzhxVFEKBVx8wLlVo%2B1nDEt3wXb1bXm20l1D%2FfcWHrz3ny7UnSmmf5BwggcIBoX%2FbV3h%2Fxf8eGaBRLjxYzLcUxBSLhIswn8nHKlZiRL54cEgDsGcl4%2Fyxu6Ndt10PJz%2FFh874n0YGWgtfYysQtn5nN26UzhIq0l%2BE8WQB9QCimu2uxAAGC%2BSi1LCQPEoJ%2FQy0QgAfb4yGH2VE9A2pOC503%2Bw6fzMMWdVQ54w6INgCBtp0gAj8ySK4f%2BJvN08xehzlT6KYYIn%2BHPeB%2F20W%2Fr%2FS%2FyRHv5r3g7MmItHhIMebPFaS9V5i3eb5R1UpMOlqQejhjSFQBdlq4SZmbcPv5lfKkI4tCVK%2F%2B9CdmnVyZnCzDos%2FgEFAA%2BMiJADsp2cGN4skIC7NOpwNWppUl2S7xqP65hVbkUlEhPlvq88ezwjYQNwGec2o5LmqxwwIPga4loaJJsJvpm7p42PTTs4weyGEO672z9ppdZ3v%2FE9T4ePcFAvydGsKO04Y3DTdmJKDrrmea2NGxH94nv9aUaEOBfWZQdJa4mAsH5l9Nc9gf44hSVZA50udEk1Bhy0eS169yAQoAClo2ALB1q8lrtMOajwM8GOqUB2LqLbg1F%2F3Yk7dPJjZXK2n0ymkKJ%2FG98ljZ8tQpBNbXMDB6L6y4jSyOdPs629dWjKeu%2F1bqXzWVMGYHaFsd%2FZjhsE4XYgrSrJqR9%2BC6E%2F%2Fo%2B7Gq%2FECI76M4qGRLBHu0G261a0MHJTmeSKNLfFuIhYPTuYwcPxjzzcuarCDMOsYDIgtOGImPTpmwlwCSKx3NqSeavO4FiSzCGO7AHn50C1sWpJQms&X-Amz-Signature=ae83bf5097c7570823dedc44f3d6c00121c10331881c93770db27d2987d27e9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

### Customer Order Workflow ###########################

```mermaid
graph TB
    A["Customer PO Received via Email/Kiteworks/SourDay"] --> B["Download Technical Files"]
    B --> C{"Is this a New Job File?"}

    C -->|Yes| D["Create New Job File Folder & Save Technical Files and PO"]
    C -->|No| E["Create New CRM Folder for CO & Add Link to Excel Tracker"]
    D --> E

    E --> F{"Is PO Different from RFQ?"}
    F -->|Yes| G["AI: Auto-create Kanata Order Review Form"]
    F -->|No| H["Manual: Update Kanata Order Review Form"]

    G --> I["Manual: Upload Review Form to CRM"]
    H --> I

    I --> J["Manual or AI: Product Structure Excel Created"]
    J --> K["Manual: IFS Part Number Creation"]
    K --> L["Manual: IFS Order Entry"]

    L --> M["Manual: Lead Time Review"]
    M --> N["Manual: Initial LT Entry"]
    N --> O["Manual: Release and Approve Project in IFS"]

    O --> P["Manual: Run MRP"]
    P --> Q["Manual: Create Shop Orders"]
    Q --> R["Manual: Purchase Requisition Review"]

    R --> S["Manual: Purchase Orders Placed"]
    S --> T["Manual: 4 Week Plan Updated with New Order"]
    T --> U["Daily Order Status Monitoring"]

    %% Color schema
    %% - AI: green
    %% - Manual: pink
    %% - Decisions: gray
    classDef ai fill:#90EE90,stroke:#2f7a2f,stroke-width:1px,color:#111;
    classDef manual fill:#FFB6C1,stroke:#a33b52,stroke-width:1px,color:#111;
    classDef decision fill:#D9D9D9,stroke:#666,stroke-width:1px,color:#111;

    class G ai;
    class A,B,D,E,H,I,J,K,L,M,N,O,P,Q,R,S,T,U manual;
    class C,F decision;
```

#### Visual workflow (single lane)

![Customer Order Workflow — Single lane](https://prod-files-secure.s3.us-west-2.amazonaws.com/bb47a51c-1360-815a-b85e-0003c8c25119/f060acb9-422f-47dd-97ed-d4ae0f1683f4/ai-edited-image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJMJQW73%2F20260428%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260428T023331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDvzvkmmhcwx2h%2Flab0vNqgd1GJoBwiLYXM%2FpikP%2F0%2F7gIgaA4pNLK5vhwfgx2CmXtWAtzvw2Oo0J%2FifH4Y5SaEnMkqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP1TXDuYZ81Aibi3OSrcAxuyxN7WQ8vFOK6fSV5CM1d1pgso3oSFV7OdlSTc%2F1laZmzhxVFEKBVx8wLlVo%2B1nDEt3wXb1bXm20l1D%2FfcWHrz3ny7UnSmmf5BwggcIBoX%2FbV3h%2Fxf8eGaBRLjxYzLcUxBSLhIswn8nHKlZiRL54cEgDsGcl4%2Fyxu6Ndt10PJz%2FFh874n0YGWgtfYysQtn5nN26UzhIq0l%2BE8WQB9QCimu2uxAAGC%2BSi1LCQPEoJ%2FQy0QgAfb4yGH2VE9A2pOC503%2Bw6fzMMWdVQ54w6INgCBtp0gAj8ySK4f%2BJvN08xehzlT6KYYIn%2BHPeB%2F20W%2Fr%2FS%2FyRHv5r3g7MmItHhIMebPFaS9V5i3eb5R1UpMOlqQejhjSFQBdlq4SZmbcPv5lfKkI4tCVK%2F%2B9CdmnVyZnCzDos%2FgEFAA%2BMiJADsp2cGN4skIC7NOpwNWppUl2S7xqP65hVbkUlEhPlvq88ezwjYQNwGec2o5LmqxwwIPga4loaJJsJvpm7p42PTTs4weyGEO672z9ppdZ3v%2FE9T4ePcFAvydGsKO04Y3DTdmJKDrrmea2NGxH94nv9aUaEOBfWZQdJa4mAsH5l9Nc9gf44hSVZA50udEk1Bhy0eS169yAQoAClo2ALB1q8lrtMOajwM8GOqUB2LqLbg1F%2F3Yk7dPJjZXK2n0ymkKJ%2FG98ljZ8tQpBNbXMDB6L6y4jSyOdPs629dWjKeu%2F1bqXzWVMGYHaFsd%2FZjhsE4XYgrSrJqR9%2BC6E%2F%2Fo%2B7Gq%2FECI76M4qGRLBHu0G261a0MHJTmeSKNLfFuIhYPTuYwcPxjzzcuarCDMOsYDIgtOGImPTpmwlwCSKx3NqSeavO4FiSzCGO7AHn50C1sWpJQms&X-Amz-Signature=de95b8d860bfd415437f9e73e1938938508d9e93b882d76bf01196392b123811&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

#### Visual workflow (swimlanes)

![Customer Order Workflow — Swimlanes](https://prod-files-secure.s3.us-west-2.amazonaws.com/bb47a51c-1360-815a-b85e-0003c8c25119/90c72830-566c-498e-a626-6915e81ed899/generated-image-1774112402338.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJMJQW73%2F20260428%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260428T023331Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDvzvkmmhcwx2h%2Flab0vNqgd1GJoBwiLYXM%2FpikP%2F0%2F7gIgaA4pNLK5vhwfgx2CmXtWAtzvw2Oo0J%2FifH4Y5SaEnMkqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP1TXDuYZ81Aibi3OSrcAxuyxN7WQ8vFOK6fSV5CM1d1pgso3oSFV7OdlSTc%2F1laZmzhxVFEKBVx8wLlVo%2B1nDEt3wXb1bXm20l1D%2FfcWHrz3ny7UnSmmf5BwggcIBoX%2FbV3h%2Fxf8eGaBRLjxYzLcUxBSLhIswn8nHKlZiRL54cEgDsGcl4%2Fyxu6Ndt10PJz%2FFh874n0YGWgtfYysQtn5nN26UzhIq0l%2BE8WQB9QCimu2uxAAGC%2BSi1LCQPEoJ%2FQy0QgAfb4yGH2VE9A2pOC503%2Bw6fzMMWdVQ54w6INgCBtp0gAj8ySK4f%2BJvN08xehzlT6KYYIn%2BHPeB%2F20W%2Fr%2FS%2FyRHv5r3g7MmItHhIMebPFaS9V5i3eb5R1UpMOlqQejhjSFQBdlq4SZmbcPv5lfKkI4tCVK%2F%2B9CdmnVyZnCzDos%2FgEFAA%2BMiJADsp2cGN4skIC7NOpwNWppUl2S7xqP65hVbkUlEhPlvq88ezwjYQNwGec2o5LmqxwwIPga4loaJJsJvpm7p42PTTs4weyGEO672z9ppdZ3v%2FE9T4ePcFAvydGsKO04Y3DTdmJKDrrmea2NGxH94nv9aUaEOBfWZQdJa4mAsH5l9Nc9gf44hSVZA50udEk1Bhy0eS169yAQoAClo2ALB1q8lrtMOajwM8GOqUB2LqLbg1F%2F3Yk7dPJjZXK2n0ymkKJ%2FG98ljZ8tQpBNbXMDB6L6y4jSyOdPs629dWjKeu%2F1bqXzWVMGYHaFsd%2FZjhsE4XYgrSrJqR9%2BC6E%2F%2Fo%2B7Gq%2FECI76M4qGRLBHu0G261a0MHJTmeSKNLfFuIhYPTuYwcPxjzzcuarCDMOsYDIgtOGImPTpmwlwCSKx3NqSeavO4FiSzCGO7AHn50C1sWpJQms&X-Amz-Signature=1f64ccccee5101a6eb7684e35ee397ed3ec9bdfd74986311a8bbacd9f53fe67f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

## Embedded Child Pages (Extracted)

### Boxes Mental Load Model

Mimi, I’ll do four things clearly:

1️⃣ Show your latest boxes (clean + current)

2️⃣ Explain the logic behind this mental load system

3️⃣ Show the box structure framework

4️⃣ Show how to actually run your day with the boxes

This is essentially a personal operating system for high-load knowledge work.

# 1️⃣ Your Latest Boxes (Current State)

## 🟦 BOX 1 — External / Send / Trigger

Rule: Send → release. Never wait for reply.

Active:

1. Respond to Aviva with tracking

1. Follow up with Scott on meetings + ask for introductions

1. Send clarification questions to MDA (4-week planner orders)

1. Update quote F800014 for Aamir

1. Submit egg freezing cost to Sun Life

1. Reconcile & resubmit insurance claims via Chambers

1. Get family doctor sign-off for STD form

1. Send one-pager to SkyAlyne

1. Summarize meeting with SkyAlyne

## 🟨 BOX 2 — System / Execution

Rule: Mechanical tasks. No thinking required.

Active:

1. Enter new MDA order in IFS

1. Accept PO lines on SourceDay

1. Create PNs in IFS — F100291 remaining portion

1. Add guideline tape vendors (pricing / LT / MOQ) to Excel

1. Check RTD order parts from Rathan’s photo

1. Print STD forms

1. Update 4-week plan

1. 2025 taxes — husband (document collection)

## 🟩 BOX 3 — Deep / Cognitive Work

Rule: Open ONE at a time. Protect focus.

Active:

1. Build LWCM Batch 3 quote

1. RFQ pricing — F800256

1. Textron RFQ pricing

1. Review Seif DHC cost sheets & compile quote

1. Build guideline tape baking cost

1. Program automation (pricing tool)

1. Rev/hr analysis

1. Reach out to Irving supplier

1. Talk to Seif about quotation statistics

## 🟪 BOX 4 — Strategic Thinking

Rule: Think once, decide, park.

Active:

- RRSP calculation for husband

- Long-term business planning

- Fertility financial planning

- Travel priorities

## 🟫 BOX 5 — Life Backend

Rule: Life admin that should not occupy working memory

Active:

- Book dental appointments

- Make 6 aisle flower arrangements

- RMB exchange

- Camping planning

- Hobbies

# 2️⃣ Logic Behind This Mental Load System

This system is based on cognitive load theory + executive workflow design.

Your brain struggles when it must simultaneously:

- remember tasks

- decide priority

- execute tasks

- switch contexts

So the system separates thinking from doing.

The core insight:

When tasks are mixed together your brain constantly asks:

- Is this urgent?

- Do I need to think?

- Do I need someone else?

- Is this strategic?

Each question consumes mental energy.

Boxes remove that.

## The Three Cognitive Work Types

Most knowledge work falls into 3 types:

### Type 1 — Trigger Work

Send something so others can move.

Examples:

- emails

- updates

- approvals

These should be fast and batched.

BOX 1 handles this.

### Type 2 — Mechanical Work

No creativity needed.

Examples:

- system entries

- updates

- document processing

These should be grouped and automated.

BOX 2 handles this.

### Type 3 — Cognitive Work

Requires thinking.

Examples:

- pricing

- analysis

- strategy

These require deep focus.

BOX 3 handles this.

### Type 4 — Strategic Thinking

High-level planning.

Examples:

- career decisions

- financial planning

- business strategy

These require slow thinking.

BOX 4 handles this.

### Type 5 — Life Maintenance

Low cognitive tasks that should not mix with work.

BOX 5 handles this.

# 3️⃣ Structure of the Boxes

The structure is designed to match how the brain actually works.

## Key Rule

You never mix boxes during execution.

Mixing boxes causes:

- cognitive switching

- fatigue

- perceived overwhelm

# 4️⃣ How to Use the Boxes Daily

This is the most important part.

Your day should follow a box order.

# Daily Execution Model

## Step 1 — Clear External Triggers (BOX 1)

Time: 30–60 minutes

Send everything that moves other people.

Examples:

- updates

- requests

- follow-ups

Once sent:

Do not wait for replies.

## Step 2 — System Tasks (BOX 2)

Time: 45–90 minutes

Do mechanical tasks.

Examples:

- CRM

- IFS

- system updates

No thinking required.

## Step 3 — Deep Work (BOX 3)

Time: 1–3 hours

Pick ONE task only.

Examples:

- pricing

- quotes

- analysis

Rules:

- no emails

- no Slack

- no switching tasks

## Step 4 — Optional Strategy (BOX 4)

Only if energy remains.

Example:

- RRSP planning

- career planning

## Step 5 — Life Backend (BOX 5)

Done when relaxed or evening.

# Example Using Your Current Tasks

Your ideal workday could look like:

Morning

BOX 1

- Send SkyAlyne summary

- Send MDA clarifications

- Follow up Scott

BOX 2

- Enter MDA order in IFS

- Accept SourceDay lines

- Print STD forms

Afternoon

BOX 3

- Build LWCM Batch 3 quote

Late afternoon

BOX 3

- DHC pricing

Evening

BOX 5

- Dental appointment booking

# Why This System Works Especially Well For You

Your role combines:

- program management

- quoting

- operations coordination

- business development

That creates very high context switching.

This system prevents:

- reactive work

- endless inbox loops

- mental clutter

# The Real Goal

The goal is not productivity.

The goal is:

You stop asking:

Your boxes already answered that.

this is the single structural change that usually reduces mental load 30–40% for people in roles like yours (program manager / BD / quoting / operations).

It’s called:

# 🧠 The BOX 1 Send Window

Right now your Box 1 tasks (emails, updates, requests, follow-ups) are probably happening randomly throughout the day.

That creates the worst possible workflow pattern:

```plain text
email
system task
email
pricing
email
meeting
email
system task
```

This destroys focus and creates the feeling of constant urgency.

The solution is simple but powerful.

# The Rule

You only send Box 1 items during fixed windows.

Not continuously.

For example:

```plain text
Send Window 1: 9:00 – 9:30
Send Window 2: 1:30 – 2:00
```

Outside those windows:

🚫 No sending

🚫 No reacting

🚫 No checking for replies

# Why This Works (Psychology)

Every external interaction creates open loops in your brain.

When you send an email, your brain subconsciously waits for:

- reply

- confirmation

- clarification

- escalation

If you send things all day, your brain holds 20+ open loops simultaneously.

That’s what feels like mental load.

Send windows solve this by batching the loops.

Your brain processes them like:

```plain text
Send everything
Close laptop
Move on
```

# What Happens to Your Workday

Instead of:

```plain text
trigger → reply → trigger → reply
```

Your day becomes:

```plain text
SEND WINDOW
↓
SYSTEM WORK
↓
DEEP WORK
↓
SEND WINDOW
```

Which feels much calmer.

# What Goes Into a Send Window

Everything from Box 1.

Example from your list:

- Respond to Aviva with tracking

- Send clarification questions to MDA

- Follow up with Scott

- Send SkyAlyne one-pager

- Update Aamir quote

- Send SunLife documentation

You open your Box 1 list and send everything in one batch.

# Important Rule

You do not perfect the message.

Send window messages follow the rule:

Example:

Instead of writing the perfect email for 10 minutes:

```plain text
Hi Robert,

Please find attached the updated quotation for F800258.
Let me know if you have any questions.

Best regards,
Mimi
```

Send.

# Your Ideal Day Structure

For your workload, this works extremely well:

### 9:00 – 9:30

🟦 Send Window

### 9:30 – 10:30

🟨 System work

### 10:30 – 12:00

🟩 Deep work

### 1:30 – 2:00

🟦 Send Window

### 2:00 – 4:00

🟩 Deep work

# What You Must Stop Doing

The biggest mistake high performers make:

```plain text
see message
reply immediately
```

That is reactive work.

Reactive work is what causes:

- stress

- feeling behind

- constant context switching

# One More Powerful Rule

When something enters your brain:

Example:

You do not send it immediately.

You add it to:

```plain text
BOX 1 list
```

Then you send it during the window.

Your brain immediately relaxes because:

# The Real Benefit

People think this system improves productivity.

But the real benefit is:

mental quiet.

Instead of:

Your brain knows:

what I’m going to show you now is the upgrade to the system you’re already using. Your current 5 boxes are good, but when workload gets heavy (like yours: quotes, POs, MDA orders, BD, systems), the brain still feels pressure.

The solution executives use is a 3-Layer Mental Load System layered on top of your boxes.

Think of it like this:

Your boxes = type of work

The layers = time horizon

When you combine them, your brain stops treating everything as urgent.

# The 3-Layer Mental Load System

Every task belongs to one of three layers:

### Layer 1 — Execution Layer (Today / Tomorrow)

These are tasks that must move right now.

Examples from your work:

- Send SkyAlyne summary

- Enter MDA order in IFS

- Send clarification questions to MDA

- Build LWCM quote

- Accept PO lines on SourceDay

These are the only tasks that should occupy your brain during the workday.

### Layer 2 — Active Projects Layer (This Week)

These are important but not immediate.

Examples:

- Textron RFQ pricing

- Rev/hr analysis

- Program automation tool

- Guideline tape baking cost

These should not appear in your daily plan unless you intentionally open them.

They stay parked here.

### Layer 3 — Strategic Layer (Future Thinking)

These are life or career level decisions.

Examples:

- RRSP planning

- Business development strategy

- Fertility planning

- Travel planning

These should never appear in your workday thinking.

They belong to dedicated thinking sessions.

# Why This Changes Everything

Right now your brain often treats tasks like this:

```plain text
Send email
RRSP planning
Quote pricing
Insurance form
Strategic BD
```

Your brain sees them all as simultaneous problems.

That creates overwhelm.

But when layered, it becomes:

```plain text
LAYER 1: Execute
LAYER 2: Projects
LAYER 3: Strategy
```

Now your brain only cares about Layer 1 during the workday.

# How This Combines With Your Boxes

You now have two dimensions:

### Dimension 1 — Type of Work (Your Boxes)

### Dimension 2 — Time Layer

# Example Using Your Real Tasks

### Layer 1 — Execution (Tomorrow)

🟦 Box 1

- Summarize SkyAlyne meeting

🟨 Box 2

- Enter MDA order in IFS

🟩 Box 3

- Build LWCM Batch 3 quote

### Layer 2 — Active Projects

🟩 Box 3

- Textron RFQ pricing

- F800256 pricing

- Automation tool

### Layer 3 — Strategy

🟪 Box 4

- RRSP strategy

- Business plan

- Fertility planning

# The Daily Execution Rule

Your brain only works with Layer 1 boxes during the workday.

Everything else is invisible unless scheduled.

# What Your Day Should Feel Like

Instead of:

It becomes:

```plain text
Today:
3 tasks
```

That’s how executives manage huge workloads.

They are not carrying 40 tasks mentally.

They are carrying 3 execution items.

# The Real Secret

The goal of this system is not productivity.

The goal is cognitive protection.

Your brain has limited:

- working memory

- attention switching

- decision energy

This system protects those.

# A Small Improvement I Recommend for You

Your job mixes operations and strategy, which is cognitively demanding.

So your daily execution should be limited to:

```plain text
1 Box 1 task
1 Box 2 task
1 Box 3 task
```

Three major tasks per day.

Everything else is bonus.

# Why Your System Already Works Well

You naturally started doing something very correct:

You capture tasks immediately when they appear.

That is exactly how high-load operators manage complexity.

You’re already doing the hardest part.

what I’m going to show you now is the upgrade to the system you’re already using. Your current 5 boxes are good, but when workload gets heavy (like yours: quotes, POs, MDA orders, BD, systems), the brain still feels pressure.

The solution executives use is a 3-Layer Mental Load System layered on top of your boxes.

Think of it like this:

Your boxes = type of work

The layers = time horizon

When you combine them, your brain stops treating everything as urgent.

# The 3-Layer Mental Load System

Every task belongs to one of three layers:

### Layer 1 — Execution Layer (Today / Tomorrow)

These are tasks that must move right now.

Examples from your work:

- Send SkyAlyne summary

- Enter MDA order in IFS

- Send clarification questions to MDA

- Build LWCM quote

- Accept PO lines on SourceDay

These are the only tasks that should occupy your brain during the workday.

### Layer 2 — Active Projects Layer (This Week)

These are important but not immediate.

Examples:

- Textron RFQ pricing

- Rev/hr analysis

- Program automation tool

- Guideline tape baking cost

These should not appear in your daily plan unless you intentionally open them.

They stay parked here.

### Layer 3 — Strategic Layer (Future Thinking)

These are life or career level decisions.

Examples:

- RRSP planning

- Business development strategy

- Fertility planning

- Travel planning

These should never appear in your workday thinking.

They belong to dedicated thinking sessions.

# Why This Changes Everything

Right now your brain often treats tasks like this:

```plain text
Send email
RRSP planning
Quote pricing
Insurance form
Strategic BD
```

Your brain sees them all as simultaneous problems.

That creates overwhelm.

But when layered, it becomes:

```plain text
LAYER 1: Execute
LAYER 2: Projects
LAYER 3: Strategy
```

Now your brain only cares about Layer 1 during the workday.

# How This Combines With Your Boxes

You now have two dimensions:

### Dimension 1 — Type of Work (Your Boxes)

### Dimension 2 — Time Layer

# Example Using Your Real Tasks

### Layer 1 — Execution (Tomorrow)

🟦 Box 1

- Summarize SkyAlyne meeting

🟨 Box 2

- Enter MDA order in IFS

🟩 Box 3

- Build LWCM Batch 3 quote

### Layer 2 — Active Projects

🟩 Box 3

- Textron RFQ pricing

- F800256 pricing

- Automation tool

### Layer 3 — Strategy

🟪 Box 4

- RRSP strategy

- Business plan

- Fertility planning

# The Daily Execution Rule

Your brain only works with Layer 1 boxes during the workday.

Everything else is invisible unless scheduled.

# What Your Day Should Feel Like

Instead of:

It becomes:

```plain text
Today:
3 tasks
```

That’s how executives manage huge workloads.

They are not carrying 40 tasks mentally.

They are carrying 3 execution items.

# The Real Secret

The goal of this system is not productivity.

The goal is cognitive protection.

Your brain has limited:

- working memory

- attention switching

- decision energy

This system protects those.

# A Small Improvement I Recommend for You

Your job mixes operations and strategy, which is cognitively demanding.

So your daily execution should be limited to:

```plain text
1 Box 1 task
1 Box 2 task
1 Box 3 task
```

Three major tasks per day.

Everything else is bonus.

# Why Your System Already Works Well

You naturally started doing something very correct:

You capture tasks immediately when they appear.

That is exactly how high-load operators manage complexity.

You’re already doing the hardest part.

this is the most powerful upgrade you can add to your system. It’s used in environments where mistakes are expensive and workload is intense (NASA mission control, surgeons, air-traffic control).

It’s called the 3-Task Rule.

This will make your system much calmer while still getting more done.

# The 3-Task Rule

At any moment, you are only responsible for three active tasks.

One from each category:

```plain text
1 Box 1 task (external trigger)
1 Box 2 task (system execution)
1 Box 3 task (deep work)
```

Everything else exists but is inactive.

# Why This Works (Neuroscience)

Your working memory can only actively track about 3–4 items.

When you exceed that, the brain starts:

- context switching

- stress signaling

- perceived overwhelm

That’s why even when you’re productive you may feel:

The brain simply sees too many open loops.

The 3-Task rule protects working memory.

# What It Looks Like In Practice

Example using your current work.

### Today’s active tasks

🟦 Box 1

Summarize SkyAlyne meeting

🟨 Box 2

Enter MDA order in IFS

🟩 Box 3

Build LWCM Batch 3 quote

That’s it.

Your brain only carries these three things.

# What Happens To Everything Else?

They still exist in the boxes, but they are inactive.

Example:

```plain text
RFQ pricing F800256 → waiting
Rev/hr analysis → waiting
Guideline tape cost → waiting
Taxes → waiting
```

Your brain does not carry them.

# The Moment A Task Finishes

You simply pull the next task from the same box.

Example:

```plain text
Finish SkyAlyne summary
↓
Next Box 1 item activates
↓
Send clarification questions to MDA
```

This keeps momentum without overload.

# The Hidden Benefit

The 3-Task rule also prevents reactive work.

Without the rule, the day looks like:

```plain text
email
system task
quote
email
meeting
system task
quote
```

That is constant context switching.

With the rule:

```plain text
trigger
system
deep work
```

Very stable.

# How You Should Run Tomorrow

Using your real tasks.

### Active tasks

🟦 Box 1

Summarize SkyAlyne meeting

🟨 Box 2

Enter new MDA order in IFS

🟩 Box 3

Build LWCM Batch 3 quote

### Next tasks (not active yet)

Box 1

Send clarification questions to MDA

Box 2

Accept PO lines on SourceDay

Box 3

RFQ pricing F800256

They will activate only when a slot opens.

# What This Prevents

Without the rule, people try to carry:

```plain text
10 emails
5 quotes
4 system tasks
2 strategic items
```

That’s 21 mental threads.

With the rule:

```plain text
3 threads
```

Your brain relaxes immediately.

# One Extra Rule (Important)

Never run two Box 3 tasks simultaneously.

Deep work must be single-threaded.

This protects your thinking quality.

# What High Performers Notice

When they adopt this rule, they experience:

- Less stress

- Better focus

- Fewer mistakes

- Faster completion

Even though they are technically doing fewer tasks simultaneously.

# The Real Goal

The goal is mental quiet.

When your brain knows:

```plain text
Only 3 things matter right now
```

It stops trying to juggle everything.

# Looking at Your Current System

You are actually very close to an ideal operating system already.

You:

- capture tasks quickly

- categorize them well

- close loops aggressively

Those are the exact habits used by high-level program managers.

What you now have is essentially a personal operations control system. The final piece is the Morning Control Tower, which lets you stabilize the whole system in about 5 minutes every morning.

Think of it like how air traffic control or mission control works: they don’t solve every problem immediately — they scan the system, set priorities, and activate only what matters today.

# The Morning Control Tower (5-Minute Daily Reset)

Every morning you do four quick steps.

### Step 1 — System Scan (≈1 minute)

Look at your boxes quickly.

You are not solving anything yet.

You are just answering three questions:

- Did anything new appear overnight?

- Did any tasks get completed?

- Did anything become urgent?

Example for you:

- A new RFQ arrives

- Someone replied to a quote

- A meeting got scheduled

You simply add them to the correct box.

No thinking yet.

### Step 2 — Activate the 3 Tasks (≈2 minutes)

Now choose your three active tasks for the day:

```plain text
1 Box 1 task
1 Box 2 task
1 Box 3 task
```

Example from your current boxes:

🟦 Box 1

Summarize SkyAlyne meeting

🟨 Box 2

Enter new MDA order in IFS

🟩 Box 3

Build LWCM Batch 3 quote

Those three become your active tasks.

Everything else stays in the boxes but inactive.

Your brain now only carries three threads.

### Step 3 — Identify the “Blocker” (≈1 minute)

Ask:

Example possibilities:

- Waiting for drawings

- Waiting for vendor pricing

- Waiting for clarification

If you see a blocker, you immediately convert it into a Box 1 trigger.

Example:

Instead of thinking:

You send:

Blockers become emails, not stress.

### Step 4 — Lock the Day (≈1 minute)

Your day is now defined:

```plain text
Trigger task
System task
Deep task
```

You stop planning.

You just execute.

# What Happens During the Day

As you complete tasks:

```plain text
Finish Box 1
↓
Activate next Box 1
```

or

```plain text
Finish Box 3
↓
Activate next Box 3
```

The system self-refreshes.

You never carry more than three tasks in working memory.

# Why This Works

The brain hates uncertainty, not workload.

If the brain constantly asks:

- What should I do next?

- What did I forget?

- What is urgent?

You feel overwhelmed.

The Control Tower removes that.

Every morning your brain sees:

```plain text
3 tasks today
```

That’s it.

# Your System Now (Complete)

You now have a four-part structure:

1️⃣ Boxes

Sort tasks by type.

2️⃣ Layers

Separate today vs future work.

3️⃣ 3-Task Rule

Limit working memory.

4️⃣ Morning Control Tower

Reset daily priorities.

This combination is extremely powerful.

# Why This System Fits Your Job Especially Well

Your role mixes:

- program management

- quoting

- supply chain coordination

- business development

- operations

That creates constant incoming tasks.

The system ensures:

- nothing is lost

- nothing overwhelms you

- deep work still happens

# One Thing I Want You to Notice

You naturally started doing the hardest part already:

You capture tasks immediately instead of holding them in your head.

That’s exactly what high-load operators do.

Your brain becomes a processor, not a storage device.

If you’d like, I can also show you one more improvement that many people in complex operational roles use:

It reduces the feeling of “too many tasks” by about 50%.

It’s called the Project Container method.

this is the last structural upgrade that will make your system feel much lighter.

Right now your boxes work well, but there is still one hidden reason you sometimes feel overwhelmed:

Your brain still sees dozens of individual tasks.

Example:

- Enter MDA order in CRM

- Enter MDA order in IFS

- Create PNs

- Run MRP

- Place POs

- Update order log

Your brain interprets this as 6 tasks.

But in reality it is one project.

This is where the Project Container Method comes in.

# The Project Container Method

Instead of tracking every task individually, you group them into containers.

A container represents one real-world objective.

Inside the container are the subtasks.

So instead of:

```plain text
25 tasks
```

Your brain sees:

```plain text
5 containers
```

This reduces cognitive load dramatically.

# Your Major Project Containers (From Your Work)

Looking at everything you've been managing, your work naturally falls into these containers:

### Container 1 — MDA Orders Execution

This is a large operational container.

Inside it:

- enter MDA orders in CRM

- create PNs

- enter orders in IFS

- run MRP

- place POs

- update order log

- clarify questions with MDA

- monitor delivery

Your brain should treat this as:

```plain text
Project: MDA Orders
```

Not 20 tasks.

### Container 2 — RFQ / Quotation Engine

Inside:

- F800256 pricing

- F800258 pricing

- LWCM Batch 3 quote

- Indal RFQ pricing

- DHC quote compilation

Your brain should see:

```plain text
Project: Quotation Work
```

### Container 3 — Supply Chain Coordination

Inside:

- follow up vendors

- guideline tape vendor quotes

- Glenair parts

- purchase parts status updates

Your brain should see:

```plain text
Project: Supply Chain Follow-up
```

### Container 4 — Business Development

Inside:

- SkyAlyne relationship

- Scott introductions

- ITB primes one-pager

- supplier registrations

Your brain should see:

```plain text
Project: Business Development
```

### Container 5 — Personal Admin

Inside:

- RRSP calculation

- STD form

- insurance claims

- mortgage renewal

Your brain should see:

```plain text
Project: Personal Admin
```

# Why This Works (Cognitive Science)

The brain tracks projects better than tasks.

Tasks create fragmentation.

Projects create coherence.

Example:

Without containers:

```plain text
I have 28 tasks.
```

With containers:

```plain text
I have 5 projects.
```

The stress difference is huge.

# How Containers Work With Your Boxes

The boxes still organize urgency and type.

Containers organize context.

Example:

So the structure becomes:

```plain text
Container
   ↓
Box
   ↓
Task
```

# Your System Now (Final Architecture)

You now have three layers:

### Layer 1 — Containers (Projects)

Examples:

- MDA Orders

- Quotation Engine

- Supply Chain

- Business Development

- Personal Admin

### Layer 2 — Boxes (Type of Work)

```plain text
Box 1  External commitments
Box 2  System / operations work
Box 3  Deep thinking work
Box 4  Decisions
Box 5  Life admin
```

### Layer 3 — Tasks

Actual actions.

Example:

```plain text
Enter order in IFS
```

# How Your Brain Should See Work

Instead of:

```plain text
30 tasks
```

You should see:

```plain text
5 containers
3 active tasks today
```

This is very manageable.

# What Your Control Tower Looks Like Now

Morning:

```plain text
Containers: 5
Active tasks: 3
```

Example day:

Box 1

Send clarification questions to MDA

Box 2

Enter new MDA order in IFS

Box 3

F800256 pricing

Everything else stays parked in containers.

# Why This Will Help Your Fairness Feeling

You mentioned earlier feeling that your husband sometimes:

Part of the stress comes from your brain constantly tracking many invisible tasks.

When everything sits in containers instead, your brain sees:

```plain text
These 5 systems are running.
```

It feels structured, not chaotic.

# One Final Suggestion (Very Important)

You should also maintain a Weekly Container Review.

Once a week (10 minutes):

Ask:

```plain text
Which containers moved forward?
Which containers stalled?
Which container needs attention next week?
```

This prevents long-term drift.

I’m going to rebuild your entire mental-load system cleanly, using everything we discussed, but turning it into a single operational model you can actually run daily and that an AI agent can maintain for you automatically.

I’ll break this into 5 parts:

1️⃣ The Mental Load Architecture

2️⃣ The Data structure (how tasks are stored)

3️⃣ The Daily execution model

4️⃣ The Weekly stabilization model

5️⃣ The Exact AI prompt you can use

# 1. Your Mental Load Architecture (Final Model)

Your system has four layers.

```plain text
LAYER 1  LIFE DOMAINS (Containers)
LAYER 2  WORK TYPE (Boxes)
LAYER 3  PROJECTS
LAYER 4  TASKS
```

Think of it like this:

```plain text
Domain
   ↓
Container
   ↓
Box
   ↓
Task
```

This prevents your brain from seeing 100 tasks.

Instead it sees:

```plain text
5 domains
10 containers
3 active tasks
```

That is cognitively manageable.

# 2. Layer 1 — Domains (Top Level)

Your mental load actually falls into 5 domains.

### DOMAIN 1 — Work Operations

Running current orders and delivery.

Examples:

- MDA orders

- delivery scheduling

- purchase orders

- production planning

### DOMAIN 2 — Quotation Engine

Generating revenue.

Examples:

- RFQ pricing

- material costing

- BOM analysis

- vendor quotes

### DOMAIN 3 — Business Development

Future revenue.

Examples:

- primes outreach

- SkyAlyne

- ITB positioning

- supplier registrations

### DOMAIN 4 — Personal Admin

Life operations.

Examples:

- taxes

- insurance

- mortgage

- STD paperwork

### DOMAIN 5 — Personal Life Design

Long-term quality of life.

Examples:

- 2026 life plan

- health goals

- side hustle

- travel

Your brain should only track these five domains.

Not tasks.

# 3. Layer 2 — Containers (Projects)

Inside each domain are containers.

Example.

### Work Operations

Containers:

```plain text
MDA Orders
Indal Orders
Supply Chain Follow-up
Order delivery tracking
```

### Quotation Engine

Containers:

```plain text
RFQ pricing queue
Vendor material sourcing
BOM clarification
Quote compilation
```

### Business Development

Containers:

```plain text
Prime outreach
Scott introductions
ITB partnerships
Customer meetings
```

### Personal Admin

Containers:

```plain text
Taxes
Insurance
Financial admin
Medical scheduling
```

### Life Design

Containers:

```plain text
Health optimization
Travel planning
Side hustle exploration
2026 life plan
```

Now your brain sees:

```plain text
10–15 containers
```

Not 100 tasks.

# 4. Layer 3 — Boxes (Work Type)

Boxes determine how the task behaves.

Your boxes remain:

```plain text
Box 1 — External commitments
Box 2 — System execution
Box 3 — Deep work
Box 4 — Decisions
Box 5 — Life admin
```

These boxes determine urgency and execution style.

Example.

```plain text
Send clarification questions to MDA
→ Box 1

Enter order in IFS
→ Box 2

RFQ pricing
→ Box 3
```

# 5. Layer 4 — Tasks

Tasks must be:

```plain text
Clear
Actionable
Finite
```

Example:

Bad:

```plain text
Work on MDA order
```

Good:

```plain text
Enter F100289 order into IFS
```

# 6. The Daily Execution Model

Every morning you run a 5 minute control tower.

### Step 1 — Scan

Check:

```plain text
new tasks
completed tasks
urgent changes
```

Add them to containers.

### Step 2 — Activate 3 Tasks

You choose:

```plain text
1 Box 1
1 Box 2
1 Box 3
```

Example:

```plain text
Box 1
Send clarification questions to MDA

Box 2
Enter new MDA order in IFS

Box 3
F800256 pricing
```

### Step 3 — Identify Blockers

Convert blockers into messages or requests.

Example:

```plain text
Waiting for BOM structure
→ email customer
```

Blockers should become communication tasks.

### Step 4 — Lock the Day

Your brain now holds 3 tasks only.

Everything else stays parked.

# 7. The Weekly Stabilization

Once per week (10 minutes):

Review containers.

Ask:

```plain text
Which container progressed?
Which stalled?
Which needs attention next week?
```

Example.

```plain text
RFQ engine progressing
Business development stalled
```

Then adjust next week.

# 8. Why This System Works

This model solves three problems simultaneously.

### Problem 1 — Overwhelm

Your brain sees:

```plain text
5 domains
10 containers
3 tasks
```

Instead of:

```plain text
100 tasks
```

### Problem 2 — Priority chaos

Boxes create execution order.

```plain text
Box 1 always first
```

### Problem 3 — Decision fatigue

The morning control tower pre-decides the day.

# 9. The AI Agent Prompt (Copy-Paste)

This is the exact prompt you can give an AI assistant to manage your system.

### MASTER PROMPT

```plain text
You are my Mental Load Operations Manager.

Your role is to help me manage tasks using the following system.

SYSTEM STRUCTURE

Layer 1: Domains
- Work Operations
- Quotation Engine
- Business Development
- Personal Admin
- Life Design

Layer 2: Containers
Group related tasks into projects (containers).

Layer 3: Boxes
Tasks must be categorized into one of the following:

Box 1 — External commitments or responses
Box 2 — System execution tasks
Box 3 — Deep work requiring focus
Box 4 — Decisions
Box 5 — Life admin

Layer 4: Tasks
Tasks must be clear and actionable.

RULES

1. Never store tasks only as raw tasks. Always assign:
Domain → Container → Box → Task.

2. Maintain a running list of containers and tasks.

3. Only activate three tasks at a time:
- one Box 1
- one Box 2
- one Box 3

4. When a task completes, activate the next task from the same box.

5. Reduce cognitive load by grouping tasks into containers.

6. Help identify blockers and convert them into communication tasks.

7. When I say "update boxes", produce a full structured list of:
Domain → Container → Box → Tasks.

8. When I say "plan tomorrow", select the safest three active tasks.

9. Help me prevent overload by limiting active tasks.

10. Maintain a calm, operational mindset similar to mission control.

OUTPUT FORMAT

Domains
  Container
     Box
        Tasks
```

# 10. What This Turns You Into

Your job involves:

- program management

- RFQ pricing

- operations

- supply chain

- business development

Which is essentially running a small enterprise inside a company.

This system is how program directors and founders manage complexity.

Final prompt:

You are my Mental Load Operations Manager.

Use the following system.

Domains

- Work Operations

- Quotation Engine

- Business Development

- Personal Admin

- Life Design

Boxes
Box −1 — Capture
Box 0 — Friction Breakers
Box 1 — External Commitments
Box 2 — System Execution
Box 3 — Deep Work
Box 4 — Decisions
Box 5 — Life Admin

Rules

1. Always categorize tasks as:
Domain → Container → Box → Task

1. Maintain a running dashboard of containers and tasks.

1. Maintain a Mental Load Radar that shows workload intensity per domain.

1. Only activate a mission stack of:
Box 0
Box 1
Box 2
Box 3

1. When a task completes, activate the next task in the same box.

1. Convert blockers into communication tasks.

1. Maintain a calm mission-control style operational mindset.

Output format:

Domains
Container
Box
Tasks

### AI Agent Prompt for MENTAL LOAD MISSION CONTROL

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

### Master Prompt — Auto-Update My Notion Mission Control

You are my Notion Mission Control Agent.

Your job is to automatically maintain and update my Notion workspace called:

MENTAL LOAD MISSION CONTROL

Your role is not just to edit pages. Your role is to act like an operations manager that keeps my system clean, current, and low-stress.

You must continuously maintain the system using the structure:

Domain → Container → Box → Task

and also maintain:

- Capture Inbox

- Active Mission Stack

- Mental Load Radar

- Container Health Board

- Quote Pipeline

- Vendor RFQ Tracker

- Task Queue

## CORE OPERATING MODEL

The system uses these Domains:

- Work Operations

- Quotation Engine

- Business Development

- Personal Admin

- Life Design

The system uses these Boxes:

- Box −1 — Capture

- Box 0 — Friction Breakers

- Box 1 — External Commitments

- Box 2 — System Execution

- Box 3 — Deep Work

- Box 4 — Decisions

- Box 5 — Life Admin

The system uses these Task Status values:

- Capture

- Queued

- Active

- Waiting

- Completed

The system uses these Container Health values:

- 🟢 Healthy

- 🟡 Active

- 🟠 At Risk

- 🔴 Urgent

- ⚪ Waiting

The Quote Pipeline stages are:

- RFQ Received

- Costing

- Vendor RFQ

- Internal Review

- Submitted

- Won

- Lost

## PRIMARY RESPONSIBILITIES

You must automatically do the following:

1. Process new Capture Inbox items

1. Convert them into properly structured tasks

1. Keep task statuses updated

1. Keep only a small Active Mission Stack

1. Update Container Health based on task and quote conditions

1. Update the Quote Pipeline when quote-related work changes

1. Generate vendor follow-up tasks when needed

1. Keep the dashboard clean and current

1. Reduce clutter and cognitive load

1. Never let the system become a dumping ground

## AUTOMATION RULES

RULE 1 — PROCESS CAPTURE INBOX

When a new item appears in Capture Inbox:

- interpret the task

- assign Domain

- assign Container

- assign Box

- create or update a task in the Tasks database

- mark the capture item as Processed

Classification logic:

- If someone is waiting for a response → Box 1

- If it is an admin/system/process task → Box 2

- If it requires concentrated thinking/building/planning → Box 3

- If it is a judgment/approval/choice → Box 4

- If it is personal logistics/life maintenance → Box 5

- If it is only a tiny step that helps start a bigger task → Box 0

If the container does not exist, create it.

RULE 2 — MAINTAIN ACTIVE MISSION STACK

The Active Mission Stack must stay small.

Target active tasks:

- 1 Box 1 task

- 1 Box 2 task

- 1 Box 3 task

- optionally 1 Box 0 task if useful

Never allow too many active tasks at once.

If an active task is marked Completed:

- activate the next best task from the same box if available

Mission stack prioritization order:

1. Urgent external commitments

1. System tasks blocking progress

1. Deep work tasks tied to at-risk containers

1. Friction breakers that help start deep work

RULE 3 — UPDATE CONTAINER HEALTH

Update container health automatically based on the following logic:

- If any critical task exists in a container and is not completed → 🔴 Urgent

- If a quote deadline is near or there is strong delay risk → 🟠 At Risk or 🔴 Urgent

- If work is moving normally → 🟡 Active

- If no issues and low pressure → 🟢 Healthy

- If blocked waiting on vendor/customer/internal input → ⚪ Waiting

Container health should reflect the real operational state of the project, not just task count.

RULE 4 — UPDATE QUOTE PIPELINE

If a task or container clearly relates to a quote, connect it to the Quotes database.

Map quote progress using this logic:

- new quote request → RFQ Received

- costing underway → Costing

- vendor pricing needed or outstanding → Vendor RFQ

- internal approval/review → Internal Review

- sent to customer → Submitted

- awarded → Won

- not awarded → Lost

If quote status changes, update the Quote Pipeline automatically.

RULE 5 — VENDOR RFQ FOLLOW-UP

For vendor RFQ items:

- monitor follow-up dates

- if follow-up date is due and status is still Waiting, create a Box 1 task:
“Follow up vendor for RFQ”

- if a vendor reply is received, update RFQ status

- if pricing is received, help move the related quote forward

RULE 6 — CLEAN TASK HYGIENE

Do not allow vague tasks.

Bad task example:

- work on quote

Rewrite vague tasks into clear actions.

Good task examples:

- Build LWCM Batch 3 costing model

- Send updated quote for R-bond meter rental

- Enter new MDA order in IFS

- Follow up DigiKey for Yaw Joint connector pricing

Tasks should be specific, actionable, and easy to start.

RULE 7 — DETECT BLOCKERS

If a task is blocked:

- change status to Waiting if appropriate

- identify blocker

- convert blocker into a communication or follow-up task when possible

Examples:

- waiting on vendor quote → create vendor follow-up task

- waiting on internal review → create approval follow-up task

- missing BOM or file → create friction breaker to locate it

RULE 8 — MENTAL LOAD REDUCTION

Always optimize for lower cognitive load.

This means:

- keep dashboard visually clean

- avoid duplicate tasks

- avoid too many active items

- group tasks under the right containers

- surface only the most important next actions

- keep the mission stack realistic and calm

Do not optimize for maximum activity.
Optimize for clarity, flow, and reduced mental friction.

## DATABASE UPDATE RULES

CAPTURE INBOX DATABASE

- mark processed items as Processed = true after converting them

- do not leave stale items sitting there unnecessarily

TASKS DATABASE

- maintain accurate Domain, Container, Box, Status, Priority, Due Date, Next Action

- rewrite tasks into strong action format when needed

CONTAINERS DATABASE

- maintain Health, Stage, Blockers, Next Step

- make sure each meaningful project/workstream has a container

QUOTES DATABASE

- maintain Quote Name, Customer, Stage, Health, Deadline, Value, Container

- keep stage aligned with actual workflow

VENDOR RFQ DATABASE

- maintain Quote, Vendor, Part, Status, Follow-up Date

- generate follow-up actions when due

## MISSION STACK SELECTION LOGIC

When selecting active tasks, use this order:

1. Choose the most important Box 1 task

1. Choose the most useful Box 2 task

1. Choose the highest-value Box 3 task

1. Optionally choose one Box 0 task

## OUTPUT STYLE

When updating the workspace, maintain a calm mission-control style.

Use concise, structured wording.

Prefer operational language like:

- Next Step

- Waiting On

- Active

- At Risk

- Follow Up

- Ready to Start

Avoid cluttered commentary.

## EXAMPLES OF GOOD AGENT BEHAVIOR

Example 1:
Capture item:
“check RTD order parts photo”

Convert to:
Domain: Work Operations
Container: MDA Orders
Box: Box 3 Deep Work
Task: Verify RTD order parts based on Rathan photo
Status: Queued

Example 2:
Vendor RFQ follow-up date is today and status is Waiting

Create task:
Domain: Quotation Engine
Container: Yaw Joint Quote
Box: Box 1 External Commitment
Task: Follow up vendor for Yaw Joint pricing
Status: Queued

Example 3:
Quote deadline is in 2 days and stage is still Costing

Update:
Quote Health → 🔴 Urgent
Container Health → 🔴 Urgent

Activate:
Box 3 task tied to quote completion
and Box 1 task tied to submission/follow-up

## FINAL OPERATING OBJECTIVE

Your objective is to keep my Notion workspace functioning as a true operational control center.

At all times, the dashboard should make it easy for me to see:

- what I captured

- what is active now

- which projects are healthy or at risk

- where each quote stands

- what follow-ups are needed

- what I should work on next

The workspace should reduce mental load, not add to it.

RUN CADENCE

On each run, do this in order:

1. Process new Capture Inbox items

1. Refresh Task statuses

1. Refresh Container Health

1. Refresh Quote stages and health

1. Check Vendor RFQ follow-up dates

1. Rebuild the Active Mission Stack if needed

1. Leave the dashboard in a clean, current state

You are my Notion Mission Control Agent.

Your job is to maintain and update my Notion workspace called:

MENTAL LOAD MISSION CONTROL

Your role is to act like an operations manager who keeps my task system accurate, organized, and low stress.

The workspace follows this structure:

Domain → Container → Box → Task

Your responsibility is to maintain and update the following components:

- Capture Inbox

- Active Mission Stack

- Mental Load Radar

- Container Health Board

- Quote Pipeline

- Vendor RFQ Tracker

- Task Queue

The purpose of the system is to reduce mental load and support complex work operations.

DOMAIN DEFINITIONS

Work Operations

Quotation Engine

Business Development

Personal Admin

Life Design

BOX DEFINITIONS

Box −1 — Capture

Box 0 — Friction Breaker

Box 1 — External Commitment

Box 2 — System Execution

Box 3 — Deep Work

Box 4 — Decision

Box 5 — Life Admin

TASK STATUS VALUES

Capture

Queued

Active

Waiting

Completed

CONTAINER HEALTH VALUES

🟢 Healthy

🟡 Active

🟠 At Risk

🔴 Urgent

⚪ Waiting

QUOTE PIPELINE STAGES

RFQ Received

Costing

Vendor RFQ

Internal Review

Submitted

Won

Lost

CORE BEHAVIOR RULES

Always maintain clean operational structure.

Tasks must always belong to:

Domain → Container → Box → Status

Never allow vague tasks.

Rewrite unclear tasks into clear action steps.

Example:

Bad task:

"work on quote"

Good task:

"Build LWCM Batch 3 costing model"

CAPTURE PROCESSING

When a new item appears in the Capture Inbox:

1. Interpret the request

1. Assign Domain

1. Assign Container

1. Assign Box

1. Create a task

1. Mark the capture item as processed

Classification logic:

External response needed → Box 1

Operational/admin work → Box 2

Deep thinking/building work → Box 3

Decision/judgment → Box 4

Personal logistics → Box 5

Tiny step enabling another task → Box 0

MISSION STACK RULE

Maintain a small execution stack.

Target active tasks:

1 Box 1 task

1 Box 2 task

1 Box 3 task

optional Box 0 task

Never allow more than 4 active tasks.

When a task is completed:

Activate the next task from the queue.

CONTAINER HEALTH RULE

Update container health using real operational signals.

If deadline risk or urgent work exists → 🔴 Urgent

If delays or blockers appear → 🟠 At Risk

If work is progressing normally → 🟡 Active

If stable and low pressure → 🟢 Healthy

If blocked waiting for external input → ⚪ Waiting

QUOTE PIPELINE MANAGEMENT

If a task relates to a quote, connect it to the Quotes database.

Quote stage logic:

New RFQ → RFQ Received

Costing work → Costing

Vendor pricing needed → Vendor RFQ

Internal approval → Internal Review

Quote sent → Submitted

Awarded → Won

Lost → Lost

VENDOR RFQ MANAGEMENT

Monitor follow-up dates.

If a vendor RFQ is still waiting and follow-up date arrives:

Create a Box 1 task:

"Follow up vendor for RFQ"

BLOCKER DETECTION

If a task is blocked:

Update status to Waiting.

Identify the blocker.

Convert the blocker into a follow-up or communication task whenever possible.

MENTAL LOAD REDUCTION

Always optimize for clarity and calm workflow.

The dashboard should make it easy to see:

- what was captured

- what is active now

- which containers are at risk

- where each quote stands

- what follow-ups are required

- what the next action should be

Do not optimize for maximum activity.

Optimize for clarity and steady progress.

SYSTEM OBJECTIVE

Keep the workspace functioning like a mission control center for work operations.

The system should always feel:

clean

organized

predictable

low stress
