---
title: Come identificare l'ID partner  Audience Manager o il sottodominio
description: Durante l'implementazione di alcune funzionalità  Experience Cloud, è necessario conoscere l'ID del proprio Audience Manager  "ID partner" (anche noto come "ID cliente" o "Sottodominio"). In questo video verranno mostrati due punti in cui è possibile ottenere questo ID nell’interfaccia utente del Audience Manager .
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# Come identificare il sottodominio  Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Durante l&#39;implementazione di alcune funzioni  Experience Cloud, è necessario conoscere il Audience Manager  `Subdomain` (a volte denominato anche `client ID` o `Partner ID`). In questo video verranno mostrati due punti in cui è possibile ottenere queste informazioni nell’interfaccia utente del Audience Manager .

## Dando via il finale... {#giving-away-the-ending}

Se preferite semplicemente entrare e trovarlo senza guardare questo breve video, potete trovare il vostro `Partner Subdomain` in due punti nell&#39;interfaccia:

1. Se avete già creato un [!UICONTROL rule-based] oggetto [!UICONTROL trait], fate clic su **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] è accanto all’ [!UICONTROL trait] nell’elenco di [!UICONTROL traits] tale cartella e l’URL includerà il vostro sottodominio nell’URL.
1. Se andate nell&#39;interfaccia **[!UICONTROL Tools]** > **[!UICONTROL Tags]** e fate clic **[!UICONTROL Get code]** per il contenitore, il sottodominio è verso la fine della linea Akamai

Se non riesci a trovarlo rapidamente con questi riferimenti rapidi, il video è un breve impegno. :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>A ciascun cliente dell&#39;Adobe Experience Cloud è assegnato un ID numerico, spesso denominato &quot;PID&quot; o ID partner. Questo non è l&#39;ID di cui stiamo parlando in questo articolo e video. Al contrario, il &quot;sottodominio partner&quot;, a volte denominato ID partner, è in genere una versione del nome client ed è il sottodominio del server a cui vengono inviati i dati. Ad esempio, se la vostra azienda è &quot;Bob&#39;s Knobs&quot; (tutte le cose che porta, ovviamente, haha), allora è probabile che il vostro sottodominio partner sia &quot;bobsknobs&quot;, mentre il &quot;PID&quot; sarebbe qualcosa di più come &quot;12345&quot;. In genere non è necessario conoscere il PID, ma il sottodominio è importante conoscerlo, in modo da poter configurare l&#39;implementazione del Audience Manager .

