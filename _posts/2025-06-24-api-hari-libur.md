---
layout: post
title: "[UPDATE] API Hari Libur yang paling akurat dan up to date"
date: 2025-06-24 13:50
categories: api
tags: id
author: guangrei
---

Hi, taukah kamu kalau project [APIHariLibur_V2](https://github.com/guangrei/APIHariLibur_V2) ada versi rest api-nya jadi tidak hanya static github raw file. <!--more-->

Untuk penggunaanya juga sangat mudah.

## Mendapatkan hari libur dengan spesifik date:

endpoints:
```
https://grei.pythonanywhere.com/id_holiday/Y-m-d
```

<script>
  var currentYear = new Date().getFullYear();
  document.write('Contoh: <a href="https://grei.pythonanywhere.com/api/id_holiday/' + currentYear + '-12-25">' + currentYear + '-12-25</a>.');
</script>
<noscript> JavaScript required to see this content! </noscript>

## List hari libur diantara 2 date:


endpoints:
```
https://grei.pythonanywhere.com/id_holiday/Y-m-d/Y-m-d
```

<script>
  var currentYear = new Date().getFullYear();
  document.write('Contoh: <a href="https://grei.pythonanywhere.com/api/id_holiday/' + currentYear + '-02-01/' + currentYear + '-04-30">'+ currentYear + '-02-01/' + currentYear + '-04-30</a>.');
</script>
<noscript> JavaScript required to see this content! </noscript>

## Response

response dari api berupa json.

contoh single event response:
```json
{"Y-m-d": "event name"}
```
contoh multi event response:
```json
{
    "Y-m-d": "event 1 name",
    "Y-m-d": "event 2 name"
}
```
jika tidak ada event maka responsenya array/list kosong.
```json
[]
```

Semoga bermanfaat 🙏
