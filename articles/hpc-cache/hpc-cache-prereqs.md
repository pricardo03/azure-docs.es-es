---
title: Requisitos previos de Azure HPC Cache (versión preliminar)
description: Requisitos previos para usar Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: v-erkell
ms.openlocfilehash: fab85785ea183736b4012c349af143ef3a8c784a
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299913"
---
# <a name="prerequisites-for-azure-hpc-cache-preview"></a>Requisitos previos de Azure HPC Cache (versión preliminar)

Antes de usar Azure Portal para crear una instancia de Azure HPC Cache, asegúrese de que su entorno cumple estos requisitos.

## <a name="azure-subscription"></a>Suscripción de Azure

Se recomienda una suscripción de pago.

> [!NOTE]
> Durante la versión preliminar, el equipo de Azure HPC Cache debe agregar su suscripción a la lista de acceso antes de poder usarla para crear una instancia de caché. Este procedimiento ayuda a garantizar que cada cliente obtiene una capacidad de respuesta de alta calidad de sus cachés de prueba. Rellene [este formulario](https://aka.ms/onboard-hpc-cache) para solicitar acceso.

## <a name="network-infrastructure"></a>Infraestructura de red

Para poder usar la caché, primero es necesario configurar dos opciones relacionadas con la red:

* Una subred dedicada para la instancia de Azure HPC Cache
* Compatibilidad con DNS para que la caché pueda acceder al almacenamiento y otros recursos

### <a name="cache-subnet"></a>Subred de caché

Azure HPC Cache necesita una subred dedicada con estas cualidades:

* La subred debe tener al menos 64 direcciones IP disponibles.
* La subred no puede hospedar ninguna otra máquina virtual, ni siquiera para servicios relacionados, como máquinas cliente.
* Si usa varias instancias de Azure HPC Cache, cada una necesita su propia subred.

El procedimiento recomendado es crear una subred para cada caché. Puede crear una red virtual y una subred como parte de la creación de la caché.

### <a name="dns-access"></a>Acceso DNS

La caché necesita DNS para acceder a los recursos que están fuera de su red virtual. En función de los recursos que use, puede que tenga que configurar un servidor DNS personalizado y el reenvío entre ese servidor y los servidores de Azure DNS:

* Para acceder a los puntos de conexión de Azure Blob Storage y otros recursos internos, necesita el servidor DNS basado en Azure.
* Para acceder al almacenamiento local, debe configurar un servidor DNS personalizado que pueda resolver los nombres de host de almacenamiento.

Si solo necesita acceder a Blob Storage, puede usar el servidor DNS predeterminado proporcionado por Azure para la caché. Sin embargo, si necesita acceder a otros recursos, debe crear un servidor DNS personalizado y configurarlo para reenviar las solicitudes de resolución específicas de Azure al servidor de Azure DNS. (También se puede usar un servidor DNS sencillo para equilibrar la carga de las conexiones de cliente entre todos los puntos de montaje de caché disponibles).

Más información sobre las configuraciones de servidor DNS y redes virtuales de Azure en [Resolución de nombres de recursos en redes virtuales de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

## <a name="permissions"></a>Permisos

Antes de empezar a crear la caché, compruebe estos requisitos previos relacionados con los permisos.

* La instancia de caché necesita poder crear interfaces de red virtual (NIC). El usuario que crea la caché debe tener privilegios suficientes en la suscripción para crear las NIC.
<!-- There are several ways to authorize this access; read [Additional prerequisites](media/preview-prereqs.md) to learn more. -->

* Si usa Blob Storage, Azure HPC Cache necesita autorización para acceder a la cuenta de almacenamiento. Puede usar el control de acceso basado en rol (RBAC) para conceder a la caché acceso al almacenamiento de blobs. Se requieren dos roles: Colaborador de la cuenta de almacenamiento y Colaborador de datos de Storage Blob. Siga las instrucciones que se indican en [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) para agregar los roles.

## <a name="storage-infrastructure"></a>Administración del almacenamiento

La caché admite contenedores de blobs de Azure o exportaciones de almacenamiento de hardware NFS. Puede definir los destinos de almacenamiento al crear la caché o agregarlos después.

Cada tipo de almacenamiento tiene unos requisitos previos específicos. 

### <a name="nfs-storage-requirements"></a>Requisitos de almacenamiento de NFS

Si usa el almacenamiento de hardware local, la caché debe contar con una red de alto ancho de banda para acceder al centro de datos desde la subred. Se recomienda el acceso [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) o similar.

El almacenamiento de back-end de NFS debe ser una plataforma de hardware o software compatible. Póngase en contacto con el equipo de Azure HPC Cache para más información.

### <a name="blob-storage-requirements"></a>Requisitos de Blob Storage

Si quiere usar Azure Blob Storage con su instancia de caché, necesita una cuenta de almacenamiento compatible y un contenedor de blobs vacío o un contenedor rellenado con datos con formato de Azure HPC Cache, como se describe en [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md).

Cree la cuenta y el contenedor antes de intentar agregarlo como destino de almacenamiento.

Para crear una cuenta de almacenamiento compatible, use esta configuración:

* Rendimiento: **Estándar**
* Tipo de cuenta: **StorageV2 (de uso general v2)**
* Replicación: **Almacenamiento con redundancia local (LRS)**
* Nivel de acceso (predeterminado): **Acceso frecuente**

Se recomienda usar una cuenta de almacenamiento que esté en la misma ubicación que la caché.
<!-- need to clarify location - same region or same resource group or same virtual network? -->

También debe proporcionar a la aplicación de caché acceso a su cuenta de almacenamiento de Azure. Siga la descripción incluida en [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) para proporcionar a la caché los roles de acceso Colaborador de la cuenta de almacenamiento y Colaborador de datos de Storage Blob. Si no es el propietario de la cuenta de almacenamiento, pida a este que realice este paso.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una instancia de Azure HPC Cache](hpc-cache-create.md) desde Azure Portal
