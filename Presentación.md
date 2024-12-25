## Proyecto en PHP

Este proyecto es un sistema de gestión de cuentas bancarias donde los usuarios pueden realizar diversas operaciones sobre su cuenta de ahorros. A continuación, te detallo los cambios realizados y las nuevas funcionalidades implementadas en el sistema:

## 1. Funcionalidad General
El sistema permite a los usuarios realizar las siguientes operaciones en su cuenta de ahorros:

a)	Depositar dinero

b)	Retirar dinero

c)	Transferir dinero a otra cuenta

Además, el sistema gestiona la creación, actualización y eliminación de cuentas de usuario y sus correspondientes cuentas de ahorros.

## 2. Cambios Implementados

### 2.1. Clase cuenta_ahorros.php
La clase cuenta_ahorros.php se encarga de representar una cuenta de ahorros vinculada a un usuario. En ella se agregaron las siguientes funcionalidades:

**Método obtenerSaldo():** Obtiene el saldo actual de la cuenta de ahorros asociada al usuario.

**Método depositar($monto):** Permite realizar un depósito en la cuenta de ahorros. Si el monto es válido, el saldo se actualiza en la base de datos.

**Método retirar($monto):** Permite retirar dinero de la cuenta de ahorros. Verifica que el saldo sea suficiente antes de realizar la operación.

**Método transferir($monto, $cuentaDestino):** Permite transferir dinero de la cuenta de ahorros del usuario a otra cuenta de ahorros, restando el monto de la cuenta de origen y sumándolo a la cuenta de destino.

**Método crearCuentaAhorros($pdo):** Crea una nueva cuenta de ahorros con saldo inicial de 0 para un usuario nuevo en la base de datos.

**Método eliminarCuenta():** Nueva funcionalidad agregada para eliminar la cuenta de ahorros del usuario desde la base de datos cuando el usuario decide eliminar su cuenta.

### 2.2. Clase crud_usuario

La clase CRUDUsuario maneja las operaciones de creación, actualización y eliminación de usuarios. La principal modificación fue la incorporación de un método adicional:

**Método eliminarUsuario($pdo, $id):** Elimina al usuario de la base de datos. Se utiliza cuando el usuario decide eliminar su cuenta completamente del sistema.

### 2.3. Archivo eliminar.php

El archivo **eliminar.php** se encargó de gestionar la eliminación tanto de la cuenta de ahorros como de la cuenta de usuario:

Se implementó un formulario que solicita al usuario confirmar si desea eliminar su cuenta.

Flujo de eliminación: 

Cuando el usuario confirma la eliminación, se llama al método eliminarCuenta() de la clase CuentaAhorros para eliminar su cuenta de ahorros.

Después de eliminar la cuenta de ahorros, se procede a eliminar al usuario con el método eliminarUsuario() de la clase CRUDUsuario.
    Finalmente, se destruye la sesión del usuario, y el sistema lo redirige al inicio.
    
### 2.4. Archivo procesar_transacciones.php

El archivo procesar_transacciones.php maneja las operaciones bancarias básicas, como depositar, retirar o transferir dinero entre cuentas:

**Depósitos:** Permite a los usuarios depositar dinero en su cuenta de ahorros.

**Retiros:** Permite a los usuarios retirar dinero de su cuenta de ahorros, siempre y cuando haya saldo suficiente.

**Transferencias:** Permite a los usuarios transferir dinero a otra cuenta de ahorros dentro del sistema.

### 2.5. Flujo de Eliminación de Usuario y Cuenta

Cuando un usuario decide eliminar su cuenta, el flujo es el siguiente:

1.	El usuario accede a la página eliminar.php, donde puede confirmar su deseo de eliminar la cuenta.
2.	Al confirmar, se ejecuta el método eliminarCuenta() de la clase CuentaAhorros, lo que elimina la cuenta de ahorros vinculada al usuario.
3.	Luego, se elimina el usuario de la base de datos mediante el método eliminarUsuario() de la clase CRUDUsuario.
4.	Finalmente, la sesión del usuario es destruida, y se redirige al inicio del sistema.

## 3. Seguridad y Validaciones

**Validaciones en los formularios:** Se validan los montos antes de permitir depósitos, retiros y transferencias, asegurando que el monto sea mayor que cero y que haya saldo suficiente en caso de un retiro.

**Autenticación:** Si el usuario no está logueado, el sistema lo redirige a la página de inicio para que inicie sesión antes de realizar cualquier acción.

**Seguridad en la eliminación:** Se asegura de que el usuario no pueda eliminar su cuenta sin una confirmación explícita.

## 4. Estructura de la Base de Datos

El sistema utiliza varias tablas para almacenar los datos de usuarios y sus cuentas de ahorros:

**Tabla usuarios:** Contiene los datos del usuario, como su nombre, correo electrónico y clave.

**Tabla cuentas_ahorros:** Contiene el saldo y la relación con el usuario mediante el campo id_usuario.

## 5. Conclusión

Con estos cambios, el sistema ahora soporta:

Operaciones bancarias básicas como depósitos, retiros y transferencias.

Eliminación completa de la cuenta de usuario y su cuenta de ahorros asociada.

Validaciones y seguridad para garantizar que solo los usuarios autenticados puedan realizar operaciones y que los montos sean válidos antes de cualquier operación bancaria.

Este proyecto ahora proporciona una funcionalidad completa para manejar cuentas bancarias de forma sencilla y segura.
