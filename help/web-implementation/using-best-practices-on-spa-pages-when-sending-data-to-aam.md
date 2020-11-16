---
title: Utilizzo delle procedure ottimali SPA pagine durante l'invio di dati a AAM
description: In questo documento verranno descritte le best practice da seguire e di cui si deve essere a conoscenza durante l'invio di dati da applicazioni a pagina singola (SPA) ad Adobe Audience Manager (AAM). Questo documento si concentrerà sull'utilizzo di Launch by Adobe, che è il metodo di implementazione consigliato.
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# Utilizzo delle procedure ottimali SPA pagine durante l&#39;invio di dati a AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

In questo documento descriveremo diverse best practice da seguire e di cui sei consapevole quando invii dati da [!UICONTROL Single Page Applications] (SPA) ad Adobe Audience Manager (AAM). Questo documento si concentra sull&#39;utilizzo [!UICONTROL Experience Platform Launch], che è il metodo di implementazione consigliato.

## Note iniziali

* Gli elementi seguenti presupporranno che si stia utilizzando [!DNL Platform Launch] per implementare sul sito. Se non utilizzi [!DNL Platform Launch]le considerazioni, devi adattarle al metodo di implementazione.
* Tutte le SPA sono diverse, quindi potrebbe essere necessario modificare alcuni dei seguenti elementi per soddisfare al meglio le vostre esigenze, ma desideriamo condividere con voi alcune best practice; cose a cui devi pensare quando invii dati da SPA pagine a  Audience Manager.

## Diagramma semplice di lavoro con SPA e AAM in Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa per aam in [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Come già detto, questo è un diagramma semplificato di come SPA pagine vengono gestite in un&#39;implementazione Adobe Audience Manager (senza  Adobe Analytics) utilizzando [!DNL Platform Launch]. Come potete vedere, è abbastanza dritto, con la grande decisione è come comunicare un cambiamento di vista (o un&#39;azione) a [!DNL Platform Launch].

## Attivazione [!DNL Launch] dalla pagina SPA {#triggering-launch-from-the-spa-page}

Due dei metodi più comuni per attivare una regola in [!DNL Platform Launch] (e quindi inviare dati  Audience Manager) sono:

* Impostazione di eventi personalizzati JavaScript (vedere l&#39;esempio [QUI](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) con  Adobe Analytics)
* Utilizzo di un [!UICONTROL Direct Call Rule]

In questo  Audience Manager, utilizzeremo un [!UICONTROL Direct Call rule] componente per attivare [!DNL Launch] l&#39;hit che va  Audience Manager. Come vedrete nelle sezioni successive, questo diventa molto utile impostando il valore [!UICONTROL Data Layer] su un nuovo valore, in modo che possa essere recuperato dal [!UICONTROL Data Element] in [!DNL Platform Launch].

## Pagina demo {#demo-page}

È stata creata una piccola pagina demo che illustra come modificare un valore nella pagina [!DNL data layer] e inviarlo a AAM, come potete fare in una pagina SPA. Questa funzionalità può essere modellata per apportare modifiche più complesse. È possibile trovare questa pagina demo [QUI](https://aam.enablementadobe.com/SPA-Launch.html).

## Impostazione della variabile [!DNL data layer] {#setting-the-data-layer}

Come già detto, quando un nuovo contenuto viene caricato sulla pagina o quando un utente esegue un&#39;azione sul sito, [!DNL data layer] deve essere impostato in modo dinamico nell&#39;intestazione della pagina PRIMA [!DNL Launch] di essere chiamato ed esegue il [!UICONTROL rules], in modo da [!DNL Platform Launch] poter raccogliere i nuovi valori dal [!DNL data layer] e spingerli in  Audience Manager.

Se andate al sito demo elencato sopra e guardate la sorgente della pagina, vedrete:

* L&#39;oggetto [!DNL data layer] si trova nell&#39;intestazione della pagina, prima della chiamata a [!DNL Platform Launch]
* Il codice JavaScript nel collegamento simulato SPA modifica le chiamate [!UICONTROL Data Layer]e THEN [!DNL Platform Launch] (chiamata _satellite.track()). Se invece utilizzate eventi JavaScript personalizzati, la lezione è la stessa [!UICONTROL Direct Call Rule]. Prima cambiare il [!DNL data layer]nome e poi chiamare [!DNL Launch].

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Risorse aggiuntive {#additional-resources}

* [SPA discussione sui forum del Adobe](https://forums.adobe.com/thread/2451022)
* [Siti di architettura di riferimento per mostrare come implementare SPA in Launch by Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Utilizzo delle best practice per il tracciamento dei SPA in  Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Sito demo utilizzato per questo articolo](https://aam.enablementadobe.com/SPA-Launch.html)
