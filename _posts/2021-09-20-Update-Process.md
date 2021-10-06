---
title: Cadenas de caracteres en Python
author: Francisco Sola
date: 2021-09-20 00:35:00 -0
categories: [Blogging, Python]
tags: [python, code, database]
---


### Clase Database

Clase encargada de conectar con la base de datos y funciones para realizar diferentes consultas

```python
    
import pymysql
from warnings import filterwarnings
filterwarnings('ignore', category=pymysql.Warning)


class Database:
    def __init__(self):
        self.connection = pymysql.connect(
            host='IP',
            port=3306,
            user='user',
            password='password',
            db='db'
        )

        try:
            self.cursor = self.connection.cursor()

        except Exception as e:
            print("Error al conectar con la BD " + e)
            raise

    def __disconnect__(self):

        self.connection.close()

    def select_data(self):

        sql = 'SELECT * FROM table'

        try:
            self.cursor.execute(sql)
            data = self.cursor.fetchall()

            return data
        except Exception as e:
            print("Error en función select_data " + e)
            raise

        finally:
            self.__disconnect__()


    def update_table(self, campo1, campo):

        sql = 'UPDATE `table` SET  `campo1`= %s WHERE `campo` = %s'
        
        values = (campo1, campo)
        try:
            self.cursor.execute(sql, values)
            self.connection.commit()

        except pymysql.IntegrityError as e:
            if not e[0] == 1062:
                raise
            print("Oops! Fallo al actualizar datos")
        finally:

            self.__disconnect__()
    
    
```

### Actualizar campos en base de datos con Python 


```python
    
    from datetime import datetime
    import time
    import DB
    
    
    def update_proces(creation_date):

    try:
        db = DB.Database()

        idprocess = str(socket.gethostname()) + "_process"
        process_pid = os.getpid()
        process_name = "processControl.py"
        update_date = str(datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
        status = True
        pc = socket.gethostname()

        print('Actualizado: ', process_name, update_date, pc)

        db.update_proces(idprocess, process_name, process_pid,
                          update_date, creation_date, status, pc)

    except Exception as e:
        print('##### Error en función update_process ####', e)
        raise

```