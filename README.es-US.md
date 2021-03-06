[English](./README.md) | Español


# Saforpres
El sistema para el registro de contabilidad presupuestaria, es un proyecto realizado en PHP usando la patrón de arquitectura de software MVC que servirá de base para aquella persona que quiera aprender la metodología. Para el manejo de la información se uso PL/SQL en MySQL para un mejor control de los datos. Se usaron también el  conjunto de bibliotecas ADOdb para brindar mas portabilidad, rapidez y facilidad en las conexiones. El sistema también maneja librerías de Email y de PDF para la generación de reportes.


Todo esta incluido y listo para usar, espero sea de utilidad.


## Vision General :mag:
Menú principal
![](https://raw.githubusercontent.com/delfinworks/saforpres/master/images/saforpre1.jpg)

Módulo de Carga 
![](https://raw.githubusercontent.com/delfinworks/saforpres/master/images/saforpre2.jpg)

## Requerimiento :white_check_mark:
- Web Server Apache-2.2.15
- PHP 5.2.13
- MySQL 5.1.46
- phpMyAdmin 3.3.3 

## PHP :eyes:
```bash
function ListProyecto($eje){				
	include_once(PATH.'/gui/ObjetoListBox.class.php');
	$lb = new ListBoxObj();
	$lb->setquery("SELECT safor_pry.id_pry, safor_pry.descripcion FROM safor_pry WHERE (safor_pry.id_eje=".$eje.") ORDER BY safor_pry.id_pry");
	$lb->setnombre_listbox('txtpry_id');
	$lb->setvalor_inicial(array('0',''));
	$lb->setajax_event('onchange');
	$lb->setajax_div('prys');
	$lb->setajax_file_root(PATH."/modulos/formulacion/plan.funciones.php");
	$lb->setajax_class_name("plan_funciones");
	$lb->setajax_parametro_function(0);
	$lb->setajax_function_on_event('Pry_Filtra');
	$lb->GENERA_LISTBOX(0,'',TRUE);				
}
```

## PL/SQL :eyes:
```bash
  CREATE DEFINER=`root`@`localhost` PROCEDURE `borrar_ai` (`v_id` INT, `v_eje` INT, `v_users` VARCHAR(15), `v_ip` VARCHAR(20))  BEGIN
        DECLARE    v_mensaje varchar(150);
        DECLARE    v_valor bool;

         IF NOT EXISTS(SELECT id_ae FROM safor_pry_ae_ai_eje_um WHERE id_ai=v_id AND id_eje=v_eje) THEN
                DELETE FROM safor_ai  WHERE id_ai=v_id AND id_eje=v_eje;
                SET v_mensaje='La accion intermedia se elimino exitosamente. ';
                SET v_valor=true;

                INSERT INTO seniat_users_log_plan (seniat_users_id, seniat_users_ip, id_ai, accion, id_eje)
                        VALUES (v_users, v_ip, v_id, 'ELIMINACION (AI)', v_eje);
        ELSE
                SET v_mensaje='La accion intermedia no puede ser eliminada, existen unidades de medida asociados a ella';
                SET v_valor=false;
        END IF;
        
        SELECT  v_id as id, v_mensaje as mensaje, v_valor as valor;
   END$$
```

## Configuración :gear:

****************************************************************************************
Los archivos de configuración de la aplicación se encuentran en el directorio "includes".
****************************************************************************************
```bash
/* Constantes de base de datos "configuracion_db.php" */
define('DB_TYPE','mysql');//manejador de base de datos
define('DB_SERVIDOR', '127.0.0.1'); //Dirección IP del servidor de base de datos
define('DB_SERVIDOR_PUERTO', '3306'); // Puerto de conexión de base de datos
define('DB_SERVIDOR_USERNAME', 'user'); // Usuario de conexión de base de datos
define('DB_SERVIDOR_PASSWORD', 'password); // Password de conexión de base de datos
define('DB_DATABASE', ' saforpre'); //Nombre de la base de datos
define('DB_CONEXION_P', false);  // Usar conexiones persistentes?
define('DEBUG_ADODB', false); // Opción para que la Clase ADODB muestre los errores arrojados
```
```bash
/* Constantes de rutas del sistema "configuracion.php" */
define('DOCUMENT_ROOT',$_SERVER['DOCUMENT_ROOT']);
define('PATH',$_SERVER['DOCUMENT_ROOT']. '/saforpre''); // 'Coloca aquí la ruta donde se encuentra el sistema a partir del directorio raíz
```

Montar la base datos db/saforpre.sql

Listo!


## Compatibilidad :triangular_ruler:

Exploradores modernos y IE11.
