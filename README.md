 ⚖️ NaijaPrecedent Desk
Zero-Hallucination Nigerian Judicial Precedent & Law Report Research Desk

> Built for the **African Technology Forum (ATF) Build Phase** — Legal Tech Track.

🔗 **Live Demo: https://jumbo11o.github.io/naija-precedent-desk/  
🎥 **Video Walkthrough:

---

 1. The Problem
Nigerian legal practitioners, researchers, and law students face two major bottlenecks when using general-purpose AI tools for legal research:
1. Citation Hallucination: Standard LLMs frequently fabricate plausible-sounding Nigerian law report citations or misquote judicial reasoning—exposing lawyers to severe judicial sanctions and wasted court time.
2. Data Privacy & Infrastructure Barriers: Law chambers handle confidential client briefs and often operate with intermittent internet connectivity, making heavy cloud-only legal platforms impractical or risky.

2. Our Solution
NaijaPrecedent Desk is a lightweight, client-side legal research desk built as a standalone browser application (`index.html`). It pairs local document ingestion with a deterministic citation verification guardrail that refuses to display any LLM claim unless every cited source exists in the user's local library and every quotation matches that source verbatim.

 Key Features
Client-Side Document Ingestion: Drag-and-drop parsing for multi-page PDF judgments (via `PDF.js`, preserving page numbers) and **DOCX** briefs/reports (via `Mammoth.js`), plus raw text paste.
* **Automated Nigerian Legal Metadata Tagging:** Regex-based extraction pre-fills **Case Title**, **Citation** (*NWLR, LPELR, SCNJ, FWLR*), **Court**, and **Year** for user review.
* **Page-Aware Verbatim Retrieval:** Searches stored authorities and returns exact passages tagged with PDF page numbers (e.g., `[PDF p. 2]`).
* **Deterministic Anti-Hallucination Guardrail:** Optional LLM synthesis runs through a strict post-generation verification layer before rendering.
* **Zero-Backend Privacy:** All ingested judgments and research sessions remain locally in the user's browser; API endpoints and keys are user-configurable.

---

## 3. System Architecture & Verification Guardrails

Unlike standard chatbots that trust LLM output blindly, this system enforces a **closed-book, verify-before-display pipeline**:

```text
[PDF / DOCX / Text] 
       │
       ▼ (Client-Side Parsing + Regex Metadata Tagger)
[Local Authority Store (Indexed with PDF Page Markers)]
       │
       ├──► 1. Deterministic Verbatim Search (Offline / Zero-API Mode)
       │
       └──► 2. Optional LLM Synthesis (Configurable OpenAI-Compatible Endpoint)
                   │
                   ▼
          [Verification Engine]
           ├── Does cited source ID exist in local library? ──► NO  ──► Withhold Claim
           ├── Does quotation match stored text verbatim?  ──► NO  ──► Reject Quote
           └── Both checks pass?                           ──► YES ──► Render with Page Citation
```

### Epistemic Integrity & Guardrails
* **No Seeded Fictional Authorities:** The library intentionally initializes empty rather than pre-loading synthetic or unverified Nigerian cases.
* **Strict Abstention:** When a query cannot be answered from verified stored authorities, the system abstains rather than falling back on model training memory.
* **Clear Practitioner Boundaries:** Exact-quote verification guarantees textual fidelity to the uploaded document; it does not substitute for human legal verification of whether an authority remains current, un-overruled law.

---

## 4. Known Limitations & Engineering Trade-Offs
* **First-Load CDN Dependency:** `PDF.js` and `Mammoth.js` are loaded via CDN on initial use, requiring an active internet connection on first load.
* **Scanned Image PDFs:** Image-only court rulings without embedded text layers require prior OCR processing before ingestion.
* **DOCX Pagination:** Because `.docx` files do not store fixed page boundaries natively, page markers apply specifically to ingested PDF files.

---

## 5. How to Run & Test Locally (For Evaluators)
1. **Open the App:** Visit the [Live Demo URL](#) or open `index.html` directly in any modern desktop or mobile browser (no build step or server required).
2. **Ingest an Authority:** Drop a Nigerian court judgment `.pdf` or `.docx` (e.g., from [NigerianLII](https://nigerianlii.org)) or paste judgment text, then confirm the auto-detected metadata.
3. **Test Verbatim Search:** Query a legal principle or phrase present in the document to inspect page-numbered passages.
4. **Test Hallucination Rejection (Optional LLM Mode):** Configure an OpenAI-compatible endpoint in settings and ask both an in-scope question (verified quote passes) and an out-of-scope question (unsupported claims are withheld).

---

## 6. Roadmap (Post-ATF Build Phase)
* [ ] **Quote Normalization & BM25 Ranking:** Add line-break de-hyphenation and client-side BM25 scoring (`MiniSearch`) for 100+ page Supreme Court judgments.
* [ ] **Portable Chambers Library (`.json`):** One-click export/import of curated practice-area bundles and `IndexedDB` persistence.
* [ ] **Client-Side OCR:** Optional on-demand `Tesseract.js` integration for scanned historical court rulings.

---

## Team
* **[Your Name]** — `[Role, e.g., Full-Stack & AI Engineering]`
* **[Teammate Name, if applicable]** — `[Role]`
