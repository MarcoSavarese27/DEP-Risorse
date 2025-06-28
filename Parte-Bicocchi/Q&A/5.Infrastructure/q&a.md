Q1. What is service discovery, and why is it important in microservices architecture?

A1.

## 🇬🇧
Service discovery enables services to dynamically register and locate each other without manual configuration. It is essential for horizontal scaling (since we have more service istances) and resiliency (we remove service instances that are not healthy). It allows new service instances to be discovered automatically and removes unhealthy ones from the registry, ensuring high availability and fault tolerance.

## 🇮🇹
La discovery dei servizi consente ai servizi di registrarsi e localizzarsi dinamicamente senza configurazione manuale. È fondamentale per la scalabilità orizzontale (poiché abbiamo più istanze di servizio) e la resilienza (rimuoviamo le istanze non funzionanti). Permette la scoperta automatica di nuove istanze e la rimozione di quelle non funzionanti dal registro, garantendo alta disponibilità e tolleranza ai guasti.

---

Q2. Explain the key differences between client-side and server-side load balancing.

A2.

## 🇬🇧
**Load balancing** is the process of distributing incoming network traffic across multiple servers to ensure no single server becomes overwhelmed. There are two main types:

- **Client-side**: Load balancing logic resides in the client. It fetches the service list and selects one instance (e.g., via round-robin). No single point of failure. Less network latency as the client can directly call the backend servers. Cost reduction, since the absence of dedicated hw/sw. Client more complex (Discovery code mixed with service code). The client needs to have access to the service registry. (Ex. Netflix Eureka)

- **Server-side**: A central load balancer handles traffic and distributes requests. Easier to centralize logic but introduces a potential single point of failure. Furthermore, we have more network latency since the client must go through the load balancer to reach the backend servers.

## 🇮🇹
Il **load balancing** è il processo di distribuzione del traffico di rete in entrata tra più server per garantire che nessun singolo server sia sovraccarico. Ci sono due tipi principali:

* **Client-side**: La logica di bilanciamento del carico risiede nel client. Recupera la lista dei servizi e seleziona un'istanza (es. round-robin). Non c'è un singolo punto di fallimento. Meno latenza di rete poiché il client può chiamare direttamente i server backend. Riduzione dei costi, poiché non è necessario hardware/software dedicato. Il client è più complesso (codice di discovery insieme al codice del servizio). (es. Netflix Eureka).

* **Server-side**: Il bilanciamento è gestito da un nodo centrale. Più semplice ma introduce un potenziale single point of failure. Inoltre, abbiamo più latenza di rete poiché il client deve passare attraverso il load balancer per raggiungere i server backend.

---

Q3. Explain the key differences in using Spring Cloud Gateway or nginx as a server-side load balancer.

A3.

## 🇬🇧

We can implement an API Gateway using different technologies, such as:
- **Spring Cloud Gateway**: Java-based API Gateway built on Spring Boot. Best suited for microservices architectures where dynamic routing, service discovery, and request transformations are needed, particularly in Spring-based environments.
    - **Pros**:
        - Dynamic routing: routes can be dynamically updated.
        - Service Discovery Integration (e.g., with Eureka, kubernetes)
        - Powerful request processing: Supports request/response transformations, filtering, and authentication.
        - Built-in resilience features like circuit breakers, rate limiting, and retries.
    - **Cons**:
        - High resource consumption: Requires JVM-based runtime, which can increase memory and CPU usage.
        - Complexity: More complex to set up compared to a simple nginx configuration.
        - Performance overhead: Efficient, but not as performant as nginx in raw throughput.


- **nginx**: high-performance, lightweight reverse proxy and load balancer. It can be configured as an API Gateway by handling request routing, load balancing, and SSL termination. Best suited fot static routing and in non-Java environments.
    - **Pros**:
        - Performance & Scalability: Highly optimized for speed and low resource usage.
        - Non-Java Environments: Due to its language-agnostic nature, Nginx is a strong choice for non-Java environments or polyglot architectures.
        - Simplicity: Easier to set up for static routing and load balancing.
    - **Cons**:
        - Static routing: Requires manual configuration for routing, which is less flexible than dynamic routing.
        - No built-in service discovery: Must be manually updated when backend services change.
        - Lack of deep API management features: no built-in authentication, circuit breakers, or request transformations.

## 🇮🇹


Possiamo implementare un API Gateway utilizzando diverse tecnologie, come:
- **Spring Cloud Gateway**: API Gateway basato su Java costruito su Spring Boot. Più adatto per architetture a microservizi dove sono necessarie instradamenti dinamici, discovery dei servizi e trasformazioni delle richieste, particolarmente in ambienti basati su Spring.
    - **Pro**:
        - Instradamento dinamico: le rotte possono essere aggiornate dinamicamente.
        - Integrazione con Service Discovery (es. con Eureka, kubernetes)
        - Elaborazione potente delle richieste: supporta trasformazioni di richieste/risposte, filtri e autenticazione.
        - Funzionalità di resilienza integrate come circuit breaker, limitazione della velocità e retry.
    - **Contro**:
        - Consumo elevato di risorse: richiede un runtime basato su JVM, che può aumentare l'uso di memoria e CPU.
        - Complessità: più complesso da configurare rispetto a una semplice configurazione nginx.
        - Sovraccarico di prestazioni: efficiente, ma non performante come nginx in termini di throughput.          
- **nginx**: reverse proxy e load balancer ad alte prestazioni e leggero. Può essere configurato come API Gateway gestendo instradamenti delle richieste, bilanciamento del carico e terminazione SSL. Più adatto per instradamenti statici e in ambienti non Java. 
    - **Pro**:
        - Prestazioni e scalabilità: altamente ottimizzato per velocità e basso utilizzo di risorse.
        - Ambienti non Java: grazie alla sua natura indipendente dal linguaggio, Nginx è una scelta forte per ambienti non Java o architetture poliglotti.
        - Semplicità: più facile da configurare per instradamenti statici e bilanciamento del carico.
    - **Contro**:
        - Instradamento statico: richiede configurazione manuale per l'instradamento, meno flessibile rispetto all'instradamento dinamico.
        - Nessuna discovery dei servizi integrata: deve essere aggiornata manualmente quando i servizi backend cambiano.
        - Mancanza di funzionalità avanzate di gestione delle API: nessuna autenticazione integrata, circuit breaker o trasformazioni delle richieste.

---

Q4. Explain the role of heartbeats (towards a Eureka node) in client-side load balancing.

A4.

## 🇬🇧
Heartbeats allow Eureka clients to maintain their registration by periodically notifying the server they are alive. This ensures that only healthy services remain in the registry and are used by clients for load balancing. It can be done in memory.

## 🇮🇹
Gli heartbeat permettono ai client Eureka di mantenere la registrazione notificando periodicamente che sono attivi. Questo assicura che nel registro restino solo i servizi funzionanti. Può essere fatto in memory.

---

Q5. What is an API Gateway? Is this pattern correlated with Server-side load balancing? How?
A5.

## 🇬🇧
**API Gateway** is a design pattern used to handle incoming requests and route them to appropriate backend services. It serves as a reverse proxy that consolidates requests, performing authentication, routing, load balancing and request transformation. It acts as a single entry point for clients, abstracting the complexity of individual microservices from the client.
Without an API, clients must handle multiple endpoints, there's a difficult API composition, and cross-cutting concerns like authentication and logging must be implemented in each service.

## 🇮🇹

L'**API Gateway** è un design pattern utilizzato per gestire le richieste in entrata e instradarle ai servizi backend appropriati. Funziona come un reverse proxy che consolida le richieste, eseguendo autenticazione, instradamento, bilanciamento del carico e trasformazione delle richieste. Agisce come un punto d'ingresso unico per i client, astrando la complessità dei singoli microservizi dal client.
Senza un API Gateway, i client devono gestire più endpoint, c'è una difficile composizione delle API e le preoccupazioni trasversali come autenticazione e logging devono essere implementate in ogni servizio.

---

Q6. What is a cross-cutting concern and how it could be addressed with a gateway service? Is a shared library a viable alternative?

A6.
## 🇬🇧
Cross-cutting concerns like authentication, logging, and rate limiting affect multiple services. A gateway can centralize them. While it’s possible to use a shared library for embedding these capabilities into all services, the more capabilities we build into a common framework shared across all our services, the more difficult it is to change or add behavior in our common code without having to recompile and redeploy all our services.

## 🇮🇹
I cross-cutting concern (autenticazione, logging, ecc.) toccano più servizi. Un gateway può centralizzarli. Sebbene sia possibile utilizzare una shred library per incorporare queste capacità in tutti i servizi, più funzionalità costruiamo in un framework comune condiviso tra tutti i nostri servizi, più difficile è cambiare o aggiungere comportamenti nel nostro codice comune senza dover ricompilare e ridistribuire tutti i nostri servizi.

---

Q7. Discuss the differences and similarities between the API gateway and BFF pattern.
A7.
## 🇬🇧
- **Similarities**: 
  - Both manage access to backend services.
  - Both can perform routing, authentication, and request transformations.

- **Differences**:
    - **Granularity**: The API Gateway is a single entry point for all clients, while BFFs provide multiple, client-specific entry points.
    - **Client Optimization**: BFFs are focused on optimizing for the needs of specific clients (e.g., mobile vs. desktop), whereas the API Gateway is a more general-purpose solution.
    - **Complexity**: The API Gateway provides a unified control point, while BFFs offer more flexibility at the cost of increased backend complexity.

Problems of BFFs:
- **Increased maintenance**: Managing multiple backends for different clients can introduce more complexity and increase the overhead of maintaining different APIs.
- **Redundant logic**: Certain business logic might need to be replicated across different BFFs, leading to code duplication.

Advantages of BFFs:
- **Client-specific customization**: Each frontend receives data in a format that is most useful to it, leading to better performance and a more efficient user experience.
- **Reduced latency for frontends**: Because the BFF is tailored to specific clients, it can pre-aggregate data from various microservices, reducing the number of round trips between the client and backend.
- **Decoupled Frontend Development**:  Teams can develop and evolve frontend applications independently, without worrying about impacting other clients.


## 🇮🇹
- **Somiglianze**: 
  - Entrambi gestiscono l'accesso ai servizi backend.
  - Entrambi possono eseguire instradamento, autenticazione e trasformazioni delle richieste.
- **Differenze**:
    - **Granularità**: L'API Gateway è un unico punto di accesso per tutti i client, mentre i BFF forniscono più punti di accesso specifici per il client.
    - **Ottimizzazione del client**: I BFF si concentrano sull'ottimizzazione per le esigenze di client specifici (es. mobile vs desktop), mentre l'API Gateway è una soluzione più generale.
    - **Complessità**: L'API Gateway fornisce un punto di controllo unificato, mentre i BFF offrono maggiore flessibilità a costo di una maggiore complessità backend.

Problemi dei BFF:
- **Maggiore manutenzione**: Gestire più backend per diversi client può introdurre più complessità e aumentare il sovraccarico di mantenimento di diverse API.
- **Logica ridondante**: Alcune logiche di business potrebbero dover essere replicate in diversi BFF, portando a duplicazione del codice.

Vantaggi dei BFF:
- **Personalizzazione specifica del client**: Ogni frontend riceve dati nel formato più utile, migliorando le prestazioni e l'esperienza utente.
- **Riduzione della latenza per i frontend**: Poiché il BFF è adattato a client specifici, può pre-aggregare i dati da vari microservizi, riducendo il numero di round trip tra client e backend.
- **Sviluppo Frontend disaccoppiato**: I team possono sviluppare ed evolvere le applicazioni frontend in modo indipendente, senza preoccuparsi di influenzare altri client.



---

Q8. What are the advantages of using centralized configuration management in distributed architectures?

A8.
## 🇬🇧
Centralized configuration management separates the configuration from the code, making possible changing the configuration without recompiling or redeploying the application. 
Many developers turn to property files to manage configuration, but this approach can work only for small applications. It might also represent a security concern, since sensitive data like passwords or API keys are stored in plain text. 

Developers should follow best practices for configuration management, such as:

- **Segregate** configuration from application logic. Configuration should be passed as environment variables or from an external repository when the application starts.
- **Centralize** configuration management to avoid having many repositories with configuration files, which can lead to inconsistencies and difficulties in managing changes.
- **Abstract** configuration management behind a service interface. Instead of accessing configuration files directly, applications should use REST-based JSON services to retrieve configuration data.
- **Harden** configuration in order to make the solution higly available and redundant. 

## 🇮🇹

La gestione centralizzata della configurazione separa la configurazione dal codice, rendendo possibile modificare la configurazione senza ricompilare o ridistribuire l'applicazione.
Molti sviluppatori si affidano ai file di proprietà per gestire la configurazione, ma questo approccio può funzionare solo per piccole applicazioni. Può anche rappresentare un problema di sicurezza, poiché dati sensibili come password o chiavi API sono memorizzati in testo semplice.
Gli sviluppatori dovrebbero seguire le best practice per la gestione della configurazione, come:
- **Separare** la configurazione dalla logica dell'applicazione. La configurazione dovrebbe essere passata come variabili d'ambiente o da un repository esterno all'avvio dell'applicazione.
- **Centralizzare** la gestione della configurazione per evitare di avere molti repository con file di configurazione, che possono portare a incoerenze e difficoltà nella gestione dei cambiamenti.
- **Astrarre** la gestione della configurazione dietro un'interfaccia di servizio. Invece di accedere direttamente ai file di configurazione, le applicazioni dovrebbero utilizzare servizi REST basati su JSON per recuperare i dati di configurazione.
- **Rafforzare** la configurazione per rendere la soluzione altamente disponibile e ridondante.

---

Q9. How does Spring Cloud Config Server work, and what are its main components?

A9.
## 🇬🇧 
It provides configuration via REST API from backends like Git. Services request config on startup. 

**Key components**:
- Config Server: Exposes REST API for configuration.
- Config Repository: Stores configuration data (e.g., Git, filesystem).
- Clients: Services that fetch configuration from the server at startup.

**It supports**:
- Profiles: allows different configurations for different environments (e.g., dev, prod).
- Encryption: Encrypts sensitive data in configuration files.
- Discovery integration: Works with service discovery tools like Eureka or Consul to locate the Config Server dynamically.

## 🇮🇹
Fornisce configurazioni via API REST da backend come Git. I servizi richiedono la configurazione all'avvio.

**Componenti chiave**:
- Config Server: Espone API REST per la configurazione.
- Config Repository: Memorizza i dati di configurazione (es. Git, filesystem).
- Client: Servizi che recuperano la configurazione dal server all'avvio.

**Supporta**:
- Profili: consente configurazioni diverse per ambienti diversi (es. dev, prod).
- Crittografia: Cifra i dati sensibili nei file di configurazione.
- Integrazione con Discovery: Funziona con strumenti di discovery dei servizi come Eureka o Consul per localizzare dinamicamente il Config Server.

---

Q10. What is the significance of using a Git repository for centralized configuration management?

A10.
## 🇬🇧 
Git enables version control, rollback, and traceability of changes. It integrates with CI/CD, ensures auditability (tracking who changed what and when), and allows distributed teams to manage configuration collaboratively and securely.

## 🇮🇹
Git consente controllo versione, rollback e tracciabilità dei cambiamenti. Si integra con CI/CD, garantisce auditability (tracciamento di chi ha cambiato cosa e quando) e consente a team distribuiti di gestire la configurazione in modo collaborativo e sicuro.
