# LabLens — AI-Powered Medical & Lab Report Analyzer

### *Your Lab Results, Clearly Explained. Scan. Analyze. Understand.*

LabLens is a production-ready, fully responsive, and highly interactive Next.js 15+ SaaS web application that helps users upload and understand their laboratory and medical reports. By leveraging Google Gemini Vision and language models, LabLens automatically extracts test metrics, flags values outside lab reference boundaries, translates complex terminology into simple definitions, generates AI summaries, and provides an interactive educational chatbot.

---

## Key Features

1. **Smart Report Upload & Drag-and-Drop:** Upload scan results, report images (JPG, JPEG, PNG), or text document PDFs up to 10MB.
2. **Intelligent OCR & Data Extraction:** Automatically reads and structures lab names, report dates, test parameters, numeric values, units, and printed reference limits.
3. **Reference Range Cross-Evaluation:** Highlights values that fall *low*, *high*, or *borderline* compared specifically to the reference scale defined on the report.
4. **AI-Powered Summary & Translations:** Translates medical jargon into plain, patient-friendly definitions.
5. **Interactive Conversational AI Assistant:** Ask questions about specific test parameters (e.g., "What does Vitamin D measure?") with strict clinical safety guardrails (no diagnosis, no prescription recommendations).
6. **Health Trends Dashboard:** Tracks and charts historical fluctuations of specific test parameters over time using interactive line charts.
7. **Client-Side PDF Summary Download:** Generate and download a formatted PDF summary of report details, test values, and AI summaries with a single click.
8. **Dual-Mode System (Demo Mode & Real Database Mode):** Works out of the box in high-fidelity **Demo Mode** using mock databases and simulated extractors. Simply add API keys to switch to active **Gemini Vision OCR** and **Supabase Database/Auth** integrations.
9. **Dark Mode Support:** Fully compliant light/dark theme toggle remembering user choices locally.

---

## Tech Stack

- **Frontend:** Next.js 15+, React 19, TypeScript, Tailwind CSS v4, Lucide React (Icons), Framer Motion (Animations).
- **Data Visualization:** Recharts (Summary donut charts, interactive health trends line charts).
- **Backend Services:** Next.js Server Actions.
- **AI Engine:** Google Gemini API SDK (`@google/generative-ai` using `gemini-2.5-flash`).
- **Database & Authentication:** Supabase Client (`@supabase/supabase-js` wrapping PostgreSQL and Auth).
- **PDF Generation:** `jspdf` client-side document layout designer.

---

## Directory Structure

```text
src/
├── app/                        # Next.js App Router Pages
│   ├── layout.tsx              # Root wrapper (Theme provider, Auth context)
│   ├── page.tsx                # Landing Page (Sticky navbar, Hero, Features, timeline, FAQs, footer)
│   ├── login/                  # Login Page (Split-screen visual form)
│   ├── register/               # Register Page (Validation forms)
│   └── dashboard/              # Protected Dashboard Route
│       ├── layout.tsx          # Shared Dashboard layout (Guarded Session, Sidebar + Header)
│       ├── page.tsx            # Dashboard Overview (Health stats cards, Recent reports, Quick actions)
│       ├── analyze/            # Upload report workspace (Drag & Drop, progress tickers, Base64 encoder)
│       ├── assistant/          # AI Chat Assistant (Context selector, prompt chips, chat bubbles)
│       ├── reports/            # My Reports List Page
│       │   └── [id]/           # Detailed Report Analysis (Donut chart, Test tables, details drawers)
│       ├── trends/             # Health Trends page (Dynamic parameter chart, date filters, stats deltas)
│       ├── profile/            # User Profile settings (Personal details, avatar displays)
│       └── settings/           # UI Settings (Theme switchers, Data management, Dev credentials panel)
├── components/                 # Reusable UI components
│   ├── dashboard/              # Sidebar & Header
│   ├── auth-context.tsx        # Session state provider (Supabase Auth / LocalStorage fallback)
│   ├── theme-provider.tsx      # Dark Mode transition context
│   └── LabLensLogo.tsx         # SVG Icon Flask + Lens brand logo
├── lib/                        # Utility & Integration classes
│   ├── db.ts                   # Supabase Database client actions (with local storage mock fallbacks)
│   ├── gemini.ts               # Google Gemini API server actions (with Vision OCR simulated extraction)
│   ├── pdf.ts                  # jsPDF drawing layout builder
│   ├── types.ts                # TypeScript schemas (LabReport, ReportTest, ChatMessage, etc.)
│   └── utils.ts                # Class merger (cn) & formatted date helpers
└── services/                   # Business data layer
    └── mockData.ts             # Prepopulated CBC, Vitamin, and Lipid mockup records
```

---

## Installation & Local Development

### 1. Clone & Install Dependencies
Navigate to the root project directory:
```bash
npm install
```

### 2. Configure Environment Variables
Create a `.env.local` file by copying the template:
```bash
copy .env.example .env.local
```

### 3. Launch Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) to view the application in your browser.

---

## Database Integration (Supabase Setup)

If you wish to deploy the app with real database persistence, create a project on [Supabase](https://supabase.com) and execute the following SQL scripts in the SQL Editor to initialize the tables:

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- 1. Users Profile Table
CREATE TABLE public.users (
  id UUID REFERENCES auth.users ON DELETE CASCADE PRIMARY KEY,
  full_name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  avatar_url TEXT,
  dob DATE,
  gender TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Enable RLS for users
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own profile" ON public.users FOR SELECT USING (auth.uid() = id);
CREATE POLICY "Users can update own profile" ON public.users FOR UPDATE USING (auth.uid() = id);

-- 2. Reports Table
CREATE TABLE public.reports (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES public.users(id) ON DELETE CASCADE NOT NULL,
  report_name TEXT NOT NULL,
  report_type TEXT NOT NULL,
  file_url TEXT,
  report_date DATE NOT NULL,
  lab_name TEXT,
  patient_name TEXT,
  patient_age INT,
  patient_gender TEXT,
  status TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.reports ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage own reports" ON public.reports FOR ALL USING (auth.uid() = user_id);

-- 3. Report Tests Table
CREATE TABLE public.report_tests (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  report_id UUID REFERENCES public.reports(id) ON DELETE CASCADE NOT NULL,
  test_name TEXT NOT NULL,
  value NUMERIC,
  value_text TEXT NOT NULL,
  unit TEXT,
  reference_min NUMERIC,
  reference_max NUMERIC,
  reference_text TEXT,
  status TEXT NOT NULL,
  ai_explanation TEXT,
  what_it_measures TEXT,
  what_result_means TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.report_tests ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own tests" ON public.report_tests FOR ALL USING (
  EXISTS (
    SELECT 1 FROM public.reports 
    WHERE reports.id = report_tests.report_id AND reports.user_id = auth.uid()
  )
);

-- 4. Report Analysis Table
CREATE TABLE public.report_analysis (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  report_id UUID REFERENCES public.reports(id) ON DELETE CASCADE NOT NULL,
  summary TEXT NOT NULL,
  tests_count INT NOT NULL,
  normal_count INT NOT NULL,
  attention_count INT NOT NULL,
  unknown_count INT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.report_analysis ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own summaries" ON public.report_analysis FOR ALL USING (
  EXISTS (
    SELECT 1 FROM public.reports 
    WHERE reports.id = report_analysis.report_id AND reports.user_id = auth.uid()
  )
);

-- 5. Chat Messages Table
CREATE TABLE public.chat_messages (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  user_id UUID REFERENCES public.users(id) ON DELETE CASCADE NOT NULL,
  report_id UUID REFERENCES public.reports(id) ON DELETE CASCADE,
  role TEXT NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.chat_messages ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage own chats" ON public.chat_messages FOR ALL USING (auth.uid() = user_id);
```

Add your Supabase URL and Anon Keys in the `.env.local` file or directly inside the **Settings** page in the dashboard to connect dynamically.

---

## Medical & Safety Disclaimer

**IMPORTANT: LabLens provides AI-generated educational explanations of medical and laboratory reports. It does not provide medical diagnoses, treatment recommendations, or professional medical advice. Always consult a qualified healthcare professional for interpretation of medical results and healthcare decisions.**

All AI prompts, chat flows, and table visualizations strictly follow non-alarmist, objective patterns:
- Values are matched strictly against the boundaries printed on the patient's uploaded slip.
- The assistant is hard-blocked from recommending dosages, medication adjustments, or asserting definite diagnostic statements (e.g. "You have diabetes").
- Safety prompts strongly recommend doctor consults.
