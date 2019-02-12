---
title: ¿Qué es Azure Backup?
description: Proporciona información general del servicio Azure Backup, y cómo implementarlo como parte de la estrategia de continuidad empresarial y recuperación ante desastres (BCDR).
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 298c9fabca9d1994e0b952fdf8b48b70370c3ec2
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490677"
---
# <a name="what-is-azure-backup"></a>¿Qué es Azure Backup?

El servicio Azure Backup realiza copias de seguridad de los datos en la nube de Microsoft Azure. Puede hacer copias de seguridad de las cargas de trabajo locales y máquinas locales, así como de las máquinas virtuales de Azure.


## <a name="why-use-azure-backup"></a>¿Por qué usar Azure Backup?

Azure Backup proporciona las siguientes ventajas principales:

- **Descarga de copia de seguridad local**: Azure Backup ofrece una solución sencilla para hacer copias de seguridad de los recursos locales en la nube. Obtenga copias de seguridad a corto y largo plazo sin necesidad de implementar complejas soluciones de copias de seguridad locales. Elimine la necesidad de usar cintas o un sistema de copias de seguridad externo.
- **Copia de seguridad de máquinas virtuales de IaaS de Azure**: Azure Backup proporciona copias de seguridad independientes y aisladas para evitar la destrucción accidental de datos originales. Las copias de seguridad se almacenan en un almacén de Recovery Services con puntos de recuperación administrados e integrados. La configuración y escalabilidad es sencilla, las copias de seguridad están optimizadas y puede restaurarlas fácilmente cuando sea necesario.
- **Escala sencilla**: Azure Backup usa la eficacia subyacente y la escala ilimitada de la nube de Azure para proporcionar alta disponibilidad, sin sobrecarga de mantenimiento o supervisión. Puede configurar alertas para proporcionar información sobre los eventos, pero no tiene que preocuparse por la alta disponibilidad de los datos en la nube.
- **Obtención de una transferencia de datos ilimitados**: Azure Backup no limita la cantidad de datos de entrada y salida que se transfieren. Tampoco cobra por los datos que se transfieren. Sin embargo, si usa el servicio Azure Import/Export para importar grandes cantidades de datos, existe un costo asociado con los datos de entrada. Para más información sobre este costo, consulte [Flujo de trabajo de copia de seguridad sin conexión en Azure Backup](backup-azure-backup-import-export.md). Datos de salida hacen referencia a los datos transferidos desde un almacén de Recovery Services durante una operación de restauración.
- **Protección de los datos**: el cifrado de datos permite la transmisión y el almacenamiento seguros de los datos del cliente en la nube pública. La frase de contraseña de cifrado se almacena localmente y nunca se transmite ni se almacena en Azure. Si es necesario restaurar algunos de los datos, solo cuenta con la frase de contraseña o clave de cifrado.
- **Obtención de copias de seguridad coherentes de la aplicación**: una copia de seguridad coherente con la aplicación significa un punto de recuperación que tiene todos los datos necesarios para restaurar la copia de seguridad. Azure Backup proporciona copias de seguridad coherentes con la aplicación, lo que garantiza que no se necesitarán correcciones adicionales para restaurar los datos. La restauración de datos coherentes con la aplicación reduce el tiempo de restauración, lo que permite volver rápidamente a un estado de ejecución.
- **Obtención de una retención a corto y a largo plazo**: **Retención a largo plazo**: los almacenes de Recovery Services se pueden usar para la retención de datos tanto a corto como a largo plazo. Azure no limita el tiempo que los datos pueden permanecer en un almacén de Recovery Services. Los datos se pueden conservar en un almacén tanto tiempo como se desee. Azure Backup tiene un límite de 9999 puntos de recuperación por instancia protegida. Consulte la sección [Copia de seguridad y retención](backup-introduction-to-azure-backup.md#backup-and-retention) de este artículo para obtener una explicación de la forma en que este límite puede afectar a sus necesidades de copia de seguridad.
- **Administración de almacenamiento automática**: los entornos híbridos requieren con frecuencia almacenamiento heterogéneo, unos local y otros en la nube. Con Azure Backup, no hay ningún costo por el uso de dispositivos de almacenamiento local. Azure Backup asigna y administra automáticamente almacenamiento de copia de seguridad y emplea un modelo de pago por uso. Esto significa que solo paga por el almacenamiento que consume. Para más información, consulte el [artículo sobre precios de Azure](https://azure.microsoft.com/pricing/details/backup).
- **Varias opciones de almacenamiento**: un aspecto de alta disponibilidad es la replicación del almacenamiento. Azure Backup ofrece dos tipos de replicación: [almacenamiento con redundancia local](../storage/common/storage-redundancy-lrs.md) y [almacenamiento con redundancia geográfica](../storage/common/storage-redundancy-grs.md). Elija la opción de almacenamiento de copia de seguridad más acorde con sus necesidades:
    - El almacenamiento con redundancia local (LRS) replica los datos tres veces (crea tres copias de los datos) en una unidad de escalado de almacenamiento de un centro de datos. Todas las copias de los datos se encuentran en la misma región. LRS es una opción de bajo costo para proteger los datos contra errores de hardware local.
    - El almacenamiento con redundancia geográfica (GRS) es el valor predeterminado y la opción de replicación recomienda. GRS replica los datos en una región secundaria (a cientos de kilómetros de la ubicación principal de los datos de origen). GRS cuesta más que LRS, pero proporciona un mayor nivel de durabilidad de los datos, incluso si hay una interrupción regional.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>¿Cuál es la diferencia entre Azure Backup y Azure Site Recovery?

Tanto el servicio Azure Backup como el servicio Azure Site Recovery contribuyen a su estrategia de continuidad empresarial y recuperación ante desastres (BCDR) de su empresa. BCDR consta de dos objetivos generales:

- Mantener los datos empresariales seguros y recuperables cuando se produzcan interrupciones.
- Mantener las aplicaciones y cargas de trabajo en funcionamiento durante los tiempos de inactividad planeados y no planeados.

Ambos servicios ofrecen una funcionalidad complementaria pero diferente.

- **Azure Site Recovery**: Site Recovery proporciona una solución de recuperación ante desastres para máquinas locales y para máquinas virtuales de Azure. Se replican las máquinas de una ubicación principal a una secundaria. Cuando ocurre un desastre, las máquinas conmutan por error en la ubicación secundaria y se accede a ellas desde allí. Cuando todo vuelve a funcionar normalmente, las máquinas conmutan por recuperación en el sitio principal.
- **Azure Backup**: El servicio Azure Backup realiza copias de seguridad de los datos de las máquinas locales y de las máquinas virtuales de Azure. Se puede hacer una copia de seguridad de datos que luego se recuperan a nivel pormenorizado, con copias de seguridad de archivos, carpetas, estado del sistema de la máquina y copias de seguridad de datos con reconocimiento de aplicaciones. Azure Backup trata los datos a un nivel más pormenorizado que Site Recovery. Como ejemplo, si una presentación en el equipo portátil resulta dañada, podría usar Azure Backup para restaurarla. Si desea mantener una configuración de máquina virtual y datos seguros y accesibles, puede utilizar Site Recovery.  

Use los puntos de la tabla para ayudar a determinar las necesidades de BCDR. 

**Objetivo** | **Detalles** | **Comparación**
--- | --- | --- | --- |
**Copia de seguridad y retención de datos** | Los datos de las copias de seguridad pueden conservarse y almacenarse durante días, meses o incluso años si es necesario desde el punto de vista del cumplimiento. | Las soluciones de copia de seguridad como Azure Backup le permiten recoger con precisión los datos de los que desea hacer copias de seguridad y ajustar con precisión las directivas de copia de seguridad y retención.<br/><br/> Site Recovery no permite el mismo ajuste preciso.
**Objetivo de punto de recuperación (RPO)** | La cantidad aceptable de pérdida de datos si debe realizarse una recuperación. | Las copias de seguridad tienen un RPO más variable.<br/><br/> Las copias de seguridad de máquina virtual normalmente tienen un RPO de un día, mientras que las copias de seguridad de base de datos tienen RPO bajos, de hasta 15 minutos.<br/><br/> Site Recovery proporciona un RPO bajo ya que la replicación es continua o frecuente, por lo que la diferencia entre la copia del origen y de la réplica es pequeña.
**Objetivo de tiempo de recuperación (RTO)** |La cantidad de tiempo que se tarda en completar una recuperación o restauración. | Debido a un RPO mayor, la cantidad de datos que una solución de copia de seguridad debe procesar es normalmente mucho mayor, lo que da lugar a RTO más largos. Por ejemplo, la restauración de datos de cintas puede tardar días, dependiendo del tiempo necesario para transportar la cinta desde una ubicación externa. | Las soluciones de recuperación ante desastres como Site Recovery tienen un RPO bajo, ya que la replicación continua o frecuente significa por lo general que el destino está más sincronizado con el origen. |

## <a name="what-backup-scenarios-are-supported"></a>¿Qué escenarios de copia de seguridad se admiten?

Azure Backup puede hacer copias de seguridad tanto de las máquinas locales como de las máquinas virtuales de Azure.

**Máquina** | **Escenario de copia de seguridad**
--- | ---
**Máquinas locales (físicas o virtuales)** |  Puede hacer copias de seguridad de máquinas locales individuales.<br/><br/>Puede hacer copias de seguridad de máquinas locales que están protegida por System Center Data Protection Manager (DPM)<br/><br/> Puede hacer copias de seguridad de máquinas locales que están protegidas por Microsoft Azure Backup Server (MABS).
**Máquinas virtuales de Azure** | Puede hacer copias de seguridad de máquinas virtuales individuales de Azure.<br/><br/> Puede hacer copias de seguridad de máquinas virtuales de Azure protegidas por DPM o MABS.

### <a name="back-up-servers"></a>Copia de seguridad de servidores

Es posible que desee hacer una copia de seguridad de los servidores y cargas de trabajo locales, o de las máquinas virtuales de Azure y sus cargas de trabajo, primero en un servidor de copia de seguridad y después en un almacén de Recovery Services. 

**Servidor de copia de seguridad** | **Detalles**
--- | ---
**System Center Data Protection Manager (DPM)** | Puede utilizar Azure Backup para hacer copias de seguridad de los datos que protege DPM:<br/><br/> - Se puede ejecutar DPM en un entorno local (físico o virtual) o en Azure.<br/><br/> - Puede proteger diferentes tipos de datos que se ejecutan en máquinas locales y en máquinas virtuales de Azure mediante la realización de copias de seguridad de los datos en el servidor DPM.<br/><br/> A su vez, se puede hacer una copia de seguridad del servidor DPM en un almacén de Recovery Services mediante el servicio Azure Backup.<br/><br/> El servidor DPM y las máquinas que protege deben estar en la misma red. Las máquinas locales solo se pueden proteger con un servidor DPM local. Del mismo modo, las máquinas virtuales de Azure solo se pueden proteger por DPM que se ejecute en Azure.
**Microsoft Azure Backup Server (MABS)** | Puede utilizar Azure Backup para hacer copias de seguridad de los datos con protección de MABS:<br/><br/> -MABS se puede ejecutar de forma local (física o virtual) o en Azure.<br/><br/> - Puede proteger los diferentes tipos de datos que se ejecutan en máquinas locales y en máquinas virtuales de Azure mediante la realización de copias de seguridad de los datos en MABS.<br/><br/> - A su vez, se puede hacer una copia de seguridad de MABS en un almacén de Recovery Services mediante el servicio Azure Backup.<br/><br/> - MABS proporciona una funcionalidad similar a la de DPM, excepto que no es posible hacer copias de seguridad en cinta mediante MABS. MABS no requiere una licencia de System Center.<br/><br/> Al igual que DPM, las máquinas locales solo se pueden proteger con una instancia de MABS local. Las máquinas virtuales de Azure solo se pueden proteger con una instancia de MABS en Azure.

Las ventajas de la realización de copias de seguridad primero en DPM o MABS y después en un almacén son las siguientes:

- La copia de seguridad en DPM/MAB proporciona copias de seguridad con reconocimiento de aplicaciones optimizadas para aplicaciones comunes como SQL Server, Exchange y SharePoint, además de copias de seguridad de archivos, carpetas o volúmenes, y copias de seguridad del estado de la máquina (estado del sistema, sin sistema operativo).
- No es necesario que instale el agente de Azure Backup en cada máquina de la que desee hacer una copia de seguridad. Las máquinas ejecutan el agente de protección de DPM o MABS, y el agente de Microsoft Azure Recovery Services de Azure Backup solo se ejecuta en el servidor DPM o MABS.
- Tiene más flexibilidad y opciones de programación pormenorizado para ejecutar copias de seguridad.
- Puede administrar las copias de seguridad de varios equipos que reunirá en grupos de protección en una única consola. Esto es particularmente útil cuando las aplicaciones están organizadas en niveles sobre varias máquinas y se quiere hacer una copia de seguridad de las mismas.

## <a name="what-can-be-backed-up"></a>¿De qué se puede hacer una copia de seguridad?

**Máquina** | **Servidor de copia de seguridad** | **Copias de seguridad**
--- | --- | ---
Máquinas virtuales Windows locales | Sin copia de seguridad realizada en DPM o MABS | Copia de seguridad de archivos, carpetas, estado del sistema.
Máquinas virtuales de Azure (Windows y Linux) | Sin copia de seguridad realizada en DPM o MABS | Copia de seguridad de archivos, carpetas, estado del sistema.<br/><br/> Las copias de seguridad reconocen las aplicaciones en las máquinas Windows y los archivos en las máquinas Linux.
Máquinas virtuales de Azure o máquinas virtuales locales | Con protección de DPM | Haga una copia de seguridad de todo lo que esté protegido por DPM, incluidos archivos, carpetas, recursos compartidos, volúmenes y datos específicos de la aplicación. [Obtenga información](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807) sobre los elementos de los que DPM puede realizar una copia de seguridad.
Máquinas virtuales de Azure o máquinas virtuales locales | Con protección de MABS | Haga una copia de seguridad de todo lo que esté protegido por MABS, incluidos archivos, carpetas, recursos compartidos, volúmenes y datos específicos de la aplicación. [Obtenga información](backup-mabs-protection-matrix.md) sobre los elementos de los que MABS puede hacer una copia de seguridad.

## <a name="what-backup-agents-do-i-need"></a>¿Qué agentes de copia de seguridad necesario?
**Escenario** | **Agent** | **Detalles**
--- | --- | ---
Máquinas locales (sin servidor de copia de seguridad) | El agente de Microsoft Azure Recovery Services (MARS) se ejecuta en la máquina Windows. | Puede descargar e instalar al agente MARS durante la implementación de Azure Backup.<br/><br/> Solo se admite para máquinas Windows.
Máquinas virtuales de Azure (sin servidor de copia de seguridad) | No se requiere ningún agente explícito. La extensión de la máquina virtual de Azure para copias de seguridad se ejecuta en la máquina virtual de Azure. | La extensión se instala al ejecutar la primera copia de seguridad de la máquina virtual de Azure.<br/><br/> Compatibilidad con Windows y Linux.
Máquinas locales y máquinas virtuales de Azure con protección de DPM. | El agente MARS se ejecuta en el servidor DPM. | No necesita el agente MARS en máquinas individuales.<br/><br/> Cuando se implementa DPM, se instala el agente de protección DPM en cada máquina que se protege. 
Copia de seguridad de máquinas locales y de máquinas virtuales de Azure con protección de MABS | El agente MARS se ejecuta en MABS. | No necesita el agente MARS en máquinas individuales.<br/><br/> Cuando se implementa MABS, se instala el agente de protección MABS en cada máquina que se protege. 


## <a name="which-backup-agent-should-i-use"></a>¿Qué agente de copia de seguridad se debe usar?

**Copia de seguridad** | **Solución** | **Limitación**
--- | --- | ---
Quiero hacer una copia de seguridad de las máquinas Windows locales. Las máquinas no están protegidas por DPM o MABS | Instale el agente MARS en la máquina. | Puede hacer copias de seguridad de archivos, carpetas y del estado del sistema en Azure. Las copias de seguridad no reconocen las aplicaciones.
Quiero hacer una copia de seguridad de las máquinas Linux locales. Las máquinas no están protegidas por DPM o MABS. | Necesita implementar DPM o MABS para hacer una copia de seguridad en Azure.
Quiero hacer una copia de seguridad de las aplicaciones que se ejecutan en máquinas Windows locales | Para las copias de seguridad con reconocimiento de aplicaciones, las máquinas Windows deben protegerse con DPM o MABS.
Quiero hacer una copia de seguridad de las máquinas virtuales de Azure | Ejecute una copias de seguridad con Azure Backup. La extensión de copia de seguridad se configurará automáticamente en la máquina virtual de Azure para Linux o Windows. | Se hace una copia de seguridad de los discos de la máquina virtual.<br/><br/> En el caso de las máquinas virtuales Windows, la copia de seguridad es coherente con la aplicación. Para las máquinas virtuales Linux, no es coherente con el archivo. Si necesita que tenga reconocimiento de aplicaciones, tendrá que configurarlo con scripts personalizados.
Quiero hacer una copia de seguridad de máquinas virtuales de Azure con flexibilidad pormenorizada de la configuración de copia de seguridad y recuperación | Proteja las máquinas virtuales de Azure con DPM o MABS que se ejecutan en Azure para mayor flexibilidad en la programación de copias de seguridad, y total flexibilidad para proteger y restaurar archivos, carpetas, volúmenes, aplicaciones y estado del sistema.


## <a name="next-steps"></a>Pasos siguientes

- [Revise](backup-architecture.md) la arquitectura y los componentes de los diferentes escenarios de copia de seguridad.
- [Compruebe](backup-support-matrix.md) las características y configuración admitidas para la copia de seguridad.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

