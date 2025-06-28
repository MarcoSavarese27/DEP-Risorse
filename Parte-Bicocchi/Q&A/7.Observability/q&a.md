Q1. What is observability, and why is it important in microservices architectures?

A1. 

## 🇬🇧
Observability is the ability to understand what's happening inside a system based on its external outputs. It's crucial in microservices because distributed systems are inherently complex and prone to failure. Observability enables teams to ask arbitrary questions about the system to detect and diagnose issues quickly, improving system reliability and user experience.

Pillars of observability:
- **Sources**: Part of the infrastructure and application layer, such as a microservice, a device, a database, a message queue, or the operating system. They typically must be instrumented to emit signals.
- **Signals**: The data emitted by sources, which can be logs, metrics, or traces.
- **Agents**: Components that collect, preprocess, and route signals to destinations.
- **Destinations**: Where the signals are analyzed and visualized, such as monitoring dashboards or log management systems.
- **Telemetry**: The overall process of collecting, processing, and exporting signals.

## 🇮🇹

L'osservabilità è la capacità di comprendere cosa sta accadendo all'interno di un sistema basandosi sui suoi output esterni. È fondamentale nelle architetture a microservizi perché i sistemi distribuiti sono intrinsecamente complessi e soggetti a guasti. L'osservabilità consente ai team di porre domande arbitrarie sul sistema per rilevare e diagnosticare rapidamente i problemi, migliorando l'affidabilità del sistema e l'esperienza dell'utente.

Pilastri dell'osservabilità:
- **Sources**: Parte dell'infrastruttura e del layer applicativo, come un microservizio, un dispositivo, un database, una coda di messaggi o il sistema operativo. Di solito devono essere strumentati per emettere segnali.
- **Signals**: I dati emessi dalle sources, che possono essere log, metriche o tracce.
- **Agents**: Componenti che raccolgono, preprocessano e instradano i segnali verso le destinazioni.
- **Destinations**: Dove i segnali vengono analizzati e visualizzati, come dashboard di monitoraggio o sistemi di gestione dei log.
- **Telemetry**: Il processo complessivo di raccolta, elaborazione ed esportazione dei segnali.

---
Q2. Explain the distinct role of metrics, logs, and traces in observability.

A2. 

## 🇬🇧
- **Metrics**: Quantitative, time-series data used for monitoring system health and performance (e.g., CPU usage, request counts). Great for trend analysis and alerting.
Types:
    - **Counters**: Incrementing values (e.g., number of requests).
    - **Gauges**: Current values (e.g., memory usage).
    - **Histograms**: Distribution of values (e.g., request latency).
    - **Summaries**: Similar to a histogram but provides precomputed quantiles (e.g., 50th, 90th percentile) instead of bucketed counts. Useful for monitoring latency percentiles.  

- **Logs**: Detailed, timestamped textual records capturing system events. Essential for debugging, auditing and incident investigation.

- **Traces**: Traces track the path of a request as it moves through various services in a distributed system.  
They help visualize how requests propagate across different components. They are helpful in distributed contexts since a request can span multiple services, databases, and external APIs.  
Furthermore, traces are composed of **spans**, which represent a single operation within a service.  
Traces provide visibility into the time taken by each service involved in processing a request.  
In doing so they help with **performance analysis**, **dependency visualization** and **root cause analysis** (useful in identifying bottlenecks in distributed systems).

## 🇮🇹
- **Metrics**: Dati quantitativi in serie temporali utilizzati per monitorare la salute e le prestazioni del sistema (es. utilizzo CPU, conteggio richieste). Ottimi per analisi delle tendenze e allerta.
Tipi:
    - **Counters**: Valori incrementali (es. numero di richieste).
    - **Gauges**: Valori correnti (es. utilizzo memoria).
    - **Histograms**: Distribuzione dei valori (es. latenza richieste).
    - **Summaries**: Simili agli histogram ma forniscono quantili pre-calcolati (es. 50°, 90° percentile) invece di conteggi a bucket. Utili per monitorare i percentili di latenza.
- **Logs**: Registri testuali dettagliati e temporizzati che catturano eventi del sistema. Essenziali per il debug, l'auditing e l'investigazione degli incidenti.
- **Traces**: Tracce che seguono il percorso di una richiesta attraverso vari servizi in un sistema distribuito.
Aiutano a visualizzare come le richieste si propagano tra i diversi componenti. Sono utili nei contesti distribuiti poiché una richiesta può attraversare più servizi, database e API esterne.
Le tracce sono composte da **spans**, che rappresentano un singolo operazione all'interno di un servizio.
Le tracce forniscono visibilità sul tempo impiegato da ciascun servizio coinvolto nell'elaborazione di una richiesta.
Le aiutano con l'**analisi delle prestazioni**, la **visualizzazione delle dipendenze** e l'**root cause analysis** (utili per identificare bottlenecks nei sistemi distribuiti).

---
Q3. Signals, sources, agents, destinations are the components of observability architectures. Describe their role and mutual interactions.

A3. 

## 🇬🇧
- **Sources**: Systems/components that generate telemetry (e.g., microservices, databases).

- **Signals**: The data emitted (logs, metrics, traces).

- **Agents**: Collect, preprocess, and route signals (e.g., OpenTelemetry agent).

- **Destinations**: Where data is analyzed/visualized (e.g., Grafana, Kibana).

Together, they form a pipeline: sources emit → agents process → destinations consume.

## 🇮🇹
- **Sources**: Sistemi/componenti che generano telemetria (es. microservizi, database).
- **Signals**: I dati emessi (log, metriche, tracce).
- **Agents**: Raccolgono, preprocessano e instradano i segnali (es. OpenTelemetry agent).
- **Destinations**: Dove i dati vengono analizzati/visualizzati (es. Grafana, Kibana).
Insieme, formano una pipeline: le sources emettono → gli agents processano → le destinations consumano.

---
Q4. What is defined as telemetry in the context of observability? In this context, describe the role of OpenTelemetry and its advantages over previous approaches.

A4. 
## 🇬🇧

**Telemetry** refers to collecting, processing, and exporting signals (metrics, logs, traces).

OpenTelemetry standardizes telemetry collection across tools and languages. Its advantages:

- Vendor-neutral

- Supports all three signal types (metrics, logs, traces)

- Unified collection pipeline (receivers → processors → exporters)

- Reduces the need for multiple agents

## 🇮🇹
Con la parola **Telemetry** ci si riferisce alla raccolta, elaborazione ed esportazione dei segnali (metriche, log, tracce).
OpenTelemetry standardizza la raccolta della telemetria tra strumenti e linguaggi. I suoi vantaggi:
- Indipendente dal fornitore
- Supporta tutti e tre i tipi di segnali (metrics, logs, traces)
- Pipeline di raccolta unificata (ricevitori → processori → esportatori)
- Riduce la necessità di più agenti

---
Q5. What is the *Business Logic to Instrumentation* ratio? Describe the key observability-related costs.

A5. 
## 🇬🇧
- B2I Ratio = `LOC_after_instrumentation / LOC_before_instrumentation`

    - It measures the code overhead added for observability.

- Costs include:

    
    - **Data Ingestion**: High-frequency metrics, logs, and traces increase ingestion costs, especially in real-time monitoring.
    - **Storage**: Storing high-cardinality and high-volume data can be expensive; retention policies and compression help manage costs.
    - **Compute**: Querying, aggregations, and anomaly detection require processing power, increasing costs in large-scale systems.
    - **Networking**: Cross-region data transfers, streaming analytics, and API requests can lead to significant network costs.
    - **Licensing & Tooling**: Proprietary tools add licensing fees, while open-source solutions require infrastructure and maintenance costs.
    - **Optimization Strategies**: Reducing data collection frequency, filtering unnecessary logs, and optimizing queries can control costs while maintaining visibility.

**Cardinality**: Refers to the number of unique label values (or dimensions) associated with a metric. 
Consider a metric that tracks the latency of API requests. It might have the following labels (dimensions):

- `endpoint`: The specific API endpoint being accessed (e.g., /users, /orders).
- `nodeid`: The id of the server (e.g., node-567, node-343).
- `region`: The geographical location of the server (e.g., us-east-1, eu-west-1).

Each unique combination creates a time series that must be stored and tracked over time:

| **endpoint**  | **nodeid**   | **region**     | **Latency (ms) - T1** | **Latency (ms) - T2** |  
|--------------|-------------|---------------|----------------|----------------|  
| `/users`     | `node-567`  | `us-east-1`   | 120            | 130            |  
| `/users`     | `node-343`  | `us-east-1`   | 135            | 140            |  
| `/orders`    | `node-567`  | `us-east-1`   | 200            | 210            |  
| `/orders`    | `node-343`  | `us-east-1`   | 220            | 225            |  
| `/users`     | `node-567`  | `eu-west-1`   | 180            | 185            |  
| `/orders`    | `node-567`  | `eu-west-1`   | 190            | 195            |  


For example, if you have 1,000 endpoints running on 100 nodes distributed across 3 regions, you could end up collecting 300,000 time series at each time step.


## 🇮🇹
- B2I Ratio = `LOC_after_instrumentation / LOC_before_instrumentation`

    - Misura il sovraccarico di codice aggiunto per l'osservabilità.

- I costi includono:

    - **Data Ingestion**: Metriche, log e tracce ad alta frequenza aumentano i costi di ingestione, specialmente nel monitoraggio in tempo reale.
    - **Storage**: Memorizzare dati ad alta cardinalità e volume può essere costoso; le politiche di retention e la compressione aiutano a gestire i costi.
    - **Compute**: Le query, le aggregazioni e il rilevamento delle anomalie richiedono potenza di elaborazione, aumentando i costi nei sistemi su larga scala.
    - **Networking**: I trasferimenti di dati tra regioni, l'analisi in streaming e le richieste API possono comportare costi significativi di rete.
    - **Licensing & Tooling**: Gli strumenti proprietari aggiungono costi di licenza, mentre le soluzioni open-source richiedono costi di infrastruttura e manutenzione.
    - **Ottimizzazione**: Ridurre la frequenza di raccolta dei dati, filtrare i log non necessari e ottimizzare le query può controllare i costi mantenendo la visibilità.

**Cardinalità**: Si riferisce al numero di valori unici delle etichette (o dimensioni) associate a una metrica.
Considera una metrica che traccia la latenza delle richieste API. Potrebbe avere le seguenti etichette (dimensioni):
- `endpoint`: Il specifico endpoint API accessibile (es. /users, /orders).
- `nodeid`: L'id del server (es. node-567, node-343).
- `region`: La posizione geografica del server (es. us-east-1, eu-west-1).

Ogni combinazione unica crea una serie temporale che deve essere memorizzata e tracciata nel tempo:

| **endpoint**  | **nodeid**   | **region**     | **Latency (ms) - T1** | **Latency (ms) - T2** |  
|--------------|-------------|---------------|----------------|----------------|  
| `/users`     | `node-567`  | `us-east-1`   | 120            | 130            |  
| `/users`     | `node-343`  | `us-east-1`   | 135            | 140            |  
| `/orders`    | `node-567`  | `us-east-1`   | 200            | 210            |  
| `/orders`    | `node-343`  | `us-east-1`   | 220            | 225            |  
| `/users`     | `node-567`  | `eu-west-1`   | 180            | 185            |  
| `/orders`    | `node-567`  | `eu-west-1`   | 190            | 195            |  

Se ad esempio hai 1.000 endpoint in esecuzione su 100 nodi distribuiti su 3 regioni, potresti finire per raccogliere 300.000 serie temporali a ogni quanto di tempo.

---

Q6. Java provides a form of automatic, zero-code instrumentation. Describe it, and its advantages compared to manual alternatives.

A7. 

## 🇬🇧
Zero Code Instrumentation refers to a technique where no code changes are required to instrument an application for monitoring, observability, or performance tracking. This is typically achieved through automatic instrumentation provided by agents (such as OpenTelemetry Java agent) or frameworks.

A Java agent is a special type of Java program that can modify the behavior of Java bytecode at runtime. It leverages the Java Instrumentation API, allowing developers or tools to inject custom behavior into Java classes before they are loaded into memory by the JVM.

```bash
java -javaagent:/path/to/agent.jar -jar app.jar
```
Advantages:

- No need to modify application code

- Lower engineering effort

- Consistent instrumentation across services

## 🇮🇹

La Zero Code Instrumentation si riferisce a una tecnica in cui non sono necessarie modifiche al codice per strumentare un'applicazione per il monitoraggio, l'osservabilità o il tracciamento delle prestazioni. Questo viene tipicamente realizzato attraverso la strumentazione automatica fornita da agenti (come l'agente Java di OpenTelemetry) o framework.
Un agente Java è un tipo speciale di programma Java che può modificare il comportamento del bytecode Java a runtime. Sfrutta l'API di strumentazione Java, consentendo agli sviluppatori o agli strumenti di iniettare comportamenti personalizzati nelle classi Java prima che vengano caricate in memoria dalla JVM.

```bash
java -javaagent:/path/to/agent.jar -jar app.jar
```
Vantaggi:
- Nessuna necessità di modificare il codice dell'applicazione
- Minore sforzo ingegneristico
- Strumentazione coerente tra i servizi

---
Q7. What is Prometheus, and how does it facilitate metrics collection in applications? Which is its primary approach to metrics collection?

A7. 

## 🇬🇧
Prometheus is an open-source monitoring and alerting toolkit widely used for recording real-time metrics and generating alerts.

**Primary approach**: Pull-based model — Prometheus scrapes exposed metrics from application endpoints (e.g., `/actuator/prometheus`).

It facilitates metrics collection by:
- **Service Discovery**: Automatically detects and scrapes targets using mechanisms like Kubernetes, Eureka, static configs (easy replica management).
- **Scraping Metrics**: Collects data from targets (applications, services, systems) via HTTP at set intervals.
- **PromQL**: Provides a powerful query language for aggregating and analyzing metrics.
- **Alerting**: Supports alert rules based on PromQL queries, sending notifications via Alertmanager, which manages, deduplicates and routes alerts.
- **Client Libraries**: Enable applications to expose custom metrics using libraries for Go, Java, Python, and more.
- **Prometheus Server**: Stores scraped metrics in a time-series database, allowing for efficient querying and visualization.
- **Data Storage**: Uses a time-series database to store metrics, allowing for efficient querying and visualization.



## 🇮🇹
Prometheus è un toolkit open-source per il monitoraggio e l'allerta ampiamente utilizzato per registrare metriche in tempo reale e generare avvisi.
**Approccio principale**: Modello pull-based — Prometheus raccoglie metriche esposte dagli endpoint delle applicazioni (es. `/actuator/prometheus`).
Facilita la raccolta di metriche attraverso:
- **Service Discovery**: Rileva e raccoglie automaticamente i target utilizzando meccanismi come Kubernetes, Eureka, configurazioni statiche (gestione facile delle repliche).
- **Scraping Metrics**: Raccoglie dati dai target (applicazioni, servizi, sistemi) tramite HTTP a intervalli impostati.
- **PromQL**: Fornisce un potente linguaggio di query per aggregare e analizzare le metriche.
- **Alerting**: Supporta regole di allerta basate su query PromQL, inviando notifiche tramite Alertmanager, che gestisce, deduplica e instrada gli avvisi.
- **Client Libraries**: Consentono alle applicazioni di esporre metriche personalizzate utilizzando librerie per Go, Java, Python e altro.
- **Prometheus Server**: Memorizza le metriche raccolte in un database time-series, consentendo query e visualizzazione efficienti.
- **Data Storage**: Utilizza un database time-series per memorizzare le metriche, consentendo query e visualizzazione efficienti.

---
Q8. Describe the process of exposing application metrics using both Spring Boot Actuator and the Prometheus endpoint.

A8. 
## 🇬🇧
1. **Spring Boot Actuator** exposes metrics via endpoints.
2. Add dependencies like `micrometer-registry-prometheus` and `spring-boot-starter-actuator`.
3. Configure the endpoint (e.g., `/actuator/prometheus`).
4. Prometheus scrapes metrics from this endpoint at intervals.

Example metric output:

```lua
http_server_requests_seconds_count{method="GET", status="200", uri="/actuator"} 1
```
## 🇮🇹
1. **Spring Boot Actuator** espone metriche tramite endpoint.
2. Aggiungi dipendenze come `micrometer-registry-prometheus` e `spring-boot-starter-actuator`.
3. Configura l'endpoint (es. `/actuator/prometheus`).
4. Prometheus raccoglie le metriche da questo endpoint a intervalli regolari.
Esempio di output metrica:

```lua
http_server_requests_seconds_count{method="GET", status="200", uri="/actuator"} 1
```

---
Q9. What is the ELK stack? Describe its architecture and key components.

A9. 

## 🇬🇧
The ELK stack consists of three components:
- **Elasticsearch**: A distributed search and analytics engine for storing and querying logs.
- **Logstash**: A data processing pipeline that ingests, transforms, and sends logs to Elasticsearch.
- **Kibana**: A visualization tool for exploring and analyzing data stored in Elasticsearch. 

The ELK stack is used for managing and analyzing large volumes of data, particularly logs.

- **Centralized Logging**: The ELK stack allows organizations to centralize logs from multiple sources, making it easier to manage and analyze log data.
- **Flexible Visualization**: Kibana’s visualization tools help users present data in various formats, enabling better decision-making based on insights derived from the data.
- **Scalability**: The distributed nature of Elasticsearch allows the ELK stack to scale with the growth of data, making it suitable for small to large enterprises.


## 🇮🇹
Lo stack ELK è composto da tre componenti:
- **Elasticsearch**: Un motore di ricerca e analisi distribuito per memorizzare e interrogare i log.
- **Logstash**: Una pipeline di elaborazione dei dati che raccoglie, trasforma e invia i log a Elasticsearch.
- **Kibana**: Uno strumento di visualizzazione per esplorare e analizzare i dati memorizzati in Elasticsearch.

Lo stack ELK viene utilizzato per gestire e analizzare grandi volumi di dati, in particolare i log.
- **Centralized Logging**: Lo stack ELK consente alle organizzazioni di centralizzare i log da più fonti, rendendo più facile gestire e analizzare i dati dei log.
- **Visualizzazione Flessibile**: Gli strumenti di visualizzazione di Kibana aiutano gli utenti a presentare i dati in vari formati, consentendo una migliore presa di decisioni basata sulle intuizioni derivate dai dati.
- **Scalabilità**: La natura distribuita di Elasticsearch consente allo stack ELK di scalare con la crescita dei dati, rendendolo adatto per piccole e grandi imprese.

---
Q10. Describe distributed tracing, traces, spans.

A10.
- **Distributed Tracing**: Tracks requests across multiple services.

- **Trace**: A collection of spans representing a request’s journey.

- **Span**: A single operation within a trace, with timing and metadata.

Used for latency analysis, root cause detection, and understanding service dependencies.

## 🇮🇹
- **Distributed Tracing**: Traccia le richieste attraverso più servizi.
- **Trace**: Una raccolta di spans che rappresentano il percorso di una richiesta.
- **Span**: Una singola operazione all'interno di una trace, con tempi e metadati.

Usati per l'analisi della latenza, root cause analysis e la comprensione delle dipendenze dei servizi.

---
Q11. What is OpenTelemetry, how does its collector works?

A11. 
## 🇬🇧

OpenTelemetry is an open standard for collecting metrics, logs, and traces.
The Collector:

- Ingests data via receivers (e.g., Prometheus, Jaeger).

- Processes it (e.g., filtering, batching).

- Exports it to backends via exporters (e.g., Grafana, Elasticsearch).

It unifies telemetry collection and simplifies pipeline managemen


## 🇮🇹
OpenTelemetry è uno standard aperto per la raccolta di metriche, log e tracce.
Il Collector:
- Raccoglie i dati tramite ricevitori (es. Prometheus, Jaeger).
- Li elabora (es. filtraggio, batching).
- Li esporta verso i backend tramite esportatori (es. Grafana, Elasticsearch).

Unifica la raccolta della telemetria e semplifica la gestione della pipeline.

---