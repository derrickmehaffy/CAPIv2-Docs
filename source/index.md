---
title: Canonn APIv2 - Docs

language_tabs:
  - javascript
  - python
  - bash

toc_footers:
  - <a href='https://github.com/canonn-science/CAPIv2-Strapi'>CAPIv2 Github</a>
  - <a href='https://github.com/canonn-science/CAPIv2-Docs'>Help us manage these Docs!</a>
  - <a href='http://github.com/mpociot/whiteboard'>Documentation Powered by Whiteboard</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Canonn APIv2! Our API is built to catelog and store information for the game Elite: Dangerous. Within these docs we will show you examples of how to use our data to build tools or to send data to us.

It should be noted that right now we support a limited number of "sites" and that more will be added as soon as we can.

## Celestial

Below you will find the different types of celestial data we support and some of our data is cached from EDSM such as the x/y/z coords in `Systems` and most of the data in `Bodies` and `Rings`.

The celestial bodies are gathered in reports, additional information is pulled from EDSM and cached in our database for `x` amount of time. (Currently we do not have our script enabled to pull this data).

* Systems
* Bodies
    * Star
    * Planet
* Rings

## Sites

These are the sites we currently support, please note that not all of our data may be avaible on the API yet. We are currently gathering our data from our Google docs and importing it as we can (with lots of clean up of course).

Below you will see each site listed by its name and short code, we do use the shortcodes in our API (specifically in the locations model).

### Guardians

* Guardian Ruins : GR
* Guardian Structures : GS

### Thargoids

* Thargoid Barnacles : TB
* Thargoid Structures : TS

### Biology

* Brain Trees : BT
* Fungal Gourds : FG
* Tube Worms : TW

### Geology

* Bark Mounds : BM
* Fumaroles : FM
* Geysers : GY
* Lava Spouts : LS

### Orbital

* Generation Ships : GEN
* Megaships : MS
* Unknown Signal Sources : USS (Note this endpoint is subject to change often)

# Filters

Coming Soon!

# Reporting

<aside class="notice">
Please remember to only send the data we require, all reports should be sent with the reportStatus as "pending". Further details will be listed under each endpoint and required fields will be notated; fields labeled as "Canonn Only Use" should never be sent.
</aside>

<aside class="success">
All of these reporting endpoints do not require any authentication and can be used by anyone to send us data. If you have any recommendations please file a feature request on our github.
</aside>

<aside class="warning">You can only send one (1) report at a time. Attempting to send nested reports will fail!</aside>

## Guardians

### Guardian Ruins

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/grreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/grreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/grreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/grreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/grreport/count`

---

#### GR Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "Meene",
        "bodyName": "AB 5 A",
        "latitude": 45.9034,
        "longitude": 148.9801,
        "type": "Gamma",
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | Meene | Name of System
bodyName | string | Yes | AB 5 A | Name of Body
latitude | decimal | Yes | 45.9034 | Latitude should match this format 80.4567
longitude | decimal | Yes | 148.9801 | Longitude should match this format 80.4567
type | enum | Yes | Gamma | Alpha, Beta, or Gamma
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
comment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to

---

### Guardian Structures

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/gsreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/gsreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/gsreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/gsreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/gsreport/count`

---

#### GS Report Structure

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | Meene | Name of System
bodyName | string | Yes | AB 5 A | Name of Body
latitude | decimal | Yes | 45.9034 | Latitude should match this format 80.4567
longitude | decimal | Yes | 148.9801 | Longitude should match this format 80.4567
type | enum | Yes | Structure | *Right now only Structure*
hasDatabank | bool| false | Databank tracking
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
comment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to

## Thargoids

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Thargoid Barnacles

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Thargoid Structures

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

## Biology

### Brain Trees

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Fungal Gourds

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Tube Worms

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

## Geology

### Bark Mounds

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Fumaroles

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Geysers

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Lava Spouts

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

## Orbial

### Generation Ships

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Megaships

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.technology/report`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.technology/report`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.technology/report/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.technology/report/count`

---

### Unknown Signal Sources
<aside class="warning">Currently this endpoint is still being developed, so at this time we cannot accept reporting on this.</aside>

# Offical Site Lists

Coming Soon!