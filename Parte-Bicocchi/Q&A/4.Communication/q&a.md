## 1) Commenta le principali fallacie dei sistemi distribuiti.

Le **fallacie del calcolo distribuito** sono convinzioni errate comunemente fatte dagli sviluppatori quando progettano sistemi distribuiti. Queste assunzioni possono portare a implementazioni fragili, inefficaci o addirittura fallimentari. Di seguito una panoramica delle principali:

---

### 1. **La rete è affidabile**

È un errore presumere che le connessioni tra i nodi siano sempre attive e stabili. In realtà:

- Le reti possono cadere temporaneamente.
- I pacchetti possono essere persi o duplicati.
- I timeout possono generare ambiguità (la richiesta è arrivata o no?).

✅ **Cosa fare:** Implementare retry, timeout intelligenti, logging e fallback. Usare l'iniezione delle dipendenze aiuta a testare meglio i componenti che dipendono da servizi remoti.

---

### 2. **La latenza è zero**

Presupporre che le comunicazioni tra componenti siano istantanee è sbagliato:

- La latenza può essere significativa (specialmente su Internet).
- Le differenze di latenza impattano le prestazioni e la UX.

📊 Esempio: 1s CPU = 100 anni in latenza Internet (in scala normalizzata).

```
| Process                             | Duration | Normalized |
|-------------------------------------|----------|------------|
| 1 CPU cycle                         | 0.3ns    | 1s         |
| L1 cache access                     | 1ns      | 3s         |
| L2 cache access                     | 3ns      | 9s         |
| L3 cache access                     | 13ns     | 43s        |
| DRAM access (from CPU)              | 120ns    | 6min       |
| SSD I/O                             | 0.1ms    | 4days      |
| HDD I/O                             | 1-10ms   | 1-12months |
| Internet: San Francisco to New York | 40ms     | 4years     |
| Internet: San Francisco to London   | 80ms     | 8years     |
| Internet: San Francisco to Sydney   | 130ms    | 13years    |
| TCP retransmit                      | 1s       | 100years   |
| Container reboot                    | 4s       | 400years   |
```

✅ **Cosa fare:** Progettare le applicazioni per essere tolleranti alla latenza, ad esempio con circuit breaker, cache locali e logica asincrona.

---

### 3. **La larghezza di banda è infinita**

Si tende a pensare che si possano inviare e ricevere dati illimitatamente. In realtà:

- La banda è limitata fisicamente e logicamente.
- Serializzazione, congestione di rete e overhead riducono la banda effettiva.

✅ **Cosa fare:** Minimizzare il traffico, evitare payload pesanti, usare compressione e streaming se possibile.

---

### 4. **La rete è sicura**

Assumere che i dati viaggino in modo sicuro è pericoloso:

- I dati possono essere intercettati o manipolati.
- Accessi non autorizzati possono verificarsi.

✅ **Cosa fare:** Usare TLS, autenticazione robusta, gestione dei secret, auditing e sicurezza end-to-end.

---

### 5. **La topologia non cambia**

Molti credono che la configurazione della rete sia statica, ma:

- I nodi possono essere aggiunti, rimossi o spostati.
- Le IP cambiano, i DNS si aggiornano, i container vengono riavviati.

✅ **Cosa fare:** Usare service discovery, DNS dinamici, e orchestratori (es. Kubernetes) per adattarsi ai cambiamenti.

---

### 6. **C'è un solo amministratore**

Spesso si crede che ci sia una sola persona a gestire tutto. Ma nei team distribuiti:

- Più sviluppatori e team interagiscono con il sistema.
- Il coordinamento è essenziale per evitare conflitti e disallineamenti.

✅ **Cosa fare:** Automatizzare i deployment, usare controllo di versione per la configurazione, definire processi condivisi.

---

### 7. **Il costo del trasporto è zero**

Trasferire dati ha sempre un costo:

- Latenza, serializzazione, CPU per encoding/decoding.
- Costo economico nel cloud (data egress, API calls, etc.).

✅ **Cosa fare:** Limitare i dati trasferiti, ottimizzare la serializzazione, monitorare i costi nei servizi cloud.

---

### 8. **La rete è omogenea**

È errato pensare che tutti i nodi siano uguali. In realtà:

- Tecnologie diverse (Java, Python, Go, Rust…).
- Sistemi operativi, database e formati di dati eterogenei.

✅ **Cosa fare:** Usare protocolli standard (REST, gRPC), formati comuni (JSON, Protobuf), e strumenti di integrazione robusti.

---

### Conclusione

Le **fallacie del calcolo distribuito** sono pericolose perché portano a sottovalutare la complessità della rete, della comunicazione e della cooperazione tra sistemi. Riconoscerle e progettare tenendone conto è essenziale per costruire **sistemi distribuiti resilienti, scalabili e manutenibili**.

## 2) Come contribuisce l'iniezione delle dipendenze alla diffusione delle fallacie dei sistemi distribuiti?

L’**iniezione delle dipendenze (Dependency Injection)** è una tecnica utile per aumentare la modularità e testabilità del codice, ma **può contribuire alla diffusione delle fallacie del calcolo distribuito** se utilizzata in modo inconsapevole nei contesti distribuiti. Ecco perché:

1. **Maschera la natura remota delle chiamate**

Quando un servizio remoto viene iniettato come una normale dipendenza (ad esempio un client HTTP), il codice **sembra locale**, ma **non lo è**.

```java
public class OrderService {
    private final PaymentService paymentService; // potrebbe sembrare locale

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void processOrder(Order order) {
        paymentService.charge(order); // in realtà è una chiamata remota
    }
}
```

👉 **Fallacia alimentata**: *“La rete è affidabile”* o *“La latenza è zero”*

Poiché la chiamata sembra una normale invocazione Java, gli sviluppatori dimenticano di gestire timeout, errori di rete, ritenti, fallback ecc.

---

2. **Favorisce l’astrazione eccessiva**

L’iniezione di un client remoto tramite interfacce astratte può far perdere consapevolezza che ci si sta collegando a un servizio remoto, potenzialmente instabile o con prestazioni variabili.

👉 **Fallacia alimentata**: *“Il costo di trasporto è zero”*

L’invocazione di metodi remoti può sembrare trascurabile, ma in realtà ogni chiamata comporta serializzazione, latenza, costi di rete, possibili ritrasmissioni.

---

3. **Rende difficile distinguere i confini di sistema**

Con l’iniezione automatica di dipendenze remote (es. tramite Spring o CDI), i confini tra componenti locali e distribuiti si offuscano. Questo può portare a una **scarsa progettazione della comunicazione tra servizi**, che è critica in un’architettura distribuita.

👉 **Fallacia alimentata**: *“La rete è omogenea”*

Non tutti i servizi parlano allo stesso modo o si comportano nello stesso modo. Trattarli come “black box” interscambiabili può essere rischioso.

---

4. **Difficoltà nel trattamento degli errori specifici**

Un client iniettato può nascondere eccezioni specifiche del contesto di rete (es. `HttpTimeoutException`, `ConnectException`), inducendo lo sviluppatore a trattarle come eccezioni normali o generiche.

👉 **Fallacia alimentata**: *“La rete è sicura”* o *“La topologia non cambia”*

Se non si distinguono i casi in cui il servizio è irraggiungibile da quelli in cui restituisce un errore, si possono prendere decisioni sbagliate lato business o sicurezza.

**In sintesi: *non dimenticare mai che una dipendenza remota non è una dipendenza locale*.**

## 3) Quali sono i quattro stili di comunicazione fondamentali nei sistemi distribuiti?

I **quattro stili di comunicazione fondamentali nei sistemi distribuiti** derivano dalla combinazione di due assi principali:

1. **Tipo di relazione** tra client e servizio:
    - **Uno-a-Uno**
    - **Uno-a-Molti**
2. **Modalità temporale** della comunicazione:
    - **Sincrona**
    - **Asincrona**

**Richiesta/Risposta (Uno-a-Uno, Sincrono):** In questo stile classico di interazione, un client invia una richiesta a un servizio e attende una risposta diretta. L’aspettativa è di avere una risposta tempestiva, il che può portare a un accoppiamento stretto tra il client e il servizio. Questo modello è comune nelle applicazioni web tradizionali e nelle API dove è richiesto un feedback immediato. -> ma è bloccante

**Richiesta/Risposta Asincrona (Uno-a-Uno, Asincrono):** In questa interazione, un client invia una richiesta ma non attende una risposta immediata. Invece, può continuare a elaborare altre attività. Il servizio risponderà eventualmente, ma il client non si blocca mentre attende. Questo modello aiuta a migliorare l’esperienza utente prevenendo freeze o ritardi dell’interfaccia utente.

**Publish/Subscribe (Uno-a-Molti, Asincrono):** Questo modello di interazione consente a un client di pubblicare messaggi su un argomento o canale che possono essere consumati da più servizi. I servizi interessati si iscrivono a questi messaggi, che possono essere elaborati in modo indipendente dal pubblicatore.

Questo disaccoppia i client dai servizi, consentendo un’architettura più flessibile.

**Publish/Subscribe Asincrone (Uno-a-Molti, Asincrono):** In questa variazione, un client pubblica un messaggio di richiesta a più servizi e attende risposte per un periodo di tempo specificato. Questo consente al client di ricevere input da vari servizi senza bloccare la propria operazione, aumentando l’efficienza.

| Tipo di Comunicazione | Relazione | Temporalità | Uso Tipico |
| --- | --- | --- | --- |
| Richiesta/Risposta | Uno-a-Uno | Sincrono | API REST, servizi web |
| Richiesta/Risposta Asincrona | Uno-a-Uno | Asincrono | Code di messaggi, elaborazioni batch |
| Publish/Subscribe | Uno-a-Molti | Asincrono | Event-driven architecture, notifiche |
| Pub/Sub con Risposte | Uno-a-Molti | Asincrono | Aggregazione di dati da più fonti |

## 4) Quali sono i limiti della comunicazione sincrona?

La **comunicazione sincrona** è un modello in cui un componente (tipicamente un client) invia una richiesta e attende una risposta prima di poter proseguire. Sebbene sia semplice da implementare (come nel caso di REST), presenta **diversi limiti strutturali**, specialmente in ambienti distribuiti e ad alta concorrenza, come quelli basati su microservizi.

### 🔷**Accoppiamento Temporale**

**Il concetto di accoppiamento temporale** si verifica quando i componenti o i servizi devono essere disponibili e reattivi nello stesso momento per funzionare correttamente. ⇒ *Una delle due piaghe irrisolvibili nella comunicazione sincrona.*

Questo accade spesso nei modelli di comunicazione sincrona, un componente deve attendere che un altro elabori una richiesta prima di procedere. Nei casi in cui una catena di più servizi deve comunicare, **la latenza accumulata può degradare significativamente le prestazioni**. **[non risolvibile con approcci sincroni!]**

- **Definizione:** Il client e il server devono essere attivi nello stesso momento.
- **Problema:** Se il server è lento o non risponde, il client è bloccato in attesa.
- **Conseguenza:** Aumenta la latenza percepita e può causare timeout.
- **Esempio:** Un client che chiama una catena di servizi (es. A → B → C) si blocca se uno dei servizi è lento o non disponibile.

### 🔷 **Accoppiamento Spaziale**

Il concetto di **accoppiamento spaziale**  si riferisce  al grado di dipendenza tra diversi componenti o servizi in un sistema in un dato momento.
Un alto grado di accoppiamento spaziale significa che i componenti sono strettamente connessi, richiedendo una conoscenza diretta dell'esistenza, delle interfacce o delle posizioni reciproche. Ciò può portare a una minore flessibilità e a una maggiore complessità di manutenzione. ⇒ La modifica di un componente comporta la modifica di tutti i componenti da esso dipendenti

- **Definizione:** I componenti devono conoscere esattamente la posizione (es. URL, porta) e l’interfaccia degli altri.
- **Problema:** Cambiamenti a un servizio impongono modifiche a tutti i componenti che lo utilizzano.
- **Esempio:** Un `OrderService` che chiama direttamente `http://payment-service:8080` è rigidamente accoppiato.

### 🔷 **Accoppiamento API**

**Accoppiamento API** si riferisce al grado di dipendenza tra un client e un’API.

Un'API altamente accoppiata significa che le modifiche nell'API possono facilmente rompere il client, mentre un'API a basso accoppiamento fornisce maggiore flessibilità e resilienza ai cambiamenti.

- **Definizione:** Il client è strettamente legato alla struttura dell’API.
- **Problema:** Qualsiasi modifica ai payload (es. aggiunta di campi) può rompere la compatibilità.
- **Esempio:** Modificare la classe `Payment` aggiungendo un campo `currency` può causare errori se il client non è aggiornato.

### 🔷 **Over-fetching**

**Over-fetching:** si verifica quando **un'API restituisce più dati di quanto il client necessiti effettivamente, portando a un utilizzo inefficiente della banda e a un tempo di elaborazione aumentato**. Questo avviene tipicamente nelle API REST con strutture di risposta fisse, dove un client non può specificare esattamente quali campi richiede.

*Scenario*: Un client desidera solo il titolo e l'autore di un libro, ma l'API restituisce l'intero oggetto libro, inclusi campi non necessari come ISBN, descrizione, editore, ecc.

- **Definizione:** Il client riceve **più dati** di quelli richiesti.
- **Problema:** Utilizzo inefficiente di rete e maggiore latenza.
- **Esempio:** Un client vuole solo `title` e `author`, ma l’API REST restituisce l’intero oggetto `Book` (inclusi `isbn`, `description`, ecc.).

### 🔷 **Under-fetching (Chattiness)**

**Under-fetching (noto anche come chattiness):** si verifica quando **un client richiede dati da un'API ma non riceve tutte le informazioni necessarie in una singola risposta.** Di conseguenza, il client deve effettuare ulteriori richieste per recuperare i dati mancanti, portando a inefficienze e a una latenza aumentata.

*Scenario*: L'endpoint /books restituisce un modello semplificato per i libri. Se il client ha bisogno anche dell'editore, deve effettuare richieste aggiuntive per recuperare i dettagli dell'editore come: GET /books/{title}

- **Definizione:** Il client non riceve **tutti i dati necessari** in una singola risposta.
- **Problema:** Deve fare più chiamate, aumentando la latenza e il carico.
- **Esempio:** Recupero dei dettagli del libro seguito da chiamate separate per l’editore e i commenti.

### 🔷**Esaurimento del Pool di Thread sul Client**

I client che attendono una risposta dal server consumano risorse di sistema (thread, memoria), il che può essere problematico in ambienti ad alta concorrenza. **[non risolvibile con approcci sincroni!]**

*Scenario* : Un client interroga l'endpoint /books 100 volte al secondo. Ogni richiesta richiede in media 1 secondo per essere soddisfatta. In qualsiasi momento, il **client** ha circa 100 thread nello stato di attesa (in attesa della risposta del server). Dato che ogni thread richiede 1 MB di RAM, cosa succede se il servizio libri smette di rispondere per 10 secondi? Il client avrà bisogno di 1 GB di RAM solo per gestirli!

- **Definizione:** Ogni richiesta sincrona occupa un thread finché non riceve risposta.
- **Problema:** In scenari ad alta concorrenza, i thread si esauriscono causando degrado delle performance.
- **Esempio:** Se 100 richieste REST durano 1 secondo, sono necessari 100 thread in attesa. Se il server rallenta, il consumo di RAM esplode.

### 🔷 **Serializzazione Verbosa**

**La serializzazione** è il processo di conversione di strutture dati o oggetti in un formato che può essere facilmente trasmesso o archiviato. Questo formato è spesso uno stream di byte o testo, che può essere successivamente deserializzato (convertito nuovamente) nella struttura dati o nell'oggetto originale. Questo concetto può portare ad una maggiore latenza nella risoluzione della richiesta.

- **Definizione:** REST utilizza solitamente JSON o XML.
- **Problema:** Sono formati **testuali**, verbosi, lenti da serializzare e deserializzare.
- **Conseguenza:** Aumenta la latenza della comunicazione.

## 5) Come Protobuf e GraphQL mitigano gli svantaggi di REST?

Rest soffre di tutti i limiti delle comunicazioni sincrone

**Come GraphQL Risolve le Limitazioni di REST**:

- **Accoppiamento API**: A differenza di REST, dove diverse risorse sono esposte attraverso molteplici endpoint, GraphQL opera attraverso un unico endpoint per tutte le query, semplificando la struttura dell'API. Diversamente da REST, che spesso richiede il versionamento per gestire i cambiamenti, lo schema flessibile di GraphQL permette modifiche non invalidanti, come l'aggiunta di nuovi campi senza impattare i client esistenti.
- **Over-fetching e under-fetching (verbosità)**: GraphQL permette ai client di richiedere solo i campi di cui hanno bisogno, risolvendo il problema di REST del ritorno di dati non necessari (over-fetching) o della necessità di effettuare multiple richieste per ottenere i dati necessari (under-fetching).
- **Serializzazione**: Il numero ridotto di richieste e risposte (grazie all'utilizzo di un singolo endpoint flessibile) riduce anche il carico complessivo della serializzazione.

**Come gRPC Risolve le Limitazioni di REST**:

- **Serializzazione**: gRPC utilizza Protocol Buffers (Protobuf), un formato binario compatto, per la serializzazione dei dati invece di JSON o XML (comunemente utilizzati nelle API REST). I formati binari sono molto più efficienti in termini di dimensioni e velocità perché occupano meno spazio e sono più rapidi da serializzare e deserializzare.
- **Accoppiamento API**: gRPC definisce contratti API rigorosi, garantendo una tipizzazione forte e riducendo il rischio di modifiche invalidanti. Con il supporto integrato per la compatibilità sia retroattiva che futura, l'evoluzione delle API è più fluida rispetto a REST, dove le modifiche nei payload JSON o nelle specifiche possono più facilmente introdurre incompatibilità.
- **Over-fetching**: gRPC riduce l'over-fetching attraverso una serializzazione binaria efficiente.
- **Under-fetching (chattiness)**: gRPC minimizza i viaggi multipli sfruttando il multiplexing di [HTTP/2](https://www.cloudflare.com/learning/performance/http2-vs-http1.1/), permettendo una comunicazione efficiente

**Limitazioni di GraphQL**:

- **Complessità nell'ottimizzazione delle query**: Mentre GraphQL offre flessibilità ai client, pone anche maggiore responsabilità sul server per ottimizzare le query. Se non gestite correttamente, le query complesse o profondamente annidate possono portare a colli di bottiglia nelle prestazioni lato server.
- **Sfide nel caching**: Il caching in GraphQL è più complesso rispetto a REST, poiché le query possono essere dinamiche e granulari. Le API REST possono sfruttare più facilmente il caching basato su HTTP (basato sugli endpoint), mentre GraphQL richiede strategie di caching più sofisticate.
- **Overhead degli schema**: Il sovraccarico della gestione degli schema e dei resolver potrebbe non giustificare i benefici in tutti i casi d'uso.

**Limitazioni di gRPC**:

- **Complessità**: Protobuf introduce una maggiore complessità in termini di definizioni degli schemi e generazione del codice.
- **Supporto Browser**: Mentre gRPC è eccellente per la comunicazione backend e tra servizi, non è ben supportato negli ambienti browser, limitando il suo utilizzo per le applicazioni frontend.
- **Tipizzazione Rigida**: Mentre la tipizzazione rigida garantisce robustezza, introduce anche rigidità, poiché le modifiche all'API richiedono una gestione attenta dei contratti Protobuf.

## 6) Quali sono i trade-offs tra l'utilizzo della messaggistica sincrona e asincrona in un sistema distribuito?

| Aspetto | Comunicazione **Sincrona** | Comunicazione **Asincrona** |
| --- | --- | --- |
| **Definizione** | Il client invia una richiesta e **attende la risposta** del servizio remoto. | Il client invia un messaggio e **non aspetta** una risposta immediata. La comunicazione avviene in modo decoupled. |
| **Tecnologie comuni** | REST, gRPC, HTTP | RabbitMQ, Kafka, SQS, NATS, Event Streams |
| **Semplicità** | Più semplice da implementare e testare | Richiede progettazione più attenta: messaggi, eventi, gestione errori |
| **Accoppiamento** | **Forte accoppiamento**: i servizi devono essere attivi contemporaneamente | **Basso accoppiamento**: il producer non ha bisogno che il consumer sia disponibile |
| **Prestazioni** | **Maggiore latenza percepita** per l’utente finale | **Migliore scalabilità** e throughput, minor tempo bloccante per i produttori |
| **Affidabilità** | Più fragile: se il servizio remoto è giù, la chiamata fallisce | Più resiliente: i messaggi possono essere accodati e processati quando possibile |
| **Debug/Tracing** | Più facile il tracing end-to-end (es. HTTP 500) | Più difficile il tracing: serve correlare eventi e gestire la delivery asincrona |
| **Consistenza** | Più semplice mantenere consistenza immediata | Consistenza eventuale: più difficile coordinare aggiornamenti distribuiti |

✅ **Quando usare la comunicazione sincrona**:

- Quando è richiesta una **risposta immediata** (es. autenticazione utente)
- Per **chiamate semplici e dirette** tra pochi servizi
- Quando la **consistenza immediata** è prioritaria

✅ **Quando preferire l'asincrona**:

- Per gestire **flussi ad alto carico**
- Per favorire **disaccoppiamento e resilienza**
- In architetture **event-driven** o **a microservizi**

## 7) Quali sono i trade-offs tra gli stili di comunicazione asincrona basati su broker e senza broker?

---

### 🔁 Con **Broker (Message Broker)**

Esempi: RabbitMQ, Kafka, ActiveMQ, Amazon SQS

| Vantaggi | Svantaggi |
| --- | --- |
| ✅ Gestione affidabile dei messaggi (persistenza, retry, dead-letter) | ❌ Introduce una **dipendenza centrale** (il broker) |
| ✅ Decoupling forte: producer e consumer non si conoscono | ❌ Più complesso da monitorare e gestire |
| ✅ Supporta pattern come fanout, topic, delayed delivery | ❌ Rischio di **single point of failure** (risolvibile con clustering) |

### 🔗 Senza **Broker (Brokerless, peer-to-peer)**

Esempi: ZeroMQ, WebSocket P2P, HTTP polling/event push, NATS (in alcune modalità)

| Vantaggi | Svantaggi |
| --- | --- |
| ✅ **Minore latenza**: meno hop nella rete | ❌ Meno affidabilità (nessuna persistenza integrata) |
| ✅ Più leggero, ideale per sistemi embedded o edge | ❌ Il producer deve conoscere e contattare direttamente i consumer |
| ✅ Più semplice in scenari con pochi nodi | ❌ Scalabilità e resilienza inferiori rispetto ai broker |

---

## 8) Cosa sono gli scambi di messaggi e le code, e come supportano la comunicazione nelle architetture a microservizi?

Nelle **architetture a microservizi**, la comunicazione tra servizi può avvenire in modalità **asincrona** utilizzando un sistema di **messaggistica**. Due componenti fondamentali di questi sistemi sono:

- **Exchange (Scambio di messaggi)**
- **Queue (Coda di messaggi)**

Sono concetti chiave di **AMQP** (Advanced Message Queuing Protocol), utilizzato da sistemi come RabbitMQ.

### 🔁 Exchange (Scambio)

Un **Exchange** è il componente che **riceve i messaggi dai produttori** (producer) e decide **come inoltrarli alle code** (queue), in base a **regole di instradamento** (routing).

📌 L’Exchange **non memorizza i messaggi**, ma **li distribuisce** alle code in base al tipo di exchange e alla *routing key* del messaggio.

### Tipi di exchange principali:

| Tipo di Exchange | Comportamento | Use case |
| --- | --- | --- |
| **Direct** | Inoltra il messaggio alla coda che ha **esattamente** la stessa `routing key`. | Log di sistema per livelli (es. ERROR, INFO). |
| **Fanout** | Inoltra **a tutte le code collegate** (ignora la routing key). | Notifiche broadcast (es. invio email/SMS a tutti). |
| **Topic** | Inoltra a code che corrispondono a un **pattern wildcard** nella routing key. | Eventi categorizzati, es. `ordine.spedito`, `utente.creato`. |
| **Headers** | Instrada in base a **metadati del messaggio** (header), non alla routing key. | Routing complesso (es. per area geografica, priorità). |

Le code sono un componente fondamentale che memorizza i messaggi. Agiscono come intermediari tra i produttori di messaggi e i consumatori.  → I messaggi inviati dalle applicazioni vengono instradati attraverso gli exchange e quindi finiscono in coda, in attesa di essere elaborati dalle applicazioni consumer.

Gli attributi chiave delle code in RabbitMQ, ad esempio, includono:

- **Archiviazione:** le code archiviano i messaggi fino a quando non vengono elaborati o utilizzati dalle applicazioni.
- **Durabilità**: le code possono essere durevoli, il che significa che sopravvivono al riavvio del broker. La durabilità garantisce che i messaggi non vadano persi anche in caso di riavvio di RabbitMQ.
- **Ordine dei messaggi:** per impostazione predefinita, RabbitMQ mantiene l'ordine dei messaggi all'interno di una coda (FIFO - First-In, First-Out).
- **Proprietà configurabili:** le code hanno proprietà configurabili come la lunghezza massima, i livelli di priorità massimi, il TTL (Time-To-Live) del messaggio, ecc., che consentono l'ottimizzazione per soddisfare requisiti specifici.

### 🧩 Come supportano le architetture a microservizi?

| Beneficio | Descrizione |
| --- | --- |
| ✅ **Disaccoppiamento** | Il produttore di un messaggio **non deve conoscere** il destinatario. Il messaggio è pubblicato su un exchange e recapitato alla queue giusta. |
| ✅ **Scalabilità** | Più consumer possono essere associati a una coda per elaborare messaggi in parallelo. |
| ✅ **Resilienza** | Se un servizio è momentaneamente non disponibile, la queue **mantiene i messaggi** fino al suo ritorno. |
| ✅ **Elasticità** | Possibilità di aggiungere nuovi microservizi ascoltando le stesse code senza modificare i producer. |
| ✅ **Event-driven architecture** | I microservizi reagiscono agli eventi pubblicati nel sistema: esempio, un servizio di fatturazione reagisce all’evento `ordine.pagato`. |

## 9) Come si possono scalare i consumer RabbitMQ in un'applicazione Spring Boot per gestire grandi volumi di messaggi? Descrivi le differenze tra gruppi di consumer, partizioni e routing dei messaggi.

RabbitMQ è un broker di messaggi open-source ampiamente utilizzato che facilita la comunicazione tra diverse parti di un sistema distribuito. Permette alle applicazioni di inviare e ricevere messaggi attraverso un sistema di messaggistica robusto e flessibile basato sul ***Protocollo Avanzato di Accodamento Messaggi (AMQP).*** RabbitMQ supporta vari modelli di messaggistica, rendendolo adatto a diverse casistiche.

In un sistema distribuito basato su RabbitMQ, **scalare i consumer** significa **aumentare la capacità dell'applicazione di consumare e processare messaggi in parallelo**, soprattutto in scenari ad alto volume.

In **Spring Boot con Spring AMQP**, è possibile scalare in diversi modi:

---

### 🔹 1. **Avviare più istanze dell’applicazione**

- Ogni istanza agisce come un **consumer** connesso alla stessa **queue**.
- RabbitMQ distribuisce automaticamente i messaggi in modo **round-robin** tra i consumer **concorrenti** (di default uno per istanza).
- ✅ Vantaggio: semplice da configurare, adatto al **bilanciamento del carico**.
- ⚠️ Limite: tutti i consumer leggono dalla **stessa queue** → nessuna partizione logica.

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        concurrency: 5      # numero minimo di consumer
        max-concurrency: 10 # numero massimo (auto-scaling)
```

---

### 🔹 2. **Configurare più consumer in una singola istanza**

- È possibile gestire **più thread consumer** all’interno della stessa JVM (Spring Boot app).
- Usando `SimpleMessageListenerContainer`, si può definire la **concorrenza locale** (thread multipli).

```java
@Bean
public SimpleRabbitListenerContainerFactory rabbitListenerContainerFactory(ConnectionFactory connectionFactory) {
    SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
    factory.setConnectionFactory(connectionFactory);
    factory.setConcurrentConsumers(5);
    factory.setMaxConcurrentConsumers(10);
    return factory;
}
```

***Componenti Chiave di RabbitMQ***

- **Produttore**: L’applicazione che invia messaggi al broker.
- **Consumatore:** L’applicazione che riceve messaggi da una coda.
- **Coda:** Un buffer che memorizza i messaggi fino a quando non vengono consumati.
- **Scambio:** Un meccanismo di instradamento che determina come i messaggi vengono distribuiti alle code.
- **Collegamento:** Un legame tra uno scambio e una coda che definisce le regole di instradamento per i messaggi.
- **Chiave di Routing:** L’attributo di messaggio che viene preso in considerazione dallo scambio quando si decide come instradare un messaggio.

![image](https://github.com/user-attachments/assets/9e013b1c-cda7-4746-8844-194ed3f7f0fb)


Gli Exchange e le code vanno configurate in modo da rispettare i requisiti del sistema.

### ⚙️ **Differenze tra gruppi di consumer, partizioni e routing dei messaggi**

🔸 **Consumer Group** (concetto più legato a Kafka)

- In RabbitMQ, non esistono formalmente **consumer group** come in Kafka.
- Tuttavia, si può **simulare** un comportamento simile assegnando **code diverse a gruppi di consumer** o usando **routing keys dinamici**.
- Ogni consumer riceve messaggi solo da **una queue**. Se più consumer ascoltano la **stessa queue**, condividono il carico.

**🔸 Partizioni (concetto di Kafka, assente in RabbitMQ)**

- RabbitMQ **non supporta partizioni native**. Ogni queue è un’unità di consumo singola.
- Si può **simulare la partizione** creando **più code** e usando il routing per distribuire i messaggi.
- Esempio:
    - `queue.1`, `queue.2`, `queue.3`
    - Routing key: `event.1`, `event.2`, `event.3`

🔸 **Routing dei messaggi (Exchange)**

- Il **meccanismo principale per scalare logicamente i consumer in RabbitMQ**.
- Puoi usare **exchange + routing key** per smistare i messaggi su **più code**, ognuna consumata da un gruppo dedicato.

| Tipo Exchange | Routing Strategy | Use Case |
| --- | --- | --- |
| **Direct** | Match esatto con routing key | Log per livello (INFO, ERROR) |
| **Topic** | Pattern-based (`*.user`) | Eventi categorizzati (user.signup, user.login) |
| **Fanout** | Nessuna routing key (broadcast) | Notifiche push a tutti i consumer |
| **Headers** | Match su metadati (es. `x-match=all`) | Filtraggio avanzato multi-condizione |

---

Per scalare i consumer RabbitMQ in Spring Boot:

- Usa **più consumer concorrenti** (thread o istanze)
- Configura **routing + exchange** per dividere logicamente il carico
- Tieni presente che RabbitMQ **non ha partizioni né consumer group** come Kafka
- Puoi comunque **simulare la segmentazione** tramite exchange, routing key e code multiple

## 10) Cosa sono i DTO e qual è il loro ruolo nella comunicazione distribuita?

Un **DTO (Data Transfer Object)** è un **design pattern** usato per **trasferire dati tra processi**, livelli o moduli di un'applicazione, in particolare in **contesti distribuiti**, come microservizi o API RESTful.

### Obiettivo principale:

> Trasportare dati in modo semplificato, efficiente e disaccoppiato, evitando di esporre direttamente entità interne (es. oggetti di dominio o entità del database).
> 

---

### Caratteristiche chiave

| Caratteristica | Descrizione |
| --- | --- |
| **Senza logica di business** | Sono "contenitori passivi" (POJO in Java, Pydantic/BaseModel in Python), che includono solo dati e metodi di accesso (`getter/setter`). |
| **Selezione dei campi** | Contengono solo i campi **necessari per la comunicazione**, riducendo il payload. |
| **Serializzabili** | Possono essere facilmente **serializzati/deserializzati** (JSON, XML, etc.) per viaggiare attraverso la rete. |
| **Disaccoppiano** | Separano **modelli interni** e rappresentazioni esterne (API), garantendo maggiore **flessibilità e sicurezza**. |

In un sistema distribuito (es. microservizi, SOA), i moduli comunicano attraverso la rete. In questo contesto, i DTO svolgono ruoli fondamentali:

***Astrazione del modello interno***

I modelli di dominio (entità) sono spesso complessi o contengono dati sensibili.

- I DTO fungono da **rappresentazioni semplificate e sicure**.

***Riduzione della quantità di dati trasmessi***

I DTO evitano il trasferimento di dati superflui.

Questo riduce:

- Carico sulla rete
- Tempo di serializzazione
- Costi su reti mobili o API pubbliche

***Stabilità dell’interfaccia***

Le modifiche al modello interno **non influenzano** i consumatori esterni, perché l’interfaccia pubblica (il DTO) resta stabile.

***Validazione e sicurezza***

I DTO possono essere dotati di **validazione dei campi** (es. Pydantic in Python) e mascherare dati sensibili. Questo migliora la **sicurezza** delle API e la robustezza contro input malformati.

***Facilitano la serializzazione***

Essendo progettati per la trasmissione, i DTO sono facilmente **convertibili in formati standard** (JSON, XML) per la comunicazione tra servizi scritti in linguaggi diversi.

### 🔄 **Mapping: DTO vs Entità**

Poiché i DTO sono separati dalle entità di dominio o dal modello dati, è necessario un **mapping bidirezionale**:

- **Da Entità a DTO** → per inviare dati in uscita (es. risposte API)
- **Da DTO a Entità** → per ricevere e validare input (es. creazione/modifica di record)

| Linguaggio | Libreria | Note |
| --- | --- | --- |
| Java | MapStruct | Mapping a tempo di compilazione, veloce e type-safe |
| Java | ModelMapper | Mapping flessibile, a runtime |
| Python | Marshmallow | Serializzazione + validazione |
| Python | Pydantic | Validazione, conversione e auto-documentazione (es. con FastAPI) |

### 🧪 **Esempio Pratico (Java)**

```java
// Entità
public class UserEntity {
    private Long id;
    private String username;
    private String passwordHash;
    private Date createdAt;
}

// DTO
public class UserDTO {
    private String username;
}

```

➡️ Espone solo `username`, **nascondendo la password** e altri dati interni

---

## 11) Perché Spring Cloud Stream fornisce un livello di astrazione rispetto all’uso diretto delle librerie native dei broker?

Spring Cloud Stream è progettato per semplificare lo sviluppo di applicazioni basate su messaggistica asincrona, offrendo un **livello di astrazione** che nasconde le complessità specifiche di ciascun sistema di messaggistica (come Kafka, RabbitMQ, Amazon Kinesis, ecc.). Questo livello di astrazione si traduce in:

1. **Portabilità e indipendenza dalla tecnologia sottostante:**
    
    Gli sviluppatori scrivono il codice utilizzando API e concetti comuni (binding, channels, sources, sinks) senza dover conoscere le API specifiche del broker. Se si decide di cambiare broker, basterà modificare la configurazione senza toccare il codice applicativo.
    
2. **Semplificazione dello sviluppo:**
    
    La gestione delle connessioni, serializzazione, error handling, partizionamento e bilanciamento del carico sono gestiti automaticamente dal framework, riducendo la quantità di codice boilerplate e i potenziali errori.
    
3. **Supporto a pattern comuni e best practice:**
    
    Spring Cloud Stream incorpora nativamente pattern di messaging come pub-sub, consumer groups, gestione dei gruppi di consumatori, partizionamento, retry automatici, circuit breaker e altre funzionalità avanzate, che altrimenti richiederebbero implementazioni manuali.
    

### Vantaggi dell’astrazione rispetto all’uso diretto delle librerie native:

- **Maggiore velocità di sviluppo** e manutenzione più semplice.
- **Flessibilità:** possibilità di cambiare broker senza riscrivere il codice.
- **Uniformità:** si usa un modello standard indipendente dal provider.

### Trade-off e limiti:

- **Minor controllo sui dettagli più specifici e avanzati** di ciascun broker (es. configurazioni custom o ottimizzazioni di basso livello).
- **Overhead di astrazione:** una leggera perdita in performance e configurabilità fine.
- **Possibili ritardi nel supporto delle nuove funzionalità** specifiche del broker finché non vengono integrate in Spring Cloud Stream.
- **Dipendenza da un framework esterno**, che richiede aggiornamenti e attenzione alle versioni.

Spring Cloud Stream è ideale per chi vuole **scrivere codice pulito, modulare e facilmente mantenibile**, senza dover conoscere i dettagli di ogni broker di messaggistica. Offre un bilanciamento ottimale tra facilità d’uso e potenza, ma chi ha necessità di funzionalità molto specifiche o ottimizzazioni avanzate può valutare l’uso diretto delle librerie native.
