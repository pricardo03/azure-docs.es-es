---
title: Guía de administración del portal de Azure Data Box | Microsoft Docs
description: Describe cómo usar Azure Portal para administrar Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 10/19/2018
ms.author: alkohli
ms.openlocfilehash: 1b228a66f2d59b3ff252df266783f7bd5d27139e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645446"
---
# <a name="use-the-azure-portal-to-administer-your-data-box"></a>Use Azure Portal para administrar Data Box

En este artículo se describen algunas de las tareas de administración y los flujos de trabajo complejos que se pueden realizar en Data Box. Puede administrar Data Box a través de Azure Portal y de la interfaz de usuario web local. 

Este artículo se centra en las tareas que se pueden realizar mediante Azure Portal. Use Azure Portal para administrar pedidos, administrar los Data Box y realizar un seguimiento del estado del pedido mientras continúa hacia la finalización.


## <a name="cancel-an-order"></a>Cancelación de un pedido

Es posible que deba cancelar un pedido por diversos motivos después de realizarlo. Solo puede cancelar el pedido antes de que se procese. Una vez que el pedido se haya procesado y Data Box esté preparado, no es posible cancelar el pedido. 

Realice los pasos siguientes para cancelar un pedido.

1.  Vaya a **Overview > Cancel** (Información general > Cancelar). 

    ![Cancelación del pedido 1](media/data-box-portal-admin/cancel-order1.png)

2.  Indique un motivo para cancelar el pedido.  

    ![Cancelación del pedido 2](media/data-box-portal-admin/cancel-order2.png)

3.  Una vez que se cancela el pedido, el portal actualiza su estado y lo muestra como **Canceled** (Cancelado). 

## <a name="clone-an-order"></a>Clonación de un pedido

La clonación es útil en determinadas situaciones. Por ejemplo, un usuario ha usado Data Box para transferir algunos datos. A medida que se generan más datos, se necesita otro Data Box para transferirlos a Azure. En este caso, simplemente se puede clonar el mismo pedido.

Realice los pasos siguientes para clonar un pedido.

1.  Vaya a **Overview > Clone** (Información general > Clonar). 

    ![Clonación del pedido 1](media/data-box-portal-admin/clone-order1.png)

2.  Todos los detalles del pedido permanecen iguales. El nombre del pedido es el del pedido original seguido de *-Clone*. Seleccione la casilla para confirmar que ha revisado la información de privacidad. Haga clic en **Create**(Crear).

El clon se crea en unos minutos y el portal se actualiza para mostrar el nuevo pedido.


## <a name="delete-order"></a>Eliminación del pedido

Es posible que desee eliminar un pedido cuando se complete. El pedido contiene información personal, como su nombre, dirección e información de contacto. Esta información personal se elimina cuando se elimina el pedido.

Solo se pueden eliminar pedidos completados o cancelados. Realice los siguientes pasos para eliminar un pedido.

1. Vaya a **Todos los recursos**. Busque su pedido.

2. Haga clic en el pedido que desee eliminar y vaya a **Overview** (Información general). En la barra de comandos, haga clic en **Delete** (Eliminar).

    ![Eliminación del pedido de Data Box 1](media/data-box-portal-admin/delete-order1.png)

3. Escriba el nombre del pedido cuando se le pida que confirme su eliminación. Hacer clic en **Eliminar**.

## <a name="download-shipping-label"></a>Descarga de la etiqueta de envío

Deberá descargar la etiqueta de envío si no funciona la pantalla de tinta electrónica de su Data Box y no muestra la etiqueta de envío de devolución. 

Siga estos pasos para descargar una etiqueta de envío.

1.  Vaya a **Overview > Download shipping label** (Información general > Descargar etiqueta de envío). Esta opción está disponible solo una vez enviado el dispositivo. 

    ![Descarga de la etiqueta de envío](media/data-box-portal-admin/download-shipping-label.png)

2.  Se descargará la siguiente etiqueta de envío devuelto. Guarde la etiqueta e imprímala. Doble e inserte la etiqueta en la funda transparente del dispositivo. Asegúrese de que la etiqueta está visible. Quite cualquier etiqueta del envío anterior que aún se encuentre en el dispositivo.

    ![Ejemplo de etiqueta de envío](media/data-box-portal-admin/example-shipping-label.png)

## <a name="edit-shipping-address"></a>Edición de la dirección de envío

Puede que tenga que editar la dirección de envío, una vez que se haya realizado el pedido. Esta opción solo está disponible hasta que se envía el dispositivo. Después, deja de estarlo.

Realice los pasos siguientes para editar el pedido.

1. Vaya a **Order details > Edit shipping address** (Detalles del pedido > Editar dirección de envío).

    ![Edición de la dirección de envío 1](media/data-box-portal-admin/edit-shipping-address1.png)

2. Edite y valide la dirección de envío y, a continuación, guarde los cambios.

    ![Edición de la dirección de envío 2](media/data-box-portal-admin/edit-shipping-address2.png)

## <a name="edit-notification-details"></a>Edición de los detalles de la notificación

Es posible que deba cambiar los usuarios que desea que reciban los mensajes de estado del pedido. Por ejemplo, hay que informar a un usuario cuando se entrega o recoge el dispositivo. Otro usuario deba ser informado cuando se completa la copia de datos para que pueda comprobar que están en la cuenta de almacenamiento de Azure antes de eliminarlos del origen. En estos casos, puede editar los detalles de notificación.

Realice los siguientes pasos para editar una plantilla de notificación.

1. Vaya a **Order details > Edit notification details** (Detalles de pedidos > Editar detalles de la notificación).

    ![Edición de los detalles de la notificación 1](media/data-box-portal-admin/edit-notification-details1.png)

2. Ahora puede editar los detalles de la notificación y, a continuación, guardar los cambios.
 
    ![Edición de los detalles de la notificación 2](media/data-box-portal-admin/edit-notification-details2.png)


## <a name="download-order-history"></a>Descarga del historial de pedidos

Una vez completado el pedido a Data Box, se eliminan los datos de los discos del dispositivo. Una vez limpio el dispositivo, puede descargar el historial de pedidos de Azure Portal.

Realice los siguientes pasos para descargar el historial de pedidos.

1. En el pedido de Data Box, vaya a **Introducción**. Asegúrese de que el pedido se haya completado. Con el pedido completado y el dispositivo limpio, vaya a **Detalles del pedido**. La opción **Descargar el historial de pedidos** está disponible.

    ![Descarga del historial de pedidos](media/data-box-portal-admin/download-order-history-1.png)

2. Haga clic en **Descargar el historial de pedidos**. En el historial descargado verá un registro de las entradas de seguimiento del transportista. Al desplazarse hasta el final de este registro verá los vínculos a:
    
    - **Registros de copia**: lista de los archivos sin errores durante la copia de datos de Data Box a la cuenta de almacenamiento de Azure.
    - **Registros de auditoría**: contienen información sobre el encendido y el acceso compartido en Data Box cuando está fuera del centro de datos de Azure.
    - **BOM files**(Archivos de listas de materiales): lista de archivos (también conocida como el manifiesto de archivos) que puede descargar durante el proceso de **Preparación para el envío** y que tiene los nombres de archivo, los tamaños de archivo y las sumas de comprobación de archivo.

        ```
        -------------------------------
        Microsoft Data Box Order Report
        -------------------------------
        
        Name                                               : eastusdryrun                                      
        StartTime(UTC)                                     : 9/6/2018 12:54:47 PM +00:00                       
        DeviceType                                         : ImolaPod                                          
        
        -------------------
        Data Box Activities
        -------------------
        
        Time(UTC)             | Activity                       | Status          | Description                                                                                                                                           
        
        9/6/2018 12:54:51 PM  | OrderCreated         | Completed  |                                                                                                                              
        9/11/2018 8:57:38 PM  | DevicePrepared       | Completed  |                                                                                                                                                       
        9/12/2018 7:28:15 PM  | ShippingToCustomer   | InProgress | Pickup Scan. Local Time : 9/12/2018 2:52:31 PM at Chantilly                                                                                           
        9/13/2018 2:33:04 AM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/12/2018 9:00:00 PM at Chantilly                                                                                                                                                                                                                                                              
        9/13/2018 12:40:31 PM | ShippingToCustomer   | InProgress | Arrival Scan. Local Time : 9/13/2018 5:00:00 AM at Oakland                                                                                            
        9/13/2018 2:42:10 PM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/13/2018 6:08:00 AM at Oakland                                                                                          
        9/13/2018 3:42:12 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:14:08 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:56:54 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Out For Delivery Today. Local Time : 9/13/2018 9:11:21 AM at Sunnyvale                                                                                
        9/13/2018 5:43:07 PM  | ShippingToCustomer   | Completed  | Delivered. Local Time : 9/13/2018 9:44:17 AM at SUNNYVALE                                                                                             
        9/14/2018 11:48:35 PM | ShippingToDataCenter | InProgress | Pickup Scan. Local Time : 9/14/2018 3:55:37 PM at Sunnyvale                                                                                                                                                                                 
        9/15/2018 1:52:35 AM  | ShippingToDataCenter | InProgress | Arrival Scan. Local Time : 9/14/2018 6:31:00 PM at San Jose                                                                                           
        9/15/2018 2:52:39 AM  | ShippingToDataCenter | InProgress | Departure Scan. Local Time : 9/14/2018 7:17:00 PM at San Jose                                                                                                                                                                             
        9/17/2018 8:23:31 AM  | ShippingToDataCenter | InProgress | Destination Scan. Local Time : 9/17/2018 4:14:37 AM at Chantilly                                                                                      
        9/17/2018 12:24:42 PM | ShippingToDataCenter | InProgress | Loaded on Delivery Vehicle. Local Time : 9/17/2018 7:45:36 AM at Chantilly                                                                            
        9/17/2018 1:25:11 PM  | ShippingToDataCenter | InProgress | Out For Delivery Today. Local Time : 9/17/2018 8:27:11 AM at Chantilly                                                                                
        9/17/2018 2:25:51 PM  | ShippingToDataCenter | Completed | Delivered. Local Time : 9/17/2018 9:56:32 AM at STERLING                                                                                              
        9/18/2018 9:55:41 PM  | DeviceBoot           | Completed | Appliance booted up successfully                                                                                                                      
        9/18/2018 11:00:25 PM | DataCopy             | Started   |                                                                                                                                                       
        9/18/2018 11:01:33 PM | DataCopy             | Completed | Copy Completed.                                                                                                                                       
        9/18/2018 11:20:58 PM | SecureErase          | Started   |                                                                                                                                                       
        9/18/2018 11:28:46 PM | SecureErase          | Completed | Azure Data Box:BY506B4B616700 has been sanitized according to NIST 800 -88 Rev 1.                                                                     
        
        ----------------------
        Data Box Job Log Links
        ----------------------
        
        Account Name         : eastusdryrun                                         
        Copy Logs Path       : copylog/copylogd695869a2a294396b7b903296c208388.xml                                                                                                                                                     
        Audit Logs Path      : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700                                                                                                                     
        BOM Files Path       : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700
        ```
A continuación, puede ir a la cuenta de almacenamiento y ver los registros de copia.

![Registros en las cuentas de almacenamiento](media/data-box-portal-admin/logs-in-storage-acct-2.png)

También puede ver la cadena de los registros de custodia que incluyen los registros de auditoría y los archivos de la lista de materiales.

![Registros en las cuentas de almacenamiento](media/data-box-portal-admin/logs-in-storage-acct-1.png)

## <a name="view-order-status"></a>Visualización del estado del pedido

Cuando cambie el estado del dispositivo en el portal, se le notificará por correo electrónico.

|Estado del pedido |DESCRIPCIÓN |
|---------|---------|
|Ordered (Realizado)     | Se ha realizado correctamente un pedido. <br>Si el dispositivo está disponible, Microsoft identifica uno para el envío y lo prepara. <br> Si el dispositivo no está disponible inmediatamente, el pedido se procesará cuando el dispositivo esté disponible. El pedido puede tardar varios días o un par de meses en procesarse. Si el pedido no se puede completar en 90 días, se cancela el pedido y se le notifica.         |
|Processed (Procesado)     | El procesamiento del pedido está completado. Según su pedido, el dispositivo se prepara para su envío en el centro de datos.         |
|Dispatched (Enviado)     | Ha enviado el pedido. Use el identificador de seguimiento que se muestra en el pedido del portal para realizar un seguimiento del envío.        |
|Delivered (Entregado)     | El envío se entregó a la dirección especificada.        |
|Picked up (Recogido)     |El transportista recogió y analizó el envío de devolución.         |
|Received     | El dispositivo se recibió y examinó en el centro de datos de Azure. <br> Una vez que se inspecciona el envío, se iniciará la carga del dispositivo.      |
|Copia de datos     | La copia de datos está en curso. Realice un seguimiento del progreso de la copia del pedido en Azure Portal. <br> Espere hasta que se complete. |
|Completed       |El pedido se realizó correctamente.<br> Compruebe que los datos están en Azure antes de eliminarlos en el entorno local de los servidores.         |
|Completed with errors (Completado con errores)| Se completó la copia de datos pero se produjeron errores durante la copia. <br> Revise los registros de copia con la ruta de acceso proporcionada en Azure Portal.   |
|Canceled            |El pedido se ha cancelado. <br> Se canceló el pedido o se produjo un error y el servicio lo canceló. Si el pedido no se puede completar en 90 días, también se cancela el pedido y se le notifica.     |
|Limpieza | Se borran los datos en los discos del dispositivo. La limpieza del dispositivo se considera completa cuando el historial de pedidos está disponible para su descarga en Azure Portal.|



## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [Solucionar problemas de Data Box](data-box-faq.md).
