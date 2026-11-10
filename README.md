# AegisGST

### The Shield Between Your Business and the Paperwork

AegisGST is an agentic compliance co-pilot for Indian micro-enterprises.

It ingests invoices, GST records, TDS documents, and e-way bills through camera capture, uploads, or CSV imports, extracts information using AI, validates it through a deterministic compliance engine, and surfaces issues as a **Leak Ledger™** ranked by the exact amount of money at risk.

Most compliance software helps businesses file returns.

AegisGST helps businesses discover what is wrong *before* filing.

The AI reads documents, explains notices, and drafts communication.

The compliance engine performs every tax calculation, validation, reconciliation, and rule check deterministically.

> **We automate the paperwork, not the accountability.**

**Status:** Production-grade prototype. Filing is currently simulated. Real GST filing requires GST Suvidha Provider (GSP) licensing.

---

## Why We Built This

India's micro-enterprises spend an enormous amount of time navigating compliance workflows that create little direct business value.

Invoices arrive in WhatsApp groups, PDFs, emails, and paper bundles. Input Tax Credit is lost because suppliers fail to file. Filing deadlines are missed because nobody notices a missing document until the last minute. GST notices arrive long after the underlying issue occurred.

The result is predictable:

- Hours spent collecting and organizing paperwork
- Input Tax Credit left unclaimed
- Penalties and late fees
- Reconciliation disputes
- Constant coordination between businesses and accountants

The existing software ecosystem largely assumes that data is already clean.

In reality, cleaning, validating, reconciling, and understanding compliance data is the hard part.

That is the problem AegisGST solves.

---

## What AegisGST Does

AegisGST operates upstream of filing.

```text
Documents
    ↓
AI Extraction
    ↓
Deterministic Validation
    ↓
Supplier Reconciliation
    ↓
Risk Quantification
    ↓
Drafted Resolution
    ↓
Human Approval
    ↓
Audit Pack
    ↓
Filing
```

Instead of generating generic alerts, every issue is converted into financial exposure.

```text
₹18,000  ITC at Risk
          Supplier invoice missing from GSTR-2B

₹9,400   Late Fee Risk
          GSTR-3B due in 4 days

₹7,100   Potential Duplicate Claim
          Invoice appears twice

₹—        Consignment Risk
           Missing e-way bill document
```

The owner sees:

> ₹34,500 at risk across 6 active issues.

The accountant sees a prioritized queue ranked entirely by rupees.

---

## Two Experiences, One Compliance Record

### Owner App

Built for business owners who care about cash flow, not tax jargon.

Owners can:

- Capture documents
- Track compliance health
- Review risks
- Approve recommended actions
- Monitor money protected

Everything is explained in plain language and localized to the user's preferred language.

### Accountant Workspace

Built for Chartered Accountants and bookkeeping teams managing multiple businesses.

Accountants can:

- Review extraction results
- Resolve mismatches
- Reconcile supplier filings
- Generate audit packs
- Track compliance across clients

A CA with twenty clients remains a single user with twenty organization memberships.

---

## Flagship Feature: Leak Ledger™

The Leak Ledger is the core product.

Most compliance systems answer:

> "What is wrong?"

AegisGST answers:

> "What is wrong, how much money is involved, and what should be fixed first?"

Every entry contains:

- Financial impact
- Compliance category
- Supporting evidence
- Rule citation
- Resolution status
- Audit history

The result is a compliance workflow organized around business impact rather than forms and deadlines.

---

## Flagship Feature: Supplier Trust Score™

Most businesses only see problems affecting themselves.

AegisGST introduces a network-level reliability signal.

```text
Sharma Steel
Trust Score: 42 / 100

4 mismatches detected
3 businesses affected
2 delayed filing events
```

Trust Scores are generated from historical reconciliation outcomes, filing consistency, mismatch frequency, and document quality.

A habitual problem supplier becomes visible long before a penalty appears.

This creates a data network effect unavailable to standalone accounting software.

---

## The AI Boundary

AegisGST follows a strict design principle:

**Language models are never responsible for compliance outcomes.**

| AI Handles | Deterministic Engine Handles |
|------------|-----------------------------|
| OCR & Extraction | GST Calculations |
| Notice Explanation | TDS Calculations |
| Translation | ITC Eligibility |
| Message Drafting | GSTIN Validation |
| Language Generation | Reconciliation |
| User Guidance | Duplicate Detection |

All monetary calculations use `Decimal`, never floating-point arithmetic.

Every compliance flag must reference a versioned rule.

No rule citation. No flag.

---

## Architecture

```text
                      +-------------------+
                      |   Owner Mobile    |
                      +-------------------+
                                |
                                v
                      +-------------------+
                      | FastAPI Backend   |
                      +-------------------+
                                |
          +---------------------+---------------------+
          |                     |                     |
          v                     v                     v
  +---------------+   +----------------+   +----------------+
  | AI Extraction |   | Audit Logging  |   | PostgreSQL     |
  +---------------+   +----------------+   +----------------+
        |
        v
+------------------------+
| Deterministic Engine   |
| GST / TDS / ITC Rules  |
+------------------------+
        |
        v
+------------------------+
| Reconciliation Layer   |
+------------------------+
        |
        v
+------------------------+
| Leak Ledger            |
+------------------------+
        |
        v
+------------------------+
| Audit Pack Generator   |
+------------------------+
        |
        v
+------------------------+
| Simulated Filing       |
+------------------------+
```

### Stack

- **Frontend:** Next.js, App Router, next-intl
- **Backend:** FastAPI, Pydantic, Python 3.12
- **AI:** Gemini with PaddleOCR fallback
- **Database:** PostgreSQL
- **Auth:** Argon2id + session-based authentication
- **Documents:** WeasyPrint
- **Localization:** Noto fonts + locale-aware workflows

---

## Core Data Model

```text
users
organizations
memberships
sessions
invites

documents
invoices
flags
leak_ledger

suppliers
trust_scores

packs
audit_events

rule_library
```

Multi-tenancy is membership-based.

A user gains access through organization memberships rather than user-supplied organization identifiers. Every query is scoped through authenticated membership context.

---

## Security

Security principles are intentionally conservative.

- Argon2id password hashing
- HTTP-only secure session cookies
- Server-side session storage
- Role-based access control
- Organization-level isolation
- Login rate limiting
- Append-only audit logging

Every significant action is recorded and attributable.

---

## Multilingual Strategy

Owner-facing content is localized.

Regulatory references are not.

| Language | Status |
|-----------|----------|
| English | Native Reviewed |
| Hindi | Native Reviewed |
| Kannada | Machine Assisted |
| Tamil | Machine Assisted |
| Marathi | Machine Assisted |
| Telugu | Machine Assisted |

A tax-specific glossary is injected into all agent workflows to maintain terminology consistency.

> We localize understanding, not the legal record.

---

## Scope

### Included

- Camera, upload, and CSV intake
- AI extraction with confidence scoring
- Human review workflows
- GST validation
- TDS validation
- Reconciliation engine
- Leak Ledger™
- Supplier Trust Score™
- Notice triage
- E-way bill checks
- Evidence Vault
- Audit pack generation
- Simulated filing
- Audit logging

### Not Included

- Real GST filing
- WhatsApp integrations
- Inbound email processing
- Native mobile applications
- Autonomous filing agents

---

## Roadmap

**Near Term**

- Native review of all supported languages
- Voice-based document intake
- Rule-change impact analysis

**Mid Term**

- GST Suvidha Provider partnership
- DPDP compliance program
- Production reliability and monitoring

**Long Term**

The long-term vision is to transform verified compliance history into an MSME trust and credit-readiness layer.

The same infrastructure that verifies invoices, reconciles supplier behavior, and tracks compliance risk can eventually support lending decisions, working-capital assessments, and broader business credibility systems.

---

## Vision

Most businesses do not lose money because they lack customers.

They lose money because critical operational systems remain invisible until something breaks.

AegisGST makes compliance visible, measurable, and actionable.

Instead of discovering issues after penalties arrive, businesses know where money is leaking, why it is happening, and what action will recover the most value.

**AegisGST**  
*The Shield Between Your Business and the Paperwork.*
