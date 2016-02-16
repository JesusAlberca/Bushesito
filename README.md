# Bushesito
Android 5.1 API 21

Usuario: miguel		Usuario: jesus
Contraseña: 1234	Contraseña: 1234

La aplicación consta de 4 actividades, la primera (Login) básicamente se autentifica en
mi servidor, la segunda (eligeActividad) carga las lineas de autobuses Aucorsa, la 
tercera (mostrarTipos) recoge el número de linea de la segunda actividad y carga todas
las paradas asociadas a esa linea, y la cuarta (tiempoEspera) recoge el número de parada
de la tercera actividad y carga información del tiempo de espera, más un textView con el
nombre de la parada que al hacer click sobre ella, abre Google Maps con la localización
de esta parada, con un marcador. Controla todos los errores posibles que se dan en la
aplicación, si falla el servidor, si el usuario no es correcto, etc.. mostrando así un
mensaje personalizado y vibrando.

He utilizado un logo para la aplicación que estará disponible en todos los tamaños, además
también está disponible en Español (defecto) y Ingles. Añadí una imagen en todos los 
tamaños para la actividad (Login). Los layout estan disponible en vertical y horizontal.
El background de los layout tiene un color amigable y le agregue un tema para quitar 
el titulo de cada Actividad al ser ejecutada (android:theme="@android:style/Theme.NoTitleBar">)

Mi JSON es generado a traves de un hosting gratuito:
(Login) Utilizado en la actividad Login: http://jesusalberca.esy.es/php/acceso.php?usuario=miguel&password=1234

Los JSON que he utilizado los encontre analizando la página m.aucorsa.es, y son:
(Linea) Utilizado en la actividad eligeActividad: http://m.aucorsa.es/inc/json/lineas.json
(Parada) Utilizado en la actividad mostrarTipos: http://m.aucorsa.es/inc/json/paradas_50_3.json
(GeoLocalización) Utilizado en la actividad tiempoEspera: http://m.aucorsa.es/action.admin_operaciones.php?op=parada&numero=410
(Tiempo) Utilizado en la actividad tiempoEspera: http://m.aucorsa.es/action.admin_operaciones.php?op=tiempos&parada=410

___________________________________PHP UTILIZADO EN LOGIN________________________________
<?php
header('Content-Type: text/html; charset=utf-8');
$root = realpath($_SERVER["DOCUMENT_ROOT"]);
include "$root/php/db.php";

$usuario = $_REQUEST['usuario'];
$pass = $_REQUEST['password']; 
$key = sha1($pass);

// Consulta
$sql = "select count(*) 'Existe', usuario, nombre from usuarios where usuario='$usuario' and password = '$key'";
$datos=$conn->query($sql);
$json=array();
while($count= mysqli_fetch_assoc($datos)){
    //si devuelve 0 -> no existe en la BD
    //Si devuelve 1 -> si existe ocurrencia
    $json[]=$count;
}
	echo json_encode($json);
?> 
__________________________________________________________________________________________
