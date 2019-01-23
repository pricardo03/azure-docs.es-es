---
title: Personalización de la configuración de evaluación de Azure Migrate | Microsoft Docs
description: Describe cómo configurar y ejecutar una evaluación para migrar máquinas virtuales de VMware a Azure con Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/10/2019
ms.author: raynew
ms.openlocfilehash: 8419d7e7a91e4cbfd0eebfe00d35bf498cf5998c
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200316"
---
# <a name="customize-an-assessment"></a>Personalización de una evaluación

[Azure Migrate](migrate-overview.md) crea evaluaciones con propiedades predeterminadas. Después de crear una evaluación, puede modificar las propiedades predeterminadas siguiendo las instrucciones de este artículo.


## <a name="edit-assessment-properties"></a>Editar propiedades de evaluación

1. En la página **Evaluaciones** del proyecto de migración, seleccione la evaluación y haga clic en **Editar propiedades**.
2. Personalice las propiedades de evaluación en función de los detalles siguientes:

    **Configuración** | **Detalles** | **Valor predeterminado**
    --- | --- | ---
    **Ubicación de destino** | La ubicación de Azure a la que desea realizar la migración.<br/><br/> Azure Migrate admite actualmente 30 regiones entre las que se incluyen: Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, India central, Centro de EE. UU., Este de China, Norte de China, Asia Oriental, Este de EE. UU., Centro de Alemania, Noreste de Alemania, Este de EE. UU. 2, Japón Oriental, Japón Occidental, Centro de Corea del Sur, Corea del Sur, Centro y norte de EE. UU., Europa del Norte, Centro y sur de EE. UU., Sudeste Asiático, India del Sur, Sur del Reino Unido, Oeste del Reino Unido, US Gov Arizona, US Gov Texas, US Gov Virginia, Centro occidental de EE. UU., Europa Occidental, India occidental, Oeste de EE. UU. y Oeste de EE. UU. 2. |  Oeste de EE. UU. 2 es la ubicación predeterminada.
    **Tipo de almacenamiento** | Puede usar esta propiedad para especificar el tipo de discos a los que desea migrar en Azure. Para los tamaños locales, puede especificar el tipo de disco de destino como discos administrados Premium o Estándar. Para tamaños basados en rendimiento, puede especificar el tipo de disco de destino como automático o como discos administrados Premium o Estándar. Cuando se especifica el tipo de almacenamiento como automático, la recomendación de disco se realiza basándose en los datos de rendimiento de los discos (IOPS y rendimiento). Por ejemplo, si desea conseguir un [Acuerdo de Nivel de Servicio de máquina virtual de instancia única del 99,9 %](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), es posible que desee especificar el tipo de almacenamiento como discos administrados prémium. Esto garantiza que todos los discos de la evaluación se recomienden como discos administrados prémium. Tenga en cuenta que Azure Migrate solo admite discos administrados para la valoración de la migración. | El valor predeterminado es discos administrados prémium (con el criterio de ajuste de tamaño *como local*).
    **Instancias reservadas** |  También puede especificar si tiene [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en Azure y Azure Migrate estimará el costo en función de ello. Las instancias reservadas solo se admiten actualmente para la oferta de pago por uso en Azure Migrate. | El valor predeterminado de esta propiedad es instancias reservadas de 3 años.
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

## <a name="faqs-on-assessment-properties"></a>Preguntas frecuentes sobre las propiedades de evaluación

### <a name="what-is-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>¿Cuál es la diferencia entre el ajuste de tamaño como local y el ajuste de tamaño basado en el rendimiento?

Al especificar el criterio de ajuste de tamaño como local, Azure Migrate no tiene en cuenta los datos de rendimiento de las máquinas virtuales y ajusta el tamaño de las máquinas virtuales en función de la configuración local. Si el criterio de ajuste de tamaño se basa en el rendimiento, el ajuste se realiza basándose en datos de uso. Por ejemplo, si hay una máquina virtual local con 4 núcleos y 8 GB de memoria con un 50 % de uso de CPU y un 50 % de uso de memoria. Si el criterio de ajuste de tamaño es como local, se recomienda ajustar el tamaño de una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria. Sin embargo, si el criterio de ajuste de tamaño se basa en el rendimiento, se recomienda una SKU de máquina virtual de 2 núcleos y 4 GB, ya que el porcentaje de uso se tiene en cuenta al recomendar el tamaño.

De forma similar, en el caso de los discos, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento. Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, los valores de capacidad de proceso y de IOPS del disco se tendrán en cuenta para identificar el tipo de disco de destino (estándar o prémium). Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es prémium, se recomienda un disco prémium. La SKU de disco prémium en Azure se selecciona en función del tamaño del disco local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o prémium.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y uso de percentil en las recomendaciones de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento. Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure.

- La aplicación del recopilador realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos.
- El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos y lo envía a Azure.
- Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño.

Por ejemplo, si ha establecido la duración del rendimiento en 1 día y el valor del percentil en el percentil 95, Azure Migrate usa puntos de ejemplo de 15 min que envía el recopilador para el último día, los ordena de manera ascendente y elige el valor del percentil 95 como el uso efectivo. El valor del percentil 95 garantiza que se van a omitir los valores atípicos que pueden aparecer si elige el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, debe seleccionar el percentil 99.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
