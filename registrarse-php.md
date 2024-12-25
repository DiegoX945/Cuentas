``` bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Usuario</title>
    <link rel="stylesheet" href="belleza.css">
</head>
<body>
    <div class="container">
        <h1>Registro de Usuario</h1>
        <form action="controller_login.php" method="POST">
            <input type="hidden" name="action" value="register">
            
            <label for="nombre">Nombre:</label>
            <input type="text" name="nombre" id="nombre" required>
            
            <label for="email">Correo:</label>
            <input type="email" name="email" id="email" required>
            
            <label for="clave">Contraseña:</label>
            <input type="password" name="clave" id="clave" required>
            
            <button type="submit">Registrar</button>
        </form>
        <a href="index.php">Volver al inicio</a>
    </div>
</body>
</html>
```
