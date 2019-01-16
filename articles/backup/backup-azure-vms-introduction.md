---
title: Acerca de la copia de seguridad de máquina virtual de Azure
description: Obtenga información sobre la copia de seguridad de máquina virtual de Azure y tenga en cuenta algunos procedimientos recomendados.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: cac219414418277ace09ba3a0b442f3bf74e6025
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107436"
---
# <a name="about-azure-vm-backup"></a>Acerca de la copia de seguridad de máquina virtual de Azure

En este artículo se describe cómo el [servicio Azure Backup](backup-introduction-to-azure-backup.md) realiza una copia de seguridad de las máquinas virtuales de Azure.

## <a name="backup-process"></a>Proceso de copia de seguridad

Aquí se explica cómo completa Azure Backup una copia de seguridad para las máquinas virtuales de Azure.

1. En el caso de las máquinas virtuales de Azure que están seleccionadas para la copia de seguridad, el servicio Azure Backup inicia un trabajo de copia de seguridad según la programación de copia de seguridad que especifique.
2. El servicio desencadena la extensión de copia de seguridad.
    - Las máquinas virtuales de Windows usan la extensión _VMSnapshot_.
    - Las máquinas virtuales de Linux usan la extensión _VMSnapshotLinux_.
    - La extensión se instala durante la primera copia de seguridad de la máquina virtual.
    - Para instalar la extensión, la máquina virtual debe estar ejecutándose.
    - Si no se está ejecutando la máquina virtual, el servicio Azure Backup toma una instantánea del almacenamiento subyacente (ya que no se produce ninguna escritura de la aplicación mientras se detiene la máquina virtual).
4. La extensión de copia de seguridad toma una instantánea coherente con el archivo, el bloqueo y el nivel de almacenamiento.
5. Después de tomar la instantánea, los datos se transfieren al almacén. Para que el proceso resulte más eficaz, el servicio identifica y transfiere únicamente los bloques de datos que han cambiado desde la última copia de seguridad (las diferencias).
5. Cuando finaliza la transferencia de datos, se elimina la instantánea y se crea un punto de recuperación.

![Arquitectura de copia de seguridad de máquinas virtuales de Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Cifrado de datos

Azure Backup no cifra los datos como parte del proceso de copia de seguridad. Azure Backup admite la copia de seguridad de las máquinas virtuales de Azure que estén cifradas mediante Azure Disk Encryption.

- La copia de seguridad de las máquinas virtuales se cifra solo con Clave de cifrado de Bitlocker (BEK) y se admite BEK junto con Clave de cifrado de claves (KEK), tanto para las máquinas virtuales de Azure administradas como para las no administradas.
- Las claves BEK (secretos) y KEK (claves) que se han incluido en la copia de seguridad se cifran para que se puedan leer y usar solo cuando los usuarios autorizados las restauran de nuevo en el almacén de claves.
- Puesto que la clave BEK también se incluye en una copia de seguridad, en los escenarios en los que la clave BEK se pierde, los usuarios autorizados pueden restaurar la clave BEK para el almacén de claves y recuperar la máquina virtual cifrada. Las claves y los secretos de las máquinas virtuales cifradas se incluyen en la copia de seguridad en formato cifrado, por lo que ni los usuarios no autorizados ni Azure pueden leerlos ni usar las claves y los secretos de la copia de seguridad. Solo los usuarios con el nivel adecuado de permisos pueden realizar la copia de seguridad y la restauración de las máquinas virtuales cifradas, así como las claves y los secretos.

## <a name="snapshot-consistency"></a>Coherencia de instantáneas

Para realizar instantáneas mientras se ejecutan las aplicaciones, Azure Backup toma instantáneas coherentes con la aplicación.

- **Máquinas virtuales Windows**: En el caso de las máquinas virtuales Windows, el servicio Azure Backup se coordina con el Servicio de instantáneas de volumen (VSS) para obtener una instantánea coherente de los discos de la máquina virtual.
    - De forma predeterminada, Azure Backup realiza copias de seguridad de VSS completas. [Más información](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Si desea cambiar la configuración para que Azure Backup realice copias de seguridad de las copia de VSS, establezca la clave del registro siguiente:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
- **Máquinas virtuales Linux**: Para asegurarse de que las máquinas virtuales Linux son coherentes con la aplicación, cuando Azure Backup toma una instantánea, puede usar la plataforma de scripts anteriores y posteriores de Linux. Puede escribir sus propios scripts personalizados para garantizar la coherencia al tomar una instantánea de la máquina virtual.
    -  Azure Backup solo invoca los scripts anteriores y posteriores que escriba el usuario.
    - Azure Backup marcará el punto de recuperación como coherente con la aplicación si la ejecución de los scripts anteriores y posteriores se realiza correctamente. Sin embargo, es el responsable final de la coherencia con la aplicación cuando se utilizan scripts personalizados.
    - [Más información](backup-azure-linux-app-consistent.md) acerca de la configuración de scripts.


#### <a name="consistency-types"></a>Tipos de coherencia

La siguiente tabla explica los diferentes tipos de coherencia.

**Instantánea** | **Basada en VSS** | **Detalles** | **Recuperación**
--- | --- | --- | ---
**Coherente con la aplicación** | Sí (solo Windows) | Las copias de seguridad coherentes con las aplicaciones capturan el contenido de la memoria y las operaciones de E/S pendientes. Las instantáneas coherentes con la aplicación utilizan el escritor VSS (o script anterior o posterior para Linux) que garantizan la coherencia de datos de la aplicación antes de que se produzca una copia de seguridad. | Al recuperar con una instantánea coherente con la aplicación, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones se inician en un estado coherente.
**Coherente con el sistema de archivos** | Sí (solo Windows) |  Las copias de seguridad coherentes de archivos permiten la coherencia de las copias de archivos de disco al tomar una instantánea de todos los archivos al mismo tiempo.<br/><br/> Los puntos de recuperación de Azure Backup son coherentes con:<br/><br/> - Las copias de seguridad de máquinas virtuales Linux que no tienen scripts anteriores o posteriores, o cuyo script dio error.<br/><br/> - Las copias de seguridad de máquinas virtuales Windows en las que VSS dio error. | Al llevar a cabo la recuperación con una instantánea coherente con la aplicación, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones deben implementar su propio mecanismo de corrección para asegurarse de que los datos restaurados son coherentes.
**Coherente frente a bloqueos** | Sin  | La coherencia frente a bloqueos suele tener lugar cuando una máquina virtual de Azure se cierra en el momento de la copia de seguridad.  Solamente se capturan y se hace una copia de seguridad de los datos que ya existen en el disco en el momento de la copia de seguridad.<br/><br/> Un punto de recuperación coherente con el bloqueo no garantiza la coherencia de datos para el sistema operativo o la aplicación. | No se garantiza, pero normalmente la máquina virtual se inicia y a continuación se comprueba el disco para corregir los daños producidos por errores. Los datos en memoria o las escrituras que no se hayan transferido al disco se pierden. Las aplicaciones implementan su propia comprobación de los datos. Por ejemplo, en el caso de una aplicación de base de datos, si el registro de transacciones tiene entradas que no están presentes en la base de datos, el software de la base de datos realiza una reversión hasta que los datos sean coherentes.


## <a name="service-and-subscription-limits"></a>Límites de servicio y suscripción

Azure Backup tiene una serie de limitaciones con respecto a las suscripciones y los almacenes.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Rendimiento de Backup

### <a name="disk-considerations"></a>Consideraciones de disco

La operación de copia de seguridad se optimiza al hacer una copia de seguridad de cada uno de los discos de la máquina virtual en paralelo. Por ejemplo, si una máquina virtual tiene cuatro discos, el servicio tratará de hacer copia de seguridad de los cuatro en paralelo. Para cada disco cuya copia de seguridad se realiza, Azure Backup lee los bloques en el disco y solo almacena los datos cambiados (copia de seguridad incremental).


### <a name="scheduling-considerations"></a>Consideraciones sobre programación

La programación de copias de seguridad afecta al rendimiento.

- Si configura las directivas de modo que todas las máquinas virtuales se incluyan en una copia de seguridad al mismo tiempo, ha programado un atasco de tráfico, ya que el proceso de copia de seguridad intenta realizar una copia de seguridad de los discos en paralelo.
- Para reducir el tráfico de copia de seguridad, realice copias de seguridad de las máquinas virtuales en distintos momentos del día, sin superponerse.


### <a name="time-considerations"></a>Consideraciones de tiempo

Aunque la mayoría del tiempo de copia de seguridad se dedica a leer y copiar los datos, hay otras operaciones que aportan al tiempo total necesario para la copia de seguridad de una máquina virtual:

- **Instalación de la extensión de copia de seguridad:**: tiempo necesario para instalar o actualizar la extensión de copia de seguridad.
- **Instantánea de desencadenador**: tiempo necesario para desencadenar una instantánea. Las instantáneas se desencadenan cerca de la hora de copia de seguridad programada.
- **Tiempo de espera en la cola**: Dado que el servicio Backup procesa trabajos de varias cuentas de almacenamiento de clientes al mismo tiempo, puede que los datos de la instantánea no se copien inmediatamente en el almacén de Recovery Services. En los momentos de máxima carga, puede que tarde hasta ocho horas antes de que se procesen las copias de seguridad. Sin embargo, el tiempo total de la copia de seguridad de máquina virtual será de menos de 24 horas para las directivas de copia de seguridad diarias.
- **Copia de seguridad inicial**: Aunque el tiempo total de copia de seguridad inferior a 24 horas es válido para las copias de seguridad incrementales, podría no serlo para la primera copia de seguridad. El tiempo necesario dependerá del tamaño de los datos y de cuándo se realizó la copia de seguridad.
- **Tiempo de transferencia de los datos**: el tiempo necesario para que el servicio de copia de seguridad calcule los cambios incrementales de la copia de seguridad anterior y transfiera esos cambios al almacén de almacenamiento.

### <a name="factors-affecting-backup-time"></a>Factores que afectan al tiempo de copia de seguridad

La copia de seguridad consta de dos fases: toma de instantáneas y transferencia de las instantáneas al almacén. El servicio Backup optimiza el almacenamiento.

- Al transferir los datos de instantánea a un almacén, el servicio solo transfiere cambios incrementales de la instantánea anterior.
- Para determinar los cambios incrementales, el servicio calcula la suma de comprobación de los bloques.
    - Si se cambia un bloque, el bloque se identifica como un bloque que se envía al almacén.
    - El servicio irá profundizando en cada uno de los bloques identificados buscando oportunidades para minimizar los datos que se van a transferir.
    - Después de evaluar todos los bloques cambiados, el servicio combina los cambios y los envía al almacén.

Las siguientes son algunas situaciones que pueden afectar al tiempo de copia de seguridad:

- **Copia de seguridad inicial de un disco recién agregado a una máquina virtual ya protegida**: Si una máquina virtual está realizando una copia de seguridad incremental y se le agrega un nuevo disco, la duración de la copia de seguridad puede ir más allá de 24 horas, ya que el disco recién agregado debe someterse a la replicación inicial, junto con la replicación diferencial de los discos existentes.
- **Fragmentación**: El producto de copia de seguridad examina los cambios incrementales entre las operaciones de dos copias de seguridad. Las operaciones de copia de seguridad son más rápidas cuando los cambios en el disco están colocados en comparación con los cambios que están distribuidos en el disco. 
- **Renovación**: La renovación diaria (para la replicación incremental) por cada disco de más de 200 GB puede tardar más de 8 horas para completar la operación. Si la máquina virtual tiene más de un disco y uno de esos discos está tardando más tiempo en completar su copia de seguridad, la operación de copia de seguridad general podría resultar afectada (o podría producirse un error). 
- **Modo de comparación de suma de comprobación**: El modo de comparación de suma de comprobación es más lento en comparación con el modo optimizado que usa Instant RP (Punto de recuperación instantáneo). Si ya usa Instant RP (Punto de recuperación instantáneo) y ha eliminado las instantáneas de nivel 1, la copia de seguridad se cambia al modo de comparación de suma de comprobación provocando que la operación de copia de seguridad tarde más de 24 horas (o dé errores).

## <a name="restore-considerations"></a>Consideraciones de la restauración

Una operación de restauración consta de dos tareas principales: copiar datos desde el almacén a la cuenta de almacenamiento seleccionada y crear la máquina virtual. El tiempo necesario para copiar lo datos desde el almacén depende de dónde estén almacenadas las copias de seguridad en Azure y de la cuenta de almacenamiento. El tiempo necesario para copiar los datos depende de:

- **Tiempo de espera en la cola**: Dado que el servicio procesa los trabajos de restauración desde varias cuentas de almacenamiento al mismo tiempo, las solicitudes de restauración se ponen en una cola.
- **Tiempo de copia de datos**: los datos se copian desde el almacén a la cuenta de almacenamiento. El tiempo de restauración depende del IOPS y del rendimiento de la cuenta de almacenamiento seleccionada que usa el servicio Azure Backup. Para reducir el tiempo de copia durante el proceso de restauración, seleccione una cuenta de almacenamiento que no esté cargada con otras lecturas y escrituras de aplicaciones.

## <a name="best-practices"></a>Procedimientos recomendados

Se recomienda seguir estos procedimientos recomendados al configurar copias de seguridad para máquinas virtuales:

- Actualice los almacenes a Instant RP (Punto de recuperación instantáneo) Revise estas [ventajas](backup-upgrade-to-vm-backup-stack-v2.md) y [consideraciones](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade) y después continúe con la actualización según estas [instrucciones](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  
- Considere la posibilidad de modificar el tiempo de directiva previsto de manera predeterminada (por ej. si la hora de la directiva predeterminada es 12:00 A.M., considere la posibilidad de incrementarlo en minutos) cuando se realicen las instantáneas de datos para asegurarse de que los recursos se utilizan de forma óptima.
- En el caso de una copia de seguridad de VM Premium en una característica diferente de Instant RP (Punto de recuperación instantáneo), se asigna aproximadamente el 50 % del espacio total de la cuenta de almacenamiento. El servicio de copia de seguridad requiere este espacio para copiar la instantánea en la misma cuenta de almacenamiento y para transferirla al almacén.
- Para restaurar máquinas virtuales desde un solo almacén, es muy recomendable usar diferentes [cuentas de almacenamiento v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade)  para asegurarse de que la cuenta de almacenamiento de destino no se verá limitada. Por ejemplo, cada máquina virtual debe tener una cuenta de almacenamiento diferente (si se restauran 10 VM, considere el uso de 10 cuentas de almacenamiento diferentes).
- Las restauraciones desde la capa de almacenamiento de nivel 1 (instantánea) se completarán en cuestión de minutos (ya que es la misma cuenta de almacenamiento), a diferencia de la capa de almacenamiento de nivel 2 (almacén), que puede tardar horas. Se recomienda usar la característica [Instant RP](backup-upgrade-to-vm-backup-stack-v2.md) (Punto de recuperación instantáneo) para restauraciones más rápidas en los casos en que los datos están disponibles en el nivel 1 (si los datos tienen que restaurarse desde el almacén, la operación tardará un tiempo).
- El límite del número de discos por cuenta de almacenamiento depende de la frecuencia con que se esté accediendo a los discos por parte de las aplicaciones que se ejecutan en VM de IaaS. Compruebe si se hospedan varios discos en una única cuenta de almacenamiento. Como práctica general, si hay entre 5 y 10 o más en una única cuenta de almacenamiento, equilibre la carga moviendo algunos discos para separar las cuentas de almacenamiento.

## <a name="backup-costs"></a>Costos de la copia de seguridad

Las máquinas virtuales de Azure incluidas en la copia de seguridad de Azure Backup están sujetas a los [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

- La facturación no se inicia hasta que se completa la primera copia de seguridad correcta. En este punto, se iniciará la facturación del almacenamiento y las instancias protegidas.
- La facturación continúa mientras haya datos de cualquier copia de seguridad almacenados en un almacén para la máquina virtual. Si detiene la protección en la máquina virtual, pero existen datos de copia de seguridad de la máquina virtual en un almacén, continúa la facturación.
- La facturación de una máquina virtual especificada solo se suspenderá si se detiene la protección y se eliminan los datos de la copia de seguridad.
- Cuando no hay ningún trabajo de copia de seguridad activo y se ha detenido la protección, el tamaño de la última copia de seguridad correcta de la máquina virtual se convierte en el tamaño de instancia protegida en el que se basa la factura mensual.
- El cálculo de instancias protegidas se basa en el tamaño *real* de la máquina virtual, que es la suma de todos los datos de la máquina virtual, excepto el almacenamiento temporal.
- Los precios se basan en los datos almacenados en el disco de datos.
- Los precios de la copia de seguridad de máquinas virtuales no se basa en el tamaño máximo admitido para cada disco de datos conectado a la máquina virtual.
- De forma similar, la factura de almacenamiento de copia de seguridad se basa en la cantidad de datos almacenados en Azure Backup, que es la suma de los datos reales de cada punto de recuperación.

Por ejemplo, veamos una máquina virtual de tamaño estándar A2 con dos discos de datos adicionales con un tamaño máximo de 4 TB cada uno. La tabla siguiente proporciona los datos almacenados en cada uno de estos discos:

| Tipo de disco | Tamaño máximo | Datos reales presentes |
| --------- | -------- | ----------- |
| Disco del sistema operativo |4095 GB |17 GB |
| Disco local/disco temporal |135 GB |5 GB (no incluidos en la copia de seguridad) |
| Disco de datos 1 |4095 GB |30 GB |
| Disco de datos 2 |4095 GB |0 GB |

- El tamaño real de la máquina virtual en este caso es de 17 GB + 30 GB + 0 GB = 47 GB.
- Este tamaño de instancias protegidas (47 GB) se convierte en la base de la factura mensual.
- A medida que crece la cantidad de datos en la máquina virtual, el tamaño de instancia protegida usado para la facturación también cambia en consecuencia.


## <a name="next-steps"></a>Pasos siguientes

Después de revisar el proceso de copia de seguridad y las consideraciones de rendimiento, haga lo siguiente:

- [Obtenga información sobre](../virtual-machines/windows/premium-storage-performance.md) el ajuste de las aplicaciones con un alto rendimiento con Azure Storage. El artículo se centra en el almacenamiento prémium, pero también es aplicable a los discos de almacenamiento estándar.
- [Empiece](backup-azure-arm-vms-prepare.md) con la copia de seguridad; para ello, revise las limitaciones y la compatibilidad de las máquinas virtuales, cree un almacén y prepare las máquinas virtuales para hacer copias de seguridad.
