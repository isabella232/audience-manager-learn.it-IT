---
title: Migrazione dell’implementazione Audience Manager del sito dall’inoltro lato client a DIL lato server
description: Scopri come migrare l’implementazione di Audience Manager (AAM) del tuo sito dall’inoltro lato client a DIL lato server. Questa esercitazione si applica se disponi sia di AAM che di Adobe Analytics e invii gli hit dalla pagina a AAM utilizzando il codice DIL (Data Integration Library) e invii gli hit dalla pagina ad Adobe Analytics.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# Migrazione dell’implementazione Audience Manager del sito dall’inoltro lato client a DIL lato server {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Questa esercitazione si applica se disponi sia di Adobe Audience Manager (AAM) che di Adobe Analytics e stai attualmente inviando un hit dalla pagina a AAM utilizzando DIL ([!DNL Data Integration Library]) e invia anche un hit dalla pagina ad Adobe Analytics. Poiché hai entrambe queste soluzioni e poiché entrambe fanno parte di Adobe Experience Cloud, hai l&#39;opportunità di seguire la best practice per attivare l&#39;inoltro lato server, che consente alle [!DNL Analytics] server di raccolta dati per inoltrare i dati di analisi del sito in tempo reale ad Audience Manager, invece di far sì che il codice lato client invii un hit aggiuntivo dalla pagina a AAM. Questa esercitazione descrive i passaggi necessari per passare dall’implementazione DIL lato client precedente al metodo di inoltro lato server più recente.

## Lato client (DIL) e lato server {#client-side-dil-vs-server-side}

Quando si confrontano e si confrontano questi due metodi per ottenere i dati di Adobe Analytics in AAM, può essere prima utile visualizzare le differenze nell’immagine seguente:

![lato client a lato server](assets/client-side_vs_server-side_aam_implementation.png)

### Implementazione DIL lato client {#client-side-dil-implementation}

Se utilizzi questo metodo per ottenere i dati di Adobe Analytics in AAM, hai due hit provenienti dalle pagine web: Uno che va a [!DNL Analytics]e una AAM (dopo aver copiato il [!DNL Analytics] nella pagina Web. [!UICONTROL Segments] vengono restituiti da AAM alla pagina, dove possono essere utilizzati per la personalizzazione e così via. Questa viene considerata un’implementazione legacy e non è più consigliata.

Oltre al fatto che questo non è conforme alle best practice, gli svantaggi di utilizzare questo metodo includono:

* Due hit provenienti dalla pagina invece di uno solo
* l&#39;inoltro lato server è necessario per la condivisione in tempo reale dei tipi di pubblico AAM [!DNL Analytics], pertanto le implementazioni lato client non consentono questa funzione (e potenzialmente altre funzionalità in futuro)

È consigliabile passare a un metodo di inoltro lato server per l&#39;implementazione AAM.

### Implementazione dell&#39;inoltro lato server {#server-side-forwarding-implementation}

Come mostrato nell&#39;immagine precedente, un hit viene dalla pagina web ad Adobe Analytics. [!DNL Analytics] quindi inoltra tali dati a AAM in tempo reale e i visitatori vengono valutati in caratteristiche AAM e [!UICONTROL segments], proprio come se l’hit fosse arrivato direttamente dalla pagina.

[!UICONTROL Segments] vengono restituiti sullo stesso hit in tempo reale per [!DNL Analytics], che inoltra la risposta alla pagina Web per la personalizzazione e così via.

Il passaggio a Server-Side Forwarding non comporta alcun rallentamento. L&#39;Adobe raccomanda vivamente a chiunque abbia sia Audience Manager che [!DNL Analytics] utilizza questo metodo di implementazione.

## Sono disponibili due attività principali {#you-have-two-main-tasks}

C&#39;è un bel po&#39; di informazioni su questa pagina, ed è tutto importante, ovviamente. Tuttavia, **tutto si riduce a due cose principali che è necessario fare**:

1. Modifica il codice dal codice DIL lato client al codice di inoltro lato server
1. Capovolgere l&#39;interruttore nella [!DNL Analytics] [!DNL Admin Console] per avviare l&#39;effettivo inoltro dei dati (per [!UICONTROL report suite])

Se salti una di queste attività, l&#39;inoltro lato server non funzionerà correttamente. Sono stati aggiunti passaggi e dati aggiuntivi a questo documento per aiutarti a eseguire correttamente questi due passaggi per la configurazione.

## Opzioni di implementazione {#implementation-options}

Quando si passa dall&#39;inoltro lato client all&#39;inoltro lato server, una delle attività che si avrà è la modifica del codice al nuovo codice di inoltro lato server. Questa operazione viene eseguita utilizzando una delle seguenti opzioni:

* Tag Adobe Experience Platform : opzione di implementazione consigliata da Adobe per le proprietà Web. Noterai che si tratta di un’attività facile, in quanto i tag Platform hanno svolto tutto il lavoro necessario.
* Nella pagina puoi anche inserire il nuovo codice SSF direttamente nel `doPlugins` all&#39;interno della `appMeasurement.js` file , se non utilizzi (ancora) Adobe Launch
* Altri gestori di tag : questi possono essere trattati come l’opzione precedente (Sulla pagina), in quanto il codice SSF verrà comunque inserito in `doPlugins`, ovunque l’altro gestore di tag memorizzi l’ [!DNL AppMeasurement] codice

Di seguito sono elencate le seguenti opzioni _Aggiornamento del codice_ sezione .

## Passaggi di implementazione {#implementation-steps}

I passaggi seguenti descrivono l’implementazione.

### Passaggio 0: Prerequisito: Servizio Experience Cloud ID (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

Il prerequisito principale per lo spostamento all&#39;inoltro lato server è l&#39;implementazione del servizio Experience Cloud ID. Questo viene fatto molto facilmente se si utilizza Experience Platform Launch, nel qual caso si installa semplicemente l&#39;estensione ECID e farà il resto.

Se utilizzi un TMS non Adobe o nessun TMS, implementa ECID per l’esecuzione **prima** qualsiasi altra soluzione di Adobe. Consulta la sezione [Documentazione ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html) per ulteriori dettagli. L’unico altro prerequisito riguarda le versioni del codice, in modo da applicare semplicemente le versioni più recenti del codice nei passaggi seguenti, sarà bene.

>[!NOTE]
>
>Leggere l&#39;intero documento prima di implementarlo. La sezione &quot;Timing&quot; riportata di seguito contiene informazioni importanti su *quando* devi implementare ogni pezzo, incluso ECID (se non è ancora implementato).

### Passaggio 1: Registra le opzioni attualmente utilizzate dal codice DIL {#step-record-currently-used-options-from-dil-code}

Quando ti prepari a passare dal codice DIL lato client all’inoltro lato server, il primo passaggio consiste nell’identificare tutte le operazioni che esegui con il codice DIL, incluse le impostazioni personalizzate e i dati inviati a AAM. Gli aspetti da considerare e da considerare includono:

* Normale [!DNL Analytics] variabili, utilizzando `siteCatalyst.init` Modulo DIL - Non devi preoccuparti di questo, perché il suo compito è solo quello di inviare il normale [!DNL Analytics] e questo accade semplicemente avendo abilitato l&#39;inoltro lato server.
* Sottodominio partner - Nel `DIL.create` funzione, prendere nota del `partner` parametro . Questo è noto come &quot;sottodominio partner&quot; o talvolta come &quot;ID partner&quot; e sarà necessario quando inserisci il nuovo codice di inoltro lato server.
* [!DNL Visitor Service Namespace] - Conosciuto anche come &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID],&quot; è necessario anche questo quando si imposta il nuovo codice di inoltro lato server. Prendetene nota.
* containerNSID, uuidCookie e altre opzioni avanzate - Prendi nota di eventuali opzioni avanzate aggiuntive che stai utilizzando in modo da poterle impostare anche nel codice di inoltro lato server.
* Variabili di pagina aggiuntive - Se altre variabili vengono inviate a AAM dalla pagina (in aggiunta alla normale [!DNL Analytics] variabili gestite da siteCatalyst.init), è necessario prenderne nota in modo che possano essere inviate tramite inoltro lato server (spoiler: tramite [!DNL contextData] variabili).

### Passaggio 2: Aggiorna il codice {#step-updating-the-code}

In [Opzioni di implementazione](#implementation-options) (in precedenza), vengono fornite più opzioni su come e dove implementare l&#39;inoltro lato server. Affinché questa sezione sia efficace, dobbiamo suddividerla in queste sezioni (con due di esse combinate). Vai al metodo di questa sezione che descrive meglio le tue esigenze.

#### Tag Adobe Experience Platform {#launch-by-adobe}

Guarda il video seguente per scoprire come spostare in Experience Platform Launch le opzioni di implementazione dal codice DIL lato client all’inoltro lato server.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;Sulla pagina&quot; o gestore di tag non Adobe {#on-the-page-or-non-adobe-tag-manager}

Guarda il video seguente per scoprire come spostare le opzioni di implementazione dal codice DIL lato client all’inoltro lato server in [!DNL AppMeasurement] che risiedono in un file o in un sistema di gestione dei tag non Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Passaggio 3: Abilitazione dell&#39;inoltro (per [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Finora in questa esercitazione abbiamo dedicato tutto il nostro tempo a passare dal codice DIL lato client all’inoltro lato server. Va bene, perché è la parte più difficile. Questa sezione, anche se è super facile, è importante quanto l&#39;aggiornamento del codice. Questo video illustra come capovolgere l’interruttore che consente l’effettivo inoltro dei dati da Analytics ad Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Come indicato nel video, ricorda che ci vorranno fino a 4 ore per consentire l’implementazione completa dell’inoltro sul back-end dell’Experience Cloud.

## Tempi {#timing}

Come promemoria, esistono due attività principali per passare da DIL lato client all’inoltro lato server:

1. Aggiornamento del codice
1. Capovolgimento dell&#39;interruttore nel [!DNL Analytics] [!DNL Admin Console]

Ma la domanda è, quale fai prima? Importa? Ok, scusate, erano due domande. Ma le risposte sono.. dipende, e sì, è *può* importante. Com&#39;è vago? Distruggiamola. Ma prima una domanda aggiuntiva che può venire fuori se siete una grande organizzazione con numerosi siti: Devo fare tutto immediatamente? Quella è un po&#39; più facile. No. Potete farlo pezzo per pezzo.

### Un po&#39; più profondo {#a-little-deeper-dive}

Il motivo per cui la tempistica e l&#39;ordine contano è a causa di come l&#39;inoltro _davvero_ lavori, riassunti nei seguenti elementi tecnici:

* Se hai implementato il servizio Experience Cloud ID (ECID) e il pulsante nella [!DNL Analytics] [!DNL Admin Console] (&quot;l&#39;interruttore&quot;) è attivato, i dati verranno inoltrati da [!DNL Analytics] AAM, anche se non hai ancora aggiornato il codice.
* Se non hai implementato ECID, i dati non verranno inoltrati, anche se hai attivato l&#39;interruttore, e disponi del codice di inoltro lato server.
* Il codice di inoltro lato server (nei tag Platform o nella pagina) gestisce davvero la risposta ed è necessario per completare la migrazione.
* L&#39;interruttore di inoltro lato server è abilitato dalla [!UICONTROL report suite], ma il codice viene gestito dalla proprietà nei tag Platform o dal [!DNL AppMeasurement] se non utilizzi i tag Platform.

### Best practice {#best-practices}

Sulla base di questi dettagli tecnici, ecco le raccomandazioni per il momento in cui eseguire e quando:

#### Se NON hai ancora implementato ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Inverti l&#39;interruttore [!DNL Analytics] per ciascuno [!UICONTROL report suite] che verrà attivato per l&#39;inoltro lato server.

   1. L’inoltro non inizia ancora perché non hai ECID.

1. Per sito, aggiorna il codice da DIL lato client all’inoltro lato server (potrebbe essere nei tag Platform) o sulla pagina, come descritto in un’altra sezione precedente).

   1. L’inoltro ora scorre (come hai aggiunto ECID) e dovresti anche ricevere una risposta JSON corretta al tuo [!DNL Analytics] beacon (per ulteriori informazioni, consulta la sezione Convalida e risoluzione dei problemi di seguito).

#### Se ECID è stato implementato {#if-you-do-have-ecid-implemented}

1. Prepara e pianifica in modo da essere pronto ad aggiornare il codice dall’inoltro lato server a DIL [!UICONTROL report suite] che verrà attivato per l&#39;inoltro lato server:

   1. Inverti l&#39;interruttore [!DNL Analytics] per abilitare l&#39;inoltro lato server.

      1. L&#39;inoltro verrà avviato perché l&#39;ECID è abilitato.
   1. Al più presto, aggiorna il codice da DIL lato client all’inoltro lato singolo (potrebbe essere nei tag di Platform o sulla pagina, come descritto in un’altra sezione precedente).

      1. Dovresti ricevere una risposta JSON appropriata al tuo [!DNL Analytics] (vedi [Convalida e risoluzione dei problemi](#validation-and-troubleshooting) per ulteriori informazioni).


>[!NOTE]
>
>È importante eseguire questi due passaggi il più vicino possibile tra loro, perché tra i passaggi 1 e 2 di cui sopra, si avrà una duplicazione dei dati in AAM. In altre parole, l&#39;inoltro lato singolo avrà iniziato l&#39;invio di dati da [!DNL Analytics] AAM e poiché il codice di DIL è ancora nella pagina, ci sarà anche un hit che va direttamente dalla pagina a AAM, raddoppiando in tal modo i dati. Non appena aggiorni il codice dall’inoltro lato server da DIL, questo verrà eliminato.

>[!NOTE]
>
>Se preferisci avere una piccola discrepanza nei dati invece di una piccola duplicazione dei dati, puoi cambiare l’ordine dei passaggi 1 e 2 precedenti. Lo spostamento del codice dall’inoltro lato server a DIL interromperebbe il flusso di dati in AAM fino a quando non si è riusciti a capovolgere lo switch per attivare l’inoltro lato server per il [!UICONTROL report suite]. In genere, i clienti preferiscono raddoppiare i dati anziché lasciarli alle caratteristiche e [!UICONTROL segments].

#### Tempi di migrazione in presenza di molti siti e [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Questo argomento è trattato brevemente nelle sezioni precedenti, in quanto la strategia principale può essere riassunta come segue:

Migrazione di un sito/[!UICONTROL report suite] (o gruppo di siti/[!UICONTROL report suites]) alla volta.

Tuttavia, questo può diventare un po&#39; complicato in base ad alcuni possibili scenari:

* Hai un sito che contiene diversi elementi distinti [!UICONTROL report suites]
* Hai un [!UICONTROL report suite] che include diversi siti (come un [!UICONTROL report suite])
* Puoi utilizzare una proprietà Tag Platform per coprire più siti
* Hai diversi team di sviluppo per siti diversi

A causa di questi elementi, può diventare un po&#39; complicato. Le migliori cose che posso suggerire sono:

* Prenditi del tempo per elaborare una strategia per la migrazione all’inoltro lato server, in base alle informazioni precedentemente illustrate
* In base al fatto che una singola proprietà nei tag Platform (o un singolo [!DNL AppMeasurement] file) in genere è mappato a 1 o 2 valori distinti [!UICONTROL report suites], sarà probabilmente possibile creare un piano che funzioni su questi gruppi distinti uno per uno, aggiornando l&#39;azienda all&#39;inoltro lato server
* Se lavori con Adobe Consulting, rivolgiti a loro riguardo al tuo piano di migrazione, in modo che possano aiutarti in base alle tue esigenze

## Convalida e risoluzione dei problemi {#validation-and-troubleshooting}

Il modo principale per verificare che l&#39;inoltro lato server sia in esecuzione, consiste nell&#39;esaminare la risposta a uno degli hit Adobe Analytics provenienti dall&#39;app.

Se non esegui l&#39;inoltro lato server dei dati da [!DNL Analytics] ad Audience Manager, allora non c&#39;è veramente alcuna risposta al [!DNL Analytics] beacon (oltre a un pixel 2x2). Tuttavia, se esegui l&#39;inoltro lato server, ci sono elementi che puoi verificare nella [!DNL Analytics] richiesta e risposta che ti informerà di questo [!DNL Analytics] comunica correttamente con l&#39;Audience Manager, inoltra l&#39;hit e riceve una risposta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Attenzione al falso esito &quot;Success&quot;. Se c&#39;è una risposta e tutto sembra funzionare, assicurati di avere `stuff` nella risposta. Se non lo fai, potresti vedere un messaggio che dice `"status":"SUCCESS"`. Per quanto sembri folle, questa è la prova che NON funziona correttamente.
>
>Se viene visualizzato questo messaggio, significa che hai completato l’aggiornamento del codice nei tag Platform o [!DNL AppMeasurement], ma che l&#39;inoltro nel [!DNL Analytics] [!DNL Admin Console] non è ancora stato completato. In questo caso è necessario verificare di aver abilitato l&#39;inoltro lato server in [!DNL Analytics] [!DNL Admin Console] per [!UICONTROL report suite]. Se lo avete fatto, e non sono ancora state 4 ore, siate pazienti, in quanto può richiedere così tanto per fare tutte le modifiche necessarie sul backend.


![falsa operazione](assets/falsesuccess.png)

Per ulteriori informazioni sull&#39;inoltro lato server, consulta la [documentazione](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
