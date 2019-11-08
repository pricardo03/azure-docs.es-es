---
title: Montaje de una instancia de Azure HPC Cache
description: Conexión de clientes a un servicio de Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: d906ed9a1a55e936c6374806a9037085c47e3b01
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582210"
---
# <a name="mount-the-azure-hpc-cache"></a>Montaje de la instancia de Azure HPC Cache

Después de crear la caché, los clientes NFS pueden acceder a ella con un sencillo comando de montaje.

El comando mount se compone de dos elementos:

* Una de las direcciones de montaje de la caché (que aparece en la página información general de la caché)
* La ruta de acceso del espacio de nombres virtual que estableció al crear el destino de almacenamiento

![captura de pantalla de la página de información general de la instancia de Azure HPC Cache, con un cuadro de resaltado alrededor de la lista de direcciones de montaje en la parte inferior derecha](media/hpc-cache-mount-addresses.png)

> [!NOTE] 
> Las direcciones de montaje de la caché corresponden a las interfaces de red dentro de la subred de la caché. En un grupo de recursos, estas NIC aparecen con nombres que terminan en `-cluster-nic-` y en un número. No las modifique ni elimine o la caché dejará de estar disponible.

Las rutas de acceso del espacio de nombres virtual se muestran en la página **Destinos de almacenamiento**. Haga clic en un nombre de destino de almacenamiento para ver sus detalles, incluidas las rutas de acceso del espacio de nombres agregadas asociadas a él.

![captura de pantalla del panel de destino de almacenamiento de la caché, con un cuadro de resaltado alrededor de una entrada en la columna Ruta de acceso de la tabla](media/hpc-cache-view-namespace-paths.png)

## <a name="mount-command-syntax"></a>Sintaxis del comando de montaje

Use un comando de montaje como el siguiente:

> sudo mount *cache_mount_address*:/*namespace_path* *local_path* {*options*}

Ejemplo:

```
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -orw,tcp,mountproto=tcp,vers3,hard
root@test-client:/tmp# 
```

Si este comando se ejecuta correctamente, el contenido de la exportación de almacenamiento será visible en el directorio ``hpccache`` del cliente.

> [!NOTE] 
> Los clientes deben poder acceder a la red virtual y la subred que aloja la caché. Por ejemplo, cree máquinas virtuales cliente dentro de la misma red virtual, o bien use un punto de conexión, una puerta de enlace u otra solución de la red virtual para acceder desde fuera. Recuerde que no se puede hospedar nada más en la subred de la caché.

### <a name="mount-command-options"></a>Opciones del comando de montaje

Para garantizar un montaje sólido del cliente, pase esta configuración y argumentos en el comando de montaje: 

``mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Configuración recomendada del comando de montaje | |
--- | --- 
``hard`` | Los montajes flexibles en Azure HPC Cache están asociados con errores de la aplicación y la posible pérdida de datos. 
``proto=netid`` | Esta opción es compatible con el control adecuado de los errores de la red NFS.
``mountproto=netid`` | Esta opción es compatible con el control adecuado de errores de red para las operaciones de montaje.
``retry=n`` | Configure ``retry=30`` para evitar errores de montaje transitorios. (Se recomienda usar un valor diferente en los montajes de primer plano).

## <a name="next-steps"></a>Pasos siguientes

* Para mover datos a los destinos de almacenamiento de la caché, lea [Rellenado del nuevo almacenamiento de blobs de Azure](hpc-cache-ingest.md).
