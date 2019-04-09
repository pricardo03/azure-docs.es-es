---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 710bb8cba7fbbe4bc9b9fdc52b0767c96f97fe72
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631010"
---
## <a name="create-base-resources"></a>Creación de recursos de base

Antes de configurar el enrutamiento de mensajes, deberá crear una instancia de IoT Hub, una cuenta de almacenamiento y una cola de Service Bus. Estos recursos pueden crearse mediante uno de los cuatro artículos que están disponibles para la parte 1 de este tutorial: la CLI de Azure, Azure PowerShell, Azure Portal o una plantilla de Azure Resource Manager.

Use el mismo grupo de recursos y la misma ubicación para todos los recursos. Al final, puede quitar todos los elementos en un solo paso mediante la eliminación del grupo de recursos.

En las secciones siguientes se describen los pasos que se deben realizar.

1. Cree un [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md).

2. Creación de una instancia de IoT Hub en el nivel S1. Agregue un grupo de consumidores a la instancia de IoT Hub. El grupo de consumidores lo utiliza Azure Stream Analytics al recuperar datos.

   > [!NOTE]
   > Debe usar un centro de Iot en un nivel de pago para realizar este tutorial. El nivel gratis solo le permite configurar un punto de conexión, y en este tutorial se necesitan varios.
   > 

3. Cree una cuenta de almacenamiento V1 estándar con replicación Standard_LRS.

4. Cree un espacio de nombres de Service Bus y una cola.

5. Cree una identidad de dispositivo para el dispositivo simulado que envíe mensajes al centro. Guarde la clave para la fase de pruebas. (Si crea una plantilla de Resource Manager, esto se realizará después de implementar la plantilla).