---
title: Aggiornamento alla versione DIL 8.0 (o successiva) di Adobe Audience Manager
description: Questo articolo contiene passaggi e consigli sull'aggiornamento del codice Adobe Audience Manager (AAM) Data Integration Library (DIL) alla versione 8.0 o successiva. Questo si riferisce all'implementazione DIL "lato client", non all'inoltro lato server di  dati Adobe Analytics, e riguarderà DTM, Launch by Adobe e implementazioni senza soluzione di gestione tag  Adobe.
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 1%

---


# Aggiornamento alla versione DIL 8.0 (o successiva) di Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Questo articolo contiene passaggi e consigli sull&#39;aggiornamento del codice Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) alla versione 8.0 o successiva. Questo si riferisce all&#39;implementazione DIL &quot;lato client&quot;, non all&#39;inoltro lato server di  dati Adobe Analytics, e riguarderà DTM, Launch by Adobe e implementazioni senza soluzione di gestione tag  Adobe.

## Panoramica {#overview}

 codice del Audience Manager [!DNL Data Integration Library] (DIL) consente di implementare AAM sul sito Web*. Durante l’implementazione di versioni precedenti di DIL, non era necessario che  Adobe fosse implementato anche il servizio ID Experience Cloud (ECID) (anche se era un’ottima idea). A partire dalla versione DIL 8.0, esiste una dipendenza rigida da da ECID versione 3.3 o successiva. Se implementate DIL 8.0 o versione successiva senza ECID 3.3 o con una versione precedente, viene visualizzato un errore che non funzionerà. Poiché sono disponibili diversi modi per implementare AAM, abbiamo creato questa pagina per illustrarvi alcuni passaggi da seguire, nonché alcune raccomandazioni. Di seguito sono riportati i passaggi e le raccomandazioni suddivisi per piattaforma/metodo di implementazione. Ulteriori informazioni sui DIL sono disponibili nella [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html).

* Come indicato nella descrizione di questa pagina, questo includerà solo le implementazioni DIL &quot;lato client&quot; utilizzate da AAM clienti che non hanno  Adobe Analytics. Se  Adobe Analytics, è necessario utilizzare il metodo di inoltro lato server per implementare AAM. Questo metodo è descritto nella [documentazione](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).

## Elementi e metodi duplicati e obsoleti {#duplicate-and-deprecated-elements-and-methods}

Nelle versioni precedenti di DIL e ECID, esistevano metodi duplicati (metodi che svolgono la stessa funzione sia in DIL che in ECID), che generavano confusione riguardo a quale utilizzare. In genere, era necessario utilizzarli entrambi e combinarli, e quel messaggio non era ben comunicato ai nostri clienti. A partire da DIL 8.0, questi metodi e elementi duplicati sono stati dichiarati obsoleti in DIL ed è consigliabile utilizzare la versione ECID.

Ad esempio:

* Quando si utilizza [!DNL DIL.create], alcuni elementi sono obsoleti e al loro posto è necessario utilizzare gli elementi ECID. Questi elementi sono descritti nella [[!DNL DIL.create] documentazione](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html).
* Anche il metodo a livello di [!DNL idSync] istanza è stato dichiarato obsoleto, come indicato nella [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)del metodo.

## Sincronizzazione ID con un ID cliente {#id-syncing-with-a-customer-id}

In AAM, potete sincronizzare l&#39;UUID (ID utente univoco anonimo) sul computer con un ID cliente, in modo da poter caricare dati offline su tale cliente e collegarlo con il suo comportamento online, per comprendere meglio i clienti. In passato, ciò è stato fatto in uno dei due modi seguenti:

* Metodo a livello di [!DNL idSync] istanza
* L&#39; [!DNL declaredId] elemento in [!DNL DIL.create]

Se avete usato uno di questi metodi meno recenti per la sincronizzazione con un ID cliente, si consiglia vivamente di effettuare l’aggiornamento all’uso del [!DNL setCustomerIDs] metodo, che fa parte del servizio ECID. Ulteriori informazioni su [!DNL setCustomerIDs] sono disponibili nella [documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)del metodo.

**Suggerimento rapido:** Se in precedenza si utilizzava uno dei metodi indicati sopra, si faceva riferimento al AAM [!UICONTROL Data Source] con l&#39; [!UICONTROL Data Source] ID (AKA, il &quot;DPID&quot;). Quando effettuate l’aggiornamento a [!DNL setCustomerIDs], dovrete utilizzare [!UICONTROL Data Source]il &quot;[!UICONTROL Integration Code]&quot; del AAM. Indica comunque lo stesso [!UICONTROL Data Source] ma è solo un identificatore diverso. Questo viene mostrato nel video seguente.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Nelle sezioni seguenti sono elencati i passaggi e le raccomandazioni per l&#39;aggiornamento al DIL 8.0 in base al metodo di implementazione:

## Aggiornamento al DIL 8.0 in  Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}

Passaggi di base per l&#39;aggiornamento al DIL 8.0

1. Se utilizzate un DIL pre-8.0, prima di eseguire l&#39;aggiornamento andate alla configurazione DIL nell&#39;estensione AAM e prendete nota di eventuali opzioni avanzate utilizzate (da utilizzare in un passaggio successivo)
1. Aggiornare l&#39;estensione AAM alla versione 8.0 o successiva
1. Verifica che l’estensione del servizio ID Experience Cloud  sia 3.3.0 o successiva
1. Per tutti i metodi/elementi obsoleti (come &quot;[!DNL disableIDSyncs]&quot;) presenti nell’estensione AAM precedente alla 8.0 o nel codice personalizzato per DIL, abilitate i metodi ECID nell’estensione ECID.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) dichiaratoId -> (ECID) setCustomerIDs

1. Pubblicare le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Aggiornamento al DIL 8.0 in  DTM Adobe {#updating-to-dil-in-adobe-dtm}

1. Aggiornate lo strumento AAM alla versione 8.0 o successiva. Questa impostazione della versione si trova nella sezione &quot;Generale&quot; dello strumento AAM.
1. Per tutti i metodi/elementi obsoleti (come &quot;disableIDSyncs&quot;) presenti nel codice personalizzato per DIL dello strumento AAM precedente alla 8.0, annotateli (in modo che possiate aggiungerli allo strumento ECID) e quindi rimuoverli dall&#39;applicazione personalizzata [!DNL DIL code] nello strumento AAM.
1. Aggiorna l’estensione del servizio ID Experience Cloud  alla versione 3.3.0 o successiva
1. Aggiungete le opzioni avanzate allo strumento ECID che avete rimosso dal codice personalizzato dello strumento AAM.
1. Pubblicare le modifiche

## Aggiornamento al DIL 8.0 senza soluzione di gestione tag Adobe  {#additional-resources}

Se state aggiornando il codice direttamente sulla pagina, potete semplicemente sostituire gli elementi meno recenti con quelli più recenti, ad eccezione di quando è necessario spostare metodi/elementi da DIL a ECID, come descritto in precedenza. In tal caso, sostituirete semplicemente il vecchio metodo/elemento nella posizione DIL con il metodo/elemento ECID nella posizione ECID.

Lo stesso vale per i gestori di tag non  Adobe. Ovunque si disponga delle versioni precedenti di tale soluzione di gestione tag, sostituitela con il nuovo codice come descritto nei passaggi seguenti.

1. Aggiornate la libreria DIL alla versione più recente (8.0 o successiva). Dovrete ottenere il codice DIL più recente da  Consulenza Adobe o  Assistenza clienti di Adobe, poiché al momento non è disponibile in una posizione pubblica. Quindi sostituisci semplicemente il vecchio codice della libreria DIL con il nuovo codice della libreria DIL e passa al passaggio successivo (non fermarti adesso o ti imbatti in problemi, ha).
1. Installate [!DNL ECID Service] o aggiornate la versione esistente a 3.3.0 o versione successiva. Puoi scaricare la versione più recente  Servizio ID Experience Cloud [dalla nostra pagina](https://github.com/Adobe-Marketing-Cloud/id-service/releases)GitHub. Se hai bisogno di aiuto, consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/) o parla con un consulente di Adobe .

1. Verificate che eventuali metodi o elementi obsoleti presenti nel codice personalizzato per i DIL siano spostati nei metodi ECID:

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) dichiaratoId -> (ECID) setCustomerIDs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)