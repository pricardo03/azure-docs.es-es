---
title: Implementación desde el repositorio de Git local en Azure App Service
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
ms.date: 06/14/2019
ms.author: dariagrigoriu;cephalin
ms.custom: seodec18
ms.openlocfilehash: e66c625c3f30580715762d2dd3f48eeaa6e548dc
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67143951"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implementación de Git local a Azure App Service

Esta guía muestra cómo implementar el código en [Azure App Service](overview.md) desde un repositorio Git en la máquina local.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos de esta guía:

* [Instale Git](https://www.git-scm.com/downloads).
* Mantenga un repositorio Git local con el código que desea implementar.

Para usar un repositorio de ejemplo para continuar, ejecute el comando siguiente en la ventana del terminal local:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-builds"></a>Implementación con compilaciones de Kudu

La manera más fácil de habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu es utilizar Cloud Shell.

### <a name="configure-a-deployment-user"></a>Configuración de un usuario de implementación

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

> [!NOTE]
> En lugar de credenciales de nivel de cuenta, también puede implementar con credenciales de nivel de aplicación, que se generan automáticamente para cada aplicación.
>

### <a name="enable-local-git-with-kudu"></a>Habilitación de GIT local con Kudu

Para habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu, ejecute [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) en Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Para crear en su lugar una aplicación habilitada para Git, ejecute [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) en Cloud Shell con el parámetro `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

### <a name="deploy-your-project"></a>Implementación del proyecto

En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _\<username>_ con el usuario de implementación en [Configurar un usuario de implementación](#configure-a-deployment-user) y _\<app-name>_ con el nombre de la aplicación en [Habilitar Git en la aplicación](#enable-local-git-with-kudu).

```bash
git remote add azure https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git
```

> [!NOTE]
> Como alternativa, para implementar con credenciales de nivel de aplicación, obtenga las credenciales específicas de la aplicación ejecutando el siguiente comando en Cloud Shell:
>
> ```azurecli-interactive
> az webapp deployment list-publishing-credentials -n <app-name> -g <group-name> --query scmUri --output tsv
> ```
>
> Luego, use el resultado del comando para ejecutar `git remote add azure <url>`, tal como se ha hecho anteriormente.

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando se le pida una contraseña, asegúrese de escribir la que creó en [Configuración de un usuario de implementación](#configure-a-deployment-user), no la que usa para iniciar sesión en Azure Portal.

```bash
git push azure master
```

Es posible que vea la automatización específica para el entorno de tiempo de ejecución en la salida, como MSBuild para ASP.NET, `npm install` para Node.js y `pip install` para Python. 

Vaya a la aplicación para comprobar que se implementó el contenido.

## <a name="deploy-with-azure-devops-builds"></a>Implementación con compilaciones de Azure DevOps

> [!NOTE]
> Para que App Service cree las instancias necesarias de Azure Pipelines en la organización de Azure DevOps Services, la cuenta de Azure debe tener el rol de **propietario** en la suscripción a Azure.
>

Para habilitar la implementación de GIT local para la aplicación con el servidor de compilación Kudu, vaya a la aplicación en [Azure Portal](https://portal.azure.com).

En el panel de navegación de la izquierda de la página de la aplicación, haga clic en **Centro de implementación** > **GIT local** > **Continuar**.

![](media/app-service-deploy-local-git/portal-enable.png)

Haga clic en **Azure Pipelines (versión preliminar)**  > **Continuar**.

![](media/app-service-deploy-local-git/pipeline-builds.png)

En la página **Configurar**, cree una organización de Azure DevOps o especifique una organización existente. Cuando haya terminado, haga clic en **Continuar**.

> [!NOTE]
> Si quiere usar una organización existente de Azure DevOps que no aparece, debe [vincular la organización de Azure DevOps Services a la suscripción de Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

En función del [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/plans/) de App Service, también puede ver una página **Implementar en el almacenamiento provisional**. Elija si desea habilitar ranuras de implementación y después haga clic en **Continuar**.

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

La organización de Azure DevOps Services tarda unos minutos en estar preparada. Cuando esté listo, copie la dirección URL del repositorio de GIT en el centro de implementación.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _\<url>_ por la dirección URL obtenida en el último paso.

```bash
git remote add vsts <url>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando se lo solicite el Administrador de credenciales de GIT, inicie sesión con su usuario de visualstudio.com. Para métodos de autenticación adicionales, vea la [información general sobre autenticación de Azure DevOps Services](/vsts/git/auth-overview?view=vsts).

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
* [Implementación continua en Azure App Service](deploy-continuous-deployment.md)
* [Ejemplo: Creación de una aplicación web e implementación de código desde un repositorio local de Git (CLI de Azure)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Ejemplo: Creación de una aplicación web e implementación de código desde un repositorio local de Git (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
