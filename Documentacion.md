# Practica 1

**Contenido**
Comandos Docker Oracle
Habilitar FRA
Full Backup en modo NOARCHIVELOG
Habilitar modo ARCHIVELOG

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

Entramos a Oracle utilizando el comando:

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

Se modifica el parámetro `DB_RECOVERY_FILE_DEST_SIZE`  y se específica el tamaño de área de recuperación rápida utilizando el comando:

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
![](img/FRA-Enable.png)

---

## 2. Creación de Full Backup en modo NoArchiveLog

Verificamos en que modo estamos trabajando con la instrucción:

```
SQL> SELECT LOG_MODE FROM V$DATABASE;
```

Si la base de datos no se encuentra en modo `noarchivelog` lo habilitamos con los siguientes comandos:

```
SQL>SHUTDOWN IMMEDIATE;
SQL>STARTUP MOUNT;
SQL>ALTER DATABASE NOARCHIVELOG;
SQL>ALTER DATABASE OPEN;
```

Una vez hemos cambiado al modo `noarchivelog` detenemos la base de datos nuevamente:

```
SQL> SHUTDOWN IMMEDIATE;
SQL> !
```

Ingresamos al directorio:

```bash
$ cd /u01/app/oracle/oradata/ORCL18
```

Se crea el directorio en el cual se almacenará el backup:

```
$ mkdir /home/oracle/fullbackup
```

Copiamos todos los archivos que se encuentran en el directorio `/u01/app/oracle/oradata/ORCL18` al que hemos creado en el paso anterior:

```
$ cp -R * /home/oracle/fullbackup/
```

Comprobamos que los archivos hayan sido copiados al directorio del backup, con el siguiente resultado:

<img src="img/fullbackup-noarchivelog.png" style="zoom:50%;" />

---

## 3. Habilitar el modo ArchiveLog

Para habilitar el modo Archivelog ejecutamos los siguientes comandos:

```
SQL>SHUTDOWN IMMEDIATE;
SQL>STARTUP MOUNT;
SQL>ALTER DATABASE ARCHIVELOG;
SQL>ALTER DATABASE OPEN;
```

Comprobamos en que modo esta trabajando la base de datos:

```
$ SELECT LOG_MODE FROM V$DATABASE;
```

Obtenemos el siguiente resultado:
![](img/database-mode.png)

---

