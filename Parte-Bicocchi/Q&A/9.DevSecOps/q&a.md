## 🇬🇧
Software production lifecycle consists if three essential stages:

1. **Requirements Gathering**: 
    - Define the scope, functionality, and specification of the software.
    - Document requirements to guide development process.
2. **Development and Testing**:
    - Design and implement the software according to requirements.
    - Perform testing to ensure functionality, performance, and security and resolve defects.
3. **Operations and Maintenance**:
    - Set up and manage the infrastructure required for the application.
    - Monitor, maintain, and update the software to ensure performance, security and reliability.

## 🇮🇹
Il ciclo di vita della produzione software consiste in tre fasi essenziali:
1. **Raccolta dei Requisiti**: 
    - Definire l'ambito, le funzionalità e le specifiche del software.
    - Documentare i requisiti per guidare il processo di sviluppo.
2. **Sviluppo e Test**:
    - Progettare e implementare il software secondo i requisiti.
    - Eseguire test per garantire funzionalità, prestazioni e sicurezza e risolvere i difetti
3. **Operazioni e Manutenzione**:
    - Configurare e gestire l'infrastruttura necessaria per l'applicazione.
    - Monitorare, mantenere e aggiornare il software per garantire prestazioni, sicurezza e affidabilità.

---
Q1. How does the DevOps approach differ from traditional models such as Waterfall and Agile? In what ways does DevOps improve automation in the software development lifecycle?

A1.

## 🇬🇧

**About Waterfall**:
It creates friction between requirements gathering and development given its rigid and linear structure. No flexibility to adapt to changes once the requirements are set. Key concepts:
- **Rigid change management**: Changes are difficult to implement after requirements are defined.
- **Late discovery of issues**: Testing occurs late in the process, leading to costly fixes.
- **Misalgnment with Client Expectations**: What is delivered may not match what the client actually needs.
- **Long feedback loops**: Feedback is only gathered at the end, delaying improvements.

**About Agile**:
It eases the friction between requirements gathering and development by allowing iterative and flexible approach to software production and frequent feedback. However, it often lacks strong operational practices, leading to challenges in deployment and maintenance, leading to friction between development and deployment. Key concepts:
- **Limited Integration with Operations**: Agile focuses on development but often neglects deployment aspects.
- **Deployment as an Afterthought**: Deployment processes are not integrated into the Agile workflow, leading to manual and error-prone releases.
- **Low Feedback**: Once the software is deployed, there's weak feedback from production issues back into development cycle.

**About DevOps**:
DevOps integrates development and operations into a single team responsible for the entire software lifecycle. Unlike Waterfall (rigid, linear) or Agile (iterative but weak on ops), DevOps emphasizes automation, continuous feedback, and collaboration. It reduces friction between development and deployment by introducing automated CI/CD, monitoring, and infrastructure provisioning.

## 🇮🇹

**Waterfall**:
Crea attriti tra raccolta requisiti e sviluppo a causa della sua struttura rigida e lineare. Non consente flessibilità per adattarsi ai cambiamenti una volta definiti i requisiti. Concetti chiave:
- **Gestione dei cambiamenti rigida**: I cambiamenti sono difficili da implementare dopo la definizione dei requisiti.
- **Scoperta tardiva dei problemi**: I test avvengono tardi nel processo, portando a correzioni costose.
- **Disallineamento con le aspettative del cliente**: Ciò che viene consegnato potrebbe non corrispondere a ciò di cui il cliente ha realmente bisogno.
- **Lunghi cicli di feedback**: Il feedback viene raccolto solo alla fine, ritardando i miglioramenti.

**Agile**:
Allevia l'attrito tra raccolta requisiti e sviluppo consentendo un approccio iterativo e flessibile alla produzione software e feedback frequenti. Tuttavia, spesso manca di pratiche operative solide, portando a sfide nel deployment e nella manutenzione, creando attriti tra sviluppo e deployment. Concetti chiave:
- **Integrazione limitata con le operazioni**: Agile si concentra sullo sviluppo ma spesso trascura gli aspetti del deployment.
- **Deployment come un ripensamento**: I processi di deployment non sono integrati nel flusso di lavoro Agile, portando a rilasci manuali e soggetti a errori.
- **Basso feedback**: Una volta che il software è distribuito, c'è un debole feedback sui problemi di produzione nel ciclo di sviluppo.

**DevOps**:
Integra sviluppo e operazioni in un unico team responsabile dell'intero ciclo di vita del software. A differenza di Waterfall (rigido, lineare) o Agile (iterativo ma debole sulle operazioni), DevOps enfatizza automazione, feedback continuo e collaborazione. Riduce l'attrito tra sviluppo e deployment introducendo CI/CD automatizzato, monitoraggio e provisioning dell'infrastruttura.


---

Q2. What are the key benefits of DevOps automation, and which tools are commonly used to support it? What is the role of Jenkins in DevOps pipelines?


A2.

## 🇬🇧
DevOps automation reduces human error, speeds up development, and enables repeatable deployments.
Tools include:
| **Stage**                       | **Purpose**                                    | **Tools**                             |
| ------------------------------- | ---------------------------------------------- |---------------------------------------|
| 1. Infrastructure Provisioning  | Define and create infrastructure as code (IaC) | Terraform                             |
| 2. Configuration Management     | Install software and configure instances       | Ansible, Puppet, Chef                 |
| 3. Build and Test Automation    | Compile code, run tests, and build artifacts   | Jenkins, GitHub Actions, Gitlab CI/CD |
| 4. Deployment Automation        | Deploy apps to test/staging/prod environments  | Jenkins, Ansible, Helm, ArgoCD        |
| 5. Monitoring and Observability | Track metrics, logs, and traces                | OpenTelemetry, Grafana, Prometheus    |

Jenkins acts as a central orchestrator in CI/CD pipelines, automating builds, tests, and deployments.

## 🇮🇹 
L'automazione DevOps riduce gli errori umani, accelera lo sviluppo e consente rilasci ripetibili. 
Tra gli strumenti usati troviamo:
| **Stage**                       | **Purpose**                                    | **Tools**                             |
| ------------------------------- | ---------------------------------------------- |---------------------------------------|
| 1. Infrastructure Provisioning  | Define and create infrastructure as code (IaC) | Terraform                             |
| 2. Configuration Management     | Install software and configure instances       | Ansible, Puppet, Chef                 |
| 3. Build and Test Automation    | Compile code, run tests, and build artifacts   | Jenkins, GitHub Actions, Gitlab CI/CD |
| 4. Deployment Automation        | Deploy apps to test/staging/prod environments  | Jenkins, Ansible, Helm, ArgoCD        |
| 5. Monitoring and Observability | Track metrics, logs, and traces                | OpenTelemetry, Grafana, Prometheus    |

Jenkins agisce come orchestratore centrale nelle pipeline CI/CD automatizzando build, test e deploy.

---

Q3. What are the seven C's of DevOps, and how do they contribute to the success of DevOps practices? Does DevOps require only technical implementation, or does it also involve organizational change?

A3.

## 🇬🇧
The 7 C's are:
- **Continuous Planning**: This phase involves planning and developing the software. Development is beoken into smaller, manageable tasks, following Agile methodologies that focus on "just-in-time" requirements. It includes defining user stories and refining the product backlog (which is a prioritized list of features and tasks).
- **Continuous Inegration**: In this phase, developers frequently merge their code changes into a shared repository. Automated builds and tests are run to ensure that the new code integrates well with the existing codebase, catching issues early.
- **Continuous Testing**: This phase involves writing and running automated tests to validate the functionality, performance, and security of the software. It ensures that code changes do not introduce new bugs or regressions.
- **Continuous Delivery/Deployment**:   
**Delivery** refers to the continuous creation of an updated software artifact.  
**Deployment** refers to the automatic deployment of application code to production environments, facilitaing seamless delivery to end users. This ensures that new features and fixes can be released quickly to users.

- **Continuous Monitoring**: This phase ensures that systems and application are continuously monitored for performance, availability, and security. It involves collecting metrics, logs, and traces to identify issues in real-time. Alerts can be set to notify stakeholders of any anomalies, to allow for timely action to resolve any issues.
- **Continuous Feedback**: This crucial phase involves gathering feedback from users, stakeholders, and systems to inform future development. This feedback helps teams identify what went well, what needs improvement, driving continuous improvement in the development process.
- **Continuous Operations**: This phase ensures that systems are available 24/7. It focuses on building highly available and scalable infrastructure, through automation to minimize downtime and ensure reliability.

They ensure automation and feedback across the lifecycle. DevOps requires both technical tools and cultural/organizational shifts toward shared responsibility and collaboration.

## 🇮🇹
I 7 C di DevOps sono: 

- **Continuous Planning**: Questa fase prevede la pianificazione e lo sviluppo del software. Lo sviluppo è suddiviso in compiti più piccoli e gestibili, seguendo metodologie Agile che si concentrano su requisiti "just-in-time". Include la definizione di user stories e il perfezionamento del product backlog (una lista prioritaria di funzionalità e compiti).
- **Continuous Integration**: In questa fase, i developer integrano frequentemente le loro modifiche di codice in un repository condiviso. Vengono eseguiti build e test automatici per garantire che il nuovo codice si integri bene con il codice esistente, rilevando problemi precocemente.
- **Continuous Testing**: Questa fase prevede la scrittura e l'esecuzione di test automatici per convalidare la funzionalità, le prestazioni e la sicurezza del software. Garantisce che le modifiche al codice non introducano nuovi bug o regressioni.
- **Continuous Delivery/Deployment**:
**Delivery** si riferisce alla creazione continua di un pacchetto software aggiornato.
**Deployment** si riferisce al rilascio automatico del codice applicativo negli ambienti di produzione, facilitando una consegna senza soluzione di continuità agli utenti finali. Ciò garantisce che nuove funzionalità e correzioni possano essere rilasciate rapidamente agli utenti.

- **Continuous Monitoring**: Questa fase garantisce che i sistemi e le applicazioni siano monitorati continuamente per prestazioni, disponibilità e sicurezza. Include la raccolta di metriche, log e tracce per identificare problemi in tempo reale. Possono essere impostati avvisi per notificare le parti interessate di eventuali anomalie, consentendo un'azione tempestiva per risolvere i problemi.
- **Continuous Feedback**: Questa fase cruciale prevede la raccolta di feedback da utenti, parti interessate e sistemi per informare lo sviluppo futuro. Questo feedback aiuta i team a identificare cosa ha funzionato, cosa necessita miglioramenti, guidando il miglioramento continuo nel processo di sviluppo.
- **Continuous Operations**: Questa fase garantisce che i sistemi siano disponibili 24/7. Si concentra sulla creazione di infrastrutture altamente disponibili e scalabili, attraverso l'automazione per ridurre al minimo i tempi di inattività e garantire affidabilità.

Garantiscono automazione e feedback lungo tutto il ciclo. DevOps non è solo tecnica, ma implica anche cambiamenti culturali e organizzativi per una responsabilità condivisa.

---

Q4. What are the differences between continuous integration, continuous delivery, and continuous deployment?


A4.

## 🇬🇧

* **CI**: Developers merge code frequently; builds and tests run automatically.
* **CD (Delivery)**: Artifacts are automatically created and tested, ready for manual deployment.
* **CD (Deployment)**: Artifacts are automatically deployed to production.

## 🇮🇹

* **CI**: I developer integrano codice spesso; build e test vengono eseguiti automaticamente.
* **CD (Delivery)**: I pacchetti vengono creati e testati automaticamente, pronti per il deploy manuale.
* **CD (Deployment)**: I pacchetti vengono rilasciati automaticamente in produzione.

---

Q5. What are DORA metrics, and how can an organization improve its performance based on these metrics over time?


A5.

## 🇬🇧
DORA metrics, developed by DevOps Research and Assessment, are: 
- Cycle Time (Lead time for change)
- Deployment Frequency
- Change Failure Rate
- Time to Restore Service

How to improve:
- Monitor the metrics regularly.
- **Reduction of codebase size**: Reducing size/complexity of codebase improves **Change Failure Rate**, **Time to Restore Service**, and **Deployment Frequency**.
- **Automate CI/CD pipelines**: Automation reduces manual errors, accelerates testing and deployment processes, improcing **Deployment Frequency** and **Lead Time for Change**.
- **Reduction of PR size**: smaller Pull Requests are easier to review, merge and test, reducing the risk of issues and speeding up the overall deployment process, which impacts on **Lead Time for Change**.
- **Implement Deployment Strategies**: Strategies reduce the impact of failed releases by gradually introducing changes to a small subset of users before full rollout, improving **Change Failure Rate** and **Time to Restore Service**.

## 🇮🇹
Le metriche DORA, sviluppate da DevOps Research and Assessment, sono:
- Cycle Time (Lead time for change)
- Deployment Frequency
- Change Failure Rate
- Time to Restore Service

Come migliorare:
- Monitorare regolarmente le metriche.
- **Riduzione della dimensione del codice**: Ridurre la dimensione/complessità del codice migliora **Change Failure Rate**, **Time to Restore Service** e **Deployment Frequency**.
- **Automatizzare le pipeline CI/CD**: L'automazione riduce gli errori manuali, accelera i processi di test e deployment, migliorando **Deployment Frequency** e **Lead Time for Change**.
- **Riduzione della dimensione delle PR**: Pull Request più piccole sono più facili da rivedere, unire e testare, riducendo il rischio di problemi e accelerando il processo di deployment, impattando su **Lead Time for Change**.
- **Implementare strategie di deployment**: Le strategie riducono l'impatto dei rilasci falliti introducendo gradualmente le modifiche a un piccolo gruppo di utenti prima del rilascio completo, migliorando **Change Failure Rate** e **Time to Restore Service**.
---

Q6. What deployment strategies are used to improve DORA metrics, and how do they operate?


A6.

## 🇬🇧

* **Blue-Green**: Uses two identical environments, one active (blue) and one idle (green). The new version is deployed to the idle environment and tested. Once verified, all traffic is instantly switched to it. This minimizes downtime and makes rollback simple by redirecting traffic back to the old environment if needed. It's ideal for major upgrades.

* **Rolling**: Gradually replaces old instances of the application with new ones across the infrastructure. All users are progressively exposed to the new version as the update spreads. This approach avoids full downtime, but rollbacks are more involved since already updated instances must be reverted individually. It works well in stable environments with low rollback risk.

* **Canary**: Starts by releasing the new version to a small subset of users. If no issues are observed, the rollout continues to a larger user base. This minimizes risk by limiting initial exposure and allows quick rollback if needed. It's especially useful when introducing potentially disruptive changes or testing new features under real-world conditions.

## 🇮🇹

* **Blue-Green**: Utilizza due ambienti identici, uno attivo (blue) e uno inattivo (green). La nuova versione viene distribuita nell'ambiente inattivo e testata. Una volta verificata, tutto il traffico viene reindirizzato istantaneamente a esso. Ciò riduce al minimo i tempi di inattività e rende il rollback semplice reindirizzando il traffico all'ambiente vecchio se necessario. È ideale per aggiornamenti importanti.

* **Rolling**: Rimpiazza gradualmente le vecchie istanze dell'applicazione con nuove in tutta l'infrastruttura. Tutti gli utenti sono esposti progressivamente alla nuova versione nel corso dell'aggiornamento. Questo approccio evita tempi di inattività completi, ma i rollback sono più complessi poiché le istanze già aggiornate devono essere ripristinate singolarmente. Funziona bene in ambienti stabili con basso rischio di rollback.

* **Canary**: Si inizia rilasciando la nuova versione a un piccolo gruppo di utenti. Se non si osservano problemi, il rollout continua a una base utenti più ampia. Ciò riduce al minimo il rischio limitando l'esposizione iniziale e consente un rollback rapido se necessario. È particolarmente utile quando si introducono modifiche potenzialmente dirompenti o si testano nuove funzionalità in condizioni reali.

---

Q7. How does DevSecOps incorporate security into the CI/CD pipeline, and what are its main advantages? What challenges are commonly faced when adopting DevSecOps in an organization?

A7.

## 🇬🇧
DevSecOps integrates security checks into every pipeline stage, instrad of the traditional approach of adding security at the end. Furthermore, security tools have tipically operated in isolation, with each application security test focusing on a single application, often just examining the code. This made it difficult to gain an organization-wide view of security risks or to assess the overall security posture of the organization. 

DevSecOps addresses this by embedding security practices into the CI/CD pipeline across pre-production (development, testing, staging) and prodution (operation) environments. By adopting DevSecOps, teams can release higher quality software faster and detect and respond to software vulnerabilities in production more quickly.

- **Advantages**: early vulnerability detection, faster fixes, security ownership by all teams. 

- **Challenges**: cultural resistance of well-established teams, selecting the right tools and integrating them into existing workflows.

## 🇮🇹
DevSecOps integra i controlli di sicurezza in ogni fase della pipeline, invece dell'approccio tradizionale di aggiungere la sicurezza alla fine. Inoltre, gli strumenti di sicurezza hanno tipicamente operato in isolamento, con ogni test di sicurezza delle applicazioni focalizzato su un'unica applicazione, spesso esaminando solo il codice. Ciò rendeva difficile ottenere una visione a livello organizzativo dei rischi di sicurezza o valutare la postura di sicurezza complessiva dell'organizzazione.

DevSecOps affronta questo problema integrando le pratiche di sicurezza nella pipeline CI/CD attraverso gli ambienti pre-produzione (sviluppo, test, staging) e produzione (operazioni). Adottando DevSecOps, i team possono rilasciare software di qualità superiore più rapidamente e rilevare e rispondere alle vulnerabilità del software in produzione più velocemente.

- **Vantaggi**: rilevamento precoce delle vulnerabilità, correzioni più rapide, responsabilità della sicurezza da parte di tutti i team.
- **Sfide**: resistenza culturale di team ben consolidati, selezione degli strumenti giusti e integrazione nei flussi di lavoro esistenti.


---

Q8. What is GitHub Actions and how does it work? What are five widely used CI/CD tools in the industry, and what are their key characteristics?


A8.

## 🇬🇧
GitHub Actions is a CI/CD tool that allows users to automate workflows directly within GitHub repositories. It uses YAML files to define workflows, which can include jobs that run in parallel or sequentially based on dependencies defined with the keyword `needs`. 

5 tools:

* **GitHub Actions**: Cloud-native, simple GitHub integration.
* **Jenkins**: Self-hosted, highly customizable.
* **GitLab CI**: Built into GitLab, powerful with YAML.
* **CircleCI**: Cloud-based, fast parallel jobs.
* **Travis CI**: Easy GitHub integration, less used now.

## 🇮🇹
GitHub Actions è uno strumento CI/CD che consente agli utenti di automatizzare i flussi di lavoro direttamente all'interno dei repository GitHub. Utilizza file YAML per definire i flussi di lavoro, che possono includere job che vengono eseguiti in parallelo o in sequenza in base alle dipendenze definite con la parola chiave `needs`.

5 strumenti:

* **GitHub Actions**: Nativo su GitHub, semplice.
* **Jenkins**: Self-hosted, altamente personalizzabile.
* **GitLab CI**: Integrato in GitLab, potente con YAML.
* **CircleCI**: Cloud, job paralleli veloci.
* **Travis CI**: Integrazione GitHub facile, meno usato oggi.

---

Q9. How do Infrastructure as Code (IaC) principles enhance DevOps practices?

A9.
## 🇬🇧
IaC allows infrastructure to be defined in code, versioned, and 
deployed automatically. It improves reproducibility, consistency, and auditability. It can have a declarative or imperative approach. In the first we describe a desired end state, while in the second we define a sequence of commands to achieve that state. 
Tool: Terraform.

## 🇮🇹
IaC consente di definire l'infrastruttura come codice, versionarla e distribuirla automaticamente. Migliora riproducibilità, coerenza e tracciabilità. Può avere un approccio dichiarativo o imperativo. Nel primo descriviamo uno stato finale desiderato, mentre nel secondo definiamo una sequenza di comandi per raggiungere quello stato.
Tool: Terraform.

---

Q10. What is configuration drift, how does it relate to configuration as code, and what are the main tools used to address it?


A10.

## 🇬🇧
Configuration as Code (CaC) is a practice that allows teams to manage and provision infrastructure using code, ensuring consistency and repeatability. 

Configuration drift occurs when actual infrastructure deviates from the intended state. 
Causes:
- Manual changes made directtly on systems (via SSH)
- Failed or partial deployments
- Unmanaged updates or patches
- Lack of version control in configuration files

Why is it a problem?
- Leads to inconsistencies between servers
- Makes debugging and troubleshooting difficult
- Violates security baselines and udit requirements.
- Can lead to compiance issues

How to detect configuration drift:
- Manuallly monitor: extremely time-consuming and error-prone.

- Scan for compliance. While not as tedious as the first level, this still requires a certain level of interaction to create administrative credentials for the tool to scan with, as well as someone to schedule or run the scans when required and remediate the results. This is typically done once a month or once a quarter to try to get ahead of the audit process.

- Monitor all systems in a near real-time manner. Here is required the presence of a lightweight agent that can monitor/update the system continuously.

Tools: DriftCTL, OpenTelemetry, Grafana, Prometheus.

## 🇮🇹

La Configuration as Code (CaC) è una pratica che consente ai team di gestire e fornire infrastrutture utilizzando il codice, garantendo coerenza e ripetibilità.

La configuration drift si verifica quando l'infrastruttura effettiva si discosta dallo stato previsto.
Cause:
- Modifiche manuali apportate direttamente sui sistemi (tramite SSH)
- Rilasci parziali o falliti
- Aggiornamenti o patch non gestiti
- Mancanza di controllo di versione nei file di configurazione

Perché è un problema?
- Porta a incoerenze tra i server
- Rende difficile il debug e la risoluzione dei problemi
- Viola le basi di sicurezza e i requisiti di audit
- Può portare a problemi di conformità

Come rilevare la configuration drift:
- Monitoraggio manuale: estremamente dispendioso in termini di tempo e soggetto a errori.
- Scansione per conformità. Anche se non così noiosa come il primo livello, richiede comunque un certo livello di interazione per creare credenziali amministrative per lo strumento da utilizzare, oltre a qualcuno che pianifichi o esegua le scansioni quando necessario e risolva i risultati. Questo viene tipicamente fatto una volta al mese o una volta al trimestre per cercare di anticipare il processo di audit.
- Monitoraggio di tutti i sistemi in un modo quasi in tempo reale. Qui è richiesto la presenza di un agente leggero che possa monitorare/aggiornare il sistema continuamente.
Strumenti: DriftCTL, OpenTelemetry, Grafana, Prometheus.