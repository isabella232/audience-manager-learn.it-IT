---
title: Utilizzo delle best practice sulle SPA pagine durante l’invio di dati a AAM
description: In questo documento verranno descritte diverse best practice da seguire e di cui tenere conto durante l’invio di dati da applicazioni a pagina singola (SPA) a Adobe Audience Manager (AAM). Questo documento si concentrerà sull'utilizzo di Launch by Adobe, che è il metodo di implementazione consigliato.
feature: Nozioni di base sull’implementazione
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# Utilizzo delle best practice sulle SPA pagine durante l’invio di dati a AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

In questo documento, descriveremo diverse best practice da seguire e di cui dovresti essere a conoscenza durante l’invio di dati da [!UICONTROL Single Page Applications] (SPA) a Adobe Audience Manager (AAM). Questo documento si concentra sull&#39;utilizzo di [!UICONTROL Experience Platform Launch], che è il metodo di implementazione consigliato.

## Note iniziali

* Gli elementi seguenti presupporranno l&#39;utilizzo di [!DNL Platform Launch] per l&#39;implementazione sul sito. Le considerazioni esistono ancora se non utilizzi [!DNL Platform Launch], ma dovrai adattarle al metodo di implementazione.
* Tutte le SPA sono diverse, quindi potrebbe essere necessario modificare alcuni dei seguenti elementi per soddisfare al meglio le tue esigenze, ma abbiamo voluto condividere alcune best practice con te; cose a cui devi pensare quando invii dati da pagine SPA ad Audience Manager.

## Diagramma semplice del lavoro con SPA e AAM nel Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![spa per aam in  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>Come indicato, questo è un diagramma semplificato del modo in cui SPA pagine vengono gestite in un’implementazione Adobe Audience Manager (senza Adobe Analytics) utilizzando [!DNL Platform Launch]. Come puoi vedere, è abbastanza dritta, con la grande decisione è come comunicare un cambiamento di visualizzazione (o un&#39;azione) a [!DNL Platform Launch].

## Attivazione di [!DNL Launch] dalla pagina SPA {#triggering-launch-from-the-spa-page}

Due dei metodi più comuni per attivare una regola in [!DNL Platform Launch] (e quindi inviare dati ad Audience Manager) sono:

* Impostazione di eventi personalizzati JavaScript (vedi esempio [HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) con Adobe Analytics)
* Utilizzo di [!UICONTROL Direct Call Rule]

In questo Audience Manager, utilizzeremo un [!UICONTROL Direct Call rule] in [!DNL Launch] per attivare l&#39;hit che entra in Audience Manager. Come vedrai nelle sezioni successive, questo diventa molto utile impostando il valore [!UICONTROL Data Layer] su un nuovo valore, in modo che possa essere raccolto dal valore [!UICONTROL Data Element] in [!DNL Platform Launch].

## Pagina demo {#demo-page}

Abbiamo creato una piccola pagina demo che illustra come modificare un valore in [!DNL data layer] e inviarlo a AAM, come puoi fare in una pagina SPA. Questa funzionalità può essere modellata per apportare modifiche più elaborate necessarie. È possibile trovare questa pagina demo [HERE](https://aam.enablementadobe.com/SPA-Launch.html).

## Impostazione di [!DNL data layer] {#setting-the-data-layer}

Come accennato, quando un nuovo contenuto viene caricato sulla pagina o quando un utente esegue un&#39;azione sul sito, è necessario impostare in modo dinamico l&#39; [!DNL data layer] nell&#39;intestazione della pagina PRIMA che venga chiamato [!DNL Launch] ed esegua l&#39; [!UICONTROL rules], in modo che [!DNL Platform Launch] possa raccogliere i nuovi valori da [!DNL data layer] e inviarli nell&#39;Audience Manager.

Se visiti il sito demo elencato sopra e osserva la sorgente della pagina, vedrai:

* Il [!DNL data layer] si trova nella parte superiore della pagina, prima della chiamata a [!DNL Platform Launch]
* Il JavaScript nel collegamento simulato SPA cambia il [!UICONTROL Data Layer] e QUINDI le chiamate [!DNL Platform Launch] (la chiamata _satellite.track() ). Se stavi utilizzando eventi personalizzati JavaScript invece di questo [!UICONTROL Direct Call Rule], la lezione è la stessa. Modificare prima il [!DNL data layer], quindi chiamare [!DNL Launch].

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## Risorse aggiuntive {#additional-resources}

* [SPA discussione sui forum Adobi](https://forums.adobe.com/thread/2451022)
* [Siti dell’architettura di riferimento per mostrare come implementare SPA in Launch by Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Utilizzo delle best practice per il tracciamento dei SPA in Adobe Analytics](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [Sito demo utilizzato per questo articolo](https://aam.enablementadobe.com/SPA-Launch.html)
