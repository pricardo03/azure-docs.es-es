---
title: archivo de inclusión
description: archivo de inclusión
services: signalr
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 97b8ef5e260a853ecdffb3c502f8a5051dd0e143
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006306"
---
## <a name="create-an-azure-signalr-service-instance"></a>Creación de una instancia del servicio Azure SignalR

La aplicación se conectará a una instancia del servicio SignalR en Azure.

1. Seleccione el botón Nuevo de la esquina superior izquierda en Azure Portal. En la pantalla Nuevo, escriba *SignalR Service* (Servicio SignalR) en el cuadro de búsqueda y presione Entrar.

    ![Búsqueda del servicio SignalR](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Seleccione **SignalR Service** (Servicio SignalR) en los resultados de la búsqueda y, a continuación, seleccione **Crear**.

1. Escriba la siguiente configuración.

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre del recurso** | Nombre único globalmente | Nombre que identifica la nueva instancia del servicio SignalR. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en que se creará esta nueva instancia del servicio SignalR. | 
    | **[Grupo de recursos](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Nombre del nuevo grupo de recursos en el que se va a crear la instancia del servicio SignalR. | 
    | **Ubicación** | Oeste de EE. UU. | Seleccione una [región](https://azure.microsoft.com/regions/) cerca de usted. |
    | **Plan de tarifa** | Gratuito | Pruebe el servicio Azure SignalR de forma gratuita. |
    | **Recuento de unidades** |  No aplicable | El recuento de unidades especifica cuántas conexiones puede aceptar la instancia del servicio SignalR. Solo es configurable en el nivel Estándar. |

    ![Creación de una instancia del servicio SignalR](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. Seleccione **Crear** para empezar a implementar la instancia del servicio SignalR.
