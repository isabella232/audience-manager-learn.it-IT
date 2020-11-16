---
title: Migrazione dell'implementazione AAM del sito dall'DIL lato client all'inoltro lato server
description: Questa esercitazione si applica agli utenti che dispongono di Adobe Audience Manager (AAM) e  Adobe Analytics e che al momento inviano un hit dalla pagina a AAM utilizzando il codice "DIL" (Data Integration Library) e inviando anche un hit dalla pagina a  Adobe Analytics. Poiché entrambe le soluzioni sono disponibili e poiché entrambe fanno parte dell’Adobe Experience Cloud, è possibile attivare l’opzione "Server-Side Forwarding (SSF)", che consente ai server di raccolta dati di Analytics di inoltrare dati di analisi del sito in tempo reale a  Audience Manager, anziché far sì che il codice lato client invii un’ulteriore hit dalla pagina a AAM. Questa esercitazione illustra i passaggi necessari per passare dalla precedente implementazione "DIL client-side" al più recente metodo di inoltro lato server.
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# Migrazione dell&#39;implementazione AAM del sito da [!DNL Client-Side] DIL a [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

Questa esercitazione si applica agli utenti che dispongono di Adobe Audience Manager (AAM) e  Adobe Analytics e che al momento inviano un hit dalla pagina a AAM utilizzando il codice &quot;DIL&quot; ([!DNL Data Integration Library]) e inviando anche un hit dalla pagina a  Adobe Analytics. Poiché entrambe le soluzioni sono disponibili e poiché entrambe fanno parte dell&#39;Adobe Experience Cloud, è possibile seguire la procedura consigliata per attivare &quot;[!DNL Server-Side Forwarding] (SSF)&quot;, che consente ai server di raccolta dei [!DNL Analytics] dati di inoltrare i dati di analisi del sito in tempo reale a  Audience Manager, anziché far sì che il [!DNL client-side] codice invii un hit aggiuntivo dalla pagina a AAM. Questa esercitazione illustra i passaggi necessari per passare dall’implementazione &quot;[!DNL Client-Side DIL]&quot; precedente al metodo &quot;[!DNL Server-Side forwarding]&quot; più recente.

## [!DNL Client-Side] (DIL) vs. [!DNL Server-Side] {#client-side-dil-vs-server-side}

Quando si confrontano e si confrontano questi due metodi per AAM  dati Adobe Analytics, potrebbe essere utile visualizzare le differenze nell&#39;immagine seguente:

![lato client a lato server](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] Implementazione DIL {#client-side-dil-implementation}

Se si utilizza questo metodo per inserire  dati Adobe Analytics in AAM, è possibile ottenere due hit dalle pagine Web: Uno andrà [!DNL Analytics]e uno AAM (dopo aver copiato i [!DNL Analytics] dati sulla pagina Web). [!UICONTROL Segments] vengono restituiti dalla AAM alla pagina, dove possono essere utilizzati per la personalizzazione, ecc. Questa operazione è considerata un&#39;implementazione legacy e non è più consigliata.

Oltre al fatto che questo non segue le best practice, gli svantaggi di utilizzare questo metodo includono:

* Due hit provenienti dalla pagina invece di un solo
* [!UICONTROL Server-Side Forwarding] è necessaria per la condivisione in tempo reale di AAM pubblico a [!DNL Analytics], pertanto [!DNL Client-side] le implementazioni non consentono questa funzione (e potenzialmente altre funzioni in futuro)

È consigliabile passare a un [!UICONTROL Server-Side Forwarding] metodo di implementazione AAM.

### [!UICONTROL Server-Side Forwarding] Implementazione {#server-side-forwarding-implementation}

Come mostrato nell&#39;immagine qui sopra, un hit viene dalla pagina Web a  Adobe Analytics. [!DNL Analytics] quindi inoltra i dati a AAM in tempo reale, e i visitatori vengono valutati in AAM [!UICONTROL traits] e [!UICONTROL segments], come se l’hit fosse arrivato direttamente dalla pagina.

[!UICONTROL Segments] vengono restituiti sullo stesso hit in tempo reale a [!DNL Analytics], che inoltra la risposta sulla pagina Web per la personalizzazione, ecc.

Non si verifica alcun rallentamento del passaggio all&#39;inoltro lato server. Consigliamo vivamente a chiunque abbia sia  Audience Manager che [!DNL Analytics] utilizzi questo metodo di implementazione.

## Sono Disponibili DUE Operazioni Principali {#you-have-two-main-tasks}

C&#39;è un bel po&#39; di informazioni su questa pagina, ed è tutto importante, naturalmente. Tuttavia, **tutto si riduce a due cose principali che è necessario fare**:

1. Cambia il codice da codice [!DNL Client-Side] DIL a [!UICONTROL Server-Side Forwarding] codice
1. Riflettere l&#39;interruttore [!DNL Analytics] per avviare [!DNL Admin Console] l&#39;effettivo inoltro dei dati (per [!UICONTROL report suite])

Se si salta uno di questi due, SSF non funzionerà correttamente. In questo documento sono stati aggiunti passaggi e dati aggiuntivi per facilitare la corretta configurazione di questi due passaggi.

## Opzioni di implementazione {#implementation-options}

Passando [!DNL client-side] a [!DNL server-side], una delle attività da svolgere è modificare il codice in un nuovo [!UICONTROL Server-Side Forwarding] codice. Questa operazione viene eseguita utilizzando una delle seguenti opzioni:

*  Adobe Experience Platform Launch - Opzione di implementazione consigliata per le proprietà Web. Vedrete che questo è un compito molto facile, come [!DNL Launch] ha fatto per voi tutte le cose difficili.
* Sulla pagina, è inoltre possibile inserire il nuovo codice SSF direttamente nella `doPlugins` funzione all&#39;interno del [!DNL appMeasurement.js] file, se non si utilizza (ancora)  Adobe Launch
* Altri gestori di tag: questi possono essere trattati come l’opzione precedente (Sulla pagina), in quanto il codice SSF viene comunque inserito in `doPlugins`, ovunque l’altro gestore di tag memorizzi il [!DNL AppMeasurement] codice

Esamineremo ciascuno di questi elementi nella sezione Aggiornamento del codice.

## Passaggi di implementazione {#implementation-steps}

### Passaggio 0: Prerequisito:  servizio ID Experience Cloud (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

Il prerequisito principale per il passaggio a [!UICONTROL Server-Side Forwarding] è l’implementazione del servizio ID Experience Cloud . Questa operazione è molto facile se si utilizza il Experience Platform Launch, nel qual caso si installa semplicemente l&#39;estensione ECID e farà il resto.

Se si utilizza un TMS non  Adobe, o nessun TMS, implementare ECID per eseguire **prima** di qualsiasi altra soluzione  Adobe. Per ulteriori informazioni, consulta la documentazione [](https://marketing.adobe.com/resources/help/en_US/mcvid/) ECID. L&#39;unico altro prerequisito riguarda le versioni del codice, in quanto si applicano semplicemente le versioni più recenti del codice nei seguenti passaggi, sarà bene.

>[!NOTE]
>
>Leggere l&#39;intero documento prima di eseguire l&#39;implementazione. La sezione &quot;Timing&quot; di seguito contiene informazioni importanti su *quando* è necessario implementare ogni pezzo, incluso ECID (se non è ancora implementato).

### Passaggio 1: Opzioni di registrazione attualmente utilizzate dal codice DIL {#step-record-currently-used-options-from-dil-code}

Quando vi preparate a passare dal codice [!DNL Client-Side] DIL a [!UICONTROL Server-Side Forwarding], il primo passo consiste nell&#39;identificare tutte le operazioni che state eseguendo con il codice DIL, incluse le impostazioni personalizzate e i dati inviati a AAM. Elementi da notare e considerare:

* normali [!DNL Analytics] variabili, utilizzando il modulo [!DNL siteCatalyst.init] DIL - Non è necessario preoccuparsi di questo, perché il suo compito è solo inviare le normali [!DNL Analytics] variabili, e questo accade in virtù del semplice fatto che SSF è abilitato.
* Sottodominio partner - Nella funzione DIL.create, prendere nota del `partner` parametro. Questo è noto come &quot;sottodominio partner&quot; o talvolta come &quot;ID partner&quot; e sarà necessario quando si inserisce il nuovo codice SSF.
* [!DNL Visitor Service Namespace] - Noto anche come &quot;[!DNL Org ID]&quot; o &quot;[!DNL IMS Org ID]&quot;, è necessario anche questo quando si configura il nuovo codice SSF. Prendetene una nota.
* containerNSID, uidCookie e altre opzioni avanzate - Prendete nota di eventuali opzioni avanzate aggiuntive utilizzate per poterle impostare anche nel codice SSF.
* Ulteriori variabili di pagina - Se altre variabili vengono inviate a AAM dalla pagina (oltre alle normali [!DNL Analytics] variabili gestite da siteCatalyst.init), sarà necessario prenderne nota in modo che possano essere inviate tramite SSF (spoiler alert: tramite [!DNL contextData] variabili).

### Passaggio 2: Aggiornamento del codice {#step-updating-the-code}

Nella sezione sopra intitolata &quot;Opzioni di implementazione&quot;, vengono fornite più opzioni relative alle modalità e alle posizioni di implementazione [!UICONTROL Server-Side Forwarding]. Affinché questa sezione sia efficace, dobbiamo suddividerla in queste sezioni (con due di esse combinate). Vai al metodo di questa sezione che meglio descrive le tue esigenze.

#### Adobe Experience Platform Launch {#launch-by-adobe}

Guardate il video seguente per scoprire come spostare le opzioni di implementazione dal codice [!DNL Client-Side] DIL al [!UICONTROL Server-Side Forwarding] Experience Platform Launch.

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;Sulla pagina&quot; o Adobe Tag Manager non  {#on-the-page-or-non-adobe-tag-manager}

Guardate il video seguente per scoprire come spostare le opzioni di implementazione dal codice [!DNL Client-Side] DIL al [!UICONTROL Server-Side Forwarding] codice nel [!DNL AppMeasurement] codice, risiedendo in un file o in un sistema di gestione tag non  Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### Passaggio 3: Abilitazione dell&#39;inoltro (per [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

Finora in questa esercitazione abbiamo dedicato tutto il nostro tempo a cambiare il codice da [!DNL Client-Side DIL] codice a [!UICONTROL Server-Side Forwarding]. Va bene, perché è la parte più difficile. Questa sezione, anche se vedrete che è super facile, è importante quanto l&#39;aggiornamento del codice. In questo video verrà illustrato come capovolgere lo switch che consente l&#39;effettivo inoltro dei dati da Analytics a  Audience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**NOTA:** Come indicato nel video, ricordate che ci vorranno fino a 4 ore prima che l&#39;attivazione dell&#39;inoltro sia completamente implementata sul back-end del Experience Cloud .

## Tempo {#timing}

Come promemoria, vi sono due compiti principali da svolgere per passare da [!DNL Client-Side DIL] a [!UICONTROL Server-Side Forwarding]:

1. Aggiornamento del codice
1. Riavvolgendo l&#39;interruttore nel pannello [!DNL Analytics] [!DNL Admin Console]

Ma la domanda è, quale si fa prima? Importa? Ok, scusate, erano due domande. Ma le risposte sono... dipende, e sì, *può* essere importante. Com&#39;è per vago? Distruggiamolo. Ma prima una domanda aggiuntiva che può emergere se siete una grande organizzazione con molti siti: Devo fare tutto immediatamente? Quella è un po&#39; più facile. No. Puoi farlo pezzo per pezzo... una specie di... :)

### Un po&#39; di immersione profonda {#a-little-deeper-dive}

Il motivo per cui la tempistica e la materia dell&#39;ordine è dovuto a come funziona l&#39;avanzamento *realmente *, che può essere riassunto nei seguenti alcuni fatti tecnici:

* Se hai implementato il servizio ID del Experience Cloud  (ECID) e lo switch in [!DNL Analytics] (&quot;lo switch&quot;) è attivato, i dati passeranno da [!DNL Admin Console] [!DNL Analytics] a AAM, anche se non hai ancora aggiornato il codice.
* Se non è stato implementato ECID, i dati non verranno inoltrati, anche se è attivato l&#39;interruttore, e si dispone del codice SSF.
* Il codice SSF (sia nella pagina che nella [!DNL Launch] pagina) gestisce realmente la risposta ed è ovviamente necessario per completare la migrazione.
* Tenere presente che lo switch SSF è abilitato da [!UICONTROL Report Suite], ma che il codice è gestito da proprietà in [!DNL Launch], o da [!DNL AppMeasurement] file se non si utilizza [!DNL Launch]

### Best practice {#best-practices}

Sulla base di questi dettagli tecnici, ecco le raccomandazioni per la tempistica di &quot;cosa fare quando&quot;:

#### Se NON hai ancora implementato ECID {#if-you-do-not-have-ecid-yet-implemented}

1. Inverti l&#39;interruttore [!DNL Analytics] per ogni [!UICONTROL report suite] che avrai abilitato per SSF

   1. L&#39;inoltro non verrà ancora avviato perché non hai un ECID

1. Per sito, aggiornate il codice da [!DNL Client-Side DIL] a SSF (potrebbe trovarsi nella pagina [!DNL Launch] o nella pagina, come descritto in un&#39;altra sezione precedente)

   1. L&#39;inoltro ora scorrerà (come avete aggiunto ECID) e dovreste ricevere anche una risposta JSON corretta al [!DNL Analytics] beacon (consultate la sezione Convalida e risoluzione dei problemi di seguito per ulteriori dettagli)

#### Se è stato implementato ECID {#if-you-do-have-ecid-implemented}

1. Preparare e pianificare in modo da essere pronti ad aggiornare il codice da DIL a SSF PER [!UICONTROL report suite] che verrà attivato per SSF:

   1. Inverti l&#39;interruttore [!DNL Analytics] per abilitare SSF

      1. L&#39;inoltro verrà avviato perché l&#39;ECID è abilitato
   1. Non appena possibile, aggiornate il codice da [!DNL Client-Side DIL] a SSF (potrebbe trovarsi nella pagina [!DNL Launch] o nella pagina, come descritto in un&#39;altra sezione precedente)

      1. Dovresti ricevere una risposta JSON appropriata al tuo [!DNL Analytics] beacon (consulta la sezione relativa alla convalida e alla risoluzione dei problemi di seguito per ulteriori dettagli)


**NOTA 1:** È importante eseguire questi due passaggi il più vicino possibile l&#39;uno all&#39;altro, perché tra i passaggi 1 e 2 di cui sopra, si avrà una duplicazione di dati in AAM. In altre parole, SSF avrà iniziato a inviare dati da [!DNL Analytics] a AAM, e siccome il codice DIL è ancora sulla pagina, ci sarà anche un hit che va direttamente dalla pagina a AAM, raddoppiando i dati. Non appena si aggiorna il codice da DIL a SSF, questo verrà attenuato.

**NOTA 2:** Se si preferisce avere una piccola discrepanza nei dati piuttosto che una piccola duplicazione dei dati, è possibile cambiare l&#39;ordine dei passaggi 1 e 2 precedenti. Spostando il codice da DIL a SSF si arresterebbe il flusso di dati in AAM fino a quando non si è riusciti a capovolgere l&#39;interruttore per attivare l&#39;SSF per il [!UICONTROL report suite]. In genere i clienti preferiscono raddoppiare i dati piuttosto che non ricevere visitatori [!UICONTROL traits] e [!UICONTROL segments].

#### Tempo di migrazione quando si dispone di molti siti e [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

Questo argomento viene brevemente trattato nelle sezioni precedenti, in quanto la strategia principale può essere riassunta come segue:

Migrate un sito/[!UICONTROL report suite] (o un gruppo di siti/[!UICONTROL report suites]) alla volta.

Tuttavia, questo può diventare un po&#39; complicato in base ad alcuni scenari possibili:

* È presente un sito che contiene diversi [!UICONTROL report suites]
* È disponibile un [!UICONTROL report suite] che include diversi siti (come un sito globale [!UICONTROL report suite])
* È possibile utilizzare una [!DNL Launch] proprietà per coprire più siti
* Sono disponibili diversi team di sviluppo per siti diversi

Grazie a questi elementi, può diventare un po&#39; complicato. Le cose migliori che posso suggerire sono:

* Dedica un po&#39; di tempo ad adottare una strategia per la migrazione all&#39;SSF, in base alle informazioni sopra illustrate
* In base al fatto che una singola proprietà in [!DNL Launch] (o un singolo [!DNL AppMeasurement] file) in genere viene mappata a 1 o 2 elementi distinti [!UICONTROL report suites], è probabile che sarà possibile creare un piano che funzioni su questi gruppi distinti uno per uno, aggiornando l&#39;azienda a SSF
* Se state lavorando con  Adobe Consulting, parlate con loro riguardo al vostro piano di migrazione, in modo che possano essere di aiuto in base alle esigenze

## Convalida e risoluzione dei problemi {#validation-and-troubleshooting}

Il modo principale per verificare che l&#39;app [!UICONTROL Server-Side Forwarding] sia in esecuzione è di guardare la risposta a uno degli hit Adobe Analytics  provenienti dall&#39;app.

Se non si eseguono operazioni [!UICONTROL server-side forwarding] di dati da [!DNL Analytics] a  Audience Manager, non viene visualizzata alcuna risposta al [!DNL Analytics] beacon (oltre a un pixel 2x2). Tuttavia, se state utilizzando SSF, potete verificare alcuni elementi nella [!DNL Analytics] richiesta e nella risposta che vi consentiranno di sapere che [!DNL Analytics] sta comunicando correttamente con  Audience Manager, inoltrando l’hit e ottenendo una risposta.

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**AVVISO:** Attenzione al falso &quot;successo&quot; - Se c&#39;è una risposta, e tutto sembra funzionare, assicurarsi che si ha l&#39;oggetto &quot;roba&quot; nella risposta. Se non lo fai, potresti vedere un messaggio che dice [!DNL "status":"SUCCESS"]. Per quanto sembri folle, questa è la prova che non funziona correttamente. Se viene visualizzato, significa che avete completato l&#39;aggiornamento del codice in [!DNL Launch] o [!DNL AppMeasurement], ma che l&#39;inoltro nel [!DNL Analytics] file non [!DNL Admin Console] è ancora stato completato. In questo caso è necessario verificare di aver attivato SSF nel [!DNL Analytics][!DNL Admin Console] per il [!UICONTROL report suite]. Se lo avete fatto, e non sono ancora state 4 ore, siate pazienti, perché ci vuole così tanto per fare tutti i cambiamenti necessari sul retro.

![falso successo](assets/falsesuccess.png)

For more information about [!UICONTROL Server-Side Forwarding], please see the [documentation](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).
