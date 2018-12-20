## La magia y el infierno de Elastic Beanstalk

> Este artículo está en el horno. Verás cambios con el tiempo. Hay cosas erroneas o que estan en modo borrador.

Hace unos meses (mediados de 2018) en la empresa donde trabajo apareció la necesidad de disponer de un entorno "indestructible" para una aplicación de gestión que estamos desarrollando. En ese momento instalada directamente sobre hierro con los servicios necesarios para que funcionase a la antigua usanza.

- **AWS con Elastic Beanstalk.** A.K.A. EBS
- **Nuestro proyecto en Laravel.**

Presentados los contrincantes la diversión y el sufrimiento esta servido sobre la mesa del DevOp.

Voy a intentar escribir el sencillo pero doloroso proceso de aprender que os llevará a la victoria. Y no voy a pasar por explicaros cada paso punto por punto. Si no que me voy a centrar en los que no son tan obvios y que son infumables de encontrar en la documentación de AWS.

- .ebextensions
- .elasticbeanstalk
- HTTPS
- CORS
- Supervisor

Antes de empezar a meternos en comandos y gaitas. Voy a explicaros el *workflow* que he visto en el caso de como funcionan los despliegues de PHP + Apache en Beanstalk.

```
 +-------------------+             +---------------+
 |                   |             |               |
 |  /var/app/ondeck  +-------------> Mover carpeta |
 |                   |             |               |
 +-------------------+             +--------+------+
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX          |
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX          |
  +---------------+                +--------v-------+
  |               |                |                |
  | /var/www/html |                |/var/app/current|
  |               |                |                |
  +-------^-------+  +----------+  +--------+-------+
          |          | Enlace   |           |
          +----------+ simbólico| <---------+
                     +----------+
```

Bueno esto es a groso modo un poco lo que hace. No os penséis que esto es evidentemente todo. Pero os aclarará muchas dudas. La idea es que el staging se realiza en un directorio (Llaman a ***staging*** al momento en el que se prepara el código para su puesta en producción) específico. Aquí es donde deberéis realizar los cambios de permisos y demás cosillas que queráis hacer previa puesta en producción.

> Nota: Tengo la duda de si deberían lanzarse aquí las migraciones o no.

Una vez realizados los cambios el proceso pasa por mover la carpeta a su ubicación definitiva `/var/app/current`. Y ojo que no es un enlace simbólico como hacen otros sistemas de despliegue. Una vez ahí crea un enlace simbólico en `/var/www/html` con el contenido.

> Nota: No he podido comprobar aun como Apache sabe que tiene que ejecutar la carpeta ./public del proyecto de Laravel. Aunque sospecho que se configura directamente en EBS.

Entonces teniendo en mente esta forma de desplegar ya podemos realizar muchas operaciones con los `.ebextensions`. Que por si no lo sabéis es la manera que EBS se entera de lo que queréis hacer en las fases previas, posteriores y durante el despliegue. Es decir, si hay que cambiar permisos, ejecutar comandos, etc. Va aquí.

Intuyo que estan puestos para ejecutarse en orden alfabético. Es decir que por ejemplo tenemos una estructura:

- 01-myconfig-wapen.config
- 02-myconfig-wapen2.config
- 03-myconfig-wapen3.config

Va evidentemente en el orden previsto. Por cierto, los ficheros `.config` en realidad son ficheros `.yml` lo que pasa que algun genio de la lámpara prefirió ponerles `.config` porque quedaba mas ***cool***. Es decir, que si los abrís con un editor de YML os debería pintar bien la sintáxis.

Es importante ya que necesitaréis conocer esto para poder hacer la mágia que nos gusta a los desarrolladores.
