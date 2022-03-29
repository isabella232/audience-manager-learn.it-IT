---
title: Utilizza modelli lookalike per estendere l’inventario esaurito dai dati di prime parti
description: Questa esercitazione descrive i passaggi da seguire per configurare e utilizzare modelli lookalike, in modo da creare nuovi tipi di pubblico per similarità e venderli come estensione per il segmento di conversione.
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# Utilizza modelli lookalike per estendere l’inventario esaurito dai dati di prime parti {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

In questa esercitazione verranno descritti i passaggi da effettuare per configurare e utilizzare lookalike [!UICONTROL Models], per creare nuovi tipi di pubblico lookalike e venderli come estensione al segmento di conversione.

## Dettagli del caso d’uso {#use-case-details}

Sei un editore di contenuti. Se hai già esaurito l&#39;inventario per i convertitori sul tuo sito, potresti pensare che la tua opportunità finisca lì. Inserisci l’aspetto simile a AAM [!UICONTROL Models]. Utilizzando questa funzione, puoi estendere ulteriormente le scorte esaurite e vendere anche tipi di pubblico di persone che forse non si sono ancora convertite, ma che assomigliano/agiscono come persone che si sono convertite. Questo segmento di pubblico in genere vende per meno dei convertitori effettivi, ma tuttavia ti consente di aggiungere ai tuoi dati base fornendo un’opzione di pubblico aggiuntiva per gli inserzionisti che desiderano inserire annunci sul tuo sito. Il vantaggio aggiuntivo di questo caso d’uso è che non ti costa nulla eseguire questo modello sui dati di prime parti.

I passaggi di questa esercitazione saranno i seguenti:

1. Identificare/creare una caratteristica o un segmento ideale per l’utente (conversione)
1. Crea un modello utilizzando questa caratteristica/segmento di conversione come elemento base
1. Scegli [!UICONTROL First party] origini dati nel modello ed esegui il modello
1. Crea un [!UICONTROL Algorithmic Trait] dai risultati del modello e aggiungi la caratteristica a un segmento
1. Offri il segmento agli inserzionisti interessati per estendere le vendite del segmento di conversione

## Identificare o creare una caratteristica o un segmento dell’utente ideale (conversione) {#identify-create-an-ideal-user-conversion-trait-or-segment}

Cosa stai cercando di fare alle persone sul tuo sito? Qual è l’evento di conversione? Naturalmente, ci sono molte risposte diverse a questa domanda, a seconda del tipo di sito/verticale e i vostri obiettivi organizzativi. In ogni caso, è comune in AAM creare una caratteristica per i visitatori che hanno soddisfatto tali criteri.

In questo caso d’uso, si presume già, perché si è esaurito l’inventario per le persone che sono convertitori. Tuttavia, allo scopo di questa esercitazione, è utile discuterne come riferimento per il resto del caso d’uso.

Inoltre, quando si utilizzano gli eventi per creare le caratteristiche, è necessario tenere presente un elemento gotcha importante, in modo da non raccogliere più utenti di quanto si dovrebbe nella caratteristica. Guarda il seguente video per la grande rivelazione. :

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**NOTA:** Nel video precedente, l’esempio che mostro presuppone che tu disponga di Adobe Analytics. Ovviamente non è così. Se disponi di Google Analytics (GA), è disponibile un modulo che puoi utilizzare per inviare dati a AAM (consulta la sezione [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)) e se l’attività di conversione sul sito viene inviata a AAM da GA, puoi creare la caratteristica di conversione da tale attività. Se disponi di una soluzione di analisi diversa (o nessuna soluzione di analisi), puoi comunque inviare i dati a AAM tramite il nostro codice DIL e il `submit` funzioni, ecc. (vedi [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)). Quindi, crea nuovamente la caratteristica di conversione in base ai dati inviati quando l’attività di conversione viene eseguita sul sito.

## Creare un modello lookalike da dati di prime parti {#creating-a-look-alike-model-from-first-party-data}

In questo passaggio, creeremo un [!UICONTROL First Party] Modello lookalike. Questo significa che non solo utilizzeremo una caratteristica/segmento di conversione di prima parte per la nostra caratteristica/segmento di base (sarebbe normale comunque per la maggior parte dei modelli), ma anche che cercheremo solo nel pool di dati di prime parti per più persone che assomigliano ai convertitori. Non esamineremo dati di seconde o terze parti.

In questo caso d&#39;uso, questo è importante, perché stiamo cercando di creare un segmento di utenti sul nostro sito che assomigliano a convertitori ma non si sono ancora convertiti, in modo da poter vendere questo segmento di somiglianza agli inserzionisti interessati.

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## Creare una caratteristica algoritmica {#creating-an-algorithmic-trait}

Ora è necessario creare un [!UICONTROL Algorithmic Trait], in modo da poter utilizzare i risultati del modello. Senza creare un tratto, il modello è inutile. Quindi, dopo l&#39;esecuzione del modello, assicurati di entrare nella finestra di dialogo delle caratteristiche e creare un [!UICONTROL Algorithmic Trait]. Il seguente video illustra la procedura dettagliata e mostra un paio di suggerimenti.

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## Offerta [!UICONTROL Algorithmic Segment] agli inserzionisti {#offering-the-algorithmic-segment-to-advertisers}

Una volta creata una [!UICONTROL Algorithmic Trait], puoi creare un nuovo segmento per inserirlo, in modo da poter attivare i dati (non puoi attivare una caratteristica, ma piuttosto creare un nuovo segmento a una caratteristica con la [!UICONTROL Algorithmic Trait] in modo da poter attivare (utilizzare) il segmento.

Dopo aver creato un segmento di visitatori di prime parti che hanno ottenuto un punteggio elevato nel modello lookalike (cioè che assomigliano ai convertitori ma che non hanno ancora effettuato la conversione), puoi offrire questo segmento agli inserzionisti sul sito, anche dopo aver esaurito tutto il tuo inventario dei convertitori effettivi sul sito. Questo è un ottimo modo per estendere questo pubblico e continuare a visualizzare entrate aggiuntive utilizzando lookalike [!UICONTROL Models] Audience Manager.
