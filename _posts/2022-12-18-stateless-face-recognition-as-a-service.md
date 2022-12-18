---
layout: post
title: Facesenpai: Stateless Facerecognition as A Service
date: 2022-12-18 22:10:31
categories: AI
tags: facerecognition api ai
author: guangrei
---

Pada kesempatan kali ini aku ingin sedikit menjelaskan project lamaku yang baru-baru ini di update dan diberi nama facesenpai.

## Dimulai dari face detection vs face recognition

kita dapat melakukan face detection dengan  cepat dan resource yang sedikit tapi untuk face recognition diperlukan algorithma AI atau kecerdasan buatan agar dapat mengextract face descriptor dari wajah yang terdeteksi. 

Library `facerecognition` yang ada dipython dapat membatu dalam memproses pengenalan wajah dengan cepat, namun library ini tidaklah pure python dan memerlukan dlib yang dibuat dengan C++.

Kompilasi dan pemasangan pada case dan platform tertentu bisa jadi sangat tricky dan untuk itulah face_senpai dibuat :)

## Microservice

Jadi bisa dibilang face_senpai ini adalah microservice untuk mengextract face descriptor dari suatu foto.

Ide awalnya face_senpai ini menserialize numpy array menggunakan `jsonpickle` sehingga response apinya hanya bisa diproses dengan python dan micropython.

Untuk update sekarang sudah tidak lagi menggunakan json pickle jadi bisa compatible dengan bahasa pemrogaman apapun karena face descriptor berupa 2d Array.

## Demo

Karena heroku sudah tidak lagi gratis maka untuk sementara demo api ditiadakan dulu.

github https://github.com/guangrei/face_senpai