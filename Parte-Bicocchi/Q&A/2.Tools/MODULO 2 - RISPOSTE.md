# MODULO 2 - RISPOSTE

---

### 1) Che cos’è Maven e quale ruolo svolge nella gestione di progetti Java?

Maven è un sistema di gestione del build e delle dipendenze per progetti Java. 

In un progetto Maven, tutte le configurazioni sono centralizzate, e il ciclo di vita del progetto (compilazione, test, packaging) è definito attraverso fasi e plugin. Spring Boot lo utilizza come base per facilitare l’avvio e la gestione delle dipendenze.

Organizza il progetto secondo una struttura standardizzata e semplifica l’integrazione di librerie esterne tramite un file centrale (`pom.xml`). 

Nel **`pom.xml`**,  si:

- definisce il parent (`spring-boot-starter-parent`)
- specificano `groupId`, `artifactId`, `version`
- configurano le dipendenze necessarie per la compilazione del progetto e la realizzazione dei jar.

Spring Boot aggiunge un plugin Maven (`spring-boot-maven-plugin`) che:

- Consente di eseguire l’applicazione con `mvn spring-boot:run`
- Permette di creare automaticamente un **fat JAR** (o “uber-jar”) contenente tutte le dipendenze in un unico archivio eseguibile ().

---

### 2) Come vengono definite le dipendenze nel file `pom.xml` di Maven? Esempio:

Le dipendenze sono dichiarate nella sezione `<dependencies>` del file `pom.xml`, specificando `groupId`, `artifactId` e, facoltativamente, `version`. Esempio:

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```

Questo approccio favorisce la separazione tra codice e configurazioni e quindi soddisfa la regola #2 The Twelve Factors: *la gestione delle dipendenze deve
avvenire in modo centralizzato dividendo la configurazione di esse dal codice sorgente.* 

Spring  Boot offre il supporto per il dependency injection, che consente di gestire le dipendenze delle componenti dell’applicazione in modo centralizzato.

---

### 3) Cosa sono i concetti di lifecycle, phases e goals nel processo di build Maven?

Maven organizza il processo di build in **lifecycle**, ciascuno dei quali ha delle **fasi** (phases) come `compile`, `test`, `package`, ecc. Ogni fase può includere uno o più **goal**, ovvero azioni concrete eseguite da plugin (es. `compile:compile`, `surefire:test`). È un modello dichiarativo che consente automazione e coerenza nel ciclo di vita del software.

- **Lifecycle `default`** include:
    - `validate`
    - `compile`
    - `test`
    - `package`
    - `install`
    - `deploy`

Ogni **fase** attiva uno o più **goal**. Ad esempio:

- `compile` → compila i sorgenti (`compiler:compile`)
- `test` → esegue i test (`surefire:test`)
- `package` → crea il JAR/WAR

Ad esempio `mvn clean package`:

1. **Fase `clean`**

- Elimina i file generati da build precedenti, in particolare la cartella `target/` dove Maven crea gli artefatti (`.class`, `.jar`, ecc.).
- Garantisce una **build "pulita"**, evitando conflitti con versioni precedenti.

2. **Fase `package`**

- Compila il codice sorgente (`compile`)
- Esegue la fase `test`, **ma** in questo caso è disabilitata con `Dmaven.skip.test=true`
- Impacchetta tutto in un **file `.jar`**
- Grazie al plugin `spring-boot-maven-plugin`, viene creato un **fat JAR**, cioè un archivio `.jar` **eseguibile** che include:
    - tutte le classi compilate del progetto
    - tutte le dipendenze definite nel `pom.xml`
    - file di configurazione (es. `application.properties`)
    - un manifest con il main class

Output generato: Un file  fat `.jar` eseguibile nella directory `target/`

---

### 4) Cos’è un fat JAR e perché è utile in produzione?

Un **fat JAR** è un file `.jar` che contiene l'applicazione e tutte le sue dipendenze, rendendolo eseguibile autonomamente. Spring Boot crea fat JAR tramite il plugin Maven. È utile usare un jar in produzione perché:

✅ **Vantaggi in produzione**:

- Portabilità: basta una sola unità eseguibile.
- Nessuna necessità di avere Maven o codice sorgente.
- Facile da usare in container Docker.

---

### 5) È consigliabile usare `mvn spring-boot:run` in produzione?

**No**, non è consigliato perché:

- Il codice sorgente deve essere presente sul server.
- L’avvio è lento (scarica dipendenze, compila, esegue).
- Non è ottimizzato per ambienti produttivi.

Meglio eseguire un fat JAR o un'immagine Docker precompilata per avvio rapido e stabilità.

---

### 6) Come acquisisce Spring Boot le configurazioni? Approccio gerarchico:

Spring Boot segue una **gerarchia** per acquisire le configurazioni:

1. Argomenti da linea di comando (`-server.port=8082`)
2. Variabili d’ambiente (`SERVER_PORT=8182`)
3. File `application.properties` o `application.yml`
4. File `application-{profile}.properties`
5. Configurazioni nel codice Java (annotazioni)

Questo consente flessibilità e supporta il principio **“configurazione esterna al codice”**, in linea con la metodologia **12-Factor App**.

---

### 7) Cos’è Project Lombok e in che modo semplifica lo sviluppo Java?

**Lombok** è una libreria che riduce il codice boilerplate in Java tramite annotazioni come `@Getter`, `@Setter`, `@Data`, `@Builder`, ecc. Evita di scrivere manualmente costruttori, `toString()`, `equals()`, ecc., aumentando la produttività e migliorando la leggibilità del codice.

Esempio: 

```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Event<K, T> {
  private Type eventType;
  private K key;
  private T data;
  private ZonedDateTime eventCreatedAt;
}
```

---

### 8) Cos’è l’inversion of control (IoC) e come viene implementata da Spring?

L’ Inversion of Control è un principio nella programmazione  software che trasferisce il controllo di porzioni di un programma a un contenitore o un framework esterno. Lo utilizziamo più spesso nel contesto della programmazione orientata agli oggetti.
A differenza dell’OOP tradizionale, in cui il nostro codice personalizzato chiama una libreria,  l’IoC  consente a un framework di controllare il flusso di un programma e di chiamare il nostro codice personalizzato.

Per consentire ciò, i framework utilizzano astrazioni con comportamenti aggiuntivi integrati. Se vogliamo aggiungere il nostro comportamento,  dobbiamo estendere le classi del framework o collegare le nostre classi.
Possiamo ottenere l’Inversione di Controllo attraverso vari meccanismi: 

- Pattern Strategy
- Pattern Service Locator
- Pattern Factory
- Pattern di Iniezione delle Dipendenze

Spring realizza l’IoC attraverso l’**iniezione delle dipendenze**, gestita dal suo contenitore (`ApplicationContext` che è responsabile dell’istanza, della configurazione e dell’assemblaggio dei bean). Le classi non istanziano le proprie dipendenze, ma le ricevono dall’esterno: 

- via costruttore ✅ consigliata
- via setter
- via campo (📛 sconsigliata)

**Via costruttore**

In questo caso, iniettiamo le dipendenze in una classe tramite gli argomenti del suo costruttore (ogni argomento rappresenta una dipendenza che Spring inietterà automaticamente).

```java
@Service
public class ProductService {
    private final ProductRepository productRepository;

    @Autowired
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }
    // ...
}
```

Nell’iniezione basata su setter, iniettiamo le dipendenze utilizzando i metodi setter delle dipendenze richieste dichiarate come campi: 

- dipendenze opzionali
- cambiamenti delle dipendenze a runtime

Nell’iniezione delle dipendenze tramite campo **iniettiamo le dipendenze utilizzando l’annotazione *@Autowired* direttamente sui campi**. Questo approccio non è più  raccomandato.

---

### 9) Cos’è un bean in Spring? Modi per definirlo, @Primary e @Qualifier:

Un **bean** è un oggetto gestito dal contenitore Spring. Può essere definito:

1. Con annotazioni stereotipate (`@Component`, `@Service`, `@Repository`, `@RestController`)
2. Tramite metodo annotato con `@Bean` in una classe `@Configuration`

`@Primary`: indica il bean da usare di default se ne esistono più di uno dello stesso tipo. 

```java
@Component @Primary
public class Car implements Vehicle { … }

@Component
public class Bike implements Vehicle { … }

@Component
public class Driver {
    private final Vehicle vehicle;

    @Autowired
    public Driver(Vehicle vehicle) { // inietta Car senza bisogno di @Qualifier
        this.vehicle = vehicle;
    }
    // ...
}
```

`@Qualifier`: specifica quale bean usare quando ci sono ambiguità.

```java
@Component
public class Car implements Vehicle { … }

@Component
public class Bike implements Vehicle { … }

@Component
public class Driver {
    private final Vehicle vehicle;

    @Autowired
    public Driver(@Qualifier("car") Vehicle vehicle) { // inietta Car
        this.vehicle = vehicle;
    }
    // ...
}

```

| Metodo | Descrizione | Esempio |
| --- | --- | --- |
| `@Component` | Generico | `@Component class X` |
| `@Service` | Logica di business | `@Service class MyService` |
| `@Repository` | Accesso ai dati | `@Repository class MyRepo` |
| `@RestController` | Web REST API | `@RestController class ApiCtrl` |
| `@Bean` + `@Configuration` | Manuale | `@Bean method()` in `@Configuration` |
| XML (legacy) | Configurazione esterna | `<bean class="..."/>` |

---

### 10) Ruolo dei file `application.properties` in Spring Boot. Vantaggi:

I file `application.properties` (o `.yml`) permettono di configurare l'applicazione senza modificare il codice. Consentono:

- Separazione tra codice e configurazione
- Profilazione (`application-dev.properties`, `application-prod.properties`): 
 Quando si attiva un profilo (tramite variabile `spring.profiles.active`), Spring carica prima il file specifico, poi quello generico, sovrascrivendo i valori.

**Vantaggi**:

- Non serve ricompilare l’applicazione per cambiare parametri: basta modificare il file di configurazione esterno (oppure passare variabili d’ambiente).
- Si seguono i principi della **12-Factor App**, in particolare il punto #3 “Config”: “Store config in environment”. Separando configurazione e codice, garantiamo:
    - **Portabilità**: lo stesso artefatto JAR funziona in ambienti differenti (dev, test, prod), basta cambiare le properties.
    - **Sicurezza**: le credenziali non sono scritte nel codice, ma lette in runtime da un file o da environment variables.
    - **Manutenibilità**: i parametri di tuning (pool di connessioni, timeouts, ecc.) si gestiscono esternamente ().

---

### 11) Come funziona il sistema di logging? Utilizzo efficace dei livelli di log:

Quando aggiungiamo la dipendenza spring-boot-starte**r, spring-boot-starter-logging è già incluso in modo transitivo**. Questo include tutte le dipendenze necessarie per il logging. Questa pratica è utile per:

- Controllare la verbosità dei log
- Aiutare nel debugging e nel monitoraggio
- Migliorare l’osservabilità del sistema

Il logging in Spring Boot si basa su SLF4J + Logback. I principali livelli sono:

- `TRACE`: dettagli minimi, utile per debugging fine
- `DEBUG`: informazioni per lo sviluppatore
- `INFO`: eventi generali (es. avvio servizi)
- `WARN`: situazioni anomale non critiche
- `ERROR`: errori da correggere

**Uso corretto**:

- In dev: `DEBUG` o `TRACE`
- In prod: `INFO` o superiore per evitare sovraccarico

---

### 12) Come aiutano gli Spring Boot Actuator nella gestione di microservizi in produzione?

**Gli attuatori sono strumenti di monitoraggio**. Portano funzionalità pronte per la produzione nella nostra app con uno sforzo molto ridotto. Gli attuatori forniscono vari endpoint (documentati [qui](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)) che aiutano nel monitoraggio e, fino a un certo punto, nella gestione della nostra applicazione. Il modo migliore per abilitare gli attuatori è aggiungere la dipendenza **spring-boot-starter-actuator.**

**Spring Boot Actuator** espone endpoint REST per monitorare e gestire l’applicazione:

- **`/actuator/health`**: restituisce lo stato di salute (up/down) dell’applicazione, e può includere sotto-check (database, cache, ecc.).
- **`/actuator/metrics`**: fornisce metriche sulle performance (CPU, memoria, tempi di risposta, contatori custom).
- **`/actuator/info`**: mostra informazioni aggiuntive definite in `application.properties` (versione, nome applicazione).
- **`/actuator/env`**: per visualizzare l’ambiente di esecuzione (proprietà caricate).
- **`/actuator/beans`**: elenca tutti i bean creati da Spring, utile per debug e verifica delle dipendenze.

Permette **osservabilità** e **monitoraggio**, fondamentali nei microservizi per diagnosi e gestione dinamica, spesso integrato con strumenti come Prometheus, Grafana, ecc.

---

### 13) Funzionalità principali di Spring Data JPA:

Spring Data JPA fornisce un livello di astrazione per l’accesso ai dati:

- Definizione di repository con interfacce (senza implementazioni)
- Derivazione automatica delle query dai nomi dei metodi (`findByUuid`)
- Supporto per auditing (data creazione/modifica)
- Gestione trasparente delle entità tramite annotazioni JPA

Semplifica l’interazione con il database riducendo drasticamente la quantità di codice necessario.

Vantaggi:

- Nessuna implementazione manuale
- Facile da testare
- Supporto per operazioni CRUD e complesse

---