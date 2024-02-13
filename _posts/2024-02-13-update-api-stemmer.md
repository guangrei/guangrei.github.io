---
layout: post
title: Update API Stemmer
date: 2024-02-13 15:03:31
categories: api
tags: api, nlp, stemmer
author: guangrei
---

Hi, buat yang pakai api stemmer yang dulu aku buat, sekarang ada perubahan nih. <!--more-->

yakni endpoints yang tadinya host di vercel, sekarang pindah ke `pythonanywhere` dan untuk methodnya juga ada perubahan.

Api endpoints :`https://grei.pythonanywhere.com/api/stemmer`

Method: `POST`

Headers: `Content-Type:application/json`

Body:
```
{
  "text": "diperlukan",
  "language": "optional"
}
```
Parameter:
- `text`: text yang ingin dianalysis.

- `language`: bahasa yang digunakan, default indonesian.

contoh penggunaan api dengan php
```php
<?php
function stemmer($text, $language = "indonesian")
{
    $api = 'https://grei.pythonanywhere.com/api/stemmer';
    $data=json_encode(array( "text"=>$text, "language"=>$language));
    $ch = curl_init($api);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type:application/json'));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $result = curl_exec($ch);
    curl_close($ch);
    $data = json_decode($result, true);
    if($data["status"] == "success") {
        return $data["result"];
    } else{
        throw new Exception($data["reason"]);
    }
}

$test = stemmer("Selamat pagi dunia!");
print_r($test);
```