---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 84cd8ed79281b005407b5a857398b5669635c072
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320535"
---
Si necesita configurar un proxy HTTP para realizar solicitudes de salida, use estos dos argumentos:

| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|--|--|
|HTTP_PROXY|string|El proxy que se va a utilizar, por ejemplo, `http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|Las credenciales necesarias para autenticarse en el proxy, por ejemplo, nombre de usuario:contraseña.|
|`<proxy-user>`|string|El usuario del proxy.|
|`<proxy-password>`|string|La contraseña asociada con `<proxy-user>` para el proxy.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
