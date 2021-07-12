---
title: Utilizzo di modelli lookalike per estendere l’inventario venduto dai dati di prime parti
description: In questa esercitazione, passeremo attraverso i passaggi da seguire per configurare e utilizzare Modelli lookalike per creare nuovi tipi di pubblico lookalike e venderli come estensione al segmento di conversione.
feature: Modelli algoritmici
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---

# Utilizzo di lookalike [!UICONTROL Models] per estendere l’inventario venduto dai dati [!UICONTROL First Party] {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

Questa esercitazione illustra i passaggi da seguire per configurare e utilizzare lookalike [!UICONTROL Models], in modo da poter creare nuovi tipi di pubblico lookalike e venderli come estensione alla conversione [!UICONTROL segment].

## Dettagli del caso d’uso {#use-case-details}

Sei un editore di contenuti. Se hai già esaurito l&#39;inventario per i convertitori sul tuo sito, potresti pensare che la tua opportunità finisca lì. Inserisci l’aspetto simile a AAM [!UICONTROL Models]. Utilizzando questa funzione, puoi estendere ulteriormente le scorte esaurite e vendere anche tipi di pubblico di persone che forse non si sono ancora convertite, ma che assomigliano/agiscono come persone che si sono convertite. Questo pubblico [!UICONTROL segment] in genere vende per meno dei convertitori effettivi, ma tuttavia ti consente di aggiungere al tuo fondo fornendo un&#39;opzione di pubblico aggiuntiva per gli inserzionisti che desiderano inserire annunci sul tuo sito. Il vantaggio aggiuntivo di questo caso d’uso è che non ti costa nulla eseguire questo modello sui dati di prime parti.

I passaggi di questa esercitazione saranno i seguenti:

1. Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment]
1. Crea un [!UICONTROL model] utilizzando questa conversione [!UICONTROL trait]/[!UICONTROL segment] come elemento base
1. Scegli [!UICONTROL First party] origini dati in [!UICONTROL model] ed esegui il [!UICONTROL model]
1. Crea un algoritmo [!UICONTROL Trait] dai risultati [!UICONTROL model] e aggiungi il [!UICONTROL trait] a un [!UICONTROL segment]
1. Offri il [!UICONTROL segment] agli inserzionisti interessati per estendere le vendite di conversione [!UICONTROL segment]

## Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di fare alle persone sul tuo sito? Qual è l’evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e dei vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare un [!UICONTROL trait] per i visitatori che hanno soddisfatto tali criteri.

In questo caso d’uso, si presume già, perché si è esaurito l’inventario per le persone che sono convertitori. Tuttavia, allo scopo di questa esercitazione, è utile discuterne come riferimento per il resto del caso d’uso.

Inoltre, quando utilizzi gli eventi per creare [!UICONTROL traits], è necessario tenere presente un elemento gotcha importante, in modo da non raccogliere più utenti di quanto dovresti nel file [!UICONTROL trait]. Guarda il seguente video per la grande rivelazione. :

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente non è così. Se disponi di Google Analytics (GA), disponiamo di un modulo che puoi utilizzare per inviare dati in AAM (consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)) e se l’attività di conversione sul tuo sito viene inviata a AAM da GA, puoi creare la caratteristica di conversione da questo. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati a AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (consulta la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Quindi, crea nuovamente la caratteristica di conversione in base ai dati inviati quando l’attività di conversione viene eseguita sul sito.

## Creazione di un aspetto simile a [!UICONTROL Model] dai dati [!UICONTROL First Party] {#creating-a-look-alike-model-from-first-party-data}

In questo passaggio verrà creato un aspetto [!UICONTROL First Party] simile a [!UICONTROL Model]. Questo significa che non solo utilizzeremo una [!UICONTROL first party] conversione [!UICONTROL trait]/[!UICONTROL segment] per la nostra base [!UICONTROL trait]/[!UICONTROL segment] (questo sarebbe normale per la maggior parte [!UICONTROL models] comunque), ma cercheremo anche solo nel pool di dati [!UICONTROL first party] per più persone che assomigliano ai convertitori. I dati [!UICONTROL second party] o [!UICONTROL third party] non verranno visualizzati.

In questo caso d&#39;uso, questo è importante, perché stiamo cercando di creare un [!UICONTROL segment] di utenti sul nostro sito che assomigliano a convertitori ma non si sono ancora convertiti, in modo da poter vendere questo sosia [!UICONTROL segment] agli inserzionisti interessati.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creazione di un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Ora è necessario creare un algoritmo [!UICONTROL Trait] in modo che sia possibile utilizzare i risultati del [!UICONTROL model]. Senza creare un [!UICONTROL trait], il [!UICONTROL model] è inutile. Quindi, dopo l&#39;esecuzione di [!UICONTROL model], assicurati di accedere alla finestra di dialogo [!UICONTROL trait] e creare un algoritmo [!UICONTROL Trait]. Il seguente video illustra la procedura dettagliata e mostra un paio di suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offrire l&#39;algoritmo [!UICONTROL Segment] agli inserzionisti {#offering-the-algorithmic-segment-to-advertisers}

Dopo aver creato un algoritmo [!UICONTROL Trait], è possibile creare un nuovo [!UICONTROL segment] per inserirlo, in modo da poter attivare i dati (non è possibile attivare un [!UICONTROL trait], ma crearne uno nuovo-[!UICONTROL trait] [!UICONTROL segment] con l&#39;algoritmo [!UICONTROL Trait] in modo da poter attivare (utilizzare) il [!UICONTROL segment].

Dopo aver creato un [!UICONTROL segment] di [!UICONTROL first party] visitatori che hanno ottenuto un punteggio alto nell’aspetto simile [!UICONTROL model] (ovvero che assomigliano ai convertitori ma che non si è ancora convertito), puoi offrire questo [!UICONTROL segment] agli inserzionisti sul tuo sito, anche dopo aver esaurito tutto l’inventario dei convertitori effettivi sul tuo sito. Questo è un ottimo modo per estendere questo pubblico e continuare a visualizzare entrate aggiuntive utilizzando l’Audience Manager lookalike [!UICONTROL Models] .
