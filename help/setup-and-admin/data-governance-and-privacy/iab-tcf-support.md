---
title: 'Supporto IAB TCF 2.0 nel Audience Manager '
description: ' Adobe offre gli strumenti per gestire e comunicare le scelte di privacy degli utenti attraverso la funzionalità di consenso e il plug-in del Audience Manager  per la trasparenza e il supporto di IAB Framework 2.0 (TCF 2.0). Questo articolo funziona insieme alla documentazione per comprendere meglio il plug-in del Audience Manager  a IAB TCF e come funziona insieme all''oggetto di consenso  Adobe e al provider di gestione del consenso (CMP).'
feature: data governance & privacy
topics: null
audience: implementer, architect
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
translation-type: tm+mt
source-git-commit: df8afb50ed3971dc47e6506d31a8222a7f488b25
workflow-type: tm+mt
source-wordcount: '1123'
ht-degree: 0%

---


# Supporto IAB TCF 2.0 nel Audience Manager  {#iab-tcf-support-in-audience-manager}

 Adobe offre gli strumenti per gestire e comunicare le scelte di privacy degli utenti attraverso la funzionalità di consenso e il plug-in del Audience Manager  per la trasparenza e il supporto di IAB Framework 2.0 (TCF 2.0). Questo articolo funziona insieme alla documentazione per comprendere meglio il plug-in del Audience Manager  a IAB TCF e come funziona insieme all&#39;oggetto di consenso  Adobe e al provider di gestione del consenso (CMP). Per ulteriori informazioni sull&#39;IAB, consultare il sito Web all&#39;indirizzo [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## Primo passaggio: Comprendere il consenso di ECID {#first-step-understand-ecid-s-opt-in}

Per comprendere come utilizzare lo IAB TCF, è innanzitutto necessario conoscere la funzionalità [!DNL Opt-in], che fa parte della libreria  Experience Cloud ID Service (ECID). Se non avete familiarità con il funzionamento del consenso, consultate prima [questo utile articolo](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html). È inoltre necessario esaminare la documentazione di consenso [](https://docs.adobe.com/content/help/it-IT/id-service/using/implementation/opt-in-service/optin-overview.html). Una volta completate queste risorse, tornate a questa pagina e continuate.

## Il plug-in del Audience Manager  per IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

Ora che avete almeno una conoscenza di base del funzionamento del servizio di consenso,  Audience Manager può aggiungervi un livello di supporto [!DNL IAB Transparency and Consent Framework (TCF)], che viene eseguito tramite un plug-in nell&#39;oggetto di consenso.

Il plug-in  Audience Manager per IAB TCF amplia le funzionalità del consenso e consente AAM clienti di valutare, onorare e inoltrare le scelte relative alla privacy degli utenti ai partner a valle in conformità con IAB TCF. Fornisce uno standard che consente di controllare i dati (ossia l&#39;utente come cliente  Adobe) e i fornitori (DMP, DSP, SSP, Ad Server, ecc.) può utilizzare per comprendere il consenso nel panorama del consenso.

## Abilitazione di IAB TCF {#enabling-iab-tcf}

Abilitare il plug-in del Audience Manager  per IAB TCF è semplice se si utilizza  Adobe Experience Platform Launch, in quanto è una semplice casella di controllo, come mostrato nel breve video seguente:

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

In alternativa, se non utilizzi Launch, puoi utilizzare `isIabContext=true` per attivarlo quando crei un&#39;istanza del Visitatore  Experience Cloud. Questo avvia il flusso IAB TCF, aggiungendo un altro passo alla raccolta del consenso, utilizzando lo IAB TCF per eseguire una query sulla stringa IAB TC e lo fornisce nuovamente a Opt-in, che a sua volta comunica con le soluzioni del Experience Cloud .

## IAB TC String {#iab-tcf-consent-string}

Uno degli standard che IAB fornisce è una &quot;stringa di consenso&quot; (nota anche come &quot;DaisyBit&quot;), che è in realtà due liste messe insieme:

1. Finalità: **Cosa** viene dato il consenso?
1. Fornitori: **Chi** è autorizzato?

### Finalità {#purposes}

Con IAB TCF 2.0, esistono dieci &quot;scopi&quot; per raccogliere il consenso (ciò che i venditori possono fare con i dati del visitatore). Adobe Audience Manager non richiede tutti e dieci, ma richiede solo il consenso per i seguenti scopi, oltre al consenso del fornitore:

* **1:** Archiviare e/o accedere alle informazioni su un dispositivo;
* **10:** Sviluppare e migliorare i prodotti;
* **Scopo speciale 1:** Protezione, prevenzione di frodi e debug.

Questa è la prima parte della stringa IAB TC ed è registrata come 1 e 0, a seconda che lo scopo/l&#39;attività sia o meno approvato.

>[!NOTE]
>
>In base alle normative IAB, lo scopo speciale 1 (garantire la sicurezza, prevenire le frodi e il debug) è sempre consentito e gli utenti non possono opporvisi.

### Fornitori {#vendors}

Un&#39;altra parte della stringa IAB TC è un lungo elenco di diverse centinaia di fornitori, in modo che i visitatori possano essere presentati con un elenco di fornitori applicabili che hanno dei tag sul sito e possono scegliere quali fornitori utilizzare. I fornitori mantengono il proprio posto nell&#39;elenco. Ad esempio, il numero fornitore di Adobe Audience Manager in questo elenco è 565. Se tale numero nell&#39;elenco ha &quot;1&quot;,  Audience Manager può eseguire gli scopi approvati dalla parte anteriore dell&#39;elenco. Se AAM&#39;area spot ha un valore &quot;0&quot;, non può eseguire alcuna operazione con i dati.

**Per  Audience Manager, per fornire un&#39;interfaccia utente che consenta ai clienti di utilizzare lo IAB TCF per scegliere questi scopi e fornitori, o per approvare/disapprovare tutte le attività, è necessario utilizzare un partner CMP registrato con IAB TCF o crearne uno che supporti lo IAB TCF ed è registrato con lo IAB TCF.**

## Consenso: Traduzione tra le soluzioni IAB e  Adobe {#opt-in-translating-between-iab-and-adobe-solutions}

Uno dei vantaggi dell&#39;utilizzo dello IAB TCF è che gli scopi standard sopra elencati probabilmente danno all&#39;utente finale più di un&#39;idea di cosa stanno approvando rispetto a un elenco di soluzioni  Adobe. Gli utenti finali potrebbero non sapere cosa significhi &quot;approvare&quot;  Audience Manager o [!DNL Target], ma &quot;Archiviare e/o accedere alle informazioni su un dispositivo&quot; o &quot;Sviluppare e migliorare i prodotti&quot; è probabilmente più semplice per loro da capire e accettare.

Affinché  Audience Manager possa essere approvato (ossia per la traduzione di finalità IAB ai fini del consenso AAM un &quot;sì&quot;), gli scopi 1 e 10, elencati sopra, devono essere indicati con il consenso dell&#39;utente finale. Se uno di questi non è approvato, o se un fornitore non è approvato, AAM non eseguire attivazioni pixel o impostare cookie. È inoltre importante sapere che molti clienti scelgono semplicemente di fornire all’utente finale un’interfaccia utente &quot;all or nothing&quot;, che ovviamente consentirebbe o impedirebbe l’uso di  Audience Manager (e delle altre soluzioni di Experience Cloud ).

Nella [documentazione](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html) sono disponibili alcune informazioni importanti sul modo in cui il flusso TCF del plug-in del Audience Manager  si applica sia ai casi di utilizzo Editore che agli inserzionisti.

## IAB: Invio del consenso a valle {#iab-sending-consent-downstream}

Quando si utilizza il plug-in del Audience Manager  per IAB TCF, le scelte di consenso dell&#39;utente saranno inviate anche alle sincronizzazioni ID a livello di piattaforma (3rd party) per i partner presenti nell&#39;elenco dei fornitori globali, in modo che il partner disponga delle informazioni di consenso dell&#39;utente e possa agire anche su di esso. Queste informazioni vengono inviate in due variabili:

* gdpr = 1
* gdpr_assenso = [stringa di consenso codificata]

Se l’utente si trova in un contesto IAB e non fornisce il consenso (o fornisce un consenso negativo),  Audience Manager non raccoglie la stringa IAB TC e, di conseguenza, abbandona le chiamate. Quindi, in quel caso... non passano i permessi a valle.

## Demo{#demo}

Nel video seguente, scopri come i cookie e i beacon di ECID e delle soluzioni sono influenzati dalle selezioni di scelta utente IAB.

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

Per informazioni più dettagliate sul plug-in del Audience Manager  per IAB TCF 2.0, tra cui come implementare e testare, casi di utilizzo e flusso di lavoro, consultare la [documentazione](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
