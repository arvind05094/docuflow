# DocuFlow

DocuFlow is an intelligent document processing (IDP) and data extraction pipeline. It automates the extraction of structured data from unstructured files (invoices, contracts, receipts, spreadsheets) utilizing AI, background processing, and a configurable per-tenant schema schema, providing confidence scores for every extracted field.

**Links:**

* **Live Demo:** [docu-flow-9p7.pages.dev](https://www.google.com/search?q=https://docu-flow-9p7.pages.dev)
* **API Documentation:** [Render Scalar Docs](https://docuflow-sqa0.onrender.com/scalar/)

*Note: The backend is hosted on a free cloud tier that sleeps when idle; the initial request may take approximately 10 seconds to process.*

---

| | |
|:---:|:---:|
| ![Dashboard](docs/screenshot-dashboard.png) | ![Uploading a document](docs/screenshot-document-upload.png) |
| *Live Document Processing Status* | *Drag-and-Drop Document Upload* |

---

## Core Features

* **AI-Powered Extraction:** Employs configurable LLMs (e.g., Groq) to parse documents against predefined schemas, outputting typed fields and confidence scores.


* **Background Processing Pipeline:** Non-blocking uploads managed by Hangfire background jobs ensure smooth document ingestion and processing across state transitions (Queued → Processing → Completed/Failed).


* **Secure Multi-Tenancy:** Complete tenant data isolation is enforced at the database level using Entity Framework Core global query filters.


* **Automated Webhooks:** The system automatically fires webhooks upon the completion or failure of a document extraction job.


* **Versatile File Support:** Accepts and parses PDF, TXT, CSV, and Excel formats (up to 5MB) using S3-compatible path-style storage.



## Architecture

```mermaid
flowchart TD
    User([User]) --> SWA[React frontend<br/>Cloudflare Pages]
    SWA -->|REST| API[.NET API<br/>Render]
    API --> PG[(PostgreSQL / Neon)]
    API --> S3[(Supabase S3 / Cloudflare R2)]
    API --> Groq[Groq AI<br/>LLM Extraction]
    API --> Hangfire[Hangfire<br/>Background Jobs]
    Hangfire --> Groq

```

## Technology Stack

* **Backend:** .NET 10, ASP.NET Core Web API, Clean Architecture + CQRS (MediatR)


* **Frontend:** React 18, TypeScript, Vite, TanStack Query, Tailwind CSS, React Hook Form + Zod


* **Database & ORM:** PostgreSQL, Entity Framework Core (Npgsql)


* **AI & Processing:** Groq API, Hangfire, PdfPig, EPPlus


* **Infrastructure:** Render (Backend API), Cloudflare Pages (Frontend), Neon (Serverless Database), Supabase S3 / R2 (Storage)



## Engineering Highlights

* **Clean Architecture & CQRS:** The application strictly adheres to Clean Architecture principles, utilizing MediatR for Command Query Responsibility Segregation (CQRS) and executing validation and logging as pipeline behaviors.


* **Database-Level Multi-Tenancy:** Query filtering is integrated directly into the `AppDbContext` to guarantee cross-tenant data security.


* **Path-Style S3 Compatibility:** `AWSSDK.S3` is configured for path-style compatibility (`ForcePathStyle = true`), enabling seamless integration with object storage providers like Supabase and Cloudflare R2.


* **Dynamic AI Configuration:** Extraction models are configurable via environment variables, allowing for dynamic LLM selection.



## Local Development

Prerequisites for local deployment include Docker, the .NET 10 SDK, and Node 18+.

```bash
# Start Postgres via Docker
docker-compose up -d

# Configure and run the backend
cd src/DocuFlow.Api
cp .env.example .env # Fill in your local credentials here
dotnet run 

# Configure and run the frontend
cd frontend
cp .env.example .env # Fill in your local credentials here
npm install
npm run dev

```

## Project Structure Overview

* `DocuFlow.Domain/`: Enterprise logic, entities, enums, and domain events (zero dependencies).


* `DocuFlow.Application/`: CQRS handlers, validation, and repository contracts.


* `DocuFlow.Infrastructure/`: EF Core, Hangfire, S3 Storage, Groq API, and MailKit implementations.


* `DocuFlow.Api/`: HTTP Controllers, JWT middleware, tenant resolution, and Dependency Injection.


* `frontend/`: React client application directory.



## Contact

Arvind Chauhan

* **LinkedIn:** [linkedin.com/in/arvind-chauhan-dev](https://www.linkedin.com/in/arvind-chauhan-dev/)
* **Email:** [arvind05094@gmail.com](mailto:arvind05094@gmail.com)