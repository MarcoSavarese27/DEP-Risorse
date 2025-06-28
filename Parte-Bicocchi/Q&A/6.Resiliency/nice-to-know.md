# Partizionamento dei dati

Il partizionamento implica la suddivisione dei dati in subset, detti **shards** e li distribuisce (nel nostro caso) su più nodi Redis. Abbiamo due strategie principali per il partizionamento:
- **Range-based partitioning**: si definiscono range di chiavi basati su criteri specifici(prefisso o valori della chiave) e si assegna ogni range a un nodo specifico. Approccio utile quando si riesce a prevedere la distribuzione delle chiavi nel dataset, ma che mostra le sue debolezze quando la distribuzione non è uniforme.
- **Hash-based partitioning**: si applica una funzione di hash alla chiave per determinare il nodo a cui assegnarla. Questo approccio è più robusto e distribuisce uniformemente le chiavi, ma può rendere difficile la gestione dei dati se un nodo viene rimosso o aggiunto (bisogna rehashare le chiavi).
- **Consistent hashing**: una variante del partizionamento basato su hash che consente di ridurre il numero di chiavi da rehashare quando un nodo viene aggiunto o rimosso. Fa uso di nodi virtuali. Ogni nodo fisico è associato a multipli punti dell'hash ring, e quando un nodo viene aggiunto o rimosso, solo le chiavi associate ai nodi virtuali interessati devono essere rehashate. 

# Cache invalidation

La **cache invalidation** è il processo di assicurare che i dati memorizzati nella cache rimangano accurati rimuovendo le informazioni obsolete o non aggiornate. È considerato un problema difficile a causa della tensione tra **prestazioni** e **coerenza**: mantenere un recupero rapido dei dati (grande cache, poche invalidazioni) garantendo al contempo che i dati siano aggiornati (cache più piccola, invalidazioni frequenti).

Nei sistemi distribuiti, la difficoltà aumenta a causa della loro **complessità**: più nodi, stati di dati variabili e potenziali guasti di rete rendono difficile coordinare l'invalidazione della cache. Garantire la coerenza dei dati su tutti i nodi ottimizzando le prestazioni è un equilibrio delicato.

Strategie chiave:
* **Basata sul tempo**: Le voci della cache scadono dopo un periodo stabilito. Questo è il metodo più semplice, ma può portare a servire dati obsoleti se i tempi di scadenza non sono correttamente regolati.
* **Basata sugli eventi**: Le voci della cache vengono invalidate quando si verifica un evento specifico o una modifica nella fonte dei dati, garantendo coerenza. Tuttavia, è più complesso implementare e gestire gli eventi.
* **Basata sulla dimensione**: Quando la cache supera il suo limite di memoria, le voci vengono rimosse. Sebbene utile per l'efficienza della memoria, questo metodo può rimuovere dati ancora rilevanti.
* **Basata sulla versione**: I dati sono contrassegnati con versioni o timestamp, e la cache viene invalidata quando è disponibile una nuova versione. Bilancia freschezza e prestazioni, ma può diventare complicato con aggiornamenti frequenti dei dati.

