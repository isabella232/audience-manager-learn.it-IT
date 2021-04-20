---
title: Aggiornamento ad Adobe Audience Manager DIL versione 8.0 (o successiva)
description: Questo articolo ti fornirà passaggi e raccomandazioni sull’aggiornamento del codice della Data Integration Library (DIL) di Adobe Audience Manager (AAM) alla versione 8.0 o successiva. Si tratta dell’implementazione DIL lato client, non dell’inoltro lato server dei dati Adobe Analytics, e riguarderà DTM, Launch da Adobe e implementazioni senza soluzione Adobe Tag Manager.
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: "Developer, Data Engineer"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---


# Aggiornamento a DIL versione 8.0 (o successiva) di Adobe Audience Manager {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

Questo articolo ti fornirà passaggi e raccomandazioni sull’aggiornamento del codice Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL) alla versione 8.0 o successiva. Si tratta dell’implementazione DIL lato client, non dell’inoltro lato server dei dati Adobe Analytics, e riguarderà DTM, Launch da Adobe e implementazioni senza soluzione Adobe Tag Manager.

## Panoramica {#overview}

Il codice [!DNL Data Integration Library] (DIL) di Audience Manager consente di implementare AAM sul sito Web*. Quando si implementavano le versioni precedenti di DIL, non era necessario che fosse implementato anche il servizio Experience Cloud ID (ECID) di Adobe (anche se si trattava di un’idea molto valida). A partire dalla versione 8.0 di DIL, esiste una dipendenza rigida da ECID versione 3.3 o successiva. Se implementi DIL 8.0 o versioni successive senza ECID 3.3 o con una versione precedente, ottieni un errore e non funzionerà. Poiché sono disponibili diversi modi per implementare AAM, abbiamo creato questa pagina per fornire alcuni passaggi da seguire e alcuni consigli. Di seguito sono riportati questi passaggi e raccomandazioni suddivisi per piattaforma/metodo di implementazione. Ulteriori informazioni su DIL sono disponibili nella [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html).

* Come indicato nella descrizione di questa pagina, riguarderà solo le implementazioni DIL lato client, utilizzate dai clienti AAM che non hanno Adobe Analytics. Se disponi di Adobe Analytics, utilizza il metodo di inoltro lato server per implementare AAM. Questo metodo è descritto nella [documentazione](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).

## Elementi e metodi duplicati ed obsoleti {#duplicate-and-deprecated-elements-and-methods}

Nelle versioni precedenti di DIL ed ECID, esistevano metodi duplicati (metodi che svolgono la stessa funzione sia in DIL che in ECID) che generavano confusione su quale usare. In genere, era necessario utilizzarli entrambi e farli corrispondere, e il messaggio non veniva comunicato bene ai nostri clienti. A partire da DIL 8.0, questi metodi ed elementi duplicati sono stati dichiarati obsoleti in DIL e si consiglia di utilizzare la versione ECID.

Ad esempio:

* Quando utilizzi [!DNL DIL.create], alcuni elementi sono stati dichiarati obsoleti e devi invece utilizzare gli elementi ECID . Questi elementi vengono richiamati nella [[!DNL DIL.create] documentazione](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html).
* Anche il metodo a livello di istanza [!DNL idSync] è stato dichiarato obsoleto, come indicato nella sezione [documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html) del metodo.

## Sincronizzazione ID con un ID cliente {#id-syncing-with-a-customer-id}

In AAM, puoi sincronizzare il tuo UUID (ID utente univoco anonimo) sul computer con un ID cliente, in modo da caricare i dati offline su quel cliente e collegarli al suo comportamento online, per comprendere meglio i tuoi clienti. In passato, ciò è stato fatto in uno dei due modi seguenti:

* Il metodo [!DNL idSync] a livello di istanza
* L&#39;elemento [!DNL declaredId] in [!DNL DIL.create]

Se utilizzi uno di questi metodi meno recenti per la sincronizzazione con un ID cliente, ti consigliamo vivamente di eseguire l’aggiornamento a utilizzando il metodo [!DNL setCustomerIDs] , che fa parte del servizio ECID. Ulteriori informazioni su [!DNL setCustomerIDs] sono disponibili nella [documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html) del metodo.

**Suggerimento rapido:** in precedenza, utilizzando uno dei metodi di cui sopra, hai fatto riferimento all&#39;AAM  [!UICONTROL Data Source] con l&#39; [!UICONTROL Data Source] ID (AKA, l&#39;&quot;DPID&quot;). Quando esegui l’aggiornamento a [!DNL setCustomerIDs], dovrai utilizzare il simbolo &quot;[!UICONTROL Integration Code]&quot; di AAM [!UICONTROL Data Source]. Indica comunque lo stesso [!UICONTROL Data Source] ma è solo un identificatore diverso. Questo è illustrato nel video seguente.

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

Nelle sezioni seguenti sono elencati i passaggi e le raccomandazioni per l’aggiornamento a DIL 8.0 in base al metodo di implementazione:

## Aggiornamento a DIL 8.0 in Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}

Passaggi fondamentali per l’aggiornamento a DIL 8.0

1. Se utilizzi un DIL precedente alla 8.0, prima di eseguire l’aggiornamento, passa alla configurazione DIL nell’estensione AAM e prendi nota delle opzioni avanzate in uso (da utilizzare in un passaggio successivo)
1. Aggiorna l&#39;estensione AAM alla versione 8.0 o successiva
1. Verifica che la tua estensione del servizio Experience Cloud ID sia 3.3.0 o successiva
1. Per tutti i metodi/elementi obsoleti (come &quot;[!DNL disableIDSyncs]&quot;) presenti nell&#39;estensione AAM precedente alla 8.0 o nel codice personalizzato per DIL, abilita i metodi ECID nell&#39;estensione ECID.

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) dichiaratoId -> (ECID) setCustomerIDs

1. Pubblicare le modifiche

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Aggiornamento a DIL 8.0 in Adobe DTM {#updating-to-dil-in-adobe-dtm}

1. Aggiorna lo strumento AAM alla versione 8.0 o successiva. Questa impostazione della versione si trova nella sezione &quot;Generale&quot; dello strumento AAM.
1. Per tutti i metodi/elementi obsoleti (come &quot;disableIDSyncs&quot;) presenti nel codice personalizzato dello strumento AAM precedente alla versione 8.0 per DIL, prendine nota (in modo da poterli aggiungere allo strumento ECID) e rimuoverli dall’ personalizzato [!DNL DIL code] nello strumento AAM.
1. Aggiorna l’estensione del servizio Experience Cloud ID alla versione 3.3.0 o successiva
1. Aggiungi le opzioni avanzate allo strumento ECID rimosso dal codice personalizzato dello strumento AAM.
1. Pubblicare le modifiche

## Aggiornamento a DIL 8.0 senza soluzione di gestione tag Adobe {#additional-resources}

Se aggiorni il codice direttamente sulla pagina, puoi semplicemente sostituire gli elementi meno recenti con quelli più recenti, ad eccezione di quando devi spostare metodi/elementi da DIL a ECID, come descritto in precedenza. In tal caso, sostituirai semplicemente il vecchio metodo/elemento nella posizione DIL con il metodo/elemento ECID nella posizione ECID.

Lo stesso vale per i gestori di tag non Adobe. Dovunque si disponga delle versioni precedenti in quella soluzione di gestione dei tag, sostituiscilo con il nuovo codice come descritto nei passaggi seguenti.

1. Aggiorna la libreria DIL alla versione più recente (8.0 o successiva): devi ottenere l&#39;ultimo codice DIL da Adobe Consulting o dall&#39;Assistenza clienti Adobe, in quanto non è attualmente disponibile in una posizione pubblica. Quindi sostituisci semplicemente il vecchio codice della libreria DIL con il nuovo codice della libreria DIL e passa al passaggio successivo (non fermarti ora o ti imbatterai in problemi, ha).
1. Installa [!DNL ECID Service] o aggiorna la versione esistente alla versione 3.3.0 o successiva. Scarica la versione più recente del servizio Experience Cloud ID [dalla nostra pagina GitHub](https://github.com/Adobe-Marketing-Cloud/id-service/releases). Per assistenza, consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/) o rivolgiti a un consulente Adobe.

1. Verifica che tutti i metodi o gli elementi obsoleti presenti nel codice personalizzato per DIL vengano spostati nei metodi ECID:

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) dichiaratoId -> (ECID) setCustomerIDs

      [Documentazione](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)