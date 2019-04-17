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
ms.openlocfilehash: cd7fc7487a41979f37c9a55baeb0b8e172e808c4
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59587159"
---
En Azure Cloud Shell, configure las credenciales de implementación con el comando [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Se requiere este usuario de implementación para la implementación de FTP y Git local en una aplicación web. El nombre de usuario y la contraseña predeterminados están en el nivel de la cuenta. _No son los mismos que los de las credenciales de suscripción de Azure._

En el siguiente ejemplo, reemplace *\<username>* y *\<password>* (corchetes angulares incluidos) por un nuevo nombre de usuario y contraseña. El nombre de usuario debe ser único en Azure. La contraseña debe tener al menos ocho caracteres y dos de los tres elementos siguientes: letras, números y símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Se obtiene una salida de JSON y la contraseña mostrada como `null`. Si se produce un error `'Conflict'. Details: 409`, cambie el nombre de usuario. Si se produce un error `'Bad Request'. Details: 400`, use una contraseña más segura. El nombre de usuario de la implementación no debe contener el símbolo ' @' en las inserciones de Git locales.

Este usuario de implementación se configura una sola vez. Puede usarlo para todas las implementaciones de Azure.

> [!NOTE]
> Grabe el nombre de usuario y la contraseña. Los va a necesitar para implementar la aplicación web más adelante.
>
>
