``` bash
/* --- Estilos generales --- */
body {
    font-family: 'Arial', sans-serif;
    background-color: #f0f2f5;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

/* --- Contenedor principal --- */
.container {
    background-color: #ffffff;
    padding: 40px 30px;
    border-radius: 12px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    text-align: center;
    width: 100%;
    max-width: 400px;
}

h1 {
    font-size: 2rem;
    color: #333333;
    margin-bottom: 20px;
    font-weight: bold;
}

/* --- Campos del formulario --- */
form {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

label {
    font-size: 14px;
    color: #555555;
    text-align: left;
}

/* --- Estilo adicional para el campo "Nombre" y otros campos de texto --- */
input[type="text"],
input[type="email"],
input[type="password"] {
    width: 100%;
    padding: 12px 15px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 16px;
    color: #333;
    transition: all 0.3s ease-in-out;
}

input[type="text"]:focus,
input[type="email"]:focus,
input[type="password"]:focus {
    border-color: #007bff;
    outline: none;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.3);
}

/* --- Botón de envío --- */
button {
    padding: 12px;
    background-color: #28a745;
    border: none;
    border-radius: 8px;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.2s ease;
}

button:hover {
    background-color: #218838;
    transform: translateY(-2px);
}

/* --- Enlace de registro --- */
a {
    font-size: 14px;
    color: #007bff;
    text-decoration: none;
    margin-top: 15px;
    display: block;
    transition: color 0.3s ease;
}

a:hover {
    color: #0056b3;
}

/* --- Diseño responsivo --- */
@media (max-width: 480px) {
    .container {
        padding: 30px 20px;
    }

    h1 {
        font-size: 1.8rem;
    }

    button {
        font-size: 14px;
    }
}

```
