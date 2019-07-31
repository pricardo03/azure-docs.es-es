---
title: Cálculos de evaluación de Azure Migrate | Microsoft Docs
description: En este artículo se proporciona una introducción a los cálculos de evaluación del servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 87aa48c992b7887ce86c43cac29910dcc57b3e91
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234288"
---
# <a name="assessment-calculations"></a>Cálculos de evaluación

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de instancias en la nube privadas o públicas. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).

Server Assessment es una herramienta de Azure Migrate que evalúa los servidores locales para la migración a Azure. En este artículo se proporciona información sobre cómo se calculan las evaluaciones.

## <a name="whats-in-an-assessment"></a>¿Qué es una evaluación?

**Propiedad** | **Detalles**
--- | ---
**Ubicación de destino** | La ubicación de Azure a la que desea realizar la migración.<br/> Server Assessment admite actualmente estas regiones de destino: Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, India central, Centro de EE. UU., Este de China, Norte de China, Asia Oriental, Este de EE. UU., Este de EE. UU. 2, Centro de Alemania, Noreste de Alemania, Japón Oriental, Japón Occidental, Centro de Corea del Sur, Sur de Corea del Sur, Centro y norte de EE. UU., Europa del Norte, Centro y sur de EE. UU., Sudeste Asiático, India del Sur, Sur de Reino Unido, Oeste de Reino Unido, US Gov Arizona, US Gov Texas, US Gov Virginia, Centro occidental de EE. UU., Europa Occidental, India occidental, Oeste de EE. UU. y Oeste de EE. UU. 2.
**Tipo de almacenamiento** | Puede usar esta propiedad para especificar el tipo de discos a los que desea migrar en Azure. <br/><br/> Para los tamaños locales, puede especificar el tipo de almacenamiento de destino como discos administrados premium, discos administrados SSD estándar o discos administrados HDD estándar. Para tamaños según el rendimiento, puede especificar el tipo de disco de destino como Automático, discos administrados premium, discos administrados HDD estándar o discos administrados SSD estándar.<br/><br/> Cuando se especifica el tipo de almacenamiento como automático, la recomendación de disco se realiza basándose en los datos de rendimiento de los discos (IOPS y rendimiento). Si especifica el tipo de almacenamiento como premium/estándar, la evaluación recomendará una SKU de disco dentro del tipo de almacenamiento seleccionado. Si desea conseguir un Acuerdo de Nivel de Servicio de máquina virtual de instancia única del 99,9 %, es posible que desee especificar el tipo de almacenamiento como discos administrados premium. Esto garantiza que todos los discos de la evaluación se recomienden como discos administrados prémium. Tenga en cuenta que Azure Migrate solo admite discos administrados para la valoración de la migración.
**Instancias reservadas (RI)** | Esta propiedad le ayuda a especificar si tiene [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en Azure, entonces, las estimaciones de costos en la evaluación se realizan teniendo en cuenta los descuentos de instancias reservadas. Las instancias reservadas solo se admiten actualmente para la oferta de pago por uso en Azure Migrate.
**Criterio de ajuste de tamaño** | El criterio que se debe utilizar para ajustar el tamaño de las máquinas virtuales para Azure. Puede hacer un ajuste de tamaño *según el rendimiento* o puede ajustar el tamaño de las máquinas virtuales *como en el entorno local*, sin tener en cuenta el historial de rendimiento.
**Historial de rendimiento** | La duración que se debe tener en cuenta para evaluar los datos de rendimiento de las máquinas. Esta propiedad solo es aplicable cuando el criterio de ajuste de tamaño es *según el rendimiento*.
**Uso de percentil** | El valor de percentil del ejemplo de rendimiento establecido para determinar el tamaño adecuado. Esta propiedad solo es aplicable cuando el ajuste de tamaño es *según el rendimiento*.
**Series de VM** |     Puede especificar la series de VM que quiera tener en cuenta para determinar el tamaño adecuado. Por ejemplo, si tiene un entorno de producción que no vaya a migrar a la serie A de las máquinas virtuales de Azure, puede excluir la serie A de la lista o serie, y el ajuste de tamaño correcto se realizará solo en la serie seleccionada.
**Factor de confort** | Azure Migrate Server Assessment tiene en cuenta un búfer (factor de confort) durante la evaluación. Dicho búfer se aplica además de los datos de uso de la máquina en las máquinas virtuales (CPU, memoria, disco y red). El factor de confort se tiene en cuenta en problemas como el uso estacional, un historial de rendimiento corto y los posibles aumentos en el uso futuro.<br/><br/> Por ejemplo, una máquina virtual de 10 núcleos con un uso del 20 % normalmente genera una máquina virtual de 2 núcleos. Sin embargo, con un factor de confort de 2.0 x, el resultado es una máquina virtual de 4 núcleos.
**Oferta** | La [oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) en la que está inscrito. Azure Migrate calcula el costo en consecuencia.
**Moneda** | Divisa de facturación.
**Descuento (%)** | Cualquier descuento específico de la suscripción que reciba además de la oferta de Azure.<br/> La configuración predeterminada es 0 %.
**Tiempo de actividad de VM** | Si las máquinas virtuales no se van a ejecutar todas las horas en Azure, puede especificar la duración (número de días al mes y número de horas al día) del período en el que se estarán ejecutando y las estimaciones de costo se realizarán en consecuencia.<br/> El valor predeterminado es 31 días al mes y 24 horas al día.
**Ventaja híbrida de Azure** | Especifique si dispone de Software Assurance y tiene derecho a [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Si se establece en Sí, los precios de Azure que no son de Windows se toman en cuenta para las máquinas virtuales de Windows. El valor predeterminado es Yes.

## <a name="how-are-assessments-calculated"></a>¿Cómo se calculan las evaluaciones?

Una evaluación en Azure Migrate Server Assessment se calcula con los metadatos recopilados acerca de los servidores locales. El cálculo de la evaluación se realiza en tres fases; para cada servidor, el cálculo de la evaluación comienza con el análisis de idoneidad de Azure, seguido del ajuste de tamaño y, por último, una estimación del costo mensual. Un servidor solo se mueve a una fase posterior si aprueba la anterior. Por ejemplo, si un servidor no supera la comprobación de idoneidad de Azure, se marca como no adecuado para Azure, y el ajuste de tamaño y los costos no se calculan para el mismo.

## <a name="azure-suitability-analysis"></a>Análisis de idoneidad de Azure

No todas las máquinas locales son adecuadas para ejecutarse en Azure. Azure Migrate Server Assessment evalúa si las máquinas locales son adecuadas para la migración a Azure y clasifica las máquinas evaluadas en una de las siguientes categorías de idoneidad:
- **Preparada para Azure**: la máquina se puede migrar como está a Azure sin realizar ningún cambio. Se iniciará en Azure con compatibilidad completa con Azure.
- **Condicionalmente preparada para Azure**: la máquina puede iniciarse en Azure, pero no tendrá compatibilidad completa con Azure. Por ejemplo, una máquina con una versión anterior del sistema operativo Windows Server no se admite en Azure. Antes de migrar estas máquinas a Azure y seguir la guía de soluciones sugeridas en la valoración, debe tener cuidado para resolver los problemas de preparación antes de realizar la migración.
- **No preparada para Azure**: la máquina no se inicia en Azure. Por ejemplo, si una máquina local tiene asociado un disco con un tamaño superior a 64 TB, no se puede hospedar en Azure. Debe seguir las instrucciones de soluciones sugeridas en la valoración para solucionar el problema de preparación antes de migrar a Azure. La estimación del tamaño adecuado y del costo no se realiza en máquinas marcadas como no preparadas para Azure.
- **Preparación desconocida**: Azure Migrate no ha podido encontrar la preparación de la máquina debido a una recopilación de metadatos insuficiente en el entorno local.

Azure Migrate Server Assessment revisa las propiedades de la máquina y el sistema operativo invitado para identificar la preparación para Azure de la máquina local.

### <a name="machine-properties"></a>Propiedades de la máquina

Server Assessment revisa las siguientes propiedades de la máquina virtual local para identificar si se puede ejecutar en Azure.

**Propiedad** | **Detalles** | **Estado de la preparación para Azure**
--- | --- | ---
**Tipo de arranque** | Azure admite máquinas virtuales con el tipo de arranque de BIOS y no UEFI. | Condicionalmente preparada si el tipo de arranque es UEFI.
**Núcleos** | El número de núcleos de las máquinas debe ser igual o menor que el número máximo de núcleos (128) admitidos en una máquina virtual de Azure.<br/><br/> Si el historial de rendimiento está disponible, Azure Migrate tiene en cuenta los núcleos utilizados para la comparación. Si se especifica un factor de confort en la configuración de evaluación, el número de núcleos utilizados se multiplica por dicho factor de confort.<br/><br/> Si no hay ningún historial de rendimiento, Azure Migrate usa los núcleos asignados sin aplicar el factor de confort. | Preparada si es menor o igual a los límites.
**Memoria** | La memoria de la máquina debe tener un tamaño igual o menor que la memoria máxima (3892 GB en Azure M serie Standard_M128m&nbsp;<sup>2</sup>) permitida en una máquina virtual de Azure. [Más información](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Si hay algún historial de rendimiento disponible, Azure Migrate tiene en cuenta la memoria utilizada para la comparación. Si se especifica un factor de confort, la memoria utilizada se multiplica por dicho factor de confort.<br/><br/> Si no hay ningún historial, se utiliza la memoria asignada sin aplicar el factor de confort.<br/><br/> | Preparada si está dentro de los límites.
**Disco de almacenamiento** | El tamaño asignado de un disco debe ser de 32 TB o menos.<br/><br/> El número de discos conectados a la máquina debe ser 65 o menos, incluido el disco de sistema operativo. | Preparada si está dentro de los límites.
**Redes** | Una máquina debe tener 32 NIC o menos conectadas. | Preparada si está dentro de los límites.

### <a name="guest-operating-system"></a>Sistema operativo invitado
Junto con las propiedades de la máquina virtual, Azure Migrate Server Assessment examina el sistema operativo invitado de las máquinas para identificar si se puede ejecutar en Azure.

> [!NOTE]
> En el caso de las máquinas virtuales de VMware, Azure Migrate Server Assessment utiliza el sistema operativo especificado para la máquina virtual de vCenter Server para realizar el análisis del sistema operativo invitado. En el caso de las máquinas virtuales de Linux que se ejecutan en VMware, actualmente no se identifica la versión exacta del kernel del sistema operativo invitado.

Azure Migrate Server Assessment emplea la siguiente lógica para identificar la preparación para Azure en función del sistema operativo.

**Sistema operativo** | **Detalles** | **Estado de la preparación para Azure**
--- | --- | ---
Windows Server 2016 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2012 R2 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2012 y todos los Service Pack | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2008 R2 con todos los Service Pack | Azure proporciona compatibilidad completa.| Preparada para Azure
Windows Server 2008 (32 bits y 64 bits) | Azure proporciona compatibilidad completa. | Preparada para Azure
Windows Server 2003 y 2003 R2 | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un [contrato de soporte técnico personalizado (CSA)](https://aka.ms/WSosstatement) para obtener soporte técnico en Azure. | Condicionalmente preparada para Azure, considere la posibilidad de actualizar el sistema operativo antes de migrar a Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Estos sistemas operativos han alcanzado la fecha de finalización del soporte técnico, la máquina pude arrancar en Azure, pero Azure no proporciona ningún soporte técnico para el sistema operativo. | Condicionalmente preparada para Azure, se recomienda actualizar el sistema operativo antes de migrar a Azure.
Cliente de Windows 7, 8 y 10 | Azure solo proporciona compatibilidad con la [suscripción de Visual Studio](https://docs.microsoft.com/azure/virtual-machines/windows/client-images). | Condicionalmente preparada para Azure
Windows 10 Pro Desktop | Azure proporciona compatibilidad con [derechos de hospedaje multiinquilino.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Condicionalmente preparada para Azure
Windows Vista, XP Professional | Estos sistemas operativos han alcanzado la fecha de finalización del soporte técnico, la máquina pude arrancar en Azure, pero Azure no proporciona ningún soporte técnico para el sistema operativo. | Condicionalmente preparada para Azure, se recomienda actualizar el sistema operativo antes de migrar a Azure.
Linux | Azure aprueba estos [sistemas operativos Linux](../virtual-machines/linux/endorsed-distros.md). Otros sistemas operativos Linux pueden arrancar en Azure, pero se recomienda actualizar el sistema operativo a una versión aprobada antes de migrar a Azure. | Preparada para Azure si la versión está aprobada.<br/><br/>Condicionalmente preparada si la versión no se ha aprobado.
Otros sistemas operativos<br/><br/> Por ejemplo, Oracle Solaris, Apple Mac OS, FreeBSD, etc. | Azure no aprueba estos sistemas operativos. La máquina puede arrancar en Azure, pero Azure no proporciona ningún sistema operativo. | Condicionalmente preparada para Azure, se recomienda instalar un sistema operativo compatible antes de migrar a Azure.  
Sistema operativo especificado como **otro** en vCenter Server. | Azure Migrate no puede identificar el sistema operativo en este caso. | Preparación desconocida. Asegúrese de que el sistema operativo que se ejecuta dentro de la máquina virtual se admite en Azure.
Sistemas operativos de 32 bits | La máquina puede arrancar en Azure, pero Azure no puede proporcionar soporte técnico completo. | Condicionalmente preparada para Azure, considere la posibilidad de actualizar el sistema operativo de la máquina de un sistema operativo de 32 bits a uno de 64 bits antes de migrar a Azure.

## <a name="sizing"></a>Ajuste de tamaño

Una vez que un equipo está marcado como listo para Azure, Server Assessment realiza el ajuste de tamaño, lo que implica la identificación de la máquina virtual de Azure y de la SKU del disco adecuadas para la máquina virtual local. Las recomendaciones de tamaño varían en función de las propiedades de evaluación especificadas.

- Si la evaluación utiliza el *ajuste de tamaño según el rendimiento*, Azure Migrate tiene en cuenta el historial de rendimiento de la máquina para identificar el tamaño y el tipo de disco de la máquina virtual en Azure. Este método es especialmente útil si ha asignado en exceso la máquina virtual local, pero el uso es bajo, y desea ajustar adecuadamente el tamaño de la máquina virtual en Azure para ahorrar costos. Este método le ayudará a optimizar los tamaños durante la migración.
- Si no desea tener en cuenta los datos de rendimiento para el ajuste de tamaño de la máquina virtual y quiere llevar las máquinas virtuales locales a Azure tal y como están, puede especificar el criterio de ajuste de tamaño *como local* y Server Assessment ajustará entonces el tamaño de las máquinas virtuales en función de la configuración local sin tener en cuenta los datos de uso. En este caso, el ajuste de tamaño de disco se hará según el tipo de almacenamiento que especifique en las propiedades de evaluación (discos HDD estándar, discos SSD estándar o discos premium).

### <a name="performance-based-sizing"></a>Ajuste de tamaño según el rendimiento

Para realizar el ajuste de tamaño según el rendimiento, Azure Migrate Server Assessment comienza con los discos asociados a la máquina virtual, después los adaptadores de red y, a continuación, asigna una SKU de la máquina virtual de Azure en función de los requisitos de proceso de la máquina virtual local. El dispositivo de Azure Migrate realiza un perfil del entorno local para recopilar datos de rendimiento de la CPU, memoria, discos y adaptador de red.

**Recopilación de datos de rendimiento**

- En el caso de las máquinas virtuales de VMware, el dispositivo de Azure Migrate recopila un punto de ejemplo en tiempo real en cada intervalo de 20 segundos, en el caso de las máquinas virtuales de Hyper-V, el punto de ejemplo en tiempo real se recopila en cada intervalo de 30 segundos.
- A continuación, el dispositivo acumula los puntos de ejemplo recopilados cada 10 minutos y envía el valor máximo de los últimos 10 minutos a Azure Migrate Server Assessment.
- Azure Migrate Server Assessment almacena todos los puntos de ejemplo de 10 minutos para el último mes y, en función de las propiedades de evaluación especificadas para *Historial de rendimiento* y *Uso de percentil*, identifica el punto de datos adecuado que se debe usar para el ajuste de tamaño adecuado. Por ejemplo, si el historial de rendimiento está establecido en un día y el uso de percentil es 95, utiliza los puntos de ejemplo de cada 10 minutos para el último día, los clasifica en orden ascendente y selecciona el percentil 95 para el ajuste de tamaño adecuado.
- El valor anterior se multiplica luego por el factor de confort para obtener los datos de uso de rendimiento efectivos para cada métrica (uso de CPU, uso de memoria, IOPS de disco (lectura y escritura), rendimiento de disco (lectura y escritura), rendimiento de red (entrada y salida)) que el dispositivo recopila.
- Una vez que se identifica el valor de uso efectivo, se realiza el ajuste de tamaño de proceso, almacenamiento y red de la siguiente manera:

**Ajuste de tamaño de almacenamiento**: Azure Migrate intenta asignar cada disco asociado a la máquina a un disco en Azure.

> [!NOTE]
> Azure Migrate: Server Assessment admite solo discos administrados para la evaluación.

  - Las IOPS de lectura y escritura de un disco se suman para obtener el total de IOPS necesario, y de forma similar, los valores de rendimiento de lectura y escritura se suman para obtener el rendimiento total de cada disco.
  - Si ha especificado el tipo de almacenamiento como Automático, en función de los valores de rendimiento e IOPS efectivos, Azure Migrate Server Assessment identifica si el disco debe estar asignado a un disco HDD estándar, disco SSD estándar o disco premium en Azure. Si el tipo de almacenamiento se establece en HDD estándar/SSD/premium, intentará encontrar una SKU de disco en el tipo de almacenamiento seleccionado (discos HDD estándar/SSD/premium).
  - Si Server Assessment no puede encontrar un disco con los valores de rendimiento e IOPS necesarios, marca la máquina como no adecuada para Azure.
  - Si encuentra un conjunto de discos adecuados, selecciona aquellos que admiten la ubicación especificada en la configuración de evaluación.
  - Si hay varios discos aptos, selecciona el que tiene el costo más bajo.
  - Si los datos de rendimiento de cualquier disco no están disponibles, se usan los datos de configuración del disco (tamaño del disco) para buscar un disco SSD estándar en Azure.

**Ajuste de tamaño de red**: Azure Migrate Server Assessment intenta encontrar una máquina virtual de Azure que pueda admitir el número de adaptadores de red asociados a la máquina local y el rendimiento que requieren estos adaptadores de red.
    - Para obtener el rendimiento de red efectivo de la máquina virtual local, Server Assessment suma los datos transmitidos por segundo (MBps) fuera de la máquina (salida de red), en todos los adaptadores de red, y aplica el factor de confort. Este número se utiliza para buscar una máquina virtual de Azure que pueda cumplir el rendimiento de red requerido.
    - Junto con el rendimiento de la red, también tiene en cuenta si la máquina virtual de Azure puede admitir el número de adaptadores de red necesarios.
    - Si no hay datos de rendimiento de red disponibles, solo se tiene en cuenta el número de adaptadores de red para ajustar el tamaño de la máquina virtual.

**Ajuste de tamaño de proceso** : después de que se calculan los requisitos de red y almacenamiento, Azure Migrate Server Assessment tiene en cuenta los requisitos de CPU y memoria para encontrar un tamaño de máquina virtual adecuado en Azure.
    - Azure Migrate examina los núcleos y la memoria utilizados efectivos para encontrar un tamaño de máquina virtual adecuado en Azure.
    - Si no encuentra ningún tamaño adecuado, la máquina se marca como no adecuada para Azure.
    - Si encuentra un tamaño adecuado, Azure Migrate aplica los cálculos de red y almacenamiento. Después, aplica la configuración de ubicación y plan de tarifa para realizar la recomendación final de tamaño de máquina virtual.
    - Si hay varios tamaños de máquina virtual de Azure adecuados, se recomienda el que tiene el costo más bajo.

### <a name="as-on-premises-sizing"></a>Ajuste de tamaño como local

Si utiliza el *ajuste de tamaño local*, Server Assessment no tiene en cuenta el historial de rendimiento de las máquinas virtuales ni de los discos y asigna una SKU de máquina virtual en Azure en función del tamaño asignado en el entorno local. De manera similar a la del ajuste de tamaño de disco, examina el tipo de almacenamiento especificado en las propiedades de evaluación (HDD estándar/SSD/premium) y recomienda el tipo de disco según corresponda. El tipo de almacenamiento predeterminado es discos premium.

## <a name="confidence-ratings"></a>Clasificaciones de confianza
Cada evaluación según el rendimiento de Azure Migrate está asociada a una clasificación de confianza comprendida entre una estrella (la más baja) y cinco estrellas (la más alta).
- La clasificación de confianza se asigna a una valoración que se basa en la disponibilidad de puntos de datos necesarios para calcular tal valoración.
- La clasificación de confianza de una valoración le ayuda a calcular la confiabilidad de las recomendaciones de tamaño que proporciona Azure Migrate.
- La clasificación de confianza no es aplicable a evaluaciones como las locales.
- Para ajustes de tamaño según el rendimiento, Azure Migrate Server Assessment necesita lo siguiente:
    - Los datos de uso de la CPU y la memoria de la máquina virtual.
    - Además, para cada disco asociado a la máquina virtual, necesita el valor de IOPS de disco y los datos de rendimiento.
    - De forma similar, para cada adaptador de red asociado a una máquina virtual, la clasificación de confianza necesita la entrada o la salida de red para realizar el ajuste de tamaño según el rendimiento.
    - Si alguna de las cifras de uso anteriores no está disponible en vCenter Server, la recomendación de tamaño podría no ser confiable.

Según el porcentaje de puntos de datos disponibles, se proporciona la clasificación de confianza para la valoración, tal como se indica a continuación:

   **Disponibilidad de puntos de datos** | **Clasificación de confianza**
   --- | ---
   0 % - 20 % | 1 estrella
   21 % - 40 % | 2 estrellas
   41 % - 60 % | 3 estrellas
   61 % - 80 % | 4 estrellas
   81 % - 100 % | 5 estrellas

### <a name="low-confidence-ratings"></a>Clasificaciones de confianza bajas

Estos son algunos de los motivos por los que una evaluación puede obtener una clasificación de confianza baja:

- No ha generado un perfil de su entorno durante el tiempo que ha estado creando la evaluación. Por ejemplo, si crea la evaluación con la duración de rendimiento establecida en un día, debe esperar al menos un día después de empezar la detección para que se recopilen todos los puntos de datos.
- Se han apagado algunas máquinas virtuales en el período durante el que se calcula la evaluación. Si alguna máquina virtual se ha apagado durante un tiempo, Azure Migrate Server Assessment no puede recopilar los datos de rendimiento durante ese período.
- Algunas máquinas virtuales se han creado en algún momento del período durante el cual se calcula la evaluación. Por ejemplo, si crea una evaluación para el historial de rendimiento del último mes, pero ha creado algunas máquinas virtuales en el entorno hace solo una semana, el historial de rendimiento de las nuevas máquinas virtuales no indicará para toda la duración.

> [!NOTE]
> Si la clasificación de confianza de una evaluación se sitúa por debajo de cinco estrellas, se recomienda que espere al menos un día para que el dispositivo genere el perfil del entorno y luego recalcule la evaluación. Si no lo hace, es posible que el ajuste de tamaño según el rendimiento no sea confiable y se recomienda que cambie la evaluación para usarla como ajuste de tamaño local.

## <a name="monthly-cost-estimation"></a>Estimación del costo mensual

Una vez completadas las recomendaciones de ajuste de tamaño, Azure Migrate calcula los costos de almacenamiento y proceso para después de la migración.

- **Costo de proceso**: con el tamaño recomendado de máquina virtual de Azure, Azure Migrate usa Billing API para calcular el costo mensual de la máquina virtual.
    - El cálculo tiene en cuenta la configuración del sistema operativo, Software Assurance, las instancias reservadas, el tiempo de actividad de las máquinas virtuales, la ubicación y la moneda.
    - Para calcular el costo de proceso mensual total, agrega el costo a todas las máquinas.
- **Costo de almacenamiento**: el costo de almacenamiento mensual de una máquina se calcula sumando el costo mensual de todos los discos conectados a la máquina.
    - Azure Migrate Server Assessment calcula los costos de almacenamiento mensuales totales sumando los costos de almacenamiento de todas las máquinas.
    - Actualmente, el cálculo no tiene en cuenta las ofertas especificadas en la configuración de evaluación.

Los costos se muestran en la moneda especificada en la configuración de evaluación.


## <a name="next-steps"></a>Pasos siguientes

Creación de una evaluación para [máquinas virtuales de VMware](tutorial-assess-vmware.md) o [máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md).
