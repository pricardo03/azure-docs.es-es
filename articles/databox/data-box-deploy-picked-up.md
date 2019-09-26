---
title: Tutorial sobre la devolución de Azure Data Box | Microsoft Docs
description: Aprenda a enviar un dispositivo Data Box a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 28666aaac4ec221acca00d937d54a753a4e6a055
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71172679"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: Devolución de Azure Data Box y comprobación de la carga de datos en Azure

::: zone-end

::: zone target="chromeless"

# <a name="return-data-box-and-verify-data-upload-to-azure"></a>Devolución de Data Box y comprobación de la carga de datos en Azure

::: zone-end

::: zone target="docs"

En este tutorial se describe cómo devolver Azure Data Box y comprobar los datos cargados en Azure.

En este tutorial, aprenderá sobre temas como:

> [!div class="checklist"]
> * Requisitos previos
> * Preparación para el envío
> * Envío de Data Box a Microsoft
> * Comprobación de la carga de datos en Azure
> * Eliminación de datos de Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

- Ha completado el [Tutorial: Copia de datos a Azure Data Box y comprobación de](data-box-deploy-copy-data.md). 
- Los trabajos de copia están completos. Preparación para el envío no se puede ejecutar mientras que los trabajos de copia están en curso.

## <a name="prepare-to-ship"></a>Preparación para el envío

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Una vez completada la copia de datos, prepare y envíe el dispositivo. Cuando el dispositivo llega al centro de datos de Azure, los datos se cargan automáticamente en Azure.

## <a name="prepare-to-ship"></a>Preparación para el envío

Antes de prepararse para enviar, asegúrese de que los trabajos de copia se han completado.

1. Vaya a la página **Preparar para enviar** de la interfaz de usuario web local y comience la preparación del envío. 
2. Desactive el dispositivo desde la interfaz de usuario web local. Quite los cables del dispositivo. 

Los siguientes pasos vienen determinados por el lugar al que se vaya a devolver el dispositivo.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Devolución de Data Box

Asegúrese de que la copia de datos se ha completado en el dispositivo y que la ejecución **Preparación para el envío** se ha realizado correctamente. Según la región a dónde envíe el dispositivo, el procedimiento es distinto.

::: zone-end

## <a name="in-us-canada-europetabin-us-canada-europe"></a>[En EE. UU., Canadá, Europa](#tab/in-us-canada-europe)

Realice los pasos siguientes si va a devolver el dispositivo en Estados Unidos, Canadá o Europa.

1. Asegúrese de que el dispositivo está apagado y de que se han quitado los cables. 
2. Enrolle y coloque de forma segura el cable de alimentación que se proporcionó junto con el dispositivo en la parte posterior del mismo.
3. Asegúrese de que la etiqueta de envío aparece en la pantalla de tinta electrónica y programe una recogida con su transportista. Si la etiqueta está dañada, se ha perdido o no aparece en la pantalla de tinta electrónica, póngase en contacto con el servicio de soporte técnico de Microsoft. Si el soporte técnico lo sugiere, puede ir a **Información general > Descargar la etiqueta de envío** en Azure Portal. Descargue la etiqueta de envío y péguela en el dispositivo. 
4. Programe una recogida con UPS si está devolviendo el dispositivo. Para programar una recogida:

    - Llame a la oficina local de UPS (número gratuito específico del país o región).
    - En la llamada, indique el número de seguimiento del envío inverso, que se muestra en la pantalla E-ink (Tinta electrónica) o la etiqueta impresa.
    - Si no se indica el número de seguimiento, UPS solicitará que el abono de una cantidad adicional en la recogida.

    En lugar de programar la recogida, también devolver la instancia de Data Box en la ubicación de recogida más cercana.
4. Una vez que el transportista recoge y examina el dispositivo Data Box, el estado del pedido en el portal se actualiza a **Picked up** (Recogido). También se muestra un identificador de seguimiento.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]



::: zone-end


## <a name="in-australiatabin-australia"></a>[En Australia](#tab/in-australia)

Los centros de datos de Azure en Australia tienen una notificación de seguridad adicional. Todos los envíos entrantes deben tener una notificación avanzada. Realice los pasos siguientes si el envío se realiza en Australia.


1. Conserve la caja original utilizada para devolver el dispositivo.
2. Asegúrese de que la copia de datos se ha completado en el dispositivo y que la ejecución **Preparación para el envío** se ha realizado correctamente.
3. Apague el dispositivo y quite los cables.
4. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo.
5. Envíe un correo electrónico a Quantium Solutions para solicitar una recogida. Consulte el número de referencia de servicio especificado en Azure Portal. Use el siguiente asunto en el correo electrónico: *Request for reverse shipping label with TAU code* (Solicitud de etiqueta de envío inverso con código TAU). Asegúrese de incluir los siguientes detalles en el mensaje de correo electrónico: 

    ```
    To: Azure@quantiumsolutions.com
    Subject: Pickup request for Azure｜Reference number：XXX XXX XXX
    Body: 
    - Company name：
    - Address:
    - Contact name:
    - Contact number:
    - Requested pickup date: mm/dd
    ```
6. Quantium Solutions Australia le enviará por correo electrónico una etiqueta de devolución.
7. Imprima la etiqueta de devolución y péguela en la caja de envío.
8. Entregue el paquete al mensajero.

Si es necesario, puede escribir al soporte técnico de Quantium Solutions a Azure@quantiumsolutions.com o puede ponerse en contacto por teléfono.

Para consultas telefónicas relacionadas con su pedido:

- Primero, envíe un correo electrónico para la recogida.
- Proporcione el nombre del pedido por teléfono.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-japantabin-japan"></a>[En Japón](#tab/in-japan) 

1. Conserve la caja original utilizada para devolver el dispositivo.
2. Apague el dispositivo y quite los cables.
3. Enrolle y coloque de forma segura el cable de alimentación que se suministró junto con el dispositivo en la parte posterior del mismo.
4. Escriba el nombre y la dirección de la empresa en la nota de entrega como información del remitente.
5. Envíe un correo electrónico a Quantium Solutions mediante la plantilla de correo electrónico que tiene a continuación.

    - Tanto si no se incluyó la nota de entrega de Japan Post Chakubarai como si falta, especifíquelo en este correo electrónico. Quantium Solutions Japan se encargará de solicitar a Japan Post que le proporcionen una nota de entrega en la recogida.
    - Si tiene varios pedidos, envíe un correo electrónico para comprobar cada recogida individual.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. Recibirá un correo electrónico de confirmación de Quantium Solutions tras concertar una recogida. Este correo electrónico también incluye información sobre la nota de entrega de Chakubarai.

Si es necesario, puede ponerse en contacto con el soporte técnico de Quantium Solutions (en japonés) en: 

- Correo electrónico: Customerservice.JP@quantiumsolutions.com 
- Teléfono：03-5755-0150 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Comprobación de la carga de datos en Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Eliminación de datos de Data Box
 
Una vez que se completa la carga en Azure, Data Box elimina los datos de los discos según las [directrices de la revisión 1 de NIST SP 800-88](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end



