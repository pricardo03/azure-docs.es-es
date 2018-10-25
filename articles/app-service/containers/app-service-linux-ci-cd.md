---
title: Implementación continua desde un registro de contenedor de Docker con Web App for Containers - Azure | Microsoft Docs
description: Describe cómo configurar una implementación continua desde un registro de contenedor de Docker en Web App for Containers.
keywords: azure app service, linux, docker, acr,oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: msangapu
ms.openlocfilehash: 20ca63b7126a6800538129115ff339308c11d8c5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867038"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Implementación continua con Web App for Containers

En este tutorial, configurará una implementación continua para una imagen de contenedor personalizada desde los repositorios administrados de [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) o [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="enable-the-continuous-deployment-feature"></a>Habilitación de la característica de implementación continua

Habilite la característica de implementación continua mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) y la ejecución del siguiente comando:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

En [Azure Portal](https://portal.azure.com/), seleccione la opción **App Service** en el lado izquierdo de la página.

Seleccione el nombre de la aplicación para la que quiere configurar una implementación continua de Docker Hub.

En la página **Configuración del contenedor**, seleccione **Activado** y, luego, seleccione **Guardar** para habilitar la implementación continua.

![Captura de pantalla de la configuración de la aplicación](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Preparación de la dirección URL del webhook

Obtenga la dirección URL del webhook mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) y la ejecución del siguiente comando:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Anote la dirección URL del webhook. La necesitará en la siguiente sección.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Puede obtener `publishingusername` y `publishingpwd` descargando el perfil de publicación de la aplicación web mediante Azure Portal.

![Captura de pantalla de la adición del webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Adición de un webhook

Para agregar un webhook, siga los pasos descritos en estas guías:

- [Azure Container Registry](../../container-registry/container-registry-webhook.md) con una dirección URL de webhook
- [Webhooks para Docker Hub](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure App Service en Linux](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Creación de una aplicación web de .NET Core en App Service en Linux](quickstart-dotnetcore.md)
* [Creación de una aplicación web de Ruby en App Service en Linux](quickstart-ruby.md)
* [Implementación de una aplicación web de Docker o Go en Web App for Containers](quickstart-docker-go.md)
* [Peguntas más frecuentes sobre Azure App Service en Linux](./app-service-linux-faq.md)
* [Administración de Web App for Containers mediante la CLI de Azure](./app-service-linux-cli.md)
