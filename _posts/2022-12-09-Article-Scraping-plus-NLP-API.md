---
layout: post
title: Article Scraping + Nlp API
date: 2022-12-09 15:03:45
categories: api
tags: api nlp scraping
author: guangrei
---

Di python terdapat library `newspaper` yang bisa digunakan untuk scraping article.

library ini juga didukung oleh nlp (natural language processing) sehingga dapat mengrangkum article dan mengextract keywords.

agar lebih mudah digunakan, aku membuat api dari library ini, berikut endpointnya `https://my-awn.vercel.app/api/news.php`.

method: `Get`

parameter:

- url (required): url web article/berita.

- html (optional): set `html=1` untuk menampilkan html pada response.

- language (optional): bahasa yang digunakan untuk parsing article, default `id` (Indonesia)

sekian dan semoga bermanfaat 🙏