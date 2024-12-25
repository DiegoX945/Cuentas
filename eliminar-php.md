``` bash
<?php
session_start();
require 'conexion.php';
require 'usuario.php';  // Asegúrate de incluir esta línea
require 'crud_usuario.php';
require 'cuenta_ahorros.php';  // Incluir la clase CuentaAhorros

if (!isset($_SESSION['usuario'])) {
    // Si el usuario no está logueado, redirige al inicio
    header("Location: index.php");
    exit;
}

$usuario = $_SESSION['usuario'];
$cuenta = new CuentaAhorros($usuario['id']);  // Crear la cuenta asociada al usuario

// Si se confirma la eliminación
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Llamamos al método de CRUD para eliminar al usuario
    // Primero eliminamos la cuenta de ahorros
    if ($cuenta->eliminarCuenta()) {
        // Ahora eliminamos el usuario
        if (CRUDUsuario::eliminarUsuario($pdo, $usuario['id'])) {
            // Eliminamos la sesión del usuario después de que se haya eliminado de la base de datos
            session_destroy();
            echo "Tu cuenta y cuenta de ahorros han sido eliminadas con éxito.";
            header("Location: index.php");  // Redirigimos al inicio
            exit;
        } else {
            echo "Hubo un problema al eliminar tu cuenta. Intenta nuevamente.";
        }
    } else {
        echo "Hubo un problema al eliminar tu cuenta de ahorros. Intenta nuevamente.";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eliminar Cuenta</title>
    <link rel="stylesheet" href="belleza.css">
</head>
<body>
    <div class="container">
        <h1>¿Estás seguro que deseas eliminar tu cuenta?</h1>
        <form method="POST">
            <button type="submit" class="delete-btn">Eliminar mi cuenta</button>
        </form>
        <a href="cuenta.php">Cancelar</a>
    </div>
</body>
</html>
```
