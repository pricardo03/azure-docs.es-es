---
title: 'Implementación continua: Azure App Service | Microsoft Docs'
description: Aprenda a habilitar la implementación continua en Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: fcb2c270b36d5efbe7b799787cf2a123b51bea5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765705"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Implementación continua en Azure App Service
En este artículo se muestra cómo configurar la implementación continua en [Azure App Service](overview.md). App Service permite la implementación continua desde BitBucket, GitHub y [Azure DevOps Services](https://www.visualstudio.com/team-services/) mediante la extracción de las actualizaciones más recientes del repositorio existente en alguno de estos servicios.

Para más información sobre cómo configurar una implementación continua manualmente desde un repositorio en la nube que no aparece en Azure Portal (como [GitLab](https://gitlab.com/)), consulte [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps) (Configuración de la implementación continua de forma manual).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Publique el repositorio preparado en alguno de los servicios compatibles. Para más información sobre cómo publicar el proyecto en estos servicios, vea [Crear un repositorio (GitHub)], [Crear un repositorio (BitBucket)] y la [introducción a Azure DevOps Services].

## <a name="deploy-continuously-from-github"></a>Implementación continua desde GitHub

Para habilitar la implementación continua con GitHub, vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **GitHub** > **Autorizar**. Siga las indicaciones de autorización. 

![](media/app-service-continuous-deployment/github-choose-source.png)

Solo necesita autorizarse una sola vez con GitHub. Si ya dispone de autorización, simplemente haga clic en **Continuar**. Puede cambiar la cuenta de GitHub autorizada si hace clic en **Cambiar cuenta**.

![](media/app-service-continuous-deployment/github-continue.png)

En la página **Proveedor de compilación**, elija el proveedor de compilación y haga clic en > **Continuar**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Opción 1: Usar el servidor de compilación de Kudu para App Service

En la página **Configurar**, seleccione la organización, el repositorio y la rama desde los que desea realizar la implementación continua. Cuando haya terminado, haga clic en **Continuar**.

Para implementar desde un repositorio en una organización de GitHub, vaya a GitHub y vaya a **configuración** > **aplicaciones** > **autorizado aplicaciones OAuth**. A continuación, haga clic en "Azure App Service".

![Configuración > aplicaciones > OAuth aplicaciones autorizadas > Azure App Service](media/app-service-continuous-deployment/github-settings-navigation.png)

En la página siguiente, conceda acceso de App Service a repositorios de su organización, haga clic en el botón "Concesión" en el lado derecho.

![Haga clic en "Autorizar" para conceder acceso de App Service en los repositorios de la organización](media/app-service-continuous-deployment/grant-access.png)

Su organización debería aparecer ahora en la lista de "Organización" en el **configurar** página del centro de implementación.

### <a name="option-2-use-azure-pipelines-preview"></a>Opción 2: Usar Azure Pipelines (versión preliminar)

> [!NOTE]
> Para que App Service cree las instancias necesarias de Azure Pipelines en la organización de Azure DevOps Services, la cuenta de Azure debe tener el rol de **propietario** en la suscripción a Azure.
>

En la página **Configurar**, en la sección **Código**, seleccione la organización, el repositorio y la rama desde los que desea realizar la implementación continua. Cuando haya terminado, haga clic en **Continuar**.

En la página **Configurar**, en la sección **Compilar**, cree una organización de Azure DevOps Services o especifique una organización existente. Cuando haya terminado, haga clic en **Continuar**.

> [!NOTE]
> Si desea usar una organización existente de Azure DevOps Services que no aparece, debe [vincular la organización de Azure DevOps Services a la suscripción de Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

En la página **Probar**, elija si desea habilitar las pruebas de carga y, después, haga clic en **Continuar**.

En función del [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/plans/) de App Service, también puede ver una página **Implementar en el almacenamiento provisional**. Elija si desea [habilitar ranuras de implementación](deploy-staging-slots.md) y después haga clic en **Continuar**.

### <a name="finish-configuration"></a>Finalización de la configuración

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

Cuando finalice la configuración, las nuevas confirmaciones en el repositorio seleccionado se implementan continuamente en la aplicación de App Service.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Implementación continua desde BitBucket

Para habilitar la implementación continua con BitBucket, vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **BitBucket** > **Autorizar**. Siga las indicaciones de autorización. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

Solo necesita autorizarse una sola vez con BitBucket. Si ya dispone de autorización, simplemente haga clic en **Continuar**. Puede cambiar la cuenta de BitBucket autorizada si hace clic en **Cambiar cuenta**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

En la página **Configurar**, seleccione el repositorio y la rama desde los que desea realizar la implementación continua. Cuando haya terminado, haga clic en **Continuar**.

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

Cuando finalice la configuración, las nuevas confirmaciones en el repositorio seleccionado se implementan continuamente en la aplicación de App Service.

## <a name="deploy-continuously-from-azure-repos-devops-services"></a>Implementación continua desde Azure Repos (DevOps Services)

Para habilitar la implementación continua con [Azure Repos](https://docs.microsoft.com/azure/devops/repos/index), vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **Azure Repos** > **Continuar**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

En la página **Proveedor de compilación**, elija el proveedor de compilación y haga clic en > **Continuar**.

> [!NOTE]
> Si desea usar una organización existente de Azure DevOps Services que no aparece, debe [vincular la organización de Azure DevOps Services a la suscripción de Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

### <a name="option-1-use-app-service-kudu-build-server"></a>Opción 1: Usar el servidor de compilación de Kudu para App Service

En la página **Configurar**, seleccione la organización de Azure DevOps Services, el proyecto, el repositorio y la rama desde los que desea realizar la implementación continua. Cuando haya terminado, haga clic en **Continuar**.

### <a name="option-2-use-azure-devops-services-continuous-delivery"></a>Opción 2: Uso de la entrega continua de Azure DevOps Services

> [!NOTE]
> Para que App Service cree las instancias necesarias de Azure Pipelines en la organización de Azure DevOps Services, la cuenta de Azure debe tener el rol de **propietario** en la suscripción a Azure.
>

En la página **Configurar**, en la sección **Programar**, seleccione la organización de Azure DevOps Services, el proyecto, el repositorio y la rama desde los que desea realizar la implementación continua. Cuando haya terminado, haga clic en **Continuar**.

En la página **Configurar**, en la sección **Compilar**, especifique el marco del lenguaje que debe usar Azure DevOps Services para ejecutar las tareas de compilación para el repositorio seleccionado. Cuando haya terminado, haga clic en **Continuar**.

En la página **Probar**, elija si desea habilitar las pruebas de carga y, después, haga clic en **Continuar**.

En función del [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/plans/) de App Service, también puede ver una página **Implementar en el almacenamiento provisional**. Elija si desea [habilitar ranuras de implementación](deploy-staging-slots.md) y después haga clic en **Continuar**. 

### <a name="finish-configuration"></a>Finalización de la configuración

En la página **Resumen**, verifique las opciones y haga clic en **Finalizar**.

Cuando finalice la configuración, las nuevas confirmaciones en el repositorio seleccionado se implementan continuamente en la aplicación de App Service.

## <a name="disable-continuous-deployment"></a>Deshabilitación de la implementación continua

Para deshabilitar la implementación continua, vaya a la página de la aplicación de App Service en [Azure Portal](https://portal.azure.com).

En el menú de la izquierda, haga clic en **Centro de implementación** > **GitHub**, **Azure DevOps Services** o **BitBucket** > **Desconectar**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Recursos adicionales

* [How to investigate common issues with continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Uso de PowerShell para Azure]
* [Documentación de Git]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Usar Azure para generar automáticamente una canalización de CI/CD para implementar una aplicación de ASP.NET 4](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[Azure DevOps portal]: https://azure.microsoft.com/services/devops/
[Installing Git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
[Uso de PowerShell para Azure]: /powershell/azureps-cmdlets-docs
[Documentación de Git]: https://git-scm.com/documentation

[Crear un repositorio (GitHub)]: https://help.github.com/articles/create-a-repo
[Crear un repositorio (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Introducción a Azure DevOps Services]: https://docs.microsoft.com/azure/devops/user-guide/devops-alm-overview
