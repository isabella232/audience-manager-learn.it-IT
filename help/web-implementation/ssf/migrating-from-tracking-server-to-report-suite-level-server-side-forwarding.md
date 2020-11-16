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


# Migrazione da [!UICONTROL Tracking Server] a [!UICONTROL Report Suite]livello [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

Questo articolo e questo video mostrano come abilitare [!UICONTROL server-side forwarding] i [!DNL Analytics] dati per  Audience Manager a un [!UICONTROL report suite] livello invece che a un [!UICONTROL tracking server] livello.

## Introduzione {#introduction}

Se si dispone di Adobe Audience Manager E  Adobe Analytics, è possibile implementare &quot;[!UICONTROL Server-side Forwarding]&quot; dei [!DNL Analytics] dati per  Audience Manager. Questo significa che, invece di inviare 2 hit alla pagina (uno a [!DNL Analytics] e uno a  Audience Manager), può semplicemente inviare un hit a [!DNL Analytics], e [!DNL Analytics] inoltrare tali dati a  Audience Manager. Se questo è già in funzione e lo avete già attivato o implementato prima di ottobre 2017, [!UICONTROL server-side forwarding] potreste essere basati sul vostro &quot;[!UICONTROL Tracking Server]&quot;, che doveva essere attivato dall&#39;Assistenza clienti di  Adobe o  Consulenza Adobe. A partire da ottobre 2017, ora puoi configurare [!UICONTROL server-side forwarding] te stesso e farlo a [!UICONTROL Report Suite] livello (inoltro PER [!UICONTROL Report Suite]). A questo proposito si aprono notevoli vantaggi, che saranno discussi di seguito.

## [!UICONTROL Tracking Server] Inoltro {#tracking-server-forwarding}

Il percorso in cui [!UICONTROL tracking server] vengono inviati [!DNL Analytics] i dati e anche il dominio in cui vengono scritti la richiesta di immagini e il cookie. Deve essere impostato in Gestione dinamica dei tag o [!DNL Experience Platform Launch], o nel [!DNL AppMeasurement.js] file, e avrà in genere questo aspetto, con il tuo sito o nome commerciale che sostituisce &quot;miosito&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se [!UICONTROL server-side forwarding] è impostato per l’inoltro a [!UICONTROL tracking server] livello, qualsiasi hit che viene inviato a questo [!UICONTROL tracking server] (se è abilitato anche il servizio ID Experience Cloud) verrà inoltrato a  Audience Manager. Questa funzione doveva essere abilitata dall&#39;Assistenza clienti  Adobe o dalla Consulenza  Adobe. Sono anche quelli che possono disattivarlo, DOPO che hai passato all&#39; [!UICONTROL report suite] inoltro, come descritto di seguito.

Se non sei sicuro se [!DNL tracking server forwarding] è abilitato, contatta l&#39;Assistenza clienti  Adobe o la Consulenza  Adobe e questi devono essere in grado di avvisarti.

## [!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

Uno dei maggiori vantaggi per passare all&#39; [!UICONTROL report suite] inoltro dall&#39; [!UICONTROL tracking server] inoltro è che ora sarà in grado di utilizzare &quot; Audience Analytics&quot;, che è la capacità di inoltrare  Audience Manager [!UICONTROL segments] nuovamente  Adobe Analytics per un&#39; [!UICONTROL segment] analisi dettagliata. Questa grande funzionalità NON è supportata se siete ancora in [!UICONTROL tracking server] avanzamento e non [!UICONTROL report suite] in inoltro. Per ulteriori informazioni  Audience Analytics, consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, dopo aver impostato tutte le [!UICONTROL report suites] opzioni da inoltrare per  Audience Manager, è necessario contattare l&#39;Assistenza clienti  Adobe o  Consulenza Adobe e disattivare l&#39; [!UICONTROL tracking server] inoltro. Non è un&#39;emergenza per voi fare questo, perché avere sia [!UICONTROL tracking server] l&#39;inoltro che [!UICONTROL report suite] l&#39;inoltro NON si tradurrà in hit duplicati. Tuttavia, è buona norma solo [!UICONTROL report suite] inoltrare. Se [!UICONTROL tracking server] continuate, non solo potreste inoltrare i dati da [!UICONTROL report suites] cui non volete essere inoltrati, ma in futuro, dopo che voi (e tutti quelli della vostra azienda) avete dimenticato che [!UICONTROL tracking server] l&#39;inoltro è attivo, potreste pensare che i dati non stiano inoltrando per uno specifico [!UICONTROL report suite] (perché non è attivato a livello di suite di rapporti), ma i dati sono comunque inoltrati a causa del [!UICONTROL tracking server]. In questo modo si perderanno tempo e denaro per capire il motivo dell&#39;inoltro e per pagare AAM chiamate server che non si aspettavano. È quindi consigliabile disattivare l&#39; [!UICONTROL tracking server] inoltro non appena si dispone di tutti i [!UICONTROL report suites] set per l&#39;avanzamento, adatti alle esigenze aziendali.
