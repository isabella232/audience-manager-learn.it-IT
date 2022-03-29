---
title: Aumenta il ROAS utilizzando modelli algoritmici (lookalike)
description: La vera potenza della Modellazione lookalike di Audience Manager viene fornita quando si cerca di espandere il pubblico di riferimento rispetto a una qualità, un nuovo set di utenti da fonti di dati di seconde e terze parti. In questa esercitazione, scopri i passaggi per creare un modello a partire da questi dati.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# Aumenta il ROAS utilizzando modelli algoritmici (lookalike) in Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Il vero potere del look-alike dell&#39;Audience Manager [!UICONTROL Modeling] viene fornito quando cerchi di espandere il pubblico di base rispetto a un set di utenti di qualità e nuovo di zecca provenienti da origini dati di seconde e terze parti. In questa esercitazione, scopri i passaggi necessari per creare un modello a partire da questi dati.

## Abilitare i flussi di dati di seconde o terze parti dall’Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Per utilizzare dati di seconde e terze parti in un modello lookalike, è necessario prima abilitare questi dati nell’interfaccia di Audience Manager. Adobe dispone di un numero elevato di provider di dati di seconde e terze parti da cui è possibile scegliere. Sono disponibili in un’interfaccia self-service in AAM tramite l’Audience Marketplace . Passa all’Audience Marketplace e sfoglia le possibilità. Il video seguente illustra come eseguire questa operazione, tra cui come abilitare i flussi &quot;try before you buy&quot; gratuiti, in modo da poter bloccare i dati che saranno più utili per la tua organizzazione prima di impegnarti nei prezzi del provider di dati.

Inoltre, per aiutarti a cercare e decidere su quale provider di dati utilizzare, una grande risorsa è la [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificare o creare una caratteristica o un segmento dell’utente ideale (conversione) {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di fare alle persone sul tuo sito? Qual è l’evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e i vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare una caratteristica per i visitatori che hanno soddisfatto tali criteri.

Nel video seguente, vi mostrerò come creare una caratteristica di conversione, che vorrete avere in atto mentre proseguite questa esercitazione e create un modello lookalike.

Inoltre, quando si utilizzano gli eventi di Adobe Analytics per creare le caratteristiche, è necessario tenere presente un importante elemento gotcha, in modo da non raccogliere più utenti di quanti dovrebbero nella caratteristica. Guarda il seguente video per la grande rivelazione. :

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l’esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente non è così. Se disponi di Google Analytics (GA), è disponibile un modulo che puoi utilizzare per inviare dati a AAM (consulta la sezione [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)) e se l’attività di conversione sul sito viene inviata a AAM da GA, puoi creare la caratteristica di conversione da tale attività. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare i dati a AAM tramite il nostro codice DIL e il `submit` funzioni, ecc. (vedi [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)). Quindi crea la caratteristica di conversione in base ai dati inviati quando l’attività di conversione viene eseguita sul sito.

## Creare un modello lookalike da dati di seconde o terze parti {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Dopo aver completato i passaggi precedenti, siamo ora pronti per creare un modello algoritmico (lookalike). Durante la configurazione del modello, utilizzeremo la caratteristica di conversione come caratteristica di base (visitatori chiave da duplicare) e il flusso di dati di terze parti abilitato come pool di persone da cui prelevare.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una best practice importante {#an-important-best-practice}

Quando si crea il modello algoritmico in Audience Manager, ovviamente vogliamo che il modello sia il più efficace possibile. Poiché il modello sta considerando tutte le caratteristiche di cui fanno parte i membri della caratteristica/segmento di base, non aiuta il modello se TUTTE le persone si trovano in una caratteristica/segmento. Pertanto, se disponi di caratteristiche super generiche (come tutti gli utenti che hanno visitato il tuo sito o tutti coloro che hanno ricevuto annunci da te, ecc.), assicurati che l&#39;origine dati a cui appartengono NON sia inclusa nelle origini dati nel tuo modello. Nel caso d’uso di questo articolo, è improbabile che tu lo faccia, perché ci stiamo concentrando sulla ricerca di dati di terze parti per i nostri nuovi look-alias, ma vale la pena menzionarlo comunque, e si applica a TUTTI i tuoi modelli algoritmici.

## Crea un [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un  [!UICONTROL Algorithmic Trait], in modo da poter utilizzare i risultati del modello. Senza creare un tratto, il modello è inutile. Quindi, dopo l&#39;esecuzione del modello, assicurati di entrare nella finestra di dialogo delle caratteristiche e creare un [!UICONTROL Algorithmic Trait]. Il seguente video illustra la procedura dettagliata e mostra un paio di suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Crea un segmento dai dati del modello e invialo a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Una volta creata una [!UICONTROL Algorithmic Trait], puoi creare un nuovo segmento per inserirlo, in modo da poter attivare i dati (non puoi attivare una caratteristica, ma piuttosto creare un nuovo segmento a una caratteristica con la [!UICONTROL Algorithmic Trait] in modo da poter attivare (utilizzare) il segmento.

Dopo aver creato un segmento da questa caratteristica algoritmica, avrai a disposizione un pubblico di potenziali clienti che assomiglia a persone che si sono già convertite sul tuo sito. Ora puoi mappare questo segmento a una qualsiasi delle tue destinazioni DSP nell’Audience Manager. Potrai eseguire il targeting del tuo marketing per quei look-alikes, che hanno più probabilità di convertire sul tuo sito che solo il pubblico normale, aumentando il tuo Return on Ad Spend.
