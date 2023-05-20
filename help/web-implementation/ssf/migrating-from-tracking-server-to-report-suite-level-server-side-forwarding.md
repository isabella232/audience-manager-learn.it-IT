---
title: Migrazione dal server di tracciamento all’inoltro lato server a livello di suite di rapporti
description: Scopri come abilitare l’inoltro lato server dei dati di Adobe Analytics per l’Audience Manager a livello di suite di rapporti anziché a livello di server di tracciamento.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Migrazione dal server di tracciamento all’inoltro lato server a livello di suite di rapporti {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Questo articolo e video illustra come abilitare l’inoltro lato server di [!DNL Analytics] Dati da Audience Manager in un [!UICONTROL report suite] livello invece di a [!UICONTROL tracking server] livello.

## Introduzione {#introduction}

Se disponi di Adobe Audience Manager E Adobe Analytics, puoi implementare l’inoltro lato server del [!DNL Analytics] dati di Audience Manager. Questo significa che, invece di inviare due hit dalla pagina (uno a [!DNL Analytics] e uno ad Audience Manager), può inviare un hit a [!DNL Analytics], e [!DNL Analytics] inoltra tali dati ad Audience Manager.

Se disponi già di questa funzionalità operativa e se era abilitata/implementata prima di ottobre 2017, l’inoltro lato server potrebbe essere basato su [!UICONTROL Tracking Server], che doveva essere abilitato dall’Assistenza clienti Adobe o dalla consulenza Adobe. A partire da ottobre 2017, è ora possibile configurare autonomamente l’inoltro lato server e a livello di suite di rapporti (inoltro per suite di rapporti). Questo comporta vantaggi significativi, che vengono discussi di seguito.

## [!UICONTROL Tracking server] inoltro {#tracking-server-forwarding}

Il tuo [!UICONTROL tracking server] è la posizione in cui stai inviando il tuo [!DNL Analytics] e anche il dominio in cui vengono scritti la richiesta di immagine e il cookie. Deve essere impostato in DTM o [!DNL Experience Platform Launch], o nella [!DNL AppMeasurement.js] , e avrà in genere questo aspetto, con il tuo sito o la tua ragione sociale che sostituisce &quot;miosito&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se l&#39;inoltro lato server è configurato per l&#39;inoltro a [!UICONTROL tracking server] livello, qualsiasi hit inviato a questo [!UICONTROL tracking server] (SE è abilitato anche il servizio ID Experience Cloud) verrà inoltrato ad Audience Manager. Questo doveva essere abilitato dall’Assistenza clienti Adobe o dalla consulenza Adobe. Sono anche quelli che possono disattivarlo, DOPO che sei passato a [!UICONTROL report suite] inoltro, come descritto di seguito.

Se non è sicuro se [!DNL tracking server forwarding] è abilitato per te, contatta l’Assistenza clienti Adobe o il servizio di consulenza Adobe e dovrebbe essere in grado di dirtelo.

## [!UICONTROL Report-suite]Inoltro lato server {#report-suite-level-server-side-forwarding}

Uno dei principali vantaggi del passaggio a [!UICONTROL report suite] inoltro da [!UICONTROL tracking server] L’inoltro consiste nella possibilità di utilizzare &quot;Audience Analytics&quot;, che è la possibilità di inoltrare Audience Manager. [!UICONTROL segments] torna ad Adobe Analytics per un’analisi dettagliata dei segmenti. Questa funzione eccezionale NON è supportata se si è ancora in [!UICONTROL tracking server] inoltro e non [!UICONTROL report suite] inoltro. Ulteriori informazioni sull’Audience Analytics sono disponibili in [documentazione](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, una volta che hai tutte le [!UICONTROL report suites] impostato per inoltrare a Audience Manager, contatta l’Assistenza clienti o la Consulenza di Adobe Adobe e disattiva l’opzione [!UICONTROL tracking server] inoltro. Non è un&#39;emergenza per te fare questo, perché avere entrambi [!UICONTROL tracking server] inoltro e [!UICONTROL report suite] l’inoltro non genera hit duplicati. Tuttavia, è buona prassi disporre solo di [!UICONTROL report suite] inoltro in data.

Se esci [!UICONTROL tracking server] inoltro in data, non solo potrebbe inoltrare dati da [!UICONTROL report suites] che non desideri essere inoltrato, ma in futuro, dopo che avrai dimenticato (e tutti coloro che lavorano nella tua azienda) che [!UICONTROL tracking server] inoltro è attivo, potresti pensare che i dati non vengano inoltrati per un [!UICONTROL report suite]. Questo perché non è attivato a livello di suite di rapporti, ma i dati vengono comunque inoltrati a causa del [!UICONTROL tracking server]. In questo modo, perderai tempo e denaro per capire perché inoltra e paga le chiamate al server AAM che non ti aspettavi. Quindi, è una buona idea disattivare [!UICONTROL tracking server] inoltro non appena si dispone di tutte le [!UICONTROL report suites] è stata impostata per inoltrare i contenuti in base alle esigenze aziendali.
