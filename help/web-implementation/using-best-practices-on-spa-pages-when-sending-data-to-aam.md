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

In questo documento descriveremo diverse best practice da seguire e di cui dovreste essere consapevoli durante l&#39;invio di dati da [!UICONTROL Single Page Applications] (SPA) ad Adobe Audience Manager (AAM). Questo documento si concentra sull&#39;utilizzo di [!UICONTROL Experience Platform Launch], che è il metodo di implementazione consigliato.

## Note iniziali

* Gli elementi seguenti presupporranno che si stia utilizzando [!DNL Platform Launch] per implementare sul sito. Le considerazioni resterebbero valide se non si utilizza [!DNL Platform Launch], ma è necessario adattarle al metodo di implementazione.
* Tutte le SPA sono diverse, quindi potrebbe essere necessario modificare alcuni dei seguenti elementi per soddisfare al meglio le vostre esigenze, ma desideriamo condividere con voi alcune best practice; cose a cui devi pensare quando invii dati da SPA pagine a  Audience Manager.

## Diagramma semplice di lavoro con SPA e AAM in Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa per aam in  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Come indicato, si tratta di un diagramma semplificato del modo in cui SPA pagine vengono gestite in un&#39;implementazione Adobe Audience Manager (senza  Adobe Analytics) utilizzando [!DNL Platform Launch]. Come si può vedere, è abbastanza dritto, con la grande decisione è come si sta andando a comunicare un cambiamento di vista (o un&#39;azione) a [!DNL Platform Launch].

## Attivazione di [!DNL Launch] dalla pagina SPA {#triggering-launch-from-the-spa-page}

Due dei metodi più comuni per attivare una regola in [!DNL Platform Launch] (e quindi inviare dati  Audience Manager) sono:

* Impostazione di eventi JavaScript personalizzati (vedere esempio [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) con  Adobe Analytics)
* Utilizzo di un [!UICONTROL Direct Call Rule]

In questo  Audience Manager, utilizzeremo un [!UICONTROL Direct Call rule] in [!DNL Launch] per attivare l&#39;hit che va  Audience Manager. Come vedrete nelle sezioni successive, questo diventa molto utile impostando il [!UICONTROL Data Layer] su un nuovo valore, in modo che possa essere recuperato dal [!UICONTROL Data Element] in [!DNL Platform Launch].

## Pagina demo {#demo-page}

È stata creata una piccola pagina demo che illustra come modificare un valore in [!DNL data layer] e inviarlo a AAM, come potete fare in una pagina SPA. Questa funzionalità può essere modellata per apportare modifiche più complesse. Questa è la pagina demo [HERE](https://aam.enablementadobe.com/SPA-Launch.html).

## Impostazione di [!DNL data layer] {#setting-the-data-layer}

Come già detto, quando un nuovo contenuto viene caricato sulla pagina o quando un utente esegue un&#39;azione sul sito, [!DNL data layer] deve essere impostato dinamicamente nell&#39;intestazione della pagina prima che [!DNL Launch] venga chiamato ed esegua il [!UICONTROL rules], in modo che [!DNL Platform Launch] possa acquisire i nuovi valori dal [!DNL data layer] e inviarli al  Audience Manager.

Se andate al sito demo elencato sopra e guardate la sorgente della pagina, vedrete:

* La [!DNL data layer] si trova nell&#39;intestazione della pagina, prima della chiamata a [!DNL Platform Launch]
* Il codice JavaScript nel collegamento simulato SPA modifica la chiamata [!UICONTROL Data Layer], quindi la chiamata [!DNL Platform Launch] (la chiamata _satellite.track()). Se si utilizzano eventi personalizzati JavaScript invece di questo [!UICONTROL Direct Call Rule], la lezione è la stessa. Modificare prima il [!DNL data layer], quindi chiamare [!DNL Launch].

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Risorse aggiuntive {#additional-resources}

* [SPA discussione sui forum del Adobe ](https://forums.adobe.com/thread/2451022)
* [Siti di architettura di riferimento per mostrare come implementare SPA in Launch by Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Utilizzo delle best practice per il tracciamento dei SPA in  Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Sito demo utilizzato per questo articolo](https://aam.enablementadobe.com/SPA-Launch.html)
