# Market Classification — WhatsApp-Dependent Businesses in LATAM

## The Universal Pattern

Every business in this classification shares the same manual loop:

```
WhatsApp message arrives
  → Human reads it
    → Human checks some system (spreadsheet, calendar, inventory, paper)
      → Human decides what to respond
        → Human types reply in WhatsApp
          → Human updates their system with new data
```

The differences between categories are: **what system they check** and **what data flows back.** The WhatsApp layer and the agentic intake are identical across all of them.

---

## Category 1: Field Services & Maintenance

### Archetype: Redin (first customer)

**What they do**: Dispatch workers to physical locations to perform services

**Examples**:
- Mantenimiento locativo (Redin's case)
- Plomería, electricidad, fumigación
- Instalación y reparación de aires acondicionados
- Cerrajería
- Servicios de aseo y limpieza
- Jardinería y paisajismo
- Reparación de electrodomésticos
- Instalación de redes y telecomunicaciones
- Construcción y remodelación (contratistas pequeños)

**The WhatsApp loop**:
1. Customer sends message describing a problem (often with photos, voice notes)
2. Owner/coordinator reads and classifies the issue
3. Checks technician availability by city/specialty
4. Assigns technician
5. Coordinates schedule with customer (back-and-forth)
6. Requests site access
7. Updates work order system
8. Follows up post-service

**Typical systems**: AppSheet, Google Sheets, Excel, paper notebooks, WhatsApp groups with technicians

**Integration need**: Work order management, technician database, scheduling

**Bot complexity**: HIGH — multi-step, requires classification, assignment, and coordination

**WhatsApp characteristics**: Heavy use of photos (showing the problem), voice notes (explaining urgency), and back-and-forth scheduling messages

---

## Category 2: Appointment-Based Services (Physical Location)

### Archetype: The neighborhood barbershop / beauty salon

**What they do**: Provide services at a fixed location by appointment

**Examples**:
- Barberías y peluquerías
- Salones de belleza y spa
- Consultorios médicos y odontológicos
- Veterinarias
- Gimnasios y centros de entrenamiento
- Academias (música, idiomas, artes marciales)
- Consultorios psicológicos
- Centros de estética
- Talleres de yoga/pilates
- Ópticas

**The WhatsApp loop**:
1. Customer asks: "¿Tienes disponibilidad para mañana?"
2. Owner checks calendar/agenda
3. Proposes available times
4. Customer confirms
5. Owner writes appointment in their system
6. Day-before reminder (manual WhatsApp message)
7. If cancellation: repeat entire loop

**Typical systems**: Google Calendar, paper agenda, Google Sheets, no system (just WhatsApp history)

**Integration need**: Calendar/booking, client database, reminders

**Bot complexity**: MEDIUM — availability check + booking + reminders

**WhatsApp characteristics**: Short messages, quick confirmations, high frequency of "¿hay disponibilidad?"

---

## Category 3: Orders & Delivery

### Archetype: The restaurant that takes orders via WhatsApp

**What they do**: Sell products and coordinate delivery or pickup

**Examples**:
- Restaurantes y comidas rápidas
- Panaderías y pastelerías
- Tiendas de barrio y minimercados
- Farmacias independientes
- Licoreras y cigarrerías
- Florerías
- Tiendas de mascotas (alimento, accesorios)
- Papelerías
- Ferreterías pequeñas
- Carnicerías y fruterías

**The WhatsApp loop**:
1. Customer asks: "¿Qué tienen hoy?" or sends an order
2. Owner shares menu/catalog (often as an image)
3. Customer selects items
4. Owner confirms availability and price
5. Customer confirms order
6. Owner coordinates delivery or pickup time
7. Payment (transfer, Nequi/Daviplata, cash on delivery)

**Typical systems**: Menu images in WhatsApp, price lists in Sheets, Nequi/Daviplata for payments, delivery apps

**Integration need**: Product catalog, inventory, order tracking, payment confirmation

**Bot complexity**: MEDIUM — catalog display + order taking + delivery coordination

**WhatsApp characteristics**: Catalog images, price inquiries, payment screenshots (Nequi/Daviplata confirmation)

---

## Category 4: Professional Services (Client-Facing, Remote or In-Person)

### Archetype: The independent accountant or lawyer

**What they do**: Provide knowledge-based services, client communication heavy

**Examples**:
- Abogados independientes
- Contadores y asesores tributarios
- Asesores de seguros
- Agentes inmobiliarios
- Corredores de finca raíz
- Consultores empresariales
- Diseñadores gráficos y web freelance
- Fotógrafos
- Tutores y profesores particulares
- Nutricionistas

**The WhatsApp loop**:
1. Potential client asks about services/rates
2. Professional explains services (often repeating the same info)
3. Client asks to schedule consultation
4. Professional checks availability
5. Meeting scheduled
6. Post-meeting: document sharing, follow-up
7. Billing (manual invoice)

**Typical systems**: Google Calendar, Sheets for client tracking, email, sometimes CRM

**Integration need**: FAQ automation, appointment scheduling, document templates, basic CRM

**Bot complexity**: LOW-MEDIUM — FAQ + scheduling (most value from automating repetitive initial questions)

**WhatsApp characteristics**: Long text messages, document sharing (PDFs, images of documents), payment screenshots

---

## Category 5: Logistics & Fleet / Field Workers

### Archetype: The small trucking company or courier service

**What they do**: Coordinate drivers/vehicles to move goods or people

**Examples**:
- Transporte de carga regional
- Empresas de mensajería y domicilios
- Servicios de mudanza
- Taxis y transporte ejecutivo (WhatsApp-based)
- Grúas y asistencia vial
- Distribuidores y mayoristas (route sales)
- Empresas de reciclaje y recolección

**The WhatsApp loop**:
1. Customer requests pickup/delivery
2. Dispatcher checks driver availability and routes
3. Assigns driver
4. Confirms time and price with customer
5. Driver confirms pickup
6. Status updates (picked up, in transit, delivered)
7. Payment coordination

**Typical systems**: GPS tracking apps, Google Sheets, WhatsApp groups with drivers

**Integration need**: Driver/vehicle database, route management, status tracking, pricing

**Bot complexity**: HIGH — multi-actor (customer + dispatcher + driver), real-time status

**WhatsApp characteristics**: Location sharing, real-time status messages, multi-party coordination via groups

---

## Category 6: Rentals & Bookings

### Archetype: The vacation rental owner or event space

**What they do**: Manage reservations for physical spaces

**Examples**:
- Alojamientos y fincas de recreo
- Salones de eventos
- Canchas sintéticas y deportivas
- Coworking spaces
- Estudios de grabación/fotografía
- Alquiler de vehículos
- Alquiler de equipos (sonido, construcción, carpas)

**The WhatsApp loop**:
1. Customer asks availability for dates
2. Owner checks calendar
3. Shares photos, prices, conditions
4. Customer confirms
5. Payment (advance/deposit)
6. Check-in coordination
7. Post-event follow-up

**Typical systems**: Google Calendar, Booking.com (some), Sheets, paper

**Integration need**: Availability calendar, pricing rules, booking confirmations, payment tracking

**Bot complexity**: MEDIUM — availability check + booking + payment coordination

**WhatsApp characteristics**: Photo requests ("¿me mandas fotos?"), price negotiations, deposit confirmations

---

## Cross-Category Analysis

### Automation Value by Category

| Category | Message volume | Repetitiveness | Integration complexity | Bot value |
|----------|---------------|----------------|----------------------|-----------|
| Field Services | Medium-High | Medium | High (work orders) | ⭐⭐⭐⭐⭐ |
| Appointments | Very High | Very High | Low (calendar) | ⭐⭐⭐⭐⭐ |
| Orders & Delivery | High | High | Medium (catalog + inventory) | ⭐⭐⭐⭐ |
| Professional Services | Medium | Very High (FAQ) | Low | ⭐⭐⭐⭐ |
| Logistics | Medium | Medium | High (multi-actor) | ⭐⭐⭐ |
| Rentals | Low-Medium | High | Low-Medium | ⭐⭐⭐ |

### Recommended Launch Order

1. **Appointments** — Highest volume, most repetitive, simplest integration (calendar). Massive number of businesses. Quick win.
2. **Field Services** — Proven (Redin), high value per customer, higher pricing justified
3. **Orders & Delivery** — Large market (restaurants alone are huge), medium complexity
4. **Professional Services** — Easy to build (FAQ + scheduling), good for upselling
5. **Logistics** — Complex but high value per customer
6. **Rentals** — Niche but straightforward

### Common Integration Points (All Categories)

| Component | Tool | Customer action required |
|-----------|------|------------------------|
| Calendar/scheduling | Google Calendar | "Allow access" (OAuth) |
| Data/records | Google Sheets | "Share this sheet with me" |
| Payments | Nequi/Daviplata | Screenshot of confirmation (vision analysis) |
| Location | Google Maps/WhatsApp | Share location pin |
| Documents | Google Drive | Share folder |

### What ALL Categories Share

1. **WhatsApp is already the channel** — no adoption barrier
2. **The owner is the bottleneck** — they personally handle most messages
3. **80% of messages are repetitive** — same questions, same flow, same answers
4. **Existing systems are low-tech** — Sheets, paper, WhatsApp history
5. **The pain is acute at night/weekends** — missing messages = lost business
6. **Payment is informal** — Nequi, Daviplata, cash. Not Stripe.
7. **Voice notes are critical** — LATAM customers prefer audio over typing

### LATAM-Specific Considerations

- **Nequi/Daviplata** are the dominant payment methods for SMBs in Colombia (not Stripe/credit cards)
- **Voice notes** usage is significantly higher than in US/Europe — audio transcription is essential
- **Informal Spanish** varies by country — Colombian, Mexican, Argentine tones are distinct
- **Trust is personal** — customers trust the business owner's WhatsApp number, not a generic business account
- **"Deja el visto"** — leaving a WhatsApp message on "read" without replying is socially unacceptable in business. This creates pressure on owners to reply instantly, which is exactly the pain the bot solves
