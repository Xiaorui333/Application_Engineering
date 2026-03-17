# INFO 5100 - Application Engineering and Development

**Xiaorui Liu | NUID: 002087084**

---

## Abstract

This repository documents a progressive journey through full-stack Java application engineering, completed as part of the INFO 5100 coursework. Across six assignments, the work evolves from foundational desktop GUI programming to cloud-deployed REST services integrated with third-party APIs and persistent databases, and ultimately to AI-powered conversational applications with full data persistence.

The overarching theme is **incremental complexity**: each assignment builds on the skills and patterns established in the previous one, mirroring real-world software development where systems grow from simple prototypes into production-grade architectures.

---

## Step-by-Step Workstream

```
Assignment 1 ─── Java Swing Basics & MVC Pattern
      │
      ▼
Assignment 2 ─── In-Memory CRUD with Data Structures
      │
      ▼
Assignment 3 ─── OpenAI API Integration (In-Memory Chat)
      │
      ▼
Assignment 4 ─── Spring Boot REST API + YouTube Data API + Cloud Deployment
      │
      ▼
Assignment 5 ─── Database Persistence (PostgreSQL + jOOQ) + Caching Layer
      │
      ▼
Assignment 6 ─── Persistent AI Chat App (OpenAI + PostgreSQL + jOOQ)
```

| Phase | Focus Area | Key Leap |
|-------|-----------|----------|
| **1 → 2** | Single-record UI → Multi-record directory | Data structures, CRUD operations, JTable |
| **2 → 3** | Local data → External API integration | HTTP clients, OpenAI API, environment variables |
| **3 → 4** | Desktop app → Web service | Spring Boot, REST controllers, Docker, Fly.io |
| **4 → 5** | Stateless API → Stateful with DB caching | PostgreSQL, jOOQ ORM, database-first caching |
| **5 → 6** | Web service persistence → Desktop app persistence | Combining AI + DB in a Swing app, message history |

---

## Projects

### Assignment 1 — Personal Profile Manager

A Java Swing desktop application for creating and viewing a personal profile. Uses the MVC (Model-View-Controller) pattern with a split-pane layout: a navigation panel on the left and a dynamic work area on the right that swaps between Create and View panels.

**What it does:**
- Captures 18+ personal data fields (name, contact, address, identification, professional info)
- Persists data in-memory via a `PersonalProfile` model object
- Provides a form-based Create panel with input validation and a read-only View panel

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| Java SE | Core language, OOP principles, encapsulation via getters/setters |
| Java Swing | JFrame, JPanel, JSplitPane, JTextField, JButton, GroupLayout |
| NetBeans GUI Builder | Visual form designer (.form files) for rapid UI prototyping |
| MVC Architecture | Separation of `model.PersonalProfile` from `ui.*` view classes |
| Apache Ant | Build automation via `build.xml` |

**Impact:** Established the foundational pattern of separating data models from UI components — a principle that carries through every subsequent assignment.

---

### Assignment 2 — Person Directory (Address Book)

A full-featured CRUD application managing a directory of persons, each with personal details and dual addresses (home and work). Employs CardLayout for multi-view navigation and JTable for tabular data display.

**What it does:**
- **Create** new person records with full personal info + home/work addresses
- **Read/List** all persons in a scrollable JTable
- **Search** by first name, last name, or street address
- **Update** existing records after searching
- **Delete** records by search criteria
- Pre-loaded with 5 sample records for immediate demonstration

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| Object-Oriented Design | Composition (`Person` has two `Address` objects), encapsulation |
| Data Structures | `ArrayList<Person>` with `Iterator` for safe deletion |
| Java Swing (Advanced) | CardLayout, JTable, DefaultTableModel, JScrollPane, JOptionPane |
| CRUD Operations | Full create/read/update/delete lifecycle |
| Input Validation | Required-field checks, numeric parsing with error handling |

**Impact:** Introduced the concept of managing collections of domain objects with full lifecycle operations — the in-memory precursor to database-backed persistence introduced later.

---

### Assignment 3 — AI Chat Application (In-Memory)

A desktop chat application that integrates with the OpenAI GPT-3.5 Turbo API to deliver conversational AI responses. Maintains full conversation context in-memory for multi-turn dialogue.

**What it does:**
- Provides a real-time chat interface with a scrollable message area and text input
- Sends user messages to OpenAI's GPT-3.5 Turbo model via the `jvm-openai` client library
- Maintains full conversation history in-memory (`List<ChatMessage>`) to preserve context across turns
- Appends prompt-engineering instructions to optimize response quality

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| Third-Party API Integration | OpenAI Chat Completions API via `jvm-openai:0.11.0` |
| Environment Variables | Secure API key management via `OPENAI_API_KEY` |
| Event-Driven Programming | Swing ActionListener for send button |
| Conversation State Management | Accumulating `ChatMessage` list for multi-turn context |
| Gradle (Kotlin DSL) | Modern build configuration with dependency management |

**Impact:** First exposure to external API integration and secure credential handling — foundational for the REST service and persistent chat projects that follow.

---

### Assignment 4 — YouTube Search REST API Service

A Spring Boot web application that exposes a REST endpoint for searching YouTube videos by topic. Containerized with Docker and deployed to the cloud via Fly.io.

**What it does:**
- Exposes `GET /news?topic={query}` endpoint returning HTML-formatted search results
- Calls YouTube Data API v3 to fetch the top 10 video results for any search term
- Returns video titles as clickable hyperlinks to YouTube
- Deployed as a Docker container on Fly.io (San Jose region)

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| Spring Boot 3.1 | Application bootstrapping, auto-configuration, embedded Tomcat |
| RESTful API Design | `@RestController`, `@GetMapping`, `@RequestParam` |
| Google YouTube Data API v3 | OAuth client setup, search endpoint, snippet parsing |
| Dependency Injection | Constructor-based DI for `YouTubeService` → `YouTubeController` |
| Docker | Multi-stage containerization with `openjdk:17` base image |
| Fly.io Cloud Deployment | `fly.toml` configuration, HTTPS enforcement, auto-scaling |

**Impact:** Transitioned from desktop applications to web services — a pivotal shift introducing REST architecture, cloud deployment, and containerization practices.

---

### Assignment 5 — YouTube Search API with Database Caching

An enhanced version of Assignment 4 that adds a PostgreSQL database layer (hosted on Supabase) for caching YouTube search results. Uses jOOQ as a type-safe SQL query builder to read/write cached data, reducing redundant API calls.

**What it does:**
- First checks PostgreSQL for cached results matching the search topic
- If cached data exists, returns it immediately (skipping the YouTube API call)
- If no cache hit, queries YouTube Data API, returns results, and persists them to the `SearchResults` table
- Fully deployed on Fly.io with remote Supabase PostgreSQL

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| PostgreSQL | Relational database for structured search result storage |
| Supabase | Cloud-hosted PostgreSQL with connection pooling |
| jOOQ 3.18 | Type-safe SQL — `SELECT`, `INSERT`, table/column references from generated code |
| jOOQ Code Generation | Auto-generated Java classes from database schema (`SearchResults` table) |
| Caching Strategy | Database-first lookup to minimize external API calls |
| Spring Boot + JDBC | Raw `DriverManager` connection within a Spring controller |

**Impact:** Introduced database persistence and caching — a critical optimization pattern that reduces latency and API costs in production systems. Demonstrated the full lifecycle: API call → persist → cache-first retrieval.

---

### Assignment 6 — Persistent AI Chat Application

The culmination of the coursework: a desktop chat application combining OpenAI GPT-3.5 Turbo with PostgreSQL persistence. Every message (user and AI) is saved to a `MessageHistory` table via jOOQ, and the full conversation history is restored on startup.

**What it does:**
- Connects to Supabase PostgreSQL on launch and loads prior conversation history
- Displays restored messages in the chat area so users can continue previous conversations
- Sends new messages to OpenAI with the full accumulated context
- Persists every user message and AI response to the `MessageHistory` table in real-time
- Gracefully handles database connection failures with user-friendly error dialogs

**Skillsets Used:**
| Skill | Details |
|-------|---------|
| OpenAI API + jvm-openai | Multi-turn conversation with GPT-3.5 Turbo |
| PostgreSQL + Supabase | Cloud database for durable message storage |
| jOOQ 3.18 | Type-safe `INSERT` and `SELECT` on `MessageHistory` table |
| jOOQ Code Generation | Schema-driven generated classes for compile-time safety |
| Java Pattern Matching | `instanceof` pattern matching for `ChatMessage` subtypes |
| Error Handling | Database failure recovery, API error handling with fallback messages |
| Data Persistence Design | Write-through pattern (save on every message, load on startup) |

**Impact:** Unified all skills from the course — UI design, API integration, database persistence, and ORM usage — into a single cohesive application. Demonstrates a production-ready pattern where conversational state survives application restarts.

---

## Technology Stack Summary

| Category | Technologies |
|----------|-------------|
| **Language** | Java 17 / 21 |
| **Desktop UI** | Java Swing (JFrame, JPanel, JTable, CardLayout, GroupLayout) |
| **Web Framework** | Spring Boot 3.1 |
| **Build Tools** | Gradle (Kotlin DSL & Groovy DSL), Apache Ant |
| **APIs** | OpenAI Chat Completions (GPT-3.5 Turbo), YouTube Data API v3 |
| **Database** | PostgreSQL (Supabase cloud-hosted) |
| **ORM** | jOOQ 3.18 (type-safe SQL, code generation) |
| **Containerization** | Docker (openjdk:17 base) |
| **Cloud Deployment** | Fly.io (auto-scaling, HTTPS, SJC region) |
| **IDE** | NetBeans (Assignments 1–2), IntelliJ IDEA / Cursor (Assignments 3–6) |
| **Version Control** | Git |

---

## Repository Structure

```
Xiaorui_Liu_002087084_assignments/
├── ASSIGNMENT 1/          # Personal Profile Manager (Swing + Ant)
├── Assignment 2/          # Person Directory / Address Book (Swing + Ant)
├── Assignment 3/          # AI Chat App — In-Memory (Swing + OpenAI + Gradle)
├── Assignment_4/          # YouTube REST API (Spring Boot + Docker + Fly.io)
├── Assignment_5/          # YouTube API + DB Caching (Spring Boot + PostgreSQL + jOOQ)
├── Assignment_PersistentChatApp/  # Persistent AI Chat (Swing + OpenAI + PostgreSQL + jOOQ)
└── README.md
```

---

## Key Takeaways

1. **Progressive Complexity** — Each assignment incrementally introduced new architectural layers (UI → data structures → APIs → web services → databases → persistence), building confidence and competence step by step.

2. **Full-Stack Fluency** — The coursework spans the entire stack: desktop GUIs, RESTful web services, third-party API integrations, relational databases, ORMs, containerization, and cloud deployment.

3. **Production Patterns** — Real-world engineering patterns are embedded throughout: MVC separation, dependency injection, caching strategies, write-through persistence, secure credential management, and error recovery.

4. **Tool Diversity** — Hands-on experience with a broad toolchain (Ant → Gradle, NetBeans → IntelliJ, in-memory → PostgreSQL, local → Docker → Fly.io) reflects the adaptability required in professional software engineering.
