Q1. What challenges arise in maintaining data consistency when using the one database per service pattern in microservices?

A1. 
## 🇬🇧
Using a separate database per service improves independence and scalability, but it complicates maintaining ACID properties across services. When a transaction spans multiple databases, ensuring atomicity, consistency, isolation, and durability becomes difficult without global coordination.

## 🇮🇹  
Utilizzare un database separato per ciascun servizio migliora l'indipendenza e la scalabilità, ma rende difficile mantenere le proprietà ACID tra i servizi. Quando una transazione coinvolge più database, garantire atomicità, consistenza, isolamento e durabilità diventa complicato senza un coordinamento globale.

---

Q2. What is the Saga pattern, and why is it essential for managing distributed transactions? Describe the choreography and orchestration approaches.

A2.
## 🇬🇧
The Saga pattern defines a saga as a sequence of transactions that can be interleaved with one another.

The Saga pattern breaks a transaction into smaller, local transactions with compensating actions for rollback. 

It guarantees that either all operations complete successfully or the corresponding compensation transactions are run to undo the work previously completed.

The Saga pattern aligns with **AP** in the **CAP** theorem, favoring Availability. The Saga pattern offers **eventual consistency** through **compensating transactions**, ensuring the system eventually reaches a valid state without requiring strong consistency. It enhances **availability** by being **non-blocking**, allowing operations to proceed even during failures, and supports partition tolerance by executing steps asynchronously without the need for global coordination.

It is ideal for systems where **high availability** and **partition tolerance** are priorities, and where eventual consistency is acceptable, such as e-commerce order processing.

- **Orchestration**: A central Saga Execution Coordinator manages the flow and recovery.
    - For any failure, the SEC inspects the Saga log to identify the impacted components and the sequence in which the compensating transactions should run.
    - For any failure in the SEC, it can read the Saga log once it's coming back up. It can then identify the transactions successfully rolled back, which ones are pending, and can take appropriate actions.
- **Choreography**: Services communicate through events without central control, suited for simpler workflows.

## 🇮🇹

Il pattern Saga definisce una saga come una sequenza di transazioni che possono essere interlacciate tra loro.

Il pattern Saga suddivide una transazione in transazioni locali più piccole con azioni compensative per il rollback.

Garantisce che tutte le operazioni vengano completate con successo o che le transazioni di compensazione corrispondenti vengano eseguite per annullare il lavoro precedentemente completato.

Il pattern Saga si allinea con l'**AP** nel **CAP** theorem, privilegiando la disponibilità. Il pattern Saga offre **consistenza eventuale** attraverso **transazioni compensative**, garantendo che il sistema raggiunga infine uno stato valido senza richiedere una consistenza forte. Migliora la **disponibilità** essendo **non bloccante**, consentendo alle operazioni di procedere anche durante i guasti, e supporta la tolleranza alle partizioni eseguendo i passaggi in modo asincrono senza la necessità di un coordinamento globale.

- **Orchestrazione**: Un coordinatore centrale (Saga Execution Coordinator) gestisce il flusso e il recupero della Saga.
    - In caso di errore, il SEC ispeziona il log della Saga per identificare i componenti interessati e la sequenza in cui devono essere eseguite le transazioni compensative.
    - In caso di errore nel SEC, può leggere il log della Saga una volta che è tornato operativo. Può quindi identificare le transazioni che sono state annullate con successo, quelle in sospeso e prendere le azioni appropriate.

- **Coreografia**: I servizi comunicano tramite eventi senza controllo centrale, adatto a flussi di lavoro più semplici.

---

Q3. What does the CAP theorem state, and how does it affect the design of distributed systems?

A3.
## 🇬🇧 
The CAP theorem states that a distributed system can guarantee at most two out of the following three properties:
- **Consistency**: All nodes see the same data at the same time.
- **Availability**: Every request receives a response.
- **Partition Tolerance**: The system continues to operate despite network partitions.

Working in a distributed environment, we are almost always faced with network partitions. Therefore, systems must choose between being consistent or available.

## 🇮🇹
Il teorema CAP afferma che un sistema distribuito può garantire al massimo due delle seguenti tre proprietà:
- **Consistenza**: Tutti i nodi vedono gli stessi dati nello stesso momento.
- **Disponibilità**: Ogni richiesta riceve una risposta.
- **Tolleranza alle partizioni**: Il sistema continua a funzionare nonostante le partizioni di rete.

Lavorando in un ambiente distribuito, ci troviamo quasi sempre di fronte a partizioni di rete. Pertanto, i sistemi devono scegliere tra essere consistenti o disponibili.

---

Q4. What role do compensating transactions play in the Saga pattern, and why must they be idempotent?

A4.
## 🇬🇧
Compensating transactions undo the effects of completed steps in case of failure. They must be idempotent to ensure safe retries without unintended side effects, and retryable to guarantee eventual rollback.

## 🇮🇹
Le transazioni compensative annullano gli effetti dei passaggi completati in caso di errore. Devono essere idempotenti per garantire ripetizioni sicure senza effetti collaterali e ripetibili per assicurare il rollback finale.

---

Q5. What is the two-phase commit protocol, and how does it ensure atomicity in distributed transactions?

A5.
## 🇬🇧 
2PC is a protocol where a coordinator first asks all participants if they can commit (prepare phase). If all agree, it instructs them to commit (commit phase). This ensures all-or-nothing behavior (atomicity).

## 🇮🇹
Il 2PC è un protocollo in cui un coordinatore chiede prima a tutti i partecipanti se possono eseguire il commit (fase di preparazione). Se tutti concordano, ordina loro di completare la transazione (fase di commit). Questo garantisce il comportamento tutto-o-niente (atomicità).

---

Q6. What are the performance and scalability trade-offs of using two-phase commit in a distributed environment?

A6.
## 🇬🇧
2PC introduces delays due to coordination and waiting for all participants. It has a single point of failure (the coordinator) and doesn't work well with NoSQL databases, limiting scalability and fault tolerance.

## 🇮🇹 
Il 2PC introduce ritardi a causa del coordinamento e dell'attesa di tutti i partecipanti. Ha un punto singolo di errore (il coordinatore) e non è compatibile con molti database NoSQL, limitando scalabilità e tolleranza ai guasti.

---

Q7. What is Conductor, and how does it help manage distributed workflows?

A7.
## 🇬🇧
Conductor is a microservices orchestration framework developed by Netflix that facilitates the management of complex processes within microservices architectures. It provides retries, error handling, and supports Saga-style compensation.
Conductor can be run either locally or in the cloud. It provides a web-based UI for monitoring and managing workflows, making it easier to visualize and control complex processes.

## 🇮🇹
Conductor è un framework di orchestrazione di microservizi sviluppato da Netflix che facilita la gestione di processi complessi all'interno di architetture a microservizi. Fornisce funzionalità di retry, gestione degli errori e supporta la compensazione in stile Saga.
Conductor può essere eseguito localmente o nel cloud. Fornisce una UI web based per il monitoraggio e la gestione dei flussi di lavoro, rendendo più facile visualizzare e controllare processi complessi.

---

Q8. What challenges drive the adoption of the CQRS pattern in large-scale applications?

A8.
## 🇬🇧
CQRS is adopted when read and write workloads differ significantly. Traditional CRUD models struggle with performance at scale, especially for read-heavy systems or when queries require complex aggregations.

## 🇮🇹
Il CQRS viene adottato quando i carichi di lavoro di lettura e scrittura differiscono significativamente. I modelli CRUD tradizionali faticano a gestire le prestazioni su larga scala, specialmente nei sistemi read-heavy o con query complesse.

---

Q9. How does the CQRS pattern enable independent scaling and performance optimization for read and write operations?

A9.
## 🇬🇧
By separating reads (queries) and writes(commands) into different services, CQRS allows each to scale independently. Read services can use denormalized data or caches, while write services can prioritize data integrity.

## 🇮🇹
Separando letture (queries) e scritture (commands) in servizi diversi, CQRS consente una scalabilità indipendente. I servizi di lettura possono usare dati denormalizzati o cache, mentre quelli di scrittura possono concentrarsi sull'integrità dei dati.

---

Q10. How can a CQRS-based system align with either the CP or AP model under the CAP theorem?

A10.
## 🇬🇧
CQRS are not inherently CP or AP but can align with either based on consistency needs:
- **CP**: when immediate consistency is needed, ensuring that read and write operations are synchronized. When data is not synchronized, reads may block until consistency is achieved.
- **AP**: when eventual consistency is acceptable when a sync write is not available, reads may return temporarily stale data, but the system remains available.

## 🇮🇹
CQRS non è intrinsecamente CP o AP, ma può allinearsi con uno dei due in base alle esigenze di consistenza:
- **CP**: quando è necessaria consistenza immediata garantendo che le operazioni di lettura e scrittura siano sincronizzate. Quando i dati non sono sincronizzati, le letture vengono bloccate fino al raggiungimento della consistenza.
- **AP**: quando è accettabile una consistenza eventuale quando una scrittura di sincronizzazione non è disponibile, le letture possono restituire dati temporaneamente obsoleti, ma il sistema rimane disponibile.

