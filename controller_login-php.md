``` bash
<?php
session_start();
require 'conexion.php';
require 'usuario.php';
require 'crud_usuario.php';
require 'cuenta_ahorros.php'; // Asegúrate de incluir la clase CuentaAhorros

$action = $_POST['action'] ?? null;

if ($action === 'register') {
    $nombre = $_POST['nombre'];
    $email = $_POST['email'];
    $clave = $_POST['clave'];

    // Crear un nuevo objeto de Usuario
    $usuario = new Usuario($nombre, $clave, $email);

    // Insertar el usuario en la base de datos
    $insertado = CRUDUsuario::insertarUsuario($pdo, $usuario);

    if ($insertado) {
        // Obtener el id del usuario insertado (último ID insertado)
        $id_usuario = $pdo->lastInsertId();

        // Crear la cuenta de ahorros para el usuario
        $cuentaAhorros = new CuentaAhorros($id_usuario);
        $cuentaAhorros->crearCuentaAhorros($pdo); // Crear cuenta con saldo inicial 0

        // Redirigir a la página de inicio de sesión o cuenta
        header("Location: index.php");
        exit;
    } else {
        echo "Error al registrar el usuario.";
    }
} elseif ($action === 'login') {
    $email = $_POST['email'];
    $clave = $_POST['clave'];

    // Buscar al usuario por su correo electrónico
    $usuario = CRUDUsuario::buscarPorEmail($pdo, $email);

    if ($usuario && password_verify($clave, $usuario['clave'])) {
        // Almacenar los datos del usuario en la sesión
        $_SESSION['usuario'] = $usuario;
        header("Location: cuenta.php");
        exit;
    } else {
        echo "Usuario o contraseña incorrectos.";
    }
}
?>
```
