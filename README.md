const fs = require('fs');
const path = require('path');

const readmeContent = `# <p align="center"><img src="https://raw.githubusercontent.com/tandpfun/skill-icons/main/icons/Flask-Dark.svg" width="36" height="36" alt="LabLens" align="center"/> <strong>LabLens</strong> — AI-Powered Medical Report Analyzer</p>

<p align="center">
  <em>Transforming complex diagnostic laboratory sheets into clear, actionable, patient-friendly health intelligence.</em>
</p>

<p align="center">
  <a href="#-key-features"><img src="https://img.shields.io/badge/Next.js-15.0+-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" alt="Next.js" /></a>
  <a href="#-key-features"><img src="https://img.shields.io/badge/React-19.0-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React 19" /></a>
  <a href="#-key-features"><img src="https://img.shields.io/badge/TypeScript-5.0+-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" /></a>
  <a href="#-key-features"><img src="https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS" /></a>
  <a href="#-ai-engine--architecture"><img src="https://img.shields.io/badge/Google_Gemini-2.5_Flash-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white" alt="Google Gemini" /></a>
  <a href="#-database--auth"><img src="https://img.shields.io/badge/Supabase-Database_%26_Auth-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" /></a>
  <a href="#-license"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="License" /></a>
</p>

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-key-features">Key Features</a> •
  <a href="#-preview--ui-walkthrough">UI Preview</a> •
  <a href="#-system-architecture">Architecture</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-project-structure">Folder Structure</a> •
  <a href="#-getting-started">Quick Start</a> •
  <a href="#-environmental-variables">Environment</a> •
  <a href="#-disclaimer">Disclaimer</a>
</p>

---

## 🖼️ Preview & UI Walkthrough

<p align="center">
  <img src="./public/lablens-preview.jpg" alt="LabLens Dashboard Mockup Preview" width="100%" style="border-radius: 12px; box-shadow: 0 10px 30px rgba(0,0,0,0.35); border: 1px solid rgba(255,255,255,0.1);" />
</p>

<p align="center">
  <sub>✨ <i>LabLens interactive dashboard with real-time biometric biomarker classification, visual health risk index, and Gemini-driven diagnostics.</i></sub>
</p>

---

## 🌟 Overview

**LabLens** is an intelligent, privacy-conscious medical report analyzer application. Patients frequently receive laboratory test sheets filled with intricate medical jargon, cryptic unit measurements (e.g., \`x10³/µL\`, \`mg/dL\`, \`mIU/L\`), and overwhelming reference ranges. 

**LabLens** solves this by combining **Multimodal Optical Character Recognition (OCR)** powered by **Google Gemini 2.5 Flash** with clinical translation logic. It extracts individual test metrics, evaluates them against diagnostic normal/high/low reference boundaries, provides human-readable explanations ("What it measures" & "What your result means"), and charts longitudinal biometric health trends over time.

---

## 🚀 Key Features

<table>
  <tr>
    <td width="50%">
      <h3>📄 Multimodal Document Ingestion</h3>
      <p>Seamlessly drag-and-drop or upload medical lab reports in <strong>PDF, PNG, JPG, or JPEG</strong> format (up to 10MB). Handles single and multi-page lab sheets.</p>
    </td>
    <td width="50%">
      <h3>🧠 Gemini 2.5 Flash AI Extraction</h3>
      <p>Utilizes Google's state-of-the-art vision models to extract laboratory names, test dates, patient metadata, biomarkers, units, and reference limits into structured JSON.</p>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <h3>🔬 Dynamic Range Cross-Evaluation</h3>
      <p>Automatically flags test values as <kbd>🟢 Normal</kbd>, <kbd>🔴 High</kbd>, <kbd>🔵 Low</kbd>, or <kbd>🟡 Borderline</kbd> based on the lab's printed reference boundaries.</p>
    </td>
    <td width="50%">
      <h3>💡 Jargon-Free Patient Explanations</h3>
      <p>Translates complex clinical terminology (e.g., Ferritin, MCV, SGPT, eGFR, HbA1c) into clear summaries answering: <em>"What does this measure?"</em> and <em>"What does my result mean?"</em>.</p>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <h3>📈 Longitudinal Health Trends</h3>
      <p>Visualize chronological changes across repeated lab tests using interactive <strong>Recharts</strong> line graphs to spot health patterns before they become clinical issues.</p>
    </td>
    <td width="50%">
      <h3>🤖 Clinical AI Health Assistant</h3>
      <p>Ask contextual questions regarding your uploaded reports with conversational memory and strict safety guardrails preventing unverified self-medication.</p>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <h3>📑 One-Click PDF Clinical Summary</h3>
      <p>Generate clean, exportable PDF summary reports using <code>jspdf</code> to easily share key insights and anomalous markers with your primary care physician.</p>
    </td>
    <td width="50%">
      <h3>⚡ Instant Demo & Production Modes</h3>
      <p>Ships with built-in high-fidelity <strong>Demo Mode</strong> featuring pre-loaded CBC, Vitamin Panels, and Lipid tests. Plug in API keys anytime for live cloud sync.</p>
    </td>
  </tr>
</table>

---

## 📐 System Architecture

```mermaid
flowchart TD
    A[📄 Patient Lab Report PDF / Image] --> B[📤 Upload & Base64 Encoder]
    B --> C{API Key Configured?}
    
    C -- Yes --> D[🧠 Google Gemini 2.5 Flash Multimodal Vision API]
    C -- No (Demo Mode) --> E[⚡ High-Fidelity Mock Diagnostic Engine]
    
    D --> F[📋 Structured JSON Biomarker Extraction]
    E --> F
    
    F --> G[🔬 Reference Range Boundary Classifier]
    G --> H1[🟢 Normal Values]
    G --> H2[🔴 High Flagged Anomalies]
    G --> H3[🔵 Low Flagged Deficiencies]
    
    F --> I[📊 Interactive Health Trends & Recharts]
    F --> J[🤖 Guardrailed AI Health Assistant Chat]
    F --> K[📑 jspdf Instant Summary Export]
    
    H1 & H2 & H3 --> L[💻 Responsive Next.js 15 UI Dashboard]
    I --> L
    J --> L
    K --> L
```

---

## 🛠️ Tech Stack

<div align="center">

| Layer | Technologies |
|---|---|
| **Frontend Framework** | ![Next.js](https://img.shields.io/badge/Next.js_15-black?logo=nextdotjs) ![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB) ![TypeScript](https://img.shields.io/badge/TypeScript_5-007ACC?logo=typescript&logoColor=white) |
| **Styling & Icons** | ![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS_v4-38B2AC?logo=tailwind-css&logoColor=white) ![Lucide](https://img.shields.io/badge/Lucide_Icons-F56565?logo=lucide&logoColor=white) |
| **Artificial Intelligence** | ![Google Gemini](https://img.shields.io/badge/Gemini_2.5_Flash-8E75B2?logo=googlegemini&logoColor=white) ![Generative AI](https://img.shields.io/badge/Google_GenAI_SDK-4285F4?logo=google&logoColor=white) |
| **Charts & Visuals** | ![Recharts](https://img.shields.io/badge/Recharts-22B5BF?logo=chartdotjs&logoColor=white) ![Framer Motion](https://img.shields.io/badge/Framer_Motion-black?logo=framer&logoColor=white) |
| **Backend & Storage** | ![Next Server Actions](https://img.shields.io/badge/Server_Actions-black?logo=nextdotjs) ![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-3ECF8E?logo=supabase&logoColor=white) |
| **Document Export** | ![jsPDF](https://img.shields.io/badge/jsPDF-Client_PDF_Gen-EC1C24?logo=adobeacrobatreader&logoColor=white) |

</div>

---

## 📁 Project Structure

\`\`\`plaintext
REPORT LEBLENS/
├── public/                     # Static assets, SVG icons, and banner preview
│   ├── lablens-preview.jpg     # Dashboard showcase image
│   ├── favicon.ico             # Application favicon
│   └── *.svg                   # System vector assets
├── src/
│   ├── app/                    # Next.js 15 App Router
│   │   ├── dashboard/          # Protected Patient Portal
│   │   │   ├── analyze/        # Multi-file upload, OCR & extraction pipeline
│   │   │   ├── assistant/      # Context-aware AI Chatbot with medical guardrails
│   │   │   ├── profile/        # Patient profile & personal health records
│   │   │   ├── reports/        # Stored report archives & test details
│   │   │   ├── settings/       # Theme, API Keys, and preferences
│   │   │   ├── trends/         # Historical biomarker visualization charts
│   │   │   ├── layout.tsx      # Dashboard navigation layout with sidebar
│   │   │   └── page.tsx        # Dashboard overview metrics & quick actions
│   │   ├── login/              # Authentication & Guest login portal
│   │   ├── register/           # New patient account registration
│   │   ├── globals.css         # Global Tailwind & design token stylesheet
│   │   ├── layout.tsx          # Root layout with ThemeProvider & AuthProvider
│   │   └── page.tsx            # High-conversion Landing Page & feature showcase
│   ├── components/             # Reusable UI component library
│   │   ├── dashboard/          # Header, Sidebar, Metric Cards, Status Badges
│   │   ├── auth-context.tsx    # Session management & user state provider
│   │   ├── theme-provider.tsx  # Dark / Light theme context provider
│   │   └── LabLensLogo.tsx     # Custom SVG brand mark
│   ├── lib/                    # Core business logic & integrations
│   │   ├── db.ts               # Database abstraction (Supabase + Local Fallback)
│   │   ├── gemini.ts           # Gemini 2.5 Flash Vision OCR & AI prompt pipelines
│   │   ├── pdf.ts              # Client-side PDF generation & parser utilities
│   │   ├── types.ts            # TypeScript interfaces & data contracts
│   │   └── utils.ts            # Tailwind clsx/twMerge helper functions
│   └── services/               # Mock dataset & simulated test generators
│       └── mockData.ts         # High-fidelity CBC, Vitamin, & Lipid mock reports
├── .env.example                # Template for environment configuration
├── package.json                # Project dependencies & npm scripts
├── tsconfig.json               # TypeScript compiler options
└── README.md                   # Project documentation
\`\`\`

---

## ⚡ Getting Started

Follow these steps to run LabLens locally on your machine.

### 1️⃣ Prerequisites
Ensure you have the following installed:
- [Node.js](https://nodejs.org/) (version **18.18.0** or higher recommended)
- [npm](https://www.npmjs.com/) or [pnpm](https://pnpm.io/) or [yarn](https://yarnpkg.com/)

### 2️⃣ Clone Repository
\`\`\`bash
git clone https://github.com/your-username/lablens-report-analyzer.git
cd lablens-report-analyzer
\`\`\`

### 3️⃣ Install Dependencies
\`\`\`bash
npm install
\`\`\`

### 4️⃣ Set Up Environment Variables
Create a \`.env.local\` file in the root directory:
\`\`\`bash
cp .env.example .env.local
\`\`\`

Populate the required credentials (or leave blank to automatically run in **Demo Mode**):
\`\`\`env
# Google Gemini AI Key for Vision OCR & Chatbot
GEMINI_API_KEY=your_gemini_api_key_here

# (Optional) Supabase Database & Auth Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
\`\`\`

### 5️⃣ Launch Development Server
\`\`\`bash
npm run dev
\`\`\`

Open [http://localhost:3000](http://localhost:3000) in your browser to view LabLens!

---

## 🧪 Sample Demonstration Reports

LabLens includes comprehensive mock datasets so you can test the system immediately without requiring an active Gemini API key:

| Report Type | Key Biomarkers Included | Range Indicators Tested |
|---|---|---|
| **🩸 Complete Blood Count (CBC)** | Hemoglobin, RBC, WBC, Platelets, Hematocrit, MCV, MCH | High, Low, & Normal |
| **☀️ Vitamin & Mineral Panel** | Vitamin D (25-OH), Vitamin B12, Serum Iron, Ferritin | Low Deficiencies & Normal |
| **🫀 Lipid & Metabolic Panel** | Total Cholesterol, HDL, LDL, Triglycerides, Fasting Blood Glucose | High Risk & Borderline |

---

## 🛡️ Security & Privacy Guardrails

- 🔒 **No PHI Retention in Demo Mode:** All uploads in demo mode execute purely in browser memory without sending patient medical records to third parties.
- 🩺 **Strict AI Medical Guardrails:** The AI prompt design enforces safety disclaimers: it strictly acts as an educational translator, explicitly refusing to write drug prescriptions or diagnose fatal conditions.
- 🔑 **Environment Key Isolation:** Server Actions ensure third-party API credentials (`GEMINI_API_KEY`) are never exposed to the client bundle.

---

## ⚠️ Medical Disclaimer

> [!IMPORTANT]
> **LabLens is designed for educational and informational purposes only.**  
> The data analysis, explanations, and insights generated by this application do **not** constitute medical advice, clinical diagnoses, or treatment plans. Always consult a qualified healthcare professional or licensed physician regarding any medical conditions, abnormal laboratory results, or therapeutic questions.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the Project
2. Create your Feature Branch (\`git checkout -b feature/AmazingFeature\`)
3. Commit your Changes (\`git commit -m 'Add some AmazingFeature'\`)
4. Push to the Branch (\`git push origin feature/AmazingFeature\`)
5. Open a Pull Request

---

## 📜 License

Distributed under the **MIT License**. See \`LICENSE\` for more information.

<p align="center">
  <sub>Built with ❤️ for accessible healthcare and intuitive medical literacy.</sub>
</p>
`;

const targetPath = 'd:/PROJECTS/REPORT LEBLENS/README.md';
fs.writeFileSync(targetPath, readmeContent, 'utf8');
console.log('README.md written successfully to ' + targetPath);
`;

const scriptPath = 'C:/Users/dhruv/.gemini/antigravity-ide/brain/8a89d46a-551e-4601-a0db-656b8845c24f/scratch/write_readme.js';
fs.mkdirSync(path.dirname(scriptPath), { recursive: true });
fs.writeFileSync(scriptPath, readmeContent, 'utf8');
console.log('Scratch script written successfully');
