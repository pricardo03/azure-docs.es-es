---
title: Devolver el dispositivo de borde del cuadro de datos de Azure | Microsoft Docs
description: Describe cómo devolver el dispositivo de borde del cuadro de datos de Azure y eliminar el pedido para el dispositivo.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/07/2019
ms.author: alkohli
ms.openlocfilehash: 9aeae0ab68d809b36a3316054f12a5a9657721f1
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65468610"
---
# <a name="return-your-azure-data-box-edge-device"></a>Devolver el dispositivo de borde del cuadro de datos de Azure

En este artículo se describe cómo borrar los datos y, a continuación, devolver el dispositivo de borde del cuadro de datos de Azure. Una vez que haya devuelto el dispositivo, también puede eliminar el recurso asociado con el dispositivo.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Borrar los datos de los discos de datos en el dispositivo
> * Abra una incidencia de soporte técnico para devolver el dispositivo
> * Módulo de seguridad del dispositivo y programar una recogida
> * Eliminar el recurso en Azure portal

## <a name="erase-data-from-the-device"></a>Borrar los datos del dispositivo

Para borrar los datos de los discos de datos del dispositivo, deberá restablecer el dispositivo. Puede restablecer el dispositivo mediante la interfaz de usuario web local o la interfaz de PowerShell.

Antes de restablecer, crear una copia de los datos locales en el dispositivo si es necesario. Puede copiar los datos desde el dispositivo a un contenedor de almacenamiento de Azure.

Para restablecer el dispositivo mediante la interfaz de usuario web local, siga estos pasos.

1. En la interfaz de usuario de web local, vaya a **Mantenimiento > restablece dispositivo**.
2. Seleccione **Restablecer dispositivo**.

    ![Restablecer el dispositivo](media/data-box-edge-return-device/device-reset-1.png)

3. Cuando se le pida confirmación, revise la advertencia y seleccione **Sí** para continuar.

    ![Confirme el restablecimiento](media/data-box-edge-return-device/device-reset-2.png)  

El restablecimiento borra los datos de los discos de datos del dispositivo. Según la cantidad de datos en el dispositivo, este proceso tarda unos 30-40 minutos.

Como alternativa, conéctese a la interfaz de PowerShell del dispositivo y utilice la `Reset-HcsAppliance` cmdlet para borrar los datos de los discos de datos. Para obtener más información, consulte [restablecer el dispositivo](data-box-edge-connect-powershell-interface.md#reset-your-device).

> [!NOTE]
> - Si va a intercambiar o actualizar a un nuevo dispositivo, se recomienda restablecer el dispositivo después de que haya recibido el nuevo dispositivo.
> - El dispositivo restablecer solo elimina todos los datos locales, desconecte el dispositivo. No se elimina de los datos que se están en la nube y recopila [cargos](https://azure.microsoft.com/pricing/details/storage/). Estos datos deben eliminarse por separado mediante una herramienta de administración de almacenamiento en la nube como [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="open-a-support-ticket"></a>Abra una incidencia de soporte técnico

Para comenzar el proceso de devolución, realice los pasos siguientes.

1. Abra una incidencia de soporte técnico con Microsoft Support que indica que desea devolver el dispositivo. Seleccione el tipo de problema como **datos de cuadro de borde Hardware**.

    ![Abrir la incidencia de soporte técnico](media/data-box-edge-return-device/open-support-ticket-1.png)  

2. Un ingeniero de Microsoft Support se comunicará con usted. Proporcione los detalles del trasvase de registros.
3. Si tiene un cuadro de trasvase de registros devuelto, puede solicitarla. Respuesta **Sí** a la pregunta **necesita un cuadro vacío para devolver**.


## <a name="schedule-a-pickup"></a>Programar una recogida

1. Apague el dispositivo. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Seleccione **apagar**. Cuando se le pida confirmación, haga clic en **Sí** para continuar. Para obtener más información, consulte [administrar power](data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. Desconecte los cables de alimentación y quite todos los cables de red del dispositivo.
4. Preparar el paquete de envío mediante su propio cuadro o el cuadro vacío que recibió de Azure. Coloque el dispositivo y los cables de alimentación que se enviaron con el dispositivo en el cuadro.
5. Colocar la etiqueta de envío que ha recibido de Azure en el paquete.
6. Programar una recogida con su operador regional. Si el dispositivo vuelve de la zona horaria del Pacífico, el operador es SAI (UPS). Para programar una recogida:

    1. Llame a la oficina local de UPS (número gratuito específico del país).
    2. En la llamada, el número, tal como se muestra en la etiqueta impresa de seguimiento del envío inverso entre comillas.
    3. Si el número de seguimiento no está entre comillas, SAI (UPS), deberá pagar un costo adicional durante la recogida.

    En lugar de programar la recogida, también puede quitar fuera del borde del cuadro de datos en la ubicación de recogida más cercana.

## <a name="delete-the-resource"></a>Elimine el recurso

Una vez recibido el dispositivo del centro de datos de Azure, el dispositivo se inspecciona para daños o signos de manipulación.

- Si el dispositivo llega intactos y en buen estado, el medidor de facturación se detiene para ese recurso. Microsoft Support se comunicará con usted para confirmar que se devolvió el dispositivo. A continuación, puede eliminar el recurso asociado con el dispositivo en el portal de Azure.
- Si el dispositivo llega significativamente dañado, pueden aplicar multas. Para obtener más información, consulte el [preguntas más frecuentes sobre el dispositivo perdido o dañado](https://azure.microsoft.com/pricing/details/databox/edge/) y [condiciones del servicio de producto](https://www.microsoft.com/licensing/product-licensing/products).  


Puede eliminar el dispositivo en el portal de Azure:
-   Después de realizar el pedido y antes de que el dispositivo está preparado por Microsoft.
-   Tras devolver el dispositivo a Microsoft, pasa las inspecciones físicas del centro de datos de Azure y llama Microsoft Support para confirmar que se devolvió el dispositivo.

Si ha activado el dispositivo con otra suscripción o ubicación, Microsoft moverá el pedido a la nueva suscripción o ubicación dentro de un día laborable. Después de mover el orden, puede eliminar este recurso.


Siga estos pasos para eliminar el dispositivo y el recurso en Azure portal.

1. En el portal de Azure, vaya al recurso de y, a continuación, en **Introducción**. En la barra de comandos, seleccione **eliminar**.

    ![Seleccionar Eliminar](media/data-box-edge-return-device/delete-resource-1.png)

2. En el **Eliminar dispositivo** hoja, escriba el nombre del dispositivo que desea eliminar y seleccione **eliminar**.

    ![Confirmar eliminación](media/data-box-edge-return-device/delete-resource-2.png)

Se le notificará una vez el dispositivo y el recurso asociado se eliminó correctamente.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](data-box-edge-manage-bandwidth-schedules.md).
