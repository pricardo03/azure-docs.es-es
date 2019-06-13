---
title: ¿Qué es Azure Backup?
description: Proporciona información general del servicio Azure Backup, y cómo contribuye a la estrategia de continuidad empresarial y recuperación ante desastres (BCDR).
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 04/24/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e926ca2625f98522652ae7e7d245ecf2ed576c4
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688722"
---
# <a name="what-is-azure-backup"></a>¿Qué es Azure Backup?

El servicio Azure Backup realiza copias de seguridad de los datos en la nube de Microsoft Azure. Puede hacer copias de seguridad de las cargas de trabajo locales y máquinas locales, así como de las máquinas virtuales de Azure.


## <a name="why-use-azure-backup"></a>¿Por qué usar Azure Backup?

Azure Backup proporciona las siguientes ventajas principales:

- **Descarga de copia de seguridad local**: Azure Backup ofrece una solución sencilla para hacer copias de seguridad de los recursos locales en la nube. Obtenga copias de seguridad a corto y largo plazo sin necesidad de implementar complejas soluciones de copias de seguridad locales.
- **Copia de seguridad de máquinas virtuales de IaaS de Azure**: Azure Backup proporciona copias de seguridad independientes y aisladas para evitar la destrucción accidental de datos originales. Las copias de seguridad se almacenan en un almacén de Recovery Services con puntos de recuperación administrados e integrados. La configuración y la escalabilidad son sencillas, las copias de seguridad están optimizadas y puede restaurarlas fácilmente cuando sea necesario.
- **Escala sencilla**: Azure Backup usa la eficacia subyacente y la escala ilimitada de la nube de Azure para proporcionar alta disponibilidad, sin sobrecarga de mantenimiento o supervisión.
- **Obtención de una transferencia de datos ilimitada**: Azure Backup no limita la cantidad de datos de entrada y salida que se transfieren ni cobra por ellos.
    - Datos de salida hacen referencia a los datos transferidos desde un almacén de Recovery Services durante una operación de restauración.
    - Si realiza una copia de seguridad inicial sin conexión mediante el servicio Azure Import/Export para importar grandes cantidades de datos, hay un costo asociado con los datos de entrada.  [Más información](backup-azure-backup-import-export.md).
- **Protección de los datos**: Azure Backup proporciona soluciones para proteger los datos en tránsito y en reposo.
- **Obtención de copias de seguridad coherentes de la aplicación**: una copia de seguridad coherente con la aplicación significa un punto de recuperación que tiene todos los datos necesarios para restaurar la copia de seguridad. Azure Backup proporciona copias de seguridad coherentes con la aplicación, lo que garantiza que no se necesitarán correcciones adicionales para restaurar los datos. La restauración de datos coherentes con la aplicación reduce el tiempo de restauración, lo que permite volver rápidamente a un estado de ejecución.
- **Retención de los datos a corto y largo plazo**: puede usar los almacenes de Recovery Services para la retención de datos tanto a corto como a largo plazo. Azure no limita el tiempo que los datos pueden permanecer en un almacén de Recovery Services. Puede conservarlos el tiempo que desee. Azure Backup tiene un límite de 9999 puntos de recuperación por instancia protegida. 
- **Administración de almacenamiento automática**: los entornos híbridos requieren con frecuencia almacenamiento heterogéneo, unos local y otros en la nube. Con Azure Backup, no hay ningún costo por el uso de dispositivos de almacenamiento local. Azure Backup asigna y administra automáticamente almacenamiento de copia de seguridad y usa un modelo de pago por uso para que solo pague por el almacenamiento que consume. [Más información](https://azure.microsoft.com/pricing/details/backup) sobre precios.
- **Varias opciones de almacenamiento**: Azure Backup ofrece dos tipos de replicación para mantener la alta disponibilidad de los datos o del almacenamiento.
    - El [almacenamiento con redundancia local (LRS)](../storage/common/storage-redundancy-lrs.md) replica los datos tres veces (crea tres copias de los datos) en una unidad de escalado de almacenamiento de un centro de datos. Todas las copias de los datos se encuentran en la misma región. LRS es una opción de bajo costo para proteger los datos contra errores de hardware local.
    - El [almacenamiento con redundancia geográfica (GRS)](../storage/common/storage-redundancy-grs.md) es la opción de replicación predeterminada y recomendada. GRS replica los datos en una región secundaria (a cientos de kilómetros de la ubicación principal de los datos de origen). GRS cuesta más que LRS, pero proporciona un mayor nivel de durabilidad de los datos, incluso si hay una interrupción regional.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>¿Cuál es la diferencia entre Azure Backup y Azure Site Recovery?

Tanto el servicio Azure Backup como el servicio Azure Site Recovery contribuyen a su estrategia de continuidad empresarial y recuperación ante desastres (BCDR) de su empresa. BCDR consta de dos objetivos generales:

- Mantener los datos empresariales seguros y recuperables cuando se produzcan interrupciones.
- Mantener las aplicaciones y cargas de trabajo en funcionamiento durante los tiempos de inactividad planeados y no planeados.

Ambos servicios ofrecen una funcionalidad complementaria pero diferente.

- **Azure Site Recovery**: Site Recovery proporciona una solución de recuperación ante desastres para máquinas locales y para máquinas virtuales de Azure. Se replican las máquinas de una ubicación principal a una secundaria. Cuando ocurre un desastre, las máquinas conmutan por error en la ubicación secundaria y se accede a ellas desde allí. Cuando todo vuelve a funcionar normalmente, las máquinas conmutan por recuperación en el sitio principal.
- **Azure Backup**: El servicio Azure Backup realiza copias de seguridad de los datos de las máquinas locales y de las máquinas virtuales de Azure. Se puede hacer una copia de seguridad de datos que luego se recuperan a nivel pormenorizado, con copias de seguridad de archivos, carpetas, estado del sistema de la máquina y copias de seguridad de datos con reconocimiento de aplicaciones. Azure Backup trata los datos a un nivel más pormenorizado que Site Recovery. Como ejemplo, si una presentación en el equipo portátil resulta dañada, podría usar Azure Backup para restaurarla. Si desea mantener una configuración de máquina virtual y datos seguros y accesibles, puede utilizar Site Recovery.  

Use los puntos de la tabla para ayudar a determinar las necesidades de BCDR.

**Objetivo** | **Detalles** | **Comparación**
--- | --- | ---
**Copia de seguridad y retención de datos** | Los datos de las copias de seguridad pueden conservarse y almacenarse durante días, meses o incluso años si es necesario desde el punto de vista del cumplimiento. | Las soluciones de copia de seguridad como Azure Backup le permiten recoger con precisión los datos de los que desea hacer copias de seguridad y ajustar con precisión las directivas de copia de seguridad y retención.<br/><br/> Site Recovery no permite el mismo ajuste preciso.
**Objetivo de punto de recuperación (RPO)** | La cantidad aceptable de pérdida de datos si debe realizarse una recuperación. | Las copias de seguridad tienen un RPO más variable.<br/><br/> Las copias de seguridad de máquina virtual normalmente tienen un RPO de un día, mientras que las copias de seguridad de base de datos tienen RPO bajos, de hasta 15 minutos.<br/><br/> Site Recovery proporciona un RPO bajo ya que la replicación es continua o frecuente, por lo que la diferencia entre la copia del origen y de la réplica es pequeña.
**Objetivo de tiempo de recuperación (RTO)** |La cantidad de tiempo que se tarda en completar una recuperación o restauración. | Debido a un RPO mayor, la cantidad de datos que una solución de copia de seguridad debe procesar es normalmente mucho mayor, lo que da lugar a RTO más largos. Por ejemplo, la restauración de datos de cintas puede tardar días, dependiendo del tiempo necesario para transportar la cinta desde una ubicación externa.

## <a name="what-backup-scenarios-are-supported"></a>¿Qué escenarios de copia de seguridad se admiten?

Azure Backup puede hacer copias de seguridad tanto de las máquinas locales como de las máquinas virtuales de Azure.

**Máquina** | **Escenario de copia de seguridad**
--- | ---
**Copia de seguridad local** |  1) Ejecute el agente de Microsoft Azure Recovery Services (MARS) de Azure Backup en máquinas Windows locales para realizar la copia de seguridad de archivos individuales y del estado del sistema. <br/><br/>2) Realice una copia de seguridad de las máquinas locales en un servidor de copia de seguridad (System Center Data Protection Manager [DPM] o Microsoft Azure Backup Server [MABS]) y, luego, configure el servidor de copia de seguridad para la copia de seguridad en un almacén de Azure Backup Recovery Services de Azure.
**Máquinas virtuales de Azure** | 1) Habilite la copia de seguridad de máquinas virtuales de Azure individuales. Al habilitar la copia de seguridad, Azure Backup instala una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina virtual. El agente realiza una copia de seguridad de toda la máquina virtual.<br/><br/> (2) Ejecute al agente de MARS en una máquina virtual de Azure. Esto es útil si quiere realizar la copia de seguridad de archivos y carpetas individuales en la máquina virtual.<br/><br/> (3) Realice una copia de una máquina virtual de Azure en un servidor DPM o MABS que se ejecuta en Azure. A continuación, realice la copia de seguridad del servidor DPM o MABS en un almacén mediante Azure Backup.


## <a name="why-use-a-backup-server"></a>¿Por qué usar un servidor de copia de seguridad?
Las ventajas de realizar una copia de seguridad de las máquinas y aplicaciones en almacenamiento MABS o DPM y, luego, hacer lo mismo con el almacenamiento DPM o MABS en un almacén son las siguientes:

- La copia de seguridad en DPM o MABS proporciona copias de seguridad compatibles con aplicaciones optimizadas para aplicaciones comunes como SQL Server, Exchange y SharePoint, además de copias de seguridad de archivos, carpetas o volúmenes, y copias de seguridad del estado de la máquina (estado del sistema, sin sistema operativo).
- Con máquinas locales, no es necesario instalar al agente de MARS en cada máquina de la que quiera hacer una copia de seguridad. Cada máquina ejecuta el agente de protección DPM o MABS, y el agente MARS se ejecuta solo en MABS o DPM.
- Tiene más flexibilidad y opciones de programación pormenorizado para ejecutar copias de seguridad.
- Puede administrar las copias de seguridad de varios equipos que reunirá en grupos de protección en una única consola. Esto es particularmente útil cuando las aplicaciones están organizadas en niveles sobre varias máquinas y se quiere hacer una copia de seguridad de las mismas.

Más información sobre [cómo funciona la copia de seguridad](backup-architecture.md#architecture-back-up-to-dpmmabs) cuando se usa un servidor de copia de seguridad y los [requisitos de soporte técnico](backup-support-matrix-mabs-dpm.md) para servidores de copia de seguridad.

## <a name="what-can-i-back-up"></a>¿De qué puedo hacer copia de seguridad?

**Máquina** | **Método de copia de seguridad** | **Copias de seguridad**
--- | --- | ---
**Máquinas virtuales Windows locales** | Ejecutar el agente de MARS | Copia de seguridad de archivos, carpetas, estado del sistema.<br/><br/> Máquinas Linux no admitidas.
**Máquinas locales** | copia de seguridad en DPM o MABS | Haga una copia de seguridad de todo lo que esté protegido por [DPM](backup-support-matrix-mabs-dpm.md#supported-backups-to-dpm) o [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs), como archivos, carpetas, recursos compartidos, volúmenes y datos específicos de la aplicación.
**Máquinas virtuales de Azure** | Ejecutar la extensión de copia de seguridad del agente de máquina virtual de Azure | Realizar la copia de seguridad entera de la máquina virtual
**Máquinas virtuales de Azure** | Ejecutar el agente de MARS | Copia de seguridad de archivos, carpetas, estado del sistema.<br/><br/> Máquinas Linux no admitidas.
**Máquinas virtuales de Azure** | Realizar la copia de seguridad en MABS o DPM que se ejecutan en Azure | Haga una copia de seguridad de todo lo que esté protegido por [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs) o [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807), como archivos, carpetas, recursos compartidos, volúmenes y datos específicos de la aplicación.

## <a name="what-backup-agents-do-i-need"></a>¿Qué agentes de copia de seguridad necesario?

**Escenario** | **Agent**
--- | ---
**Copia de seguridad de máquinas virtuales de Azure** | No se necesita ningún agente. La extensión de máquina virtual de Azure para copia de seguridad se instala en la máquina virtual de Azure al ejecutar la primera copia de seguridad de máquina virtual de Azure.<br/><br/> Compatibilidad con Windows y Linux.
**Copia de seguridad de máquinas virtuales Windows locales** | Descargue, instale y ejecute el agente de MARS directamente en la máquina.
**Copia de seguridad de máquinas virtuales de Azure con el agente de MARS** | Descargue, instale y ejecute el agente de MARS directamente en la máquina. El agente de MARS se puede ejecutar junto con la extensión de copia de seguridad.
**Copia de seguridad de máquinas locales y máquinas virtuales de Azure en DPM o MABS** | El agente de protección de DPM o MABS se ejecuta en las máquinas que quiera proteger. El agente de MARS se ejecuta en el servidor DPM o MABS para realizar una copia de seguridad en Azure.

## <a name="which-backup-agent-should-i-use"></a>¿Qué agente de copia de seguridad se debe usar?

**Copia de seguridad** | **Solución** | **Limitación**
--- | --- | ---
**Quiero hacer una copia de seguridad de una máquina virtual de Azure completa** | Habilite la copia de seguridad para la máquina virtual. La extensión de copia de seguridad se configurará automáticamente en la máquina virtual de Azure para Linux o Windows. | Se copia la máquina virtual entera <br/><br/> En el caso de las máquinas virtuales Windows, la copia de seguridad es coherente con la aplicación. Para las máquinas virtuales Linux, no es coherente con el archivo. Si necesita compatibilidad con aplicaciones en máquinas virtuales Linux, debe configurar estos scripts personalizados.
**Quiero hacer una copia de seguridad de archivos o carpetas específicos en máquinas virtuales de Azure** | Implemente al agente de MARS en la máquina virtual.
**Quiero hacer una copia de seguridad directamente en máquinas Windows locales** | Instale el agente MARS en la máquina. | Puede hacer copias de seguridad de archivos, carpetas y del estado del sistema en Azure. Las copias de seguridad no reconocen las aplicaciones.
**Quiero hacer una copia de seguridad directamente en máquinas Linux locales** | Necesita implementar DPM o MABS para hacer una copia de seguridad en Azure. | No se admite la copia de seguridad del host Linux, solo puede realizar la copia de seguridad de la máquina invitada Linux hospedada en Hyper-V o VMWare.
**Quiero hacer una copia de seguridad de las aplicaciones que se ejecutan en el entorno local** | En el caso de copias de seguridad compatibles con aplicaciones, las máquinas deben estar protegidas con DPM o MABS.
**Quiero una configuración de copia de seguridad y recuperación pormenorizada y flexible para máquinas virtuales de Azure** | Proteja las máquinas virtuales de Azure con MABS o DPM que se ejecutan en Azure para mayor flexibilidad en la programación de copias de seguridad, y total flexibilidad para proteger y restaurar archivos, carpetas, volúmenes, aplicaciones y estado del sistema.

## <a name="backup-and-retention"></a>Copia de seguridad y retención

Azure Backup tiene un límite de 9999 puntos de recuperación, también conocidos como copias de seguridad o instantáneas, por cada *instancia protegida*.

- Una instancia protegida es un equipo, un servidor (físico o virtual) o una carga de trabajo configurada para realizar copias de seguridad en Azure. Una instancia está protegida una vez que se ha guardado una copia de seguridad de los datos.
- La copia de seguridad de los datos es la protección. Si los datos de origen se pierden o dañan, la copia de seguridad puede restaurar los datos de origen.

En la tabla siguiente se muestra la frecuencia de copia de seguridad máxima para cada componente. La configuración de la directiva de copia de seguridad determina la rapidez con la que se consumen los puntos de recuperación. Por ejemplo, si crea un punto de recuperación cada día, puede conservarlos durante 27 años antes de que se agoten. Si crea un punto de recuperación cada mes, puede conservarlos durante 833 años antes de que se agoten. El servicio Backup no establece un límite de tiempo de expiración para un punto de recuperación.

|  | Agente de Azure Backup | System Center DPM | Azure Backup Server | Copia de seguridad de máquina virtual de IaaS de Azure |
| --- | --- | --- | --- | --- |
| Frecuencia de copia de seguridad<br/> (en el almacén de Recovery Services) |Tres copias de seguridad por día |Dos copias de seguridad por día |Dos copias de seguridad por día |Una copia de seguridad por día |
| Frecuencia de copia de seguridad<br/> (en el disco) |No aplicable |Cada 15 minutos para SQL Server<br/><br/> Cada hora para otras cargas de trabajo |Cada 15 minutos para SQL Server<br/><br/> Cada hora para otras cargas de trabajo |No aplicable |
| Opciones de retención |Diariamente, semanalmente, mensualmente y anualmente |Diariamente, semanalmente, mensualmente y anualmente |Diariamente, semanalmente, mensualmente y anualmente |Diariamente, semanalmente, mensualmente y anualmente |
| Número máximo de puntos de recuperación por instancia protegida |9.999|9.999|9.999|9.999|
| Período de retención máximo |Depende de la frecuencia de la copia de seguridad |Depende de la frecuencia de la copia de seguridad |Depende de la frecuencia de la copia de seguridad |Depende de la frecuencia de la copia de seguridad |
| Puntos de recuperación en disco local |No aplicable | 64 en el caso de servidores de archivos<br/><br/> 448 en el caso de servidores de aplicaciones | 64 en el caso de servidores de archivos<br/><br/> 448 en el caso de servidores de aplicaciones |No aplicable |
| Puntos de recuperación en cinta |No aplicable |Ilimitado |No aplicable |No aplicable |

## <a name="how-does-azure-backup-work-with-encryption"></a>¿Cómo funciona Azure Backup con el cifrado?

**Cifrado** | **Copia de seguridad local** | **Copia de seguridad de máquinas virtuales de Azure** | **Copia de seguridad de SQL en máquinas virtuales de Azure**
--- | --- | --- | ---
Cifrado en reposo<br/> (Cifrado de datos donde persisten o están almacenados) | La frase de contraseña especificada por el cliente se usa para cifrar los datos | Azure [Storage Service Encryption (SSE)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) se usa para cifrar los datos que se encuentran en el almacén.<br/><br/> Backup cifra automáticamente los datos antes de almacenarlos. Azure Storage descifra los datos antes de recuperarlos. No se admite actualmente el uso de claves administradas por el cliente para SSE.<br/><br/> Puede realizar copias de seguridad de máquinas virtuales que usan [Azure Disk Encryption (ADE)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) para cifrar discos de datos y del sistema operativo. Azure Backup admite máquinas virtuales cifradas con BEK únicamente y con BEK y [KEK](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/). Revise las [limitaciones](backup-azure-vms-encryption.md#encryption-support). | Azure Backup admite la copia de seguridad de bases de datos o servidores SQL Server con [TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) habilitado. Backup admite TDE con claves administradas por Azure o con claves administradas por el cliente (BYOK).<br/><br/> Backup no realiza ningún cifrado de SQL como parte del proceso de copia de seguridad.
Cifrado en tránsito<br/> (Cifrado de los datos que se transfieren desde una ubicación a otra) | Los datos se cifran con AES256 y se envían al almacén de Azure a través de HTTPS | Dentro de Azure, los datos entre Azure Storage y el almacén se protegen mediante HTTPS. Estos datos permanecen en la red troncal de Azure.<br/><br/> Para la recuperación de archivos, iSCSI protege los datos transmitidos entre el almacén y la máquina virtual de Azure. La tunelización segura protege el canal iSCSI. | Dentro de Azure, los datos entre Azure Storage y el almacén se protegen mediante HTTPS.<br/><br/> La recuperación de archivos no es pertinente para SQL.

## <a name="next-steps"></a>Pasos siguientes

- [Revise](backup-architecture.md) la arquitectura y los componentes de los diferentes escenarios de copia de seguridad.
- [Compruebe](backup-support-matrix.md) los requisitos de soporte técnico y las limitaciones de copia de seguridad en general y de la [copia de seguridad de máquinas virtuales de Azure](backup-support-matrix-iaas.md) en particular.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
