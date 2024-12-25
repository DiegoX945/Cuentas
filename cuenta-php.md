``` bash
<?php
session_start();
require 'conexion.php';  // Conexión a la base de datos
require 'usuario.php';    // Clase Usuario
require 'crud_usuario.php'; // CRUDUsuario
require 'cuenta_ahorros.php'; // Incluir la clase CuentaAhorros

// Verificar si el usuario está logueado
if (!isset($_SESSION['usuario'])) {
    header("Location: index.php");
    exit;
}

$usuario = $_SESSION['usuario'];

// Crear un objeto CuentaAhorros para el usuario logueado
$cuenta = new CuentaAhorros($usuario['id']);

// Mostrar el saldo actual de la cuenta
$saldo = $cuenta->obtenerSaldo();
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cuenta</title>
    <link rel="stylesheet" href="belleza.css">
</head>
<body>
    <div class="account-container">
        <h1>Bienvenido, <?php echo htmlspecialchars($usuario['nombre']); ?>!</h1>
        <p>Tu saldo actual es: $<?php echo number_format($saldo, 2); ?></p>
        
        <!-- Formulario para realizar un depósito -->
        <h2>Depositar dinero</h2>
        <form action="procesar_transaccion.php" method="POST">
            <input type="hidden" name="accion" value="depositar">
            <label for="deposito">Monto a depositar:</label>
            <input type="number" name="monto" id="deposito" required>
            <button type="submit">Depositar</button>
        </form>
        
        <!-- Formulario para realizar un retiro -->
        <h2>Retirar dinero</h2>
        <form action="procesar_transaccion.php" method="POST">
            <input type="hidden" name="accion" value="retirar">
            <label for="retiro">Monto a retirar:</label>
            <input type="number" name="monto" id="retiro" required>
            <button type="submit">Retirar</button>
        </form>

        <!-- Formulario para realizar una transferencia -->
        <h2>Transferir dinero</h2>
        <form action="procesar_transaccion.php" method="POST">
            <input type="hidden" name="accion" value="transferir">
            <label for="transferir">Monto a transferir:</label>
            <input type="number" name="monto" id="transferir" required>
            <label for="cuenta_destino">ID de la cuenta destino:</label>
            <input type="number" name="cuenta_destino" id="cuenta_destino" required>
            <button type="submit">Transferir</button>
        </form>
        
        <a href="logout.php" class="logout-button">Cerrar sesión</a>
    </div>
</body>
</html>
```
