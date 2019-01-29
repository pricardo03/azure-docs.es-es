---
title: 'Creación de aplicación web Ruby en Linux: Azure App Service | Microsoft Docs'
description: Aprenda a crear una aplicación de Ruby on Rails con App Service en Linux.
keywords: azure app service, linux, oss, ruby, rails
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: cfowler
editor: ''
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: cfowler
ms.custom: seodec18
ms.openlocfilehash: 3ddf98abf0c52d3bc1fb0a6ce43755d5adcf87fa
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848638"
---
# <a name="create-a-ruby-on-rails-app-in-app-service-on-linux"></a>Crear una aplicación de Ruby on Rails en App Service en Linux

[Azure App Service en Linux](app-service-linux-intro.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En esta guía de inicio rápido se muestra cómo crear una aplicación de [Ruby on Rails](https://rubyonrails.org/) básica y, posteriormente, implementarla en Azure como Azure Web App on Linux.

> [!NOTE]
> La pila de desarrollo de Ruby solo admite Ruby on Rails de momento. Si quiere usar otra plataforma, como Sinatra, o una [versión de Ruby no admitida](app-service-linux-intro.md), consulte el inicio rápido de [Web App for Containers](https://docs.microsoft.com/azure/app-service/containers/).

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Instalación de Ruby 2.3 o superior</a>
* <a href="https://git-scm.com/" target="_blank">Instalación de Git</a>

## <a name="download-the-sample"></a>Descarga del ejemplo

En una ventana de terminal, ejecute el siguiente comando para clonar el repositorio de la aplicación de ejemplo en el equipo local:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Ejecución de la aplicación de forma local

Ejecute la aplicación localmente para ver cómo debería ser si se implementara en Azure. Abra una ventana de terminal, cambie al directorio `hello-world` y use el comando `rails server` para iniciar el servidor.

El primer paso es instalar los archivos gem necesarios. Hay un `Gemfile` incluido en el ejemplo, por lo que no es necesario especificar ninguno para instalarlo. Para esto se usa un software que instala varios programas:

```
bundle install
```

Una vez que se han instalado los archivos gem, se usa un software que instala varios programas para iniciar la aplicación:

```bash
bundle exec rails server
```

Mediante el explorador web, vaya a `http://localhost:3000` para probar la aplicación localmente.

![Hello World configurada](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Creación de una aplicación web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

Vaya al sitio para ver la aplicación web recién creada con una imagen integrada. Reemplace _&lt;app name>_ por el nombre de la aplicación web.

```bash
http://<app_name>.azurewebsites.net
```

Este es el aspecto que debería tener su nueva aplicación web:

![Página de presentación](./media/quickstart-ruby/splash-page.png)

## <a name="deploy-your-application"></a>Implementación de aplicación

Ejecute los comandos siguientes para implementar la aplicación local en el sitio web de Azure:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Confirme que las operaciones de implementación remota se han realizado correctamente. Los comandos generan una salida similar al texto siguiente:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Una vez que la implementación haya finalizado, reinicie la aplicación web para que la implementación surta efecto mediante el comando [`az webapp restart`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-restart), como se muestra aquí:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Vaya al sitio y verifique los resultados.

```bash
http://<app name>.azurewebsites.net
```

![Aplicación web actualizada](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Mientras se reinicia la aplicación, al tratar de obtener acceso al sitio, se obtendrá un código de estado HTTP `Error 503 Server unavailable`. Puede tardar unos minutos en reiniciarse completamente.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ruby on Rails con MySQL](tutorial-ruby-postgres-app.md)
