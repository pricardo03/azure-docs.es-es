---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 3ea41a16f79f903b5ff7d2b93f74f7d3d2a0dd83
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133111"
---
La galería de imágenes compartidas es un servicio que ayuda a crear la estructura y la organización en torno a las imágenes administradas. Las galerías de imágenes compartidas proporcionan:

- Replicación global administrada de las imágenes.
- Control de versiones y agrupación de las imágenes para facilitar la administración.
- Imágenes de alta disponibilidad con cuentas de almacenamiento con redundancia de zona (ZRS) en las regiones donde esté disponible Availability Zones. ZRS ofrece más resistencia a errores de zona.
- Imágenes compartidas entre suscripciones e, incluso, entre inquilinos de Active Directory (AD) mediante RBAC.
- Escalado de las implementaciones con réplicas de imagen en cada región.

Uso de una galería de imágenes compartidas para compartir imágenes con diferentes usuarios, entidades de servicio o grupos de AD dentro de su organización. Las imágenes compartidas se pueden replicar en varias regiones, para un escalado más rápido de las implementaciones.

Una imagen administrada es una copia de una VM completa (incluidos los discos de datos asociados) o, simplemente, el disco del sistema operativo, según cómo cree la imagen. Cuando crea una VM desde la imagen, la copia de los VHD de la imagen se usa para crear los discos para la nueva VM. La imagen administrada permanece en el almacenamiento y puede usarse una y otra vez para crear nuevas VM.

Si tiene un gran número de imágenes administradas que se deben mantener y quiere que estén disponibles en toda la empresa, puede usar una galería de imágenes compartidas como repositorio que facilite la tarea de compartir las imágenes. 

La característica de galería de imágenes compartidas tiene varios tipos de recursos:

| Recurso | DESCRIPCIÓN|
|----------|------------|
| **Imagen administrada** | Una imagen básica que se puede usar por sí sola o para crear una **versión de imagen** de una galería de imágenes. Las imágenes administradas se crean desde máquinas virtuales generalizadas. Una imagen administrada es un tipo de VHD especial que se puede usar para crear varias máquinas virtuales y que ahora se puede usar para crear versiones de imágenes compartidas. |
| **Galería de imágenes** | Al igual que Azure Marketplace, una **galería de imágenes** es un repositorio para administrar y compartir imágenes, pero usted puede controlar quién tiene acceso. |
| **Definición de la imagen** | Las imágenes se definen dentro de una galería y contienen información sobre la imagen y los requisitos para usarla en la organización. Puede incluir información como si la imagen es Windows o Linux, los requisitos mínimos y máximo de la memoria, y notas de la versión. Es una definición de un tipo de imagen. |
| **Versión de la imagen** | Una **versión de la imagen** es lo que se usa para crear una VM cuando se usa una galería. Puede tener varias versiones de una imagen según sea necesario para su entorno. Al igual que una imagen administrada, cuando se usa una **versión de la imagen** para crear una VM, la versión de la imagen se usa para crear nuevos discos para la VM. Las versiones de las imágenes pueden usarse varias veces. |

<br>


![Gráfico que muestra cómo puede tener varias versiones de una imagen en la galería](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>Definiciones de imagen

Las definiciones de la imagen son una agrupación lógica de las versiones de una imagen. La definición de la imagen contiene información acerca de por qué se creó la imagen, para qué sistema operativo sirve e información acerca del uso de la imagen. Una definición de imagen es como un plan para todos los detalles que rodean la creación de una imagen específica. No se implementa una máquina virtual desde una definición de imagen, sino desde la versión de imagen creada a partir de la definición.


Hay tres parámetros para cada definición de imagen que se usan en combinación: **Publisher**, **Offer** y **SKU**. Se utilizan para buscar una definición de imagen específica. Puede tener versiones de imágenes que comparten uno o dos valores, pero no los tres.  Por ejemplo, estas son las tres definiciones de imágenes y sus valores:

|Definición de imágenes|Publicador|Oferta|SKU|
|---|---|---|---|
|myImage1|Contoso|Finance|Back-end|
|myImage2|Contoso|Finance|Front-end|
|myImage3|Prueba|Finance|Front-end|

Los tres tienen conjuntos de valores únicos. El formato es similar a cómo puede especificar actualmente el editor, la oferta y la SKU para las [imágenes de Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) en Azure PowerShell para obtener la última versión de una imagen de Marketplace. Cada definición de imagen debe tener un conjunto único de estos valores.

Los siguientes son otros parámetros que se pueden establecer en la definición de la imagen para que pueda realizar un seguimiento más sencillo de sus recursos:

* Estado del sistema operativo: puede establecer el estado del sistema operativo en Generalizado o Especializado, pero actualmente solo se admite el estado Generalizado. Las imágenes deben crearse a partir de máquinas virtuales que se hayan generalizado mediante Sysprep para Windows o `waagent -deprovision` para Linux.
* Sistema operativo: puede ser Windows o Linux.
* Descripción: la descripción de uso para proporcionar información más detallada sobre por qué existe la definición de la imagen. Por ejemplo, podría tener una definición de la imagen para el servidor front-end que tenga la aplicación preinstalada.
* CLUF: puede utilizarse para señalar un contrato de licencia de usuario final específico para la definición de la imagen.
* Declaración de privacidad y Notas de la versión: almacene las notas de la versión y las declaraciones de privacidad en el almacenamiento Azure y proporcione un identificador URI para acceder a ellas como parte de la definición de imagen.
* Fecha del final de la duración: adjunte una fecha del final de la duración a la definición de imagen para poder utilizar la automatización y eliminar las definiciones de imagen antiguas.
* Etiqueta: puede agregar etiquetas al crear la definición de imagen. Para más información sobre las etiquetas, consulte [Uso de etiquetas para organizar los recursos](../articles/azure-resource-manager/resource-group-using-tags.md).
* Número mínimo y máximo de vCPU y recomendaciones de memoria: si la imagen tiene vCPU y recomendaciones de memoria, puede adjuntar esa información a la definición de imagen.
* Tipos de disco no permitidos: puede proporcionar información acerca de las necesidades de almacenamiento para la máquina virtual. Por ejemplo, si la imagen no es adecuada para los discos HDD estándar, agréguelos a la lista de no permitidos.


## <a name="regional-support"></a>Compatibilidad regional

Las regiones de origen se muestran en la tabla siguiente. Todas las regiones públicas pueden ser regiones de destino, pero para replicar en el Centro de Australia y Centro de Australia 2 debe tener su suscripción en la lista de permitidos. Para solicitar la inclusión en la lista blanca, visite: https://www.microsoft.com/en-au/central-regions-eligibility/


| Regiones de origen |
|---------------------|-----------------|------------------|-----------------|
| Centro de Australia   | EUAP del centro de EE. UU. | Corea Central    | Centro occidental de EE.UU. |
| Centro de Australia 2 | Asia oriental       | Corea del Sur      | Europa occidental     |
| Este de Australia      | Este de EE. UU         | Centro-Norte de EE. UU | Oeste de la India      |
| Sudeste de Australia | Este de EE. UU. 2       | Europa del Norte     | Oeste de EE. UU.         |
| Sur de Brasil        | EUAP de Este de EE. UU. 2  | Centro-Sur de EE. UU | Oeste de EE. UU. 2       |
| Centro de Canadá      | Centro de Francia  | Sur de la India      |                 |
| Este de Canadá         | Sur de Francia    | Sudeste asiático   |                 |
| India Central       | Este de Japón      | Sur de Reino Unido 2         |                 |
| Centro de EE. UU.          | Oeste de Japón      | Oeste de Reino Unido          |                 |



## <a name="limits"></a>límites 

Hay límites por suscripción, para implementar los recursos con las galerías de imágenes compartidas:
- 100 galerías de imágenes compartidas por suscripción, por región
- 1000 definiciones de imágenes por suscripción, por región
- 10 000 versiones de imágenes por suscripción, por región

Para más información, consulte [Comparación del uso de recursos con los límites](https://docs.microsoft.com/azure/networking/check-usage-against-limits) para obtener ejemplos sobre cómo comprobar el uso actual.
 

## <a name="scaling"></a>Escalado
La galería de imágenes compartidas le permite especificar el número de réplicas de las imágenes que quiere que Azure mantenga. Esto ayuda en los escenarios de implementación de varias VM, ya que las implementaciones de VM se pueden distribuir a las distintas réplicas, lo que reduce la probabilidad de que el proceso de creación de instancias quede limitado por la sobrecarga de una única réplica.


Con la galería de imágenes compartidas, ahora puede implementar hasta 1000 instancias de máquinas virtuales en una conjunto de escalado de máquinas virtuales (a partir de 600 con imágenes administradas). Las réplicas de imágenes proporcionan un mejor rendimiento de implementación, confiabilidad y coherencia.  Puede establecer un número de réplicas diferente en cada región de destino, en función de las necesidades de escala de la región. Dado que cada réplica es una copia en profundidad de la imagen, esto ayuda a escalar las implementaciones linealmente con cada réplica adicional. Aunque entendemos que no hay dos imágenes o regiones iguales, he aquí nuestra guía general sobre cómo usar réplicas en una región:

- Por cada 20 máquinas virtuales que cree simultáneamente, le recomendamos que conserve una réplica. Por ejemplo, si va a crear 120 máquinas virtuales simultáneamente mediante la misma imagen en una región, se recomienda conservar al menos seis réplicas de la imagen. 
- Para cada implementación de un conjunto de escalado con hasta 600 instancias, se recomienda conservar al menos una réplica. Por ejemplo, si va a crear cinco conjuntos de escalado de forma simultánea, cada uno con 600 instancias de máquinas virtuales con la misma imagen en una única región, se recomienda conservar al menos cinco réplicas de la imagen. 

Siempre se recomienda aprovisionar en exceso el número de réplicas debido a factores como el tamaño de la imagen, el contenido y el tipo de sistema operativo.


![Gráfico que muestra cómo puede escalar imágenes](./media/shared-image-galleries/scaling.png)



## <a name="make-your-images-highly-available"></a>Hacer que las imágenes tengan alta disponibilidad

[El almacenamiento con redundancia de zona (ZRS) de Azure](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) proporciona resistencia frente a un error de la zona de disponibilidad en la región. Con la disponibilidad general de la galería de imágenes compartidas, puede elegir almacenar sus imágenes en cuentas de ZRS en regiones con zonas de disponibilidad. 

También puede elegir el tipo de cuenta para cada una de las regiones de destino. El tipo de cuenta de almacenamiento predeterminado es Standard_LRS, pero puede elegir Standard_ZRS para regiones con zonas de disponibilidad. Compruebe la disponibilidad regional de ZRS [aquí](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs).

![Gráfico que muestra ZRS](./media/shared-image-galleries/zrs.png)


## <a name="replication"></a>Replicación
La galería de imágenes compartidas también permite replicar las imágenes en otras regiones de Azure automáticamente. Cada versión de una imagen compartida se puede replicar en distintas regiones en función de lo que sea más conveniente para su organización. Un ejemplo es replicar siempre la imagen más reciente en varias regiones, mientras que todas las versiones anteriores solo están disponibles en una región. Esto puede ayudar a ahorrar costos de almacenamiento para las versiones de imágenes compartidas. 

Las regiones donde puede replicarse la versión de una imagen compartida se pueden actualizar después de su creación. El tiempo necesario para replicar en diferentes regiones depende de la cantidad de datos que se copien y del número de regiones en las que se replicará la versión. En algunos casos, esto puede tardar varias horas. Mientras se produce la replicación, puede ver el estado de replicación por región. Una vez que se complete la replicación de la imagen en una región, podrá implementar una máquina virtual o conjunto de escalado con esa versión de la imagen en la región.

![Gráfico que muestra cómo puede replicar imágenes](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access

Dado que la galería de imágenes compartidas, la definición de la imagen y la versión de la imagen son recursos, pueden compartirse con los controles integrados nativos de Azure RBAC. Con RBAC puede compartir estos recursos con otros usuarios, entidades de servicio y grupos. Incluso se puede compartir el acceso a personas ajenas al inquilino en el que se crearon. Cuando el usuario tiene acceso a la versión de las imágenes compartidas, puede implementar una máquina virtual o un conjunto de escalado de máquinas virtuales.  Aquí está la matriz de uso compartido que ayuda a entender a lo que el usuario tiene acceso:

| Compartido con el usuario     | Galería de imágenes compartidas | Definición de imágenes | Versión de la imagen |
|----------------------|----------------------|--------------|----------------------|
| Galería de imágenes compartidas | Sí                  | Sí          | Sí                  |
| Definición de imágenes     | Sin                   | Sí          | Sí                  |

Se recomienda el uso compartido en el nivel de la galería para una mejor experiencia. No se recomienda compartir las versiones individuales de la imagen. Para más información, consulte [Administración del acceso a los recursos de Azure mediante RBAC](../articles/role-based-access-control/role-assignments-portal.md).

Las imágenes también se pueden compartir, a escala, incluso entre inquilinos mediante un registro de aplicación de varios inquilinos. Para más información acerca de cómo compartir imágenes entre los inquilinos, consulte [Compartir imágenes de máquina virtual de la galería en inquilinos de Azure](../articles/virtual-machines/linux/share-images-across-tenants.md).

## <a name="billing"></a>Facturación
No hay ningún cargo adicional por usar el servicio de la galería de imágenes compartidas. Se le cobrará por los siguientes recursos:
- Costos de almacenamiento de las versiones de imágenes compartidas. El costo depende del número de réplicas de la versión de la imagen y del número de regiones en las que se replique la versión. Por ejemplo, si tiene dos imágenes y ambas se replican en tres regiones, entonces se le cambiará por seis discos administrados según su tamaño. Para más información, consulte [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).
- Cargos de salida de red para la replicación de la primera versión de la imagen desde la región de origen a las regiones replicadas. Las réplicas subsiguientes se tratan dentro de la región, por lo que no habrá ningún cargo adicional. 

## <a name="updating-resources"></a>Actualización de recursos

Una vez creados, puede realizar algunos cambios en los recursos de la galería de imágenes. Estos se limitan a:
 
Galería de imágenes compartidas:
- DESCRIPCIÓN

Definición de la imagen:
- vCPU recomendadas:
- Memoria recomendada
- DESCRIPCIÓN
- Fecha final del ciclo de vida

Versión de la imagen:
- Recuento de réplicas regionales
- Regiones de destino
- Excluir de la versión más reciente
- Fecha final del ciclo de vida


## <a name="sdk-support"></a>Compatibilidad con SDK

Los siguientes SDK admiten la creación de galerías de imágenes compartidas:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/azure-arm-compute/?view=azure-node-latest)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Go](https://docs.microsoft.com/go/azure/)

## <a name="templates"></a>Plantillas

Puede crear recursos de galería de imágenes compartidas con plantillas. Hay varias plantillas de Inicio rápido de Azure disponibles: 

- [Creación de una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Creación de una definición de imagen en una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Creación de una versión de imagen en una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Creación de una máquina virtual a partir de la versión de la imagen](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 

**P.** ¿Cómo puedo mostrar todos los recursos de la galería de imágenes compartidas en las suscripciones? 
 
 A. Para enumerar todos los recursos de la galería de imágenes compartidas a través de las suscripciones a las que tiene acceso en Azure Portal, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com).
1. Vaya a **Todos los recursos**.
1. Seleccione todas las suscripciones en las que quiera enumerar todos los recursos.
1. Busque los recursos de tipo **Galería privada**.
 
   Para ver las definiciones de imagen y las versiones de imagen, debe seleccionar también **Mostrar tipos ocultos**.
 
   Para enumerar todos los recursos de la galería de imágenes compartidas mediante las suscripciones para las que tiene permiso, use el comando siguiente en la CLI de Azure:

   ```bash
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```


**P.** ¿Puedo mover una imagen existente a la galería de imágenes compartidas?
 
 A. Sí. Hay tres escenarios basados en los tipos de imagen que pueda tener.

 Escenario 1: Si tiene una imagen administrada, puede crear una definición de la imagen y versión de la imagen a partir de ella.

 Escenario 2: Si tiene una imagen generalizada no administrada, puede crear una imagen administrada a partir de ella y, luego, crear una definición de la imagen y una versión de la imagen a partir de ella. 

 Escenario 3: Si tiene un VHD en el sistema de archivos locales, deberá cargar el VHD, crear una imagen administrada y, luego, crear una definición de la imagen y una versión de la imagen a partir de ella.
- Si el VHD es de una VM Windows, consulte [Carga de un VHD generalizado](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Si el VHD es para una VM Linux, consulte [Cargar un disco duro virtual](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd).


**P.** ¿Puedo crear una versión de la imagen desde un disco especializado?

 A. No, actualmente no admitimos discos especializados como imágenes. Si tiene un disco especializado, deberá [crear una VM a partir del VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) al asociar el disco especializado a una nueva VM. Una vez que tenga una máquina virtual en ejecución, deberá seguir las instrucciones para crear una imagen administrada desde la [máquina virtual Windows](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) o la [máquina virtual Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Cuando tenga una imagen administrada generalizada, puede iniciar el proceso para crear una descripción de imagen compartida y la versión de la imagen.

 
**P.** Una vez que se cree, ¿puedo mover el recurso de la galería de imágenes compartidas a otra suscripción?

 A. No, no puede mover el recurso de la galería de imágenes compartidas a otra suscripción. Sin embargo, podrá replicar las versiones de la imagen en la galería a otras regiones según sea necesario.

**P.** ¿Puedo replicar las versiones de mis imágenes en las nubes: Azure China 21Vianet, Azure Alemania y la nube de Azure Government? 

 A. No, no puede replicar las versiones de la imagen en las nubes.

**P.** ¿Puedo replicar las versiones de mis imágenes entre suscripciones? 

 A. No, puede replicar las versiones de la imagen en varias regiones de una suscripción y usarla en otras suscripciones a través de RBAC.

**P.** ¿Puedo compartir versiones de imágenes entre los inquilinos de Azure AD? 

 A. Sí, puede utilizar RBAC para compartirlo con las personas entre los inquilinos. Pero, para compartir a escala, consulte "Uso compartido de imágenes de la galería entre inquilinos de Azure" con [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) o [CLI](../articles/virtual-machines/linux/share-images-across-tenants.md).


**P.** ¿Cuánto tarda la replicación de versiones de imágenes en las regiones de destino?

 A. El tiempo de replicación de la versión de una imagen depende totalmente del tamaño de la imagen y del número de regiones en las que se va a replicar. Sin embargo, como práctica recomendada para obtener los mejores resultados, se recomienda mantener una imagen pequeña y las regiones de origen y de destino cerca. Puede comprobar el estado de la replicación mediante la marca -ReplicationStatus.


**P.** ¿Cuál es la diferencia entre la región de origen y la región de destino?

 A. La región de origen es la región en la que se creará la versión de la imagen, y las regiones de destino son las regiones donde se almacenará una copia de la versión de la imagen. Para cada versión de una imagen, solo puede tener una región de origen. Además, asegúrese de pasar la ubicación de la región de origen como una de las regiones de destino cuando cree una versión de la imagen.  


**P.** ¿Cómo puedo especificar la región de origen al crear la versión de la imagen?

 A. Al crear una versión de la imagen, puede usar la etiqueta **--location** en la CLI y la etiqueta **-Location** en PowerShell para especificar la región de origen. Asegúrese de que la imagen administrada que usa como imagen de base para crear la versión de la imagen esté en la misma ubicación que la ubicación en la que va a crear la versión de la imagen. Además, asegúrese de pasar la ubicación de la región de origen como una de las regiones de destino cuando cree una versión de la imagen.  


**P.** ¿Cómo puedo especificar el número de réplicas de la versión de la imagen que se creará en cada región?

 A. Hay dos maneras de especificar el número de réplicas de la versión de la imagen que se creará en cada región:
 
1. El recuento de réplicas regionales que especifica el número de réplicas que desea crear para cada región. 
2. El recuento de réplicas comunes que sea el recuento predeterminado por región en caso de que no se especifique el recuento de réplicas regionales. 

Para especificar el recuento de réplicas regionales, pase la ubicación junto con el número de réplicas que quiere crear en esa región: "Centro-sur de EE. UU. = 2". 

Si el recuento de réplicas regionales no se especifica con cada ubicación, el número predeterminado de réplicas será el recuento de réplicas comunes que especificó. 

Para especificar el recuento de réplicas comunes en la CLI, use el argumento **--replica-count** en el comando `az sig image-version create`.


**P.** ¿Puedo crear la galería de imágenes compartidas en una ubicación diferente de en la que quiero crear la definición de la imagen y la versión de la imagen?

 A. Sí, es posible. Pero como práctica recomendada, le animamos a mantener el grupo de recursos, la galería de imágenes compartidas, la definición de la imagen y la versión de la imagen en la misma ubicación.


**P.** ¿Cuáles son los cargos por uso de la galería de imágenes compartidas?

 A. No hay ningún cargo por usar el servicio de la galería de imágenes compartidas, excepto los cargos de almacenamiento de las versiones de las imágenes y los cargos de salida de red para la replicación de las versiones de las imágenes de la región de origen a las regiones de destino.

**P.** ¿Qué versión de API debo usar para crear la galería de imágenes compartidas, la definición de la imagen, la versión de la imagen y VM/VMSS a partir de la versión de la imagen?

 A. Para implementaciones de VM y conjuntos de escalado de máquinas virtuales que usan una versión de la imagen, se recomienda usar la API versión 2018-04-01 o posterior. Para trabajar con galerías de imágenes compartidas, definiciones de imágenes y versiones de imágenes, se recomienda usar la API versión 2018-06-01. El almacenamiento con redundancia de zona (ZRS) requiere la versión de 2019-03-01 o posterior.
