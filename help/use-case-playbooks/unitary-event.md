---
title: Eenmalige gebeurtenis
description: Dit is een instructiepagina voor het simuleren van '[!UICONTROL Unitary event]' type van Reisbevestiging.
source-git-commit: 0a52611530513fc036ef56999fde36a32ca0f482
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# Eenmalige gebeurtenis

## Te volgen stappen {#steps-to-follow}

>[!CONTEXTUALHELP]
>id="marketerexp_sampledata_unitaryevent"
>title="Hoe wordt het gebruikt?"
>abstract="Klik op de koppeling voor meer informatie"

>[!IMPORTANT]
>
>Deze instructies kunnen **[!UICONTROL Playbook]** Raadpleeg dus altijd het gedeelte Voorbeeldgegevens van de betreffende **[!UICONTROL Playbook]**.

## Vereiste

* De Postman-software moet zijn geïnstalleerd
* Afspeelboek gebruiken om instanties te maken zoals **[!UICONTROL Journey]**, **[!UICONTROL Schemas]**, **[!UICONTROL Segments]**, **[!UICONTROL Messages]** enz.

Gemaakte elementen worden weergegeven op `Bill Of Material` Pagina

![Materiaallijst](../assets/bom-page.png)

## Postman voorbereiden met vereiste verzameling

1. Bezoek **[!UICONTROL Use Case Playbook]** toepassing.
1. Klik op de respectievelijke **[!UICONTROL Playbook]** kaart voor bezoek **[!UICONTROL Playbook]** detailpagina.
1. Bezoek **[!UICONTROL Bill Of Material]** pagina en zoek de **[!UICONTROL Sample data]** sectie.
1. Download de `postman.json` door op de desbetreffende knoppen in de gebruikersinterface te klikken.
1. Importeren `postman.json` in de **[!DNL Postman Software]**.
1. Een speciale Postman-omgeving maken voor deze validatie (bijvoorbeeld `Adobe <PLAYBOOK_NAME>`).

## IMS-token ophalen

>[!NOTE]
>
>Alle omgevingsvariabelen zijn hoofdlettergevoelig, dus gebruik altijd de exacte variabelenaam.

1. Volg deze [API&#39;s van Experience Platforms verifiëren en openen](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html) documentatie om de Token van de Toegang te produceren.
1. Sla de waarde van het Toegangstoken in genoemde variabelen van het Milieu op `ACCESS_TOKEN`.
1. Andere verificatie-gerelateerde waarden opslaan, zoals `API_KEY`, `IMS_ORG` en `SANDBOX_NAME` in Omgevingsvariabelen.

>[!IMPORTANT]
>
>Voordat u een API uit Postman uitvoert, moet u controleren of alle vereiste omgevingsvariabelen moeten worden toegevoegd.

## De reis publiceren die is gemaakt door Playbook

Er zijn twee manieren om de reis te publiceren; u kunt om het even welke van hen kiezen:

1. **De interface van AJO gebruiken** - klik op de koppeling Reis op `Bill Of Material Page`; dit leidt u naar de pagina Journey waarop u kunt klikken **[!UICONTROL Publish]** en Journey worden gepubliceerd.

   ![Reisobject](../assets/journey-object.png)

1. **De Postman API gebruiken**

   1. Trigger **[!DNL Publish Journey]** verzoek van **[!DNL Journey Publish]** > **[!DNL Queue journey publish job]**.
   1. Het kan enige tijd duren voordat de publicatiestatus op reis is gecontroleerd. Voer daarom de publicatiestatus-API voor de publicatiestatus controleren uit tot de `response.status` is `SUCCESS`, zorg ervoor om 10 tot 15 seconden te wachten als de reis publiceert tijd vergt.

   >[!NOTE]
   >
   >Alle omgevingsvariabelen zijn hoofdlettergevoelig, dus gebruik altijd de exacte variabelenaam.

## Maak kennis met het profiel van de klant

>[!TIP]
>
>U kunt hetzelfde e-mailadres opnieuw gebruiken door het toe te voegen `+<variable>` in uw e-mail, bijvoorbeeld `usertest@email.com` kan worden gebruikt zoals `usertest+v1@email.com` of `usertest+24jul@email.com`. Dit is handig als u telkens een nieuw profiel wilt gebruiken, maar toch dezelfde e-mailadres gebruikt.

1. De eerste gebruiker moet de **[!DNL customer dataset]** en **[!DNL HTTP Streaming Inlet Connection]**.
1. Als u al **[!DNL customer dataset]** en **[!DNL HTTP Streaming Inlet Connection]**, ga verder met de stap `5`.
1. Trigger **[!DNL Customer Profile Ingestion]** > **[!DNL Create Customer Profile InletId]** > **[!DNL Create Dataset]** om **[!DNL customer dataset]**; hiermee wordt een `CustomerProfile_dataset_id` in postmanomgevingsvariabelen.
1. Maken **[!DNL HTTP Streaming Inlet Connection]**, gebruik Postman API&#39;s onder **[!DNL Customer Profile Ingestion > Create Customer Profile InletId]**.

   1. `CustomerProfile_dataset_id` moet beschikbaar zijn in postman omgevingsvariabelen, als dit niet het geval is, zie stap `3`.
   1. Trigger **[!DNL `CREATE Base Connection`]** tot [!DNL create base connection].
   1. Trigger **[!DNL `CREATE Source Connection`]** tot [!DNL create source connection].
   1. Trigger **[!DNL `CREATE Target Connection`]** tot [!DNL create target connection].
   1. Trigger **[!DNL `CREATE Dataflow`]** tot [!DNL create dataflow].
   1. Trigger **[!DNL `GET Base Connection`]**- automatisch opslaan `CustomerProfile_inlet_id` in de postmanomagevariabelen.

1. In deze stap moet u beschikken over `CustomerProfile_dataset_id` en `CustomerProfile_inlet_id` in postmanomgevingsvariabelen; zo niet, gelieve stap te verwijzen `3` of `4` respectievelijk.
1. Om klant in te voeren, moet de gebruiker opslaan `customer_country_code`, `customer_mobile_no`, `customer_first_name`, `customer_last_name` en `email` in postmanomgevingsvariabelen.

   1. `customer_country_code` zou de landcode van het mobiele nummer zijn, bijvoorbeeld `91` of `1`
   1. `customer_mobile_no` zou een mobiel nummer zijn, bijvoorbeeld `9987654321`
   1. `customer_first_name` zou de voornaam van de gebruiker zijn
   1. `customer_last_name` zou de achternaam van de gebruiker zijn
   1. `email` Het e-mailadres van de gebruiker zou zijn, dit is essentieel om verschillende e-mailadressen te gebruiken zodat een nieuw profiel kan worden opgenomen.

1. Postman-aanvraag bijwerken **[!DNL Customer Ingestion]** > **[!DNL Customer Streaming Ingestion]** het voorkeurkanaal van de klant wijzigen; standaard [!DNL `email`] wordt gevormd in verzoek.

   ```js
   "consents": {
       "marketing": {
           "preferred": "email",
           "email": {
               "val": "y"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "n"
           }
       }
   }
   ```

1. Gprefereerd kanaal wijzigen in `sms` of `push` en maak de respectievelijke kanaalwaarde aan `y` en `n` naar andere waarden, bijvoorbeeld

   ```js
   "consents": {
       "marketing": {
           "preferred": "sms",
           "email": {
               "val": "n"
           },
           "push": {
               "val": "n"
           },
           "sms": {
               "val": "y"
           }
       }
   }
   ```

1. Eindtrigger **[!DNL `Customer Profile Ingestion > Customer Profile Streaming Ingestion`]** om het klantprofiel in te voeren.

## Ingest-gebeurtenis

1. De eerste keer dat de gebruiker de **[!DNL event dataset]** en **[!DNL HTTP Streaming Inlet Connection for events]**
1. Als u al **[!DNL event dataset]** en **[!DNL HTTP Streaming Inlet Connection for events]**, ga verder met de stap `5`.
1. Trigger **[!DNL `Schemas Data Ingestion > AEP Demo Schema Ingestion > Create AEP Demo Schema InletId > Create Dataset`]** om **[!DNL event dataset]**, wordt een `AEPDemoSchema_dataset_id` in postboomomvariabelen
1. Maken **[!DNL HTTP Streaming Inlet Connection for events]**, gebruik Postman API&#39;s onder **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL Create AEP Demo Schema InletId]**.

   1. `AEPDemoSchema_dataset_id` moet beschikbaar zijn in postman omgevingsvariabelen, als dit niet het geval is, zie stap `3`
   1. Trigger **[!DNL `CREATE Base Connection`]** tot [!DNL create base connection]
   1. Trigger **[!DNL `CREATE Source Connection`]** tot [!DNL create source connection]
   1. Trigger **[!DNL `CREATE Target Connection`]** tot [!DNL create target connection]
   1. Trigger **[!DNL `CREATE Dataflow`]** tot [!DNL create dataflow]
   1. Trigger **[!DNL `GET Base Connection`]**- automatisch opslaan `AEPDemoSchema_inlet_id` in de postboomomvariabelen

1. In deze stap moet u beschikken over `AEPDemoSchema_dataset_id` en `AEPDemoSchema_inlet_id` in postboomomvariabelen , zo niet , gelieve stap te verwijzen `3` of `4` respectievelijk
1. Om gebeurtenis in te voeren, moet de gebruiker de tijdvariabele veranderen `timestamp` op verzoek **[!DNL Schemas Data Ingestion]** > **[!DNL AEP Demo Schema Ingestion]** > **[!DNL AEP Demo Schema Streaming Ingestion]** in postbode.

   1. `timestamp` Als het tijdstip van gebeurtenis zou optreden, gebruikt u het huidige tijdstempel, bijvoorbeeld `2023-07-21T16:37:52+05:30` Pas de tijdzone naar wens aan.

1. Trigger **[!DNL Schemas Data Ingestion > AEP Demo Schema Ingestion > AEP Demo Schema Streaming Ingestion]** om het evenement in te voeren, zodat de reis kan worden geactiveerd

## Laatste validatie

U moet een bericht ontvangen over het geselecteerde voorkeurkanaal dat in **[!DNL Ingest the Customer Profile]** stap `8`

* `SMS` indien voorkeurkanaal `sms` op `customer_country_code` en `customer_mobile_no`
* `Email` indien voorkeurkanaal `email` op `email`

U kunt ook controleren `Journey Report`klikt u op `Journey Object` op `Bill of Materials page` dit leidt u naar `Journey Details page`.

Voor elke gepubliceerde Reis-gebruiker moet een **[!UICONTROL View report]** knop
![Pagina Reisrapport](../assets/journey-report-page.png)


## Opruimen

Gelieve te hebben niet de veelvoudige instanties van `Journey` Als u tegelijkertijd werkt, moet u de Reis stoppen als deze alleen geldig is nadat de validatie is voltooid.
