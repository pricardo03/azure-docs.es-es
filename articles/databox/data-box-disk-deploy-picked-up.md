---
title: Devolución de Microsoft Azure Data Box Disk | Microsoft Docs
description: Use este tutorial para saber cómo enviar Azure Data Box Disk a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 357fa8a34afc8b426d308940462e22895130169f
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158778"
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
2. Envuelva todos los discos y los cables de conexión en un envoltorio de burbujas y colóquelos en la caja de envío.
3. Utilice la etiqueta de envío de devolución en la funda plástica transparente fijada en la caja. Si la etiqueta se daña o se pierde, descargue una nueva etiqueta de envío desde Azure Portal y péguela en el dispositivo. Vaya a **Overview > Download shipping label** (Información general > Descargar etiqueta de envío). 

    ![Descarga de la etiqueta de envío](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    De esta forma se descarga una etiqueta de envío de devolución como se muestra a continuación.

    ![Ejemplo de etiqueta de envío](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Selle la caja de envío y asegúrese de que la etiqueta de envío de devolución está visible.
5. Programe una recogida con UPS, si está devolviendo el dispositivo en Estados Unidos. Si va a devolver el dispositivo en Europa con DHL, para solicitar la recogida de DHL, visite su sitio Web y especifique el número de factura de porte aéreo. Visite el sitio Web de DHL Express del país y elija **Book a Courier Collection > eReturn Shipment** (Reservar una colección Courier > Envío eReturn).

    ![Envío de DHL ereturn](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
    Especifique el número de factura de porte aéreo y haga clic en **Schedule Pickup** (Programar la recogida) para organizar la recogida.

      ![Programación de la recogida](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

7. Una vez que los discos son recogidos por el transportista, el estado del pedido en el portal se actualiza a **Picked up** (Recogido). También se muestra un identificador de seguimiento.

    ![Discos recogidos](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

Cuando Microsoft recibe y examina el disco, el estado del trabajo se actualiza a **Received** (Recibido). 

![Discos recibidos](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Los datos se copian automáticamente una vez que los discos se conectan a un servidor en el centro de datos de Azure. Dependiendo del tamaño de los datos, la operación de copia puede tardar horas o días en completarse. Puede supervisar el progreso del trabajo de copia en el portal.

Una vez finalizada la copia, el estado del pedido se actualiza a **Completed** (Completado).

![Copia de datos finalizada](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Compruebe que los datos estén en las cuentas de almacenamiento antes de eliminarlos del origen. Para comprobar que los datos se han cargado en Azure, realice los pasos siguientes:

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


