---
title: Planeamiento de una implementación de Azure Files | Microsoft Docs
description: Conozca los puntos que debe tener en cuenta al planear una implementación de Azure Files.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: fecefbbed39f4fc12db79c7466006409e3da7dd1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64574474"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planeamiento de una implementación de Azure Files

[Azure Files](storage-files-introduction.md) ofrece recursos compartidos de archivos en la nube totalmente administrados a los que se puede acceder mediante el protocolo SMB estándar. Dado que Azure Files está totalmente administrado, su implementación en escenarios de producción resulta mucho más sencilla que la implementación y administración de un servidor de archivos o un dispositivo NAS. En este artículo se tratan las cuestiones que deben tenerse en cuenta al implementar un recurso compartido de archivos de Azure para su uso en producción dentro de la organización.

## <a name="management-concepts"></a>Conceptos de administración

 El siguiente diagrama muestra las construcciones de administración de Azure Files:

![Estructura de archivos](./media/storage-files-introduction/files-concepts.png)

* **Storage Account** (Cuenta de almacenamiento): Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Consulte el artículo sobre los [objetivos de escalado y rendimiento](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para información sobre la capacidad de la cuenta de almacenamiento.

* **Recurso compartido**: un recurso compartido de File Storage es un recurso compartido de archivos de SMB en Azure. Todos los directorios y archivos se deben crear en un recurso compartido principal. Una cuenta puede contener un número ilimitado de recursos compartidos, y un recurso compartido puede almacenar un número ilimitado de archivos, hasta la capacidad total de 5 TiB del recurso compartido de archivos.

* **Directorio**: una jerarquía de directorios opcional.

* **Archivo**: se trata de un archivo del recurso compartido. Un archivo puede tener un tamaño de hasta 1 TiB.

* **Formato de dirección URL**: en las solicitudes a un recurso compartido de archivos de Azure realizadas con el protocolo de REST de archivo, los archivos son direccionables mediante el formato de dirección URL siguiente:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Método de acceso a datos

Azure Files ofrece dos cómodos métodos de acceso a datos integrados que puede usar por separado o combinados entre sí para acceder a los datos:

1. **Acceso directo a la nube**: cualquier recurso compartido de archivos de Azure se puede montar mediante [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) o [Linux](storage-how-to-use-files-linux.md) con el protocolo de bloque de mensaje de servidor (SMB) estándar del sector o a través de la API de REST de archivo. Con SMB, las operaciones de lectura y escritura en archivos del recurso compartido se realizan directamente en el recurso compartido de archivos en Azure. Para montar mediante una máquina virtual en Azure, el cliente SMB del sistema operativo debe ser compatible al menos con SMB 2.1. Para montar en local, como en la estación de trabajo de un usuario, el cliente SMB compatible con la estación de trabajo debe ser compatible al menos con SMB 3.0 (con cifrado). Además de SMB, hay nuevas aplicaciones o servicios que pueden acceder directamente al recurso compartido de archivos a través de REST de archivo, lo que proporciona una interfaz de programación de aplicaciones escalable y sencilla para el desarrollo de software.
2. **Azure File Sync**: con Azure File Sync, se pueden replicar los recursos compartidos de archivos a servidores de Windows Servers de forma local o en Azure. Los usuarios accederían al recurso compartido de archivos mediante el servidor de Windows Server, por ejemplo, a través de un recurso compartido de SMB o NFS. Esto resulta útil en escenarios en los que es necesario acceder a los datos y modificarlos lejos de un centro de datos de Azure, como puede ser en una sucursal. Lo datos pueden replicarse entre varios puntos de conexión de Windows Server, por ejemplo, entre varias sucursales. Por último, los datos pueden colocarse en niveles en Azure Files, de modo que se pueda seguir accediendo a todos los datos a través del servidor, pero este no tenga una copia completa de ellos. Los datos se recuperan sin problemas cuando los abre el usuario.

En la tabla siguiente se muestra cómo pueden acceder los usuarios y las aplicaciones al recurso compartido de archivos de Azure:

| | Acceso directo a la nube | Azure File Sync |
|------------------------|------------|-----------------|
| ¿Qué protocolos necesita usar? | Azure Files admite SMB 2.1, SMB 3.0 y API de REST de archivo. | Acceda al recurso compartido de archivos de Azure a través de cualquier protocolo compatible de Windows Server (SMB, NFS, FTPS, etc.) |  
| ¿Dónde se ejecuta la carga de trabajo? | **En Azure**: Azure Files ofrece acceso directo a los datos. | **En local con red lenta**: los clientes de Windows, Linux y macOS pueden montar un recurso compartido de archivos de Windows local como una memoria caché rápida del recurso compartido de archivos de Azure. |
| ¿Qué nivel de ACL necesita? | Nivel de recurso compartido y archivo. | Nivel de recurso compartido, archivo y usuario. |

## <a name="data-security"></a>Seguridad de los datos

Azure Files tiene varias opciones integradas para garantizar la seguridad de los datos:

* Admite el cifrado en ambos protocolos inalámbricos: cifrado SMB 3.0 y REST de archivo a través de HTTPS. De forma predeterminada: 
    * Los clientes que admiten el cifrado SMB 3.0 enviar y reciben datos a través de un canal cifrado.
    * Los clientes que no admiten SMB 3.0 con cifrado pueden comunicarse entre centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado. No se permite a los clientes SMB comunicarse entre centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado.
    * Los clientes pueden comunicarse a través de REST de archivo con HTTP o HTTPS.
* Cifrado en reposo ([Azure Storage Service Encryption](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): La característica Storage Service Encryption está habilitada para todas las cuentas de almacenamiento. Los datos en reposo se cifran con claves completamente administradas. En el cifrado en reposo no se aumentan los costos de almacenamiento ni se reduce el rendimiento. 
* Requisito opcional de datos cifrados en tránsito: cuando está seleccionado, Azure Files rechaza el acceso a los datos a través de canales sin cifrar. En concreto, solo se permiten HTTPS y SMB 3.0 con conexiones de cifrado.

    > [!Important]  
    > La exigencia de transferencia segura de datos hace que los clientes SMB más antiguos que no son capaces de comunicarse con SMB 3.0 con cifrado experimenten un error. Para más información, consulte [Montaje en Windows](storage-how-to-use-files-windows.md), [Montaje en Linux](storage-how-to-use-files-linux.md) y [Montaje en macOS](storage-how-to-use-files-mac.md).

Para lograr la máxima seguridad, se recomienda encarecidamente habilitar siempre el cifrado en reposo y el cifrado de datos en tránsito cuando se usen clientes modernos para acceder a los datos. Por ejemplo, si tiene que montar un recurso compartido en una máquina virtual de Windows Server 2008 R2 que solo es compatible con SMB 2.1, debe permitir el tráfico sin cifrar a la cuenta de almacenamiento, dado que SMB 2.1 no admite el cifrado.

Si usa Azure File Sync para acceder al recurso compartido de archivos de Azure, use siempre HTTPS y SMB 3.0 con cifrado para sincronizar los datos en los servidores de Windows Server, independientemente de si se exige cifrado de datos en reposo.

## <a name="file-share-performance-tiers"></a>Niveles de rendimiento de un recurso compartido de archivos

Azure Files ofrece dos niveles de rendimiento: estándar y premium.

* Los **recursos compartidos de archivos estándar** tienen el respaldo de discos duros (HDD) que giran y ofrecen un rendimiento confiable para cargas de trabajo de E/S que no dan tanta importancia a la variabilidad del rendimiento, como recursos compartidos de archivos de uso general y entornos de desarrollo y pruebas. Los recursos compartidos de archivos estándar solo están disponibles en un modelo de facturación de pago por uso.
* Los **recursos compartidos de archivos Premium (versión preliminar)** están respaldados por discos en estado sólido (SSD) que proporcionan alto rendimiento y baja latencia de forma consistente en menos de 10 milisegundos en la mayoría de las operaciones de E/S para las cargas de trabajo intensivas con mayor uso de E/S, lo que hace que sean adecuados para una amplia variedad de cargas de trabajo como bases de datos, hospedaje de sitios web, entornos de desarrollo, etc. Los recursos compartidos de archivos Premium solo están disponibles en un modelo de facturación aprovisionada. Recursos compartidos de archivos Premium usan un modelo de implementación independiente de los recursos compartidos de archivos estándar.

Azure Backup está disponible para recursos compartidos de archivos de premium y Azure Kubernetes Service es compatible con premium recursos compartidos de archivos en la versión 1.13 y versiones posteriores.

Si desea obtener información sobre cómo crear un recurso compartido de archivos de premium, consulte nuestro artículo sobre el tema: [Cómo crear una cuenta de premium de Azure file storage](storage-how-to-create-premium-fileshare.md).

Actualmente, no se puede convertir directamente entre un recurso compartido de archivos estándar y un recurso compartido de archivos de premium. Si desea cambiar a cualquier nivel, debe crear un nuevo recurso compartido de archivos en ese nivel y copiar manualmente los datos desde el recurso compartido original en el nuevo recurso compartido que creó. Puede hacerlo mediante cualquiera de las herramientas de copia de archivos de Azure compatibles, como AzCopy.

> [!IMPORTANT]
> Recursos compartidos de archivos Premium están aún en versión preliminar, solo están disponibles con LRS y están disponibles en la mayoría de las regiones que ofrecen las cuentas de almacenamiento. Para averiguar si los recursos compartidos de archivos premium están disponibles actualmente en su región, consulte el [productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=storage) página de Azure.

### <a name="provisioned-shares"></a>Recursos compartidos aprovisionados

Recursos compartidos de archivos de Premium (versión preliminar) se aprovisionan en función de una relación fija de GiB/IOPS/rendimiento. Por cada GiB aprovisionado, se generará un IOPS y un rendimiento de 0,1 MiB por segundo en el recurso compartido hasta los límites máximos por recurso compartido. El aprovisionamiento mínimo que se permite es 100 GiB con un IOPS/rendimiento mínimos.

En su máximo esfuerzo, todos los recursos compartidos pueden aumentar hasta tres IOPS por GiB de almacenamiento aprovisionado durante 60 minutos, o más, según el tamaño del recurso compartido. Los nuevos recursos compartidos comienzan con todos los créditos de aumento según la capacidad aprovisionada.

Los recursos compartidos deben aprovisionarse en incrementos de 1 GB. Tamaño mínimo es 100 GB, siguiente tamaño es GIB 101 y así sucesivamente.

> [!TIP]
> Línea de base de e/s por segundo = 1 * aprovisionado GiB. (Hasta un máximo de 100 000 e/s por segundo).
>
> Límite de ráfaga = 3 * la línea de base de e/s por segundo. (Hasta un máximo de 100 000 e/s por segundo).
>
> tasa de salida = 60 MiB/s + 0,06 * aprovisionado GiB
>
> tasa de entrada = 40 MiB/s + 0,04 * aprovisionado GiB

Tamaño del recurso compartido se puede aumentar en cualquier momento, pero se puede reducir únicamente después de 24 horas desde el último incremento. Después de esperar 24 horas sin un aumento de tamaño, puede reducir el tamaño del recurso compartido tantas veces como sea necesario, hasta que aumente de nuevo. Los cambios de escala IOPS/rendimiento será efectivos en cuestión de minutos después del cambio de tamaño.

Es posible reducir el tamaño de los recursos compartidos aprovisionados por debajo de su GiB usado. Si lo hace, no se pierden los datos, pero, aún se facturará por el tamaño usado y el rendimiento (e/s por segundo de la línea de base, el rendimiento y e/s por segundo de ráfaga) del recurso compartido aprovisionado, no el tamaño usado de recepción.

En la tabla siguiente se muestra algunos ejemplos de estas fórmulas para los tamaños de recurso compartido aprovisionado:

(Los tamaños que se indican con un * están en versión preliminar pública limitada)

|Capacidad (GiB) | IOPS base | Ráfaga de e/s por segundo | Salida (MiB/s) | Entrada (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | Hasta 300     | 66   | 44   |
|500         | 500     | 1.500   | 90   | 60   |
|1024       | 1024   | Hasta 3.072   | 122   | 81   |
|5120       | 5120   | Hasta 15.360  | 368   | 245   |
|10,240 *     | 10,240  | Hasta 30.720  | 675 | 450   |
|33,792 *     | 33,792  | Hasta 100.000 | 2,088 | 1,392   |
|51,200 *     | 51,200  | Hasta 100.000 | 3132 | 2,088   |
|102,400 *    | 100 000 | Hasta 100.000 | 6,204 | 4,136   |

Actualmente, los tamaños de recurso compartido de archivos hasta 5 TB están en versión preliminar pública, mientras que los tamaños de hasta 100 TB están en versión preliminar pública limitada para solicitar acceso a la versión preliminar pública limitada completa [esta encuesta.](https://aka.ms/azurefilesatscalesurvey)

### <a name="bursting"></a>Creación de ráfagas

Recursos compartidos de archivos Premium pueden ampliar sus IOPS hasta un factor de tres. Ampliación es automatizada y opera basándose en un sistema de crédito. Ampliación funciona en base al mejor esfuerzo y el límite de ráfaga no es una garantía, pueden aumentar los recursos compartidos de archivos *hasta* el límite.

Créditos se acumulan en un depósito de ráfaga, siempre que el tráfico para el recurso compartido de archivos está por debajo de la línea de base de e/s por segundo. Por ejemplo, un recurso compartido de GiB 100 tiene previsto 100 IOPS. Si tráfico real en el recurso compartido estaba 40 IOPS para un intervalo específico de 1 segundo, el número de 60 IOPS sin usar se abona a un depósito de ráfaga. Estos créditos, a continuación, se usará más adelante cuando las operaciones, se superará la línea de base de e/s por segundo.

> [!TIP]
> Tamaño del depósito ráfaga = línea de base de e/s por segundo * 3600 * 2.

Cada vez que un recurso compartido supera la línea de base de e/s por segundo y tiene créditos en un depósito de ráfaga, emite ráfagas. Pueden seguir los recursos compartidos de ráfaga siempre quedan créditos, aunque los recursos compartidos de menores que 50 TiB sólo permanecerán en el límite de ráfagas de hasta una hora. Recursos compartidos de mayores que 50 TiB técnicamente puede superar este límite de una hora, de dos horas, pero esto se basa en el número de créditos de ráfaga acumulados. Cada E/S más allá de la línea de base de e/s por segundo consume un crédito y una vez que se consumen todos los créditos devolvería el recurso compartido a la línea de base de e/s por segundo.

Los créditos de recurso compartido tienen tres estados:

- Acumulado, cuando el recurso compartido de archivos usa menos de la línea de base de e/s por segundo.
- Disminuye cuando el recurso compartido de archivos es ampliación.
- Restante constante, cuando hay ninguna créditos o la línea de base de e/s por segundo están en uso.

Inicio de los recursos compartidos de archivo nuevo con el número total de créditos en el depósito de ráfaga. Si el recurso compartido de IOPS cae por debajo de la línea de base de e/s por segundo debido a la limitación por el servidor, no se acumularán créditos de ráfaga.

## <a name="file-share-redundancy"></a>Redundancia del recurso compartido de archivos

Recursos compartidos estándares de Azure Files admite tres opciones de redundancia de datos: almacenamiento con redundancia local (LRS), almacenamiento con redundancia de zona (ZRS) y almacenamiento con redundancia geográfica (GRS).

Azure premium de archivos, recursos compartidos solo admite almacenamiento con redundancia (LRS).

En las siguientes secciones se describen las diferencias entre las diferentes opciones de redundancia:

### <a name="locally-redundant-storage"></a>Almacenamiento con redundancia local

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Almacenamiento con redundancia de zona

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Almacenamiento con redundancia geográfica

> [!Warning]  
> Si usa el recurso compartido de archivos de Azure como punto de conexión en la nube en una cuenta de almacenamiento GRS, no debe iniciar la conmutación por error de la cuenta de almacenamiento. Si lo hace, la sincronización dejará de funcionar y también podría provocar una pérdida inesperada de datos en el caso de archivos recién organizados en capas. En caso de pérdida de una región de Azure, Microsoft activará la conmutación por error de la cuenta de almacenamiento de forma que sea compatible con Azure File Sync.

El almacenamiento con redundancia geográfica(GRS) está diseñado para proporcionar al menos el 99.99999999999999 % (dieciséis nueves) de durabilidad de objetos a lo largo de un año. Para ello, replica los datos a una región secundaria que se encuentra a cientos de kilómetros de la región primaria. Si la cuenta de almacenamiento tiene habilitado GRS, sus datos se mantienen incluso ante un apagón regional completo o un desastre del cual la región principal no se puede recuperar.

Si opta por el almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS), se debe saber que Azure Files no admite almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS) en cualquier región en este momento. Recursos compartidos de archivos en la cuenta de almacenamiento de RA-GRS funcionan como lo harían en las cuentas GRS y son los precios GRS cargados.

GRS replica los datos en otro centro de datos en una región secundaria, pero esos datos están disponibles para ser de solo lectura si Microsoft inicia una conmutación por error desde la región primaria a la región secundaria.

Para una cuenta de almacenamiento con GRS habilitado, todos los datos se replican primero con almacenamiento con redundancia local (LRS). Una actualización se confirma primero en la ubicación principal y se replican mediante LRS. La actualización luego se replica de manera asincrónica en la región secundaria con GRS. Cuando los datos se escriben en la ubicación secundaria, también se replican dentro de esa ubicación con LRS.

Las regiones primarias y secundarias administran las réplicas entre dominios de error y de actualización diferentes dentro de una unidad de escalado de almacenamiento. La unidad de escalado de almacenamiento es la unidad de replicación básica dentro del centro de datos. La replicación en este nivel se realiza por LRS; Para obtener más información, consulte [almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](../common/storage-redundancy-lrs.md).

Tenga en cuenta estos puntos cuando decida qué opción de replicación usar:

* Almacenamiento con redundancia de zona (ZRS) ofrece alta disponibilidad con replicación sincrónica y puede ser una opción mejor para algunos escenarios que GRS. Para más información sobre ZRS, consulte [ZRS](../common/storage-redundancy-zrs.md).
* La replicación asincrónica implica un retraso desde el momento en que se escriben los datos en la región principal hasta que se replican en la región secundaria. En el caso de un desastre regional, los cambios que no se hayan replicado en la región secundaria pueden perderse si dichos datos no se pueden recuperar desde la región principal.
* Con GRS, la réplica no está disponible para acceso de lectura o escritura a menos que Microsoft inicie la conmutación por error en la región secundaria. En el caso de una conmutación por error, tendrá acceso de lectura y escritura a dichos datos después de que se haya completado la conmutación por error. Para más información, consulte la [guía de recuperación ante desastres](../common/storage-disaster-recovery-guidance.md).

## <a name="data-growth-pattern"></a>Patrón de crecimiento de datos

En la actualidad, el tamaño máximo de un recurso compartido de archivos de Azure es de 5 TB (100 TB para archivo premium comparten versión preliminar pública limitada). Debido a esta limitación actual, debe tener en cuenta el crecimiento esperado de los datos al implementar un recurso compartido de archivos de Azure.

Es posible sincronizar varios recursos compartidos de archivos de Azure en un único servidor de archivos de Windows con Azure File Sync. Esto permite garantizar que los recursos compartidos de archivos anteriores de gran tamaño que pueda tener en un entorno local se incluyen en Azure File Sync. Para más información, consulte [Planeamiento de una implementación de Azure File Sync](storage-files-planning.md).

## <a name="data-transfer-method"></a>Método de transferencia de datos

Existen muchas opciones sencillas para la transferencia masiva de datos desde un recurso de archivos existente, como un recurso compartido de archivos local, a Azure Files. Algunas populares incluyen (lista no exhaustiva):

* **Azure File Sync**: como parte de una primera sincronización entre un recurso compartido de archivos de Azure (un "punto de conexión de nube") y un espacio de nombres de directorio de Windows (un "punto de conexión de servidor"), Azure File Sync replica todos los datos del recurso compartido de archivos existente en Azure Files.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: El servicio Azure Import/Export permite transferir de forma segura grandes cantidades de datos a un recurso compartido de archivos de Azure mediante el envío de unidades de disco duro a un centro de datos de Azure. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy es una herramienta de copia conocida que se incluye con Windows y Windows Server. Robocopy puede usarse para transferir datos a Azure Files al montar el recurso compartido de archivos localmente y luego usar la ubicación montada como destino en el comando de Robocopy.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy es una utilidad de línea de comandos diseñada para copiar datos a y desde Azure Files, así como Azure Blob Storage, mediante sencillos comandos con un rendimiento óptimo. AzCopy está disponible para Windows y Linux.

## <a name="next-steps"></a>Pasos siguientes
* [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
* [Implementación de Azure Files](storage-files-deployment-guide.md)
* [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
