``` bash
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Operación Bancaria</title>
    <!-- Conectar el archivo CSS -->
    <link rel="stylesheet" href="respuesta.css">
</head>
<body>

<?php
session_start();
require 'conexion.php';
require 'usuario.php';
require 'crud_usuario.php';
require 'cuenta_ahorros.php'; // Asegúrate de incluir la clase CuentaAhorros

if (!isset($_SESSION['usuario'])) {
    header("Location: index.php");
    exit;
}

$usuario = $_SESSION['usuario'];
$cuenta = new CuentaAhorros($usuario['id']);  // Crear la cuenta del usuario actual

// Procesar la acción del formulario
$accion = $_POST['accion'] ?? null;
$monto = $_POST['monto'] ?? 0;
$cuenta_destino_id = $_POST['cuenta_destino'] ?? null;

if ($accion === 'depositar') {
    // Validar monto
    if ($monto > 0) {
        // Verifica que el saldo actual antes de realizar el depósito sea correcto
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-info'>
                    Saldo antes del depósito: $" . $cuenta->obtenerSaldo() . "<br>
                </div>
              </div>";

        if ($cuenta->depositar($monto)) {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-exito'>
                        Depósito exitoso! Nuevo saldo: $" . $cuenta->obtenerSaldo() . "
                    </div>
                  </div>";
        } else {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-error'>
                        Error al realizar el depósito.
                    </div>
                  </div>";
        }
    } else {
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-error'>
                    El monto debe ser mayor que 0.
                </div>
              </div>";
    }
} elseif ($accion === 'retirar') {
    // Validar monto
    if ($monto > 0) {
        if ($cuenta->retirar($monto)) {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-exito'>
                        Retiro exitoso! Nuevo saldo: $" . $cuenta->obtenerSaldo() . "
                    </div>
                  </div>";
        } else {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-error'>
                        Error al realizar el retiro o saldo insuficiente.
                    </div>
                  </div>";
        }
    } else {
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-error'>
                    El monto debe ser mayor que 0.
                </div>
              </div>";
    }
} elseif ($accion === 'transferir') {
    // Validar monto y cuenta destino
    if ($monto > 0 && $cuenta_destino_id) {
        $cuenta_destino = new CuentaAhorros($cuenta_destino_id);  // Crear la cuenta destino

        if ($cuenta->transferir($monto, $cuenta_destino)) {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-exito'>
                        Transferencia exitosa! Nuevo saldo de la cuenta de origen: $" . $cuenta->obtenerSaldo() . "<br>
                        Saldo de la cuenta destino: $" . $cuenta_destino->obtenerSaldo() . "
                    </div>
                  </div>";
        } else {
            echo "<div class='operacion-container'>
                    <div class='operacion-mensaje mensaje-error'>
                        Error al realizar la transferencia o saldo insuficiente.
                    </div>
                  </div>";
        }
    } else {
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-error'>
                    El monto debe ser mayor que 0 y la cuenta destino debe ser válida.
                </div>
              </div>";
    }
} elseif ($accion === 'eliminar') {
    // Si la acción es eliminar, eliminamos el usuario y la cuenta de ahorros
    $id_usuario = $usuario['id'];

    if (CRUDUsuario::eliminarUsuario($pdo, $id_usuario)) {
        // Eliminación exitosa, destruimos la sesión y redirigimos
        session_destroy();
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-exito'>
                    Cuenta eliminada exitosamente.
                </div>
              </div>";
        header("Location: index.php");  // Redirigir a la página de inicio después de eliminar
        exit;
    } else {
        echo "<div class='operacion-container'>
                <div class='operacion-mensaje mensaje-error'>
                    Error al eliminar la cuenta.
                </div>
              </div>";
    }
} else {
    echo "<div class='operacion-container'>
            <div class='operacion-mensaje mensaje-info'>
                Acción no reconocida.
            </div>
          </div>";
}
?>

</body>
</html>
```
