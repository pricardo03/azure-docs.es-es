---
title: Aprovisionamiento de grupos de Azure Batch desde una imagen personalizada |Microsoft Docs
description: Cree un grupo de Batch a partir de una imagen personalizada para aprovisionar los nodos de proceso que contienen el software y los datos que se necesitan para la aplicación. Las imágenes personalizadas son una manera eficaz de configurar los nodos de proceso para ejecutar las cargas de Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 10/04/2018
ms.author: danlep
ms.openlocfilehash: 7d0526dd233afd3976b22d257300681db0bfcead
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885225"
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Uso de una imagen personalizada para crear un grupo de máquinas virtuales 

Al crear un grupo en Azure Batch con Configuración de máquina virtual, se especifica una imagen de máquina virtual (VM) que proporciona el sistema operativo para cada nodo de proceso en el grupo. Puede crear un grupo de máquinas virtuales con una imagen de Azure Marketplace compatible o con una personalizada (una imagen de máquina virtual que haya creado y configurado). La imagen personalizada debe ser un recurso de *imagen administrada* en la misma región y suscripción de Azure que la cuenta de Batch.

## <a name="why-use-a-custom-image"></a>¿Por qué usar una imagen personalizada?

Al proporcionar una imagen personalizada, tiene el control sobre la configuración y el tipo del sistema operativo y los discos de datos que se usarán. La imagen personalizada puede incluir los datos de referencia y las aplicaciones que están disponibles en los nodo de grupo de Batch en cuanto se aprovisionan.

El uso de una imagen personalizada le ahorra tiempo a la hora de preparar los nodos de proceso de su grupo para ejecutar la carga de trabajo de Batch. Aunque siempre puede utilizar una imagen de Azure Marketplace e instalar el software en cada nodo de proceso una vez que se haya aprovisionado, el uso de una imagen personalizada podría ser más eficiente.

El uso de una imagen personalizada configurada para su escenario puede proporcionar varias ventajas:

- **Configurar el sistema operativo (OS)**. La configuración del disco de sistema operativo de la imagen se puede personalizar. 
- **Preinstalar las aplicaciones.** Puede preinstalar aplicaciones en el disco del sistema operativo, lo que es más eficaz y menos propenso a errores que instalar las aplicaciones después de aprovisionar los nodos de proceso mediante una tarea de inicio.
- **Ahorrar tiempo de reinicio en las máquinas virtuales.** La instalación de aplicaciones normalmente requiere reiniciar la máquina virtual, lo que lleva mucho tiempo. Puede ahorrar tiempo de reinicio si instala previamente las aplicaciones. 
- **Copiar grandes cantidades de datos una vez.** Convierta en estáticos parte de los datos de la imagen personalizada administrada copiándolos en los discos de datos de una imagen administrada. Solo debe hacerse una vez y los datos estarán disponibles para cada nodo del grupo.
- **Elegir los tipos de disco.** Tiene la opción de usar almacenamiento premium para el disco del sistema operativo y el disco de datos.
- **Aumentar los grupos a tamaños grandes.** Al usar una imagen personalizada administrada para crear un grupo, este puede aumentar sin que tenga que realizar copias de los discos duros virtuales de los blobs de imágenes. 


## <a name="prerequisites"></a>Requisitos previos

- **Un recurso de imagen administrada**. Para crear un grupo de máquinas virtuales con una imagen personalizada, tiene que tener o crear un recurso de imagen administrada en la misma suscripción y región de Azure que la cuenta de Batch. La imagen debe crearse desde instantáneas del disco del sistema operativo de la máquina virtual y, opcionalmente, de sus discos de datos conectados. Para más información y los pasos para preparar una imagen administrada, consulte la siguiente sección. 
  - Use una imagen personalizada única para cada grupo que cree.
  - Para crear un grupo con la imagen mediante las API de Batch, especifique el **identificador de recurso** de la imagen, que tiene el formato `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. Para usar el portal, utilice el **nombre** de la imagen.  
  - El recurso de la imagen administrada debe existir durante la vigencia del grupo para permitir su escalado vertical y que se pueda eliminar después de eliminar el grupo.

- **Autenticación de Azure Active Directory (AAD)**. La API de cliente de Batch debe utilizar la autenticación de AAD. La compatibilidad de Azure Batch con AAD se documenta en [Autenticación de soluciones de servicio de Batch con Active Directory](batch-aad-auth.md).

## <a name="prepare-a-custom-image"></a>Preparación de una imagen personalizada

En Azure se puede preparar una imagen administrada a partir de instantáneas del sistema operativo de la máquina virtual de Azure y discos de datos, una máquina virtual de Azure general con discos administrados o un disco duro virtual local general que cargue. Para escalar grupos de Batch de forma confiable con una imagen personalizada se recomienda crear una imagen administrada *solo* con el primer método: instantáneas de los discos de la máquina virtual. Consulte los siguientes pasos para preparar la máquina virtual, tomar una instantánea y crear una imagen a partir de ella. 

### <a name="prepare-a-vm"></a>Preparación de la máquina virtual 

Si va a crear una máquina virtual para la imagen, use una imagen de Azure Marketplace admitida por Batch como imagen base para la imagen administrada y, luego, personalícela.  Para obtener una lista de las referencias de imagen de Marketplace de Azure compatibles con Azure Batch, consulte la operación [List node agent SKUs](/rest/api/batchservice/account/listnodeagentskus). No se puede usar una imagen de terceros como imagen base.

* Asegúrese de que la máquina virtual se crea con un disco administrado. Se trata de la configuración de almacenamiento predeterminada cuando se crea una máquina virtual.
* No instale extensiones de Azure, como la extensión de script personalizado, en la máquina virtual. Si la imagen contiene una extensión preinstalada, Azure podría experimentar problemas al implementar el grupo de Batch.
* Asegúrese de que la imagen del sistema operativo base que proporcione usa la unidad temporal predeterminada. El agente de nodo de Batch actualmente espera la unidad temporal predeterminada.
* Una vez que la máquina virtual está en ejecución, conéctese a ella a través de RDP (para Windows) o SSH (para Linux). Instale el software necesario o copie los datos deseados.  

### <a name="create-a-vm-snapshot"></a>Creación de una instantánea de máquina virtual

Una instantánea es una copia completa de solo lectura de un disco duro virtual. Para crear una instantánea del sistema operativo la máquina virtual o discos de datos, puede usar Azure Portal o herramientas de la línea de comandos. Para pasos y opciones de creación de una instantánea, consulte la guía para máquinas virtuales [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) o [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md).

### <a name="create-an-image-from-one-or-more-snapshots"></a>Creación de una imagen a partir de una o varias instantáneas

Para crear una imagen administrada a partir de una instantánea, use las herramientas de la línea de comandos de Azure, como el comando [az image create](/cli/azure/image#az_image_create). Puede crear una imagen mediante la especificación de una instantánea del disco del sistema operativo y, opcionalmente, una o varias instantáneas de disco de datos.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Creación de un grupo a partir de una imagen personalizada en el portal

Una vez que guarde la imagen personalizada y conozca su identificador de recurso o su nombre, cree un grupo de Batch a partir de esa imagen. En los siguientes pasos se muestra cómo crear un grupo desde Azure Portal.

> [!NOTE]
> Si va a crear el grupo mediante una de las API de Batch, asegúrese de que la identidad que usa para la autenticación de AAD tiene permisos para el recurso de imagen. Vea [Autenticación de soluciones de servicio de Batch con Active Directory](batch-aad-auth.md).
>

1. Vaya a la cuenta de Batch en Azure Portal. Esta cuenta debe estar en la misma suscripción y región que el grupo de recursos que contiene la imagen personalizada. 
2. En la ventana **Configuración** que aparece a la izquierda, seleccione el elemento de menú **Grupos**.
3. En la ventana **Grupos**, seleccione el comando **Agregar**.
4. En la ventana **Agregar grupo**, seleccione **Imagen personalizada (Linux/Windows)** en el menú desplegable **Tipo de imagen**. Desde el menú desplegable **Imagen de VM personalizada**, seleccione el nombre de la imagen (forma abreviada del identificador del recurso).
5. Seleccione el valor correcto de **publicador/oferta/SKU** para la imagen personalizada.
6. Especifique los valores requeridos restantes, incluido el **tamaño de nodo**, los **nodos dedicados de destino** y los **nodos de baja prioridad**, además de cualquier otro valor opcional que desee.

    Por ejemplo, para una imagen personalizada de Microsoft Windows Server Datacenter 2016, la ventana **Agregar grupo** aparece como se muestra a continuación:

    ![Agregar grupo desde imagen personalizada de Windows](media/batch-custom-images/add-pool-custom-image.png)
  
Para comprobar si un grupo existente se basa en una imagen personalizada, consulte la propiedad **Operating System** en la sección del resumen de recursos de la ventana **Grupo**. Si el grupo se creó a partir de una imagen personalizada, está establecida en **Imagen de máquina virtual personalizada**.

Todas las imágenes personalizadas asociadas a un grupo se muestran en la ventana **Propiedades** del grupo.

## <a name="considerations-for-large-pools"></a>Consideraciones para los grupos grandes

Si va a crear un grupo con cientos de máquinas virtuales o más con una imagen personalizada, es importante seguir las instrucciones anteriores para usar una imagen creada a partir de una instantánea de máquina virtual.

También tenga en cuenta lo siguiente:

- **Límites de tamaño**: Batch limita el tamaño de grupo a 2500 nodos de proceso especializados o 1000 nodos de prioridad baja con las imágenes personalizadas.

  Si usa la misma imagen (o varias imágenes basadas en la misma instantánea subyacente) para crear varios grupos, los nodos de proceso totales de los grupos no pueden superar los límites anteriores. No se recomienda usar una imagen o su instantánea subyacente para más de un grupo.

  Si configura el grupo con [grupos NAT de entrada](pool-endpoint-configuration.md) los límites pueden reducirse.

- **Tiempo de espera del cambio de tamaño**: si el grupo contiene un número de nodos fijo (no se escala automáticamente), aumente la propiedad resizeTimeout del grupo a un valor de 20 a 30 minutos. Si el grupo no alcanza su tamaño de destino en el tiempo de espera, realice otra [operación de cambio de tamaño](/rest/api/batchservice/pool/resize).

  Si tiene previsto un grupo con más de 300 nodos de proceso, es posible que necesite cambiar el tamaño del grupo varias veces para alcanzar el objetivo.

## <a name="next-steps"></a>Pasos siguientes

- Para información general más detallada acerca de Batch, consulte [Desarrollo de soluciones de procesos paralelos a gran escala con Batch](batch-api-basics.md).
