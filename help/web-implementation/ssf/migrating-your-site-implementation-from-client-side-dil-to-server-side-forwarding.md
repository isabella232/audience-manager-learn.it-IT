---
title: Migrare l’implementazione Audience Manager del sito da DIL lato client all’inoltro lato server
description: Scopri come migrare l’implementazione dell’Audience Manager del tuo sito (AAM) da DIL lato client a inoltro lato server. Questo tutorial è applicabile se disponi sia di AAM che di Adobe Analytics e se invii hit dalla pagina all’AAM utilizzando il codice DIL (Data Integration Library), nonché se invii hit dalla pagina ad Adobe Analytics.
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

# Migrare l’implementazione Audience Manager del sito da DIL lato client all’inoltro lato server {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Questo tutorial si applica se disponi sia di Adobe Audience Manager (AAM) che di Adobe Analytics e stai attualmente inviando un hit dalla pagina all’AAM utilizzando DIL ([!DNL Data Integration Library]) e l&#39;invio di un hit dalla pagina ad Adobe Analytics. Poiché si dispone di entrambe queste soluzioni e poiché fanno entrambe parte di Adobe Experience Cloud, è possibile seguire la best practice di attivazione dell&#39;inoltro lato server, che consente [!DNL Analytics] server di raccolta dati per inoltrare i dati di analisi del sito in tempo reale ad Audience Manager, invece di richiedere che il codice lato client invii un hit aggiuntivo dalla pagina all’AAM. Questo tutorial illustra i passaggi necessari per passare dall’implementazione precedente di DIL lato client al più recente metodo di inoltro lato server.

## Lato client (DIL) e lato server {#client-side-dil-vs-server-side}

Quando si confrontano e si confrontano questi due metodi per ottenere i dati di Adobe Analytics nell’AAM, potrebbe essere utile innanzitutto visualizzare le differenze nell’immagine seguente:

![lato client-lato server](assets/client-side_vs_server-side_aam_implementation.png)

### Implementazione di DIL lato client {#client-side-dil-implementation}

Se utilizzi questo metodo per inserire i dati di Adobe Analytics nell’AAM, puoi avere due risultati provenienti dalle pagine web: uno [!DNL Analytics], e uno verso l&#39;AAM (dopo aver copiato il [!DNL Analytics] nella pagina Web. [!UICONTROL Segments] vengono restituiti dall’AAM alla pagina, dove possono essere utilizzati per la personalizzazione e così via. Questa viene considerata un’implementazione legacy e non è più consigliata.

Oltre al fatto che non si tratta di seguire le best practice, gli svantaggi dell’utilizzo di questo metodo includono:

* Due hit provenienti dalla pagina invece di uno solo
* l’inoltro lato server è necessario per la condivisione in tempo reale dei tipi di pubblico AAM [!DNL Analytics], pertanto le implementazioni lato client non consentono questa funzione (e potenzialmente altre funzioni in futuro)

È consigliabile passare a un metodo di inoltro lato server per l’implementazione dell’AAM.

### Implementazione dell&#39;inoltro lato server {#server-side-forwarding-implementation}

Come mostrato nell’immagine precedente, un hit viene dalla pagina web ad Adobe Analytics. [!DNL Analytics] inoltra quindi tali dati all’AAM in tempo reale, e i visitatori vengono valutati in caratteristiche AAM e [!UICONTROL segments], come se l’hit provenisse direttamente dalla pagina.

[!UICONTROL Segments] vengono restituiti sullo stesso hit in tempo reale a [!DNL Analytics], che inoltra la risposta alla pagina web per la personalizzazione e così via.

Non esiste alcun downside temporale per il passaggio a Server-Side Forwarding. Adobe consiglia vivamente a chiunque abbia sia Audience Manager che [!DNL Analytics] utilizza questo metodo di implementazione.

## Hai due attività principali {#you-have-two-main-tasks}

Ci sono un bel po &#39;di informazioni su questa pagina, ed è tutto importante, naturalmente. Tuttavia, **tutto si riduce a due cose principali da fare**:

1. Modifica il codice da codice DIL lato client a codice inoltro lato server
1. Capovolgere l&#39;interruttore nella [!DNL Analytics] [!DNL Admin Console] per avviare l&#39;effettivo inoltro dei dati (per [!UICONTROL report suite])

Se salti una di queste attività, l&#39;inoltro lato server non funzionerà correttamente. In questo documento sono stati aggiunti passaggi e dati aggiuntivi per aiutarti a eseguire questi due passaggi correttamente per la tua configurazione.

## Opzioni di implementazione {#implementation-options}

Quando passi dall’inoltro lato client a quello lato server, una delle attività che dovrai svolgere è quella di modificare il codice per inserirlo nel nuovo codice di inoltro lato server. Questa operazione viene eseguita utilizzando una delle seguenti opzioni:

* Tag Adobe Experience Platform: opzione di implementazione consigliata da Adobe per le proprietà web. Questa è un’attività semplice, poiché i tag di Platform hanno svolto tutto il lavoro necessario.
* Sulla pagina - Puoi anche inserire il nuovo codice SSF direttamente nel `doPlugins` funzione all&#39;interno del `appMeasurement.js` file, se non utilizzi (ancora) Adobe Launch
* Altri gestori di tag: possono essere trattati come l’opzione precedente (Sulla pagina), in quanto inserirai ancora il codice SSF in `doPlugins`, ovunque l’altro gestore di tag memorizzi [!DNL AppMeasurement] codice

Esamineremo ciascuno di questi elementi di seguito nella _Aggiornamento del codice_ sezione.

## Passaggi di implementazione {#implementation-steps}

I passaggi seguenti descrivono l’implementazione.

### Passaggio 0: Prerequisito: servizio ID Experience Cloud (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

Il prerequisito principale per passare all&#39;inoltro lato server è l&#39;implementazione del servizio ID Experience Cloud. Questa operazione è più semplice se si utilizza Experience Platform Launch, nel qual caso è sufficiente installare l’estensione ECID e fare il resto.

Se utilizzi un TMS non Adobe, o nessun TMS, implementa ECID per l’esecuzione **prima di** qualsiasi altra soluzione di Adobe. Consulta la [Documentazione ECID](https://experienceleague.adobe.com/docs/id-service/using/home.html) per ulteriori dettagli. L’unico altro prerequisito è relativo alle versioni del codice, quindi puoi applicare semplicemente le versioni più recenti del codice nei passaggi seguenti.

>[!NOTE]
>
>Leggi l’intero documento prima di implementare. La sezione &quot;Tempistiche&quot; riportata di seguito contiene informazioni importanti su *quando* devi implementare ogni elemento, incluso ECID (se non è ancora implementato).

### Passaggio 1: registra le opzioni attualmente utilizzate dal codice DIL {#step-record-currently-used-options-from-dil-code}

Quando ti prepari a passare dal codice DIL lato client all’inoltro lato server, il primo passaggio consiste nell’identificare tutto ciò che si sta facendo con il codice DIL, inclusi le impostazioni personalizzate e i dati inviati all’AAM. Gli aspetti da considerare includono:

* Normale [!DNL Analytics] variabili, utilizzando `siteCatalyst.init` Modulo DIL - Non devi preoccuparti di questo, perché il suo lavoro è solo quello di inviare il normale [!DNL Analytics] e questo accade semplicemente perché è stato abilitato l’inoltro lato server.
* Sottodominio partner: in `DIL.create` funzione, annota `partner` parametro. Questo è noto come &quot;sottodominio partner&quot; o talvolta &quot;ID partner&quot; e sarà necessario quando inserisci il nuovo codice di inoltro lato server.
* [!DNL Visitor Service Namespace] - Anche noto come &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID],&quot; ne avrai bisogno anche quando configuri il nuovo codice di inoltro lato server. Prendetene nota.
* containerNSID, uuidCookie e altre opzioni avanzate: prendi nota di eventuali opzioni avanzate aggiuntive in uso in modo da poterle impostare anche nel codice di inoltro lato server.
* Variabili di pagina aggiuntive - Se dalla pagina vengono inviate all’AAM altre variabili (oltre alle normali [!DNL Analytics] variabili gestite da siteCatalyst.init), dovrai prenderne nota in modo che possano essere inviate tramite inoltro lato server (avviso spoiler: tramite [!DNL contextData] variabili).

### Passaggio 2: aggiornare il codice {#step-updating-the-code}

In entrata [Opzioni di implementazione](#implementation-options) (sopra), sono fornite più opzioni relative a come e dove si implementa l’inoltro lato server. Affinché questa sezione sia efficace, è necessario suddividerla in queste sezioni (con due di esse combinate). Vai al metodo di questa sezione che descrive meglio le tue esigenze.

#### Tag Adobe Experience Platform {#launch-by-adobe}

Guarda il video seguente per scoprire come spostare le opzioni di implementazione dal codice DIL lato client all’inoltro lato server nel Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;Sulla pagina&quot; o gestore di tag non Adobe {#on-the-page-or-non-adobe-tag-manager}

Guarda il video seguente per scoprire come spostare le opzioni di implementazione dal codice DIL lato client all’inoltro lato server in [!DNL AppMeasurement] che risiedono in un file o in un sistema di gestione dei tag non Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Passaggio 3: abilitare l’inoltro (per [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Finora in questo tutorial abbiamo dedicato tutto il nostro tempo a passare dal codice DIL lato client all’inoltro lato server. Va bene, perché è la parte più difficile. Questa sezione, anche se è molto semplice, è importante quanto l’aggiornamento del codice. Questo video illustra come capovolgere lo switch che consente l’effettivo inoltro di dati da Analytics ad Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Come indicato nel video, ricorda che saranno necessarie fino a 4 ore per consentire la piena implementazione dell’inoltro sul backend Experience Cloud.

## Tempistica {#timing}

Come promemoria, ci sono due attività principali per passare dal DIL lato client all’inoltro lato server:

1. Aggiornamento del codice
1. Capovolgimento dello switch in [!DNL Analytics] [!DNL Admin Console]

Ma la domanda è, quale fai per primo? Importa? Ok, scusate, erano due domande. Ma le risposte sono... dipende, e sì, *può* è importante. Com&#39;è per essere vago? Suddividiamola. Ma prima una domanda aggiuntiva che può venire fuori se siete una grande organizzazione con numerosi siti: Devo fare tutto in una volta? Quello è un po&#39; più facile. No. Potete farlo pezzo per pezzo.

### Un po&#39; più a fondo {#a-little-deeper-dive}

Il motivo per cui la tempistica e l&#39;ordine contano è a causa di come inoltro _davvero_ lavori, che possono essere sintetizzati nei seguenti elementi tecnici:

* Se hai implementato il servizio ID Experience Cloud (ECID) e il commutatore nel [!DNL Analytics] [!DNL Admin Console] (&quot;lo switch&quot;) è acceso, i dati VERRANNO inoltrati da [!DNL Analytics] all’AAM, anche se non hai ancora aggiornato il codice.
* Se ECID non è stato implementato, i dati non verranno inoltrati, anche se l’utente è acceso e dispone del codice di inoltro lato server.
* Il codice di inoltro lato server (nei tag Platform o sulla pagina) gestisce davvero la risposta ed è necessario per completare la migrazione.
* Ricordare che il commutatore di inoltro lato server è abilitato da [!UICONTROL report suite], ma che il codice viene gestito dalla proprietà nei tag di Platform oppure [!DNL AppMeasurement] se non utilizzi i tag di Platform.

### Best practice {#best-practices}

Sulla base di questi dettagli tecnici, ecco le raccomandazioni su cosa fare e quando:

#### Se NON hai ancora implementato ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Capovolgere l&#39;interruttore [!DNL Analytics] per ogni [!UICONTROL report suite] che abiliterai per l’inoltro lato server.

   1. L’inoltro non si avvia ancora perché non disponi di ECID.

1. Per sito, aggiorna il codice da client-side DIL all’inoltro lato server (ad esempio nei tag di Platform) o sulla pagina, come descritto in un’altra sezione precedente.

   1. L’inoltro ora scorre (come hai aggiunto ECID) e dovresti anche ricevere una risposta JSON appropriata al tuo [!DNL Analytics] beacon (per ulteriori dettagli, consulta la sezione Convalida e risoluzione dei problemi di seguito).

#### Se ECID è stato implementato {#if-you-do-have-ecid-implemented}

1. Prepara e pianifica in modo da essere pronto ad aggiornare il codice da DIL all’inoltro lato server PER [!UICONTROL report suite] che abiliterai per l’inoltro lato server:

   1. Capovolgere l&#39;interruttore [!DNL Analytics] per abilitare l&#39;inoltro lato server.

      1. L’inoltro verrà avviato perché l’ECID è abilitato.
   1. Non appena possibile, aggiorna il codice da client-side DIL all’inoltro lato singolo (potrebbe trattarsi dei tag di Platform o della pagina, come descritto in un’altra sezione in precedenza).

      1. Dovresti ricevere una risposta JSON appropriata al tuo [!DNL Analytics] (vedere il [Convalida e risoluzione dei problemi](#validation-and-troubleshooting) per ulteriori dettagli).


>[!NOTE]
>
>È importante che questi due passaggi si avvicinino il più possibile, perché tra i passaggi 1 e 2 di cui sopra, si avranno duplicazioni dei dati che vanno nell’AAM. In altre parole, l’inoltro lato singolo avrà iniziato a inviare dati da [!DNL Analytics] all&#39;AAM, e poiché il codice DIL è ancora sulla pagina, ci sarà anche un hit che passa direttamente dalla pagina all&#39;AAM, raddoppiando così i dati. Non appena aggiorni il codice da DIL all&#39;inoltro lato server, questo sarà alleviato.

>[!NOTE]
>
>Se preferisci una piccola discrepanza nei dati piuttosto che una piccola duplicazione di dati, puoi cambiare l’ordine dei passaggi 1 e 2 precedenti. Lo spostamento del codice da DIL all’inoltro lato server interrompe il flusso di dati in AAM fino a quando non sei in grado di capovolgere lo switch per attivare l’inoltro lato server per [!UICONTROL report suite]. In genere, i clienti preferiscono raddoppiare i dati piuttosto che perdere l’opportunità di trasformare i visitatori in caratteristiche e [!UICONTROL segments].

#### Tempi di migrazione quando si dispone di molti siti e [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Questo argomento è trattato brevemente nelle sezioni precedenti, in quanto la strategia principale può essere riassunta come segue:

Esegui migrazione di un sito/[!UICONTROL report suite] (o gruppo di siti/[!UICONTROL report suites]) alla volta.

Tuttavia, questo può diventare un po’ complicato in base ad alcuni possibili scenari:

* Si dispone di un sito che contiene diversi [!UICONTROL report suites]
* Hai un [!UICONTROL report suite] che include diversi siti (come un [!UICONTROL report suite])
* Puoi utilizzare una proprietà Platform tags per coprire più siti
* Hai diversi team di sviluppo per siti diversi

A causa di questi elementi, può diventare un po &#39;complicato. Le cose migliori che posso suggerire sono:

* Prendi un po’ di tempo per definire una strategia per la migrazione all’inoltro lato server, in base agli elementi spiegati in precedenza
* In base al fatto che una singola proprietà nei tag di Platform (o una singola [!DNL AppMeasurement] file) viene mappato in genere a 1 o 2 elementi distinti [!UICONTROL report suites], probabilmente sarai in grado di creare un piano che funzioni su questi gruppi distinti uno per uno, aggiornando la tua azienda all&#39;inoltro lato server
* Se lavori con Adobe Consulting, rivolgiti a loro in merito al tuo piano di migrazione, in modo che possano aiutarli quando necessario

## Convalida e risoluzione dei problemi {#validation-and-troubleshooting}

Il modo principale per verificare che l’inoltro lato server sia in esecuzione, consiste nell’esaminare la risposta a qualsiasi hit di Adobe Analytics proveniente dall’app.

Se non esegui l&#39;inoltro lato server dei dati da [!DNL Analytics] ad Audience Manager, non esiste alcuna risposta al [!DNL Analytics] (oltre a un pixel 2x2). Tuttavia, se esegui l’inoltro lato server, vi sono elementi che puoi verificare nel [!DNL Analytics] richiesta e risposta che ti informeranno che [!DNL Analytics] comunica correttamente con Audience Manager, inoltrando l’hit e ricevendo una risposta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>Attenzione al falso esito &quot;Success&quot;. Se ricevi una risposta e tutto sembra funzionare, assicurati di disporre di `stuff` oggetto nella risposta. In caso contrario, potresti visualizzare un messaggio che indica `"status":"SUCCESS"`. Per quanto sembri folle, questa è la prova che NON funziona correttamente.
>
>Questo significa che hai completato l’aggiornamento del codice nei tag di Platform o [!DNL AppMeasurement], ma che l&#39;inoltro nel [!DNL Analytics] [!DNL Admin Console] non ancora completato. In questo caso è necessario verificare di aver abilitato l’inoltro lato server in [!DNL Analytics] [!DNL Admin Console] per [!UICONTROL report suite]. Se lo hai fatto, e non sono ancora passate 4 ore, attendi, poiché potrebbe essere necessario attendere così tanto per apportare tutte le modifiche necessarie sul backend.


![falso successo](assets/falsesuccess.png)

Per ulteriori informazioni sull&#39;inoltro lato server, vedi [documentazione](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
