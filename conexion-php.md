``` bash
<?php
try {
    $host = 'localhost';
    $dbname = 'mi_base_datos'; // Cambia esto si el nombre de tu base de datos es diferente
    $usuario = 'root';
    $contraseña = ''; // Contraseña vacía por defecto en XAMPP

    $pdo = new PDO("mysql:host=$host;dbname=$dbname", $usuario, $contraseña);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Error en la conexión: " . $e->getMessage());
}
?>
```
