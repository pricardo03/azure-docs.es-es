---
title: 'Azure Lab Services: guía del administrador | Microsoft Docs'
description: Esta guía ayuda a los administradores que crean y administran cuentas de laboratorio mediante Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: spelluru
ms.openlocfilehash: 638a90615d248b3c2829770432dd6a08eb4bb2fb
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771741"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure Lab Services: guía del administrador
Los administradores de tecnología de la información (TI) que administran los recursos en la nube de una organización suelen ser también responsables de configurar la cuenta de laboratorio en su organización. Los administradores o educadores crean laboratorios educativos en la cuenta de laboratorio. En este artículo se proporciona información general de alto nivel sobre los recursos de Azure relacionados e instrucciones para crearlos.

![Vista de alto nivel de los recursos de Azure de una cuenta de laboratorio](../media/administrator-guide/high-level-view.png)

- Los laboratorios educativos se hospedan en una suscripción de Azure propiedad de Azure Lab Services.
- Las cuentas de laboratorio, la galería de imágenes compartidas y las versiones de imágenes se hospedan en la suscripción.
- La cuenta de laboratorio y la galería de imágenes compartidas pueden residir en el mismo grupo de recursos. En este diagrama, están en distintos grupos de recursos. 

## <a name="subscription"></a>Subscription
Su organización tiene una o varias suscripciones de Azure. Las suscripciones se usan para administrar la facturación y la seguridad de todos los recursos y servicios de Azure que se usan dentro de esta plataforma, incluidas las cuentas de laboratorio.

La relación entre una cuenta de laboratorio y su suscripción es importante por los siguientes motivos:

- La facturación se notifica mediante la suscripción que contiene la cuenta de laboratorio.
- Puede conceder a los usuarios del inquilino de Azure Active Directory (AD) asociado a la suscripción el acceso a Azure Lab Services. El usuario se puede agregar como propietario o colaborador de la cuenta de laboratorio o como creador del laboratorio educativo.

Los laboratorios educativos y sus máquinas virtuales (VM) se administran de forma automática. En concreto, se hospedan en una suscripción dedicada propiedad de Azure Lab Services.

## <a name="resource-group"></a>Resource group
Una suscripción contiene uno o varios grupos de recursos. Los grupos de recursos se usan para crear agrupaciones lógicas de recursos de Azure que se emplean juntos dentro de la misma solución.  

Al crear una cuenta de laboratorio, debe configurar el grupo de recursos que la contiene. 

También se requiere un grupo de recursos para crear una [galería de imágenes compartidas](#shared-image-gallery). Puede optar por colocar su cuenta de laboratorio y la galería de imágenes compartidas en dos grupos de recursos distintos, lo que es habitual si planea compartir la galería de imágenes entre diferentes soluciones. También, puede elegir colocarlas en el mismo grupo de recursos.

Al crear una cuenta de laboratorio y crear y asociar automáticamente una galería de imágenes compartidas al mismo tiempo, la cuenta de laboratorio y la galería de imágenes compartidas se crean en grupos de recursos diferentes de forma predeterminada. Observará este comportamiento cuando siga los pasos descritos en este tutorial: [Configuración de la galería de imágenes compartidas en el momento de crear la cuenta de laboratorio](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation). La imagen que se encuentra en la parte superior de este artículo también usa esta configuración. 

Se recomienda dedicar tiempo por adelantado a planear la estructura de los grupos de recursos, ya que, una vez creado, no es posible cambiar el grupo de recursos de una cuenta de laboratorio o de la galería de imágenes compartidas. Si necesita cambiar el grupo de recursos de estos recursos, deberá eliminar y volver a crear la cuenta de laboratorio o la galería de imágenes compartidas.

## <a name="lab-account"></a>Cuenta de laboratorio
Una cuenta de laboratorio funciona como contenedor de uno o varios laboratorios educativos. Al empezar a trabajar con Azure Lab Services, es habitual tener solo una cuenta de laboratorio. A medida que se amplía el uso del laboratorio, puede optar por crear más adelante otras cuentas de laboratorio.

En la lista siguiente se resaltan escenarios en los que puede ser beneficioso tener más de una cuenta de laboratorio:

- **Administración de distintos requisitos de directivas entre laboratorios educativos** 

    Cuando se configura una cuenta de laboratorio, se establecen directivas que se aplican a todos los laboratorios educativos de la cuenta de laboratorio, por ejemplo:
    - La red virtual de Azure con recursos compartidos a los que puede acceder el laboratorio educativo. Por ejemplo, puede tener un conjunto de laboratorios educativos que necesiten acceso a un conjunto de datos compartido dentro de una red virtual.
    - Las imágenes de máquina virtual (VM) que los laboratorios educativos pueden usar para crear máquinas virtuales. Por ejemplo, puede tener un conjunto de laboratorios educativos que necesiten acceso a la imagen de Marketplace de [Data Science VM para Linux](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu).  

    Si tiene laboratorios educativos con requisitos de directivas únicos entre sí, puede ser beneficioso crear cuentas de laboratorio independientes para administrar estos laboratorios por separado.
- **Restricción del acceso de los creadores del laboratorio a laboratorios educativos concretos**  

    Cuando se agrega un usuario como creador de un laboratorio, se le concede acceso a todos los laboratorios educativos de la cuenta de laboratorio, incluidos los laboratorios creados por otros. Para limitar a los creadores de laboratorios a la administración de laboratorios concretos, puede crear cuentas de laboratorio independientes para restringir el ámbito de su acceso. Por ejemplo, puede crear una cuenta de laboratorio aparte para cada departamento de una universidad. una cuenta de laboratorio para el Departamento de Ciencias, otra para el Departamento de Matemáticas, etc.   
- **Separación de presupuestos por cuenta de laboratorio**

    En lugar de presentar todos los costos de laboratorios educativos en una sola cuenta de laboratorio, puede que deba tener un presupuesto separado más claramente. Siguiendo con el ejemplo de la viñeta anterior, podría crear una cuenta de laboratorio para cada departamento de la universidad a fin de separar el presupuesto en consecuencia. Con Azure Cost Management, puede ver luego el costo de cada cuenta de laboratorio.
- **Aislamiento de los laboratorios piloto de los laboratorios activos**

    Puede que haya casos en los que quiera realizar cambios en la directiva piloto de una cuenta de laboratorio sin que estos cambios afecten a los laboratorios activos. En este tipo de escenario, la creación de una cuenta de laboratorio independiente con fines piloto permite aislar los cambios. 

## <a name="classroom-lab"></a>Laboratorio educativo
Un laboratorio educativo contiene una o varias máquinas virtuales (VM) que se asignan a un alumno específico. En general, puede esperar lo siguiente:

- Tener un laboratorio educativo para cada clase.
- Crear un conjunto de laboratorios educativos cada semestre (o en cada período de tiempo en que se ofrezca su clase). Normalmente, en clases que tengan las mismas necesidades de imágenes, se usaría una [galería de imágenes compartidas](#shared-image-gallery) para compartir imágenes entre laboratorios y semestres.

Tenga en cuenta los siguientes puntos a la hora de determinar cómo estructurar los laboratorios educativos:

- **Todas las máquinas virtuales de un laboratorio educativo se implementan con la misma imagen con la que se han publicado**. Como resultado, si tiene una clase que requiere que se publiquen distintas imágenes de laboratorio al mismo tiempo, deben crearse laboratorios de clase distintos para cada una.
- **La cuota de uso se establece en el nivel de laboratorio y se aplica a todos los usuarios del laboratorio**. Por ejemplo, puede que tenga un conjunto de educadores que necesiten acceso a las máquinas virtuales de un aula para prepararse para dar la clase, pero que solo necesitan una cuota de 10 horas, mientras que los alumnos inscritos en el aula necesiten una cuota de 40 horas. Para establecer cuotas diferentes para los usuarios, debe crear distintos laboratorios educativos. Sin embargo, es posible agregar más horas a un usuario específico después de haber establecido la cuota.
- **La programación de inicio o apagado se establece en el nivel de laboratorio y se aplica a todas las máquinas virtuales del laboratorio**. De forma similar al punto anterior, si necesita establecer diferentes programaciones para los usuarios, debe crear distintos laboratorios educativos. 

## <a name="shared-image-gallery"></a>Galería de imágenes compartidas
Una galería de imágenes compartidas se asocia a una cuenta de laboratorio y sirve como repositorio central para almacenar imágenes. Una imagen se guarda en la galería cuando un educador elige guardarla desde la máquina virtual (VM) de plantilla de un laboratorio educativo. Cada vez que un educador realiza cambios en la máquina virtual de plantilla y los guarda, se guardan nuevas versiones de la imagen y se mantienen las versiones anteriores.

Los instructores pueden publicar una versión de una imagen desde la galería de imágenes compartidas cuando crean un laboratorio educativo. Aunque la galería puede almacenar varias versiones de una imagen, los educadores solo pueden seleccionar la versión más reciente durante la creación del laboratorio.

La galería de imágenes compartidas es un recurso opcional que es posible que no necesite de inmediato al empezar con solo algunos laboratorios educativos. Sin embargo, su uso tiene muchas ventajas que le resultarán útiles a la hora de tener más laboratorios educativos:

- **Permite guardar y administrar versiones de una imagen de máquina virtual de plantilla**.

    Resulta útil cuando se crea una imagen personalizada o se realizan cambios (software, configuración, etc.) en una imagen de la galería pública de Marketplace.  Por ejemplo, es habitual que los educadores requieran la instalación de software o herramientas diferentes. En lugar de solicitar a los alumnos que instalen manualmente estos requisitos previos por su cuenta, se pueden guardar diferentes versiones de la imagen de máquina virtual de plantilla en una galería de imágenes compartidas. Estas versiones de imagen se pueden usar luego para crear otros laboratorios educativos.
- **Permite el uso compartido o la reutilización de imágenes de máquina virtual de plantilla entre laboratorios educativos**.

    Evita tener que configurar una imagen desde el principio cada vez que crea un laboratorio educativo. Por ejemplo, si se ofrecen varias clases que necesitan la misma imagen, esta imagen solo debe crearse una vez y guardarse en la galería de imágenes compartidas para que se pueda compartir entre los laboratorios educativos.
- **Garantiza la disponibilidad de imágenes mediante la replicación**.

    Al guardar la imagen en la galería de imágenes compartidas de un laboratorio educativo, esta se replica automáticamente en otras regiones de la misma geografía. En el caso de que se produzca una interrupción en una región, la publicación de la máquina virtual de plantilla en el laboratorio educativo no resulta afectada gracias al uso de una réplica de la imagen en otras regiones. Además, ayuda al rendimiento en escenarios de publicación de varias máquinas virtuales al distribuir el uso de diferentes réplicas.

Para agrupar imágenes compartidas de forma lógica, tiene un par de opciones:

- Crear varias galerías de imágenes compartidas. Cada cuenta de laboratorio solo se puede conectar a una galería de imágenes compartidas, por lo que esta opción también requerirá la creación de varias cuentas de laboratorio.
- También, puede usar una sola galería de imágenes compartidas entre varias cuentas de laboratorio. En este caso, cada cuenta de laboratorio puede habilitar solo las imágenes aplicables a los laboratorios educativos que contenga.

## <a name="naming"></a>Nomenclatura
Cuando empiece a trabajar con Azure Lab Services, se recomienda establecer convenciones de nomenclatura para los grupos de recursos, las cuentas de laboratorio, los laboratorios educativos y la galería de imágenes compartidas. Aunque las convenciones de nomenclatura que establezca serán únicas para las necesidades de su organización, en la tabla siguiente se describen las directrices generales.

| Tipo de recurso | Role | Patrón sugerido | Ejemplos |
| ------------- | ---- | ----------------- | -------- | 
| Resource group | Contiene una o varias cuentas de laboratorio y una o varias galerías de imágenes compartidas. | \<nombre corto de organización\>-\<entorno\>-rg<ul><li>**Nombre corto de la organización**: identifica el nombre de la organización que admite el grupo de recursos.</li><li>**Entorno**: identifica el entorno del recurso, por ejemplo, prueba o producción.</li><li>**Rg**: significa el tipo de recurso: grupo de recursos (del inglés resource group).</li></ul> | contosouniversitylabs-rg<br/>contosouniversitylabs-test-rg<br/>contosouniversitylabs-prod-rg |
| Cuenta de laboratorio | Contiene uno o varios laboratorios. | \<nombre corto de laboratorio\>-\<entorno\>-la<ul><li>**Nombre corto de la organización**: identifica el nombre de la organización que admite el grupo de recursos.</li><li>**Entorno**: identifica el entorno del recurso, por ejemplo, prueba o producción.</li><li>**La**: significa el tipo de recurso: cuenta de laboratorio (del inglés lab account).</li></ul> | contosouniversitylabs-la<br/>mathdeptlabs-la<br/>sciencedeptlabs-test-la<br/>sciencedeptlabs-prod-la |
| Laboratorio educativo | Contiene una o varias máquinas virtuales. |\<nombre de clase\>-\<período de tiempo\>-\<identificador del educador\><ul><li>**Nombre de clase**: identifica el nombre de la clase que admite el laboratorio.</li><li>**Período de tiempo**: identifica el período de tiempo en el que se ofrece la clase.</li>**Identificador del educador**: identifica el educador que posee el laboratorio.</li></ul> | CS1234-fall2019-johndoe<br/>CS1234-spring2019-johndoe | 
| Galería de imágenes compartidas | Contiene una o más versiones de imágenes de máquina virtual. | \<nombre corto de la organización\>galería | contosouniversitylabsgallery |

Para más información sobre la asignación de nombres a otros recursos de Azure, consulte las [convenciones de nomenclatura para recursos de Azure](/azure/architecture/best-practices/naming-conventions).

## <a name="regions-or-locations"></a>Regiones o ubicaciones
Al configurar los recursos de Azure Lab Services, se le pedirá que proporcione una región o ubicación del centro de datos que hospedará el recurso. Aquí hay más detalles sobre cómo la región o ubicación afecta a cada uno de los siguientes recursos utilizados en la implementación de Lab Services:

- **Grupos de recursos**

    La región especifica el centro de datos donde se almacena la información sobre el grupo de recursos. Los recursos de Azure contenidos en el grupo de recursos pueden estar en otras regiones que no son la principal.
- **Cuenta de laboratorio o laboratorio educativo**

    La ubicación de la cuenta de laboratorio indica la región de este recurso. Los laboratorios educativos creados en la cuenta de laboratorio se pueden implementar en cualquier región de la misma geografía. La región específica en la que se implementan las máquinas virtuales del laboratorio se selecciona automáticamente en función de la capacidad disponible en la región en ese momento.  
    Si un administrador permite que los creadores de un laboratorio elijan la ubicación de su laboratorio educativo, las ubicaciones que estén disponibles para la selección se basarán en la capacidad regional disponible al crear el laboratorio.

    La ubicación del laboratorio educativo también determina qué tamaños de proceso de la máquina virtual están disponibles para la selección. Algunos tamaños de proceso solo están disponibles en ubicaciones concretas.
- **Galería de imágenes compartidas**

    La región indica la región de origen en la que se almacena la primera versión de la imagen antes de replicarse automáticamente en las regiones de destino.
    
Una regla general es establecer la región o ubicación de un recurso en la más próxima a sus usuarios. En el caso de los laboratorios educativos, significa crear el laboratorio lo más cerca posible de sus alumnos. En el caso de los cursos en línea donde los alumnos están ubicados en cualquier parte del mundo, deberá usar su mejor criterio para crear un laboratorio educativo que esté ubicado en un lugar central. También puede dividir una clase en varios laboratorios educativos según la región de los alumnos.

## <a name="vm-sizing"></a>Tamaño de máquina virtual
Cuando los administradores o creadores de laboratorios crean un laboratorio educativo, pueden elegir entre los siguientes tamaños de máquina virtual en función de las necesidades de su aula. Recuerde que los tamaños de proceso que haya disponibles dependerán de la región en la que se encuentre la cuenta de laboratorio:

| Size | Especificaciones | Sugerencia de uso |
| ---- | ----- | ------------- |
| Pequeña| <ul><li>2 núcleos</li><li>3,5 GB de RAM</li></ul> | Este tamaño es el más adecuado para la línea de comandos, apertura de navegador web, servidores web de poco tráfico, bases de datos pequeñas o medianas. |
| Media | <ul><li>4 núcleos</li><li>7 GB de RAM</li></ul> | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria. |
| Mediano (virtualización anidada) | <ul><li>4 núcleos</li><li>16 GB de RAM</li></ul> | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria.  Este tamaño admite la virtualización anidada. |
| grande | <ul><li>8 núcleos</li><li>32 GB de RAM</li></ul> | Este tamaño es ideal para aplicaciones que necesitan CPU más rápidas, un mejor rendimiento de los discos locales, bases de datos grandes y memorias caché grandes.  Este tamaño admite la virtualización anidada. |
| GPU pequeña (visualización) | <ul><li>6 núcleos</li><li>56 GB de RAM</li> | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. |
| GPU pequeña (proceso) | <ul><li>6 núcleos</li><li>56 GB de RAM</li></ul> |Este tamaño es más adecuado para aplicaciones de proceso intensivo, como la inteligencia artificial y el aprendizaje profundo. |
| GPU mediana (visualización) | <ul><li>12 núcleos</li><li>112 GB de RAM</li></ul> | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. |

## <a name="manage-identity"></a>Administración de identidades
Hay dos tipos de roles que puede tener el administrador de una cuenta de laboratorio:

- **Propietario**

    Un administrador al que se haya asignado el rol **Propietario** tiene acceso total a la cuenta de laboratorio, incluido el derecho a otorgar a otros usuarios acceso a ella y agregar creadores de laboratorios. El administrador que crea la cuenta de laboratorio se agrega de forma predeterminada como propietario.
- **Colaborador**

    Un administrador al que se asigna el rol de colaborador puede cambiar la configuración de la cuenta de laboratorio, pero no puede conceder acceso a otros usuarios, ni puede agregar creadores de laboratorio.

Al asociar una galería de imágenes compartidas a una cuenta de laboratorio, el acceso se concede automáticamente al administrador y a los creadores del laboratorio para que puedan ver y guardar imágenes en la galería. 

## <a name="pricing"></a>Precios

### <a name="azure-lab-services"></a>Azure Lab Services
Los precios de Azure Lab Services se describen en el artículo siguiente: [Precios de Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

También debe tener en cuenta los precios de la galería de imágenes compartidas si tiene previsto usarla para almacenar y administrar versiones de imágenes. 

### <a name="shared-image-gallery"></a>Galería de imágenes compartidas
La creación de una galería de imágenes compartidas y la asociación a su cuenta de laboratorio es gratuita. No se generan gastos hasta que se guarda una versión de la imagen en la galería. Normalmente, el precio por usar una galería de imágenes compartidas es bastante insignificante, pero es importante comprender cómo se calcula, ya que no se incluye en los precios de Azure Lab Services.  

### <a name="storage-charges"></a>Cargos de almacenamiento
Para almacenar versiones de imágenes, una galería de imágenes compartidas usa discos estándar administrados por HDD. El tamaño del disco administrado por HDD que se usa depende del tamaño de la versión de la imagen que se almacena. Consulte el siguiente artículo para ver los precios: [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).


### <a name="replication-and-network-egress-charges"></a>Cargos por salida de replicación y red
Cuando se guarda una versión de una imagen mediante la máquina virtual de plantilla (VM) de un laboratorio educativo, Lab Services la almacena primero en una región de origen y, luego, replica automáticamente la versión de la imagen de origen en una o varias regiones de destino. Es importante tener en cuenta que Azure Lab Services replica automáticamente la versión de la imagen de origen en todas las regiones de destino dentro de la ubicación geográfica donde se encuentra el laboratorio educativo. Por ejemplo, si el laboratorio educativo está en la ubicación geográfica de EE. UU., se replica una versión de la imagen en cada una de las ocho regiones que existen en EE. UU.

Un cargo por salida de red se produce cuando una versión de una imagen se replica de la región de origen a regiones de destino adicionales. La cantidad que se cobra se basa en el tamaño de la versión de la imagen cuando los datos de la imagen se transfieren inicialmente desde la región de origen.  Para información detallada sobre precios, consulte el artículo siguiente: [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

En las [soluciones educativas](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3) los clientes pueden estar exentos de pagar cargos por salida. Hable con el administrador de cuentas para más información.  Para más información, consulte las sección de **preguntas frecuentes** en el documento vinculado, en concreto, la pregunta sobre qué programas de transferencia de datos existen para los clientes académicos y cómo se puede acceder a ellos.

### <a name="pricing-example"></a>Ejemplo de precios
Para resumir los precios descritos anteriormente, echemos un vistazo a un ejemplo de cómo guardar la imagen de la máquina virtual de plantilla en la galería de imágenes compartidas. Considere los casos siguientes:

- Tiene una imagen de máquina virtual personalizada.
- Va a guardar dos versiones de la imagen.
- El laboratorio está en Estados Unidos, que tiene ocho regiones en total.
- Cada versión de la imagen tiene un tamaño de 32 GB; como resultado, el precio del disco administrado por HDD es de 1,54 USD al mes.

El costo total se calcula de la manera siguiente:

Número de imágenes × número de versiones × número de réplicas × precio del disco administrado

En este ejemplo, el costo es:

1 imagen personalizada (32 GB) x 2 versiones x 8 regiones de EE. UU. x 1,54 USD = 24,64 USD

### <a name="cost-management"></a>Administración de costos
Es importante que el administrador de la cuenta de laboratorio administre los costos mediante la eliminación rutinaria de las versiones de imágenes innecesarias de la galería. 

No debe eliminar la replicación a regiones específicas para reducir los costos (esta opción existe en la galería de imágenes compartidas). Los cambios en la replicación pueden tener efectos negativos sobre la capacidad de Azure Lab Services para publicar máquinas virtuales a partir de imágenes guardadas en una galería de imágenes compartidas.

## <a name="next-steps"></a>Pasos siguientes
Consulte el tutorial para obtener instrucciones paso a paso sobre cómo crear una cuenta de laboratorio y un laboratorio: [Tutorial: Configuración de una cuenta de laboratorio](tutorial-setup-lab-account.md)
