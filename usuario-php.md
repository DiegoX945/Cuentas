``` bash
<?php
class Usuario {
    private $id;
    private $nombre;
    private $clave;
    private $email;

    public function __construct($nombre, $clave, $email) {
        $this->nombre = $nombre;
        $this->clave = password_hash($clave, PASSWORD_BCRYPT);
        $this->email = $email;
    }

    // Método para establecer el ID
    public function setId($id) {
        $this->id = $id;
    }

    public function getId() { return $this->id; }
    public function getNombre() { return $this->nombre; }
    public function getClave() { return $this->clave; }
    public function getEmail() { return $this->email; }
}
?>
```
