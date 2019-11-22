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
ms.openlocfilehash: c97c8231265cf87f52333a56d21d6fb13180c554
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72808860"
---
## <a name="create-base-resources"></a>Creación de recursos de base

Antes de configurar el enrutamiento de mensajes, deberá crear una instancia de IoT Hub, una cuenta de almacenamiento y una cola de Service Bus. Estos recursos pueden crearse mediante uno de los cuatro artículos que están disponibles para la parte 1 de este tutorial: Azure Portal, una plantilla de Azure Resource Manager, la CLI de Azure o Azure PowerShell.

Use el mismo grupo de recursos y la misma ubicación para todos los recursos. Al final, puede quitar todos los elementos en un solo paso mediante la eliminación del grupo de recursos.

A continuación se muestra un resumen de los pasos que se deben realizar en las secciones siguientes: 

1. Cree un [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md).

2. Creación de una instancia de IoT Hub en el nivel S1. Agregue un grupo de consumidores a la instancia de IoT Hub. El grupo de consumidores lo utiliza Azure Stream Analytics al recuperar datos.

   > [!NOTE]
   > Debe usar un centro de Iot en un nivel de pago para realizar este tutorial. El nivel gratis solo le permite configurar un punto de conexión, y en este tutorial se necesitan varios.
   > 

3. Cree una cuenta de almacenamiento V1 estándar con replicación Standard_LRS.

4. Cree un espacio de nombres de Service Bus y una cola.

5. Cree una identidad de dispositivo para el dispositivo simulado que envíe mensajes al centro. Guarde la clave para la fase de pruebas. (Si crea una plantilla de Resource Manager, esto se realizará después de implementar la plantilla).