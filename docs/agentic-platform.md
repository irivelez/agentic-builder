# Agentic Platform Proposal — Autonomous WhatsApp Automation Builder

## Vision

A platform where any LATAM business that communicates with customers through WhatsApp can automate their existing workflow in under 15 minutes, with zero technical knowledge, and integrated with whatever system they already use.

**The customer experience should feel like:** "I had a chat, showed some screenshots, confirmed a few things, and now my WhatsApp answers for me."

---

## Platform Architecture

### The Agentic Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                    CUSTOMER JOURNEY                          │
│                                                              │
│  1. DISCOVERY          2. CONNECT           3. DEPLOY        │
│  ┌──────────────┐     ┌──────────────┐     ┌─────────────┐  │
│  │ Discovery     │     │ WhatsApp     │     │ Bot goes    │  │
│  │ Agent         │────▶│ Provisioning │────▶│ live        │  │
│  │ (converses +  │     │ (Embedded    │     │ (auto-      │  │
│  │  analyzes     │     │  Signup,     │     │  deployed,  │  │
│  │  screenshots) │     │  zero-touch) │     │  tested)    │  │
│  └──────────────┘     └──────────────┘     └─────────────┘  │
│         │                                         │          │
│         ▼                                         ▼          │
│  ┌──────────────┐                          ┌─────────────┐  │
│  │ Integration   │                          │ Ongoing     │  │
│  │ Agent         │                          │ Learning +  │  │
│  │ (connects to  │                          │ Iteration   │  │
│  │  their Sheets/│                          │             │  │
│  │  AppSheet)    │                          └─────────────┘  │
│  └──────────────┘                                            │
└─────────────────────────────────────────────────────────────┘
```

---

## Agent 1: Discovery Agent

### Purpose
Replaces the human-led Zoom discovery call. Conducts a natural conversation with the business owner to understand their business, workflows, and existing systems.

### How It Works

**Input**: A conversation in Spanish (text or voice via WhatsApp itself)

**Conversation flow** (not rigid — adaptive based on responses):

1. **Business understanding**
   - "Hola, cuéntame sobre tu negocio. ¿A qué se dedican?"
   - "¿Cuántas personas trabajan contigo?"
   - "¿Cuántos mensajes de clientes recibes por WhatsApp al día, más o menos?"

2. **Workflow mapping**
   - "Cuando un cliente te escribe por WhatsApp, ¿qué haces normalmente?"
   - "¿Cómo decides quién atiende cada solicitud?"
   - "¿Qué pasa después de que el servicio se completa?"

3. **System discovery (via screenshots)**
   - "¿Usas algún sistema para organizar tu negocio? Una hoja de cálculo, una app, un cuaderno..."
   - "¿Me puedes mandar un pantallazo de cómo se ve?"
   - [Receives screenshot] → Vision model analyzes and maps the data structure
   - "Veo que tienes una tabla con columnas de nombre, teléfono, dirección y servicio. ¿Eso es tu lista de clientes?"

4. **Automation proposal (plain language)**
   - "Basado en lo que me cuentas, esto es lo que tu WhatsApp puede hacer solo:"
   - "Cuando un cliente te escribe pidiendo [servicio], el bot puede [acción]. ¿Te sirve?"
   - Only YES/NO decisions from the customer

**Output**: A JSON specification containing:
```json
{
  "business": {
    "name": "Redin",
    "type": "field_services",
    "services": ["pintura", "electricidad", "plomería", ...],
    "locations": ["Bogotá", "Medellín", ...],
    "workingHours": { "start": 7, "end": 18 },
    "language": "es-CO"
  },
  "workflows": [
    {
      "trigger": "customer_requests_service",
      "steps": ["classify_request", "check_availability", "assign_technician", "confirm_schedule"],
      "escalation": "owner_whatsapp"
    }
  ],
  "integration": {
    "type": "google_sheets",
    "sheetId": "abc123...",
    "tables": {
      "clients": { "columns": [...], "mapping": {...} },
      "workOrders": { "columns": [...], "mapping": {...} }
    }
  },
  "tone": "professional_warm",
  "responses": {
    "greeting": "...",
    "outOfHours": "...",
    "escalation": "..."
  }
}
```

### Key Design Principles
- **Conversational, not forms**: Natural dialogue, not a step-by-step wizard
- **Screenshots over questions**: When in doubt, ask to see rather than ask to describe
- **Confirm, don't ask**: Propose what the bot will do, let customer say yes/no
- **Spanish-first**: All interactions in natural LATAM Spanish
- **Max 10 minutes**: The entire discovery should complete in one WhatsApp conversation

---

## Agent 2: WhatsApp Provisioning (Zero-Touch)

### The Problem
Meta's developer portal is impossible for non-technical users (see [Friction Analysis](./friction-analysis.md)). Password recovery, account age restrictions, and portal complexity block customers before they start.

### Solution Options

#### Option A: Meta Embedded Signup (Recommended First Step)
- Customer sees a button: "Conectar WhatsApp"
- Clicks → Facebook OAuth popup
- Logs in with Facebook (they all have Facebook)
- Approves permissions
- We receive Phone Number ID, WABA ID, and token programmatically
- **Customer never sees the developer portal**

**Requirements**: Register as Meta Tech Provider, implement Facebook Login for Business

**Limitation**: Still requires the customer to have a working Facebook account

#### Option B: BSP (Business Solution Provider) Model
- We register as a WhatsApp BSP (or partner with one like 360dialog, Gupshup)
- Customers provide their phone number
- We handle ALL Meta provisioning on their behalf
- Customer experience: "Dame tu número de WhatsApp" → SMS verification → done

**Requirements**: Higher Meta partnership tier, more compliance

**Limitation**: More complex to set up initially, ongoing BSP fees

#### Option C: Hybrid (Pragmatic)
- Try Embedded Signup first (fast, low friction)
- Fall back to manual provisioning with guided video walkthrough for edge cases
- Move to BSP as volume justifies it

### The Target Experience
```
Customer: [clicks "Conectar WhatsApp"]
System:   [Facebook OAuth popup]
Customer: [logs in, clicks "Allow"]
System:   "¡Listo! Tu WhatsApp está conectado. Envíale un mensaje de prueba."
Customer: [sends "hola" to their own number]
Bot:      "¡Hola! Soy el asistente de [Negocio]. ¿En qué puedo ayudarte?"
Customer: "🤯"
```

---

## Agent 3: Integration Agent (Screenshot-to-Data-Model)

### The Problem
Business owners can't provide APIs, but they CAN show their screens. The platform needs to connect to their existing system without requiring technical knowledge.

### How It Works

1. **Receive screenshots** of the customer's existing system (AppSheet, Google Sheets, Excel, etc.)

2. **Vision analysis** extracts:
   - Column/field names
   - Data types (text, numbers, dates, phone numbers)
   - Relationships between tables/sheets
   - Current workflow state (statuses, categories)
   - Sample data patterns

3. **Data model generation** — maps screenshot data to a structured schema:
   ```json
   {
     "source": "google_sheets",
     "tables": {
       "clientes": {
         "columns": [
           { "name": "Nombre", "type": "text", "maps_to": "customer_name" },
           { "name": "Teléfono", "type": "phone", "maps_to": "customer_phone" },
           { "name": "Dirección", "type": "text", "maps_to": "location" }
         ]
       },
       "ordenes": {
         "columns": [
           { "name": "Estado", "type": "enum", "values": ["Pendiente", "En proceso", "Completado"] },
           { "name": "Servicio", "type": "text", "maps_to": "service_type" }
         ]
       }
     }
   }
   ```

4. **Connection via Google Sheets** — the universal bridge:
   - Customer shares the Google Sheet (simple "Compartir" action they already know)
   - Platform accesses via Google Sheets API (OAuth)
   - Reads and writes to the same data their AppSheet/dashboard shows
   - **The customer's existing system keeps working exactly the same**

### Integration Patterns by System Type

| Customer's System | Integration Path |
|-------------------|-----------------|
| Google Sheets | Direct API (sheet shared with service account) |
| AppSheet | Through underlying Google Sheet |
| Excel (local) | Upload to Google Sheets, sync |
| Paper notebook | Digitize into new Google Sheet (bot creates it) |
| No system | Bot creates a Google Sheet as the system of record |
| Custom software | Screenshot analysis → manual bridge (future: API discovery) |

### The Key Principle
**Google Sheets is the universal integration layer.** Almost every low-tech system in LATAM either runs on Google Sheets already or can be bridged through it. The customer action is always the same: "Compartí tu hoja conmigo."

---

## Agent 4: Bot Builder (Config-to-Deploy)

### Purpose
Takes the JSON specification from the Discovery Agent + the data model from the Integration Agent and produces a working, deployed WhatsApp bot.

### How It Works

1. **Receives** the complete specification (business details, workflows, integration map, tone)
2. **Selects** the appropriate handler template (based on business type classification)
3. **Customizes** the system prompt with business-specific details (services, hours, locations, tone)
4. **Generates** the config file (the single file that defines the entire bot's behavior)
5. **Connects** the Google Sheets integration with the mapped columns
6. **Deploys** the bot instance (serverless function + webhook registration)
7. **Registers** the webhook with Meta (automatic, via API)
8. **Runs** a self-test (sends test message, verifies response)
9. **Notifies** the customer: "Tu bot está listo. Pruébalo enviando un mensaje."

### Multi-Tenant Architecture
Each customer bot is NOT a separate deployment. It's a tenant in a shared platform:
- One webhook endpoint routes to the right bot config by phone number
- Each tenant has: system prompt, handler type, Google Sheet connection, conversation history
- Shared infrastructure, individual configurations

### Handler Templates (by Business Category)

| Template | Used For | Core Workflow |
|----------|----------|---------------|
| `intake_classify` | Field services, maintenance | Receive request → classify → assign → schedule |
| `appointment_book` | Salons, clinics, vets | Check availability → propose times → book → confirm |
| `order_catalog` | Restaurants, stores | Show catalog → take order → confirm → track delivery |
| `faq_route` | Professional services | Answer common questions → route to specialist |
| `dispatch_track` | Logistics, delivery | Receive pickup request → assign driver → track status |

---

## Agent 5: Human Handoff Manager

### Purpose
No bot handles 100% of conversations. When the bot can't handle something, it must escalate gracefully to the business owner or team.

### Escalation Triggers
- Customer explicitly asks for a human: "Quiero hablar con una persona"
- Conversation loops (3+ unresolved exchanges)
- Sensitive topics (complaints, refunds, emergencies)
- Bot confidence drops below threshold
- Out-of-scope requests

### How It Works
1. Bot sends: "Te voy a comunicar con [Nombre] para que te ayude directamente."
2. Notification sent to owner's WhatsApp: "[Nuevo caso] Cliente [nombre] necesita atención directa. Contexto: [summary]"
3. Bot includes the full conversation summary so the owner doesn't ask the customer to repeat
4. Owner takes over the conversation (or responds and bot resumes)

---

## Revenue Model

| Tier | Price | Includes |
|------|-------|----------|
| Free | $0/mo | 50 conversations/mo, 1 bot, basic responses |
| Starter | $19/mo | 500 conversations, 1 bot, Google Sheets integration |
| Growth | $49/mo | 2,000 conversations, 3 bots, 3 team members, analytics |
| Pro | $99/mo | 10,000 conversations, unlimited bots, white-label, priority |

**Infrastructure cost per customer**: $0.77–5.00/month → **75–90% gross margins**

**Validated comp**: Vambe (Chile) grew from $20K to $1M ARR in 8 months at $19/month price point.

---

## Build Priorities

### Phase 1: Core Pipeline (Weeks 1-2)
- [ ] Discovery agent (conversational intake via WhatsApp)
- [ ] Screenshot-to-data-model via vision analysis
- [ ] Config generator from discovery output
- [ ] Multi-tenant bot deployment (shared infrastructure)

### Phase 2: WhatsApp Provisioning (Weeks 2-3)
- [ ] Meta Embedded Signup integration
- [ ] Automatic webhook registration
- [ ] Self-test flow

### Phase 3: Integration (Weeks 3-4)
- [ ] Google Sheets connection (OAuth flow)
- [ ] Read/write to customer's existing sheets
- [ ] Column mapping from vision analysis

### Phase 4: Monetization (Week 4+)
- [ ] Stripe checkout integration
- [ ] Usage tracking and tier enforcement
- [ ] Customer dashboard (basic analytics)

---

## Success Criteria

The platform works when:
1. A business owner with zero technical knowledge can go from "I want to automate my WhatsApp" to a working bot in **under 15 minutes**
2. They **never see** a developer portal, API key, or technical configuration screen
3. The bot **reads and writes** to their existing system (not a separate silo)
4. They only make **YES/NO decisions** during the entire setup
5. The bot handles **80%+ of routine conversations** and escalates the rest gracefully
