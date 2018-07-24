---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: wesmc7777
ms.service: iot-edge
ms.topic: include
ms.date: 06/26/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 657006360105ac26091c54fe2e0deb523d1e6dea
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008508"
---
Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar.

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

> [!IMPORTANT]
> La eliminación de los recursos de Azure y de los grupos de recursos es un proceso irreversible. Cuando se eliminan estos elementos, el grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado el centro de IoT en un grupo de recursos ya existente que tiene recursos que desea conservar, elimine solo el recurso del centro de IoT en sí en lugar de eliminar todo el grupo de recursos.
>

Para eliminar solo el centro de IoT, ejecute el comando siguiente. Reemplace \<YourIoTHub > con el nombre del centro de IoT y \<TestResources > con el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name <YourIoTHub> --resource-group <TestResources>
```


Para eliminar un grupo de recursos entero por el nombre:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Grupos de recursos**.

2. En el cuadro de texto **Filtrar por nombre**, escriba el nombre del grupo de recursos que contiene el centro de IoT. 

3. A la derecha del grupo de recursos, en la lista de resultados, seleccione los puntos suspensivos (**...**) y, después, **Eliminar grupo de recursos**.

    ![Eliminación de un grupo de recursos](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)

4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba de nuevo el nombre del grupo de recursos para confirmar y seleccione **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.






