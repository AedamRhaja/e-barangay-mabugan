# E-BARANGAY: Web-Based Barangay Information & Complaint Management System
### With AI-Assisted Classification and Event Tracking
**Developed Exclusively for Barangay Mambugan, City of Antipolo, Rizal**  
*(Community Population: 55,664 based on the 2024 POPCEN by the Philippine Statistics Authority)*

---

## 🏛️ 1. Project Overview & Scope Boundaries

**E-Barangay Mambugan** is a production-ready, full-stack web application designed to bridge Barangay Mambugan residents and local governance. It provides a digital portal for filing grievances, viewing verified community bulletins, participating in local assemblies and events, and tracking complaint resolution with AI-assisted decision support.

### Strict Scope Boundaries
- **Exclusive Jurisdiction:** Single-barangay scope exclusively for **Barangay Mambugan, Antipolo City, Rizal**. Does not generalize into multi-barangay or cross-LGU tenancy.
- **Administrative & Informational Channel Only:** The web application is strictly for non-emergency administrative grievances and public notices. It **does NOT replace** official emergency dispatchers (PNP, BFP, CDRRMO 911). Prominent disclaimers and immediate hotlines are featured on all public portals.
- **Advisory AI Boundary:** The AI triage engine provides advisory decision-support suggestions only. Per Barangay Mambugan governance policy, it **never** makes autonomous final triage dispositions, auto-closes tickets, or bypasses verification by authorized human officials.
- **Statutory Data Privacy:** Resident information is collected and processed in strict compliance with **Republic Act No. 10173 (Philippine Data Privacy Act of 2012)**.

---

## 🚀 2. System Modules & Features

### 📢 Module A: Public Announcement & Event Hub
- **Announcements Feed (`/announcements`):** Chronological feed of official bulletins and advisories published by authorized barangay officials. Includes active/archival filtering, banner photos, and keyword search.
- **Community Event Hub (`/events`):** Interactive activity list and calendar tracking town hall meetings, health missions, Oplan Linis Estero cleanups, and youth sports tournaments with exact venues within Mambugan and status (`Upcoming`, `Ongoing`, `Completed`, `Cancelled`).

### 📝 Module B: Resident Complaint & Suggestion Submission Portal (`/submit`)
- **Structured Submission Form:** Selection of Concern Type (`Complaint`, `Suggestion`, `Community Concern`), minimum 10-character description with live character counter, format hints placed above inputs for mobile keyboard accessibility, and resident identification (Full Name, Contact Number, and Sitio/Address within Mambugan).
- **50MB Multimedia Evidence Upload:** Secure upload handling supporting images (`image/jpeg`, `image/png`, `image/webp`) and video recordings (`video/mp4`, `video/quicktime`) with interactive previews and file removal controls.
- **Data Privacy Consent Checkbox (RA 10173):** Statutory agreement requirement informing residents of data usage boundaries.
- **Instant Reference Tracking Code:** Generates unique tracking code for residents upon submission, along with an advisory preview of the AI's preliminary triage assessment.

### 🔎 Module C: Resident Ticket Tracker (`/track`)
- Direct status tracker allowing citizens to monitor their ticket's lifecycle progression:  
  `New` ➔ `Under Review` ➔ `In Progress` ➔ `Resolved` (or `Dismissed`).
- Displays verified category, priority, and official notes entered by Barangay Mambugan desk officers, while masking sensitive private contact details under RA 10173.

### 🤖 Module D: AI-Assisted Classification & Urgency Engine (`src/lib/ai/classifier.ts`)
- **Bilingual NLP Classifier:** Evaluates Tagalog / Filipino and English community complaints and suggestions.
- **Predefined Categories:**
  1. `Public Safety` (live wires, fallen trees, hazards, crime reports)
  2. `Infrastructure` (potholes/lubak, collapsed canals, busted streetlights, water pipe bursts)
  3. `Sanitation` (uncollected garbage/tambak na basura, clogged drainage, odor, animal carcasses)
  4. `Noise` (late-night videoke/karaoke, modified motorcycle exhausts, curfew violations)
  5. `Maintenance` (overgrown vegetation, signage upkeep, community facility repairs)
- **Urgency & Priority Scoring:** Calculates priority (`Low`, `Medium`, `High`, `Urgent`) and confidence score (0.000 to 1.000) with detailed risk assessments and recommended actions.
- **Hybrid Architecture:** Built-in deterministic Filipino/English NLP classifier + optional Google Gemini API support (`GEMINI_API_KEY`).

### 🛡️ Module E: Administrative Dashboard (`/admin`)
- **Triage Queue & Data Grid:** Real-time indicator for new incoming submissions, multi-parameter filtering (Category, Priority, Status, Concern Type, Search), and sort controls.
- **Triage Decision Console (Modal):** Inspect resident details, full narrative, attached photo/video evidence in a lightbox, and review AI recommendations side-by-side with human-in-the-loop override fields (`verified_category`, `verified_priority`, `official_notes`).
- **Recurring Concern Analytics:** Aggregate metrics to detect recurring problems across Mambugan sitios (Sitio Ruhat, Sitio Kamias, Sitio Bagong Nayon, Francisville, etc.), category frequency distributions, priority breakdowns, and resolution rate KPI.
- **Content Management System (CMS):** Draft, publish, toggle active/archived status for announcements and community events.
- **Authorized Officials Roster:** Overview of registered personnel, positions, and roles (`Admin`, `Official`, `Staff`).

---

## 🗄️ 3. Database Architecture (Supabase PostgreSQL)

The database schema is located in `supabase/schema.sql` and includes:
- **Enum Types:** `submission_type`, `report_category`, `priority_level`, `report_status`, `event_status`, `official_role`.
- **Tables:** `official_profiles`, `announcements`, `events`, `submissions`, `submission_attachments`.
- **Indexes:** Multi-column indexes on `created_at`, `status`, `ai_category`, `verified_category`, `concern_type`.
- **Row Level Security (RLS):** Strict RLS policies enforcing public read for announcements/events, resident insert for submissions and attachments, and authorized official access verified via `is_barangay_official()`.
- **Storage Bucket:** `complaint-evidence` (Private bucket with 50MB limit and image/video mime type restrictions).
- **Realistic Seed Data:** `supabase/seed.sql` with authentic Mambugan community bulletins, upcoming events, and sample complaint records.

---

## 💻 4. Running the Application Locally

### Prerequisites
- Node.js 18+ (tested with Node v24.10.0 and npm 11.6.1)

### Installation
```bash
# Clone or navigate to the project directory
cd "c:\Users\Aedam Rhaja\Desktop\E-Barangay"

# Install dependencies (already completed)
npm install
```

### Environment Configuration
Copy `.env.example` to `.env.local`:
```bash
cp .env.example .env.local
```
Configure your Supabase credentials:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Optional: Google Gemini API Key for hybrid LLM triage enrichment
GEMINI_API_KEY=
```
> **Note:** If Supabase credentials are not yet configured, the system automatically falls back to an intelligent, zero-setup in-memory & browser-synced database pre-populated with realistic Barangay Mambugan seed data.

### Build and Start
```bash
# Build production bundle
npm run build

# Start local server
npm run start
# or start development server
npm run dev
```

Visit `http://localhost:3000` in your browser.

---

## 🔑 5. Official Demo Accounts for Testing

Navigate to `/admin/login` to access the Administrative Dashboard. Quick-access buttons are provided on the login page for:

1. **Hon. Gerardo "Gerry" San Juan**  
   *Role:* `Admin` (Punong Barangay / Barangay Captain)
2. **Kgg. Carmelita "Lita" Reyes**  
   *Role:* `Official` (Kagawad - Committee on Public Safety & Order)
3. **Mark Lester Domingo**  
   *Role:* `Staff` (Triage & Records Desk Officer)

---

## 📞 6. Emergency Contacts (Antipolo City & Mambugan)
- **Antipolo CDRRMO / Rescue:** 911 / (02) 8696-9911
- **Antipolo Police Station (PNP):** (02) 8697-2400 / 0998-598-5716
- **Antipolo Fire Station (BFP):** (02) 8697-2121 / 0923-748-8444
- **Barangay Mambugan Hall:** (02) 8696-2814
