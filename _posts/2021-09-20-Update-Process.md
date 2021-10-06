---
title: Conectar a Base de datos con Python
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

        sql = 'SELECT * FROM process'

        try:
            self.cursor.execute(sql)
            ticker = self.cursor.fetchall()

            return ticker
        except Exception as e:
            print("Error en función select_data " + e)
            raise

        finally:
            self.__disconnect__()


    def update_status_proces(self, status, idprocess):

        sql = 'UPDATE `process` SET  `status`= %s WHERE `idprocess` = %s'
        
        values = (status, idprocess)
        try:
            self.cursor.execute(sql, values)
            self.connection.commit()

        except pymysql.IntegrityError as e:
            if not e[0] == 1062:
                raise
            print("Oops! Fallo al insertar en la tabla process")
        finally:

            self.__disconnect__()
    

    def update_proces(self, idprocess, process_name, process_pid,  update_date, creation_date, status, pc):

        sql = 'UPDATE `process` SET  `idprocess` = %s, `process_name`= %s, `process_pid`=%s, `update_date`= %s, `creation_date`= %s, `status`= %s, `pc`= %s WHERE `idprocess` = %s'
        
        values = (idprocess, process_name, process_pid, update_date, creation_date, status, pc, idprocess)
        try:
            self.cursor.execute(sql, values)
            self.connection.commit()

        except pymysql.IntegrityError as e:
            if not e[0] == 1062:
                raise
            print("Oops! Fallo al insertar en la tabla process")
        finally:

            self.__disconnect__()
    
    
```

### Actualizar campos en base de datos con Python 


```python
    
    from datetime import datetime
    import time
    import DB
    
    
    def update_process(creation_date):

    try:
        db = DB.Database()

        idprocess = str(socket.gethostname()) + "_process"
        process_pid = os.getpid()
        process_name = "processControl.py"
        update_date = str(datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
        status = True
        pc = socket.gethostname()

        db.update_proces(idprocess, process_name, process_pid,
                          update_date, creation_date, status, pc)

        print('Actualizado: ', process_name, update_date, pc)

    except Exception as e:
        print('##### Error en función update_process ####', e)
        raise

```