``` bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inicio de Sesión</title>
    <link rel="stylesheet" href="belleza.css">
</head>
<body>
    <div class="container">
        <h1>Inicio de Sesión</h1>
        <form action="controller_login.php" method="POST">
            <input type="hidden" name="action" value="login">
            <label for="email">Correo:</label>
            <input type="email" name="email" id="email" required>
            <label for="clave">Contraseña:</label>
            <input type="password" name="clave" id="clave" required>
            <button type="submit">Ingresar</button>
        </form>
        <a href="registrarse.php">¿No tienes una cuenta? Regístrate</a>
    </div>
</body>
</html>
```
