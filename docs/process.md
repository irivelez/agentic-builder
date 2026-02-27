# Process & Methodology — How This Was Built

## Context

This documents the real process followed to build the first WhatsApp automation (Redin), including how the human-AI collaboration worked, what we discovered, and why this process itself reveals the platform opportunity.

## Step 1: Discovery Call (Human-Led)

**Who**: Irina (founder) on a Zoom call with the customer (Redin's owner)

**What happened**: 
- Irina listened to the customer describe his day-to-day workflows and pain points
- He explained how his business operates: customers send WhatsApp messages with maintenance issues (text, photos, voice notes), and his team manually triages, classifies, assigns technicians, and coordinates schedules
- He showed his existing system — an AppSheet application built on Google Sheets that tracks work orders, clients, technicians, and service history
- The conversation was unstructured — not a form, not a checklist, just a natural conversation about how the business works

**Key insight**: The discovery process requires human empathy and business understanding. The customer didn't describe his "data model" or "workflow automation requirements" — he described his problems in plain language. The translation from problems to solutions happened afterward.

## Step 2: Insight Transfer (Human → AI)

**Who**: Irina typing insights to Ari (AI co-builder)

**What happened**:
- After the Zoom call, Irina shared the key insights with the AI in natural language
- She described the business type, the WhatsApp-centric workflow, the pain points
- She sent **screenshots** of the customer's AppSheet system so the AI could understand the data structure
- The AI analyzed the screenshots visually and reverse-engineered the data model without any API documentation

**Key insight**: The information pipeline was: customer explains verbally → Irina translates to written insights → AI processes and proposes. Screenshots bridged the gap between a non-technical customer and a technical solution — the customer couldn't describe his schema, but the AI could read it from screen captures.

## Step 3: Solution Design (AI-Led)

**Who**: Ari (AI) proposed the architecture

**What happened**:
- Based on the insights and screenshots, the AI designed a complete solution:
  - WhatsApp webhook to receive messages (text, images, audio)
  - Claude Haiku for intelligent interpretation and classification
  - Whisper for voice note transcription
  - Structured work order generation
  - Interactive WhatsApp buttons for confirmation
  - Web dashboard for the operations team
- The AI also identified that this use case generalizes to an entire category of businesses: services with physical locations that dispatch field workers

**Key insight**: The AI didn't just build what was asked — it recognized the pattern. Field maintenance is one instance of a general problem: businesses where customers request services via WhatsApp, someone checks a system, assigns/schedules, and coordinates. This is the same pattern for barberías, veterinarias, talleres, and dozens of other business types.

## Step 4: Build (AI-Led, Parallel Execution)

**Who**: Ari (AI) using sub-agents for parallel development

**What happened**:
- Multiple sub-agents were deployed simultaneously to build different components
- The entire WhatsApp bot (webhook, AI processing, dashboard, configuration) was built in approximately 2 hours
- The bot was designed config-driven: all customer-specific details (services, cities, team, prompts) live in a single configuration file
- This design choice was intentional — the config file is what a future intake agent would generate automatically

**Key insight**: Building the bot is fast. The bottleneck was never the code. The bottleneck is everything AROUND the code: understanding the customer, getting WhatsApp connected, integrating with their existing system.

## Step 5: WhatsApp Connection Attempt (BLOCKED)

**Who**: Irina and the customer trying to set up Meta WhatsApp Cloud API

**What happened**:
1. The customer needed to create a Meta Developer account to get WhatsApp API credentials
2. He **forgot his existing Meta/Facebook password** (not a technical problem — a human memory problem that anyone can have)
3. After recovery attempts, they tried creating a **new Meta account**
4. Meta **rejected the new account** because it was too recent — new accounts aren't allowed to create developer apps immediately
5. The process was completely blocked.

**Key insight**: The password issue is incidental. The real problem is structural: **the automation depends on the customer completing a task on a platform they don't understand.** The bot was built, tested, and ready — but it couldn't go live because the customer needed to successfully navigate Meta's developer portal, create an app, register their business, configure webhooks, and generate tokens. Even without the password issue, this would have been a wall.

This creates a **dependency on the customer's side** to complete the WhatsApp provisioning. We can't do it for them (it's their business account), and they can't do it alone (it's a developer portal). That dependency — where the platform's deployment is blocked by a step the customer can't execute — is the single biggest friction point. It must be eliminated entirely from the customer experience.

## Step 6: Integration Reality Check

**What happened**:
- Even with the bot built and working, we identified a critical gap: the bot doesn't connect to the customer's existing AppSheet/Google Sheets system
- The customer uses AppSheet daily for managing work orders, but he has no idea how to access its API
- When asked about API access, he didn't understand the question
- However, AppSheet runs on **Google Sheets** underneath — and the customer knows how to share a Google Sheet (it's a common action for any Google user)

**Key insight**: Integration is mandatory for real value. Without it, the bot creates a parallel system that adds complexity instead of reducing it. The path to integration for non-technical users is through Google Sheets (share the sheet) rather than through APIs (impossible for them).

## Step 7: Pattern Recognition & Generalization

**What happened**:
- After the build and the friction analysis, the AI identified that this exact scenario applies to a broad category of businesses:
  - Any service business with physical locations
  - Any business that dispatches field workers
  - Any business where customers communicate primarily through WhatsApp
  - Any business using basic tools (Sheets, AppSheet, paper) to track operations
- The positioning was refined: we're not adding WhatsApp to businesses — we're automating the WhatsApp channel they already depend on
- The framing shifted from "here's a bot" to "your WhatsApp keeps working exactly like today, but it answers while you sleep"

**Key insight**: The process we followed manually (discover → translate → build → deploy) IS the product. An agentic platform that automates this entire pipeline — from customer conversation to live WhatsApp automation — is the platform opportunity.

## The Process as Product

```
MANUAL (today):
  Zoom call (Irina) → Insights typed to AI → AI designs → AI builds
  → Manual Meta setup (BLOCKED) → Manual integration (MISSING)

AUTONOMOUS (target):
  AI discovery agent (chat/voice) → Screenshots analyzed by vision
  → AI designs + generates config → AI builds + deploys
  → Automated WhatsApp provisioning → Automated Google Sheets integration
```

The goal is to compress a process that took a full day (and hit two blockers) into a self-service flow that takes 15 minutes with zero technical knowledge required.
