UNIVERSAL AI CALL CENTER PLATFORM
Product + Architecture + Engineering Master Prompt
Ты выступаешь как команда уровня Principal/Staff:
•	Product Architect
•	Principal Software Architect
•	AI/LLM Architect
•	Voice AI Engineer
•	Realtime Systems Engineer
•	Backend Engineer
•	Frontend Engineer
•	Data Engineer
•	Security Engineer
•	DevOps/SRE Engineer
•	QA / Test Engineer
•	Product Designer
•	Technical Product Manager
Твоя задача — спроектировать и поэтапно разработать production-grade multi-tenant SaaS-платформу для AI call center.
Важно: не воспринимай эту задачу как создание простого voicebot.
Мы строим платформу, в которой AI является автономным оркестратором бизнес-процессов компании.
________________________________________
1. PRODUCT VISION
Платформа должна позволять компании подключить телефон, корпоративные знания, CRM, базы данных, API и другие системы, после чего создать AI-агента, который способен:
•	принимать входящие звонки;
•	понимать речь в реальном времени;
•	вести естественный разговор;
•	определять намерение клиента;
•	идентифицировать клиента;
•	получать данные из корпоративных систем;
•	использовать knowledge base;
•	задавать уточняющие вопросы;
•	принимать решения в рамках заданных политик;
•	создавать и изменять бизнес-объекты;
•	отправлять сообщения;
•	вызывать API;
•	обрабатывать заявки;
•	работать с CRM;
•	работать с заказами;
•	при необходимости переводить звонок человеку;
•	передавать человеку весь контекст;
•	фиксировать результат разговора;
•	автоматически извлекать договоренности;
•	создавать лиды;
•	создавать задачи;
•	создавать cases/обращения;
•	назначать следующие шаги;
•	анализировать разговор;
•	сохранять transcript и запись;
•	использовать human feedback для дальнейшего улучшения системы.
Первый целевой набор use cases:
1.	Customer Support
2.	Прием заявок
3.	B2B Negotiation
При этом архитектура должна оставаться domain-agnostic и позволять запускать:
•	receptionist;
•	sales agent;
•	support agent;
•	dispatcher;
•	booking agent;
•	collection agent;
•	appointment agent;
•	outbound follow-up agent;
•	B2B negotiation agent;
•	custom enterprise agents.
________________________________________
2. ОСНОВНОЙ ПРИНЦИП
Не создавай отдельный backend под каждого клиента.
Создай единую multi-tenant платформу.
Различия между клиентами должны в основном задаваться через:
•	tenant configuration;
•	agent configuration;
•	policies;
•	knowledge;
•	memory;
•	tools;
•	integrations;
•	permissions;
•	evaluation datasets.
Fork кода под отдельных клиентов должен быть крайней мерой.
________________________________________
3. ПРИМЕР КЛИЕНТА
Архитектура должна быть способна обслуживать крупного enterprise-клиента уровня крупного телеком/банковского/ритейл-бизнеса.
Но не создавай архитектуру специально под одну компанию.
Примерный клиент:
"MTS-like enterprise customer"
Количество операторов потенциально:
•	десятки;
•	сотни;
•	тысячи.
Архитектура должна масштабироваться горизонтально.
________________________________________
4. MAIN KPI
Главный бизнес-KPI:
AI Fully Resolved Call Rate
Определи его как звонок, который:
1.	был полностью обработан AI;
2.	не потребовал human transfer;
3.	пользователь получил корректный результат;
4.	необходимые действия были успешно выполнены;
5.	не осталось нерешенного обязательства, требующего ручного вмешательства.
Отдельно считай:
•	AI-assisted resolution;
•	human transfer rate;
•	unresolved rate;
•	tool success rate;
•	agreement extraction accuracy;
•	hallucination rate;
•	first-contact resolution;
•	average handling time;
•	latency;
•	cost per call;
•	operator intervention;
•	customer outcome;
•	escalation accuracy.
________________________________________
5. ВАЖНЕЙШАЯ КОНЦЕПЦИЯ: INTERACTION OUTCOME
Не делай "lead" единственной сущностью результата звонка.
Создай универсальную модель:
Interaction
→ Intent
→ Request
→ Lead
→ Case
→ Agreement
→ Commitment
→ Decision
→ Task
→ Next Step
→ Appointment
→ Order Action
→ Follow-up
После звонка система создает structured Interaction Outcome.
Например:
{
"interaction_id": "...",
"customer": "...",
"intent": "...",
"summary": "...",
"agreements": [...],
"tasks": [...],
"leads": [...],
"cases": [...],
"decisions": [...],
"next_steps": [...],
"actions_executed": [...],
"unresolved_items": [...],
"confidence": 0.94
}
Разработай production-ready schema сам.
________________________________________
6. AGREEMENT ENGINE
Это одна из ключевых функций продукта.
После каждого разговора система должна определять:
•	запросы;
•	намерения;
•	предложения;
•	решения;
•	обещания;
•	обязательства;
•	договоренности;
•	задачи;
•	сроки;
•	ответственных;
•	следующие шаги;
•	нерешенные вопросы.
Нельзя путать:
REQUEST
с
AGREEMENT
и:
SPECULATION
с
COMMITMENT.
Пример:
"Наверное, мы сможем оплатить в пятницу."
Это не подтвержденная договоренность.
"Мы оплатим в пятницу."
Это commitment.
Разработай semantic classification layer.
Минимальные типы:
•	REQUEST
•	INTENT
•	QUESTION
•	PROPOSAL
•	COUNTER_PROPOSAL
•	ACCEPTANCE
•	REJECTION
•	PROMISE
•	COMMITMENT
•	AGREEMENT
•	DECISION
•	TASK
•	DEADLINE
•	NEXT_STEP
•	UNRESOLVED_ITEM
Каждая важная extracted entity должна иметь:
•	confidence;
•	source;
•	transcript evidence;
•	timestamp_start;
•	timestamp_end;
•	speaker;
•	extraction_version.
AI не должен придумывать договоренности.
При недостаточной уверенности система должна использовать:
•	clarification;
•	human review;
•	unresolved status.
________________________________________
7. HUMAN-IN-THE-LOOP
Агент автономен, но не без ограничений.
Введи уровни автономности:
LEVEL 0
Transcript only
LEVEL 1
Transcript + summary
LEVEL 2
Summary + agreements + tasks
LEVEL 3
Autonomous FAQ/support
LEVEL 4
Autonomous business actions
LEVEL 5
Fully autonomous agent
Каждая компания и каждый agent profile должны иметь собственную настройку autonomy level.
________________________________________
8. POLICY ENGINE
Никогда не помещай всю бизнес-логику в system prompt.
Используй:
•	system prompt;
•	company rules;
•	policy engine;
•	permission engine;
•	tools;
•	deterministic validators.
Пример:
Action: discount
0-5%
→ AI allowed
5-10%
→ AI allowed only under condition X
10-15%
→ human approval
15%
→ forbidden
Policy Engine должен поддерживать:
•	conditions;
•	ranges;
•	actions;
•	permissions;
•	approval requirements;
•	deny rules;
•	allow rules;
•	escalation;
•	priorities;
•	inheritance;
•	tenant overrides.
Администратор должен иметь возможность задавать policies без программирования.
________________________________________
9. TOOL SYSTEM
Создай universal Tool Registry.
Примеры tools:
get_customer
get_customer_orders
get_customer_contracts
create_lead
create_task
create_case
update_customer
update_deal
change_deal_status
create_order
cancel_order
schedule_meeting
send_sms
send_email
send_whatsapp
send_telegram
transfer_call
lookup_product
check_availability
call_external_api
Каждый tool должен иметь:
•	unique ID;
•	name;
•	description;
•	JSON schema;
•	input validation;
•	permission requirement;
•	policy requirement;
•	timeout;
•	retries;
•	idempotency;
•	audit;
•	error handling;
•	version.
LLM никогда не должна напрямую иметь unrestricted database access.
________________________________________
10. DATABASE CONNECTOR
Нужна функция подключения корпоративной БД.
Подход:
B:
AI анализирует schema и предлагает безопасные tools.
Например:
PostgreSQL
→ schema introspection
→ AI suggests tools
→ administrator reviews
→ administrator approves
→ tools become available
Не разрешай:
LLM
→ arbitrary SQL
→ production DB
Нужен controlled query/tool layer.
Поддержи как минимум архитектурно:
•	PostgreSQL;
•	MySQL;
•	REST APIs;
•	GraphQL APIs;
•	generic webhooks.
________________________________________
11. KNOWLEDGE PLATFORM
Поддерживай:
•	PDF;
•	DOCX;
•	TXT;
•	CSV;
•	web pages;
•	internal knowledge;
•	Notion-like sources;
•	Google Drive-like sources;
•	FAQs;
•	manuals;
•	regulations;
•	product documentation;
•	pricing;
•	catalogs.
Pipeline:
Source
→ ingestion
→ parsing
→ normalization
→ metadata
→ chunking
→ embeddings
→ vector index
→ retrieval
→ reranking
→ context construction
→ grounded answer
Поддерживай:
•	versioning;
•	freshness;
•	ACL;
•	metadata;
•	source attribution;
•	document deletion;
•	reindex;
•	incremental updates.
Отдельно защищай систему от:
•	prompt injection внутри документов;
•	malicious instructions inside knowledge;
•	cross-tenant retrieval;
•	data leakage.
________________________________________
12. MEMORY
Раздели:
SHORT-TERM MEMORY
Текущий разговор.
LONG-TERM MEMORY
Долгосрочная информация о клиенте.
Нельзя автоматически записывать каждую фразу клиента в long-term memory.
Memory должна иметь:
•	source;
•	confidence;
•	created_at;
•	updated_at;
•	TTL;
•	permissions;
•	deletion policy;
•	audit.
________________________________________
13. IDENTITY
Основной механизм идентификации клиента:
•	имя/ФИО;
•	номер телефона;
•	customer matching.
Но ФИО не является достаточной аутентификацией для высокорисковых операций.
Создай Identity & Verification Layer.
Уровни:
LOW RISK
name + caller metadata
MEDIUM RISK
additional verification
HIGH RISK
OTP / SMS / explicit human verification / company-specific policy
Каждый tenant самостоятельно определяет verification policy.
________________________________________
14. TELEPHONY
Нужен provider-agnostic telephony layer.
Поддержи архитектурно:
•	cloud telephony providers;
•	SIP;
•	SIP trunk;
•	PBX integration;
•	WebRTC;
•	browser softphone.
Создай abstract interfaces:
TelephonyProvider
STTProvider
TTSProvider
LLMProvider
RealtimeVoiceProvider
Не связывай бизнес-логику напрямую с конкретным provider.
Для MVP можно использовать одного поставщика.
Но замена провайдера не должна требовать переписывания Agent Runtime.
Перед выбором конкретного provider:
ПРОВЕРЬ АКТУАЛЬНУЮ ДОКУМЕНТАЦИЮ И АКТУАЛЬНЫЕ API/SDK.
Не используй устаревшие предположения.
________________________________________
15. REALTIME VOICE
Поддержи:
•	streaming STT;
•	streaming TTS;
•	VAD;
•	interruptions;
•	barge-in;
•	pauses;
•	silence detection;
•	partial transcript;
•	final transcript;
•	latency management;
•	call recording;
•	speaker identification/diarization;
•	DTMF;
•	call transfer.
Определи target latency budget для:
User speech
→ STT
→ Agent reasoning
→ TTS
→ audio playback
Найди оптимальную архитектуру для natural conversation.
________________________________________
16. HUMAN TRANSFER
Если AI не может решить задачу:
AI должен:
1.	определить причину escalation;
2.	определить требуемый отдел;
3.	найти доступного оператора;
4.	передать звонок;
5.	передать context.
Оператор должен увидеть:
•	customer identity;
•	reason;
•	summary;
•	transcript so far;
•	detected agreements;
•	attempted actions;
•	failed actions;
•	relevant CRM information;
•	recommended next step.
Supervisor должен иметь возможность:
•	observe;
•	join;
•	whisper/instruct;
•	takeover;
•	return control to AI.
________________________________________
17. HUMAN OVERRIDE
Добавь отдельный канал:
Human Supervisor
→ instruction
→ Agent
Например:
"Не предлагай скидку. Предложи бесплатную доставку."
AI должен учитывать команду, но:
•	supervisor instruction не должна становиться частью customer-visible transcript;
•	она должна быть отдельно залогирована;
•	должна иметь авторство;
•	должна иметь timestamp;
•	должна подчиняться security/policy rules.
________________________________________
18. FAILURE SEMANTICS
Критическое правило:
AI никогда не должен сообщать пользователю, что действие выполнено, пока tool не вернул подтвержденный успешный результат.
Неправильно:
AI
→ CRM call failed
→ AI: "Я создал заявку."
Правильно:
AI
→ CRM error
→ AI explains limitation
→ retry or fallback
→ create pending case if configured
→ human escalation if necessary.
Продумай:
•	retries;
•	idempotency;
•	compensation;
•	partial failure;
•	transaction boundaries;
•	eventual consistency.
________________________________________
19. CRM / INTEGRATIONS
MVP должен поддерживать:
•	Bitrix24;
•	Telegram;
•	WhatsApp.
Но архитектура должна быть provider-agnostic.
Создай:
Integration
→ Connector
→ Auth
→ Capabilities
→ Tools
→ Mapping
→ Sync
→ Webhooks
→ Retry
→ Audit
Не кодируй все интеграции непосредственно в core business logic.
Нужен Integration Adapter Framework.
________________________________________
20. OUTBOUND
Основной MVP:
INBOUND
Но архитектура должна быть готова к OUTBOUND:
•	callback;
•	follow-up;
•	reminder;
•	sales outreach;
•	order confirmation;
•	appointment reminders.
Не делай outbound core feature MVP, но не блокируй его архитектурно.
________________________________________
21. ADMIN CONFIGURATION
Компания должна настраиваться без разработки.
Company profile:
•	name;
•	industry;
•	timezone;
•	language;
•	working hours;
•	holidays;
•	departments;
•	employees;
•	contacts.
Agent profile:
•	role;
•	objective;
•	communication style;
•	voice;
•	allowed knowledge;
•	allowed tools;
•	memory;
•	autonomy;
•	escalation;
•	policies.
________________________________________
22. VISUAL POLICY BUILDER
Создай UI:
IF
condition
THEN
action
REQUIRE
approval / deny / allow / escalate
Например:
IF customer = existing_customer
AND requested_discount <= 5%
THEN allow_discount
IF requested_discount > 10%
THEN human_approval
IF topic = refund
THEN create_case
Policy Builder должен быть визуальным, но underlying representation должна быть machine-readable и versioned.
________________________________________
23. WEB APPLICATION
MVP — Web application.
Поддерживаемые роли:
AGENT
SUPERVISOR
ADMIN
TENANT_OWNER
PLATFORM_ADMIN
Нужны:
Role-based access control
Tenant-level isolation
Permission-level authorization.
________________________________________
24. AGENT CONSOLE
Главный экран оператора:
•	incoming call;
•	softphone;
•	live transcript;
•	customer profile;
•	CRM;
•	tasks;
•	agreements;
•	AI recommendations;
•	actions;
•	transfer;
•	supervisor assistance.
Оператор не должен переключаться между несколькими системами для основной работы.
________________________________________
25. SUPERVISOR CONSOLE
Live dashboard:
•	active calls;
•	agents;
•	AI agents;
•	queue;
•	AI/human status;
•	escalations;
•	performance;
•	risk flags;
•	interventions.
Supervisor должен иметь:
•	join;
•	takeover;
•	whisper;
•	AI instruction;
•	call monitoring.
________________________________________
26. ADMIN CONSOLE
Разделы:
Dashboard
Calls
Agents
Users
Knowledge
Integrations
Tools
Policies
Memory
Customers
Cases
Leads
Tasks
Evaluation
Analytics
Audit
Settings
Retention
Billing-ready usage.
________________________________________
27. AI OPERATIONS CENTER
Создай концепцию управления AI-агентами.
Например:
Agent:
Support
Status: ONLINE
Version: 2.8
Current Calls: 42
Resolution: 91.2%
Agent:
Sales
Status: ONLINE
Version: 3.1
Current Calls: 13
Resolution: 86.4%
Каждый AI agent должен иметь:
•	version;
•	model;
•	prompt version;
•	knowledge snapshot;
•	tool set;
•	policies;
•	evaluation status;
•	analytics.
________________________________________
28. VERSIONING
Обязательно version everything.
Version:
•	agent;
•	prompt;
•	knowledge;
•	tools;
•	policies;
•	extraction logic;
•	integrations configuration.
Для каждого звонка должно быть возможно установить:
какая версия чего использовалась.
Например:
Call
→ Agent v2.8
→ Prompt v5
→ Knowledge snapshot 2026-09-18
→ Policy v12
→ Tool version 7
→ Model X
Это критично для debugging, audit и evaluation.
________________________________________
29. A/B TESTING
Архитектурно поддержи:
Agent A
vs
Agent B
Сравнение:
•	resolution;
•	escalation;
•	agreement accuracy;
•	latency;
•	cost;
•	customer outcome.
Нужно version-aware experiment framework.
________________________________________
30. EVALUATION
Создай отдельный Evaluation Platform.
Inputs:
•	recorded calls;
•	transcripts;
•	expected outcomes;
•	expected agreements;
•	expected actions.
Metrics:
•	factuality;
•	relevance;
•	groundedness;
•	hallucination;
•	agreement precision;
•	agreement recall;
•	F1;
•	tool correctness;
•	escalation correctness;
•	policy compliance;
•	latency;
•	cost.
Human feedback должен быть first-class data type.
Если оператор исправил AI:
это должно попадать в:
•	feedback dataset;
•	evaluation dataset;
•	analytics;
•	improvement loop.
________________________________________
31. FEEDBACK LOOP
Pipeline:
Real Call
→ AI Result
→ Human Review
→ Correction
→ Evaluation Dataset
→ Error Classification
→ Improvement
→ New Agent Version
→ Evaluation
→ Publish
Не допускай автоматического self-modification production agent без explicit release/versioning.
________________________________________
32. OBSERVABILITY
Для каждого звонка необходимо иметь trace:
Call
→ Telephony
→ STT
→ Context Retrieval
→ Knowledge
→ Prompt
→ LLM
→ Tool
→ Tool Result
→ Agent Response
→ TTS
→ Agreement Extraction
→ Final Outcome
Поддержи:
•	logs;
•	metrics;
•	traces;
•	latency;
•	token cost;
•	provider cost;
•	tool failures;
•	STT failures;
•	TTS failures;
•	hallucination reports.
Используй standard observability practices и OpenTelemetry-compatible architecture.
________________________________________
33. SECURITY
Разработай threat model.
Учитывай:
•	tenant isolation;
•	PII;
•	prompt injection;
•	indirect prompt injection;
•	data exfiltration;
•	tool abuse;
•	privilege escalation;
•	unauthorized CRM changes;
•	secret leakage;
•	malicious documents;
•	malicious integrations;
•	webhook spoofing;
•	replay attacks;
•	auditability.
Никогда не позволяй:
Knowledge content
→ override system security policy.
Также:
Customer input
→ никогда не должен напрямую становиться system instruction.
________________________________________
34. DATA ISOLATION
Гарантируй:
Tenant A
≠
Tenant B
Это касается:
•	database rows;
•	vector search;
•	memory;
•	cache;
•	logs;
•	recordings;
•	transcripts;
•	analytics;
•	evaluation datasets;
•	prompts;
•	embeddings.
Спроектируй isolation strategy и defense-in-depth.
________________________________________
35. DATA RETENTION
Каждая компания самостоятельно задает retention policies.
Например:
audio retention
transcript retention
memory retention
audit retention
agreement retention
Поддержи:
•	TTL;
•	scheduled deletion;
•	hard deletion;
•	legal hold abstraction;
•	deletion audit.
При удалении customer data необходимо учитывать:
•	primary DB;
•	object storage;
•	vector index;
•	cache;
•	derived datasets;
•	memory;
•	backups strategy.
________________________________________
36. DATA MODEL
Спроектируй полноценную ERD.
Минимально рассмотрим:
Tenant
User
Role
Permission
Agent
AgentVersion
PromptVersion
Policy
PolicyVersion
Tool
ToolVersion
Integration
KnowledgeBase
KnowledgeSource
Document
DocumentVersion
Memory
Customer
Organization
Call
CallParticipant
Recording
Transcript
TranscriptSegment
Interaction
Intent
Agreement
Commitment
Task
Case
Lead
Deal
Order
Action
ToolExecution
Escalation
Feedback
EvaluationDataset
EvaluationRun
AuditLog
UsageRecord
Не ограничивайся этим списком.
Предложи лучший вариант.
________________________________________
37. EVENT-DRIVEN ARCHITECTURE
Определи события.
Примеры:
CallStarted
CallAnswered
TranscriptUpdated
IntentDetected
CustomerIdentified
ToolCalled
ToolCompleted
ToolFailed
AgreementDetected
TaskCreated
LeadCreated
CallEscalated
CallTransferred
CallCompleted
EvaluationCompleted
HumanCorrectionCreated
Раздели:
•	synchronous operations;
•	asynchronous jobs;
•	workflows;
•	event-driven flows.
________________________________________
38. RELIABLE WORKFLOWS
Для критичных действий используй надежную workflow/orchestration архитектуру.
Особенно:
•	external API calls;
•	retries;
•	long-running tasks;
•	call post-processing;
•	CRM synchronization;
•	agreement extraction;
•	outbound scheduling.
Сравни варианты вроде:
•	queue-based workers;
•	workflow engine;
•	Temporal-like architecture.
Выбери аргументированно.
________________________________________
39. STORAGE
Рассмотри:
Transactional DB
→ PostgreSQL-class database
Object Storage
→ S3-compatible
Cache
→ Redis-class
Vector Search
→ pgvector for initial phase and/or dedicated vector DB for later scale
Покажи trade-offs.
________________________________________
40. FRONTEND STACK
Рассмотри современный TypeScript-based web stack.
Оцени:
•	React;
•	Next.js;
•	WebRTC;
•	real-time state;
•	WebSocket;
•	accessibility;
•	responsive UI.
Выбери конкретный stack и объясни почему.
________________________________________
41. BACKEND STACK
Сравни варианты:
•	Node.js / TypeScript;
•	Python;
•	hybrid architecture.
Не выбирай стек только по популярности.
Учитывай:
•	realtime;
•	AI ecosystem;
•	maintainability;
•	developer velocity;
•	typed interfaces;
•	enterprise deployment.
Выбери один основной вариант для MVP и объясни trade-offs.
________________________________________
42. AI PROVIDERS
Архитектура должна поддерживать provider abstraction.
Не зашивай конкретную LLM.
Создай:
LLMProvider
STTProvider
TTSProvider
EmbeddingProvider
RealtimeVoiceProvider
Перед реализацией проверь актуальную документацию и текущие API выбранных providers.
Если существующая функция/SDK изменилась — используй актуальный вариант.
________________________________________
43. COST CONTROL
Рассчитай стоимость:
Cost per call =
telephony
+
STT
+
LLM
+
TTS
+
retrieval
+
storage
+
infrastructure
Предусмотри:
•	model routing;
•	caching;
•	context compression;
•	prompt optimization;
•	token budgeting;
•	model fallback;
•	cheap model for classification;
•	premium model for difficult reasoning.
________________________________________
44. DESKTOP CLIENT
Не создавай desktop application в MVP.
Архитектура должна позволять позже создать:
Windows client
macOS client
который использует те же backend APIs и тот же core web experience.
Рассмотри Tauri/Electron/native options и выбери позже на основании требований.
________________________________________
45. PRIVATE CLOUD / ENTERPRISE
Архитектура должна быть способна работать в:
1.	Shared SaaS
2.	Dedicated tenant environment
3.	Private cloud
Не требуй полного on-premise для MVP.
Но abstraction boundaries должны позволять его добавить.
________________________________________
46. COMPLIANCE
Не делай assumptions о юридических требованиях.
Нужно предусмотреть configurable mechanisms для:
•	call recording;
•	consent;
•	transcript storage;
•	PII;
•	deletion;
•	retention;
•	access logs.
Для каждой jurisdiction/industry отдельно укажи legal review requirement.
Перед любыми утверждениями о конкретных законах или требованиях используй актуальные официальные источники.
________________________________________
47. MVP
MVP должен включать:
1.	Multi-tenant foundation
2.	Authentication
3.	RBAC
4.	Web admin
5.	Web agent console
6.	Web softphone
7.	Incoming calls
8.	Real-time transcript
9.	AI conversation
10.	Knowledge base
11.	Bitrix24 integration
12.	Telegram integration
13.	WhatsApp integration
14.	Tool registry
15.	Basic database connector
16.	Policy engine
17.	Human transfer
18.	Supervisor intervention
19.	Call recording
20.	Call summary
21.	Agreement extraction
22.	Lead creation
23.	Task creation
24.	Cases
25.	Analytics
26.	Audit log
27.	Evaluation
28.	Feedback
29.	Agent versioning
30.	Basic A/B experimentation foundation.
________________________________________
48. MVP НЕ ДОЛЖЕН
Не надо пытаться сразу реализовать:
•	все возможные telephony providers;
•	все CRM;
•	все базы данных;
•	all enterprise SSO providers;
•	sophisticated outbound campaigns;
•	full desktop client;
•	self-hosted/on-prem distribution;
•	every possible industry workflow.
Архитектурно поддержать — да.
Полностью реализовать в MVP — нет.
________________________________________
49. ONBOARDING
Создай onboarding wizard:
Create Tenant
→ Company Profile
→ Phone
→ CRM
→ Messaging
→ Knowledge
→ Database/API
→ Tool suggestions
→ Tool approval
→ Policies
→ Agent
→ Test
→ Evaluation
→ Publish
Администратор не должен писать код.
________________________________________
50. TESTING
Нужны:
Unit tests
Integration tests
E2E tests
Contract tests
Load tests
Security tests
AI evaluation tests
Regression tests
Создай отдельные datasets для:
•	successful calls;
•	difficult calls;
•	escalation;
•	hallucination;
•	policy violations;
•	ambiguous agreements;
•	tool failures;
•	malicious input;
•	prompt injection.
________________________________________
51. DEVELOPMENT PROCESS
Не пиши весь проект сразу.
Работай строго по фазам.
PHASE 0
Architecture
PHASE 1
Foundation
PHASE 2
Telephony / Voice
PHASE 3
Agent Runtime
PHASE 4
Knowledge
PHASE 5
Tools / Policies
PHASE 6
CRM / Messaging
PHASE 7
Agreement Intelligence
PHASE 8
Call Center UI
PHASE 9
Supervisor
PHASE 10
Evaluation
PHASE 11
Security / Observability
PHASE 12
Production hardening
________________________________________
52. REPOSITORY
Перед кодированием предложи monorepo или alternative.
Покажи структуру уровня:
apps/
services/
packages/
infra/
docs/
tests/
scripts/
Разделяй:
•	domain;
•	application;
•	infrastructure;
•	providers;
•	integrations;
•	AI;
•	voice;
•	API;
•	frontend;
•	workers.
Не создавай огромные файлы.
________________________________________
53. DOCUMENTATION
Создай:
README
Architecture Decision Records
API docs
Database docs
Security docs
Threat model
Deployment docs
Developer setup
Runbook
Incident response
Tenant isolation docs
Provider integration docs
Agent configuration docs
________________________________________
54. ARCHITECTURE DECISION RECORDS
Обязательно создай ADR для:
•	frontend stack;
•	backend stack;
•	DB;
•	queue/workflow;
•	realtime voice;
•	telephony abstraction;
•	vector search;
•	AI provider abstraction;
•	multi-tenancy;
•	auth;
•	storage;
•	observability;
•	policy engine.
________________________________________
55. IMPORTANT RULE: DO NOT GUESS
Если техническое решение зависит от внешнего provider/API:
ПРОВЕРЬ АКТУАЛЬНУЮ ДОКУМЕНТАЦИЮ.
Не используй устаревшие SDK.
Не выдумывай endpoints.
Не выдумывай authentication flow.
Не выдумывай ограничения API.
Не говори, что функция существует, если это не подтверждено документацией.
Если ты не можешь проверить актуальную документацию, честно обозначь uncertainty и изолируй provider-specific assumption.
________________________________________
56. IMPORTANT RULE: CRITICALLY CHALLENGE THE DESIGN
Не соглашайся автоматически.
Если считаешь, что:
•	архитектура чрезмерно сложная;
•	технология выбрана неправильно;
•	feature слишком рано включен;
•	AI должен быть заменен deterministic logic;
•	нужен другой boundary;
•	риск безопасности слишком высок;
•	cost unacceptable;
скажи об этом.
Предложи альтернативу.
________________________________________
57. FIRST RESPONSE
На первом этапе НЕ ПИШИ КОД.
Создай документ:
Universal AI Call Center Platform — Architecture v1
В нем должны быть:
1.	Executive Summary
2.	Product Vision
3.	Personas
4.	User Stories
5.	Functional Requirements
6.	Non-functional Requirements
7.	System Architecture
8.	C4 Context Diagram
9.	C4 Container Diagram
10.	C4 Component Diagram
11.	Call Flow
12.	Agent Runtime Flow
13.	Knowledge Flow
14.	Agreement Extraction Flow
15.	Human Escalation Flow
16.	Tool Execution Flow
17.	Multi-tenant Architecture
18.	Security Architecture
19.	Data Architecture
20.	ERD
21.	Event Model
22.	API Architecture
23.	Provider Abstraction
24.	Technology Stack
25.	Repository Structure
26.	MVP Scope
27.	Phase Roadmap
28.	Risks
29.	ADRs
30.	Cost Model
31.	Evaluation Strategy
32.	Open assumptions.
________________________________________
58. ARCHITECTURE QUALITY BAR
Для каждого ключевого решения показывай:
Decision
Reason
Alternatives
Trade-offs
Risks
Mitigation
Не используй формулировки вроде:
"Это лучший вариант"
без объяснения критериев.
________________________________________
59. RESULT FORMAT
Ответ должен быть структурированным техническим документом.
Используй:
•	Mermaid diagrams;
•	таблицы;
•	JSON schemas;
•	sequence diagrams;
•	component diagrams;
•	ERD;
•	API examples.
Но не перегружай документ ненужными деталями.
Сначала architecture.
После architecture — implementation plan.
После implementation plan — coding.
________________________________________
60. FINAL DEVELOPMENT RULE
После утверждения Architecture v1:
Для каждого этапа:
1.	Describe goal
2.	Show files to create/change
3.	Implement
4.	Run tests
5.	Run lint/type checks
6.	Run integration checks
7.	Explain what changed
8.	Show remaining risks
9.	Update documentation
10.	Commit-ready state
Нельзя переходить к следующему критическому этапу, если текущий фундамент неработоспособен.
________________________________________
61. SUCCESS CRITERIA
В результате должна получиться не демонстрация voicebot, а основа коммерческой платформы:
A new company
→ creates tenant
→ connects phone
→ uploads knowledge
→ connects CRM
→ connects messaging
→ connects DB/API
→ enables tools
→ configures policies
→ creates AI agent
→ runs evaluation
→ publishes agent
→ receives calls
→ AI processes calls
→ performs actions
→ escalates when required
→ operator receives context
→ system extracts agreements
→ CRM/tasks/cases are updated
→ analytics are generated
→ human feedback improves evaluation
→ new agent versions are released safely.
Главная архитектурная идея:
AI — не просто conversational interface.
AI — это оркестратор, который находится между клиентом и бизнес-системами компании, но всегда действует внутри policy, permissions, tools и deterministic validation.
Начни с:
Architecture v1
и сначала задай только действительно критические вопросы, если что-то еще препятствует архитектурному решению.
Не начинай с кода.
