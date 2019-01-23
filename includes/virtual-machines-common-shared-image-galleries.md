---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 01/09/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: c65fb1f0f635e79d594a7f080124827e3218f612
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193477"
---
La galería de imágenes compartidas es un servicio que ayuda a generar estructura y organización en torno a las imágenes de VM administradas y personalizadas. Uso de una galería de imágenes compartidas para compartir imágenes con diferentes usuarios, entidades de servicio o grupos de AD dentro de su organización. Las imágenes compartidas se pueden replicar en varias regiones, para un escalado más rápido de las implementaciones.

Una imagen administrada es una copia de una VM completa (incluidos los discos de datos asociados) o, simplemente, el disco del sistema operativo, según cómo cree la imagen. Cuando crea una VM desde la imagen, la copia de los VHD de la imagen se usa para crear los discos para la nueva VM. La imagen administrada permanece en el almacenamiento y puede usarse una y otra vez para crear nuevas VM.

Si tiene un gran número de imágenes administradas que se deben mantener y quiere que estén disponibles en toda la empresa, puede usar una galería de imágenes compartidas como repositorio que facilite la tarea de actualizar y compartir las imágenes. Los cargos por uso de una galería de imágenes compartidas son simplemente los costos del almacenamiento usado por las imágenes, además de los costos de salida de red para la replicación de imágenes de la región de origen a las regiones publicadas.

La característica de galería de imágenes compartidas tiene varios tipos de recursos:

| Recurso | DESCRIPCIÓN|
|----------|------------|
| **Imagen administrada** | Se trata de una imagen básica que se puede usar por sí sola o para crear una **versión de imagen** de una galería de imágenes. Las imágenes administradas se crean desde máquinas virtuales generalizadas. Una imagen administrada es un tipo de VHD especial que se puede usar para crear varias máquinas virtuales y que ahora se puede usar para crear versiones de imágenes compartidas. |
| **Galería de imágenes** | Al igual que Azure Marketplace, una **galería de imágenes** es un repositorio para administrar y compartir imágenes, pero usted puede controlar quién tiene acceso. |
| **Definición de la imagen** | Las imágenes se definen dentro de una galería y contienen información sobre la imagen y los requisitos para usarla internamente. Esto incluye si la imagen es Windows o Linux, notas de la versión y los requisitos de memoria mínima y máxima. Es una definición de un tipo de imagen. |
| **Versión de la imagen** | Una **versión de la imagen** es lo que se usa para crear una VM cuando se usa una galería. Puede tener varias versiones de una imagen según sea necesario para su entorno. Al igual que una imagen administrada, cuando se usa una **versión de la imagen** para crear una VM, la versión de la imagen se usa para crear nuevos discos para la VM. Las versiones de las imágenes pueden usarse varias veces. |

<br>


![Gráfico que muestra cómo puede tener varias versiones de una imagen en la galería](./media/shared-image-galleries/shared-image-gallery.png)

### <a name="regional-support"></a>Compatibilidad regional

La compatibilidad regional para las galerías de imágenes compartidas se encuentra disponible como versión preliminar limitada, pero se expandirá con el tiempo. Para la versión preliminar limitada, aquí está la lista de las regiones donde puede crear galerías y la lista de regiones donde puede replicar cualquier imagen de la galería: 

| Crear galería en  | Replicar la versión en |
|--------------------|----------------------|
| Centro occidental de EE.UU.    |Todas las regiones públicas &#42;|
| Este de EE. UU. 2          ||
| Centro-Sur de EE. UU   ||
| Sudeste asiático     ||
| Europa occidental        ||
| Oeste de EE. UU.            ||
| Este de EE. UU            ||
| Centro de Canadá     ||
|                    ||



&#42; Para replicar en el Centro de Australia y Centro de Australia 2 debe tener su suscripción en la lista blanca. Para solicitar la inclusión en la lista blanca, visite: https://www.microsoft.com/en-au/central-regions-eligibility/

## <a name="scaling"></a>Escalado
La galería de imágenes compartidas le permite especificar el número de réplicas de las imágenes que quiere que Azure mantenga. Esto ayuda en los escenarios de implementación de varias VM, ya que las implementaciones de VM se pueden distribuir a las distintas réplicas, lo que reduce la probabilidad de que el proceso de creación de instancias quede limitado por la sobrecarga de una única réplica.

![Gráfico que muestra cómo puede escalar imágenes](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Replicación
La galería de imágenes compartidas también permite replicar las imágenes en otras regiones de Azure automáticamente. Cada versión de una imagen compartida se puede replicar en distintas regiones en función de lo que sea más conveniente para su organización. Un ejemplo es replicar siempre la imagen más reciente en varias regiones, mientras que todas las versiones anteriores solo están disponibles en una región. Esto puede ayudar a ahorrar costos de almacenamiento para las versiones de imágenes compartidas. 

Las regiones donde puede replicarse la versión de una imagen compartida se pueden actualizar después de su creación. El tiempo necesario para replicar en diferentes regiones depende de la cantidad de datos que se copien y del número de regiones en las que se replicará la versión. En algunos casos, esto puede tardar varias horas. Mientras se produce la replicación, puede ver el estado de replicación por región. Una vez que se complete la replicación de la imagen en una región, podrá implementar una VM o VMSS con esa versión de la imagen en la región.

![Gráfico que muestra cómo puede replicar imágenes](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access
Dado que la galería de imágenes compartidas, la imagen compartida y la versión de la imagen compartida son recursos, pueden compartirse con los controles integrados nativos de Azure RBAC. Con RBAC puede compartir estos recursos con otros usuarios, entidades de servicio y grupos de su organización. El ámbito de uso compartido de estos recursos es dentro del mismo inquilino de Azure AD. Una vez que un usuario tiene acceso a la versión de la imagen compartida, puede implementar una VM o un conjunto de escalado de máquinas virtuales en cualquiera de las suscripciones a las que tenga acceso dentro del mismo inquilino de Azure AD que la versión de la imagen compartida.  Aquí está la matriz de uso compartido que ayuda a entender a lo que el usuario tiene acceso:

| Compartido con el usuario     | Galería de imágenes compartidas | Imagen compartida | Versión de la imagen compartida |
|----------------------|----------------------|--------------|----------------------|
| Galería de imágenes compartidas | SÍ                  | Sí          | SÍ                  |
| Imagen compartida         | Sin                    | SÍ          | SÍ                  |
| Versión de la imagen compartida | Sin                    | No           | SÍ                  |



## <a name="billing"></a>Facturación
No hay ningún cargo adicional por usar el servicio de la galería de imágenes compartidas. Se le cobrará por los siguientes recursos:
- Costos de almacenamiento de las versiones de imágenes compartidas. Esto depende del número de réplicas de la versión y del número de regiones en las que se replique la versión.
- Cargos de salida de red para la replicación desde la región de origen de la versión en las regiones replicadas.

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

**P.** ¿Cómo puedo registrarme en la versión preliminar pública de la galería de imágenes compartidas?
 
 A. Para registrarse en la versión preliminar pública de la galería de imágenes compartidas, debe ejecutar los siguientes comandos desde cada una de las suscripciones en las que tiene pensado crear una galería de imágenes compartidas, una definición de imágenes o recursos de la versión de imagen, y también dónde va a implementar las máquinas virtuales con las versiones de la imagen.

**CLI**: 

```bash 
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register --name Microsoft.Compute
```

**PowerShell**: 

```powershell
Register-AzureRmProviderFeature -FeatureName GalleryPreview -ProviderNamespace Microsoft.Compute
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

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


**P.** ¿Cómo puedo compartir mis imágenes entre suscripciones?
 
 A. Puede compartir imágenes entre suscripciones mediante el Control de acceso basado en roles (RBAC). Cualquier usuario que tenga permisos de lectura para una versión de la imagen, incluso entre suscripciones, será capaz de implementar una máquina virtual con la versión de la imagen.


**P.** ¿Puedo mover una imagen existente a la galería de imágenes compartidas?
 
 A. Sí. Hay tres escenarios basados en los tipos de imagen que pueda tener.

 Escenario 1: Si tiene una imagen administrada, puede crear una definición de la imagen y versión de la imagen a partir de ella.

 Escenario 2: Si tiene una imagen generalizada no administrada, puede crear una imagen administrada a partir de ella y, luego, crear una definición de la imagen y una versión de la imagen a partir de ella. 

 Escenario 3: Si tiene un VHD en el sistema de archivos locales, deberá cargar el VHD, crear una imagen administrada y, luego, crear una definición de la imagen y una versión de la imagen a partir de ella. 
    - Si el VHD es de una VM Windows, consulte [Carga de un VHD generalizado](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
    - Si el VHD es para una VM Linux, consulte [Cargar un disco duro virtual](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd).


**P.** ¿Puedo crear una versión de la imagen desde un disco especializado?

 A. No, actualmente no admitimos discos especializados como imágenes. Si tiene un disco especializado, deberá [crear una VM a partir del VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) al asociar el disco especializado a una nueva VM. Una vez que tenga una máquina virtual en ejecución, deberá seguir las instrucciones para crear una imagen administrada desde la [máquina virtual Windows](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) o la [máquina virtual Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Cuando tenga una imagen administrada generalizada, puede iniciar el proceso para crear una descripción de imagen compartida y la versión de la imagen.


**P.** ¿Puedo crear una galería de imágenes compartidas, la definición de la imagen y la versión de la imagen a través de Azure Portal?

 A. No, actualmente no admitimos la creación de ninguno de los recursos de la galería de imágenes compartidas a través de Azure Portal. Sin embargo, sí se admite la creación de los recursos de la galería de imágenes compartidas a través de la CLI, plantillas y SDK. PowerShell también se publicará pronto.

 
**P.** Una vez que se cree, ¿puedo actualizar la definición de la imagen o la versión de la imagen? ¿Qué tipo de detalles puedo modificar?

 A. A continuación se mencionan los detalles que se pueden actualizar en cada uno de los recursos:
 
Galería de imágenes compartidas:
- DESCRIPCIÓN

Definición de la imagen:
- vCPU recomendadas:
- Memoria
- DESCRIPCIÓN
- Fecha final del ciclo de vida

Versión de la imagen:
- Recuento de réplicas regionales
- Regiones de destino
- Exclusión de la versión más reciente
- Fecha final del ciclo de vida


**P.** Una vez que se cree, ¿puedo mover el recurso de la galería de imágenes compartidas a otra suscripción?

 A. No, no puede mover el recurso de la galería de imágenes compartidas a otra suscripción. Sin embargo, podrá replicar las versiones de la imagen en la galería a otras regiones según sea necesario.

**P.** ¿Puedo replicar las versiones de mis imágenes en las nubes: Azure China 21Vianet, Azure Alemania y la nube de Azure Government? 

 A. No, no puede replicar las versiones de la imagen en las nubes.

**P.** ¿Puedo replicar las versiones de mis imágenes entre suscripciones? 

 A. No, puede replicar las versiones de la imagen en varias regiones de una suscripción y usarla en otras suscripciones a través de RBAC.

**P.** ¿Puedo compartir versiones de imágenes entre los inquilinos de Azure AD? 

 A. No, actualmente la galería de imágenes compartidas no admite el uso compartido de versiones de imágenes entre los inquilinos de Azure AD. Sin embargo, puede usar la característica de Ofertas privadas en Azure Marketplace para lograr esto.


**P.** ¿Cuánto tarda la replicación de versiones de imágenes en las regiones de destino?

 A. El tiempo de replicación de la versión de una imagen depende totalmente del tamaño de la imagen y del número de regiones en las que se va a replicar. Sin embargo, como práctica recomendada para obtener los mejores resultados, se recomienda mantener una imagen pequeña y las regiones de origen y de destino cerca. Puede comprobar el estado de la replicación mediante la marca -ReplicationStatus.


**P.** ¿Cuántas galerías de imágenes compartidas puedo crear en una suscripción?

 A. La cuota predeterminada es: 
- 10 galerías de imágenes compartidas por suscripción, por región
- 200 definiciones de imágenes por suscripción, por región
- 2000 versiones de imágenes por suscripción, por región


**P.** ¿Cuál es la diferencia entre la región de origen y la región de destino?

 A. La región de origen es la región en la que se creará la versión de la imagen, y las regiones de destino son las regiones donde se almacenará una copia de la versión de la imagen. Para cada versión de una imagen, solo puede tener una región de origen. Además, asegúrese de pasar la ubicación de la región de origen como una de las regiones de destino cuando cree una versión de la imagen.  


**P.** ¿Cómo puedo especificar la región de origen al crear la versión de la imagen?

 A. Al crear una versión de la imagen, puede usar la etiqueta **--location** en la CLI y la etiqueta **-Location** en PowerShell para especificar la región de origen. Asegúrese de que la imagen administrada que usa como imagen de base para crear la versión de la imagen esté en la misma ubicación que la ubicación en la que va a crear la versión de la imagen. Además, asegúrese de pasar la ubicación de la región de origen como una de las regiones de destino cuando cree una versión de la imagen.  


**P.** ¿Cómo puedo especificar el número de réplicas de la versión de la imagen que se creará en cada región?

 A. Hay dos maneras de especificar el número de réplicas de la versión de la imagen que se creará en cada región:
 
1. El recuento de réplicas regionales que especifica el número de réplicas que desea crear para cada región. 
2. El recuento de réplicas comunes que sea el recuento predeterminado por región en caso de que no se especifique el recuento de réplicas regionales. 

Para especificar el recuento de réplicas regionales, pase la ubicación junto con el número de réplicas que quiere crear en esa región del modo siguiente: "Centro-sur de EE. UU. = 2". 

Si el recuento de réplicas regionales no se especifica con cada ubicación, el número predeterminado de réplicas será el recuento de réplicas comunes que especificó. 

Para especificar el recuento de réplicas comunes en la CLI, use el argumento **--replica-count** en el comando `az sig image-version create`.


**P.** ¿Puedo crear la galería de imágenes compartidas en una ubicación diferente de en la que quiero crear la definición de la imagen y la versión de la imagen?

 A. Sí, esto es posible. Pero como práctica recomendada, le animamos a mantener el grupo de recursos, la galería de imágenes compartidas, la definición de la imagen y la versión de la imagen en la misma ubicación.


**P.** ¿Cuáles son los cargos por uso de la galería de imágenes compartidas?

 A. No hay ningún cargo por usar el servicio de la galería de imágenes compartidas, excepto los cargos de almacenamiento de las versiones de las imágenes y los cargos de salida de red para la replicación de las versiones de las imágenes de la región de origen a las regiones de destino.

**P.** ¿Qué versión de API debo usar para crear la galería de imágenes compartidas, la definición de la imagen, la versión de la imagen y VM/VMSS a partir de la versión de la imagen?

 A. Para implementaciones de VM y conjuntos de escalado de máquinas virtuales que usan una versión de la imagen, se recomienda usar la API versión 2018-04-01 o posterior. Para trabajar con galerías de imágenes compartidas, definiciones de imágenes y versiones de imágenes, se recomienda usar la API versión 2018-06-01. 
