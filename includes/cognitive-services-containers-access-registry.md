---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 02beb6bdce096c35d8948cc8aefab6cc97be907c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063937"
---
## <a name="use-the-docker-cli-to-authenticate-the-private-container-registry"></a>Uso de la CLI de Docker para autenticar un registro de contenedor privado

Puede autenticarse con el registro de contenedor privado de contenedores de Cognitive Services, pero el método recomendado de la línea de comandos es mediante el uso de la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use el comando [`docker login`](https://docs.docker.com/engine/reference/commandline/login/), como se muestra en el ejemplo siguiente, para iniciar sesión en `containerpreview.azurecr.io`, el registro de contenedor privado de los contenedores de Cognitive Services. Reemplace *\<username\>* por el nombre de usuario y *\<password\>* por la contraseña proporcionada en las credenciales que recibió del equipo de Azure Cognitive Services.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Si ha protegido las credenciales en un archivo de texto, puede concatenar el contenido de dicho archivo de texto, mediante el comando `cat`, al comando `docker login`, tal como se muestra en el ejemplo siguiente. Reemplace *\<passwordFile\>* por la ruta de acceso y el nombre del archivo de texto que contiene la contraseña, y *\<username\>* por el nombre de usuario proporcionado en las credenciales.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
