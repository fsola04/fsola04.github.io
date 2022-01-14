---
title: Crear archivos de Texto con Python
author: Francisco Sola
date: 2021-11-10 19:14:00 -0
categories: [Blogging, Python]
tags: [python, code, files]
---

### Crear archivos de texto con Python

Crear un archivo txt.

```python
    
    with open("prueba.txt", "w") as text_file:
        text_file.write("Este texto se insertará en un archivo llamado prueba.txt ")

```

### Crear archivos html con Python

Crear un archivo html con encoding.

```python
    
    with open("html1.html", "w", encoding="utf-8") as text_file:
        text_file.write(response)

```

### Crear archivos json con Python

Crear un archivo json.

```python
    
    with open("file.json", "w") as text_file:
        json.dump(data, text_file)

```


...to be continued