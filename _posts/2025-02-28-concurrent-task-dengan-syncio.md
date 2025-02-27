---
layout: post
title: Concurrent Task dengan Syncio
date: 2025-02-28 15:03:31
categories: python
tags: python, module, library
author: guangrei
---

Aku membuat module [syncio](https://pypi.org/project/syncio) yang terinspirasi dari `asyncio`.
dengan module ini kita bisa dengan mudah membuat concurrent task pada lingkungan synchronous. <!--more-->

berikut contohnya:

```python
from syncio import create_task, gather


def hello(n: int) -> str:
    return f"hello {n + 1}"


tasks = [create_task(hello)(i) for i in range(3)]
results = gather(*tasks)
print("output task_1:", results["task_1"])
print("output task_2:", results["task_2"])
print("output task_3:", results["task_3"])

# atau menggunakan iterator

for result in results:
    print(result)
```

## Create task

`create_task` digunakan untuk membuat task baru dan `create_task` dapat dijalankan dengan `create_task(callable, args=(), kwargs={})` atau `create_task(callable)(*args, **kwargs)`.

## Gather

Gather adalah metode untuk menjalankan tasks secara bersamaan dan mengumpulkan outputnya. ada 2 pilihan gather:

1. `syncio.gather(*tasks)`: gather ini memanfaatkan module `multiprocessing`.

2. `syncio.thread_gather(*tasks)`: gather ini memanfaatkan module `threading`.

pilihlah gather yang pas sesuai kebutuhan.

## TaskOutput

`TaskOutput` mengumpulkan semua output dari task gather dalam format dictionary dengan `task_id` yang berdasarkan lokasi arg pada `gather()` atau `thread_gather()`.
`TaskOutput` juga bisa di iterasi dengan `for-loop` seperti pada contoh, untuk langsung mendapatkan output.

## TaskReturnException

jika ada task yang raise exception maka outputnya adalah instance dari `TaskReturnException`.
object ini memiliki atribut `exception` yang menyimpan `BaseException e` dan `print_exception()` yang fungsinya untuk menampilkan traceback.
untuk fleksibilitas `if condition` object ini bersifat `bool(False)` dan `len(0)`.

## Type Safety

`syncio` sudah menerapakan type safety yang ketat (strict) tapi hanya aktif jika menggunakan type checking seperti `mypy` (sangat direkomendasikan).
