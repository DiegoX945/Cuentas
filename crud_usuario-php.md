``` bash
<?php
class CRUDUsuario {
    // Función para insertar un usuario
    public static function insertarUsuario($pdo, $usuario) {
        $stmt = $pdo->prepare("INSERT INTO usuarios (nombre, email, clave) VALUES (?, ?, ?)");
        return $stmt->execute([$usuario->getNombre(), $usuario->getEmail(), $usuario->getClave()]);
    }

    // Función para buscar un usuario por email
    public static function buscarPorEmail($pdo, $email) {
        $stmt = $pdo->prepare("SELECT * FROM usuarios WHERE email = ?");
        $stmt->execute([$email]);
        return $stmt->fetch(PDO::FETCH_ASSOC);
    }

    // Función para actualizar un usuario
    public static function actualizarUsuario($pdo, $usuario) {
        // Actualizar solo nombre y correo, no la contraseña
        $stmt = $pdo->prepare("UPDATE usuarios SET nombre = ?, email = ? WHERE id = ?");
        return $stmt->execute([$usuario->getNombre(), $usuario->getEmail(), $usuario->getId()]);
    }

    // Función para eliminar un usuario y su cuenta de ahorros
    public static function eliminarUsuario($pdo, $id_usuario) {
        // Primero, eliminamos la cuenta de ahorros
        $cuenta = new CuentaAhorros($id_usuario);

        // Eliminar la cuenta de ahorros
        if ($cuenta->eliminarCuenta()) {
            // Ahora eliminamos el usuario de la base de datos
            $stmt = $pdo->prepare("DELETE FROM usuarios WHERE id = ?");
            $stmt->execute([$id_usuario]);

            // Verificar si la eliminación fue exitosa
            if ($stmt->rowCount() > 0) {
                return true; // El usuario y su cuenta de ahorros fueron eliminados
            }
        }

        return false; // Algo salió mal
    }
}
?>
```
