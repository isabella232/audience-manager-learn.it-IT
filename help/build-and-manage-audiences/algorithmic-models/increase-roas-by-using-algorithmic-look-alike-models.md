---
title: Aumenta ROAS utilizzando modelli algoritmici (simili a quelli in  Audience Manager)
description: La vera potenza di  Audience Manager  Modellazione simile viene quando si cerca di espandere il pubblico di riferimento rispetto a una qualità, nuovo set di utenti da origini dati di seconda e terza parte. Questa esercitazione illustra i passaggi necessari per creare un modello a partire da questi dati.
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# Aumenta ROAS utilizzando l&#39;algoritmo (simile a un aspetto) [!UICONTROL Models] nel Audience Manager  {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La vera potenza di  Audience Manager  Look-like [!UICONTROL Modeling] viene quando si cerca di espandere il pubblico di riferimento rispetto a una qualità, nuovo set di utenti di [!UICONTROL second party] e [!UICONTROL third party] [!UICONTROL data sources]. Questa esercitazione illustra i passaggi necessari per creare una [!UICONTROL model] da tali dati.

## Abilitare i flussi di dati [!UICONTROL Second Party] o [!UICONTROL Third Party] dal Audience Marketplace  {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Per utilizzare i dati [!UICONTROL second party] e [!UICONTROL third party] in un aspetto simile [!UICONTROL model], è innanzitutto necessario abilitare questi dati nell&#39;interfaccia del Audience Manager .  Adobe dispone di un numero elevato di provider di dati [!UICONTROL second party] e [!UICONTROL third party] da cui è possibile scegliere. Sono disponibili in un&#39;interfaccia self-service in AAM, tramite il Audience Marketplace . Andate al Audience Marketplace  e sfogliate le possibilità. Il seguente video illustra come eseguire questa operazione, inclusa la modalità per abilitare i flussi &quot;prova prima di acquistare&quot; gratuiti, in modo da poter bloccare i dati che saranno più utili per la tua organizzazione prima di impegnarsi a rispettare i prezzi del fornitore di dati.

Inoltre, per facilitare la ricerca e decidere su quale provider di dati utilizzare, una grande risorsa è la [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di far fare alla gente sul tuo sito? Qual è l&#39;evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e dei vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare un [!UICONTROL trait] per i visitatori che hanno soddisfatto tali criteri.

Nel video seguente, vi mostrerò come creare una conversione [!UICONTROL trait], che desiderate avere in posizione mentre continuate questa esercitazione e create un look-like [!UICONTROL model].

Inoltre, quando utilizzate  eventi Adobe Analytics per creare [!UICONTROL traits], è necessario tenere presente un gotcha importante, in modo da non raccogliere più utenti di quanto si dovrebbe nel [!UICONTROL trait]. Guardate il seguente video per la grande rivelazione. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che si abbia  Adobe Analytics. Ovviamente non è così. Se hai Google Analytics (GA), abbiamo un modulo che puoi utilizzare per inviare dati in AAM (vedi la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), e se la tua attività di conversione sul tuo sito viene inviata a AAM da GA, puoi creare la tua conversione [!UICONTROL trait] da questo. Se hai una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati a AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (vedere la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Quindi create la conversione [!UICONTROL trait] in base ai dati inviati quando l&#39;attività di conversione viene eseguita sul sito.

## Creare un look simile [!UICONTROL Model] da [!UICONTROL Second Party] o [!UICONTROL Third Party] dati {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Dopo aver completato i passaggi descritti qui sopra, ora siamo pronti per creare un algoritmo (sosia) [!UICONTROL Model]. Durante la configurazione del [!UICONTROL model], utilizzeremo la conversione [!UICONTROL trait] come base [!UICONTROL trait] (visitatori chiave che vogliamo duplicare), e utilizzeremo il flusso di dati abilitato [!UICONTROL third party] come pool di persone da cui prelevare.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una best practice importante {#an-important-best-practice}

Quando si crea l&#39;algoritmo [!UICONTROL model] in  Audience Manager, ovviamente vogliamo che la [!UICONTROL model] sia il più efficace possibile. Poiché la [!UICONTROL model] sta considerando tutti i [!UICONTROL traits] di cui fanno parte i membri della base [!UICONTROL trait]/[!UICONTROL segment], non aiuta la [!UICONTROL model] se TUTTE le persone si trovano in una [!UICONTROL trait]/[!UICONTROL segment]. Pertanto, se si dispone di un [!UICONTROL traits] super generico (come tutti coloro che sono andati sul sito, o tutti coloro che hanno ricevuto un annuncio da voi, ecc.), assicurarsi che il [!UICONTROL data source] a cui appartengono NON sia incluso nel [!UICONTROL data sources] nel [!UICONTROL model]. In questo caso d&#39;uso, è improbabile che tu possa farlo, perché stiamo concentrando l&#39;attenzione su [!UICONTROL third party] dati per i nostri nuovi look-alias, ma vale comunque la pena menzionare, e si applica a TUTTI i tuoi algoritmi [!UICONTROL models].

## Creazione di un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un algoritmo [!UICONTROL Trait], in modo da poter utilizzare i risultati di [!UICONTROL model]. Senza creare un [!UICONTROL trait], il modello è inutile. Quindi, dopo l&#39;esecuzione di [!UICONTROL model], assicurarsi di accedere alla finestra di dialogo [!UICONTROL trait] e creare un algoritmo [!UICONTROL Trait]. Il seguente video illustra la procedura e presenta alcuni suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creazione di un [!UICONTROL Segment] dai dati [!UICONTROL Model] e invio a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Dopo aver creato un algoritmo [!UICONTROL Trait], è possibile creare un nuovo [!UICONTROL segment] per inserirlo, in modo da poter attivare i dati (non è possibile attivare un [!UICONTROL trait], ma creare un nuovo [!UICONTROL trait] [!UICONTROL segment] con l&#39;algoritmo [!UICONTROL Trait] in modo da poter attivare (utilizzare) il [!UICONTROL segment]).

Dopo aver creato un [!UICONTROL segment] da questo algoritmo [!UICONTROL trait], avrai un pubblico di potenziali clienti che somigliano a quelli che si sono già convertiti sul tuo sito. Ora è possibile eseguire la mappatura di [!UICONTROL segment] a uno dei DSP [!UICONTROL destinations] nel Audience Manager . Potrai eseguire il targeting del tuo marketing per questi look-alias, che hanno più probabilità di convertire sul tuo sito rispetto al normale pubblico, aumentando il tuo Return on Ad Spend. Buona fortuna!
