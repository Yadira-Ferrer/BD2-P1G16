# Practica 1

**Contenido**
Comandos Docker Oracle

## Comandos para utilizar Oracle con Docker

Descarga la imagen de Oracle 18c

```bash
$ docker pull dockerhelp/docker-oracle-ee-18c
```

 Crear contenedor

```bash
$ sudo docker run -p 1521:1521 -it dockerhelp/docker-oracle-ee-18c bash
```

Ejecutamos el archivo `post_install.sh` para iniciar Oracle

```bash
$ sh post_install.sh
```

Entramos a Oracle

```bash
$ sqlplus
```

---

## 1. Habilitar FRA

Se debe colocar la base de datos en modo ARCHIVELOG:

```bash
[oracle@localhost ~]$sqlplus / as sysdba
SQL>SHUTDOWN IMMEDIATE;
SQL>STARTUP MOUNT;
SQL>ALTER DATABASE ARCHIVELOG;
SQL>ALTER DATABASE OPEN;
```

Se crea un directorio destino para los archivos.
`/home/oracle/bd2_grupo16`

Se modifica el parámetro `DB_RECOVERY_FILE_DEST_SIZE` 

```
SQL> ALTER SYSTEM SET DB_RECOVERY_FILE_DEST_SIZE = 15G SCOPE=BOTH;
```

Se modifica la ruta de almacenamiento de los archivos `redolog` y `flashback log` al directorio previamente creado.

```
SQL> ALTER SYSTEM SET DB_RECOVERY_FILE_DEST = '/home/oracle/bd2_grupo16' SCOPE=BOTH;
```

Comprobamos las modificaciones realizadas utilizando el comando:

```
SQL> SHOW PARAMETER db_recovery_file;
```

Obteniendo el siguiente resultado:
![](C:\Users\yyfm9\Desktop\Repositorios\BD2-P1G16\img\FRA-Enable.png)

---

## 

