---
title: Migrazione dal server di tracciamento all’inoltro lato server a livello di suite di rapporti
description: Scopri come abilitare l’inoltro lato server dei dati di Adobe Analytics ad Audience Manager a livello di suite di rapporti invece che a livello di server di tracciamento.
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

Questo articolo e questo video ti mostreranno come abilitare l’inoltro lato server di [!DNL Analytics] Dati da Audience Manager in una [!UICONTROL report suite] livello anziché a [!UICONTROL tracking server] livello.

## Introduzione {#introduction}

Se disponi di Adobe Audience Manager E Adobe Analytics, puoi implementare l&#39;inoltro lato server della [!DNL Analytics] dati ad Audience Manager. Questo significa che, invece di inviare due hit alla pagina (uno a [!DNL Analytics] e uno ad Audience Manager), può inviare un hit a [!DNL Analytics]e [!DNL Analytics] inoltrerà tali dati all&#39;Audience Manager.

Se questo è già in esecuzione e se lo hai abilitato/implementato prima di ottobre 2017, l&#39;inoltro lato server potrebbe essere basato sul [!UICONTROL Tracking Server], che doveva essere abilitata dall’Assistenza clienti Adobe o dalla Consulenza Adobe. A partire da ottobre 2017, ora puoi configurare autonomamente l’inoltro lato server e farlo a livello di suite di rapporti (inoltro per suite di rapporti). Ciò comporta vantaggi significativi, come illustrato di seguito.

## [!UICONTROL Tracking server] inoltro {#tracking-server-forwarding}

Le [!UICONTROL tracking server] è la posizione a cui stai inviando il tuo [!DNL Analytics] e il dominio in cui vengono scritti la richiesta di immagini e il cookie. Deve essere impostato in DTM o [!DNL Experience Platform Launch]oppure [!DNL AppMeasurement.js] file e sarà in genere simile a questo, con il tuo sito o nome commerciale che sostituisce &quot;miosito&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se l&#39;inoltro lato server è impostato per l&#39;inoltro all&#39;indirizzo [!UICONTROL tracking server] livello, qualsiasi hit inviato a questo [!UICONTROL tracking server] (SE è abilitato anche il servizio Experience Cloud ID) verrà inoltrato ad Audience Manager. Questo doveva essere abilitato dall’Assistenza clienti o dalla Consulenza Adobe di Adobe. Sono anche quelli che possono disabilitarlo, DOPO che hai cambiato in [!UICONTROL report suite] inoltro, come descritto di seguito.

Se non sei sicuro se [!DNL tracking server forwarding] è abilitato per te, contatta l’Assistenza clienti Adobe o la Consulenza Adobe e devono essere in grado di avvisarti.

## [!UICONTROL Report-suite]Inoltro lato server a livello di {#report-suite-level-server-side-forwarding}

Uno dei maggiori vantaggi del passaggio a [!UICONTROL report suite] inoltro da [!UICONTROL tracking server] l&#39;inoltro è che ora potrai utilizzare &quot;Audience Analytics&quot;, che è la capacità di inoltrare l&#39;Audience Manager [!UICONTROL segments] torna ad Adobe Analytics per un’analisi dettagliata dei segmenti. Questa grande funzione NON è supportata se sei ancora attivo [!UICONTROL tracking server] inoltro e non [!UICONTROL report suite] inoltro. Per ulteriori informazioni sull’Audience Analytics in [documentazione](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, una volta che hai tutte le [!UICONTROL report suites] impostato per inoltrare l&#39;inoltro all&#39;Audience Manager, contatta l&#39;Assistenza clienti o la Consulenza Adobe di Adobe e fai loro disabilitare il [!UICONTROL tracking server] inoltro. Non è un&#39;emergenza per voi fare questo, perché avere entrambi [!UICONTROL tracking server] inoltro e [!UICONTROL report suite] l&#39;inoltro non comporta risultati duplicati. Tuttavia, è buona prassi solo [!UICONTROL report suite] avanti.

Se te ne vai [!UICONTROL tracking server] inoltro, non solo può inoltrare dati da [!UICONTROL report suites] che non vuoi inoltrare, ma in futuro, dopo che tu (e tutti quelli della tua azienda) hai dimenticato che [!UICONTROL tracking server] inoltro attivato, potresti pensare che i dati non vengano inoltrati per uno specifico [!UICONTROL report suite]. Questo perché non è attivato a livello di suite di rapporti, ma i dati vengono comunque inoltrati a causa del [!UICONTROL tracking server]. A quel punto perderai tempo e denaro per capire perché sta inoltrando e anche pagando AAM chiamate server che non ti aspettavi. Quindi, è una buona idea disabilitare [!UICONTROL tracking server] inoltro non appena hai tutti i [!UICONTROL report suites] impostare in modo da soddisfare le esigenze aziendali.
