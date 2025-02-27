---
layout: post
title: Api Daftar Hari Libur Baru
date: 2023-03-12 17:05:45
categories: api
tags: api libur json
author: guangrei
---

Pada post kali ini aku ingin memperkenalkan api daftar hari libur baru.

Sebelumnya aku sudah menyediakan api daftar hari libur yang bersumber dari Google calendar dan otomatis update.

Pada api yang baru ini masih bersumber data dari google calendar dan otomatis diperbarui, hanya saja ada verifikasi antara libur tanggal merah dengan libur bukan tanggal merah, selain itu memiliki format yang berbeda dari api sebelumnya, berikut ini sekema jsonnya:

```javascript
{
 "Y-m-d": {
 			"libur": true/false,
 			"nama": "nama hari libur"
 	},
 ...
 "info": {
 			"author": "guangrei",
 			"link": "https://github.com/guangrei",
 			"updated": "Ymd h:i:s"
 	}
}
```

- `Y-m-d` adalah format key string yang berarti tahun-bulan-tanggal.
 - `libur` adalah key untuk menandakan libur nasional dengan nilai value boolean true & false, hari libur yang bukan hari libur nasional seperti hari ibu akan ditandai dengan false.
  - `nama` adalah key untuk nama hari libur.
- `info` key ini memiliki value yang berkaitan dengan informasi api, seperti waktu terakhir pembaruan.

berikut contoh penggunaan dengan python:

```python
# -*-coding:utf8;-*-
import requests
from datetime import datetime

tahun = datetime.now().year
cek = f"{tahun}-12-25"
r = requests.get(
    "https://raw.githubusercontent.com/guangrei/Json-Indonesia-holidays/master/api.json").json()
if cek in r and r[cek]["libur"]:
    print(r[cek]["nama"])
else:
    print("tidak libur!")

```

dan contoh penggunaan dengan php:
```php
<?php

$tahun = date("Y");
$cek = $tahun."-12-25";
$req = file_get_contents("https://raw.githubusercontent.com/guangrei/Json-Indonesia-holidays/master/api.json");
$data = json_decode($req, true);
if (isset($data[$cek]) && $data[$cek]["libur"]) {
    echo $data[$cek]["nama"];
}
else{
    echo "tidak libur!";
}
    
```

seperti yang tertera diatas url api endpointnya ada di [https://raw.githubusercontent.com/guangrei/Json-Indonesia-holidays/master/api.json](https://raw.githubusercontent.com/guangrei/Json-Indonesia-holidays/master/api.json) semoga bermanfaat 