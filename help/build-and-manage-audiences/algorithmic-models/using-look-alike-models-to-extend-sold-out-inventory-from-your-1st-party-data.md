---
title: Utilizzo di modelli simili per estendere l'inventario venduto dai dati di prime parti
description: In questa esercitazione verranno illustrati i passaggi da seguire per configurare e utilizzare modelli simili a quelli disponibili, in modo da creare nuovi tipi di pubblico simili e venderli come estensione per il segmento di conversione.
feature: modelli algoritmici
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
translation-type: tm+mt
source-git-commit: ba76f9437e5d8f0495e4f2dfafb90cbf2da6454f
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---


# Utilizzo dell&#39;aspetto simile [!UICONTROL Models] per estendere l&#39;inventario venduto dai dati [!UICONTROL First Party] {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

In questa esercitazione verranno illustrati i passaggi da seguire per configurare e utilizzare l&#39;aspetto simile a [!UICONTROL Models], in modo da creare nuovi tipi di pubblico simili e venderli come estensione per la conversione [!UICONTROL segment].

## Dettagli caso di utilizzo {#use-case-details}

Siete un autore di contenuti. Se hai già esaurito le scorte per i convertitori sul tuo sito, potresti pensare che la tua opportunità finisca qui. Immettete l&#39;aspetto AAM [!UICONTROL Models]. Utilizzando questa funzione, potete estendere ulteriormente le scorte esaurite e vendere anche audience di persone che forse non si sono ancora convertite, ma che sembrano/agiscono come persone che si sono convertite. Questo pubblico [!UICONTROL segment] di solito venderebbe per meno dei convertitori effettivi, ma comunque ti consente di aggiungere ai tuoi dati, fornendo un&#39;ulteriore opzione di audience per gli inserzionisti che desiderano inserire annunci sul tuo sito. Il vantaggio aggiuntivo di questo caso di utilizzo è che non vi costa nulla eseguire questo modello sui dati di prime parti.

La procedura descritta in questa esercitazione è la seguente:

1. Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment]
1. Crea un [!UICONTROL model] utilizzando questa conversione [!UICONTROL trait]/[!UICONTROL segment] come elemento di base
1. Scegliere [!UICONTROL First party] origini dati nella [!UICONTROL model] ed eseguire la [!UICONTROL model]
1. Creare un algoritmo [!UICONTROL Trait] dai risultati [!UICONTROL model] e aggiungere il simbolo [!UICONTROL trait] a un elemento [!UICONTROL segment]
1. Offrire la [!UICONTROL segment] agli inserzionisti interessati per estendere la conversione [!UICONTROL segment] vendite

## Identificare/creare un utente ideale (conversione) [!UICONTROL trait] o [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di far fare alla gente sul tuo sito? Qual è l&#39;evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e dei vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare un [!UICONTROL trait] per i visitatori che hanno soddisfatto tali criteri.

In questo caso di utilizzo, questo è già ipotizzato, perché avete esaurito l&#39;inventario per le persone che sono convertitori. Tuttavia, allo scopo di questa esercitazione, è utile discuterne come riferimento per il resto del caso d&#39;uso.

Inoltre, quando si utilizzano gli eventi per creare [!UICONTROL traits], è necessario tenere presente un gotcha importante, in modo da non raccogliere più utenti di quanto si dovrebbe nel [!UICONTROL trait]. Guardate il seguente video per la grande rivelazione. :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l&#39;esempio che mostro presuppone che si abbia  Adobe Analytics. Ovviamente non è così. Se hai Google Analytics (GA), abbiamo un modulo che puoi utilizzare per inviare i dati in AAM (vedi la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)), e se la tua attività di conversione sul tuo sito viene inviata a AAM da GA, puoi creare la caratteristica di conversione da quella. Se hai una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare dati a AAM tramite il nostro codice DIL e la funzione `submit`, ecc. (vedere la [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)). Quindi, di nuovo, crea la caratteristica di conversione in base ai dati inviati quando l&#39;attività di conversione viene eseguita sul sito.

## Creazione di un aspetto simile a [!UICONTROL Model] da [!UICONTROL First Party] Dati {#creating-a-look-alike-model-from-first-party-data}

In questo passaggio, verrà creato un [!UICONTROL First Party] look-like [!UICONTROL Model]. Ciò significa che non solo utilizzeremo una [!UICONTROL first party] conversione [!UICONTROL trait]/[!UICONTROL segment] per la nostra base [!UICONTROL trait]/[!UICONTROL segment] (questa sarebbe normale per la maggior parte [!UICONTROL models] comunque), ma che anche solo la ricerca nel pool di [!UICONTROL first party] dati per più persone che assomigliano ai convertitori. Non verranno esaminati i dati [!UICONTROL second party] o [!UICONTROL third party].

In questo caso d&#39;uso, questo è importante, perché stiamo cercando di creare un [!UICONTROL segment] di utenti sul nostro sito che assomigliano a convertitori ma che non sono ancora stati convertiti, in modo da poter vendere questo aspetto simile [!UICONTROL segment] agli inserzionisti interessati.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creazione di un algoritmo [!UICONTROL Trait] {#creating-an-algorithmic-trait}

Successivamente sarà necessario creare un algoritmo [!UICONTROL Trait], in modo da poter utilizzare i risultati di [!UICONTROL model]. Senza creare un [!UICONTROL trait], il [!UICONTROL model] è inutile. Quindi, dopo l&#39;esecuzione di [!UICONTROL model], assicurarsi di accedere alla finestra di dialogo [!UICONTROL trait] e creare un algoritmo [!UICONTROL Trait]. Il seguente video illustra la procedura e mostra alcuni suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offrire l&#39;algoritmo [!UICONTROL Segment] agli inserzionisti {#offering-the-algorithmic-segment-to-advertisers}

Dopo aver creato un algoritmo [!UICONTROL Trait], è possibile creare un nuovo [!UICONTROL segment] per inserirlo, in modo da poter attivare i dati (non è possibile attivare un [!UICONTROL trait], ma crearne uno nuovo [!UICONTROL trait] [!UICONTROL segment] con l&#39;algoritmo [!UICONTROL Trait], in modo da poter attivare (utilizzare) il [!UICONTROL segment].

Dopo aver creato un [!UICONTROL segment] di [!UICONTROL first party] visitatori che hanno ottenuto un punteggio alto nell&#39;aspetto simile [!UICONTROL model] (ovvero che assomigliano a convertitori ma che non sono ancora stati convertiti), potete offrire questo [!UICONTROL segment] agli inserzionisti sul vostro sito, anche dopo aver esaurito tutto il vostro inventario di convertitori effettivi sul vostro sito. Questo è un ottimo modo per ampliare il pubblico e continuare a visualizzare ulteriori ricavi utilizzando l&#39;aspetto [!UICONTROL Models] nel  Audience Manager.
