---
title: Convalida ID dispositivo globale
description: Gli identificatori della pubblicità del dispositivo (ad es. iDFA, GAID, Roku ID) hanno degli standard di formattazione che devono essere soddisfatti per essere utilizzabili nell'ecosistema della pubblicità digitale. Oggi, clienti e partner possono caricare gli ID nelle nostre origini dati globali in qualsiasi formato senza che venga loro notificato se l’ID è formattato correttamente. Questa funzione introdurrà la convalida degli ID dispositivo inviati alle origini dati globali per la corretta formattazione e fornirà messaggi di errore in caso di formattazione errata degli ID. Al momento dell’avvio, supporteremo la convalida per iDFA, Google Advertising e Roku ID.
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# Convalida ID dispositivo globale {#global-device-id-validation}

Gli identificatori della pubblicità del dispositivo (ad es. iDFA, GAID, Roku ID) hanno degli standard di formattazione che devono essere soddisfatti per essere utilizzabili nell&#39;ecosistema della pubblicità digitale. Oggi, i clienti e i partner possono caricare gli ID nel nostro [!UICONTROL data sources] globale in qualsiasi formato senza che venga loro notificato se l&#39;ID è formattato correttamente. Questa funzione introdurrà la convalida degli ID dispositivo inviati al Globale [!UICONTROL data sources] per la corretta formattazione e fornirà messaggi di errore in caso di formattazione errata degli ID. Al momento dell&#39;avvio sarà supportata la convalida per [!DNL iDFA], [!DNL Google Advertising] e [!DNL Roku IDs].

## Panoramica degli standard di formato {#overview-of-format-standards}

Di seguito sono riportati i pool di ID di pubblicità dispositivo globali attualmente riconosciuti e supportati da AAM. Questi vengono implementati come [!UICONTROL Data Sources] condivisi che possono essere utilizzati da qualsiasi cliente o partner di dati che lavora con dati legati agli utenti di queste piattaforme.

<table>
  <tr>
   <td>Piattaforma </td>
   <td>ID origine dati AAM </td>
   <td>Formato ID </td>
   <td>AAM PID </td>
   <td>Note </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>2014</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12<em>esempio, 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>Questo ID deve essere raccolto in un modulo non elaborato/non con hash di riferimento - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS (IDFA)</td>
   <td>2015</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12 <em>esempio, 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>Questo ID deve essere raccolto in un modulo non elaborato/non con hash di riferimento - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku (RIDA)</td>
   <td>121963</td>
   <td>32 numeri esadecimali, generalmente presentati come 8-4-4-4-12 <em>esempio,</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>Questo ID deve essere raccolto in un modulo non elaborato/non con hash di riferimento - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID (MAID)</td>
   <td>389146</td>
   <td>Stringa numerica alfa</td>
   <td>14593</td>
   <td>Questo ID deve essere raccolto in un modulo non crittografato/non alterato Riferimento - <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>Esempio di stringa numerica alfa, 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>Questo ID deve essere raccolto in un modulo non elaborato/non con hash di riferimento - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## Impostazione di un identificatore pubblicitario nell&#39;app {#setting-an-advertising-identifier-in-the-app}

L&#39;impostazione dell&#39;ID dell&#39;inserzionista nell&#39;app è in realtà un processo in due fasi, prima recuperando l&#39;ID dell&#39;inserzionista e poi inviandolo al Experience Cloud . Per eseguire questi passaggi, sono disponibili i collegamenti riportati di seguito.

1. Recuperare l’ID
   1. [!DNL Apple] le informazioni sul  [!DNL advertising ID] sito sono disponibili  [QUI](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. Per ulteriori informazioni sull&#39;impostazione di [!DNL advertiser ID] per gli sviluppatori [!DNL Android], vedere [HERE](http://www.androiddocs.com/google/play-services/id.html).
1. Inviatelo nel Experience Cloud  utilizzando il metodo [!DNL setAdvertisingIdentifier] nell&#39;SDK
   1. Le informazioni per l&#39;utilizzo di `setAdvertisingIdentifier` si trovano nella [documentazione](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) sia per [!DNL iOS] che per [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## Messaggi di errore DCS per ID errati {#dcs-error-messaging-for-incorrect-ids}

Quando un ID dispositivo globale errato (IDFA, GAID, ecc.) viene inviato in tempo reale a  Audience Manager, viene restituito un codice di errore sull&#39;hit. Di seguito è riportato un esempio di errore restituito perché l&#39;ID viene inviato come [!DNL Apple IDFA], che deve contenere solo lettere maiuscole, ma nell&#39;ID è presente una &#39;x&#39; minuscola.

![error image](assets/image_4_.png)

Per l&#39;elenco dei codici di errore, consultare la [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code).

## ID dispositivo globale di onboarding {#onboarding-global-device-ids}

Oltre all&#39;invio in tempo reale degli ID dispositivo globale, è anche possibile &quot;[!DNL onboard]&quot; (caricare) dati anche rispetto agli ID. Questo processo è lo stesso di quando si aggiungono dati agli ID cliente (in genere tramite coppie chiave/valore), ma si utilizzano semplicemente gli ID origine dati corretti, in modo che i dati vengano assegnati all&#39;ID dispositivo globale. La documentazione relativa al processo di onboarding si trova nella [documentazione](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). Ricorda di utilizzare l&#39;ID globale [!UICONTROL data source], a seconda della piattaforma in uso.

Se gli ID dispositivo globale non corretti vengono inviati tramite il processo di onboarding, gli errori vengono visualizzati in [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

Di seguito è riportato un esempio di errore che si verificherebbe attraverso il report:

![error image](assets/image_5_.png)