---
title: Planeamiento de una implementación de Azure Files | Microsoft Docs
description: Conozca los aspectos que debe tener en cuenta al planear una implementación de Azure Files.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: b77d6fe03a051c019519f195d55cdeb00fb9afb2
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906276"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planeamiento de una implementación de Azure Files

[Azure Files](storage-files-introduction.md) ofrece recursos compartidos de archivos en la nube totalmente administrados, a los que se puede acceder mediante el protocolo SMB estándar. Dado que Azure Files está totalmente administrado, su implementación en escenarios de producción resulta mucho más sencilla que la implementación y administración de un servidor de archivos o un dispositivo NAS. En este artículo se tratan las cuestiones que deben tenerse en cuenta al implementar un recurso compartido de archivos de Azure para su uso en producción dentro de la organización.

## <a name="management-concepts"></a>Conceptos de administración

 El siguiente diagrama muestra las construcciones de administración de Azure Files:

![Estructura de archivos](./media/storage-files-introduction/files-concepts.png)

* **Storage Account** (Cuenta de almacenamiento): Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Vea [Objetivos de escalabilidad y rendimiento para cuentas de almacenamiento estándar](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para obtener más información sobre la capacidad de la cuenta de almacenamiento.

* **Recurso compartido**: un recurso compartido de File Storage es un recurso compartido de archivos de SMB en Azure. Todos los directorios y archivos se deben crear en un recurso compartido principal. Una cuenta puede contener un número ilimitado de recursos compartidos y un recurso compartido puede almacenar un número ilimitado de archivos, hasta una capacidad total del recurso compartido de archivos. La capacidad total de los recursos compartidos de archivos prémium y estándar es 100 TiB.

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
    * Los clientes que admiten el cifrado SMB 3.0 envían y reciben datos a través de un canal cifrado.
    * Los clientes que no admiten SMB 3.0 con cifrado pueden comunicarse dentro de centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado. No se permite a los clientes SMB comunicarse entre centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado.
    * Los clientes pueden comunicarse a través de REST de archivo con HTTP o HTTPS.
* Cifrado en reposo ([Azure Storage Service Encryption](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): La característica Storage Service Encryption está habilitada para todas las cuentas de almacenamiento. Los datos en reposo se cifran con claves completamente administradas. En el cifrado en reposo no se aumentan los costos de almacenamiento ni se reduce el rendimiento. 
* Requisito opcional de datos cifrados en tránsito: cuando está seleccionado, Azure Files rechaza el acceso a los datos a través de canales sin cifrar. En concreto, solo se permiten HTTPS y SMB 3.0 con conexiones de cifrado.

    > [!Important]  
    > La exigencia de transferencia segura de datos hace que los clientes SMB más antiguos que no son capaces de comunicarse con SMB 3.0 con cifrado experimenten un error. Para más información, consulte [Montaje en Windows](storage-how-to-use-files-windows.md), [Montaje en Linux](storage-how-to-use-files-linux.md) y [Montaje en macOS](storage-how-to-use-files-mac.md).

Para lograr la máxima seguridad, se recomienda encarecidamente habilitar siempre el cifrado en reposo y el cifrado de datos en tránsito cuando se usen clientes modernos para acceder a los datos. Por ejemplo, si tiene que montar un recurso compartido en una máquina virtual de Windows Server 2008 R2 que solo es compatible con SMB 2.1, debe permitir el tráfico sin cifrar a la cuenta de almacenamiento, dado que SMB 2.1 no admite el cifrado.

Si usa Azure File Sync para acceder al recurso compartido de archivos de Azure, use siempre HTTPS y SMB 3.0 con cifrado para sincronizar los datos en los servidores de Windows Server, independientemente de si se exige cifrado de datos en reposo.

## <a name="file-share-performance-tiers"></a>Niveles de rendimiento de un recurso compartido de archivos

Azure Files ofrece dos niveles de rendimiento: Estándar y Prémium.

### <a name="standard-file-shares"></a>Recursos compartidos de archivos estándar

Los recursos compartidos de archivos estándar están respaldados por unidades de disco duro (HDD). Los recursos compartidos de archivos estándar ofrecen un rendimiento confiable para cargas de trabajo de E/S menos sensibles a la variabilidad del rendimiento, como recursos compartidos de archivos de uso general y entornos de desarrollo y pruebas. Los recursos compartidos de archivos estándar solo están disponibles en un modelo de facturación de pago por uso.

> [!IMPORTANT]
> Si quiere usar recursos compartidos de archivos de más de 5 TiB, consulte la sección [Incorporación a recursos compartidos de archivos de mayor tamaño (nivel estándar)](#onboard-to-larger-file-shares-standard-tier) para ver los pasos de incorporación, además de la disponibilidad regional y las restricciones.

### <a name="premium-file-shares"></a>Recursos compartidos de archivos Prémium

Los recursos compartidos de archivos Prémium cuentan con el respaldo de unidades de estado sólido (SSD). Los recursos compartidos de archivos Prémium proporcionan un alto rendimiento y una baja latencia de forma coherente en menos de 10 milisegundos en la mayoría de las operaciones de E/S para las cargas de trabajo con un uso intensivo de E/S, lo que hace que sean adecuados para una amplia variedad de cargas de trabajo, como bases de datos, hospedaje de sitios web, y entornos de desarrollo. Los recursos compartidos de archivos Premium solo están disponibles en un modelo de facturación aprovisionada. Los recursos compartidos de archivos prémium usan un modelo de implementación independiente de los recursos compartidos de archivos estándar.

Azure Backup está disponible para recursos compartidos de archivos prémium y Azure Kubernetes Service es compatible con recursos compartidos de archivos prémium en la versión 1.13 y posteriores.

Para obtener información sobre cómo crear un recurso compartido de archivos prémium, consulte nuestro artículo sobre el tema: [Procedimientos para crea un recurso compartido de archivos de Azure premium](storage-how-to-create-premium-fileshare.md).

Actualmente, no se puede convertir directamente entre un recurso compartido de archivos estándar y un recurso compartido de archivos prémium. Para cambiar a cualquier nivel, debe crear un nuevo recurso compartido de archivos en ese nivel y copiar manualmente los datos del recurso compartido original en el nuevo recurso compartido que creó. Para hacerlo, puede usar cualquiera de las herramientas de copia de Azure Files compatibles, como Robocopy o AzCopy.

> [!IMPORTANT]
> Los recursos compartidos de archivos Prémium están disponibles con LRS en la mayoría de regiones que ofrecen cuentas de almacenamiento y con ZRS en subconjuntos de regiones más pequeños. Para averiguar si los recursos compartidos de archivos prémium están disponibles actualmente en su región, consulte la página [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=storage) para Azure. Para averiguar qué regiones admiten ZRS, consulte [Cobertura de soporte técnico y disponibilidad regional](../common/storage-redundancy-zrs.md#support-coverage-and-regional-availability).
>
> Para ayudarnos a clasificar por orden de prioridad las nuevas regiones y características del nivel Premium, rellene esta [encuesta](https://aka.ms/pfsfeedback).

#### <a name="provisioned-shares"></a>Recursos compartidos aprovisionados

Los recursos compartidos de archivos Premium se aprovisionan en función de una relación fija de GiB/IOPS/rendimiento. Por cada GiB aprovisionado, se generará un IOPS y un rendimiento de 0,1 MiB por segundo en el recurso compartido hasta los límites máximos por recurso compartido. El aprovisionamiento mínimo que se permite es 100 GiB con un IOPS/rendimiento mínimos.

En su máximo esfuerzo, todos los recursos compartidos pueden aumentar hasta tres IOPS por GiB de almacenamiento aprovisionado durante 60 minutos, o más, según el tamaño del recurso compartido. Los nuevos recursos compartidos comienzan con todos los créditos de aumento según la capacidad aprovisionada.

Los recursos compartidos se deben aprovisionar en incrementos de 1 GiB. El tamaño mínimo es de 100 GB; el siguiente, de 101 GiB y así sucesivamente.

> [!TIP]
> IOPS de línea de base = 1 * GiB aprovisionados. (Hasta un máximo de 100 000 IOPS).
>
> Límite de ráfaga = 3 * IOPS de línea de base. (Hasta un máximo de 100 000 IOPS).
>
> Velocidad de salida = 60 MiB/s + 0,06 * GiB aprovisionados
>
> Velocidad de entrada = 40 MiB/s + 0,04 * GiB aprovisionados

El tamaño de recurso compartido aprovisionado se especifica mediante la cuota de recurso compartidos. El recurso compartido puede aumentar en cualquier momento, pero solo se puede reducir una vez transcurridas 24 horas desde el último aumento. Después de esperar 24 horas sin un aumento de cuota, puede reducir el recurso compartido tantas veces como quiera hasta que lo vuelva a aumentar. Los cambios de escala de IOPS/rendimiento se aplicarán minutos después del cambio de tamaño.

Es posible reducir el tamaño del recurso compartido aprovisionados por debajo de sus GiB usados. Si lo hace, no se perderán datos, pero se le seguirá facturando el tamaño usado y seguirá recibiendo el rendimiento (IOPS de línea de base, rendimiento e IOPS de ráfaga) del recurso compartido aprovisionado, no del tamaño utilizado.

En la tabla siguiente se ilustran algunos ejemplos de estas fórmulas para los tamaños de recursos compartidos aprovisionados:

|Capacidad (GiB) | IOPS base | IOPS de ráfaga | Salida (MiB/s) | Entrada (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | Hasta 300     | 66   | 44   |
|500         | 500     | Hasta 1500   | 90   | 60   |
|1024       | 1024   | Hasta 3072   | 122   | 81   |
|5120       | 5120   | Hasta 15 360  | 368   | 245   |
|10 240      | 10 240  | Hasta 30 720  | 675 | 450   |
|33 792      | 33 792  | Hasta 100 000 | 2088 | 1392   |
|51 200      | 51 200  | Hasta 100 000 | 3132 | 2088   |
|102 400     | 100 000 | Hasta 100 000 | 6204 | 4136   |

> [!NOTE]
> El rendimiento de los recursos compartidos de archivos está sujeto a los límites de red de la máquina, el ancho de banda de red disponible, los tamaños de E/S y el paralelismo, entre muchos otros factores. Por ejemplo, en función de las pruebas internas con tamaños de e/s de lectura/escritura de 8 KiB, una sola máquina virtual de Windows (*Standard F16s_v2*) conectada al recurso compartido de archivos Premium a través de SMB podría alcanzar un valor de hasta 20 000 IOPS de lectura y 15 000 IOPS de escritura. Con tamaños de e/s de lectura/escritura de 512 MiB, la misma máquina virtual puede alcanzar un rendimiento de salida de 1,1 GiB/s y 370 MiB/s de entrada. Para lograr una escala de rendimiento máxima, distribuya la carga entre varias VM. Consulte en la [guía de solución de problemas](storage-troubleshooting-files-performance.md) algunos problemas de rendimiento comunes y soluciones alternativas.

#### <a name="bursting"></a>Creación de ráfagas

Los recursos compartidos de archivos prémium pueden crear ráfagas de su IOPS hasta un factor de tres. La creación de ráfagas está automatizada y funciona de acuerdo con un sistema de crédito. La creación de ráfagas funciona en la medida de lo posible y el límite de ráfaga no es una garantía: los recursos compartidos de archivos pueden crear ráfagas *hasta* el límite.

Cada vez que el tráfico para el recurso compartido de archivos se encuentra por debajo del valor de IOPS de la línea de base, se acumulan créditos en un cubo de ráfagas. Por ejemplo, un recurso compartido de 100 GiB tiene un valor de IOPS de línea de base de 100. Si el tráfico real del recurso compartido era de 40 IOPS para un intervalo específico de 1 segundo, el valor de 60 IOPS sin usar se agrega a un cubo de ráfagas. A continuación, estos créditos se usan más tarde si las operaciones superan el valor de IOPS de línea de base.

> [!TIP]
> Tamaño del cubo de ráfagas = IOPS de línea de base * 2 * 3600.

Cada vez que un recurso compartido supera el valor de IOPS de línea de base y tiene créditos en un cubo de ráfaga, crea ráfagas. Los recursos compartidos pueden seguir creando ráfagas mientras queden créditos, aunque los recursos compartidos de menos de 50 TiB solo permanecerán en el límite de ráfagas un máximo de una hora. Técnicamente, los recursos compartidos de más de 50 TiB pueden superar el límite de una hora, hasta dos horas, pero esto depende de la cantidad de créditos de ráfaga acumulados. Cada E/S más allá del valor de IOPS de línea de base consume un crédito y, una vez que se consumen todos los créditos, el recurso compartido volvería al valor de IOPS de la línea base.

Los créditos de recursos compartidos tienen tres estados:

- Acumulado: cuando el recurso compartido de archivos usa un valor inferior al de IOPS de línea de base.
- Declive: cuando se están creado ráfagas del recurso compartido de archivos.
- Remaining constant (Restante constante): cuando no hay créditos ni IOPS de línea de base en uso.

Los nuevos recursos compartidos de archivo empiezan con la cantidad total de créditos del cubo de ráfagas. Los créditos de ráfaga no se acumularán si el valor de IOPS del recurso compartido cae por debajo del valor de IOPS de la línea de base debido a una limitación del servidor.

## <a name="file-share-redundancy"></a>Redundancia del recurso compartido de archivos

Los recursos compartidos estándar de Azure Files admiten cuatro opciones de redundancia de datos: almacenamiento con redundancia local (LRS), almacenamiento con redundancia de zona (ZRS),almacenamiento con redundancia geográfica (GRS) y almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar).

Los recursos compartidos Prémium de Azure Files admiten tanto LRS como ZRS. ZRS está disponible actualmente en un subconjunto más pequeño de regiones.

En las siguientes secciones se describen las diferencias entre las diferentes opciones de redundancia:

### <a name="locally-redundant-storage"></a>Almacenamiento con redundancia local

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Almacenamiento con redundancia de zona

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Almacenamiento con redundancia geográfica

> [!Warning]  
> Si usa el recurso compartido de archivos de Azure como punto de conexión en la nube en una cuenta de almacenamiento GRS, no debe iniciar la conmutación por error de la cuenta de almacenamiento. Si lo hace, la sincronización dejará de funcionar y también podría provocar una pérdida inesperada de datos en el caso de archivos recién organizados en capas. En caso de pérdida de una región de Azure, Microsoft activará la conmutación por error de la cuenta de almacenamiento de forma que sea compatible con Azure File Sync.

El almacenamiento con redundancia geográfica(GRS) está diseñado para proporcionar al menos el 99.99999999999999 % (dieciséis nueves) de durabilidad de objetos a lo largo de un año. Para ello, replica los datos a una región secundaria que se encuentra a cientos de kilómetros de la región primaria. Si la cuenta de almacenamiento tiene habilitado GRS, sus datos se mantienen incluso ante un apagón regional completo o un desastre del cual la región principal no se puede recuperar.

Si opta por el almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS), debe saber que Azure Files no admite el almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS) en ninguna región en este momento. Los recursos compartidos de archivos de la cuenta de almacenamiento de RA-GRS funcionan como lo harían en las cuentas de GRS y se cobran de acuerdo con los precios de GRS.

GRS replica los datos en otro centro de datos en una región secundaria, pero esos datos están disponibles para ser de solo lectura si Microsoft inicia una conmutación por error desde la región primaria a la región secundaria.

Para una cuenta de almacenamiento con el GRS habilitado, todos los datos se replican primero con el almacenamiento con redundancia local (LRS). Una actualización se confirma primero en la ubicación principal y se replican mediante LRS. La actualización luego se replica de manera asincrónica en la región secundaria con GRS. Cuando los datos se escriben en la ubicación secundaria, también se replican dentro de esa ubicación con LRS.

Las regiones primarias y secundarias administran las réplicas entre dominios de error y de actualización diferentes dentro de una unidad de escalado de almacenamiento. La unidad de escalado de almacenamiento es la unidad de replicación básica dentro del centro de datos. LRS proporciona la replicación en este nivel. Para más información, vea [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](../common/storage-redundancy-lrs.md).

Tenga en cuenta estos puntos cuando decida qué opción de replicación usar:

* El almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) proporciona alta disponibilidad junto con el máximo de durabilidad al replicar los datos de forma sincrónica en tres zonas de disponibilidad de Azure y, después, replicar los datos de forma asincrónica en la región secundaria. También puede habilitar el acceso de lectura a la región secundaria. El almacenamiento con redundancia de zona geográfica (GZRS) está diseñado para proporcionar una durabilidad mínima del 99,99999999999999 % (16 nueves) de los objetos en un año determinado. Para obtener más información acerca de GZRS, consulte [Almacenamiento con redundancia de zona geográfica para obtener alta disponibilidad y durabilidad máxima (versión preliminar)](../common/storage-redundancy-gzrs.md).
* El almacenamiento con redundancia de zona (ZRS) ofrece alta disponibilidad con replicación sincrónica y puede ser una mejor opción para algunos escenarios que GRS. Para más información sobre ZRS, consulte [ZRS](../common/storage-redundancy-zrs.md).
* La replicación asincrónica implica un retraso desde el momento en que se escriben los datos en la región principal hasta que se replican en la región secundaria. En el caso de un desastre regional, los cambios que no se hayan replicado en la región secundaria pueden perderse si dichos datos no se pueden recuperar desde la región principal.
* Con GRS, la réplica no está disponible para acceso de lectura o escritura a menos que Microsoft inicie la conmutación por error en la región secundaria. En el caso de una conmutación por error, tendrá acceso de lectura y escritura a dichos datos después de que se haya completado la conmutación por error. Para más información, consulte la [guía de recuperación ante desastres](../common/storage-disaster-recovery-guidance.md).

## <a name="onboard-to-larger-file-shares-standard-tier"></a>Incorporación a recursos compartidos de archivos de mayor tamaño (nivel estándar)

Esta sección solo se aplica a los recursos compartidos de archivos estándar. Todos los recursos compartidos de archivos prémium están disponibles con 100 TiB de capacidad.

### <a name="restrictions"></a>Restricciones

- La conversión de cuentas de LRS/ZRS a GRS/GZRS no será posible en ninguna nueva cuenta de almacenamiento nueva creada con los recursos compartidos de archivos de gran tamaño habilitados.

### <a name="regional-availability"></a>Disponibilidad regional

Están disponibles recursos compartidos de archivos estándar con el límite de capacidad de 100 TiB a nivel global en todas las regiones de Azure.

- LRS: todas las regiones, excepto Norte de Sudáfrica y Oeste de Sudáfrica.
   - Las nubes nacionales (Government, Alemania, China) se admiten mediante PowerShell y la interfaz de la línea de comandos (CLI) de Azure. Sin compatibilidad con el portal. 
   - Este de EE. UU., Este de EE. UU. 2, Oeste de Europa: Se admiten todas las cuentas nuevas. Un número pequeño de cuentas existentes no ha completado el proceso de actualización. Puede comprobar si las cuentas de almacenamiento existentes han completado el proceso de actualización si intenta [habilitar los recursos compartidos de archivos grandes](storage-files-how-to-create-large-file-share.md).

- ZRS: todas las regiones, excepto Japón Oriental, Europa del Norte, Norte de Sudáfrica.
- GRS/GZRS: No compatible.

### <a name="enable-and-create-larger-file-shares"></a>Habilitación y creación de recursos compartidos de archivos de mayor tamaño

Para empezar a usar recursos compartidos de archivos grandes, consulte nuestro artículo [Habilitación y creación de recursos compartidos de archivos grandes](storage-files-how-to-create-large-file-share.md).

## <a name="data-growth-pattern"></a>Patrón de crecimiento de datos

Actualmente, el tamaño máximo de un recurso compartido de archivos de Azure es de 100 TiB. Debido a esta limitación actual, debe tener en cuenta el crecimiento esperado de los datos al implementar un recurso compartido de archivos de Azure.

Es posible sincronizar varios recursos compartidos de archivos de Azure en un único servidor de archivos de Windows con Azure File Sync. Esto permite garantizar que los recursos compartidos de archivos anteriores de gran tamaño que pueda tener en un entorno local se incluyen en Azure File Sync. Para más información, consulte [Planeamiento de una implementación de Azure File Sync](storage-files-planning.md).

## <a name="data-transfer-method"></a>Método de transferencia de datos

Existen muchas opciones sencillas para la transferencia masiva de datos desde un recurso de archivos existente, como un recurso compartido de archivos local, a Azure Files. Algunas populares incluyen (lista no exhaustiva):

* **[Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)** : como parte de una primera sincronización entre un recurso compartido de archivos de Azure (un "punto de conexión de nube") y un espacio de nombres de directorio de Windows (un "punto de conexión de servidor"), Azure File Sync replica todos los datos del recurso compartido de archivos existente en Azure Files.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : El servicio Azure Import/Export permite transferir de forma segura grandes cantidades de datos a un recurso compartido de archivos de Azure mediante el envío de unidades de disco duro a un centro de datos de Azure. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** : Robocopy es una herramienta de copia conocida que se incluye con Windows y Windows Server. Robocopy puede usarse para transferir datos a Azure Files al montar el recurso compartido de archivos localmente y luego usar la ubicación montada como destino en el comando de Robocopy.
* **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : AzCopy es una utilidad de línea de comandos diseñada para copiar datos a y desde Azure Files, así como Azure Blob Storage, mediante sencillos comandos con un rendimiento óptimo.

## <a name="next-steps"></a>Pasos siguientes
* [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
* [Implementación de Azure Files](storage-files-deployment-guide.md)
* [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
