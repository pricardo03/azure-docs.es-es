---
title: Supervisión y seguimiento del uso de los servicios gratuitos de Azure | Microsoft Docs
description: Aprenda a comprobar el uso de los servicios gratuitos. Uso de Azure Portal y CSV de uso.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: banders
ms.openlocfilehash: 4e940a12cd57ef136cfd9ead298f9afcd2d6ad1f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849863"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Comprobación del uso de los servicios gratuitos incluidos con la cuenta gratuita de Azure 

No se le cobrará por los servicios incluidos de forma gratuita con la cuenta gratuita de Azure, a menos que se superen los límites de estos servicios. Para no superar los límites, puede usar Azure Portal o el archivo de uso para realizar la supervisión y el seguimiento del uso de los servicios gratuitos. 

## <a name="check-usage-on-the-azure-portal"></a>Comprobación del uso de Azure Portal

1.  Inicie sesión en el [Azure Portal](https://portal.azure.com).

2.  En el área de navegación izquierda, seleccione **Todos los servicios**.

3.  Seleccione **Suscripciones**.

4.  Seleccione la suscripción que creó cuando se registró para la cuenta gratuita.

    ![Captura de pantalla que muestra todas las suscripciones](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  La sección de información general muestra información esencial sobre la suscripción, como el identificador de suscripción, el tipo de oferta y el nombre de la suscripción. También puede encontrar información sobre cuándo expirará el crédito de la cuenta gratuita.

    ![Captura de pantalla que muestra información esencial de suscripción](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Desplácese hacia abajo para buscar información sobre el costo actual y previsto. El costo incluye el uso de servicios no incluidos con su cuenta gratuita y el uso que supera los límites de los servicios gratuitos. 

    ![Captura de pantalla que muestra información de costos de suscripción](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  En la parte final de la sección de información general hay una tabla sobre el uso de los servicios gratuitos. 

    ![Captura de pantalla que muestra el uso de los servicios gratuitos](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    La tabla tiene las columnas siguientes.

* **Nombre de medidor:** Identifica la unidad de medida del medidor que se está utilizando. Para más información sobre la asignación de servicios a medidores, consulte [Descripción de la asignación de servicios a medidores](billing-understand-free-service-meter-mapping.md).
* **Uso y límite:** uso y límite del mes actual para el medidor. También puede encontrar esta información en la barra de estado.
* **Estado:** estado de uso del medidor. Según el patrón de uso, puede tener uno de estos estados.
  * **No está en uso:** no ha usado el medidor o el uso de este no llega al sistema de facturación.
  * **Superado en \<fecha>:** ha superado el límite del medidor el \<Fecha>.
  * **Superación improbable:** es poco probable que supere el límite del medidor.
  * **Lo supera en \<fecha >:** es probable que supere el límite del medidor en \<Fecha >.

## <a name="check-usage-through-the-usage-file"></a>Comprobación del uso a través del archivo de uso

El archivo de uso proporciona información detallada para la suscripción de Azure. Puede descargar el archivo de uso mensual y diariamente del Centro de cuentas de Azure. Para información sobre cómo descargar el archivo de uso y comprender el acceso necesario, consulte [Procedimiento para descargar los datos de uso diario y de factura de Azure](billing-download-azure-invoice-daily-usage-date.md). Para información sobre las columnas del archivo de uso, consulte [Descripción de los términos de los cargos de uso detallados de Microsoft Azure](billing-understand-your-usage.md).

El archivo de uso contiene información de uso de servicios gratuitos y de pago. Los medidores de servicio gratuito tendrían la palabra **Gratuito** anexada al final del nombre del medidor. Para buscar medidores gratuitos, abra el archivo en Excel y filtre la **columna Categoría del medidor** para obtener las celdas que tienen el texto **- Gratuito** (utilice el filtro Filtros de texto &rarr; Contiene)&nbsp;

![Captura de pantalla que muestra el uso de los servicios gratuitos](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).