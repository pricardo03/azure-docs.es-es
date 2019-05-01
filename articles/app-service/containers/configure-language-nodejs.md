---
title: 'Configurar aplicaciones de Node.js: Azure App Service | Microsoft Docs'
description: Obtenga información sobre cómo configurar aplicaciones de Node.js para que funcione en Azure App Service
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
ms.openlocfilehash: 43dc76e6d1e1ec2a6167f1d3e3cc7b8780f843db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850257"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Configurar una aplicación de Linux Node.js para Azure App Service

Aplicaciones de Node.js deben implementarse con todas las dependencias NPM necesarias. El motor de implementación de App Service (Kudu) se ejecuta automáticamente `npm install --production` automáticamente al implementar un [repositorio de Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), o un [paquete Zip](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) con procesos de compilación activados. Si implementa los archivos con [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), sin embargo, deberá cargar manualmente los paquetes necesarios.

Esta guía proporciona los conceptos clave e instrucciones para desarrolladores de Node.js que usa un contenedor de Linux integrado en App Service. Si nunca ha usado Azure App Service, siga el [inicio rápido de Node.js](quickstart-nodejs.md) y [Node.js con MongoDB tutorial](tutorial-nodejs-mongodb-app.md) primero.

## <a name="show-nodejs-version"></a>Mostrar la versión de Node.js

Para mostrar la versión actual de Node.js, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Para mostrar todas las versiones de Node.js compatibles, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Establezca la versión de Node.js

Para establecer la aplicación en un [admite la versión de Node.js](#show-nodejs-version), ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Esta configuración especifica la versión de Node.js que usa, tanto en tiempo de ejecución y durante la restauración de paquetes automatizadas en Kudu.

> [!NOTE]
> Debe establecer la versión de Node.js en el proyecto `package.json`. El motor de implementación se ejecuta en un contenedor independiente que contiene todas las versiones compatibles de Node.js.

## <a name="configure-nodejs-server"></a>Configurar el servidor de Node.js

Los contenedores de Node.js que vienen con [PM2](http://pm2.keymetrics.io/), un administrador de procesos de producción. Puede configurar su aplicación se inicia con PM2, con NPM o con un comando personalizado.

- [Ejecuta un comando personalizado](#run-custom-command)
- [Ejecute npm start](#run-npm-start)
- [Ejecutar con PM2](#run-with-pm2)

### <a name="run-custom-command"></a>Ejecuta un comando personalizado

App Service puede iniciar la aplicación mediante un comando personalizado, como un archivo ejecutable como *run.sh*. Por ejemplo, para ejecutar `npm run start:prod`, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Ejecute npm start

Para iniciar la aplicación mediante `npm start`, asegúrese de que un `start` script está en el *package.json* archivo. Por ejemplo: 

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Para utilizar una personalizada *package.json* en el proyecto, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>Ejecutar con PM2

El contenedor inicia automáticamente la aplicación con PM2 cuando uno de los archivos comunes de Node.js se encuentra en el proyecto:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Uno de los siguientes [PM2 archivos](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process.json* y *ecosystem.config.js*

También puede configurar un archivo de inicio personalizada con las siguientes extensiones:

- Un *.js* archivo
- Un [archivo PM2](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) con la extensión *.json*, *. config.js*, *.yaml*, o *.yml*

Para agregar un archivo de inicio personalizada, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Depuración remota

> [!NOTE]
> Depuración remota está actualmente en versión preliminar.

Puede depurar la aplicación de Node.js de manera remota en [Visual Studio Code](https://code.visualstudio.com/) si se configura para [ejecutar con PM2](#run-with-pm2), excepto cuando se ejecuta mediante un *. config.js, *.yml, o *.yaml*.

En la mayoría de los casos, no es necesaria para la aplicación de ninguna configuración adicional. Si la aplicación se ejecuta con un *process.json* archivo (predeterminado o personalizado), debe tener un `script` propiedad en la raíz JSON. Por ejemplo: 

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Para configurar Visual Studio Code para la depuración remota, instale el [extensión App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Siga las instrucciones que aparecen en la página de extensión e inicie sesión Azure en Visual Studio Code.

En el Explorador de Azure, busque la aplicación que desea depurar, haga clic en él y seleccione **iniciar la depuración remota**. Haga clic en **Sí** para habilitarla para la aplicación. App Service se inicia a un proxy de túnel y asocia al depurador. A continuación, puede realizar solicitudes a la aplicación y ver al depurador detenerse en los puntos de interrupción.

Una vez que finalice la depuración, detenga el depurador seleccionando **desconexión**. Cuando se le pida, debe hacer clic en **Sí** para deshabilitar la depuración remota. Para deshabilitar más adelante, haga clic en su aplicación en el Explorador de Azure y seleccione **deshabilitar la depuración remota**.

## <a name="access-environment-variables"></a>Acceso a variables de entorno

En App Service, puede [establecer la configuración de la aplicación](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) fuera del código de la aplicación. A continuación, puede acceder a ellos mediante el patrón estándar de Node.js. Por ejemplo, para acceder a una configuración de una aplicación denominada `NODE_ENV`, use el código siguiente:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Ejecute Bower y Grunt/Gulp

De forma predeterminada, se ejecuta Kudu `npm install --production` cuando reconoce se implementa una aplicación Node.js. Si la aplicación requiere alguna de las herramientas de automatización más populares, como Grunt, Bower o Gulp, deberá suministrar un [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) para ejecutarlo.

Para habilitar el repositorio ejecutar estas herramientas, deberá agregarlos a las dependencias de *package.json.* Por ejemplo: 

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Desde una ventana del terminal local, cambie el directorio a la raíz del repositorio y ejecute los siguientes comandos:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

La raíz del repositorio ahora tiene dos archivos adicionales: *.deployment* y *deploy.sh*.

Abra *deploy.sh* y busque el `Deployment` sección, que tiene el siguiente aspecto:

```bash
##################################################################################################################################
# Deployment
# ----------
```

En esta sección se termina con la ejecución `npm install --production`. Agregue la sección de código que necesita para ejecutar la herramienta requiere *al final* de la `Deployment` sección:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Vea un [ejemplo en el ejemplo de MEAN.js](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), donde el script de implementación también ejecuta un personalizado `npm install` comando.

### <a name="bower"></a>Bower

Este fragmento de código se ejecuta `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Este fragmento de código se ejecuta `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Este fragmento de código se ejecuta `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>Detección de sesión de HTTPS

En App Service, la [terminación de SSL](https://wikipedia.org/wiki/TLS_termination_proxy) se produce en los equilibradores de carga de red, por lo que todas las solicitudes HTTPS llegan a su aplicación en forma de solicitudes HTTP sin cifrar. Si su aplicación lógica necesita comprobar si las solicitudes de usuario están cifradas, inspeccione el encabezado `X-Forwarded-Proto`.

Los marcos web más usados le permiten acceder a la información de `X-Forwarded-*` en el patrón de aplicación estándar. En [Express](https://expressjs.com/), puede usar [confiar en los servidores proxy](http://expressjs.com/guide/behind-proxies.html). Por ejemplo: 

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Abrir sesión SSH en el explorador

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>solución de problemas

Cuando una aplicación de Node.js en funcionamiento se comporta de manera diferente en App Service o tiene errores, intente lo siguiente:

- [Acceso a la secuencia de registros](#access-diagnostic-logs).
- Probar la aplicación localmente en el modo de producción. App Service se ejecuta las aplicaciones de Node.js en el modo de producción, por lo que deberá asegurarse de que el proyecto funciona según lo previsto en modo de producción localmente. Por ejemplo: 
    - En función de su *package.json*, distintos paquetes pueden instalarse para el modo de producción (`dependencies` frente a `devDependencies`).
    - Algunos marcos web pueden implementar los archivos estáticos de forma diferente en modo de producción.
    - Algunos marcos web pueden usar scripts de inicio personalizada cuando se ejecuta en modo de producción.
- Ejecute la aplicación en App Service en el modo de desarrollo. Por ejemplo, en [MEAN.js](http://meanjs.org/), puede establecer la aplicación en modo de desarrollo en tiempo de ejecución [configuración la `NODE_ENV` configuración de la aplicación](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación Node.js con MongoDB](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [P+F sobre App Service en Linux](app-service-linux-faq.md)