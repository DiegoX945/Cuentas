``` bash
<?php
class CuentaAhorros {
    private $id;
    private $saldo;

    public function __construct($id, $saldoInicial = 0) {
        $this->id = $id;
        $this->saldo = $saldoInicial;
    }

    // Método para obtener el saldo
    public function obtenerSaldo() {
        global $pdo;
        $stmt = $pdo->prepare("SELECT saldo FROM cuentas_ahorros WHERE id_usuario = ?");
        $stmt->execute([$this->id]);
        $resultado = $stmt->fetch(PDO::FETCH_ASSOC);

        // Verificar si hay saldo o devolver 0 si no existe
        if ($resultado) {
            return $resultado['saldo'];
        } else {
            // Si no se encuentra el usuario en la tabla, retornamos 0 como saldo inicial
            return 0;
        }
    }

    // Método para depositar dinero
    public function depositar($monto) {
        if ($monto > 0) {
            // Actualizamos el saldo en la base de datos
            global $pdo;
            $stmt = $pdo->prepare("UPDATE cuentas_ahorros SET saldo = saldo + ? WHERE id_usuario = ?");
            $stmt->execute([$monto, $this->id]);

            // Verificar si la actualización fue exitosa
            if ($stmt->rowCount() > 0) {
                // Después de realizar el depósito, actualizamos el saldo en el objeto
                $this->saldo = $this->obtenerSaldo();
                return true;
            }
        }
        return false;
    }

    // Método para retirar dinero
    public function retirar($monto) {
        if ($monto > 0) {
            // Verificar el saldo antes de proceder con el retiro
            $saldo_actual = $this->obtenerSaldo();

            // Asegurarse de que hay saldo suficiente
            if ($saldo_actual >= $monto) {
                $this->saldo = $saldo_actual - $monto;
                global $pdo;
                $stmt = $pdo->prepare("UPDATE cuentas_ahorros SET saldo = ? WHERE id_usuario = ?");
                $stmt->execute([$this->saldo, $this->id]);

                return true;
            }
        }
        return false;
    }

    // Método para transferir dinero a otra cuenta
    public function transferir($monto, $cuentaDestino) {
        if ($this->retirar($monto)) {
            return $cuentaDestino->depositar($monto);
        }
        return false;
    }

    // Método para crear una cuenta de ahorros
    public function crearCuentaAhorros($pdo) {
        // Insertamos una nueva cuenta de ahorros con saldo inicial de 0 para el usuario
        $stmt = $pdo->prepare("INSERT INTO cuentas_ahorros (id_usuario, saldo) VALUES (?, ?)");
        $stmt->execute([$this->id, 0]); // Saldo inicial de 0
    }

    // Método para obtener el ID de la cuenta
    public function getId() {
        return $this->id;
    }

    // Método para eliminar la cuenta de ahorros del usuario
    public function eliminarCuenta() {
        global $pdo;
        // Eliminar la cuenta de ahorros asociada al usuario
        $stmt = $pdo->prepare("DELETE FROM cuentas_ahorros WHERE id_usuario = ?");
        return $stmt->execute([$this->id]);
    }
}
?>
```
