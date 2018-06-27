---
title: Canonn APIv2 - Docs

language_tabs:
  - examples
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

![Canonn R&D Logo](images/rd-logo.png "Canonn R&D")

Welcome to the Canonn APIv2! Our API is built to catelog and store information for the game Elite: Dangerous. Within these docs we will show you examples of how to use our data to build tools or to send data to us.

It should be noted that right now we support a limited number of "sites" and that more will be added as soon as we can.

## Current Version

The current version of the CAPIv2 is `v2.0.6` and is still in active development and testing. If you would like to contribute please PM DMehaffy on discord `DMehaffy#1337`

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

# Filters, Sort, and Limit

Note that filters can only be used on `GET` requests. If you feel there is another place these can be used please file an issue on our github.

## Filters

> Find Systems matching a certain region:

```examples
GET https://api.canonn.tech/system?systemName_contains=Pleiades Sector
```

> Find Locations by type:

```examples
GET https://api.canonn.tech/location?type=GR
```


Easily filter results according to fields values.

* `=` : Equals
* `_ne` : Not equals
* `_lt` : Lower than
* `_gt` : Greater than
* `_lte` : Lower than or equal to
* `_gte` : Greater than or equal to
* `_contains` : Contains
* `_containss` : Contains case sensitive

You can also combine multiple filters to narrow your search, or combine them with some of the options below.

## Sort

> Examples of sorting

```examples
GET https://api.canonn.tech/system?_sort=systemName:asc

GET https://api.canonn.tech/grsite?_sort=type:desc
```

Sorting allows you to sort the response based on whatever part of the data you wish in either ascending or descending

## Limit

> Example of limiting the GR Reports

```examples
GET https://api.canonn.tech/grrport?_limit=50
```

Limits by default are set to 100 on all `GET` requests, however if you need more data you can manually set the limit. Please be careful when doing so, as this could put a high amount of load on our servers thus causing issues for other users.

## Start

> Example of paging records

```examples
Grab the first "page":
GET https://api.canonn.tech/system?_start=0&_limit=50

Grab the second "page:
GET https://api.canonn.tech/system?_start=50&_limit=50
```

Due to how our limits work, you can also grab data in "pages" by using the start option. In this case the start option allows you define the depth of the records (not by id)

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

`POST https://api.canonn.tech/grreport`

In addition to the typical report you can also submit active groups and active obelisks:

`POST https://api.canonn.tech/grobeliskgroupreport`

`POST https://api.canonn.tech/grobeliskreport`

Note that you will need to post a `grreport` first as both of these endpoints will ask for the report ID. You will also need to reference the following endpoints to get the ID of the obelisk and the obelisk group:

`GET https://api.canonn.tech/grobeliskgroup`

`GET https://api.canonn.tech/grobelisk`

You can further narrow down these based on the selected type to filter these get requests. Please see the Filters documentation.

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/grreport/ID`

To update an obelisk or obelisk group report use the following endpoints:

`PUT https://api.canonn.tech/grobeliskgroupreport/ID`

`PUT https://api.canonn.tech/grobeliskreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/grreport`

Note that this endpoint will also include the related reports with the obelisks and obelisk groups

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/grreport/ID`

Note that this endpoint will also include the related reports with the obelisks and obelisk groups

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/grreport/count`

To get a count of the obelisk group reports:

`GET https://api.canonn.tech/grobeliskgroupreport/count`

To get a count of obelisk reports:

`GET https://api.canonn.tech/grobeliskreport/count`

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
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to

---

#### GR Obelisk & Group Structure

> Example response with obelisk and obelisk groups (Do not attempt to send this entire response, nested posts are not allowed)

```
{
    "id": 1,
    "systemName": "SYNUEFE XR-H D11-102",
    "bodyName": "1 B",
    "latitude": -31.7347,
    "longitude": -128.9212,
    "type": "Beta",
    "cmdrName": "Dr Arcanonn",
    "reportStatus": "pending",
    "comment": null,
    "voteCount": null,
    "added": false,
    "site": null,
    "created_at": "2018-06-24T06:50:58.000Z",
    "updated_at": "2018-06-24T07:03:41.000Z",
    "grobeliskgroupreport": [
        {
            "id": 1,
            "grreport": 1,
            "grobeliskgroup": 18,
            "created_at": "2018-06-24T06:53:39.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 2,
            "grreport": 1,
            "grobeliskgroup": 20,
            "created_at": "2018-06-24T06:54:15.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 3,
            "grreport": 1,
            "grobeliskgroup": 21,
            "created_at": "2018-06-24T06:54:31.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 4,
            "grreport": 1,
            "grobeliskgroup": 24,
            "created_at": "2018-06-24T06:54:59.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 5,
            "grreport": 1,
            "grobeliskgroup": 27,
            "created_at": "2018-06-24T06:55:04.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 6,
            "grreport": 1,
            "grobeliskgroup": 34,
            "created_at": "2018-06-24T06:55:25.000Z",
            "updated_at": "2018-06-24T07:09:58.000Z"
        },
        {
            "id": 7,
            "grreport": 1,
            "grobeliskgroup": 37,
            "created_at": "2018-06-24T06:55:29.000Z",
            "updated_at": "2018-06-24T06:55:29.000Z"
        }
    ],
    "grobeliskreport": [
        {
            "id": 1,
            "grreport": 1,
            "grobelisk": 278,
            "created_at": "2018-06-24T06:57:38.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 2,
            "grreport": 1,
            "grobelisk": 295,
            "created_at": "2018-06-24T06:59:38.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 3,
            "grreport": 1,
            "grobelisk": 301,
            "created_at": "2018-06-24T07:00:11.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 4,
            "grreport": 1,
            "grobelisk": 317,
            "created_at": "2018-06-24T07:00:38.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 5,
            "grreport": 1,
            "grobelisk": 374,
            "created_at": "2018-06-24T07:02:33.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 6,
            "grreport": 1,
            "grobelisk": 376,
            "created_at": "2018-06-24T07:02:43.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 7,
            "grreport": 1,
            "grobelisk": 401,
            "created_at": "2018-06-24T07:02:56.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 8,
            "grreport": 1,
            "grobelisk": 503,
            "created_at": "2018-06-24T07:03:12.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 9,
            "grreport": 1,
            "grobelisk": 553,
            "created_at": "2018-06-24T07:03:31.000Z",
            "updated_at": "2018-06-24T07:10:00.000Z"
        },
        {
            "id": 10,
            "grreport": 1,
            "grobelisk": 555,
            "created_at": "2018-06-24T07:03:41.000Z",
            "updated_at": "2018-06-24T07:03:41.000Z"
        }
    ],
    "screenshot": []
}
```

**GR Obelisk Group Report**

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
grreport | int | Yes | 1 | ID of the GR Report
grobeliskgroup | int | Yes | 18 | ID of the Obelisk Group

**GR Obelisk Report**

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
grreport | int | Yes | 1 | ID of the GR Report
grobelisk | int | Yes | 278 | ID of the Obelisk

### Guardian Structures

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/gsreport`

In addition to the typical report you can also submit active groups and active obelisks:

`POST https://api.canonn.tech/gsobeliskgroupreport`

`POST https://api.canonn.tech/gsobeliskreport`

Note that you will need to post a `grreport` first as both of these endpoints will ask for the report ID. You will also need to reference the following endpoints to get the ID of the obelisk and the obelisk group:

`GET https://api.canonn.tech/gsobeliskgroup`

`GET https://api.canonn.tech/gsobelisk`

You can further narrow down these based on the selected type to filter these get requests. Please see the Filters documentation.

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/gsreport/ID`

To update an obelisk or obelisk group report use the following endpoints:

`PUT https://api.canonn.tech/gsobeliskgroupreport/ID`

`PUT https://api.canonn.tech/gsobeliskreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/gsreport`

Note that this endpoint will also include the related reports with the obelisks and obelisk groups

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/gsreport/ID`

Note that this endpoint will also include the related reports with the obelisks and obelisk groups

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/gsreport/count`

To get a count of the obelisk group reports:

`GET https://api.canonn.tech/gsobeliskgroupreport/count`

To get a count of obelisk reports:

`GET https://api.canonn.tech/gsobeliskreport/count`

---

#### GS Report Structure

> Example Structure in JSON coming soon!

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | Meene | Name of System
bodyName | string | Yes | AB 5 A | Name of Body
latitude | decimal | Yes | 45.9034 | Latitude should match this format 80.4567
longitude | decimal | Yes | 148.9801 | Longitude should match this format 80.4567
type | enum | Yes | Structure | *Right now only Structure*
hasDatabank | bool | false | Databank tracking
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to

---

#### GS Obelisk & Group Structure

> Example response with obelisk and obelisk groups coming soon!

**GS Obelisk Group Report**

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
gsreport | int | Yes | 1 | ID of the GS Report
gsobeliskgroup | int | Yes | 18 | ID of the Obelisk Group

**GS Obelisk Report**

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
gsreport | int | Yes | 1 | ID of the GS Report
gsobelisk | int | Yes | 278 | ID of the Obelisk

## Thargoids

### Thargoid Barnacles

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/tbreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/tbreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/tbreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/tbreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/tbreport/count`

---

#### TB Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "Pleiades Sector IH-V C2-16",
        "bodyName": "D 2",
        "latitude": 43.3664,
        "longitude": 155.3691,
        "type": "Barnacle",
        "count": 42,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | Pleiades Sector IH-V C2-16 | Name of System
bodyName | string | Yes | D 2 | Name of Body
latitude | decimal | Yes | 43.3664 | Latitude should match this format 80.4567
longitude | decimal | Yes | 155.3691 | Longitude should match this format 80.4567
type | enum | Yes | Barnacle | Barnacle or Megabarnacle
count | int | no | 42 | Optional count of the Barnacles
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Thargoid Structures

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/tsreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/tsreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/tsreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/tsreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/tsreport/count`

---

#### TS Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "COL 285 SECTOR CV-Y D57",
        "bodyName": "AB 4 A",
        "latitude": 4.7654,
        "longitude": 136.2398,
        "status": "Inactive",
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | COL 285 SECTOR CV-Y D57 | Name of System
bodyName | string | Yes | AB 4 A | Name of Body
latitude | decimal | Yes | 4.7654 | Latitude should match this format 80.4567
longitude | decimal | Yes | 136.2398 | Longitude should match this format 80.4567
status | enum | Yes | Inactive | Active or Inactive
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


## Biology

### Brain Trees

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/btreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/btreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/btreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/btreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/btreport/count`

---

#### BT Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "COL 173 SECTOR BH-L D8-72",
        "bodyName": "B 5 A",
        "latitude": -10.8164,
        "longitude": -43.3248,
        "count": 10,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | COL 173 SECTOR BH-L D8-72 | Name of System
bodyName | string | Yes | B 5 A | Name of Body
latitude | decimal | Yes | -10.8164 | Latitude should match this format 80.4567
longitude | decimal | Yes | -43.3248 | Longitude should match this format 80.4567
count | int | No | 10 | Optional count of Brain Trees
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Fungal Gourds

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/fgreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/fgreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/fgreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/fgreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/fgreport/count`

---

#### FG Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "HIP 39768",
        "bodyName": "A 14 D A",
        "latitude": 21.9127,
        "longitude": 24.5374,
        "count": 26,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | HIP 39768 | Name of System
bodyName | string | Yes | A 14 D A | Name of Body
latitude | decimal | Yes | 21.9127 | Latitude should match this format 80.4567
longitude | decimal | Yes | 24.5374 | Longitude should match this format 80.4567
count | int | No | 26 | Optional count of Brain Trees
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Tube Worms

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/twreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/twreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/twreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/twreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/twreport/count`

---

#### TW Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "HIP 39768",
        "bodyName": "A 14 D A",
        "latitude": 21.9127,
        "longitude": 24.5374,
        "count": 26,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | HIP 39768 | Name of System
bodyName | string | Yes | A 14 D A | Name of Body
latitude | decimal | Yes | 21.9127 | Latitude should match this format 80.4567
longitude | decimal | Yes | 24.5374 | Longitude should match this format 80.4567
count | int | No | 26 | Optional count of Tube Worms
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


## Geology

### Bark Mounds

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/bmreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/bmreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/bmreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/bmreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/bmreport/count`

---

#### BM Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "FLOAWNS BV-V D3-1123",
        "bodyName": "AB 1 A",
        "latitude": 51.4334,
        "longitude": 17.5896,
        "count": 41,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | FLOAWNS BV-V D3-1123 | Name of System
bodyName | string | Yes | AB 1 A | Name of Body
latitude | decimal | Yes | 21.9127 | Latitude should match this format 80.4567
longitude | decimal | Yes | 24.5374 | Longitude should match this format 80.4567
count | int | No | 41 | Optional count of Bark Mounds
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Fumaroles

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/fmreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/fmreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/fmreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/fmreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/fmreport/count`

---

#### FM Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "BLU EUQ BM-C D13-24",
        "bodyName": "ABC 1 C A",
        "latitude": -67.1605,
        "longitude": 55.5439,
        "type": "Silicate Vapour",
        "count": 21,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | BLU EUQ BM-C D13-24 | Name of System
bodyName | string | Yes | ABC 1 C A | Name of Body
latitude | decimal | Yes | 21.9127 | Latitude should match this format 80.4567
longitude | decimal | Yes | 24.5374 | Longitude should match this format 80.4567
type | enum | Yes | Silicate Vapour | *List coming soon*
count | int | No | 21 | Optional count of Tube Worms
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Geysers

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/gyreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/gyreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/gyreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/gyreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/gyreport/count`

---

#### GY Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "SNOTRICOPA",
        "bodyName": "2 D A",
        "latitude": 61.2685,
        "longitude": 155.0603,
        "type": "Water Vapour",
        "count": 13,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | SNOTRICOPA | Name of System
bodyName | string | Yes | 2 D A | Name of Body
latitude | decimal | Yes | 61.2685 | Latitude should match this format 80.4567
longitude | decimal | Yes | 155.0603 | Longitude should match this format 80.4567
type | enum | Yes | Water Vapour | *List coming soon*
count | int | No | 13 | Optional count of Geysers
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Lava Spouts

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/lsreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/lsreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/lsreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/lsreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/lsreport/count`

---

#### LS Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "TEGNAE HT-Z D13-1",
        "bodyName": "1 D A",
        "latitude": 21.9127,
        "longitude": 24.5374,
        "count": 26,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | TEGNAE HT-Z D13-1 | Name of System
bodyName | string | Yes | 1 D A | Name of Body
latitude | decimal | Yes | -6.9893 | Latitude should match this format 80.4567
longitude | decimal | Yes | -152.8119 | Longitude should match this format 80.4567
count | int | No | 26 | Optional count of Lava Spouts
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


## Orbial

### Generation Ships

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/genreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/genreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/genreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/genreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/genreport/count`

---

#### GEN Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "Lalande 2966",
        "orbitBody": "4",
        "shipName": "Generation Ship Hyperion",
        "directionSystem": "Yemaki",
        "distance": 7340,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | Lalande 2966 | Name of System
orbitBody | string | No | 4 | Name of Body the ship orbits
shipName | string | Yes | Generation Ship Hyperion | The name of the ship including "Generation Ship"
directionSystem | string | No | Yemaki | System to fly towards to get to the ship
distance | int | No | 7340 | Distance in Ls to fly towards
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to


### Megaships

---

#### Create a new Report

To send a new report:

`POST https://api.canonn.tech/msreport`

<aside class="notice">Please only send the data we ask for, see the structure docs for more info</aside>

---

#### Update an existing Report

To update a report:

`PUT https://api.canonn.tech/msreport/ID`

<aside class="notice">Replace `ID` with the report id, you also only need to send changed fields</aside>
<aside class="warning">Please only update reports that belong to you</aside>

---

#### Get a list of all reports

To get a list of all reports:

`GET https://api.canonn.tech/msreport`

<aside class="notice">By default "GET" requests are limited to 100 entries, see the filters info to increase this</aside>

---

#### Get a specific report

To get a specific report by ID:

`GET https://api.canonn.tech/msreport/ID`

<aside class="notice">Replace `ID` with the report id</aside>

---

#### Get a count of all reports

To get a count of all the reports:

`GET https://api.canonn.tech/msreport/count`

---

#### MS Report Structure

> Example Structure in JSON

```json
[
    {
        "systemName": "PLEIADES SECTOR IR-W D1-55",
        "orbitBody": "5 A",
        "shipName": "BTG-237",
        "shipTag": "btg237",
        "type": "Banner Class Bulk Cargo Ship",
        "flightOps": false,
        "flightSchedule": false,
        "dockable": false,
        "cmdrName": "Dr Arcanonn",
        "reportStatus": "pending"
    }
]
```

Parameter | Type | Required | Example | Description |
--------- | ---- | -------- | ------- | ----------- |
systemName | string | Yes | PLEIADES SECTOR IR-W D1-55 | Name of System
orbitBody | string | No | 4 | Name of Body the ship orbits
shipName | string | Yes | BTG-237 | The name of the ship
shipTag | string | No | btg237 | Simple lowercase shortcode of the name
type | enum | Yes | Banner Class Bulk Cargo Ship | See Type List below, if your unsure use "Unknown"
flightOps | bool | Yes | false | Does the ship have flightOps capability
flightSchedule | bool | false | N/A | Does the ship have a schedule
dockable | bool | false | N/A | Can you dock with the ship
cmdrName | string | Yes | Dr Arcanonn | Only one CMDR per report please
cmdrComment | text | No | N/A | Optional comments provided by the CMDR
screenshot | upload | No | N/A | More information on this will come soon
reportStatus | enum | Yes | **pending** | You MUST send "pending"
reportComment | text | **CanonnOnly** | N/A | Comments provided by Canonn on report
voteCount | int | **CanonnOnly** | N/A | Used by our EDMC Plugin to crowd source data
added | bool | **CanonnOnly** | N/A | Used to note we have added the site to our list
site | relationID | **CanonnOnly** | N/A | References the site the report is attached to

#### MS Type List

Types:

* Alcatraz Class Prison Ship
* Aquarius Class Tanker
* Banner Class Bulk Cargo Ship
* Bellmarsh Class Prison Ship
* Bowman Class Science Vessel
* Demeter Class Agricultural Vessel
* Dionysus Class Agricultural Vessel
* Gordon Class Bulk Cargo Ship
* Henry Class Bulk Cargo Ship
* Hercules Class Bulk Cargo Ship
* Hogan Class Bulk Cargo Ship
* Lowell Class Science Vessel
* Megaship
* Naphtha Class Tanker
* Riker Class Prison Ship
* Sagan Class Tourist Ship
* Samson Class Bulk Cargo Ship
* Sanchez Class Science Vessel
* Survey Vessel
* Thomas Class Bulk Cargo Ship
* Unknown

---

### Unknown Signal Sources
<aside class="warning">Currently this endpoint is still being developed, so at this time we cannot accept reporting on this.</aside>

# Offical Site Lists

Coming Soon!