``` bash
<?php
session_start();
require 'conexion.php';
require 'usuario.php';  // Asegúrate de incluir esta línea
require 'crud_usuario.php';

if (!isset($_SESSION['usuario'])) {
    header("Location: index.php");
    exit;
}

$usuario = $_SESSION['usuario'];

// Si se envían datos para actualizar
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $nuevo_nombre = $_POST['nombre'];
    $nuevo_email = $_POST['email'];
    
    // Crear un nuevo objeto Usuario con el ID y los datos nuevos
    // Mantén la misma contraseña, solo actualiza nombre y correo
    $usuarioActualizado = new Usuario($nuevo_nombre, $usuario['clave'], $nuevo_email);
    $usuarioActualizado->setId($usuario['id']); // Asegúrate de que el ID del usuario esté asignado
    
    // Llamar al método para actualizar el usuario en la base de datos
    if (CRUDUsuario::actualizarUsuario($pdo, $usuarioActualizado)) {
        echo "Usuario actualizado con éxito!";
        $_SESSION['usuario'] = [
            'id' => $usuarioActualizado->getId(),
            'nombre' => $usuarioActualizado->getNombre(),
            'email' => $usuarioActualizado->getEmail(),
            'clave' => $usuario['clave'] // Mantener la contraseña en la sesión
        ]; // Actualizar la sesión con los nuevos datos
    } else {
        echo "Hubo un problema al actualizar el usuario.";
    }
}
?>

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Actualizar Usuario</title>
    <link rel="stylesheet" href="actualizar.css">
</head>
<body>
    <div class="container">
        <h1>Actualizar Información</h1>
        <form method="POST">
            <label for="nombre">Nuevo Nombre:</label>
            <input type="text" name="nombre" value="<?php echo htmlspecialchars($usuario['nombre']); ?>" required>
            
            <label for="email">Nuevo Correo:</label>
            <input type="email" name="email" value="<?php echo htmlspecialchars($usuario['email']); ?>" required>
            
            <button type="submit">Actualizar</button>
        </form>
        <a href="index.php">Volver al inicio</a>
    </div>
</body>
</html>
```
