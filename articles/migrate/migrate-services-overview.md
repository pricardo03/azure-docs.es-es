---
title: Acerca de Azure Migrate
description: Más información sobre el servicio de Azure Migrate
ms.topic: overview
ms.date: 12/29/2019
ms.custom: mvc
ms.openlocfilehash: c336b2f95a17896edcc7eaad2b2586c6859804f9
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772200"
---
# <a name="about-azure-migrate"></a>Acerca de Azure Migrate

En este artículo se proporciona una rápida descripción general del servicio Azure Migrate.

Utilice Azure Migrate para migrar a Azure. Azure Migrate proporciona un concentrador principal para evaluar y migrar infraestructuras, aplicaciones y datos locales a Azure, con las siguientes características:

- **Plataforma de migración unificada**: Use un único portal para iniciar, ejecutar y realizar un seguimiento del recorrido de la migración a Azure.
- **Rango de herramientas**: Rango de herramientas para la evaluación y migración El concentrador incluye Azure Migrate: Server Assessment y Azure Migrate: Server Migration. Se integra con otros servicios de Azure y con otras herramientas y ofertas de proveedores de software independientes (ISV).
- **Evaluación y migración** En el concentrador de Azure Migrate, puede evaluar y migrar:
    - **Servidores**: Evalúe y migre servidores locales a máquinas virtuales de Azure.
    - **Bases de datos**: Evalúe y migre las bases de datos locales a Azure SQL Database o a Instancia administrada de Azure SQL Database.
    - **Aplicaciones web** Evalúe y migre aplicaciones web locales a Azure App Service mediante el asistente de Azure App Service.
    - **Escritorios virtuales**: Evalúe y migre la infraestructura de escritorio virtual (VDI) local a Windows Virtual Desktop en Azure.
    - **Data**: Migre grandes cantidades de datos a Azure de manera rápida y rentable mediante productos de Azure Data Box. 


## <a name="integrated-tools"></a>Herramientas integradas

El concentrador de Azure Migrate proporciona las herramientas siguientes:

**Herramienta** | **Evaluar/migrar** | **Detalles**
--- | --- | ---
**Azure Migrate: Server Assessment** | Evaluar servidores | Detecte y evalúe máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V y servidores físicos para averiguar si están preparadas para la migración a Azure.
**Azure Migrate: Server Migration** | Migrar servidores | Migre a Azure máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtualizadas y máquinas virtuales de nube pública. 
**Database Migration Assistant (DMA)** | Evalúe las bases de datos de SQL Server locales para la migración a Azure SQL Database, Instancia administrada de Azure SQL o máquinas virtuales de Azure que ejecutan SQL Server. | DMA proporciona información sobre los posibles problemas de bloqueo de la migración. Identifica características no admitidas, así como nuevas características de las que se puede aprovechar después de la migración, y ayuda a identificar la ruta de acceso correcta para la migración de la base de datos. [Más información](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).
**Database Migration Service (DMS)** | Migre bases de datos locales a máquinas virtuales de Azure que ejecutan SQL, Azure SQL Database e Instancias administradas de Azure SQL. | [Más información](https://docs.microsoft.com/azure/dms/dms-overview) sobre DMS.
**Movere** | Evaluar servidores | [Más información](#movere) acerca de Movere.
**Migration Assistant para aplicaciones web** | Evalúe y migre aplicaciones web locales a Azure. |  Use Azure App Service Migration Assistant para evaluar sitios web locales para la migración a Azure App Service.<br/><br/> Migre aplicaciones web .NET y PHP a Azure con Azure App Service Migration Assistant. [Más información](https://appmigration.microsoft.com/) acerca del asistente.
**Azure Data Box** | Migración de datos sin conexión | Use los productos de Azure Data Box para trasladar sin conexión grandes cantidades de datos a Azure. [Más información](https://docs.microsoft.com/azure/databox/).

## <a name="isv-integration"></a>Integración de ISV

Azure Migrate se integra con varias ofertas de ISV. 

**ISV** | **Característica**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migrar servidores
[Cloudamize](https://www.cloudamize.com/platform) | Evaluar servidores
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Evaluar y migrar servidores
[Device 42](https://docs.device42.com/) | Evaluar servidores
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Evaluar VDI
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Migrar servidores
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Evaluar servidores
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Evaluar servidores y bases de datos


## <a name="azure-migrate-server-assessment-tool"></a>Azure Migrate: Herramienta Server Assessment

Azure Migrate: La herramienta Server Assessment detecta y evalúa máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V y servidores físicos para la migración a Azure. Le ayuda a identificar lo siguiente:

- **Preparación para Azure:** Evalúe si las máquinas locales están listas para la migración a Azure.
- **Ajuste de tamaño de Azure:** Tamaño de las máquinas virtuales de Azure después de la migración.
- **Estimación de costos de Azure:** Costos estimados de la ejecución de servidores locales en Azure.
- **Visualización de dependencias:** Si usa la evaluación del servidor con la visualización de dependencias, puede identificar eficazmente las dependencias entre servidores, así como las maneras óptimas de trasladar los servidores dependientes a Azure.

Server Assessment usa un dispositivo ligero que se implementa en el entorno local y se registra con Server Assessment.

- El dispositivo se ejecuta en un servidor físico o una máquina virtual, y se instala fácilmente mediante una plantilla descargada.
- El dispositivo detecta máquinas locales y envía continuamente metadatos y datos de rendimiento de las máquinas a Azure Migrate.
- La detección del dispositivo es sin agente. No es necesario instalar nada en las máquinas detectadas.
- Después de la detección, recopile las máquinas detectadas en grupos y evalúe los grupos para la migración.


## <a name="azure-migrate-server-migration-tool"></a>Azure Migrate: Herramienta Server Migration

Azure Migrate: La herramienta Server Migration le ayuda a migrar máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtualizadas y máquinas virtuales de nube pública a Azure. Puede migrar máquinas después de evaluarlas o sin una valoración.


## <a name="select-a-tool"></a>Seleccionar una herramienta

En el concentrador de Azure Migrate, seleccione la herramienta que desea usar para la evaluación y agréguela a un proyecto Azure Migrate. Si agrega una herramienta de ISV o a Movere:

- Comience con la obtención de una licencia o regístrese para obtener una evaluación gratuita, de acuerdo con las instrucciones de la herramienta. Las licencias de herramientas vienen determinadas por la herramienta o el ISV. 
- En cada herramienta, existe una opción para conectarse a Azure Migrate. Siga las instrucciones para conectarse.
- Realice un seguimiento del recorrido de la migración desde el proyecto de Azure Migrate, en todas las herramientas.


## <a name="movere"></a>Movere

Movere es una plataforma SaaS que aumenta la inteligencia empresarial al presentar con precisión entornos de TI completos en un solo día. A medida que las organizaciones crecen, cambian y optimizan digitalmente, la solución proporciona a las empresas la confianza que necesitan para tener visibilidad y control de sus entornos, con independencia de la plataforma, aplicación o geografía. Microsoft ha [adquirido](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) Movere y ya no se vende como una oferta independiente.  Movere está disponible mediante los programas de Evaluación de soluciones y Economía de la nube de Microsoft. [Más información](https://www.movere.io) acerca de Movere. Si tiene alguna pregunta, envíela a movereq@microsoft.com o póngase en contacto con su representante de Microsoft.

Le recomendamos que consulte también Azure Migrate, nuestro servicio de migración integrado. Azure Migrate proporciona un centro para simplificar la migración a la nube. El centro ofrece compatibilidad completa con diferentes cargas de trabajo, incluidos servidores físicos y virtuales, bases de datos y aplicaciones. La visibilidad de un extremo a otro facilita el seguimiento del progreso en la detección, la evaluación y la migración. Con las herramientas de Azure y de ISV de asociados integradas, Azure Migrate también tiene una amplia gama de características, como la detección de servidores físicos y virtuales, el ajuste de tamaño correcto basado en el rendimiento, la planificación de costos, las evaluaciones basadas en la importación y análisis de dependencias de aplicaciones sin agente. Si está buscando ayuda experta para empezar, Microsoft cuenta con un [proveedor cualificado de servicios administrados por expertos de Azure](https://azure.microsoft.com/partners) para guiarle a lo largo del proceso. Consulte el [sitio web de Azure Migrate](https://azure.microsoft.com/services/azure-migrate/). 
 

## <a name="azure-migrate-versions"></a>Versiones de Azure Migrate

Existen dos versiones del servicio Azure Migrate:

- **Versión actual**: use esta versión para crear proyectos de Azure Migrate, detectar máquinas locales y organizar valoraciones y migraciones. [Obtenga más información](whats-new.md) sobre las novedades de esta versión.
- **Versión anterior**: Si utilizaba la versión anterior de Azure Migrate (solo se admitía la valoración de máquinas virtuales de VMware locales), ahora debería usar la versión actual. Ya no puede crear proyectos de Azure Migrate con la versión anterior y se recomienda no realizar nuevas detecciones. Para acceder a los proyectos existentes, en Azure Portal, busque y seleccione **Azure Migrate**. En el panel de **Azure Migrate**, hay una notificación y un vínculo para acceder a proyectos antiguos de Azure Migrate.



## <a name="next-steps"></a>Pasos siguientes

- Pruebe nuestros tutoriales para evaluar [máquinas virtuales de VMware](tutorial-prepare-vmware.md), [máquinas virtuales de Hyper-V](tutorial-prepare-hyper-v.md) y [servidores físicos](tutorial-prepare-physical.md).
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
