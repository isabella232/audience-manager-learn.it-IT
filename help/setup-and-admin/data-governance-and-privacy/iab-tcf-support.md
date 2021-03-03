---
title: Supporto IAB TCF 2.0 in Audience Manager
description: Adobe consente di gestire e comunicare le scelte degli utenti in materia di privacy tramite la funzionalità Opt-in e il plug-in di Audience Manager per il supporto di IAB Transparency and Consent Framework 2.0 (TCF 2.0). Questo articolo funziona insieme alla documentazione per comprendere il plug-in Audience Manager per IAB TCF e come funziona insieme all’oggetto Opt-in di Adobe e al tuo provider di gestione dei consensi (CMP).
feature: '"Governance dei dati e privacy"'
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: '"Sviluppatore, data engineer, architect"'
level: Esperienza
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1131'
ht-degree: 0%

---


# Supporto IAB TCF 2.0 in Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe consente di gestire e comunicare le scelte degli utenti in materia di privacy tramite la funzionalità Opt-in e il plug-in di Audience Manager per il supporto di IAB Transparency and Consent Framework 2.0 (TCF 2.0). Questo articolo funziona insieme alla documentazione per comprendere il plug-in Audience Manager per IAB TCF e come funziona insieme all’oggetto Opt-in di Adobe e al tuo provider di gestione dei consensi (CMP). Per ulteriori informazioni su IAB, consulta il sito Web all’indirizzo [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primo passaggio: Comprendere l&#39;Opt-in di ECID {#first-step-understand-ecid-s-opt-in}

Per comprendere come utilizzare IAB TCF, è innanzitutto necessario comprendere la funzionalità [!DNL Opt-in], che fa parte della libreria del servizio Experience Cloud ID (ECID). Se non sai come funziona Opt-in, consulta prima [questo utile articolo](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html). È inoltre necessario esaminare la documentazione di Opt-in [](https://docs.adobe.com/content/help/it-IT/id-service/using/implementation/opt-in-service/optin-overview.html). Una volta passate in rassegna queste risorse, torna a questa pagina e continua.

## Plug-in di Audience Manager per IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Ora che hai almeno una conoscenza di base del funzionamento del servizio Opt-in, Audience Manager può aggiungerlo al supporto [!DNL IAB Transparency and Consent Framework (TCF)], che viene eseguito tramite un plug-in nell&#39;oggetto Opt-in.

Il plug-in Audience Manager per IAB TCF estende le funzionalità di Opt-in e consente ai clienti AAM di valutare, rispettare e inoltrare le scelte sulla privacy degli utenti ai partner a valle in conformità con IAB TCF. Fornisce uno standard per i titolari del trattamento dei dati (che sei tu come cliente Adobe) e per i fornitori (DMP, DSP, SSP, Ad Server, ecc.) può utilizzare per comprendere il consenso nel panorama del consenso.

## Abilitazione di IAB TCF {#enabling-iab-tcf}

Abilitare il plug-in Audience Manager per IAB TCF è facile se utilizzi Adobe Experience Platform Launch, in quanto si tratta di una semplice casella di controllo, come mostrato nel breve video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

In alternativa, se non utilizzi Launch, puoi utilizzare `isIabContext=true` per abilitarlo quando crei un&#39;istanza del visitatore Experience Cloud. Questo avvia il flusso IAB TCF, ovvero aggiunge un altro passaggio alla raccolta del consenso, utilizzando IAB TCF per eseguire una query sulla stringa IAB TC e lo fornisce nuovamente a Opt-in, che a sua volta comunica con le soluzioni Experience Cloud.

## Stringa TC IAB {#iab-tcf-consent-string}

Uno degli standard forniti da IAB è una &quot;stringa di consenso&quot; (nota anche come &quot;DaisyBit&quot;), che sono in realtà due liste messe insieme:

1. Finalità: **Cosa** viene dato il consenso?
1. Fornitori: **A chi** viene dato il consenso?

### Finalità {#purposes}

Con IAB TCF 2.0, ci sono dieci &quot;scopi&quot; per raccogliere il consenso (ciò che i fornitori possono fare con i dati del visitatore). Adobe Audience Manager non richiede tutti e dieci, ma richiede solo il consenso per le seguenti finalità, oltre al consenso del fornitore:

* **Scopo 1:** archiviare e/o accedere alle informazioni su un dispositivo;
* **Scopo 10:** sviluppare e migliorare i prodotti;
* **Scopo speciale 1:** garantire la sicurezza, prevenire frodi ed eseguire il debug.

Questa è la prima parte della stringa IAB TC e viene registrata come 1 e 0, a seconda che lo scopo/attività sia approvato o meno.

>[!NOTE]
>
>In base alle normative IAB, lo scopo speciale 1 (garantire la sicurezza, prevenire frodi e debug) è sempre consentito e gli utenti non possono obiettare.

### Fornitori {#vendors}

Un&#39;altra parte della stringa IAB TC è una lunga lista di diverse centinaia di fornitori, in modo che ai visitatori possa essere presentato un elenco di fornitori applicabili che hanno dei tag sul sito e possono scegliere quali fornitori utilizzare. I fornitori mantengono il loro posto nell&#39;elenco. Ad esempio, il numero di fornitore di Adobe Audience Manager presente in questo elenco è 565. Se tale numero nell’elenco ha &quot;1&quot;, Audience Manager può eseguire gli scopi approvati dalla parte anteriore dell’elenco. Se il punto di AAM ha un valore &quot;0&quot;, non può fare nulla con i dati.

**Affinché Audience Manager fornisca un’interfaccia utente che consenta ai clienti di utilizzare IAB TCF per scegliere questi scopi e fornitori, o per approvare/disapprovare tutte le attività, devi utilizzare un partner CMP registrato con IAB TCF o crearne uno che supporti IAB TCF e sia registrato con IAB TCF.**

## Opt-in: Traduzione tra le soluzioni IAB e Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Uno dei vantaggi dell’utilizzo di IAB TCF è che le finalità standard elencate sopra probabilmente forniscono all’utente finale un’idea di cosa sta approvando rispetto a un elenco di soluzioni Adobe. Gli utenti finali potrebbero non sapere cosa significa &quot;approvare&quot; Audience Manager o [!DNL Target], ma &quot;archiviare e/o accedere alle informazioni su un dispositivo&quot; o &quot;Sviluppare e migliorare i prodotti&quot; è probabilmente più semplice da capire e acconsentire.

Affinché Audience Manager possa essere approvato (ossia per la traduzione di finalità IAB affinché Opt-in dia ad AAM un voto favorevole), l’utente finale deve dare il consenso alle finalità 1 e 10 elencate sopra. Se uno di questi non è approvato, o se un fornitore non è approvato, AAM non eseguirà il fuoco pixel o non imposterà i cookie. È anche importante sapere che molti clienti scelgono semplicemente di fornire all’utente finale un’interfaccia utente &quot;all or nothing&quot; che consentirebbe o impedirebbe, ovviamente, l’utilizzo di Audience Manager (e delle altre soluzioni Experience Cloud).

Nella [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html) sono presenti alcune informazioni utili sul modo in cui il plug-in di Audience Manager per il flusso TCF IAB si applica sia ai casi di utilizzo di Editore che Advertiser.

## IAB: Invio del consenso a valle {#iab-sending-consent-downstream}

Quando si utilizza il plug-in Audience Manager per IAB TCF, le scelte di consenso dell’utente verranno inviate anche alle sincronizzazioni ID a livello di piattaforma (di terze parti) per i partner presenti nell’elenco dei fornitori globali, in modo che il partner disponga delle informazioni sul consenso dell’utente e possa agire su di esso. Queste informazioni vengono inviate in due variabili:

* gdpr = 1
* gdpr_permission = [stringa di consenso codificata]

È importante notare che se l’utente è nel contesto IAB e non fornisce il consenso (o fornisce un consenso negativo), Audience Manager non raccoglie affatto la stringa IAB TC e, in quanto tale, rilascia le chiamate. Quindi, in quel caso.. non passare il consenso a valle.

## Demo{#demo}

Nel video seguente, scopri in che modo le selezioni degli utenti IAB influenzano i cookie e i beacon di ECID e delle soluzioni.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Per informazioni più dettagliate sul plug-in Audience Manager per IAB TCF 2.0, tra cui come implementare e testare, i casi d’uso e il flusso di lavoro, consulta la [documentazione](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
