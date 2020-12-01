---
title: Migrazione dal server di monitoraggio all'inoltro lato server a livello di suite di rapporti
description: Questo articolo e questo video mostreranno come abilitare l'inoltro lato server dei dati di Analytics  Audience Manager a livello di suite di rapporti invece che a livello di server di tracciamento.
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# Migrazione da [!UICONTROL Tracking Server] a [!UICONTROL Report Suite] livello [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Questo articolo e questo video illustra come abilitare [!UICONTROL server-side forwarding] di [!DNL Analytics] Dati per  Audience Manager a un livello [!UICONTROL report suite] invece che a un livello [!UICONTROL tracking server].

## Introduzione {#introduction}

Se si dispone di Adobe Audience Manager AND  Adobe Analytics, è possibile implementare &quot;[!UICONTROL Server-side Forwarding]&quot; dei dati [!DNL Analytics] per  Audience Manager. Questo significa che, invece di inviare 2 hit alla pagina (uno a [!DNL Analytics] e uno  Audience Manager), può semplicemente inviare un hit a [!DNL Analytics], e [!DNL Analytics] inoltrerà tali dati a  Audience Manager. Se questo è già in funzione e lo avete attivato o implementato prima di ottobre 2017, il [!UICONTROL server-side forwarding] potrebbe essere basato sul vostro &quot;[!UICONTROL Tracking Server]&quot;, che doveva essere attivato dall&#39;Assistenza clienti  Adobe o  Consulenza Adobe. A partire da ottobre 2017, ora è possibile configurare [!UICONTROL server-side forwarding] se stessi e farlo a livello [!UICONTROL Report Suite] (inoltro PER [!UICONTROL Report Suite]). A questo proposito si aprono notevoli vantaggi, che saranno discussi di seguito.

## [!UICONTROL Tracking Server] Inoltro  {#tracking-server-forwarding}

Il [!UICONTROL tracking server] è il percorso in cui si inviano i dati [!DNL Analytics] e anche il dominio in cui vengono scritti la richiesta immagine e il cookie. Deve essere impostato in DTM o [!DNL Experience Platform Launch], o nel file [!DNL AppMeasurement.js] e avrà in genere questo aspetto, con il vostro sito o nome commerciale che sostituisce &quot;miosito&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se [!UICONTROL server-side forwarding] è impostato in modo da essere inoltrato al livello [!UICONTROL tracking server], tutti gli hit inviati a questo [!UICONTROL tracking server] (se è abilitato anche il servizio ID Experience Cloud ) verranno inoltrati al Audience Manager . Questa funzione doveva essere abilitata dall&#39;Assistenza clienti  Adobe o dalla Consulenza  Adobe. Sono anche quelli che possono disattivarlo, DOPO che hai passato all&#39;inoltro [!UICONTROL report suite], come descritto di seguito.

Se non sei sicuro se [!DNL tracking server forwarding] è abilitato per te, contatta l&#39;Assistenza clienti  Adobe o la Consulenza  Adobe e devono essere in grado di avvisarti.

## [!UICONTROL Report Suite]-Level  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Uno dei maggiori vantaggi per passare all&#39;inoltro [!UICONTROL report suite] dall&#39;inoltro [!UICONTROL tracking server] è che ora sarà possibile utilizzare &quot; Audience Analytics&quot;, ovvero la capacità di inoltrare  Audience Manager [!UICONTROL segments] nuovamente  Adobe Analytics per un&#39;analisi dettagliata [!UICONTROL segment]. Questa grande funzionalità NON è supportata se si è ancora su [!UICONTROL tracking server] inoltro e non su [!UICONTROL report suite] inoltro. Per ulteriori informazioni  Audience Analytics, vedere la sezione [documentazione](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, dopo aver impostato tutte le [!UICONTROL report suites] da inoltrare per  Audience Manager, è necessario contattare  Assistenza clienti Adobe o  Consulenza Adobe e disattivare l&#39;inoltro [!UICONTROL tracking server]. Non si tratta di un&#39;emergenza, perché avere sia l&#39;inoltro [!UICONTROL tracking server] che l&#39;inoltro [!UICONTROL report suite] NON produrranno hit duplicati. Tuttavia, è consigliabile avere solo l&#39;inoltro [!UICONTROL report suite]. Se si lascia avanti [!UICONTROL tracking server], non solo è possibile inoltrare i dati da [!UICONTROL report suites] che non si desidera inoltrare, ma in futuro, dopo che l&#39;utente (e tutti gli utenti della società) ha dimenticato che l&#39;inoltro [!UICONTROL tracking server] è attivo, è possibile che i dati non vengano inoltrati per un [!UICONTROL report suite] specifico (perché non sono attivati a livello di suite di report), ma i dati vengono comunque inoltrati a causa del [!UICONTROL tracking server]. In questo modo si perderanno tempo e denaro per capire il motivo dell&#39;inoltro e per pagare AAM chiamate server che non si aspettavano. È quindi consigliabile disattivare l&#39;inoltro [!UICONTROL tracking server] non appena si dispone di tutti i [!UICONTROL report suites] impostati in modo da poter essere portati avanti in modo appropriato per le esigenze aziendali.
