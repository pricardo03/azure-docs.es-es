---
title: Acerca de la copia de seguridad de máquina virtual de Azure
description: Obtenga información sobre la copia de seguridad de máquina virtual de Azure y tenga en cuenta algunos procedimientos recomendados.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: raynew
ms.openlocfilehash: 93be913182db56941c346ef0cad47f70c0d614c9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706837"
---
# <a name="about-azure-vm-backup"></a>Acerca de la copia de seguridad de máquina virtual de Azure

Este artículo se describe cómo el [servicio Azure Backup](backup-introduction-to-azure-backup.md) realiza una copia de seguridad de máquinas virtuales (VM).

## <a name="backup-process"></a>Proceso de copia de seguridad

Aquí es cómo Azure Backup que se complete una copia de seguridad para máquinas virtuales de Azure:

1. Para máquinas virtuales de Azure que están seleccionados para copia de seguridad, copia de seguridad de Azure se inicia un trabajo de copia de seguridad según la programación de copia de seguridad que especifique.
1. Durante la primera copia de seguridad, se instala una extensión de copia de seguridad en la máquina virtual si se está ejecutando la máquina virtual.
    - Las máquinas virtuales de Windows, el _VMSnapshot_ extensión está instalada.
    - Las máquinas virtuales de Linux, el _VMSnapshotLinux_ extensión está instalada.
1. Para máquinas virtuales de Windows que ejecutan, copia de seguridad se coordina con Windows Volume Shadow Copy Service (VSS) para tomar una instantánea coherente con la aplicación de la máquina virtual.
    - De forma predeterminada, la copia de seguridad toma copias de seguridad completas de VSS.
    - Si la copia de seguridad no puede tomar una instantánea coherente con la aplicación, a continuación, toma una instantánea coherente con archivo de almacenamiento subyacente (ya que se produzca ninguna escritura de la aplicación mientras se detiene la máquina virtual).
1. Las máquinas virtuales de Linux, copia de seguridad toma una copia de seguridad coherentes con el archivo. En las instantáneas coherentes con la aplicación, debe personalizar manualmente los scripts anteriores y posteriores.
1. Después de copia de seguridad toma la instantánea, transfiere los datos en el almacén.
    - La copia de seguridad está optimizada con una copia de cada disco de máquina virtual en paralelo.
    - Para cada disco que la copia de seguridad, copia de seguridad de Azure lee los bloques en el disco e identifica y transfiere únicamente los bloques de datos que ha cambiado (delta) desde la copia de seguridad anterior.
    - Es posible que los datos de las instantáneas no se copien inmediatamente en el almacén. Podrían tardar horas en momentos de máxima actividad. Tiempo total de copia de seguridad para una máquina virtual será de menos de 24 horas para las directivas de copia de seguridad diarias.
 1. Los cambios realizados en una máquina virtual de Windows después de habilitar la copia de seguridad de Azure en él son:
    -   Microsoft Visual C++ Redistributable(x64) 2013 - 12.0.40660 está instalado en la máquina virtual
    -   Tipo de inicio del servicio de instantáneas de volumen (VSS) cambiado a automático de manual
    -   Se agrega el servicio IaaSVmProvider Windows

1. Una vez completada la transferencia de datos, se elimina la instantánea y se crea un punto de recuperación.

![Arquitectura de copia de seguridad de máquinas virtuales de Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encryption-of-azure-vm-backups"></a>Cifrado de copias de seguridad de máquina virtual de Azure

Al realizar la copia de seguridad de máquinas virtuales de Azure con Azure Backup, las máquinas virtuales se cifran en reposo con Storage Service Encryption (SSE). Azure puede también realizar una copia de seguridad de máquinas virtuales de Azure que se cifran mediante el uso de Azure Disk Encryption.

**Cifrado** | **Detalles** | **Soporte técnico**
--- | --- | ---
**Azure Disk Encryption** | Azure Disk Encryption cifra los discos de datos y de sistema operativo para máquinas virtuales de Azure.<br/><br/> Azure Disk Encryption se integra con claves de cifrado de BitLocker (BEKs), que están protegidas en un almacén de claves como secretos. Azure Disk Encryption se integra también con las claves de cifrado de claves de Azure Key Vault (KEK). | Azure Backup admite la copia de seguridad de máquinas virtuales de Azure administrados y cifran BEKs solo o con BEKs junto con KEK.<br/><br/> BEKs y KEK es una copia de seguridad y cifrado.<br/><br/> Dado que KEK y BEKs son una copia de seguridad, los usuarios con los permisos necesarios pueden restaurar las claves y secretos se copia en el almacén de claves si es necesario. Estos usuarios también podrán recuperar la máquina virtual cifrada.<br/><br/> No se puede leer los secretos y claves de cifrado por los usuarios no autorizados o por Azure.
**SSE** | Con SSE, Azure Storage proporciona cifrado en reposo, porque cifra automáticamente los datos antes de almacenarlos. Almacenamiento de Azure también descifra los datos antes de recuperarlos. | Azure Backup utiliza SSE para el cifrado en reposo de máquinas virtuales de Azure.

Para máquinas virtuales de Azure administrados y no administrados, copia de seguridad es compatible con las máquinas virtuales cifradas solo con BEKs o máquinas virtuales cifradas con BEKs junto con KEK.

La KEK (claves) y BEKs (secretos) de copia de seguridad se cifra. Puede leer y usar solo cuando se está restaurar el almacén de claves a los usuarios autorizados. Los usuarios no autorizados ni Azure puede leer o usar la copia de seguridad claves o secretos.

BEKs también se copian. Por lo tanto, si se pierden los BEKs, los usuarios autorizados pueden restaurar la BEKs al almacén de claves y recuperar las máquinas virtuales cifradas. Solo los usuarios con el nivel necesario de permisos pueden realizar copias de seguridad y restauración de cifrados de las máquinas virtuales o las claves y secretos.

## <a name="snapshot-creation"></a>Creación de instantáneas

Copia de seguridad de Azure toma instantáneas según la programación de copia de seguridad.

- **Máquinas virtuales de Windows:** Las máquinas virtuales de Windows, el servicio de copia de seguridad se coordina con VSS para tomar una instantánea coherente con la aplicación de los discos de máquina virtual.

  - De forma predeterminada, Azure Backup realiza copias de seguridad de VSS completas. [Más información](https://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
  - Para cambiar la configuración para que Azure Backup realiza copias de seguridad de copia VSS, establezca la siguiente clave del registro desde un símbolo del sistema:

    **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f**

- **Máquinas virtuales de Linux:** Para tomar instantáneas coherentes con la aplicación de máquinas virtuales Linux, use el script anterior de Linux y posteriores a la secuencia de comandos de marco de trabajo para escribir sus propios scripts personalizados para garantizar la coherencia.

  - Azure Backup invoca sólo las secuencias de comandos de pre y post escritos por el usuario.
  - Si los scripts anteriores y posteriores se ejecutan correctamente, Azure Backup marca el punto de recuperación como coherente con la aplicación. Sin embargo, cuando se usa scripts personalizados, eres responsable último de la coherencia de la aplicación.
  - [Obtenga más información](backup-azure-linux-app-consistent.md) acerca de cómo configurar las secuencias de comandos.

### <a name="snapshot-consistency"></a>Coherencia de instantáneas

La siguiente tabla explica los diferentes tipos de coherencia de la instantánea:

**Instantánea** | **Detalles** | **Recuperación** | **Consideración**
--- | --- | --- | ---
**Coherente con la aplicación** | Las copias de seguridad coherentes con las aplicaciones capturan el contenido de la memoria y las operaciones de E/S pendientes. Instantáneas coherentes con la aplicación utilizan un escritor de VSS (o los scripts anteriores y posteriores para Linux) para garantizar la coherencia de los datos de la aplicación antes de que se produce una copia de seguridad. | Cuando se va a recuperar una máquina virtual con una instantánea coherente con la aplicación, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones se inician en un estado coherente. | Windows: Todas las instancias de VSS Writer son correctas<br/><br/> Linux: Los scripts anteriores o posteriores están configurados y son correctos
**Sistema de archivos coherente** | Las copias de seguridad coherentes del sistema de archivos proporcionan coherencia tomando una instantánea de todos los archivos al mismo tiempo.<br/><br/> | Cuando se va a recuperar una máquina virtual con una instantánea coherente del sistema de archivos, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones deben implementar su propio mecanismo de corrección para asegurarse de que los datos restaurados son coherentes. | Windows: Se produjeron errores en algunas instancias de VSS Writer <br/><br/> Linux: El valor predeterminado (si los scripts anteriores y posteriores no están configurados o no se pudo)
**Coherente frente a bloqueos** | Las instantáneas coherentes con bloqueos suelen producen si una máquina virtual de Azure se apaga en el momento de la copia de seguridad. Solamente se capturan y se hace una copia de seguridad de los datos que ya existen en el disco en el momento de la copia de seguridad.<br/><br/> Un punto de recuperación coherente con el bloqueo no garantiza la coherencia de datos para el sistema operativo o la aplicación. | Aunque no hay ninguna garantía, normalmente arranca la máquina virtual y, a continuación, se inicia una comprobación de disco para corregir errores por daños. Los datos en memoria o las operaciones de escritura que no se han transferido a disco antes de que el bloqueo se pierden. Las aplicaciones implementan su propia comprobación de los datos. Por ejemplo, una aplicación de base de datos puede usar su registro de transacciones para la comprobación. Si el registro de transacciones tiene entradas que no estén en la base de datos, el software de base de datos pone las transacciones hasta que los datos son coherentes. | La VM está en estado de cierre

## <a name="backup-and-restore-considerations"></a>Consideraciones de copia de seguridad y restauración

**Consideración** | **Detalles**
--- | ---
**Disco** | Copia de seguridad de discos de máquina virtual es paralela. Por ejemplo, si una máquina virtual tiene cuatro discos, el servicio de copia de seguridad intenta realizar una copia de seguridad de los cuatro discos en paralelo. La copia de seguridad es incremental (solo los datos modificados).
**Programación** |  Para reducir el tráfico de copia de seguridad, copia de seguridad diferentes máquinas virtuales en distintos momentos del día y asegúrese de que no se superponen los tiempos. La copia de seguridad de máquinas virtuales al mismo tiempo causa atascos del tráfico.
**Preparación de copias de seguridad** | Tenga en cuenta el tiempo necesario para preparar la copia de seguridad. El tiempo de preparación incluye la instalación o actualización de la extensión de copia de seguridad y desencadena una instantánea según la programación de copia de seguridad.
**Transferencia de datos** | Tenga en cuenta el tiempo necesario para que Azure Backup identificar los cambios incrementales desde la copia de seguridad anterior.<br/><br/> En una copia de seguridad incremental, Azure Backup determina los cambios al calcular la suma de comprobación del bloque. Si se cambia un bloque, se marca para la transferencia en el almacén. El servicio analiza los bloques identificados para intentar a fin de minimizar la cantidad de transferencia de datos. Después de evaluar todos los bloques cambiados, Azure Backup transfiere los cambios en el almacén.<br/><br/> Puede haber un retraso entre la realización de la instantánea y la copia en el almacén.<br/><br/> En las horas punta, puede tardar hasta ocho horas para las copias de seguridad que se va a procesar. El tipo de copia de seguridad de una máquina virtual será inferior a 24 horas para las copias de seguridad diarias.
**Copia de seguridad inicial** | Aunque el tiempo total de copia de seguridad para copias de seguridad incrementales es menor que 24 horas, que podría no ser el caso de la primera copia de seguridad. El tiempo necesario para la copia de seguridad inicial dependerá del tamaño de los datos y cuando se procesa la copia de seguridad.
**Restaurar cola** | Los procesos de copia de seguridad Azure trabajos de restauración de varias cuentas de almacenamiento al mismo tiempo, y pone las solicitudes de restauración en una cola.
**Restaurar copia** | Durante el proceso de restauración, los datos se copian desde el almacén en la cuenta de almacenamiento.<br/><br/> El tiempo total de restauración depende de las operaciones de E/S por segundo (IOPS) y el rendimiento de la cuenta de almacenamiento.<br/><br/> Para reducir el tiempo de copia, seleccione una cuenta de almacenamiento que no esté cargada con otras lecturas y escrituras de aplicaciones.

### <a name="backup-performance"></a>Rendimiento de Backup

Estos escenarios comunes pueden afectar a la hora de copia de seguridad total:

- **Agregar un nuevo disco a una máquina virtual de Azure protegida:** Si una máquina virtual está en proceso de copia de seguridad incremental y se agrega un nuevo disco, aumentará el tiempo de copia de seguridad. El tiempo total de copia de seguridad puede durar más de 24 horas debido a la replicación inicial del nuevo disco, junto con la replicación diferencial de los discos existentes.
- **Discos fragmentados:** Las operaciones de copia de seguridad son más rápidas cuando los cambios del disco son contiguos. Si los cambios se expanden horizontalmente y se fragmentan a lo largo de un disco, la copia de seguridad será más lenta.
- **Actividad de disco:** Si protegió los discos que se esté realizando la copia de seguridad incremental tiene una actividad diaria de más de 200 GB, copia de seguridad puede tardar mucho tiempo (más de ocho horas) para completar.
- **Versiones de copia de seguridad:** La versión más reciente de copia de seguridad (conocida como la versión de la restauración instantánea) usa un proceso más optimizado que la comparación de la suma de comprobación para identificar los cambios. Pero si usa la restauración instantánea y ha eliminado una instantánea de copia de seguridad, la copia de seguridad cambia en la comparación de la suma de comprobación. En este caso, la operación de copia de seguridad se superior a 24 horas (o un error).

## <a name="best-practices"></a>Procedimientos recomendados

Al configurar las copias de seguridad de máquina virtual, se recomienda seguir estas prácticas:

- Modificar las horas de programación predeterminado que se establecen en una directiva. Por ejemplo, si el tiempo predeterminado de la directiva es 12:00 A.M., incrementar el tiempo en varios minutos para que los recursos se utilizan de forma óptima.
- Para la copia de seguridad de máquinas virtuales que están usando almacenamiento premium, se recomienda ejecutar la versión más reciente de Azure Backup ([la restauración instantánea](backup-instant-restore-capability.md)). Si no está ejecutando la versión más reciente, copia de seguridad asigna aproximadamente 50 por ciento del espacio de almacenamiento total. El servicio de copia de seguridad requiere este espacio para copiar la instantánea a la misma cuenta de almacenamiento y para transferirlo al almacén.
- Si va a restaurar las máquinas virtuales desde un solo almacén, recomendamos encarecidamente que use diferentes [cuentas de almacenamiento de uso general v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) para asegurarse de que no se verá limitada a la cuenta de almacenamiento de destino. Por ejemplo, cada máquina virtual debe tener una cuenta de almacenamiento diferente. Por ejemplo, si se restauran 10 máquinas virtuales, use 10 cuentas de almacenamiento diferente.
- Las restauraciones de una capa de almacenamiento de uso general v1 (snapshot) se completará en cuestión de minutos, porque la instantánea está en la misma cuenta de almacenamiento. Restauraciones de la capa de almacenamiento de uso general v2 (almacén) pueden tardar horas. En casos donde los datos están disponibles en el almacenamiento de uso general v1, recomendamos que use el [la restauración instantánea](backup-instant-restore-capability.md) característica para restauraciones más rápidas. (Si los datos deben restaurarse desde un almacén, a continuación, se tardará más tiempo.)
- El límite del número de discos por cuenta de almacenamiento es relativa grado los discos están accediendo a las aplicaciones que se ejecutan en una infraestructura como servicio (IaaS) de máquina virtual. Como práctica general, si hay discos de 5 a 10 o más en una única cuenta de almacenamiento, equilibrar la carga moviendo algunos discos para separar las cuentas de almacenamiento.

## <a name="backup-costs"></a>Costos de la copia de seguridad

Las máquinas virtuales de Azure incluidas en la copia de seguridad de Azure Backup están sujetas a los [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

La facturación no se inicia hasta que finalice la primera copia de seguridad correcta. En este punto, se iniciará la facturación del almacenamiento y las máquinas virtuales protegidas. La facturación continúa siempre y cuando los datos de copia de seguridad de la máquina virtual se almacenan en un almacén. Si detiene la protección de una máquina virtual, pero existen datos de copia de seguridad para la máquina virtual en un almacén, la facturación continúa.

La facturación de una máquina virtual especificada solo se suspenderá si se detiene la protección y se eliminan los datos de la copia de seguridad. Cuando no hay ningún trabajo de copia de seguridad activo y se ha detenido la protección, el tamaño de la última copia de seguridad correcta de la máquina virtual se convierte en el tamaño de instancia protegida en el que se basa la factura mensual.

El cálculo del tamaño de instancia protegida se basa en el *real* tamaño de la máquina virtual. Tamaño de la máquina virtual es la suma de todos los datos de la máquina virtual, excepto el almacenamiento temporal. Precios se basan en los datos reales que se almacenan en los discos de datos, no en el máximo tamaño admitido para cada disco de datos que se adjunta a la máquina virtual.

De forma similar, la factura de almacenamiento de copia de seguridad se basa en la cantidad de datos que se almacenan en Azure Backup, que es la suma de los datos reales de cada punto de recuperación.

Por ejemplo, tomar una máquina virtual A2 de tamaño estándar que tiene dos discos de datos adicionales con un tamaño máximo de 4 TB. La siguiente tabla muestra los datos almacenados en cada uno de estos discos:

**Disco** | **Tamaño máximo** | **Datos reales presentes**
--- | --- | ---
Disco del sistema operativo | 4095 GB | 17 GB
Disco local/temporal | 135 GB | 5 GB (no incluidos en la copia de seguridad)
Disco de datos 1 | 4095 GB | 30 GB
Disco de datos 2 | 4095 GB | 0 GB

El tamaño real de la máquina virtual en este caso es de 17 GB + 30 GB + 0 GB = 47 GB. Este tamaño de instancias protegidas (47 GB) se convierte en la base de la factura mensual. A medida que crece la cantidad de datos en la máquina virtual, el tamaño de instancia protegida se utiliza para cambios de facturación para que coincida con.

## <a name="next-steps"></a>Pasos siguientes

Ahora, [preparar para la copia de seguridad de máquina virtual de Azure](backup-azure-arm-vms-prepare.md).
