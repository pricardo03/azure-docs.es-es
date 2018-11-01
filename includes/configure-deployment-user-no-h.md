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
ms.openlocfilehash: b5512ed5810ebd05596dc3ca7a29957b4c595c59
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035148"
---
En Cloud Shell, cree credenciales de implementación con el comando [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Se requiere este usuario de implementación para la implementación de FTP y Git local en una aplicación web. El nombre de usuario y la contraseña predeterminados están en el nivel de la cuenta. _Son diferentes de las credenciales de suscripción de Azure._

En el siguiente ejemplo, reemplace *\<username>* y *\<password>* (corchetes angulares incluidos) por un nuevo nombre de usuario y contraseña. El nombre de usuario debe ser único en Azure. La contraseña debe tener al menos ocho caracteres, con dos de los tres elementos siguientes: letras, números y símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Debe obtener una salida de JSON, con la contraseña mostrada como `null`. Si se produce un error `'Conflict'. Details: 409`, cambie el nombre de usuario. Si se produce un error ` 'Bad Request'. Details: 400`, use una contraseña más segura.

Solo es necesario crear este usuario de implementación una vez, y puede usarlo en todas las implementaciones de Azure.

> [!NOTE]
> Registre el nombre de usuario y la contraseña. Los va a necesitar para implementar la aplicación web más adelante.
>
>
