---
layout: post
title: "[update] Article Scraping + Nlp API"
date: 2022-12-20 12:00
categories: api
tags: api nlp scraping
author: guangrei
---

Di python terdapat library `newspaper3k` yang bisa digunakan untuk scraping article.

library ini juga didukung oleh nlp (natural language processing) sehingga dapat mengrangkum article dan mengextract keywords.

agar lebih mudah digunakan, aku membuat api dari library ini, berikut endpointnya `https://my-awn.vercel.app/api/newspaper.php`.

method: `Get`

parameter:

- url (required): url web article/berita.

- html (optional): set `html=1` untuk menampilkan html pada response.

- language (optional): bahasa yang digunakan untuk parsing article, default `id` (Indonesia)

contoh: `https://my-awn.vercel.app/api/newspaper.php?url=https://sport.detik.com/sepakbola/bola-dunia/d-6468666/no-debat-lionel-messi-sah-jadi-goat`

sekian dan semoga bermanfaat 🙏

UPDATE: api endpoint telah diperbarui.