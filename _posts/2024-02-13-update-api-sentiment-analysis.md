---
layout: post
title: Update API Sentiment Analysis
date: 2024-02-13 15:03:25
categories: api
tags: api, sentiment, analysis, nlp
author: guangrei
---

Hi, buat yang pakai api sentiment analysis yang dulu aku buat, sekarang ada perubahan nih. <!--more-->

yakni endpoints yang tadinya host di vercel, sekarang pindah ke `pythonanywhere` dan untuk methodnya juga ada perubahan.

Api endpoints :`https://grei.pythonanywhere.com/api/id_sentiment_analysis`

Method: `POST`

Headers: `Content-Type:application/json`

Body: `{"text": "diperlukan"}`

Parameter:
- `text`: text yang ingin dianalysis.

contoh penggunaan api dengan php
```php
<?php
function sentiment_analysis($text)
{
    $api = 'https://grei.pythonanywhere.com/api/id_sentiment_analysis';
    $data=json_encode(array( "text"=>$text));
    $ch = curl_init($api);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type:application/json'));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $result = curl_exec($ch);
    curl_close($ch);
    $data = json_decode($result, true);
    if ($data["status"] == "error" && $data["reason"] == "silahkan tambahkan lebih banyak kalimat!") {
        return "Neutral";
    } elseif($data["status"] == "success") {
        return $data["sentiment"];
    } else{
        throw new Exception($data["reason"]);
    }
}

echo sentiment_analysis("saya sedang belajar nlp");
```