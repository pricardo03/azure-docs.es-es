---
title: 'Creación de una aplicación web de Node.js: Azure App Service'
description: Cómo implementar una aplicación Node.js en Azure App Service
author: msangapu
ms.author: msangapu
ms.date: 08/12/2019
ms.topic: quickstart
ms.service: app-service
ms.devlang: javascript
ms.openlocfilehash: 8d679a95cc89c2ae7774b7f7b51b9d0aadd89d12
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390973"
---
# <a name="create-a-nodejs-app-in-azure"></a>Creación de una aplicación de Node.js en Azure

Azure App Service proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial rápido se explica cómo se implementa una aplicación de Node.js en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una cuenta de Azure, [regístrese hoy mismo](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) para obtener una cuenta gratuita con 200 USD en créditos de Azure para probar cualquier combinación de los servicios.

Necesita tener instalado [Visual Studio Code](https://code.visualstudio.com/), además de [Node.js y npm](https://nodejs.org/en/download), el administrador de paquetes de Node.js.

También tendrá que instalar la [extensión Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice), que puede usar para crear, administrar e implementar Azure App Service en Linux en la plataforma como servicio de Azure (PaaS).

### <a name="sign-in"></a>Iniciar sesión

Después de instalar la extensión, inicie sesión en su cuenta de Azure. En la barra de actividades, haga clic en el logotipo de Azure para mostrar el explorador de **AZURE APP SERVICE**. Haga clic en **Iniciar sesión en Azure…** y siga las instrucciones.

![inicio de sesión en Azure](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>solución de problemas

Si ve el error **“No se encuentra la suscripción con el nombre [identificador de suscripción]”** , puede que sea debido a que esté detrás de un proxy y no se pueda establecer la conexión con la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con su información de proxy en el terminal mediante `export`.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Si el problema no se corrige después de configurar las variables de entorno, haga clic en el botón siguiente **Tengo un problema** para ponerse en contacto con nosotros.

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

Antes de continuar, asegúrese de haber instalado y configurado todos los requisitos previos.

En VS Code, verá su dirección de correo electrónico de Azure en la barra de estado, y la suscripción en el explorador de **AZURE APP SERVICE**.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Creación de una aplicación de Node.js

Después, cree una aplicación de Node.js que se pueda implementar en la nube. En este inicio rápido, se usa un generador de aplicaciones para aplicar scaffolding rápidamente en la aplicación desde un terminal.

> [!TIP]
> Si ya ha completado el [tutorial de Node.js](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial), puede ir directamente al paso [Implementación del sitio web](#deploy-the-website).

### <a name="install-the-express-generator"></a>Instalación de Express Generator

[Express](https://www.expressjs.com) es un marco popular para crear y ejecutar aplicaciones de Node.js. Puede aplicar scaffolding en una nueva aplicación de Express (lo que equivale a crear un aplicación) mediante la herramienta [Express Generator](https://expressjs.com/en/starter/generator.html). Express Generator se proporciona como un módulo de npm y se instala mediante la herramienta de línea de comandos de npm `npm`.

```bash
npm install -g express-generator
```

El modificador `-g` instala Express Generator globalmente en el equipo para que pueda ejecutarlo desde cualquier lugar.

### <a name="scaffold-a-new-application"></a>Aplicación de scaffolding en una nueva aplicación

Después, aplique scaffolding en una nueva aplicación de Express denominada `myExpressApp`; para hacerlo, ejecute lo siguiente:

```bash
express myExpressApp --view pug --git
```

Los parámetros `--view pug --git` indican al generador que use el motor de plantillas [pug](https://pugjs.org/api/getting-started.html) (antes conocido como `jade`) y que cree un archivo de `.gitignore`.

Para instalar todas las dependencias de la aplicación, vaya a la nueva carpeta y ejecute `npm install`.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Ejecución de la aplicación

A continuación, asegúrese de que se ejecute la aplicación. Desde el terminal, inicie la aplicación con el comando `npm start` para iniciar el servidor.

```bash
npm start
```

Ahora, abra el explorador y vaya a [http://localhost:3000](http://localhost:3000), donde verá algo similar a esto:

![Ejecución de una aplicación de Express](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-website"></a>Implementación del sitio web

En esta sección, implementará un sitio web de Node.js con VS Code y la extensión Azure App Service. En este inicio rápido, se usa el modelo de implementación más básico, donde la aplicación se comprime e implementa en una instancia de Azure App Service en Linux.

### <a name="deploy-using-azure-app-service"></a>Implementación mediante Azure App Service

Primero, abra la carpeta de la aplicación en VS Code.

```bash
code .
```

En el explorador de **AZURE APP SERVICE**, haga clic en el icono de flecha arriba azul para implementar la aplicación en Azure.

![Implementación en Web App](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> También puede implementar desde la **Paleta de comandos** (CTRL + MAYÚS + P); para hacerlo, escriba “implementar en Web App” y ejecute el comando **Azure App Service: implementar en Web App**.

1. Seleccione el directorio que tenga abierto actualmente, `myExpressApp`.

2. Seleccione la opción **Crear Web App**.

3. Escriba un nombre único global para la Web App y presione ENTRAR. Los caracteres válidos para un nombre de aplicación son “a-z”, “0-9” y “-”.

4. Seleccione la **versión de Node.js** (le recomendamos que use “LTS”).

    En el canal de notificación, se muestran los recursos de Azure que se estén creando para la aplicación.

Haga clic en **Sí** cuando se le pida que actualice la configuración para ejecutar `npm install` en el servidor de destino. Después, se implementará la aplicación.

![Implementación configurada](./media/quickstart-nodejs/server-build.png)

Cuando se inicie la aplicación, se le pedirá que actualice el área de trabajo para que las implementaciones posteriores usen como destino automáticamente la misma instancia de App Service Web App. Seleccione **Sí** para asegurarse de que los cambios se implementen en la aplicación correcta.

![Implementación configurada](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Asegúrese de que la aplicación escuche en el puerto proporcionado por la variable de entorno PORT: `process.env.PORT`.

### <a name="browse-the-website"></a>Exploración del sitio web

Cuando se complete la implementación, haga clic en **Examinar sitio web** en el mensaje para ver el sitio web que acaba de implementar.

### <a name="troubleshooting"></a>solución de problemas

Si ve el error **“No tiene permiso para ver este directorio o esta página”** , puede que la aplicación no se haya iniciado correctamente. Vaya a la sección siguiente y vea la salida de registros para identificar y corregir el error. Si no puede corregirlo, haga clic en el botón siguiente **Tengo un problema** para ponerse en contacto con nosotros. Estaremos encantados de ayudarle.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="updating-the-website"></a>Actualización del sitio web

Para implementar los cambios en esta aplicación, siga el mismo proceso y seleccione la aplicación existente, en lugar de crear otra.

## <a name="viewing-logs"></a>Visualización de los registros

En esta sección, obtendrá información sobre cómo ver (o “copiar”) los registros desde el sitio web en ejecución. Las llamadas realizadas a `console.log` en el sitio se mostrarán en la ventana de salida en Visual Studio Code.

Busque la aplicación en el explorador de **AZURE APP SERVICE**, haga clic con el botón derecho en la aplicación y seleccione **Ver registros de streaming**.

Cuando se le pida, seleccione la opción para habilitar el registro y reinicie la aplicación. Después de reiniciar la aplicación, se abrirá la ventana de salida de VS Code con una conexión al streaming de registros.

![Visualización de los registros de streaming](./media/quickstart-nodejs/view-logs.png)

![Activación de los registros y reinicio](./media/quickstart-nodejs/enable-restart.png)

Después de unos segundos, verá un mensaje donde se indica que está conectado al servicio de streaming de registros.

```bash
Connecting to log-streaming service...
2017-12-21 17:33:51.428 INFO  - Container practical-mustache_2 for site practical-mustache initialized successfully.
2017-12-21 17:33:56.500 INFO  - Container logs
```

Actualice la página varias veces en el explorador para ver la salida de registros.

```bash
2017-12-21 17:35:17.774 INFO  - Container logs
2017-12-21T17:35:14.955412230Z GET / 304 141.798 ms - -
2017-12-21T17:35:15.248930479Z GET /stylesheets/style.css 304 3.180 ms - -
2017-12-21T17:35:15.378623115Z GET /favicon.ico 404 53.839 ms - 995
```

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Pasos siguientes

Ha completado correctamente este inicio rápido.

A continuación, vea otras extensiones de Azure.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Herramientas de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

También puede conseguirlas todas si instala el paquete de extensiones [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).
