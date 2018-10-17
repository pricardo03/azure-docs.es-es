---
title: Devolución de Microsoft Azure Data Box | Microsoft Docs
description: Aprenda a enviar un dispositivo Data Box a Microsoft
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/03/2018
ms.author: alkohli
ms.openlocfilehash: 05bfba9fffa7db75ef4e1a1167b3170a775e1d34
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785467"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: Devolución de Azure Data Box y comprobación de la carga de datos en Azure

En este tutorial se describe cómo devolver Azure Data Box y comprobar los datos cargados en Azure.

En este tutorial, aprenderá sobre temas como:

> [!div class="checklist"]
> * Envío de Data Box a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de datos de Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que ha completado el [Tutorial: Copia de datos en Azure Data Box y comprobación de los mismos](data-box-deploy-copy-data.md).

## <a name="ship-data-box-back"></a>Devolución de Data Box

1. Asegúrese de que el dispositivo está apagado y de que se han eliminado los cables. Enrolle y coloque de forma segura el cable de alimentación que se proporcionó junto con el dispositivo en la parte posterior del mismo.
2. Si el dispositivo se vende en Estados Unidos, asegúrese de que la etiqueta de envío aparezca en la pantalla de tinta electrónica y programe una recogida con su transportista. Si la etiqueta se daña o se pierde o no aparece en la pantalla de tinta electrónica, descargue la etiqueta de envío de Azure Portal y péguela al dispositivo. Vaya a **Overview > Download shipping label** (Información general > Descargar etiqueta de envío). 

    Si el dispositivo se vende en Europa, la pantalla de tinta electrónica no muestra la etiqueta de envío. En su lugar, la etiqueta de envío de devolución se incluye en el bolsillo transparente bajo la etiqueta de reenvío. Retire la etiqueta de envío anterior y asegúrese de que la etiqueta de envío sea claramente visible.
    
3. Programe una recogida con UPS, si está devolviendo el dispositivo en Estados Unidos. Si va a devolver el dispositivo en Europa con DHL, para solicitar la recogida de DHL, visite su sitio Web y especifique el número de factura de porte aéreo. Visite el sitio Web de DHL Express del país y elija **Book a Courier Collection > eReturn Shipment** (Reservar una colección Courier > Envío eReturn). 

    Especifique el número de factura de porte aéreo y haga clic en **Schedule Pickup** (Programar la recogida) para organizar la recogida.

4. Una vez que el transportista recoge y examina el dispositivo Data Box, el estado del pedido en el portal se actualiza a **Picked up** (Recogido). También se muestra un identificador de seguimiento.

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

Cuando Microsoft recibe y examina el dispositivo, el estado del pedido se actualiza a **Received** (Recibido). El dispositivo, a continuación, se somete a una verificación física de daños o signos de manipulación. 

Una vez completada la comprobación, el dispositivo Data Box se conecta a la red del centro de datos de Azure. La copia de datos se inicia automáticamente. Dependiendo del tamaño de los datos, la operación de copia puede tardar horas o días en completarse. Puede supervisar el progreso del trabajo de copia en el portal.

Una vez finalizada la copia, el estado del pedido se actualiza a **Completed** (Completado).

Compruebe que los datos estén en las cuentas de almacenamiento antes de eliminarlos del origen. 

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
 Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Envío de Data Box a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de datos de Data Box

Pase al siguiente artículo para obtener información sobre cómo administrar Data Box a través de la interfaz de usuario web local.

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar Azure Data Box](./data-box-local-web-ui-admin.md)


