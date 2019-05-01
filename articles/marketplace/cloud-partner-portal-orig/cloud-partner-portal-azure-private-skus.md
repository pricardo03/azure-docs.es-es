---
title: SKU privado y planes | Azure Marketplace
description: Cómo usar las SKU privadas para administrar la disponibilidad de la oferta.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 6efdb1c28777d9230727066fdba03d2850be62b0
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935919"
---
<a name="private-skus-and-plans"></a>SKU y planes privados
============

Las SKU privadas le permiten restringir la disponibilidad de las SKU a clientes específicos. Cuando una SKU se marca como privada, no está disponible en ningún catálogo público, incluidos [Azure Marketplace](https://azuremarketplace.microsoft.com) y [Azure Portal](https://portal.azure.com). En Azure Portal, solo los clientes con acceso a la SKU pueden verla. Además, también se les puede exigir que tengan acceso a las ofertas privadas.

>[!NOTE]
>Las SKU privadas deben tener identificadores de SKU o plan únicos para evitar conflictos con sus SKU públicas.

Puede usar las SKU privadas para controlar los escenarios siguientes:

1.  Publique software que quiera que esté disponible públicamente solo para clientes específicos y no para el público general.
2.  Publique las variaciones de software público a un precio personalizado para clientes específicos.
3.  Publique las variaciones de software público con condiciones y con una descripción personalizadas (a través de una oferta nueva).

Si solo desea quiere cambiar el precio, puede volver a usar los discos de otra SKU en la misma oferta. Con las SKU privadas, no tiene que volver a enviar discos en todas las SKU.

<a name="mark-a-sku-private"></a>Marcar una SKU como privada
---------------------

Para marcar una SKU como privada, active o desactive la opción que le pregunta si la SKU es privada:

![Marcar una SKU como privada](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

Puede volver a usar los discos en otra SKU y modificar los precios o la descripción. Para volver a usar los discos, seleccione **Sí** como respuesta al mensaje que pregunta si la SKU reutiliza las imágenes de una SKU pública.

Si la SKU está marcada como privada y la oferta tiene otras SKU con discos reutilizables, deberá indicar que la SKU reutiliza los discos de otra SKU. También es necesario especificar la audiencia de destino de la SKU privada.

>[!NOTE]
>Una vez publicada, una SKU pública no puede convertirse en privada.

<a name="select-an-image"></a>Seleccionar una imagen
------------------

Puede proporcionar nuevos discos para la SKU privada o volver a usar los mismos discos proporcionados en otra SKU, y modificar solo los precios o la descripción. Para volver a usar los discos, seleccione **Sí** como respuesta al mensaje que pregunta si la SKU reutiliza las imágenes de una SKU pública.

![Indicar la reutilización de imágenes](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

Después de confirmar que la SKU reutiliza las imágenes de otra SKU, deben identificar la SKU de origen de las imágenes.

Los mensajes de la siguiente captura de pantalla muestran cómo identificar si la SKU privada volvería a usar las imágenes de la SKU seleccionada:

![Seleccionar una imagen](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Al publicar la oferta, las imágenes de la SKU seleccionada estarán disponibles en el identificador de la SKU privada con las tarifas y condiciones personalizadas. La SKU privada solo será visible para la audiencia de destino.

Para las actualizaciones de imágenes, solo tendrá que actualizar la imagen de la SKU subyacente. En segundo plano, la imagen de la SKU privada también se actualizará automáticamente. De forma similar, si elimina la imagen de la SKU subyacente, esta también se quitará de la SKU privada.

<a name="restricting-the-audience"></a>Restringir la audiencia
------------------------

Las ofertas privadas solo las pueden buscar e implementar los usuarios de destino.
Actualmente, permitimos dirigirse a los usuarios a través de los identificadores de suscripción.

Estas suscripciones se pueden introducir a través de un formulario de entrada manual con un límite **de hasta 10 suscripciones**, o bien mediante la carga de un archivo CSV, que permite **hasta 20 000 suscripciones**.

Entrada manual para un público restringido:

![Restringir la audiencia manualmente](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

Carga de CSV para un público restringido:

![Usar un archivo CSV para restringir la audiencia](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Contenido del archivo CSV de ejemplo:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Al cambiar de la vista de entrada manual a la de carga de CSV o de la entrada de CSV a la entrada manual, no se conserva la lista antigua de identificadores de suscripción con acceso a la SKU. Se muestra una advertencia y solo se sobrescribe la lista al guardar la oferta.

<a name="sync-private-subscriptions"></a>Suscripciones de sincronización privada
-------------------------

Al agregar suscripciones a una oferta publicada con una SKU o plan privados, no es necesario volver a publicar la oferta para agregar información de la audiencia. Simplemente use un id. de suscripción de Azure (planes y SKU) o el id. de inquilino (solo planes) para agregar la audiencia.

<a name="previewing-private-offers"></a>Obtener una vista previa de las ofertas privadas
-------------------------

Durante el paso de previsualización o almacenamiento provisional, solo podrán acceder a la SKU las suscripciones de versión preliminar de nivel de la oferta. Esta es la fase de pruebas durante la cual puede validar el aspecto de la oferta para sus clientes de destino. Esta fase es estándar para todos los tipos de publicación.

Suscripciones de versión preliminar de nivel de la oferta para acceder a ofertas almacenadas temporalmente:

![Preview Subscription Ids (Id. de suscripción de versión preliminar)](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Cuando la oferta esté activa, solo las suscripciones de audiencia restringida (especificadas mediante entrada manual o CSV) podrán ver e implementar la SKU privada. Se recomienda **incluir siempre sus propias suscripciones en la audiencia restringida** para la SKU privada con fines de validación.

>[!NOTE]
>Con fines de depuración, los equipos de soporte técnico e ingeniería de Microsoft también tendrán acceso a estas ofertas privadas.
