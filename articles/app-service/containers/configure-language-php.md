---
title: 'Configurar aplicaciones PHP: Azure App Service | Microsoft Docs'
description: Obtenga información sobre cómo configurar aplicaciones PHP para que funcione en Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 11d0648ee5090f02cb96c2d42a8d90cc3ea0ed28
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551343"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Configurar una aplicación PHP de Linux para Azure App Service

Esta guía muestra cómo configurar el tiempo de ejecución PHP integrado para aplicaciones web, back-ends móviles y aplicaciones de API en Azure App Service.

Esta guía proporciona los conceptos clave e instrucciones para desarrolladores PHP que usa un contenedor de Linux integrado en App Service. Si nunca ha usado Azure App Service, siga el [inicio rápido PHP](quickstart-php.md) y [PHP con MySQL tutorial](tutorial-php-mysql-app.md) primero.

## <a name="show-php-version"></a>Mostrar la versión PHP

Para mostrar la versión actual de PHP, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Para mostrar todas las versiones de PHP, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>Establecer la versión de PHP

Ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com) para establecer la versión de PHP en 7.2:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="run-composer"></a>Ejecute el Compositor de reglas

De forma predeterminada, no se ejecuta Kudu [Composer](https://getcomposer.org/). Para habilitar la automatización de Composer durante la implementación de Kudu, deberá suministrar un [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

Desde una ventana del terminal local, cambie el directorio a la raíz del repositorio. Siga el [pasos de instalación de línea de comandos](https://getcomposer.org/download/) descargar *composer.phar*.

Ejecute los comandos siguientes:

```bash
npm install kuduscript -g
kuduscript --php --scriptType bash --suppressPrompt
```

La raíz del repositorio tiene ahora dos nuevos archivos además *composer.phar*: *.deployment* y *deploy.sh*. Estos archivos funcionan tanto para las versiones de Windows y Linux de App Service.

Abra *deploy.sh* y busque el `Deployment` sección. Reemplace toda la sección con el código siguiente:

```bash
##################################################################################################################################
# Deployment
# ----------

echo PHP deployment

# 1. KuduSync
if [[ "$IN_PLACE_DEPLOYMENT" -ne "1" ]]; then
  "$KUDU_SYNC_CMD" -v 50 -f "$DEPLOYMENT_SOURCE" -t "$DEPLOYMENT_TARGET" -n "$NEXT_MANIFEST_PATH" -p "$PREVIOUS_MANIFEST_PATH" -i ".git;.hg;.deployment;deploy.sh"
  exitWithMessageOnError "Kudu Sync failed"
fi

# 3. Initialize Composer Config
initializeDeploymentConfig

# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
##################################################################################################################################
```

Confirme todos los cambios e implemente el código nuevo. Compositor de reglas ahora se ejecutará como parte de la automatización de la implementación.

## <a name="customize-start-up"></a>Personalización del inicio

De forma predeterminada, el contenedor PHP integrado, ejecute el servidor Apache. En el inicio, se ejecuta `apache2ctl -D FOREGROUND"`. Si lo desea, puede ejecutar un comando diferente en el inicio, ejecutando el siguiente comando en el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a>Acceso a variables de entorno

En App Service, puede [establecer la configuración de la aplicación](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) fuera del código de aplicación. Puede tener acceso a ellos mediante el estándar [getenv()](https://secure.php.net/manual/function.getenv.php) patrón. Por ejemplo, para acceder a una configuración de una aplicación denominada `DB_HOST`, use el código siguiente:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Raíz del sitio de cambio

El marco web de su elección puede usar un subdirectorio como la raíz del sitio. Por ejemplo, [Laravel](https://laravel.com/), usa el `public/` subdirectorio como la raíz del sitio.

La imagen PHP predeterminada para App Service utiliza Apache y no le permite personalizar la raíz del sitio para la aplicación. Para solucionar esta limitación, agregue un *.htaccess* archivo a la raíz del repositorio con el siguiente contenido:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^.*$ /public/$1 [NC,L,QSA]
</IfModule>
```

Si prefiere no usar la reescritura de *.htaccess*, puede implementar la aplicación Laravel con una [imagen personalizada de Docker](quickstart-docker-go.md) en su lugar.

## <a name="detect-https-session"></a>Detección de sesión de HTTPS

En App Service, la [terminación de SSL](https://wikipedia.org/wiki/TLS_termination_proxy) se produce en los equilibradores de carga de red, por lo que todas las solicitudes HTTPS llegan a su aplicación en forma de solicitudes HTTP sin cifrar. Si su aplicación lógica necesita comprobar si las solicitudes de usuario están cifradas, inspeccione el encabezado `X-Forwarded-Proto`.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Los marcos web más usados le permiten acceder a la información de `X-Forwarded-*` en el patrón de aplicación estándar. En [CodeIgniter](https://codeigniter.com/), [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) comprueba el valor de `X_FORWARDED_PROTO` de forma predeterminada.

## <a name="customize-phpini-settings"></a>Personalizar la configuración de php.ini

Si necesita realizar cambios en la instalación de PHP, puede cambiar cualquiera de los [directivas de php.ini](http://www.php.net/manual/ini.list.php) siguiendo estos pasos.

> [!NOTE]
> La mejor forma de ver la versión de PHP y la actual *php.ini* configuración consiste en llamar a [phpinfo()](https://php.net/manual/function.phpinfo.php) en la aplicación.
>

### <a name="customize-non-phpinisystem-directives"></a>Personalizar las directivas que no sean PHP_INI_SYSTEM

Personalizar directivas PHP_INI_USER, PHP_INI_PERDIR y PHP_INI_ALL (consulte [directivas de php.ini](http://www.php.net/manual/ini.list.php)), agregue un *.htaccess* archivo al directorio raíz de la aplicación.

En el *.htaccess* , agregue las directivas mediante el `php_value <directive-name> <value>` sintaxis. Por ejemplo: 

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Volver a implementar la aplicación con los cambios y reiniciarlo. Si implementa con Kudu (por ejemplo, mediante [Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)), se reinicia automáticamente después de la implementación.

Como alternativa al uso de *.htaccess*, puede usar [ini_set ()](http://www.php.net/manual/function.ini-set.php) en la aplicación para personalizar estas directivas no PHP_INI_SYSTEM.

### <a name="customize-phpinisystem-directives"></a>Personalizar directivas PHP_INI_SYSTEM

Para personalizar las directivas PHP_INI_SYSTEM (consulte [directivas de php.ini](http://www.php.net/manual/ini.list.php)), no puede usar el *.htaccess* enfoque. App Service proporciona un mecanismo diferente utilizando el `PHP_INI_SCAN_DIR` configuración de la aplicación.

En primer lugar, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com) para agregar una aplicación llamada `PHP_INI_SCAN_DIR`:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d` es el directorio predeterminado donde *php.ini* existe. `/home/site/ini` es el directorio personalizado en el que va a agregar un personalizado *.ini* archivo. Separe los valores con un `:`.

Vaya a la sesión SSH web con el contenedor de Linux (`https://cephalin-container.scm.azurewebsites.net/webssh/host`).

Cree un directorio en `/home/site` llamado `ini`, a continuación, cree un *.ini* de archivos en el `/home/site/ini` directorio (por ejemplo, *settings.ini)* con las directivas que desea personalizar. Utilice la misma sintaxis que usaría en un *php.ini* archivo. 

> [!TIP]
> En los contenedores de Linux integrados en App Service, */home* se usa como almacenamiento compartido persistente. 
>

Por ejemplo, para cambiar el valor de [expose_php](http://php.net/manual/ini.core.php#ini.expose-php) ejecute los siguientes comandos:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Para que surtan efecto los cambios, reinicie la aplicación.

## <a name="enable-php-extensions"></a>Habilitar extensiones PHP

Las instalaciones de PHP integradas contienen las extensiones más usadas. Puede habilitar extensiones adicionales en la misma forma que [personalizar directivas de php.ini](#customize-php_ini_system-directives).

> [!NOTE]
> La mejor forma de ver la versión de PHP y la actual *php.ini* configuración consiste en llamar a [phpinfo()](https://php.net/manual/function.phpinfo.php) en la aplicación.
>

Para habilitar extensiones adicionales, siga los pasos que se detallan a continuación:

Agregar un `bin` directorio al directorio raíz de la aplicación y put el `.so` archivos de extensión en ella (por ejemplo, *mongodb.so*). Asegúrese de que las extensiones son compatibles con la versión de PHP en Azure y son VC9 y no-seguras para subprocesos (nts).

Implementar los cambios.

Siga los pasos de [directivas personalizar PHP_INI_SYSTEM](#customize-php_ini_system-directives), agregar las extensiones en personalizado *.ini* de archivos con la [extensión](https://www.php.net/manual/ini.core.php#ini.extension) o [zend_extension ](https://www.php.net/manual/ini.core.php#ini.zend-extension) directivas.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Para que surtan efecto los cambios, reinicie la aplicación.

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Abrir sesión SSH en el explorador

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>solución de problemas

Cuando una aplicación PHP en funcionamiento se comporta de manera diferente en App Service o tiene errores, intente lo siguiente:

- [Acceso a la secuencia de registro](#access-diagnostic-logs).
- Probar la aplicación localmente en el modo de producción. App Service se ejecuta las aplicaciones de Node.js en el modo de producción, por lo que deberá asegurarse de que el proyecto funciona según lo previsto en modo de producción localmente. Por ejemplo: 
    - En función de su *composer.json*, distintos paquetes pueden instalarse para el modo de producción (`require` frente a `require-dev`).
    - Algunos marcos web pueden implementar los archivos estáticos de forma diferente en modo de producción.
    - Algunos marcos web pueden usar scripts de inicio personalizada cuando se ejecuta en modo de producción.
- Ejecute la aplicación en App Service en modo de depuración. Por ejemplo, en [Laravel](http://meanjs.org/), puede configurar la aplicación para generar mensajes de depuración en producción por [configuración la `APP_DEBUG` configuración de aplicación para `true` ](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="robots933456"></a>robots933456

Puede ver el mensaje siguiente en los registros del contenedor:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Puede ignorar este mensaje. `/robots933456.txt` es una ruta de acceso de dirección URL ficticia que App Service se usa para comprobar si el contenedor es capaz de atender las solicitudes. Una respuesta 404 simplemente indica que la ruta de acceso no existe, pero permite saber que el contenedor está en buen estado y listo para responder a las solicitudes de App Service.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación PHP con MySQL](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [Preguntas más frecuentes de App Service Linux](app-service-linux-faq.md)