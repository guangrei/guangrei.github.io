---
layout: post
title: Membuat Python Strong Typed
date: 2023-03-27 02:02:48
categories: python
tags: id
author: guangrei
---

Python adalah bahasa pemrogaman dinamis dan weak typed, seperti bahasa pemrogaman interpreted pada umumnya.

Namun kita bisa membuatnya menjadi static typed dengan menambahkan anotasi dan menjalankan static type checker seperti `mypy`.

contohnya:
```
mypy --strict file.py
```

Meskipun sudah menambahkan opsi `strict`, mypy tetap tidak menampilkan error atau peringatan pada variable yang tidak memiliki anotasi, hal serupa juga terjadi pada static type checker lainnya karena jika mengimplentasikan itu akan dianggap sebagai sebuah anti-patern.

Tapi aku yang berkeinginan menulis dengan bahasa pemrograman interpreted namun memiliki rasa compiled language mencoba membuat sebuah runtime python yang strong typed.

Sayangnya setelah ngoding beberapa hari timbul rasa malas dan pikiran "mungkin cuma aku sendiri yg ingin python agar bisa strong typed" sehingga project ini untuk sementara aku hentikan sampai ada orang lain yang juga tertarik :D

Pada post ini aku akan membagikan fungsi untuk mendeteksi weak typed yang sudah aku buat, fungsi ini masih sangat jauh dari 100% dan jika ada yg berminat menyempurnakannya bisa membaca dokumentasi python tentang [ast](https://docs.python.org/3/library/ast.html).

```python
# -*-coding:utf8;-*-
import ast
import sys

sys.setrecursionlimit(10000)
alist_class_scope = {}
code = """
test:str = "ini test"
class test(object):
	def __init__(self):
		self.a:int = 4
	def coba(self):
		self.a = 7
class test2(object):
	def coba(self):
		self.a = 6
"""


def parse_strict(vars, alist, no):
    for var in vars:
        if isinstance(var, ast.Attribute):
            if var.attr not in alist:
                return f"ERR: untyped {var.attr} [line:{no}]"
        else:
            if var.id not in alist:
                return f"ERR: untyped {var.id} [line:{no}]"


def parser(tree):
    alist = []
    for body in tree.body:
        if isinstance(body, ast.AnnAssign):
            alist.append(body.target.id)
        elif isinstance(body, ast.Assign):
            check = parse_strict(body.targets, alist, body.lineno)
            if check is not None:
                quit(check)
        elif isinstance(body, ast.FunctionDef):
            check = parse_func(body, alist)
            if check is not None:
                quit(check)
        elif isinstance(body, ast.For):
            check = parse_func(body, alist)
            if check is not None:
                quit(check)
        elif isinstance(body, ast.ClassDef):
            check = parse_class(body, alist)
            if check is not None:
                quit(check)


def parse_func(tree, alist):
    alist_local_scope = alist
    for body in tree.body:
        if isinstance(body, ast.FunctionDef):
            check = parse_func(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.For):
            check = parse_func(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.ClassDef):
            check = parse_class(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.AnnAssign):
            alist_local_scope.append(body.target.id)
        elif isinstance(body, ast.Assign):
            check = parse_strict(body.targets, alist, body.lineno)
            if check is not None:
                return check


def parse_func_class(tree, name):
    for body in tree.body:
        if isinstance(body, ast.FunctionDef):
            check = parse_func(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.For):
            check = parse_func(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.ClassDef):
            check = parse_class(body, alist_local_scope)
            if check is not None:
                return check
        elif isinstance(body, ast.AnnAssign):
            alist_class_scope[name].append(body.target.attr)
        elif isinstance(body, ast.Assign):
            check = parse_strict(
                body.targets,
                alist_class_scope[name],
                body.lineno)
            if check is not None:
                return check


def parse_class(tree, alist):
    alist_local_scope = alist
    alist_class_scope[tree.name] = []

    for body in tree.body:
        if isinstance(body, ast.FunctionDef):
            check = parse_func_class(body, tree.name)
            if check is not None:
                return check


if __name__ == "__main__":
    tree = ast.parse(code.strip())
    print(parser(tree))
```

> pada fungsi diatas tidak mendeteksi antotasi pada def args, def return dan lainnya yang sudah ada pada mypy.

Selain membuat fungsi mendeteksi weak typed aku juga berencana untuk menambahkan syntax baru bernama `Declare` dan code yang dibuat dapat di compile menjadi C extension seperti yang pernah aku buat pada pada [piton lang](https://github.com/guangrei/piton-lang).

