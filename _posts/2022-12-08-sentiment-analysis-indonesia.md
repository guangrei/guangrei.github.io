---
layout: post
title: Sentiment Analysis Indonesia API
date: 2022-12-08 19:07:47
categories: api
tags: sentiment analysis ai api nlp nltk
author: guangrei
---

Pada post kali ini aku ingin membagikan api sentiment analysis untuk bahasa indonesia, yang aku kembangkan sendiri.

Api endpoints :`https://my-awn.vercel.app/api/sentiment-analysis.php`

Method: `Get`

Parameter:

- `text`: url-encoded text yang ingin dianalysis.

contoh php:

```php
<?php
$text = "Saya sedang belajar nlp";
$response = json_decode(file_get_contents("https://my-awn.vercel.app/api/sentiment-analysis.php?text=".urlencode($text)), true);

if ($response["status"] == "success") {
	echo $response["sentiment"]; // Neutral
}
```
kamu juga bisa membuat logika labelling sendiri dari output scores, seperti:

```php
<?php
$text = "aku cinta kamu";
$response = json_decode(file_get_contents("https://my-awn.vercel.app/api/sentiment-analysis.php?text=".urlencode($text)), true);

if ($response["status"] == "success") {
	$check = $response["scores"]["insetNeg"]["compound"] + $response["scores"]["insetPos"]["compound"] > 0
	echo ["Negative", "Positive"][$check]
}
```

## Akurasi

Api sentiment analysis ini menggunakan metode standard atau sama halnya dengan yang digunakan oleh platform sentiment analysis api lainnya yang tidak dapat sepenuhnya memahami konteks kalimat atau cenderung menghapus stopwords.

Meskipun begitu cukup akurat untuk menganalysis review produk, isu politik, sosial dan iklim ditwitter dst.