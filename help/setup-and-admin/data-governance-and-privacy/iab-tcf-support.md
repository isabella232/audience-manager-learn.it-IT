---
title: Supporto IAB TCF 2.0
description: Scopri il plug-in di Audience Manager a IAB TCF e come funziona con l’oggetto opt-in di Adobe e il tuo provider di gestione dei consensi (CMP).
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Supporto IAB TCF 2.0 in Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe offre i mezzi per gestire e comunicare le scelte degli utenti in materia di privacy tramite la funzionalità Opt-in e il plug-in di Audience Manager al supporto IAB Transparency and Consent Framework 2.0 (TCF 2.0). Questo articolo funziona insieme alla documentazione per comprendere il plug-in di Audience Manager a IAB TCF e come funziona insieme all’oggetto Opt-in di Adobe e al tuo provider di gestione del consenso (CMP, Consent Management Provider). Per ulteriori informazioni su IAB, consulta il sito web all’indirizzo [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primo passaggio: Comprendere l’opt-in di Experience Cloud ID {#first-step-understand-ecid-s-opt-in}

Per capire come lavorare con IAB TCF, devi prima capire [!DNL Opt-in] , che fa parte della libreria del servizio Experience Cloud ID (ECID) . Se non sai come funziona l&#39;opt-in, consulta [questo articolo utile](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) prima. È inoltre necessario rivedere Opt-in [documentazione](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html). Una volta passate in rassegna queste risorse, torna a questa pagina e continua.

## Plug-in di Audience Manager per IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Ora che hai almeno una comprensione di base del funzionamento del servizio Opt-in, Audience Manager può inserirvi un livello [!DNL IAB Transparency and Consent Framework (TCF)] , che viene eseguito tramite un plug-in nell&#39;oggetto Opt-in .

Il plug-in di Audience Manager per IAB TCF estende le funzionalità di Opt-in e consente ai clienti AAM di valutare, onorare e inoltrare le scelte relative alla privacy degli utenti ai partner a valle in conformità a IAB TCF. Fornisce uno standard che consente ai titolari del trattamento (ossia a te come cliente Adobe) e ai fornitori (DMP, DSP, SSP, Ad Server, ecc.) può utilizzare per comprendere il consenso nel panorama del consenso.

## Abilitare IAB TCF {#enabling-iab-tcf}

Abilitare il plug-in di Audience Manager per IAB TCF è facile se utilizzi Adobe Experience Platform Launch, in quanto si tratta di una semplice casella di controllo, come mostrato nel breve video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

In alternativa, se non utilizzi Launch, puoi utilizzare `isIabContext=true` per attivarlo quando crei un&#39;istanza del visitatore Experience Cloud. Questo avvia il flusso IAB TCF, ovvero aggiunge un altro passaggio alla raccolta del consenso, utilizzando IAB TCF per eseguire una query per la stringa IAB TC e lo fornisce nuovamente a Opt-in, che a sua volta comunica con le soluzioni di Experience Cloud.

## Stringa TC IAB {#iab-tcf-consent-string}

Uno degli standard forniti da IAB è una &quot;stringa di consenso&quot; (nota anche come &quot;DaisyBit&quot;), che sono in realtà due liste messe insieme:

1. Finalità: **Cosa** viene dato il consenso?
1. Fornitori: **Chi** viene dato il consenso?

### Finalità {#purposes}

Con IAB TCF 2.0, ci sono dieci &quot;scopi&quot; per raccogliere il consenso (ciò che i fornitori possono fare con i dati del visitatore). Adobe Audience Manager non richiede tutti i dieci, ma richiede solo il consenso per i seguenti scopi, oltre al consenso del fornitore:

* **Scopo 1:** archiviare e/o accedere alle informazioni su un dispositivo;
* **Finalità 10:** Sviluppare e migliorare i prodotti;
* **Scopo speciale 1:** Assicurati la sicurezza, evita frodi ed esegui il debug.

Questa è la prima parte della stringa IAB TC e viene registrata come 1 e 0, a seconda che lo scopo/attività sia approvato o meno.

>[!NOTE]
>
>In base alle normative IAB, lo scopo speciale 1 (garantire la sicurezza, prevenire frodi e debug) è sempre consentito e gli utenti non possono obiettare.

### Fornitori {#vendors}

Un&#39;altra parte della stringa IAB TC è una lunga lista di diverse centinaia di fornitori, in modo che ai visitatori possa essere presentato un elenco di fornitori applicabili che hanno dei tag sul sito e possono scegliere quali fornitori utilizzare. I fornitori mantengono il loro posto nell&#39;elenco. Ad esempio, il numero di fornitore di Adobe Audience Manager in questo elenco è 565. Se tale numero nell&#39;elenco ha un &quot;1&quot;, l&#39;Audience Manager può eseguire gli scopi approvati dalla parte anteriore dell&#39;elenco. Se il punto di AAM ha un valore &quot;0&quot;, non può fare nulla con i dati.

**Ad Audience Manager, per fornire ai clienti un’interfaccia utente che consenta loro di utilizzare IAB TCF per scegliere questi scopi e fornitori, o per approvare/disapprovare tutte le attività, devi utilizzare un partner CMP registrato con IAB TCF o crearne uno che supporti IAB TCF e sia registrato con IAB TCF.**

## Opt-in: Traduzione tra le applicazioni IAB e Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Uno dei vantaggi dell’utilizzo di IAB TCF è che gli scopi standard sopra elencati probabilmente danno all’utente finale più un’idea di ciò che sta approvando rispetto a un elenco di soluzioni di Adobe. Gli utenti finali potrebbero non sapere cosa significa &quot;approvare&quot; l&#39;Audience Manager o [!DNL Target], ma &quot;archiviare e/o accedere alle informazioni su un dispositivo&quot; o &quot;Sviluppare e migliorare i prodotti&quot; è probabilmente più facile per loro da comprendere e acconsentire.

Affinché l&#39;Audience Manager possa essere approvato (ossia per la traduzione degli scopi IAB affinché Opt-in dia AAM un voto favorevole), l&#39;utente finale deve dare il consenso alle finalità 1 e 10 di cui sopra. Se uno di questi non è approvato, o se un fornitore non è approvato, AAM non eseguirà il fuoco pixel o non imposterà i cookie. È anche bene sapere che molti clienti scelgono semplicemente di fornire all’utente finale un’interfaccia utente &quot;all or nothing&quot; che consentirebbe o impedirebbe, ovviamente, l’uso di Audienci Manager (e delle altre soluzioni di Experience Cloud).

Ci sono alcune grandi informazioni nel [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en) informazioni su come il flusso Audience Manager Plug-in per IAB TCF si applica sia ai casi di utilizzo di Publisher che di Advertiser.

## IAB: Invio del consenso a valle {#iab-sending-consent-downstream}

Quando si utilizza il plug-in di Audience Manager per IAB TCF, le scelte di consenso dell’utente verranno inviate anche alle sincronizzazioni ID a livello di piattaforma (di terze parti) per i partner presenti nell’elenco dei fornitori globali, in modo che il partner disponga delle informazioni di consenso dell’utente e possa agire su di esso. Queste informazioni vengono inviate in due variabili:

* gdpr = 1
* gdpr_permission = [stringa di consenso codificata]

È importante notare che se l’utente è nel contesto IAB e non fornisce il consenso (o fornisce il consenso negativo), l’Audience Manager non raccoglie affatto la stringa IAB TC e, di conseguenza, rilascia le chiamate. Quindi, in quel caso.. non passare il consenso a valle.

## Demo {#demo}

Nel video seguente, scopri in che modo le selezioni degli utenti IAB influenzano i cookie e i beacon di ECID e delle soluzioni.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Per informazioni più dettagliate sul plug-in di Audience Manager per IAB TCF 2.0, tra cui come implementare e testare, i casi d’uso e il flusso di lavoro, consulta la sezione [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
