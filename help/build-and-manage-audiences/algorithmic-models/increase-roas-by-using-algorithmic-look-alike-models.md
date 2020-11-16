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


# Aumenta ROAS utilizzando l&#39;algoritmo (simile all&#39;aspetto) [!UICONTROL Models] in  Audience Manager {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

La vera potenza di  Audience Manager  look [!UICONTROL Modeling] viene quando si cerca di ampliare il pubblico di riferimento rispetto a una qualità, un nuovo gruppo di utenti da [!UICONTROL second party] e [!UICONTROL third party][!UICONTROL data sources]. Questa esercitazione illustra i passaggi necessari per creare un [!UICONTROL model] tratto da questi dati.

## Abilitare [!UICONTROL Second Party] o [!UICONTROL Third Party] i flussi di dati dal Audience Marketplace  {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

Per utilizzare [!UICONTROL second party] e [!UICONTROL third party] i dati in modo simile [!UICONTROL model], dobbiamo prima abilitare questi dati nell&#39;interfaccia del Audience Manager .  Adobe dispone di un numero elevato di provider [!UICONTROL second party] e [!UICONTROL third party] di dati tra cui è possibile scegliere. Sono disponibili in un&#39;interfaccia self-service in AAM, tramite il Audience Marketplace . Andate al Audience Marketplace  e sfogliate le possibilità. Il seguente video illustra come eseguire questa operazione, inclusa la modalità per abilitare i flussi &quot;prova prima di acquistare&quot; gratuiti, in modo da poter bloccare i dati che saranno più utili per la tua organizzazione prima di impegnarsi a rispettare i prezzi del fornitore di dati.

Inoltre, per aiutarti a fare ricerca e decidere su quale provider di dati utilizzare, una grande risorsa è il [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di far fare alla gente sul tuo sito? Qual è l&#39;evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e dei vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare un messaggio [!UICONTROL trait] per i visitatori che hanno soddisfatto tali criteri.

Nel video seguente, vi mostrerò come creare una conversione [!UICONTROL trait], che desiderate avere in atto mentre proseguite con questa esercitazione e create un look simile [!UICONTROL model].

Inoltre, quando si utilizzano  eventi Adobe Analytics per creare [!UICONTROL traits], è necessario tenere presente un gotcha importante, in modo da non raccogliere più utenti di quanto si dovrebbe fare nel [!UICONTROL trait]. Guardate il seguente video per la grande rivelazione. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che si disponga  Adobe Analytics. Ovviamente non è così. Se hai Google Analytics (GA), abbiamo un modulo che puoi usare per inviare dati in AAM (vedi la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), e se la tua attività di conversione sul tuo sito viene inviata a AAM da GA, puoi creare la tua conversione [!UICONTROL trait] . Se hai una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati a AAM tramite il nostro codice DIL e la `submit` funzione, ecc. (consultare la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Quindi create la conversione [!UICONTROL trait] in base ai dati inviati quando l&#39;attività di conversione viene eseguita sul sito.

## Creare un look simile [!UICONTROL Model] da [!UICONTROL Second Party] o [!UICONTROL Third Party] dati {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

Dopo aver completato i passaggi descritti qui sopra, ora siamo pronti per creare un algoritmo (simile) [!UICONTROL Model]. Durante la configurazione del [!UICONTROL model], utilizzeremo la conversione [!UICONTROL trait] come base [!UICONTROL trait] (visitatori chiave che vogliamo duplicare), e utilizzeremo il flusso di [!UICONTROL third party] dati abilitato come pool di persone da cui prelevare.

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## Una best practice importante {#an-important-best-practice}

Quando si crea l&#39;algoritmo [!UICONTROL model] in  Audience Manager, ovviamente vogliamo che il [!UICONTROL model] sistema sia il più efficace possibile. Poiché [!UICONTROL model] sta considerando tutti i membri [!UICONTROL traits] di base [!UICONTROL trait]/[!UICONTROL segment] di cui fanno parte, non aiuta [!UICONTROL model] se TUTTE le persone si trovano in una [!UICONTROL trait]/[!UICONTROL segment]. Quindi, se avete un qualsiasi super generico [!UICONTROL traits] (come tutti quelli che sono andati al vostro sito, o tutti quelli che hanno ricevuto qualsiasi annuncio da voi, ecc.), assicuratevi che il [!UICONTROL data source] che appartengono a NON sia incluso nel [!UICONTROL data sources] nella vostra [!UICONTROL model]. In questo caso di utilizzo, è improbabile che lo si faccia, perché stiamo cercando [!UICONTROL third party] i dati per i nostri nuovi look-alias, ma vale comunque la pena menzionare, e si applica a TUTTI i vostri algoritmi [!UICONTROL models].

## Creazione di un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Successivamente, sarà necessario creare un algoritmo [!UICONTROL Trait]in modo da poter utilizzare i risultati dell&#39; [!UICONTROL model] evento. Senza creare un modello [!UICONTROL trait], il modello è inutile. Quindi, dopo le [!UICONTROL model] esecuzioni, accertatevi di entrare nella [!UICONTROL trait] finestra di dialogo e creare un algoritmo [!UICONTROL Trait]. Il seguente video illustra la procedura e presenta alcuni suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## Creazione di un [!UICONTROL Segment] da [!UICONTROL Model] Dati e invio a DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

Dopo aver creato un algoritmo [!UICONTROL Trait], è possibile creare un nuovo [!UICONTROL segment] per inserirlo, in modo da poter attivare i dati (non è possibile attivare un [!UICONTROL trait], ma crearne uno nuovo[!UICONTROL trait] con l&#39;algoritmo [!UICONTROL segment] in modo da poter attivare (usare) il [!UICONTROL Trait] [!UICONTROL segment]).

Dopo aver creato un [!UICONTROL segment] da questo algoritmo [!UICONTROL trait], avrai un pubblico di potenziali clienti che somigliano a quelli che si sono già convertiti sul tuo sito. Ora potete mappare questo [!UICONTROL segment] a qualsiasi vostro DSP [!UICONTROL destinations] in  Audience Manager. Potrai eseguire il targeting del tuo marketing per questi look-alias, che hanno più probabilità di convertire sul tuo sito rispetto al normale pubblico, aumentando il tuo Return on Ad Spend. Buona fortuna!
