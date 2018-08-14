---
title: Creación de centro de eventos de Azure | Microsoft Docs
description: Creación de un espacio de nombres de Azure Event Hubs y un centro de eventos con Azure Portal
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: shvija
ms.openlocfilehash: fb2020f7bfc8521e141adb4eefbc227e8123a269
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40002627"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal

## <a name="create-an-event-hubs-namespace"></a>Crear un espacio de nombres de Event Hubs

1. Inicie sesión en [Azure Portal][Azure portal] y haga clic en **Crear un recurso** en la parte superior izquierda de la pantalla.
2. Haga clic en **Internet de las cosas** y, luego, en **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. En **Crear espacio de nombres**, especifique el nombre del espacio de nombres. El sistema realiza la comprobación automáticamente para ver si el nombre está disponible.  

4. Después de asegurarse de que el nombre del espacio de nombres está disponible, elija el plan de tarifa (Básico o Estándar). Elija también una suscripción de Azure, el grupo de recursos y la ubicación en la que se va a crear el recurso.
 
5. Haga clic en **Crear** para crear el espacio de nombres. Puede que tenga que esperar unos minutos hasta que el sistema aprovisione totalmente los recursos.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. En la lista de espacios de nombres del portal, haga clic en el espacio de nombres recién creado.

7. Haga clic en **Directivas de acceso compartido** y después en **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. Haga clic en el botón Copiar para copiar la cadena de conexión **RootManageSharedAccessKey** en el Portapapeles. Guarde esta cadena de conexión en una ubicación temporal, como el Bloc de notas, para usarla más adelante.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Crear un centro de eventos

1. En la lista de espacios de nombres de Event Hubs, haga clic en el espacio de nombres recién creado.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. En la hoja del espacio de nombres, haga clic en **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. En la parte superior de la hoja, haga clic en **+ Centro de eventos**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Escriba el nombre del centro de eventos y, a continuación, haga clic en **Crear**. 

Ya se ha creado el centro de eventos y cuenta con las cadenas de conexión que necesita para enviar y recibir eventos.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más acerca de Event Hubs, consulte estos vínculos:

* [Información general de Event Hubs](event-hubs-what-is-event-hubs.md)
* [Introducción a la API de Event Hubs](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/