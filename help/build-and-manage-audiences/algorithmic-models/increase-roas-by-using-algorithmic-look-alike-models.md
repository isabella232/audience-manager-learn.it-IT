---
title: Aumento del ROAS utilizzando modelli algoritmici (lookalike) in Audience Manager
description: La potenza reale della Modellazione lookalike di Audience Manager viene fornita quando si cerca di espandere il pubblico di riferimento rispetto a un nuovo set di utenti di qualità provenienti da fonti di dati di seconde e terze parti. In questa esercitazione, scopri i passaggi per creare un modello a partire da questi dati.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: "Business Practitioner, Developer, Data Engineer, Architect, Data Architect, Administrator, Leader"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 0%

---


# Aumenta il ROAS utilizzando l’algoritmo (lookalike) [!UICONTROL Models] in Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La vera potenza del look-alike di Audience Manager [!UICONTROL Modeling] si ottiene quando si cerca di espandere il pubblico di riferimento rispetto a un nuovo set di utenti di qualità di [!UICONTROL second party] e [!UICONTROL third party] [!UICONTROL data sources]. In questa esercitazione, scopri i passaggi necessari per creare un [!UICONTROL model] da questi dati.

## Abilitare i flussi di dati [!UICONTROL Second Party] o [!UICONTROL Third Party] da Audience Marketplace {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Per utilizzare i dati [!UICONTROL second party] e [!UICONTROL third party] in un look-alike [!UICONTROL model], è necessario prima abilitare tali dati nell’interfaccia di Audience Manager. Adobe dispone di un numero elevato di provider di dati [!UICONTROL second party] e [!UICONTROL third party] tra cui è possibile scegliere. Sono disponibili in un’interfaccia self-service in AAM, tramite Audience Marketplace. Passa a Audience Marketplace e naviga tra le varie possibilità. Il video seguente illustra come eseguire questa operazione, tra cui come abilitare i flussi &quot;try before you buy&quot; gratuiti, in modo da poter bloccare i dati che saranno più utili per la tua organizzazione prima di impegnarti nei prezzi del provider di dati.

Inoltre, per aiutarti a cercare e decidere quale provider di dati utilizzare, una grande risorsa è la [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di fare alle persone sul tuo sito? Qual è l’evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e dei vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare un [!UICONTROL trait] per i visitatori che hanno soddisfatto tali criteri.

Nel video seguente, vi mostrerò come creare una conversione [!UICONTROL trait], che vorrai avere in posizione mentre prosegui questa esercitazione e creare un look-alike [!UICONTROL model].

Inoltre, quando utilizzi gli eventi di Adobe Analytics per creare [!UICONTROL traits], è necessario tenere presente un importante gotcha, in modo da non raccogliere più utenti di quanto dovresti nel [!UICONTROL trait]. Guarda il seguente video per la grande rivelazione. :

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente non è così. Se disponi di Google Analytics (GA), disponiamo di un modulo che puoi utilizzare per inviare dati in AAM (consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), e se l’attività di conversione sul tuo sito viene inviata ad AAM da GA, puoi creare la tua conversione [!UICONTROL trait] da questo. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati ad AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Quindi crea la conversione [!UICONTROL trait] in base ai dati inviati quando l&#39;attività di conversione viene eseguita sul sito.

## Crea un lookalike [!UICONTROL Model] da [!UICONTROL Second Party] o [!UICONTROL Third Party] dati {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Dopo aver completato i passaggi precedenti, ora siamo pronti per creare un algoritmo (lookalike) [!UICONTROL Model]. Durante la configurazione del [!UICONTROL model], utilizzeremo la conversione [!UICONTROL trait] come base [!UICONTROL trait] (visitatori chiave da duplicare) e il flusso di dati abilitato [!UICONTROL third party] verrà utilizzato come pool di persone da cui effettuare il pull.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una best practice importante {#an-important-best-practice}

Quando crei l’algoritmo [!UICONTROL model] in Audience Manager, ovviamente vogliamo che il [!UICONTROL model] sia il più efficace possibile. Poiché il [!UICONTROL model] sta considerando tutti i [!UICONTROL traits] di cui fanno parte i membri della base [!UICONTROL trait]/[!UICONTROL segment], non aiuta il [!UICONTROL model] se TUTTE le persone si trovano in un [!UICONTROL trait]/[!UICONTROL segment]. Pertanto, se disponi di un [!UICONTROL traits] super generico (come tutti coloro che sono andati sul tuo sito, o tutti coloro che hanno ricevuto un annuncio da te, ecc.), assicurati che il [!UICONTROL data source] a cui appartengono NON sia incluso nel [!UICONTROL data sources] nel tuo [!UICONTROL model]. Nel caso d’uso di questo articolo, è improbabile che tu lo faccia, perché ci stiamo concentrando sulla ricerca di dati [!UICONTROL third party] per i nostri nuovi look-alikes, ma vale la pena menzionarlo comunque, e si applica a TUTTI i tuoi algoritmi [!UICONTROL models].

## Creazione di un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un algoritmo [!UICONTROL Trait] in modo che sia possibile utilizzare i risultati di [!UICONTROL model]. Senza creare un [!UICONTROL trait], il modello è inutile. Quindi, dopo l&#39;esecuzione di [!UICONTROL model], assicurati di accedere alla finestra di dialogo [!UICONTROL trait] e creare un algoritmo [!UICONTROL Trait]. Il seguente video illustra la procedura dettagliata e mostra un paio di suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creazione di un [!UICONTROL Segment] dai dati [!UICONTROL Model] e invio a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Dopo aver creato un algoritmo [!UICONTROL Trait], è possibile creare un nuovo [!UICONTROL segment] per inserirlo, in modo da poter attivare i dati (non è possibile attivare un [!UICONTROL trait], ma crearne uno nuovo-[!UICONTROL trait] [!UICONTROL segment] con l&#39;algoritmo [!UICONTROL Trait] in modo da poter attivare (utilizzare) il [!UICONTROL segment]).

Dopo aver creato un [!UICONTROL segment] da questo algoritmo [!UICONTROL trait], avrai a disposizione un pubblico di potenziali clienti che assomiglia a persone che si sono già convertite sul tuo sito. Ora puoi mappare questo elemento [!UICONTROL segment] a qualsiasi DSP [!UICONTROL destinations] in Audience Manager. Potrai eseguire il targeting del tuo marketing per quei look-alikes, che hanno più probabilità di convertire sul tuo sito che solo il pubblico normale, aumentando il tuo Return on Ad Spend. Buona fortuna!
