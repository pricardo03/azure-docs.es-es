---
title: Incorporación de almacenamiento a una instancia de Azure HPC Cache (versión preliminar)
description: Definición de los destinos de almacenamiento para que Azure HPC Cache pueda usar el sistema NFS local o los contenedores de Azure Blob Storage para el almacenamiento de archivos a largo plazo
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: rohogue
ms.openlocfilehash: dbcc68bacf8a11a7a85d5fad7fb4435fd03c7f93
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72024562"
---
# <a name="add-storage-targets"></a>Incorporación de destinos de almacenamiento

Los *destinos de almacenamiento* son espacios de almacenamiento en servidores back-end para archivos a los que se accede a través de una instancia de Azure HPC Cache. Puede agregar almacenamiento NFS (como un sistema de hardware local) o almacenar datos en Azure Blob Storage.

Puede definir hasta diez destinos de almacenamiento diferentes para una caché. La caché presenta todos los destinos de almacenamiento en un espacio de nombres agregado.

Recuerde que las exportaciones de almacenamiento deben ser accesibles desde la red virtual de la caché. En el caso del almacenamiento en hardware local, es posible que tenga que configurar un servidor DNS que pueda resolver nombres de host para el acceso al almacenamiento de NFS. Obtenga más información en [Acceso DNS](hpc-cache-prereqs.md#dns-access).

Agregue destinos de almacenamiento después de crear su caché. El procedimiento es ligeramente diferente en función de si agrega almacenamiento de Azure Blob Storage o una exportación de NFS. A continuación se muestran los detalles de cada uno.

## <a name="open-the-storage-targets-page"></a>Abra la página de destinos de almacenamiento

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda. En la página Destinos de almacenamiento se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

![captura de pantalla del vínculo de destinos de almacenamiento en la barra lateral, en el encabezado Configurar, que se encuentra entre los encabezados de categoría Configuración y Supervisión](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Incorporación de un nuevo destino de almacenamiento de Azure Blob Storage

Un nuevo destino de almacenamiento de Azure Blob Storage necesita un contenedor de blobs vacío o un contenedor rellenado con datos con el formato de sistema de archivos en la nube de Azure HPC Cache. Obtenga más información sobre la carga previa de un contenedor de blobs en [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md).

Para definir un contenedor de blobs de Azure, escriba esta información.

![captura de pantalla de la página de incorporación de destino de almacenamiento, rellenada con información para un nuevo destino de almacenamiento de Azure Blob Storage](media/hpc-cache-add-blob.png)

<!-- need to replace screenshot after note text is updated with both required RBAC roles and also with correct search term -->

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* **Target type** (Tipo de destino): elija **Blob**.
* **Storage account** (Cuenta de almacenamiento): seleccione la cuenta con el contenedor al que se va a hacer referencia.

  Tendrá que autorizar a la instancia de caché para acceder a la cuenta de almacenamiento, tal como se describe en [Incorporación de los roles de acceso](#add-the-access-control-roles-to-your-account).

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prereqs.md#blob-storage-requirements).

* **Storage container** (Contenedor de almacenamiento): seleccione el contenedor de blobs para este destino.

* **Virtual namespace path** (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

### <a name="add-the-access-control-roles-to-your-account"></a>Incorporación de los roles de control de acceso a la cuenta

Azure HPC Cache usa el [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index) para autorizar el acceso de la aplicación de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) y [Colaborador de datos de Storage Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Puede realizar esta tarea con anterioridad, o bien hacer clic en un vínculo de la página donde se agrega un destino de almacenamiento de Azure Blob Storage.

Pasos para agregar los roles de RBAC:

1. Abra la página **Control de acceso (IAM)** correspondiente a la cuenta de almacenamiento. El vínculo de la página **Add storage target** (Agregar destino de almacenamiento) abre automáticamente esta página para la cuenta seleccionada.

1. Haga clic en **+** en la parte superior de la página y elija **Agregar una asignación de roles**.

1. Seleccione el rol "Colaborador de la cuenta de almacenamiento" en la lista.

1. En el campo **Asignar acceso a**, deje el valor predeterminado seleccionado ("Usuario, grupo o entidad de servicio de Azure AD").  

1. En el campo **Seleccionar**, busque "hpc".  Esta cadena debe coincidir con una entidad de servicio, denominada "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache). Haga clic en ella para seleccionarla.

   > [!NOTE]
   > Si una búsqueda de "hpc" no funciona, pruebe a usar la cadena "storagecache" en su lugar. Es posible que los usuarios que se han unido a la versión preliminar en el primer momento necesiten usar el nombre anterior de la entidad de servicio.

1. Haga clic en el botón **Guardar** para agregar la asignación de roles a la cuenta de almacenamiento.

1. Repita este proceso para asignar el rol "Colaborador de datos de Storage Blob".  

![captura de pantalla de la GUI de Agregar asignación de roles](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Incorporación de un nuevo destino de almacenamiento NFS

Los destinos de almacenamiento NFS tienen algunos campos adicionales para especificar cómo se llega a la exportación del almacenamiento y cómo se almacenan en caché sus datos de forma eficaz. También puede crear varias rutas de acceso de espacio de nombres desde un único host NFS si dispone de más de una exportación.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/hpc-cache-add-nfs-target.png)

Proporcione esta información para un destino de almacenamiento respaldado por NFS:

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* **Target type** (Tipo de destino): elija **NFS**.

* **Hostname** (Nombre de host): escriba la dirección IP o el nombre de dominio completo del sistema de almacenamiento NFS. Use un nombre de dominio solo si la caché tiene acceso a un servidor DNS que puede resolverlo.

* **Usage model** (Modelo de uso): elija uno de los perfiles de almacenamiento en caché de datos en función del flujo de trabajo, tal como se describe en [Selección de un modelo de uso](#choose-a-usage-model), a continuación.

### <a name="nfs-namespace-paths"></a>Rutas de acceso del espacio de nombres NFS

Un destino de almacenamiento NFS puede tener varias rutas de acceso virtuales, siempre y cuando cada ruta represente una exportación o un subdirectorio diferente sen el mismo sistema de almacenamiento.

Cree todas las rutas de acceso de un destino de almacenamiento.
<!-- You can create multiple namespace paths to represent different exports on the same NFS storage system, but you must create them all from one storage target. -->

Rellene estos valores para cada ruta de acceso del espacio de nombres:

* **Virtual namespace path** (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

<!--  The virtual path should start with a slash ``/``. -->

* **NFS export path** (Ruta de acceso de exportación de NFS): escriba la ruta de acceso a la exportación de NFS.

* **Subdirectory path** (Ruta de acceso del subdirectorio): si desea montar un subdirectorio específico de la exportación, indíquelo aquí. En caso contrario, deje este campo en blanco.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

### <a name="choose-a-usage-model"></a>Selección de un modelo de uso
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Cuando cree un destino de almacenamiento que apunte a un sistema de almacenamiento NFS, deberá elegir el *modelo de uso* de ese destino. Este modelo determina cómo se almacenan los datos en caché.

* Read heavy (Lectura intensiva): elija esta opción si usa la caché principalmente para acelerar el acceso de lectura de datos.

* Read/write (Lectura y escritura): elija esta opción si los clientes usan la memoria caché para leer y escribir.

* Clients bypass the cache (Los clientes omiten la caché): elija esta opción si los clientes escriben los datos directamente en el sistema de almacenamiento, sin escribir primero en la memoria caché.

## <a name="next-steps"></a>Pasos siguientes

Después de crear destinos de almacenamiento, considere la posibilidad de realizar una de estas tareas:

* [Montaje de la instancia de Azure HPC Cache](hpc-cache-mount.md)
* [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md)
