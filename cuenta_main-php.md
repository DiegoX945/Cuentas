``` bash
<?php
session_start();
require 'conexion.php';  // Asegúrate de incluir la conexión
require 'usuario.php';    // Asegúrate de incluir la clase Usuario
require 'crud_usuario.php'; // Asegúrate de incluir CRUDUsuario
require 'CuentaAhorros.php'; // La clase CuentaAhorros que definimos arriba

// Verificamos si el usuario está logueado
if (!isset($_SESSION['usuario'])) {
    header("Location: index.php");
    exit;
}

$usuario = $_SESSION['usuario'];

// Crear una cuenta de ahorros para el usuario logueado
$cuenta = new CuentaAhorros($usuario['id']);

// Mostrar saldo actual
echo "Saldo actual de la cuenta: $" . $cuenta->obtenerSaldo() . "<br>";

// Depositar dinero en la cuenta
$cuenta->depositar(1000);  // Depositar $1000
echo "Saldo después de depósito: $" . $cuenta->obtenerSaldo() . "<br>";

// Retirar dinero de la cuenta
if ($cuenta->retirar(500)) {
    echo "Saldo después de retirar $500: $" . $cuenta->obtenerSaldo() . "<br>";
} else {
    echo "No se pudo realizar el retiro. Verifique el saldo.<br>";
}

// Transferir dinero a otra cuenta (simulamos que otra cuenta tiene ID 2)
$cuentaDestino = new CuentaAhorros(2, 500);  // Crear una cuenta con saldo inicial de $500
if ($cuenta->transferir(200, $cuentaDestino)) {
    echo "Saldo después de transferir $200 a la cuenta de destino: $" . $cuenta->obtenerSaldo() . "<br>";
    echo "Saldo de la cuenta de destino: $" . $cuentaDestino->obtenerSaldo() . "<br>";
} else {
    echo "No se pudo realizar la transferencia.<br>";
}
?>
```
