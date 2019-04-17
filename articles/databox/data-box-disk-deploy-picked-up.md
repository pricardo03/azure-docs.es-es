---
title: Devolución de Microsoft Azure Data Box Disk | Microsoft Docs
description: Use este tutorial para saber cómo enviar Azure Data Box Disk a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 7c62ec722e5bb909c13b21a7b98d86dd72d9e665
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425957"
---
# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Tutorial: Devolución de Azure Data Box Disk y comprobación de la carga de datos en Azure

Este es el último tutorial de la serie: Implementación de Azure Data Box Disk. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Envío de Data Box Disk a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de los datos de Data Box Disk

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que ha completado [Tutorial: Copia de datos a Azure Data Box Disk y comprobación de los mismos](data-box-disk-deploy-copy-data.md).

## <a name="ship-data-box-disk-back"></a>Devolución de Data Box Disk

1. Una vez completada la validación de datos, desconecte los discos. Quite los cables de conexión.
2. Envuelva todos los discos y los cables de conexión en un envoltorio de burbujas y colóquelos en la caja de envío. La falta de accesorios puede acarrear un costo.
    - Utilice el empaquetado que se usó en el envío inicial.  
    - Es aconsejable usar un envoltorio de burbujas bien protegido para empaquetar los discos.
    - Asegúrese de que todo encaja perfectamente para reducir los movimientos dentro de la caja.

Los siguientes pasos vienen determinados por el lugar al que se vaya a devolver el dispositivo.

### <a name="pick-up-in-us-canada"></a>Recogida en EE. UU. y Canadá

Realice los pasos siguientes si va a devolver el dispositivo en Estados Unidos o Canadá.

1. Utilice la etiqueta de envío de devolución en la funda plástica transparente fijada en la caja. Si la etiqueta se daña o se pierde:
    - Vaya a **Overview > Download shipping label** (Información general > Descargar etiqueta de envío).

        ![Descarga de la etiqueta de envío](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        De esta forma se descarga una etiqueta de envío de devolución como se muestra a continuación.

        ![Ejemplo de etiqueta de envío](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Pegue la etiqueta en el dispositivo.

2. Selle la caja de envío y asegúrese de que la etiqueta de envío de devolución está visible.
3. Póngase en contacto con UPS para programar la recogida del dispositivo.


### <a name="pick-up-in-europe"></a>Recogida en Europa

Realice los pasos siguientes si va a devolver el dispositivo en Europa.

1. Utilice la etiqueta de envío de devolución en la funda plástica transparente fijada en la caja. Si la etiqueta se daña o se pierde:
    - Vaya a **Overview > Download shipping label** (Información general > Descargar etiqueta de envío).

        ![Descarga de la etiqueta de envío](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        De esta forma se descarga una etiqueta de envío de devolución como se muestra a continuación.

        ![Ejemplo de etiqueta de envío](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Pegue la etiqueta en el dispositivo.

2. Selle la caja de envío y asegúrese de que la etiqueta de envío de devolución está visible.
3. Si va a devolver el dispositivo en Europa con DHL, para solicitar la recogida de DHL, visite su sitio Web y especifique el número de factura de porte aéreo.
4. Visite el sitio Web de DHL Express del país y elija **Book a Courier Collection > eReturn Shipment** (Reservar una colección Courier > Envío eReturn).

    ![Envío de devolución de DHL](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
3. Especifique el número de factura de porte aéreo y haga clic en **Schedule Pickup** (Programar la recogida) para organizar la recogida.

      ![Programación de la recogida](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

### <a name="pick-up-in-asia-pacific-region"></a>Recogida en la región de Asia Pacífico

Esta región incluye instrucciones para la recogida en Australia, Japón y Corea.

#### <a name="pick-up-in-australia"></a>Recogida en Australia

Los centros de datos de Azure en Australia tienen una notificación de seguridad adicional. Todos los envíos entrantes deben tener una notificación avanzada. Realice los pasos siguientes para la recogida en Australia.

1. Envíe un correo electrónico a `adbops@microsoft.com` para solicitar una etiqueta de envío con el identificador entrante único o el código TAU. Para tener la etiqueta a tiempo, realice la solicitud al menos 3 días de antelación a la fecha de envío planeada.
2. El asunto del correo electrónico debe ser: *Request for reverse shipping label with TAU code* (Solicitud de etiqueta de envío inverso con código TAU). Asegúrese de incluir los siguientes detalles en el mensaje de correo electrónico: 

    - Nombre del pedido
    - Dirección
    - Nombre de contacto

#### <a name="pick-up-in-japan"></a>Recogida en Japón

1. Asegúrese de incluir la nota de devolución de Japón Post Chakubarai.
2. Escriba el nombre y la dirección de la empresa en la nota de devolución como información del remitente.
3. Llame al número de teléfono de Japón Post para solicitudes de recogida, 0800-0800-111 (gratuito). Teclee el código postal de 7 dígitos de la dirección de recogida y, después, envíelo a la oficina de correos más cercana.
    - Las horas en las que se atienden las solicitudes de recogida dependen de las distintas oficinas de correos.
    - Indique que el envío es para Japón Post Chakubarai Yu-Pack.
    - Use la nota de Chakubarai que se ha incluido.
4. Si no se ha incluido la nota de Japón Post Chakubarai, envíe un mensaje de correo electrónico a *Quantium Solutions* a la dirección `Customerservice.JP@quantiumsolutions.com`. *Quantium Solutions* solicitará a Japón Post la recogida y les pedirá que traigan la nota en la recogida.
    - Indique el número de referencia en la columna de comentarios de la nota de Chakubarai que llevará Japan Post.
    - Escriba la dirección de entrega como se muestra a continuación:   
        ```
        3F N7 Prologis Park Tokyo Ohta, 1-3-6 Tokai Ohta-ku, Tokyo 143-0001
        Microsoft Service Center c/o Quantium Solutions Japan
        TEL: 03-5755-0150
        ```

Si falta la nota de Chakubarai, puede solicitar la recogida mediante correo electrónico. Utilice la siguiente plantilla de correo electrónico para solicitar la recogida.

```
To: Customerservice.JP@quantiumsolutions.com
Subject: Pickup request for Azure Data Box Disk｜Job Name： 
Body: 
- Azure Data Box Disk job name：
- Reference number:  
- Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
```

#### <a name="pick-up-in-korea"></a>Recogida en Corea

1. Asegúrese de incluir la nota de retorno.
2. Para solicitar la recogida:
    1. Llame a la línea directa de *Quantium Solutions International*, 070-8231-1418, en horario de oficina (de 10 AM a 5 PM, de lunes a viernes). Indique *Microsoft pickup* y el número de nota para organizar la recogida.  
    2. Si la línea directa está ocupada, envíe un mensaje de correo electrónico a `microsoft@rocketparcel.com` con el asunto *Microsoft Pickup* y el número de connote como referencia.
    3. Si el mensajero no realiza la recogida, llame a la línea directa de *Quantium Solutions International* para buscar otra fecha. 

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

Una vez que los discos son recogidos por el transportista, el estado del pedido en el portal se actualiza a **Picked up** (Recogido). También se muestra un identificador de seguimiento.

![Discos recogidos](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Cuando Microsoft recibe y examina el disco, el estado del trabajo se actualiza a **Received** (Recibido). 

![Discos recibidos](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Los datos se copian automáticamente una vez que los discos se conectan a un servidor en el centro de datos de Azure. Dependiendo del tamaño de los datos, la operación de copia puede tardar horas o días en completarse. Puede supervisar el progreso del trabajo de copia en el portal.

Una vez finalizada la copia, el estado del pedido se actualiza a **Completed** (Completado).

![Copia de datos finalizada](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Compruebe que los datos estén en las cuentas de almacenamiento antes de eliminarlos del origen. Los datos pueden estar en:

- Sus cuentas de Azure Storage. Al copiar los datos en Data Box, dependiendo del tipo, estos se cargan en una de las siguientes rutas de acceso de la cuenta de Azure Storage.

  - Para los blobs en bloques y los blobs en páginas: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Para Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Como alternativa, puede ir a su cuenta de almacenamiento de Azure en Azure Portal e ir desde allí.

- Sus grupos de recursos de disco administrados. Al crear discos administrados, los discos duros virtuales se cargan como blobs en páginas y se convierten en discos administrados. Los discos administrados se conectan a los grupos de recursos especificados en el momento de creación del pedido.

  - Si la copia en los discos administrados de Azure se realizó correctamente, puede ir a **Detalles del pedido** en Azure Portal y tomar nota de los grupos de recursos especificados para los discos administrados.

      ![Visualización de los detalles de pedido](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Vaya al grupo de recursos anotado y busque los discos administrados.

      ![Grupo de recursos para discos administrados](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Si copió un VHDX o un disco duro virtual dinámico o de diferenciación, el VHD o VHDX se carga en la cuenta de almacenamiento provisional como si fuera un blob en bloques. Vaya a su almacenamiento provisional **Cuenta de almacenamiento > Blobs** y seleccione el contenedor adecuado (SSD estándar, HDD estándar o SSD prémium). Los VHD/VHDX deberían aparecer como blobs en bloques en su cuenta de almacenamiento provisional.

Para comprobar que los datos se han cargado en Azure, realice los pasos siguientes:

1. Vaya a la cuenta de almacenamiento asociada al pedido de disco.
2. Vaya a **Blob service > Examinar blobs**. Se presenta la lista de contenedores. En la subcarpeta que creó bajo las carpetas *BlockBlob* y *PageBlob*, se crean contenedores con el mismo nombre en la cuenta de almacenamiento.
    Si los nombres de carpeta no se ajustan a las convenciones de nomenclatura de Azure, se producirá un error en la carga de datos en Azure.

4. Para comprobar que ha cargado todo el conjunto de datos, use el Explorador de Microsoft Azure Storage. Asocie la cuenta de almacenamiento correspondiente al pedido de alquiler de disco y, a continuación, examine la lista de contenedores de blobs. Seleccione un contenedor, haga clic en **... Más** y, a continuación, haga clic en **Folder statistics** (Estadísticas de carpeta). En el panel **Actividades**, se muestran las estadísticas de esa carpeta incluido el número de blobs y el tamaño total de los blobs. El tamaño total de los blobs en bytes debe coincidir con el tamaño del conjunto de datos.

    ![Estadísticas de carpeta en el Explorador de Storage](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Eliminación de los datos de Data Box Disk

Una vez que se ha completado la copia y ha comprobado que los datos están en la cuenta de almacenamiento de Azure, los discos se borran de forma segura según el estándar NIST.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre temas relacionados con Azure Data Box Disk; por ejemplo:

> [!div class="checklist"]
> * Envío de Data Box Disk a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de los datos de Data Box Disk


Avance hasta el siguiente procedimiento para obtener información sobre cómo administrar Data Box Disk mediante Azure Portal.

> [!div class="nextstepaction"]
> [Uso de Azure Portal para administrar Azure Data Box Disk](./data-box-portal-ui-admin.md)


