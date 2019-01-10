---
title: Acerca de la copia de seguridad de máquina virtual de Azure
description: Obtenga información sobre la copia de seguridad de máquina virtual de Azure y tenga en cuenta algunos procedimientos recomendados.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: 9a80671a72f059e24a8cebc5de803af9261ad829
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53743959"
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
4. La extensión de copia de seguridad toma una instantánea coherente con la aplicación y de nivel de almacenamiento.
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

Azure Backup intenta completarse tan pronto como sea posible y consumir tantos recursos como pueda.

- En un intento por maximizar la velocidad, el proceso de copia de seguridad trata de copiar cada uno de los discos de la máquina virtual en paralelo.
- Por ejemplo, si una máquina virtual tiene cuatro discos, el servicio tratará de hacer copia de seguridad de los cuatro en paralelo.
- El factor más importante que determina el tráfico de copia de seguridad de la cuenta de almacenamiento es el número de discos de los que se va a realizar una copia de seguridad.
- Todas las operaciones de E/S están limitadas por la *capacidad de proceso de destino de un blob*, que tiene un límite de 60 MB por segundo.
- Para cada disco cuya copia de seguridad se realiza, Azure Backup lee los bloques en el disco y solo almacena los datos cambiados (copia de seguridad incremental). Puede usar los valores de rendimiento promedio siguientes para calcular la cantidad de tiempo que tarda en realizarse una copia de seguridad de un disco de un tamaño determinado.

    **operación** | **Mejor rendimiento**
    --- | ---
    Copia de seguridad inicial | 160 Mbps |
    Copia de seguridad incremental | 640 Mbps  <br><br> El rendimiento disminuye considerablemente si las diferencias de datos están dispersas por el disco.|



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


- **Copia de seguridad inicial de un disco recién agregado a una máquina virtual ya protegida**: Si una máquina virtual está experimentando una copia de seguridad incremental, cuando se agrega un disco nuevo, la copia de seguridad podría perder el acuerdo de nivel de servicio de un día, según el tamaño del nuevo disco.
- **Aplicación fragmentada**: Si una aplicación está mal configurada, podría no ser la óptima para el almacenamiento:
    - Si la instantánea contiene muchas escrituras pequeñas y fragmentadas, el servicio tarda más tiempo en procesar los datos que escriben las aplicaciones.
    - En el caso de las aplicaciones que se ejecutan dentro de la máquina virtual, el bloque de escrituras de aplicaciones mínimo es de 8 KB. Si la aplicación utiliza un bloque de menos de 8 KB, se realiza el rendimiento de copia de seguridad.
- **Cuenta de almacenamiento sobrecargada**: se pudo programar una copia de seguridad cuando se ejecuta la aplicación en producción o si hay más de cinco a diez discos que se hospedan en la misma cuenta de almacenamiento.
- **Modo de comprobación de coherencia (CC)**: para los discos de más de 1 TB, la copia de seguridad podría estar en modo de comprobación de coherencia por dos razones:
    - El disco administrado se mueve como parte del reinicio de la máquina virtual.
    - Se promueve la instantánea a un blob base.


## <a name="restore-considerations"></a>Consideraciones de la restauración

Una operación de restauración consta de dos tareas principales: copiar datos desde el almacén a la cuenta de almacenamiento seleccionada y crear la máquina virtual. El tiempo necesario para copiar lo datos desde el almacén depende de dónde estén almacenadas las copias de seguridad en Azure y de la cuenta de almacenamiento. El tiempo necesario para copiar los datos depende de:

- **Tiempo de espera en la cola**: Dado que el servicio procesa los trabajos de restauración desde varias cuentas de almacenamiento al mismo tiempo, las solicitudes de restauración se ponen en una cola.
- **Tiempo de copia de datos**: los datos se copian desde el almacén a la cuenta de almacenamiento. El tiempo de restauración depende del IOPS y del rendimiento de la cuenta de almacenamiento seleccionada que usa el servicio Azure Backup. Para reducir el tiempo de copia durante el proceso de restauración, seleccione una cuenta de almacenamiento que no esté cargada con otras lecturas y escrituras de aplicaciones.

## <a name="best-practices"></a>Procedimientos recomendados

Se recomienda seguir estos procedimientos recomendados al configurar copias de seguridad para máquinas virtuales:

- No programe copias de seguridad de más de 100 VM de un almacén al mismo tiempo.
- Programe las copias de seguridad de máquinas virtuales durante horas de poca actividad. De esta forma, el servicio Backup utiliza IOPS para transferir datos desde la cuenta de almacenamiento al almacén.
- Si hace una copia de seguridad discos administrados, el servicio Azure Backup controla la administración de almacenamiento. Si va a realizar la copia de seguridad de discos no administrados:
    - Asegúrese de aplicar una directiva de copia de seguridad para VM que se aplique a varias cuentas de almacenamiento.
    - La misma programación de copia de seguridad debería proteger un máximo de 20 discos de una única cuenta de almacenamiento.
    - Si tiene más de 20 discos en una cuenta de almacenamiento, distribuya esas máquinas virtuales entre varias directivas para obtener el número de IOPS necesario durante la fase de transferencia del proceso de copia de seguridad.
    - No restaure una máquina virtual que se ejecute en almacenamiento prémium en la misma cuenta de almacenamiento. Si el proceso de la operación de restauración coincide con la operación de copia de seguridad, se reduce el número de IOPS disponible para copia de seguridad.
    - En el caso de copias de seguridad de VM premium de la versión 1 de la pila de copia de seguridad de VM, solo debería asignar el 50 % del espacio total de la cuenta de almacenamiento a fin de que el servicio Backup pueda copiar la instantánea a la cuenta de almacenamiento y transferir datos desde la cuenta de almacenamiento al almacén.
- Se recomienda usar diferentes cuentas de almacenamiento con el fin de restaurar las máquinas virtuales desde un único almacén. Así se evita la limitación y se obtiene un éxito del 100 % en la restauración con buen rendimiento.
- Las restauraciones del nivel de almacenamiento 1 se completarán en minutos con las restauraciones de almacenamiento de nivel 2 que se tarda unas horas. Se recomienda usar la [característica de RP de instantánea](backup-upgrade-to-vm-backup-stack-v2.md) para agilizar las restauraciones. Esto solo se puede aplicar a las máquinas virtuales de Azure administradas.


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

- Descargue la [hoja de cálculo de Excel de planeación de la capacidad](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) para probar con diferentes cifras para la programación de copias de seguridad.
- [Obtenga información sobre](../virtual-machines/windows/premium-storage-performance.md) el ajuste de las aplicaciones con un alto rendimiento con Azure Storage. El artículo se centra en el almacenamiento prémium, pero también es aplicable a los discos de almacenamiento estándar.
- [Empiece](backup-azure-arm-vms-prepare.md) con la copia de seguridad; para ello, revise las limitaciones y la compatibilidad de las máquinas virtuales, cree un almacén y prepare las máquinas virtuales para hacer copias de seguridad.
