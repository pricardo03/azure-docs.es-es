---
title: Implementación de Git local a Azure App Service
description: Aprenda a habilitar la implementación de Git local en Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: a614dadae40fcfc28eba85e5943f60a38653224b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233910"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implementación de Git local a Azure App Service

Esta guía muestra cómo implementar el código en [Azure App Service](app-service-web-overview.md) desde un repositorio Git en la máquina local.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>requisitos previos

Para seguir los pasos de esta guía:

* [Instale Git](http://www.git-scm.com/downloads).
* Mantenga un repositorio Git local con el código que desea implementar.

Para usar un repositorio de ejemplo para continuar, ejecute el comando siguiente en la ventana del terminal local:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git-with-kudu-builds"></a>Implementación desde GIT local con compilaciones de Kudu

La manera más fácil de habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu es utilizar Cloud Shell.

### <a name="create-a-deployment-user"></a>Creación de un usuario de implementación

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Habilitación de GIT local con Kudu

Para habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu, ejecute [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) en Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Para crear en su lugar una aplicación habilitada para Git, ejecute [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) en Cloud Shell con el parámetro `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

El comando `az webapp create` debe generar algo similar a la salida siguiente:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Implementación del proyecto

En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _\<url>_ con la dirección URL del Git remoto que obtuvo en [Habilitación de Git para la aplicación](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando se le pida una contraseña, asegúrese de escribir la que creó en [Configuración de un usuario de implementación](#configure-a-deployment-user), no la que usa para iniciar sesión en Azure Portal.

```bash
git push azure master
```

Es posible que vea la automatización específica para el entorno de tiempo de ejecución en la salida, como MSBuild para ASP.NET, `npm install` para Node.js y `pip install` para Python. 

Vaya a la aplicación para comprobar que se implementó el contenido.

## <a name="deploy-from-local-git-with-vsts-builds"></a>Implementación desde GIT local con compilaciones de VSTS

> [!NOTE]
> Para que App Service cree las definiciones de compilación y de versión necesarias de la cuenta de VSTS, la cuenta de Azure debe tener el rol de **propietario** en la suscripción a Azure.
>

Para habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu, vaya a la aplicación en [Azure Portal](https://portal.azure.com).

En el panel de navegación de la izquierda de la página de la aplicación, haga clic en **Centro de implementación** > **GIT local** > **Continuar**. 

![](media/app-service-deploy-local-git/portal-enable.png)

Haga clic en **Entrega continua con VSTS** > **Continuar**.

![](media/app-service-deploy-local-git/vsts-build-server.png)

En la página **Configurar**, configure una nueva cuenta VSTS o especifique una cuenta existente. Cuando haya terminado, haga clic en **Continuar**.

> [!NOTE]
> Si desea usar una cuenta de VSTS existente que no aparece, debe [vincular la cuenta de VSTS con su suscripción a Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

En la página **Probar**, elija si desea habilitar las pruebas de carga y, después, haga clic en **Continuar**.

En función del [plan de tarifa](/pricing/details/app-service/plans/) de App Service, también puede ver una página **Implementar en el almacenamiento provisional**. Elija si desea habilitar ranuras de implementación y después haga clic en **Continuar**.

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

La cuenta de VSTS tarda unos minutos en estar preparada. Cuando esté listo, copie la dirección URL del repositorio de GIT en el centro de implementación.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _\<url>_ por la dirección URL obtenida en el último paso.

```bash
git remote add vsts <url>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando se lo solicite el Administrador de credenciales de GIT, inicie sesión con su usuario de visualstudio.com. Para consultar métodos de autenticación adicionales, vea la [información general sobre la autenticación en VSTS](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Una vez finalizada la implementación, puede consultar el progreso de la compilación en `https://<vsts_account>.visualstudio.com/<project_name>/_build` y el progreso de la implementación en `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Vaya a la aplicación para comprobar que se implementó el contenido.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Solución de problemas de implementación de Kudu

Estos son los errores o problemas comunes al usar Git para publicar en una aplicación de App Service en Azure:

---
**Síntoma**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Causa**: este error puede producirse si la aplicación no está en funcionamiento.

**Resolución**: inicie la aplicación en Azure Portal. La implementación de GIT no está disponible cuando la aplicación web está detenida.

---
**Síntoma**: `Couldn't resolve host 'hostname'`

**Causa**: este error puede producirse si la información de dirección introducida al crear el repositorio remoto "azure" no era correcta.

**Resolución**: use el comando `git remote -v` para obtener un listado de todos los remotos, junto con la dirección URL asociada. Compruebe que la URL para el repositorio correcto "azure" es correcta. Si lo necesita, suprima y vuelva a crear este repositorio remoto utilizando la URL correcta.

---
**Síntoma**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Causa**: este error puede producirse si no se especifica una rama durante `git push` o si no ha establecido el valor `push.default` en `.gitconfig`.

**Resolución**: vuelva a ejecutar `git push` y especifique la rama principal. Por ejemplo: 

```bash
git push azure master
```

---
**Síntoma**: `src refspec [branchname] does not match any.`

**Causa**: este error puede producirse si intenta insertar una rama que no es la principal en el repositorio remoto "azure".

**Resolución**: vuelva a ejecutar `git push` y especifique la rama principal. Por ejemplo: 

```bash
git push azure master
```

---
**Síntoma**: `RPC failed; result=22, HTTP code = 5xx.`

**Causa**: este error puede producirse si se trata de insertar un repositorio Git de gran tamaño a través de HTTPS.

**Resolución**: cambie la configuración de git en la máquina local para aumentar el tamaño de postBuffer.

```bash
git config --global http.postBuffer 524288000
```

---
**Síntoma**: `Error - Changes committed to remote repository but your web app not updated.`

**Causa**: este error puede ocurrir si se implementa una aplicación Node.js con un archivo _package.json_ que especifica los módulos requeridos adicionales.

**Resolución**: los mensajes adicionales con "npm ERR!" deben registrarse antes de este error y pueden proporcionar contexto adicional sobre el error. A continuación se indican las causas conocidas de este error y el mensaje 'npm ERR!' correspondiente:

* **Archivo package.json con estructura incorrecta**: npm ERR! No se pudieron leer las dependencias.
* **Módulo nativo que no tiene una distribución binaria para Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Recursos adicionales

* [Documentación de Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Implementación continua en Azure App Service](app-service-continuous-deployment.md)
* [Ejemplo: creación de una aplicación web e implementación de código desde un repositorio local de Git (CLI de Azure)](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Ejemplo: creación de una aplicación web e implementación de código desde un repositorio local de Git (PowerShell)](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
