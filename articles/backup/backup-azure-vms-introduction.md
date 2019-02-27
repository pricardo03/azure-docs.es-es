---
title: Acerca de la copia de seguridad de máquina virtual de Azure
description: Obtenga información sobre la copia de seguridad de máquina virtual de Azure y tenga en cuenta algunos procedimientos recomendados.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: c38c457bbf428d7252cf57168685201a2ca227ba
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446807"
---
# <a name="about-azure-vm-backup"></a>Acerca de la copia de seguridad de máquina virtual de Azure

En este artículo se describe cómo el [servicio Azure Backup](backup-introduction-to-azure-backup.md) realiza una copia de seguridad de las máquinas virtuales de Azure.

## <a name="backup-process"></a>Proceso de copia de seguridad

Aquí se explica cómo completa Azure Backup una copia de seguridad para las máquinas virtuales de Azure.

1. En el caso de las máquinas virtuales de Azure que están seleccionadas para la copia de seguridad, el servicio Azure Backup inicia un trabajo de copia de seguridad según la programación de copia de seguridad que especifique.
2. Durante la primera copia de seguridad, se instala una extensión de copia de seguridad en la máquina virtual si se está ejecutando.
    - En máquinas virtuales Windows, se instala la extensión _VMSnapshot_.
    - En máquinas virtuales Linux, se instala la extensión _VMSnapshotLinux_.
3. En máquinas virtuales Windows que están en ejecución, Azure Backup se coordina con VSS para tomar una instantánea coherente con la aplicación de la máquina virtual.
    - De forma predeterminada, Azure Backup realiza copias de seguridad de VSS completas.
    - Si Backup no puede realizar una instantánea coherente con la aplicación, realiza una instantánea coherente con archivos del almacenamiento subyacente (ya que no se produce ninguna escritura en la aplicación mientras la máquina virtual está detenida).
4. En máquinas virtuales Linux, Azure Backup hace una copia de seguridad coherente con el archivo. Para obtener instantáneas coherentes con la aplicación, debe personalizar manualmente los scripts previos y posteriores.
5. Después de tomar la instantánea, los datos se transfieren al almacén. 
    - Azure Backup se optimiza mediante la copia de seguridad de cada disco de máquina virtual en paralelo.
    - Para cada disco de copia de seguridad, Azure Backup lee los bloques en el disco e identifica y transfiere solo los bloques de datos que han cambiado desde la copia de seguridad anterior (solo los cambios diferenciales).
    - Después de tomar la instantánea, los datos se transfieren al almacén.
    - Es posible que los datos de las instantáneas no se copien inmediatamente en el almacén. Podrían tardar horas en momentos de máxima actividad. El tiempo total de copia de seguridad de una máquina virtual será inferior a 24 horas para las directivas de copia de seguridad diarias.

6. Cuando finaliza la transferencia de datos, se elimina la instantánea y se crea un punto de recuperación.

![Arquitectura de copia de seguridad de máquinas virtuales de Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encrypting-azure-vm-backups"></a>Cifrado de copias de seguridad de máquinas virtuales de Azure

Al realizar la copia de seguridad de máquinas virtuales de Azure con Azure Backup, las máquinas virtuales se cifran en reposo con Storage Service Encryption (SSE). Además, Azure Backup puede hacer una copia de seguridad de las máquinas virtuales de Azure que estén cifradas mediante Azure Disk Encryption (ADE).


**Cifrado** | **Detalles** | **Soporte técnico**
--- | --- | ---
**ADE** | ADE cifra los discos de datos y de sistema operativo para máquinas virtuales de Azure.<br/><br/> ADE se integra con las claves de cifrado de BitLocker (BEK), protegidas en un almacén de claves como secretos, o con las claves de cifrado de claves (KEK) de Azure Key Vault. | Azure Backup admite la copia de seguridad de máquinas virtuales de Azure no administradas y cifradas con BEK solo o con BEK junto con KEK.<br/><br/> BEK con copia de seguridad y cifrado.<br/><br/> Puesto que se realiza una copia de seguridad de KEK y BEK, si es necesario, los usuarios con permisos pueden volver a restaurar las claves y los secretos en el almacén de claves y recuperar la máquina virtual cifrada.<br/><br/> Los usuarios no autorizados o Azure no pueden leer las claves y los secretos cifrados.
**SSE** | Con SSE, Azure Storage proporciona cifrado en reposo, porque cifra automáticamente los datos antes de almacenarlos y los descifra antes de la recuperación. | Azure Backup utiliza SSE para el cifrado en reposo de máquinas virtuales de Azure.

- La copia de seguridad de las VM se cifra solo con Clave de cifrado de BitLocker (BEK) y se admite BEK junto con Clave de cifrado de claves (KEK), tanto para las VM de Azure administradas como para las no administradas.
- Los datos BEK (secretos) y KEK (claves) están cifrados. Se pueden leer y usar solo cuando los usuarios autorizados los vuelven a restaurar en el almacén de claves. Ni los usuarios no autorizados ni Azure pueden leer o usar las claves o los secretos con copia de seguridad.
- Puesto que la clave BEK también se incluye en una copia de seguridad, si la clave BEK se pierde, los usuarios autorizados pueden restaurar la clave BEK para el almacén de claves y recuperar la máquina virtual cifrada. 
- Solo los usuarios con el nivel adecuado de permisos pueden realizar la copia de seguridad y la restauración de las máquinas virtuales cifradas, así como las claves y los secretos.



## <a name="taking-snapshots"></a>Realización de instantáneas

Azure Backup realiza instantáneas según la programación de copia de seguridad. 

- **Máquinas virtuales Windows**: En el caso de las máquinas virtuales Windows, el servicio Azure Backup se coordina con el Servicio de instantáneas de volumen (VSS) para obtener una instantánea coherente con la aplicación de los discos de la máquina virtual.
    - De forma predeterminada, Azure Backup realiza copias de seguridad de VSS completas. [Más información](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Si desea cambiar la configuración para que Azure Backup realice copias de seguridad de las copias de VSS, establezca la clave del registro siguiente mediante un símbolo del sistema: **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f**.
- **Máquinas virtuales Linux**: Si desea realizar instantáneas coherentes con la aplicación de una máquina virtual Linux, use el marco de script anterior y posterior para escribir scripts personalizados a fin de garantizar la coherencia.
    -  Azure Backup solo invoca los scripts anteriores y posteriores que escriba el usuario.
    - Azure Backup marcará el punto de recuperación como coherente con la aplicación si la ejecución de los scripts anteriores y posteriores se realiza correctamente. Sin embargo, es el responsable final de la coherencia con la aplicación cuando se utilizan scripts personalizados.
    - [Más información](backup-azure-linux-app-consistent.md) acerca de la configuración de scripts.


### <a name="snapshot-consistency"></a>Coherencia de instantáneas

La siguiente tabla explica los diferentes tipos de coherencia.

**Instantánea** | **Detalles** | **Recuperación** | **Consideración**
--- | --- | --- | ---
**Coherente con la aplicación** | Las copias de seguridad coherentes con las aplicaciones capturan el contenido de la memoria y las operaciones de E/S pendientes. Las instantáneas coherentes con la aplicación utilizan el escritor VSS (o script anterior o posterior para Linux) que garantizan la coherencia de datos de la aplicación antes de que se produzca una copia de seguridad. | Al recuperar con una instantánea coherente con la aplicación, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones se inician en un estado coherente. | Windows: Todas las instancias de VSS Writer son correctas<br/><br/> Linux: Los scripts anteriores o posteriores están configurados y son correctos
**Coherente con el sistema de archivos** | Las copias de seguridad coherentes de archivos permiten la coherencia de las copias de archivos de disco al tomar una instantánea de todos los archivos al mismo tiempo.<br/><br/> | Al llevar a cabo la recuperación con una instantánea coherente con la aplicación, la máquina virtual se inicia. No se pierden ni se dañan los datos. Las aplicaciones deben implementar su propio mecanismo de corrección para asegurarse de que los datos restaurados son coherentes. | Windows: Se produjeron errores en algunas instancias de VSS Writer <br/><br/> Linux: Valor predeterminado (si los scripts anteriores y posteriores no están configurados o tienen errores)
**Coherente frente a bloqueos** | La coherencia frente a bloqueos suele tener lugar cuando una máquina virtual de Azure se cierra en el momento de la copia de seguridad.  Solamente se capturan y se hace una copia de seguridad de los datos que ya existen en el disco en el momento de la copia de seguridad.<br/><br/> Un punto de recuperación coherente con el bloqueo no garantiza la coherencia de datos para el sistema operativo o la aplicación. | No se garantiza, pero normalmente la máquina virtual se inicia y a continuación se comprueba el disco para corregir los daños producidos por errores. Los datos en memoria o las escrituras que no se hayan transferido al disco se pierden. Las aplicaciones implementan su propia comprobación de los datos. Por ejemplo, en el caso de una aplicación de base de datos, si el registro de transacciones tiene entradas que no están presentes en la base de datos, el software de la base de datos realiza una reversión hasta que los datos sean coherentes. | La VM está en estado de cierre


## <a name="restore-considerations"></a>Consideraciones de la restauración 



**Consideración** | **Detalles**
--- | ---
**Disco** | La copia de seguridad de un disco de máquina virtual se realiza en paralelo. Por ejemplo, si una máquina virtual tiene cuatro discos, el servicio tratará de hacer copia de seguridad de los cuatro en paralelo. La copia de seguridad es incremental (solo los datos modificados).
**Programación** |  Para reducir el tráfico de copia de seguridad, realice copias de seguridad de las máquinas virtuales en distintos momentos del día, sin superponerse. La copia de seguridad de máquinas virtuales al mismo tiempo causa atascos del tráfico.
**Preparación de copias de seguridad** | Tenga en cuenta el tiempo de preparación de la copia de seguridad, que incluye la instalación o actualización de la extensión de copia de seguridad y el desencadenamiento de una instantánea según la programación de copia de seguridad.
**Transferencia de datos** | El tiempo necesario para que el servicio de copia de seguridad calcule los cambios incrementales de la copia de seguridad anterior.<br/><br/> En una copia de seguridad incremental, para determinar los cambios, el servicio calcula la suma de comprobación de los bloques. Si un bloque cambia, se identifica para su envío al almacén. El servicio explora los bloques identificados para intentar minimizar más los datos que se van a transferir. Después de evaluar todos los bloques modificados, los cambios se transfieren al almacén.<br/><br/> Puede haber un retraso entre la realización de la instantánea y la copia en el almacén.<br/><br/> En horas punta, las copias de seguridad pueden tardar hasta ocho horas en procesarse. El tipo de copia de seguridad de una máquina virtual será inferior a 24 horas para las copias de seguridad diarias.
**Copia de seguridad inicial** | Aunque el tiempo total de copia de seguridad inferior a 24 horas es válido para las copias de seguridad incrementales, podría no serlo para la primera copia de seguridad. El tiempo necesario dependerá del tamaño de los datos y de cuándo se realizó la copia de seguridad.
**Restaurar cola** | Azure Backup procesa los trabajos de restauración desde varias cuentas de almacenamiento al mismo tiempo y las solicitudes de restauración se ponen en una cola.
**Restaurar copia** | Durante el proceso de restauración, los datos se copian desde el almacén en la cuenta de almacenamiento.<br/><br/> El tiempo de restauración depende del IOPS y del rendimiento de la cuenta de almacenamiento.<br/><br/> Para reducir el tiempo de copia, seleccione una cuenta de almacenamiento que no esté cargada con otras lecturas y escrituras de aplicaciones.


### <a name="backup-performance"></a>Rendimiento de Backup

Estos escenarios comunes pueden afectar al tiempo de copia de seguridad:

- Agregar un disco nuevo a una máquina virtual de Azure protegida: si una máquina virtual está en proceso de copia de seguridad incremental y se agrega un nuevo disco, la copia de seguridad puede durar más de 24 horas debido a la replicación inicial del nuevo disco, junto con la replicación diferencial de los discos existentes.
- Discos fragmentados: las operaciones de copia de seguridad son más rápidas cuando se colocan los cambios en el disco. Si los cambios se expanden horizontalmente y se fragmentan a lo largo de un disco, la copia de seguridad será más lenta. 
- Actividad de disco: si los discos protegidos que están en proceso de copia de seguridad incremental tienen una actividad diaria de más de 200 GB, la copia de seguridad puede tardar mucho tiempo (más de ocho horas) en completarse. 
- Versiones de Backup: si usa la última versión de Backup (conocida como la versión de restauración instantánea), usa un proceso más optimizado que la comparación de la suma de comprobación para comparar los cambios. Si usa la última versión y ha eliminado una instantánea de copia de seguridad, la copia de seguridad cambia para utilizar la comparación mediante suma de comprobación, y la operación de copia de seguridad tardará más de 24 horas o se producirá un error.

## <a name="best-practices"></a>Procedimientos recomendados 
Se recomienda seguir estos procedimientos recomendados al configurar copias de seguridad para máquinas virtuales:

- Considere la posibilidad de modificar la hora de programación predeterminada establecida en una directiva. Por ejemplo, si la hora predeterminada de la directiva es 12:00 a. m., considere la posibilidad de incrementarla en minutos para optimizar el uso de los recursos.
- En el caso de una copia de seguridad de VM Premium en una característica diferente de Instant RP (Punto de recuperación instantáneo), se asigna aproximadamente el 50 % del espacio total de la cuenta de almacenamiento. El servicio de copia de seguridad requiere este espacio para copiar la instantánea en la misma cuenta de almacenamiento y para transferirla al almacén.
- Para restaurar máquinas virtuales desde un solo almacén, es muy recomendable usar diferentes[cuentas de almacenamiento v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) para asegurarse de que la cuenta de almacenamiento de destino no se verá limitada. Por ejemplo, cada máquina virtual debe tener una cuenta de almacenamiento diferente (si se restauran 10 VM, considere el uso de 10 cuentas de almacenamiento diferentes).
- Las restauraciones desde la capa de almacenamiento de nivel 1 (instantánea) se completarán en cuestión de minutos (ya que es la misma cuenta de almacenamiento), a diferencia de la capa de almacenamiento de nivel 2 (almacén), que puede tardar horas. Se recomienda usar la característica [Instant Restore](backup-instant-restore-capability.md) (Recuperación instantánea) para restauraciones más rápidas en los casos en que los datos están disponibles en el nivel 1 (si los datos tienen que restaurarse desde el almacén, la operación tardará un tiempo).
- El límite del número de discos por cuenta de almacenamiento depende de la frecuencia con que se esté accediendo a los discos por parte de las aplicaciones que se ejecutan en VM de IaaS. Compruebe si se hospedan varios discos en una única cuenta de almacenamiento. Como práctica general, si hay entre 5 y 10 o más en una única cuenta de almacenamiento, equilibre la carga moviendo algunos discos para separar las cuentas de almacenamiento.

## <a name="backup-costs"></a>Costos de la copia de seguridad

Las máquinas virtuales de Azure incluidas en la copia de seguridad de Azure Backup están sujetas a los [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

- La facturación no se inicia hasta que se completa la primera copia de seguridad correcta. En este punto, se iniciará la facturación del almacenamiento y las máquinas virtuales protegidas.
- La facturación continúa mientras haya datos de cualquier copia de seguridad almacenados en un almacén para la máquina virtual. Si detiene la protección de una máquina virtual, pero existen datos de copia de seguridad para la máquina virtual en un almacén, la facturación continúa.
- La facturación de una máquina virtual especificada solo se suspenderá si se detiene la protección y se eliminan los datos de la copia de seguridad.
- Cuando no hay ningún trabajo de copia de seguridad activo y se ha detenido la protección, el tamaño de la última copia de seguridad correcta de la máquina virtual se convierte en el tamaño de instancia protegida en el que se basa la factura mensual.
- El cálculo de instancias protegidas se basa en el tamaño *real* de la máquina virtual, que es la suma de todos los datos de la máquina virtual, excepto el almacenamiento temporal.
- Los precios se basan en los datos almacenados en el disco de datos.
- Los precios de la copia de seguridad de máquinas virtuales no se basa en el tamaño máximo admitido para cada disco de datos conectado a la máquina virtual.
- De forma similar, la factura de almacenamiento de copia de seguridad se basa en la cantidad de datos almacenados en Azure Backup, que es la suma de los datos reales de cada punto de recuperación.

Por ejemplo, veamos una máquina virtual de tamaño estándar A2 con dos discos de datos adicionales con un tamaño máximo de 4 TB cada uno. La tabla siguiente proporciona los datos almacenados en cada uno de estos discos:

**Disco** | **Tamaño máximo** | **Datos reales presentes**
--- | --- | ---
Disco del sistema operativo | 4095 GB | 17 GB 
Disco local/temporal | 135 GB | 5 GB (no incluidos en la copia de seguridad) 
Disco de datos 1 | 4095 GB | 30 GB 
Disco de datos 2 | 4095 GB | 0 GB 

- El tamaño real de la máquina virtual en este caso es de 17 GB + 30 GB + 0 GB = 47 GB.
- Este tamaño de instancias protegidas (47 GB) se convierte en la base de la factura mensual.
- A medida que crece la cantidad de datos en la máquina virtual, el tamaño de instancia protegida usado para la facturación también cambia en consecuencia.


## <a name="next-steps"></a>Pasos siguientes

Ahora, [prepárese](backup-azure-arm-vms-prepare.md) para la copia de seguridad de máquinas virtuales de Azure.
