---
title: Migrazione da Tracking Server a Report Suite-Side Forwarding
description: Questo articolo e video illustra come abilitare l’inoltro lato server dei dati di Analytics ad Audience Manager a livello di suite di rapporti invece che a livello di server di tracciamento.
product: audience manager, analytics
feature: Integrazione di Adobe Analytics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: '"Sviluppatore, data engineer"'
level: Intermedio
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---


# Migrazione da [!UICONTROL Tracking Server] a [!UICONTROL Report Suite] livello [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Questo articolo e video illustra come abilitare [!UICONTROL server-side forwarding] dati ad Audience Manager a livello [!DNL Analytics] anziché a livello [!UICONTROL tracking server].[!UICONTROL report suite]

## Introduzione {#introduction}

Se disponi di Adobe Audience Manager E Adobe Analytics, puoi implementare &quot;[!UICONTROL Server-side Forwarding]&quot; dei dati [!DNL Analytics] in Audience Manager. Questo significa che, invece di inviare 2 hit alla pagina (uno a [!DNL Analytics] e uno ad Audience Manager), può semplicemente inviare un hit a [!DNL Analytics] e [!DNL Analytics] inoltrerà tali dati ad Audience Manager. Se questo è già in esecuzione e lo hai abilitato/implementato prima di ottobre 2017, il tuo [!UICONTROL server-side forwarding] potrebbe essere basato sul tuo &quot;[!UICONTROL Tracking Server]&quot;, che doveva essere abilitato dall’Assistenza clienti Adobe o da Adobe Consulting. A partire da ottobre 2017, ora puoi configurare [!UICONTROL server-side forwarding] autonomamente e farlo a livello [!UICONTROL Report Suite] (inoltro PER [!UICONTROL Report Suite]). Questo aspetto presenta notevoli vantaggi, che saranno discussi di seguito.

## [!UICONTROL Tracking Server] Inoltro  {#tracking-server-forwarding}

Il [!UICONTROL tracking server] è la posizione in cui invii i dati [!DNL Analytics] e anche il dominio in cui vengono scritti la richiesta di immagini e il cookie. Deve essere impostato in DTM o [!DNL Experience Platform Launch] o nel file [!DNL AppMeasurement.js] e avrà un aspetto simile a questo, con il tuo sito o nome commerciale che sostituisce &quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se [!UICONTROL server-side forwarding] è impostato per l&#39;inoltro a livello [!UICONTROL tracking server], tutti gli hit che vengono inviati a questo [!UICONTROL tracking server] (SE è abilitato anche il servizio Experience Cloud ID) verranno inoltrati ad Audience Manager. Questo doveva essere abilitato dall’Assistenza clienti Adobe o da Adobe Consulting. Sono anche quelli che possono disabilitarlo, DOPO che hai effettuato il passaggio a [!UICONTROL report suite] inoltro, come descritto di seguito.

Se non sei sicuro se [!DNL tracking server forwarding] è abilitato, contatta l’Assistenza clienti Adobe o Adobe Consulting e devono essere in grado di avvisarti.

## [!UICONTROL Report Suite]-Livello  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Uno dei vantaggi più importanti per il passaggio all’ [!UICONTROL report suite] inoltro da [!UICONTROL tracking server] è la possibilità di utilizzare &quot;Audience Analytics&quot;, ovvero la possibilità di inoltrare nuovamente Audience Manager [!UICONTROL segments] ad Adobe Analytics per un’analisi dettagliata [!UICONTROL segment]. Questa grande funzionalità NON è supportata se sei ancora in inoltro [!UICONTROL tracking server] e non in inoltro [!UICONTROL report suite]. Per ulteriori informazioni su Audience Analytics consulta la documentazione [documentazione](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, dopo aver impostato tutte le [!UICONTROL report suites] per inoltrare l’inoltro ad Audience Manager, contatta l’Assistenza clienti Adobe o Adobe Consulting e fai loro disabilitare l’ [!UICONTROL tracking server] inoltro. Non è un’emergenza per te eseguire questa operazione, perché l’inoltro [!UICONTROL tracking server] e l’inoltro [!UICONTROL report suite] NON determineranno risultati duplicati. Tuttavia, è consigliabile solo l’inoltro di [!UICONTROL report suite]. Se lasci [!UICONTROL tracking server] inoltrare i dati non solo da [!UICONTROL report suites] che non desideri inoltrare, ma in futuro, dopo che tu (e tutti gli utenti della tua azienda) hai dimenticato che [!UICONTROL tracking server] l&#39;inoltro è attivo, puoi pensare che i dati non vengano inoltrati per un [!UICONTROL report suite] specifico (perché non sono attivati a livello di suite di rapporti), ma i dati vengono comunque inoltrati a causa del [!UICONTROL tracking server]. A quel punto perderai tempo e denaro per capire perché sta inoltrando e anche pagando per le chiamate al server AAM che non ti aspettavi. È quindi consigliabile disattivare l’ inoltro non appena si dispone di tutte le [!UICONTROL report suites] impostate per l’inoltro che soddisfano le tue esigenze aziendali.[!UICONTROL tracking server]
