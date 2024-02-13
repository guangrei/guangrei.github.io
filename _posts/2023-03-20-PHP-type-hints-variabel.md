---
layout: post
title: PHP Type Hints Variabel
date: 2023-03-20 13:01:29
categories: php
tags: id
author: guangrei
---

Tahukah kamu jika php adalah satu-satunya bahasa pemrograman interpreted yang interpreternya bisa mengevaluasi type hints secara langsung.

type-hints sangat bermanfaat untuk memperketat code yang kita buat tanpa perlu melakukan type checking secara manual seperti `if type(variabel)`, `if isinstance(variabel, class)` dst

type-hints sudah diperkenalkan sejak php5 dan terus dikembangkan hingga sekarang (php8) tapi sayangnya belum mendukung type hints untuk pendefinisian variabel, tapi dengan adanya dukungan type hints untuk property pada php7.4 aku kepikiran sebuah ide untuk memanfaatkan anonymous class.

berikut contohnya:

```php
<?php
$struct = new class {
    public int $nomer;
    public string $text;
};
$struct->nomer = 1; // ganti 1 dengan string maka akan error :)
echo $struct->nomer;
```
> pemilihan nama variable struct digunakan agar mirip golang walaupun beda fungsinya 😁

Penggunaan anonymous class ini memiliki kekurangan tidak dapat diserialisasi, meski begitu kasus serialisasi object pada php ini sangat jarang ditemui, contoh best practice penggunaan `serialize()` dapat ditemukan pada [guangrei/queue](https://github.com/guangrei/queue).

Pada bahasa pemrograman interpreted lain kita membutuhkan static type checker untuk melakukan type checking.