---
title: Administración y solicitud de cuotas del servicio Azure Machine Learning
description: En esta guía se explican las distintas cuotas de los recursos de Azure Machine Learning y cómo ver y solicitar una cuota mayor.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 9/24/2018
ms.openlocfilehash: 0ee0a29be6a313e09cd77051dc29bc0131b57a40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997818"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Administración y solicitud de cuotas para recursos de Azure

Al igual que en otros servicios de Azure, existen límites en determinados recursos asociados con el servicio Azure Machine Learning. Dichos límites oscilan entre un tope en el número de áreas de trabajo que se pueden crear a límites en el proceso subyacente real que se usa para entrenar o inferior modelos. En este artículo se proporciona más información acerca de los límites previamente configurados de varios recursos de Azure de su suscripción. También encontrará vínculos útiles para solicitar mejoras de la cuota de todos los tipos de recurso.

Tenga en cuenta estas cuotas al diseñar y escalar verticalmente sus recursos de Azure Machine Learning. Por ejemplo, si su clúster no alcanza el número objetivo de nodos especificado, es posible que se haya alcanzado el límite de núcleos de Batch AI de la suscripción. Si planea ejecutar cargas de trabajo de producción en Batch AI, es posible que tenga que aumentar el valor predeterminado de una o varias de las cuotas. Si desea aumentar el límite o la cuota por encima del límite predeterminado, abra una solicitud de soporte técnico al cliente en línea sin cargo alguno. Los límites no se pueden subir por encima del valor de Límite máximo que se muestra en las tablas siguientes. Si la columna Límite máximo no existe, el recurso no tiene límites ajustables. 

## <a name="special-considerations"></a>Consideraciones especiales

+ Una cuota es un límite de crédito, no una garantía de capacidad. Si tiene necesidades de capacidad a gran escala, póngase en contacto con el soporte técnico de Azure.

+ La cuota se comparte entre todos los servicios, excepto en el caso de Batch AI, de su suscripción, y Azure Machine Learning es uno de ellos. Al evaluar las necesidades de capacidad, asegúrese de calcular el uso de la cuota entre todos los servicios.

+ Los límites predeterminados varían según el tipo de categoría de la oferta, por ejemplo, evaluación gratuita, pago por uso y series, como Dv2, F, G, etc.

## <a name="default-resource-quotas"></a>Cuotas de recursos predeterminadas

Este es un desglose de los límites de cuota de los distintos tipos de recursos de su suscripción de Azure. 

> [!Important]
> Los límites están sujetos a cambios. El más reciente siempre se puede encontrar en el [documento](https://docs.microsoft.com/azure/azure-subscription-service-limits/) de la cuota de nivel de servicio para todo Azure.

### <a name="virtual-machines"></a>Máquinas virtuales 
Hay un límite en el número de máquinas virtuales que se pueden aprovisionar en una suscripción de Azure mediante los servicios, o bien de forma independiente. Dicho límite se produce a nivel de región tanto en el total de núcleos como por familia.

Es importante destacar que los núcleos de las máquinas virtuales tienen un límite regional total y un límite de serie por tamaño (Dv2, F, etc.), y ambos límites se aplican por separado. Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción podría volver a implementar 30 máquinas virtuales A1, 30 máquinas virtuales D1 o una combinación de las dos, para que no se superen los 30 núcleos (por ejemplo, 10 máquinas virtuales A1 y 20 máquinas virtuales D1).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Para obtener una lista más detallada y actualizada de los límites de cuota, consulte [aquí](https://docs.microsoft.com/azure/azure-subscription-service-limits#subscription-limits-1) el artículo de la cuota en todo Azure.

### <a name="batch-ai-clusters"></a>Clústeres de Batch AI
En Batch AI, hay un límite de cuota predeterminado tanto en el número de núcleos como en el número de clústeres que se permiten por región en una suscripción. La cuota de Batch AI es independiente de la cuota de los núcleos de las máquinas virtuales anterior y actualmente los límites de los núcleos no se comparten entre los dos tipos de recursos.

Recursos disponibles:
+ Los núcleos dedicados por región tienen un límite predeterminado de 10 a 24.  El número de núcleos dedicados por suscripción de Batch AI se puede aumentar. Póngase en contacto con el soporte técnico de Azure para analizar las opciones de aumento.

+ Los núcleos de baja prioridad por región tienen un límite predeterminado de 10 a 24.  El número de núcleos de baja prioridad por suscripción de Batch AI se puede aumentar. Póngase en contacto con el soporte técnico de Azure para analizar las opciones de aumento.

+ El límite predeterminado de clústeres por región es de 20, mientras que el máximo es 200. Póngase en contacto con el soporte técnico de Azure si desea solicitar un número que supere este límite.

Para obtener una lista más detallada y actualizada de los límites de cuota, consulte [aquí](https://docs.microsoft.com/azure/azure-subscription-service-limits#batch-ai-limits) el artículo de la cuota en todo Azure.

### <a name="container-instances"></a>Instancias de contenedor

También hay un límite en el número de instancias de contenedor que se pueden poner en marcha en un período de tiempo determinado (el ámbito es una hora) o en toda la suscripción.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Para obtener una lista más detallada y actualizada de los límites de cuota, consulte [aquí](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits) el artículo de la cuota en todo Azure.

### <a name="storage"></a>Storage
Hay un límite en el número de cuentas de almacenamiento por región, así como en una suscripción determinada. Dicho límite es de 200 e incluye las cuentas Estándar y Premium Storage. Si necesita más de 200 cuentas de almacenamiento en una región dada, realice una solicitud a través del [servicio de soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). El equipo de Azure Storage revisará su caso empresarial y podría aprobar hasta 250 cuentas de almacenamiento para una región dada.


## <a name="find-your-quotas"></a>Búsqueda de cuotas

Mediante Azure Portal es fácil ver la cuota de varios recursos, como las máquinas virtuales, el almacenamiento la red.

1. En el panel izquierdo, seleccione **Todos los servicios** y, después, seleccione **Suscripciones** en la categoría General.

1. En la lista de suscripciones, seleccione la suscripción cuya cuota busca.

   **Hay una salvedad**, en concreto no se puede ver la cuota de Batch AI. Como ya se ha indicado, dicha cuota es independiente de la cuota de proceso de su suscripción. 
   En el caso de Batch AI, después de seleccionar **Todos los servicios**, busque Batch AI y ábralo.

1. En **Configuración**, seleccione **Uso y cuotas** para ver los límites y el uso de su cuota actual.

1. Seleccione su suscripción para ver los límites de cuota. No olvide filtrar por el servicio y la región en que esté interesado.


## <a name="request-quota-increases"></a>Solicitud de aumento de cuota

Si desea aumentar el límite o la cuota por encima del límite predeterminado, [abra una solicitud de soporte técnico al cliente en línea](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) sin cargo alguno. 

Los límites no se pueden subir por encima del valor del límite máximo que se muestra en las tablas siguientes. Si no existe dicho límite, significa que el recurso no tiene límites ajustables. En [este](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) artículo se trata el proceso de aumento de cuota con más detalle.

Al solicitar un aumento de cuota, es preciso seleccionar el servicio para el que se realiza la solicitud, que puede ser un servicio como cuota de Azure Machine Learning, cuota de Batch AI o cuota de Storage. 

> [!NOTE]
> Las [suscripciones de evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p) no son aptas para aumentar el límite ni la cuota. Si tiene una [suscripción de evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p), puede actualizar a una suscripción de [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/). Para obtener más información, consulte [Actualización de evaluación gratuita de Azure a pago por uso](../../billing/billing-upgrade-azure-subscription.md) y [Free Trial subscription FAQ](https://azure.microsoft.com/free/free-account-faq) (Preguntas más frecuentes de la suscripción de evaluación gratuita).
