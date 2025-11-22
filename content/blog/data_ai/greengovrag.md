---
title: GreenGovRAG
type: blog
date: 2025-11-22 04:10:04 +0530
prev: blog/data_ai/
sidebar:
  open: true

---

An Open Source AI Assistant for Australian Environmental & Planning Regulations

*Helping planners, consultants, and researchers navigate complex laws — faster, smarter, and cloud-agnostic.*

---

## Why GreenGovRAG Exists

Environmental and planning regulations in Australia are **fragmented across federal, state, and local governments**.

Planners, consultants, and ESG analysts often spend weeks digging through PDFs, council websites, and regulatory guidelines just to answer basic questions like:

- How do I calculate fugitive emissions from natural gas transmission pipelines?
- (`LGA: City of Adelaide`) What heritage guidelines apply to developments in Adelaide Park Lands?
- How do federal and Queensland planning laws interact for development near Great Barrier Reef?
- (`Topic: Emissions`) How do I report Scope 2 emissions using market-based accounting with renewable energy certificates?

**[GreenGovRAG](https://greengovrag.sundeep.id.au/)** changes that.

It's an **open-source AI-powered assistant** that makes navigating regulations as simple as asking a question — with **citations to official sources** and **location-aware filtering**.

---

<center>
    <img alt="" src="/blog/data_ai/images/greengovrag.png"/>
</center>

---

## What is GreenGovRAG?

GreenGovRAG is an **AI Retrieval-Augmented Generation (RAG) system** tailored for Australian environmental and planning regulations.

**Key Features:**

- **Ask questions in natural language** → get relevant answers with citations
- **Geo-aware search** → filter by state, LGA (Local Government Area), or region
- **Supports ESG reporting** → climate, biodiversity, water, emissions queries
- **Hybrid search** → combines BM25 keyword search with vector similarity
- **Cloud-agnostic deployment** → works on **AWS, Azure, or local Docker**
- **Multi-LLM support** → OpenAI, Anthropic, AWS Bedrock, Azure OpenAI
- **Open Source** → built for contributors, civic-tech, and researchers

**Source code**: [github.com/sdp5/green-gov-rag](https://github.com/sdp5/green-gov-rag)

---

## Example Use Cases

### 1. Environmental Impact Assessment (EIA) Pre-screening

**User:** Environmental consultant, planner, or developer
**Query:** *"Do I need an environmental impact assessment to build a solar farm in regional NSW?"*

**GreenGovRAG Output:**
- Summarizes relevant sections from NSW planning portal and EPBC Act
- Cites official sources with links
- Explains exemption criteria and thresholds

### 2. Native Vegetation Clearing Rules by Region

**User:** Local council officer or landowner
**Query:** *"Can I clear native vegetation near Murray Bridge, SA?"*

**GreenGovRAG Output:**
- Retrieves SA Government vegetation clearance policies
- Uses LGA-level filtering for local rules
- Returns allowed/disallowed activities and buffer zones

### 3. Zoning Regulations and Permitted Uses

**User:** Urban planner or real estate developer
**Query:** *"What are the zoning restrictions for coastal land in Mornington Peninsula, VIC?"*

**GreenGovRAG Output:**
- Retrieves planning scheme overlays from council documents
- Explains permitted uses, height limits, environmental constraints

### 4. Emissions and Energy Standards Compliance

**User:** Sustainability advisor or industrial developer
**Query:** *"Which emissions standards apply to industrial zones in Greater Sydney?"*

**GreenGovRAG Output:**
- Points to NSW EPA and federal requirements
- Suggests offsets or sustainable alternatives
- Links to energy incentive schemes

---

## How It Works (Technical Deep Dive)

GreenGovRAG combines **automated ETL pipelines, RAG, and cloud-native infrastructure**:

### 1. **Data Ingestion & ETL**
- **Sources:** EPBC Act (federal), SA/NSW/VIC legislation, council planning schemes, emissions data (CER/NGER)
- **Pipeline:** Apache Airflow (dev) + GitHub Actions (production)
- **Processing:** PDF parsing, HTML scraping, metadata tagging with LLM
- **Storage:** PostgreSQL with pgvector, cloud storage (S3/Azure Blob)

### 2. **Text Chunking & Embeddings**
- **Chunking:** Semantic chunking with 500-1000 token chunks, 100-200 token overlap
- **Embeddings:** `sentence-transformers/all-MiniLM-L6-v2` (default, configurable)
- **Metadata:** Preserves jurisdiction, document type, LGA, regulatory hierarchy

### 3. **Vector Store & Hybrid Search**
- **Vector Stores:** FAISS (local dev), Qdrant (production)
- **Hybrid Search:** BM25 keyword search + vector similarity
- **Geospatial Filtering:** LGA boundaries (GeoJSON), location NER for Australian places

### 4. **RAG Chain & Response Generation**
- **LLM Providers:** OpenAI (gpt-4o, gpt-4o-mini), Anthropic (Claude), AWS Bedrock, Azure OpenAI
- **Response Enhancement:** Trust scoring, citation verification, regulatory hierarchy
- **API:** FastAPI with OpenAPI docs, rate limiting, CORS support

### 5. **Deployment Options**
- **AWS:** ECS Fargate (backend), EC2 Spot (Qdrant), RDS PostgreSQL, CloudFront (frontend)
- **Azure:** Container Apps, PostgreSQL Flexible Server, Blob Storage
- **Local:** Docker Compose with all services
- **CI/CD:** GitHub Actions for automated deployments

**Tech Stack:**
- Backend: Python 3.12, FastAPI, SQLModel, LangChain
- Database: PostgreSQL with pgvector
- Vector Stores: FAISS, Qdrant
- Frontend: React + TypeScript (WIP)
- Deployment: Docker, AWS CDK, Azure Bicep

---

## Why It Matters

### For Planners
**Saves weeks of manual research** — get answers to compliance questions in seconds instead of days.

### For Consultants
**Supports ESG and compliance reporting** — quickly find relevant regulations for environmental impact statements.

### For Local Councils
**Improves transparency and community engagement** — make regulations more accessible to residents.

### For Researchers
**Builds open datasets** — contribute to civic-tech and evidence-based policy.

In Adelaide and across South Australia, there's growing interest from **urban planning and sustainability groups** who struggle with fragmented regulatory data.

---

## Project Status & Roadmap

**Current (v0.1.0):**
- Multi-source document ingestion (federal, state, local)
- Hybrid BM25 + vector search with LGA filtering
- Multi-LLM support (OpenAI, Anthropic, Bedrock, Azure)
- Production deployments on AWS and Azure
- Automated ETL pipelines (GitHub Actions)
- API with comprehensive docs

**Next Steps:**
- **Geographic coverage expansion** — all states and territories
- **Frontend improvements** — interactive query interface
- **ESG-specific features** — carbon accounting, biodiversity metrics
- **User authentication** — OAuth2, usage tracking
- **Real-time updates** — webhook-based document refresh

**Success Metrics:**
- **Community adoption** → GitHub stars, contributors, PRs
- **Utility** → queries answered correctly, user feedback
- **Coverage** → document sources across all jurisdictions
- **Engagement** → demo usage, conference presentations

---

## How You Can Help

GreenGovRAG is **open-source and community-driven**. Ways to contribute:

### For Developers
- Contribute code, tests, or documentation
- Add new document source plugins
- Improve the frontend UI/UX
- Write integration tests

### For Domain Experts
- Add your state's/council's regulations to the ETL pipeline
- Validate query results and provide feedback
- Suggest new use cases and features

### For Advocates
- Share with planners, ESG analysts, and researchers
- Present at meetups or conferences
- Provide feedback via GitHub issues

**Documentation**: [docs.greengovrag.sundeep.id.au](https://docs.greengovrag.sundeep.id.au/)<br/>
**Discussions**: [github.com/sdp5/green-gov-rag/discussions](https://github.com/sdp5/green-gov-rag/discussions)

---

## Quick Start

### Using Docker (Recommended)
```bash
git clone https://github.com/sdp5/green-gov-rag.git
cd green-gov-rag/deploy/docker
cp .env.example .env
# Edit .env with your API keys
docker-compose up
```

**Access:**
- Backend API: http://localhost:8000/docs
- Frontend: http://localhost:3000 (WIP)

### Local Development
```bash
# Backend
cd backend
pip install -e .[dev]
cp .env.example .env
alembic upgrade head
uvicorn green_gov_rag.api.main:app --reload

# Frontend
cd frontend
npm install
npm run dev
```

See [deployment guide](https://github.com/sdp5/green-gov-rag/tree/main/deploy) for AWS/Azure deployment.

---

## What's Next?

- **Expanding coverage** across all Australian states and territories
- **ESG-specific reporting features** for carbon, biodiversity, water
- **Partnerships** with civic-tech and green-tech organizations
- **User authentication** and multi-tenancy support

---

## Final Word

GreenGovRAG is our step toward making **environmental regulation accessible, transparent, and actionable** — starting in Adelaide and scaling across Australia.

Whether you're a planner navigating complex compliance requirements, a consultant preparing environmental assessments, or a researcher analyzing policy gaps — **GreenGovRAG can help**.

Join us in building it. 🌏
