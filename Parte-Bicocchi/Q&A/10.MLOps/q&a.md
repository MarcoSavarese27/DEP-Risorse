# Q&A

## Q1
What is feedback delay, and why does it complicate real-time model performance evaluation?

## A1.

Il feedback delay si riferisce a un gap temporale tra la predizione del modello e la ricezione del vero risultato. Questo ritardo può complicare il monitoraggio delle prestazioni del modello in tempo reale, poiché le metriche di valutazione non sono immediatamente disponibili.

## Q2 
What is data drift, and how can it impact the performance of a deployed ML model?

## A2
Con il termine data drift ci riferiamo a un cambio nelle proprietà statistiche e nelle caratteristiche dei dati di input nel tempo. La causa di questo fenomeno può essere attribuita alla deviazione dei dati incontrati in produzione rispetto a quelli utilizzati durante la fase di addestramento del modello.

**Impatto**:
- Il modello fa fatica a fare predizioni accurate quando la distribuzione dei dati cambia.
- I modelli sono designati per funzionare su dati con caratteristiche simili a quelli usati per l'addestramento, quindi falliscono se esposti a dati nuovi e significativamente diversi.

## Q3
How can concept drift and prediction drift impact the performance of a machine learning model in production, and what strategies can be used to detect them?

## A3

Il **concept drift** si riferisce a un cambiamento della relazione tra le features in ingresso e l'output del modello, mentre il **prediction drift** indica un cambiamento nella distribuzione delle predizioni del modello. Entrambi possono influenzare negativamente le prestazioni del modello in produzione.
**Strategie per la rilevazione**:
- Monitoraggio delle metriche di performance del modello nel tempo. Cercare shift nei valori medi, mediani, varianze e quantili delle predizioni. 
- Utilizzo di tecniche statistiche per confrontare le distribuzioni dei dati di input e delle predizioni nel tempo. Un esempio possono essere i test di Kolmogorov-Smirnov o il Chi-Squared
- Metriche di distanza che misutino quanto divergono le distribuzioni delle predizioni attuali rispetto a quelle passate, come la distanza di Jensen-Shannon o la distanza di Wasserstein.
- Rule-based checks che fungono da buone euristiche di alerting per notificare quando le predizioni del modello iniziano a deviare da quelle attese e che possano essere punto di partenza per un'analisi più approfondita.

## Q4
Why is maintaining data quality crucial for ML 
models, and how can poor data quality lead to misleading model predictions over time?

## A4

Mantenere la qualità dei dati è cruciale per i modelli di machine learning perché in casi in cui dati di bassa qualità vengono utilizzati (dati corrotti, incompleti o incorretti) per l'addestramento, il modello può apprendere schemi errati o non rappresentativi. Questo porta a predizioni fuorvianti e a una diminuzione delle prestazioni del modello nel tempo.

## Q5
What are the different MLOps maturity levels, and how do they reflect an organization's ability to automate and manage the ML lifecycle effectively?

## A5
I livelli di maturità MLOps sono classificazioni che riflettono la capacità di un'organizzazione di automatizzare e gestire efficacemente il ciclo di vita del machine learning. Partendo dal più basso, abbiamo:

1. **Livello 0**; Nessuna automazione, i processi sono manuali, non abbiamo versioning, nessuna pipeline di CI/CD, ambienti inconsistenti (modelli che vanno bene in development ma non in produzione) e assenza totale di monitoraggio.
2. **Livello 1**; Automazione parziale, alcuni processi sono automatizzati, si esegue un certo livello di monitoraggio in produzione, ma il retraining avviene ad intervalli fissi piuttosto che in risposta a eventi specifici.
3. **Livello 2**; Automazione avanzata, i processi sono ben definiti e documentati, si utilizza il versioning dei modelli e dei dati, le pipeline di CI/CD sono implementate, e il monitoraggio e alerting sono continui. Oltretutto le pipeline e i servizi di inferenza sono scalabili orizzontalmente usando training distribuito.

## Q6

Give an example of a rule-based check that could be useful for monitoring model predictions in production.

## A6

Un esempio di rule-based check utile per il monitoraggio delle predizioni del modello in produzione potrebbe essere:
- Un valore che va al di fuori di un certo intervallo atteso.
- Comparsa di un nuovo valore di categorizzazione.
- Un numero numero di valori di una certa feature superano il loro range definito di minimo e massimo.

## Q7
How does the concept of Pipeline-As-Code support Continuous Integration and Continuous Deployment (CI/CD) in machine learning workflows?
## A7
RISPOSTA ALLA DOMANDA NON PRESENTE NELLE SLIDES DEL CORSO, LA SEGUENTE RISPOSTA SARÀ GENERATA DA UN LLM E OVVIAMENTE REVISIONATA DA ME.

Con **pipeline-as-Code** si intende la pratica di definire le pipeline di machine learning attraverso il codice, permettendo così di versionare, testare e distribuire le pipeline in modo simile al codice applicativo. Questo approccio supporta CI/CD nel workflow di machine learning in quanto:
- Permette di integrare le pipeline di machine learning nei processi di sviluppo software esistenti.
- Facilita l'automazione del processo di build, test e deployment delle pipeline.
- Garantisce che le modifiche alle pipeline siano tracciabili e riproducibili, migliorando la collaborazione tra i team di sviluppo e data science.


## Q8
List and briefly describe the key stages in the machine learning development lifecycle.
## A8
Gli stage principali nel ciclo di vita dello sviluppo del machine learning includono:
1. **Data preparation**: Raccolta, integrazione, pulizia, normalizzazione, aggregazione, conversione, discretizzazione e analisi dei dati per renderli pronti per l'addestramento del modello.
2. **Model development**: Sviluppo del modello attraverso la
- Selezione del modello (Supervised, Unsupervised, Reinforcement, Deep Learning)
- Addestramento: separazione del dataset in training (60-80%, porzione di dati usata per aggiustare i parametri interni del modello), test (10-20%, porzione di dati usata per valutare le prestazioni del modello) e validation set (10-20%, porzione di dati usata per ottimizzare gli iperparametri del modello). Tutto questo viene fatto per evitare l'overfitting, cioè quando il modello si adatta troppo ai dati di addestramento e non generalizza bene su dati nuovi.
- Validazione del modello: Valutazione delle prestazioni del modello utilizzando metriche appropriate. Una tecnica è la K-fold cross validation, che divide il dataset in K parti e addestra il modello K volte, ogni volta utilizzando una parte diversa come test set e le altre come training set. Riduce il bias di valutazione, massimizza l'uso dei dati e fornisce una stima più robusta delle prestazioni del modello.
3. **Model deployment**: Distribuzione del modello in un ambiente di produzione, dove può essere utilizzato per fare predizioni su nuovi dati.
Questa fase si compone di:
- **Esportazione del modello** in un formato compatibile con il framework di inferenza scelto (es. ONNX, TensorFlow SavedModel, PyTorch Script).
- **Wrap della logica di inferenza in un servizio web** che accetti le richieste degli utenti mediante richieste HTTP, preprocessi i dati di input(seguendo le stesse trasformazioni applicate durante l'addestramento) e restituisca le predizioni.
- **Deploy del servizio**, containerizzazione con docker, deploy su un server, cloud service o edge device.
- **Uso di load balancers e autoscaling** per gestire il traffico e garantire la disponibilità del servizio.
5. **Update** o retraining del modello, per aggiornarlo con i nuovi trend di dati e evitare il concept drift. Implementare retraining automatico o continual learning.

## Q9
What is the purpose of a feature store in an MLOps system, and how does it differ from a data warehouse?

## A9
Un feature store è un sistema progettato per gestire e servire le features utilizzate nei modelli di machine learning. La sua funzione principale è quella di centralizzare la gestione delle features, rendendole facilmente accessibili per l'addestramento e l'inferenza dei modelli.
A differenza di un data warehouse, che è principalmente un sistema di archiviazione e gestione dei dati, un feature store si concentra sulla creazione, gestione e distribuzione delle features. Le features sono trasformazioni dei dati grezzi che possono essere utilizzate per addestrare i modelli di machine learning, mentre un data warehouse contiene dati in forma più grezza e non necessariamente pronti per l'uso diretto nei modelli.

## Q10
Explain the role of experiment tracking and name two popular tools used for it.

## A10
L'**experiment tracking** è il processo di registrazione e monitoraggio degli esperimenti di machine learning, inclusi i parametri del modello, le metriche di performance e le versioni dei dati. Questo è fondamentale per comprendere come le modifiche ai modelli e ai dati influenzano le prestazioni, facilitando la riproducibilità e la collaborazione tra i team.
Due strumenti popolari per l'experiment tracking sono:
- **MLflow**: Un framework open-source che consente di gestire il ciclo di vita del machine learning, inclusa la tracciabilità degli esperimenti, la gestione dei modelli e la distribuzione.
- **Weights & Biases**: Una piattaforma che offre funzionalità avanzate per il tracciamento degli esperimenti, la visualizzazione delle metriche e la gestione dei modelli, con un'interfaccia utente intuitiva e integrazioni con vari framework di machine learning.