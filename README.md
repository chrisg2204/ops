# THANOS: 172.30.1.122

Almacena integraciones entre otras cosas. Los procesos de las integraciones se ejecutan por medio de **forever**, dentro de cada una podemos ver su respectivo `run.sh`. Integraciones como TimWE y Terra están en este server.

Existe otro directorio donde podemos ver integraciones realizadas con PHP, sera comentado más abajo.

###### integraciones
> thanos:/var/node

> thanos:/var/nginx

-----

## PIXEL NOTIFICATION - THANOS

###### ubicación

> thanos:/var/script/pixelNotification

###### logs
> thanos:/var/script/pixelNotification/{env}/logs

###### repositorio
[pixel-notification](https://github.com/opratel-core/tool-pixel-notification)

Este proceso se encarga de levantar los pixeles de la tabla **Opratelinfo.dba.Presuscripcion** y notificarlos a el api [google-ads-offline-conversions](https://github.com/opratel-core/google-ads-offline-conversions/tree/v16)

###### sumar nuevo operador

Para sumar un nuevo operador necesitamos buscar dentro del archivo de configuración, el network al que se va a listar, luego añadir los varlores **process-SponsorId**, **conversionMap** y **valueaMap**. 

En caso de que necesitemos emular una prueba, se necesita insertar un registro a las tablas **Opratelinfo.dba.Presuscripcion** donde tenga relacionado el pixel, red y operador con una fecha de proceso futura ya que el SP **OpratelInfo.dbo.sp_Select_Presuscripcion_Usuarios_Suscriptos** que levanta los registros también toma en cuenta ese dato. Es necesario también que el registro exista en **Opratelinfo.dba.Suscripcion** y **Opratelinfo.dba.SuscripcionIncremental** ya que el SP ejecuta un join.

Terminado el proceso podremos revisar en los logs las operaciones llevadas a cabo y ver si el pixel pudo ser procesado de una manera correcta.

-----

## TULANDIA API - THANOS

###### ubicación

> thanos:/var/www/html/tulandia/api

###### logs
> thanos:/var/www/html/tulandia/api/logs

###### repositorio
[tulandia-api](https://github.com/opratel-core/platform-tulandia-api)

Esta API escrita en PHP se encarga del servicio web Tulandia, acá esta toda la lógica que tenga que ver con tulandia.

Normalmente se revisa más que todo logs para ver si esta funcionando correctamente; por otro lado si requiere un cambio si necesita aprobación.

-----

# HULK (PROD): 172.30.1.36 

Almacena portales de wordpress entre otras cosas. Los portales operan con `ci/cd` sobre las ramas `develop`, `qa` y `prod` (se actualiza el entorno al subir las ramas mencionadas), esto quiere decir que para hacer un pase es necesario tener usuario y acceso a [jenkins](http://jenkins.opratel.com/).

## TIMWE ME HE - HULK

###### ubicación

> hulk:/data/docker/prod/generic

###### logs
> hulk:/data/docker/prod/generic/www/logs

###### repositorio
[generic-scripts](https://github.com/Opratel-Core/generic-scripts)

Este proceso se encarga de proveer el servicio de HE para TimWE ME. En dado caso que se requiera sumar un operador nuevo, el operador nos debe compartir `username`, `password`, `psk` e `iv`, luego tenemos que añadir un nuevo registro a la configuración.

Para testear si el cambio aplicado esta andando, solo tenemos que completar la siguiente url con los datos `http://hulk.opratel.com/he-timwe-me/?o={operador}&callback={url}`, esto debería ejecutar el proceso para obtener HE y redireccionarnos a la url callback compartida.







