---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 70548db9ef98cc8fa59ebc11c1371325e63e02fc
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740042"
---
En Cloud Shell, cree una [aplicación web](../articles/app-service/app-service-web-overview.md) en el plan de App Service `myAppServicePlan`. Puede hacerlo con el comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). En el siguiente ejemplo, reemplace *\<app_name>* por un nombre único global de aplicación (los caracteres válidos son `a-z`, `0-9` y `-`). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Cuando se ha creado la aplicación web, la CLI de Azure muestra información similar a la del ejemplo siguiente:

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

Ha creado una aplicación web vacía, con la implementación de Git habilitada.

> [!NOTE]
> La dirección URL del repositorio de Git remoto se muestra en la propiedad `deploymentLocalGitUrl`, con el formato `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Guarde esta dirección URL, ya que la necesitará más adelante.
>

Vaya a la aplicación web recién creada.

```
http://<app_name>.azurewebsites.net
```

Este es el aspecto que debería tener su nueva aplicación web:
