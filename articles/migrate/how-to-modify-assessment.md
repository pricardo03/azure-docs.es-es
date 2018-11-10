---
title: Personalización de la configuración de evaluación de Azure Migrate | Microsoft Docs
description: Describe cómo configurar y ejecutar una evaluación para migrar máquinas virtuales de VMware a Azure con Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/30/2018
ms.author: raynew
ms.openlocfilehash: d0cfab51b686b5b6eb9617d4424ac3f834de8d6f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241079"
---
# <a name="customize-an-assessment"></a>Personalización de una evaluación

[Azure Migrate](migrate-overview.md) crea evaluaciones con propiedades predeterminadas. Después de crear una evaluación, puede modificar las propiedades predeterminadas siguiendo las instrucciones de este artículo.


## <a name="edit-assessment-properties"></a>Editar propiedades de evaluación

1. En la página **Evaluaciones** del proyecto de migración, seleccione la evaluación y haga clic en **Editar propiedades**.
2. Personalice las propiedades de evaluación en función de los detalles siguientes:

    **Configuración** | **Detalles** | **Valor predeterminado**
    --- | --- | ---
    **Ubicación de destino** | La ubicación de Azure a la que desea realizar la migración.<br/><br/> Azure Migrate admite actualmente 30 regiones entre las que se incluyen: Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, India central, Centro de EE. UU., Este de China, Norte de China, Asia Oriental, Este de EE. UU., Centro de Alemania, Noreste de Alemania, Este de EE. UU. 2, Japón Oriental, Japón Occidental, Centro de Corea del Sur, Corea del Sur, Centro y norte de EE. UU., Europa del Norte, Centro y sur de EE. UU., Sudeste Asiático, India del Sur, Sur del Reino Unido, Oeste del Reino Unido, US Gov Arizona, US Gov Texas, US Gov Virginia, Centro occidental de EE. UU., Europa Occidental, India occidental, Oeste de EE. UU. y Oeste de EE. UU. 2. |  Oeste de EE. UU. 2 es la ubicación predeterminada.
    **Plan de tarifa** | Puede especificar el [plan de tarifa (Básico o Estándar)](../virtual-machines/windows/sizes-general.md) de las máquinas virtuales de Azure de destino. Por ejemplo, si va a migrar un entorno de producción, debería tener en cuenta el plan Estándar. Por otro lado, si tiene un entorno de desarrollo y pruebas, quizá debería considerar el plan Básico que tiene máquinas virtuales con una latencia mayor y un costo más bajo. | De forma predeterminada se usa el plan [Estándar](../virtual-machines/windows/sizes-general.md).
    **Tipo de almacenamiento** | Puede usar esta propiedad para especificar el tipo de discos que quiera asignar en Azure. Para los tamaños locales, puede especificar el tipo de disco de destino como discos administrados Premium o Estándar. Para tamaños basados en rendimiento, puede especificar el tipo de disco de destino como automático o como discos administrados Premium o Estándar. Cuando se especifica el tipo de almacenamiento como automático, la recomendación de disco se realiza basándose en los datos de rendimiento de los discos (IOPS y rendimiento). Por ejemplo, si desea conseguir un [Acuerdo de Nivel de Servicio de máquina virtual de única instancia del 99,9 %](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), es posible que quiera especificar el tipo de almacenamiento como discos administrados Premium ya que este garantizará que todos los discos de la evaluación se recomienden como discos administrados Premium. Tenga en cuenta que Azure Migrate solo admite discos administrados para la valoración de la migración. | El valor predeterminado es Managed Disks Premium (con el criterio de ajuste de tamaño *como local*).
    **Instancias reservadas** |  También puede especificar si tiene [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en Azure y Azure Migrate estimará el costo en función de ello. Las instancias reservadas no son aplicables a regiones soberanas (Azure Government, Alemania y China) y solo son aplicables en la oferta de pago por uso de Azure Migrate. | El valor predeterminado de esta propiedad es instancias reservadas de 3 años.
    **Criterio de ajuste de tamaño** | El criterio que debe utilizar Azure Migrate para ajustar el tamaño de las máquinas virtuales para Azure. Puede hacer bien un ajuste de tamaño *basado en el rendimiento* o puede ajustar el tamaño de las máquinas virtuales *como en el entorno local*, sin tener en cuenta el historial de rendimiento. | El ajuste de tamaño basado en el rendimiento es el valor predeterminado.
    **Historial de rendimiento** | La duración a tener en cuenta para evaluar el rendimiento de las máquinas virtuales. Esta propiedad solo es aplicable cuando el criterio es el *ajuste de tamaño basado en el rendimiento*. | El valor predeterminado es un día.
    **Uso de percentil** | El valor de percentil del ejemplo de rendimiento establecido para determinar el tamaño adecuado. Esta propiedad solo es aplicable cuando el criterio es el *ajuste de tamaño basado en el rendimiento*.  | Valor predeterminado es percentil 95.
    **Series de VM** | Puede especificar la series de VM que quiera tener en cuenta para determinar el tamaño adecuado. Por ejemplo, si tiene un entorno de producción que no vaya a migrar a la serie A de las máquinas virtuales de Azure, puede excluir la serie A de la lista o serie, y el ajuste de tamaño correcto se realizará solo en la serie seleccionada. | De forma predeterminada, se seleccionan todas las series de máquinas virtuales.
    **Factor de confort** | Azure Migrate tiene en cuenta un búfer (factor de confort) durante la evaluación. Dicho búfer se aplica además de los datos de uso de la máquina en las máquinas virtuales (CPU, memoria, disco y red). El factor de confort se tiene en cuenta en problemas como el uso estacional, un historial de rendimiento corto y los posibles aumentos en el uso futuro.<br/><br/> Por ejemplo, una máquina virtual de 10 núcleos con un uso del 20 % normalmente genera una máquina virtual de 2 núcleos. Sin embargo, con un factor de confort de 2.0 x, el resultado es una máquina virtual de 4 núcleos. | La configuración predeterminada es 1.3x.
    **Oferta** | [Oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) en la que esté inscrito. | [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) es el valor predeterminado.
    **Moneda** | Divisa de facturación. | El valor predeterminado es el dólar estadounidense.
    **Descuento (%)** | Cualquier descuento específico de la suscripción que reciba además de la oferta de Azure. | La configuración predeterminada es 0 %.
    **Tiempo de actividad de VM** | Si las máquinas virtuales no se van a ejecutar todas las horas en Azure, puede especificar la duración (número de días al mes y número de horas al día) del período en el que se estarán ejecutando y las estimaciones de costo se realizarán en consecuencia. | El valor predeterminado es 31 días al mes y 24 horas al día.
    **Ventaja híbrida de Azure** | Especifique si dispone de Software Assurance y tiene derecho a [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Si se establece en Sí, los precios de Azure que no son de Windows se toman en cuenta para las máquinas virtuales de Windows. | El valor predeterminado es Yes.

3. Haga clic en **Guardar** para actualizar la evaluación.


## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
