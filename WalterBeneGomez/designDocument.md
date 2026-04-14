LTI ATS – Product & Architecture Document

⸻

📌 Fase 1: Descripción del producto (enfoque técnico + comparativas)

🧠 ¿Qué es LTI?

LTI es un ATS (Applicant Tracking System) cloud-native, multi-tenant y API-first, diseñado bajo principios de arquitectura orientada a eventos y domain-driven design (DDD). Integra un motor de inteligencia artificial (AI Copilot) transversal que asiste en tiempo real a lo largo de todo el ciclo de reclutamiento.

Características técnicas clave:
	•	Arquitectura: microservicios + event-driven (Kafka/PubSub)
	•	Backend: APIs REST/GraphQL
	•	AI Layer: servicios desacoplados (NLP, ranking, scoring)
	•	Data: modelo híbrido (OLTP + analytics)
	•	Integraciones: webhooks + APIs públicas

⸻

🚀 Valor añadido (desde el punto de vista técnico)
	•	Orquestación automatizada de workflows mediante reglas declarativas (rule engine)
	•	AI embebida en decisiones críticas (ranking, scoring, matching semántico)
	•	Procesamiento de lenguaje natural (NLP) para parsing y enriquecimiento de CV
	•	Pipeline de eventos que permite trazabilidad completa del proceso
	•	Baja latencia en operaciones críticas (UX en tiempo real)

⸻

🏆 Ventajas competitivas (vs mercado)

1. AI nativa vs AI como add-on
	•	LTI: AI integrada en el core (copilot transversal)
	•	Otros ATS: features aisladas de AI (ej. parsing básico)

2. Arquitectura moderna vs legacy
	•	LTI: microservicios + eventos
	•	ATS tradicionales: monolitos (ej. Workday, Taleo)

3. Colaboración en tiempo real
	•	LTI: modelo tipo CRDT/WebSockets (presencia en vivo, edición concurrente)
	•	Otros: colaboración asincrónica basada en comentarios

4. Automatización declarativa
	•	LTI: rule engine configurable (no-code)
	•	Otros: workflows rígidos o hardcodeados

5. Talent CRM integrado
	•	LTI: modelo unificado candidato-histórico
	•	Otros: separación ATS vs CRM (requiere módulos adicionales)

⸻

📊 Comparación con competidores

Feature	LTI	Greenhouse	Lever	Workday
Arquitectura	Microservicios + eventos	Monolito evolucionado	Monolito	Monolito enterprise
AI Copilot	Nativo y transversal	Limitado	Parcial	Muy limitado
Automatización	Avanzada (rule engine)	Media	Media	Baja
Colaboración real-time	Sí	No	Parcial	No
API-first	Sí	Parcial	Sí	Limitado
Time-to-value	Alto (config rápida)	Medio	Medio	Bajo


⸻

⚙️ Funcionalidades principales

1. Gestión de vacantes
	•	Creación y aprobación de posiciones
	•	Templates reutilizables

2. Publicación de empleos
	•	Multiposting automático
	•	Integración con job boards

3. Gestión de candidatos
	•	Base de datos centralizada
	•	Parsing de CV

4. Pipeline de reclutamiento
	•	Flujo configurable
	•	Automatización de estados

5. AI Copilot
	•	Matching automático
	•	Scoring de candidatos
	•	Recomendaciones

6. Entrevistas
	•	Scheduling automático
	•	Integración con calendarios

7. Comunicación
	•	Emails automáticos
	•	Notificaciones internas

8. Analytics
	•	Métricas clave del proceso

⸻

📊 Lean Canvas

Bloque	Contenido
Problema	Procesos manuales, lentos y poco eficientes en recruiting
Segmentos de clientes	Empresas medianas y grandes, equipos de HR
Propuesta de valor	ATS inteligente que automatiza y mejora decisiones
Solución	AI + automatización + colaboración
Canales	Ventas B2B, inbound marketing
Ingresos	Suscripción SaaS
Costes	Infraestructura, desarrollo, IA
Métricas clave	Time-to-hire, conversion rate
Ventaja competitiva	IA integrada + UX superior


⸻

📌 Fase 2: Casos de uso principales (con diagramas técnicos)

⸻

Caso de uso 1: Creación y publicación de vacante

Descripción
El recruiter crea una vacante que es validada y publicada automáticamente en múltiples canales.

Servicios involucrados
	•	Job Service
	•	Approval Service
	•	Publishing Service
	•	Integration Service

Sequence Diagram (Mermaid)

sequenceDiagram
    participant HM as Hiring Manager
    participant R as Recruiter
    participant API as Backend API
    participant JS as Job Service
    participant AS as Approval Service
    participant PS as Publishing Service
    participant IS as Integration Service

    HM->>R: Solicita nueva vacante
    R->>API: Create Job
    API->>JS: Persist Job
    JS->>AS: Request Approval
    AS-->>JS: Approved
    JS->>PS: Trigger Publish Event
    PS->>IS: Publish to Job Boards
    IS-->>PS: Success


⸻

Caso de uso 2: Evaluación de candidatos

Descripción
El sistema recibe aplicaciones, ejecuta screening automático con AI y permite evaluación colaborativa.

Servicios involucrados
	•	Candidate Service
	•	Application Service
	•	AI Service
	•	Notification Service

Sequence Diagram (Mermaid)

sequenceDiagram
    participant C as Candidate
    participant API as Backend API
    participant CS as Candidate Service
    participant AS as Application Service
    participant AI as AI Service
    participant NS as Notification Service

    C->>API: Apply to Job
    API->>CS: Create Candidate Profile
    API->>AS: Create Application
    AS->>AI: Trigger Screening
    AI-->>AS: Score + Ranking
    AS->>NS: Notify Recruiter


⸻

Caso de uso 3: Contratación

Descripción
El candidato seleccionado recibe una oferta y se formaliza la contratación.

Servicios involucrados
	•	Offer Service
	•	Approval Service
	•	Notification Service
	•	HR Integration Service

Sequence Diagram (Mermaid)

sequenceDiagram
    participant R as Recruiter
    participant API as Backend API
    participant OS as Offer Service
    participant AS as Approval Service
    participant NS as Notification Service
    participant HR as HR System

    R->>API: Generate Offer
    API->>OS: Create Offer
    OS->>AS: Request Approval
    AS-->>OS: Approved
    OS->>NS: Send Offer to Candidate
    OS->>HR: Sync Hiring Data


⸻

📌 Fase 3: Modelo de datos (expandido)

🧱 Principios de diseño
	•	Multi-tenant (tenant_id en todas las entidades)
	•	Auditoría (created_at, updated_at, created_by)
	•	Event sourcing (eventos clave del dominio)
	•	Separación clara entre entidades de dominio y proyecciones

⸻

📦 Entidades principales

Tenant
	•	id (UUID)
	•	name (string)
	•	plan (string)
	•	created_at (datetime)

User
	•	id (UUID)
	•	tenant_id (UUID)
	•	name (string)
	•	email (string)
	•	role (enum: recruiter, hiring_manager, admin)
	•	created_at (datetime)

Candidate
	•	id (UUID)
	•	tenant_id (UUID)
	•	name (string)
	•	email (string)
	•	phone (string)
	•	skills (array)
	•	experience_years (int)
	•	source (string)
	•	created_at (datetime)

Job
	•	id (UUID)
	•	tenant_id (UUID)
	•	title (string)
	•	description (text)
	•	status (enum: draft, open, closed)
	•	hiring_manager_id (UUID)
	•	created_at (datetime)

Application
	•	id (UUID)
	•	tenant_id (UUID)
	•	candidate_id (UUID)
	•	job_id (UUID)
	•	status (enum: applied, screening, interview, offer, hired, rejected)
	•	score (float)
	•	created_at (datetime)

Interview
	•	id (UUID)
	•	tenant_id (UUID)
	•	application_id (UUID)
	•	interviewer_id (UUID)
	•	scheduled_at (datetime)
	•	feedback (text)
	•	score (float)

Offer
	•	id (UUID)
	•	tenant_id (UUID)
	•	application_id (UUID)
	•	salary (float)
	•	status (enum: draft, sent, accepted, rejected)
	•	created_at (datetime)

PipelineStage
	•	id (UUID)
	•	tenant_id (UUID)
	•	name (string)
	•	order (int)

ApplicationEvent (event sourcing)
	•	id (UUID)
	•	application_id (UUID)
	•	type (string)
	•	payload (json)
	•	created_at (datetime)

⸻

🔗 Relaciones
	•	Tenant 1:N User
	•	Tenant 1:N Candidate
	•	Tenant 1:N Job
	•	Candidate 1:N Application
	•	Job 1:N Application
	•	Application 1:N Interview
	•	Application 1:1 Offer
	•	Application 1:N ApplicationEvent

⸻

📊 Diagrama ER (Mermaid)

erDiagram
    TENANT ||--o{ USER : has
    TENANT ||--o{ CANDIDATE : has
    TENANT ||--o{ JOB : has

    CANDIDATE ||--o{ APPLICATION : applies
    JOB ||--o{ APPLICATION : receives

    APPLICATION ||--o{ INTERVIEW : has
    APPLICATION ||--|| OFFER : results_in
    APPLICATION ||--o{ APPLICATION_EVENT : generates

    JOB }o--|| USER : managed_by

    USER {
        UUID id
        UUID tenant_id
        string name
        string email
        string role
    }

    CANDIDATE {
        UUID id
        UUID tenant_id
        string name
        string email
        string skills
    }

    JOB {
        UUID id
        UUID tenant_id
        string title
        string status
    }

    APPLICATION {
        UUID id
        UUID candidate_id
        UUID job_id
        string status
        float score
    }

    INTERVIEW {
        UUID id
        UUID application_id
        datetime scheduled_at
        float score
    }

    OFFER {
        UUID id
        UUID application_id
        float salary
        string status
    }


⸻

📌 Fase 4: Diseño del sistema (alto nivel)

🧠 Enfoque arquitectónico

LTI está diseñado bajo los siguientes principios:
	•	Arquitectura de microservicios
	•	Event-driven (pub/sub)
	•	CQRS (separación lectura/escritura)
	•	API-first
	•	Escalabilidad horizontal

⸻

🧩 Componentes principales
	•	Frontend (Web App): React/Next.js, comunicación vía API Gateway
	•	API Gateway: punto de entrada único (auth, routing, rate limiting)
	•	Microservicios:
	•	Candidate Service
	•	Job Service
	•	Application Service
	•	Interview Service
	•	Offer Service
	•	AI Service
	•	Event Bus: Kafka / PubSub
	•	Databases:
	•	OLTP (PostgreSQL)
	•	Search (Elasticsearch)
	•	Cache (Redis)
	•	Integrations Layer: Webhooks + conectores externos

⸻

🔄 Flujo general (event-driven)
	1.	Usuario interactúa con el frontend
	2.	API Gateway enruta la petición
	3.	Microservicio procesa comando
	4.	Se emite evento de dominio
	5.	Otros servicios reaccionan al evento
	6.	Se actualizan read models

⸻

📊 Diagrama de contenedores (C4 - Nivel 2)

flowchart LR
    User((User))
    FE[Frontend App]
    APIGW[API Gateway]

    subgraph Backend
        CS[Candidate Service]
        JS[Job Service]
        AS[Application Service]
        IS[Interview Service]
        OS[Offer Service]
        AIS[AI Service]
    end

    subgraph Data
        DB[(PostgreSQL)]
        ES[(Elasticsearch)]
        REDIS[(Redis)]
    end

    subgraph Infra
        BUS[(Event Bus)]
    end

    User --> FE
    FE --> APIGW
    APIGW --> CS
    APIGW --> JS
    APIGW --> AS
    APIGW --> IS
    APIGW --> OS

    AS --> BUS
    BUS --> AIS
    BUS --> CS

    CS --> DB
    JS --> DB
    AS --> DB

    AIS --> ES
    CS --> ES

    CS --> REDIS


⸻

📌 Fase 5: Diagrama C4 (profundización en Application Service)

🎯 Justificación

El Application Service es el core del ATS, ya que gestiona el ciclo de vida del candidato dentro de una vacante.

⸻

🧩 Responsabilidades
	•	Gestión del estado del candidato (pipeline)
	•	Orquestación de eventos
	•	Integración con AI Service
	•	Validación de reglas de negocio

⸻

📊 Diagrama de componentes (C4 - Nivel 3)

flowchart TB
    API[Application API]
    CMD[Command Handler]
    EVT[Event Publisher]
    SM[State Machine]
    RULES[Rule Engine]
    REPO[Repository]

    API --> CMD
    CMD --> SM
    CMD --> RULES
    CMD --> REPO
    CMD --> EVT

    EVT --> BUS[(Event Bus)]
    REPO --> DB[(PostgreSQL)]


⸻

🔄 Flujo interno
	1.	API recibe comando (ej. mover candidato)
	2.	Command Handler valida
	3.	State Machine verifica transición
	4.	Rule Engine evalúa automatizaciones
	5.	Persistencia en DB
	6.	Publicación de evento

⸻

📌 Ejemplo de evento
	•	CandidateApplied
	•	CandidateScreened
	•	CandidateMovedStage
	•	OfferGenerated

⸻

📌 Fase 6: AI Service (profundización)

🧠 Objetivo

Proveer capacidades de inteligencia para:
	•	Matching candidato-vacante
	•	Scoring predictivo
	•	Parsing y enriquecimiento de CV
	•	Recomendaciones en tiempo real

⸻

🧩 Subcomponentes del AI Service
	•	Ingestion Layer
	•	Recibe eventos desde el Event Bus
	•	Normaliza datos
	•	Feature Engineering
	•	Extracción de skills
	•	Embeddings (NLP)
	•	Model Layer
	•	Matching Model (similaridad candidato-job)
	•	Scoring Model (probabilidad de éxito)
	•	Ranking Model (ordenamiento dinámico)
	•	Inference API
	•	Exposición de resultados en tiempo real
	•	Feedback Loop
	•	Aprende de decisiones humanas

⸻

📊 Diagrama AI Service (C4 Nivel 3)

flowchart LR
    BUS[(Event Bus)] --> ING[Ingestion]
    ING --> FE[Feature Engineering]
    FE --> ML[Model Layer]
    ML --> INF[Inference API]

    INF --> APP[Application Service]
    APP --> FB[Feedback Loop]
    FB --> ML


⸻

🔄 Flujo de datos
	1.	Evento (ej. CandidateApplied) llega al AI Service
	2.	Se generan features (skills, experiencia, embeddings)
	3.	Modelos generan score y ranking
	4.	Resultado disponible vía API
	5.	Feedback humano mejora modelos

⸻

📌 Fase 7: Event Design (Event-Driven Architecture)

🧠 Principios
	•	Eventos inmutables
	•	Versionados
	•	Idempotencia
	•	Esquemas claros (JSON/Avro)

⸻

📦 Eventos de dominio clave

CandidateApplied

{
  "event_type": "CandidateApplied",
  "version": 1,
  "application_id": "uuid",
  "candidate_id": "uuid",
  "job_id": "uuid",
  "timestamp": "datetime"
}

CandidateScored

{
  "event_type": "CandidateScored",
  "version": 1,
  "application_id": "uuid",
  "score": 0.85,
  "model_version": "v1",
  "timestamp": "datetime"
}

CandidateMovedStage

{
  "event_type": "CandidateMovedStage",
  "version": 1,
  "application_id": "uuid",
  "from_stage": "screening",
  "to_stage": "interview",
  "timestamp": "datetime"
}

OfferAccepted

{
  "event_type": "OfferAccepted",
  "version": 1,
  "application_id": "uuid",
  "timestamp": "datetime"
}


⸻

📊 Event Flow (Mermaid)

flowchart LR
    APP[Application Service] -->|CandidateApplied| BUS[(Event Bus)]
    BUS --> AI[AI Service]
    BUS --> NOTIF[Notification Service]

    AI -->|CandidateScored| BUS
    BUS --> APP

    APP -->|CandidateMovedStage| BUS
    BUS --> AI

    APP -->|OfferAccepted| BUS
    BUS --> HR[HR Integration]


⸻

🧩 Topics sugeridos (Kafka/PubSub)
	•	applications.events
	•	candidates.events
	•	jobs.events
	•	ai.events
	•	notifications.events

⸻

⚙️ Consideraciones técnicas
	•	Idempotencia mediante event_id
	•	Versionado de eventos
	•	Retry + DLQ (dead letter queue)
	•	Schema registry

⸻
