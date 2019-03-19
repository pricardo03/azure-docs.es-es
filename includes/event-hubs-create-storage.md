---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 690e0da160042b7ac432b1a36c024a8a8f439823
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58125279"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Creación de una cuenta de almacenamiento para el host de procesador de eventos
El host de procesador de eventos es un agente inteligente que simplifica la recepción de eventos desde Event Hubs mediante la administración de puntos de control persistentes y recepciones paralelas. Para los puntos de comprobación, el host de procesador de eventos requiere una cuenta de almacenamiento. El ejemplo siguiente muestra cómo crear una cuenta de almacenamiento y cómo obtener sus claves para el acceso:

1. En Azure Portal, seleccione **Crear un recurso** en la parte superior izquierda de la pantalla.

2. Seleccione **Storage** y, a continuación, seleccione **Storage account - blob, file, table, queue** (Cuenta de almacenamiento: blob, archivo, tabla, cola).
   
    ![Seleccionar Cuenta de almacenamiento](./media/event-hubs-create-storage/create-storage1.png)

3. En la página **Crear cuenta de almacenamiento**, realice los pasos siguientes: 

   1. Escriba un nombre para la cuenta de almacenamiento. 
   2. Elija una suscripción de Azure que contenga el centro de eventos.
   3. Seleccione el grupo de recursos que tenga el centro de eventos.
   4. Seleccione una ubicación en la que crear el recurso. 
   5. Después, haga clic en **Revisar + crear**.
   
      ![Página Crear cuenta de almacenamiento](./media/event-hubs-create-storage/create-storage2.png)

4. En la página **Revisar + crear**, revise los valores y seleccione **Crear**. 

    ![Revisión de la configuración de la cuenta de almacenamiento y creación](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Una vez que aparezca el **implementaciones correctas** mensaje, seleccione **ir al recurso** en la parte superior de la página. También puede iniciar la página Cuenta de almacenamiento seleccionando la cuenta de almacenamiento en la lista de recursos.  

    ![Selección de la cuenta de almacenamiento desde la implementación](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. En la ventana **Información esencial**, haga clic en **Blobs**. 

    ![Selección del servicio Blobs](./media/event-hubs-create-storage/select-blobs-service.png)
1. Seleccione **+ Contenedor** en la parte superior, escriba un **nombre** para el contenedor y seleccione **Aceptar**. 

    ![Creación de un contenedor de blobs](./media/event-hubs-create-storage/create-blob-container.png)
1. Seleccione **Claves de acceso** en el menú de la izquierda y copie el valor de **key1**. 

    Guarde los valores siguientes en el Bloc de notas o en cualquier otra ubicación temporal.
    - Nombre de la cuenta de almacenamiento
    - Clave de acceso para la cuenta de almacenamiento
    - Nombre del contenedor
