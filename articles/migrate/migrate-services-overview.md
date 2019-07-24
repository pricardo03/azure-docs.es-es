---
title: Acerca de Azure Migrate | Microsoft Docs
description: Proporciona información general acerca del servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5409ed799454a6bb64077ee884065fc518556142
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227810"
---
# <a name="about-azure-migrate"></a>Acerca de Azure Migrate

Este artículo proporciona una descripción general de Azure Migrate.

Azure Migrate le ayuda a migrar a Azure. Azure Migrate proporciona un centro principal para realizar el seguimiento de la detección, valoración y migración de infraestructuras, aplicaciones y datos locales a Azure. El centro proporciona herramientas de Azure para la valoración y migración, así como ofertas de proveedores de software independientes (ISV) externos. Ofrece:

- **Plataforma de migración unificada**: Use un solo portal para iniciar, ejecutar y realizar un seguimiento del recorrido de la migración a Azure.
- **Rango de herramientas**: Azure Migrate proporciona herramientas nativas y se integra con otros servicios de Azure, así como con las herramientas de ISV. Seleccione las herramientas de valoración y migración adecuadas, en función de los requisitos de la organización. 
- **Azure Migrate Server Assessment**: Use la herramienta Server Assessment para evaluar máquinas virtuales de VMware locales y máquinas virtuales de Hyper-V para la migración a Azure.
- **Azure Migrate Server Migration**: Use la herramienta Server Migration para migrar las máquinas virtuales de VMware locales, las máquinas virtuales de Hyper-V y los servidores físicos a Azure.
- **Azure Migrate Database Assessment**: Evalúe las bases de datos locales para migrarlas a Azure.
- **Azure Migrate Database Migration**: Migre bases de datos locales a Azure.


## <a name="azure-migrate-versions"></a>versiones de Azure Migrate

Existen dos versiones del servicio Azure Migrate:

- **Versión actual**: Use esta versión para crear proyectos de Azure Migrate, detectar máquinas locales y organizar valoraciones y migraciones. [Obtenga más información](whats-new.md) sobre las novedades de esta versión.
- **Versión anterior**: Si usaba la versión anterior de Azure Migrate (solo se admitía la valoración de máquinas virtuales de VMware locales), ahora debería usar la versión actual. Ya no puede crear proyectos de Azure Migrate con la versión anterior y se recomienda no realizar nuevas detecciones. Para obtener acceso a los proyectos existentes, en Azure Portal > **Todos los servicios**, busque **Azure Migrate**. En el panel de Azure Migrate, hay una notificación y un vínculo para acceder a proyectos antiguos de Azure Migrate.

## <a name="isv-integration"></a>Integración de ISV

Además de las herramientas nativas de Azure, Azure Migrate se integra con varias ofertas de ISV. Identifique la herramienta que necesita y agréguela a un proyecto de Azure Migrate. Puede realizar un seguimiento centralizado del recorrido de la migración desde el proyecto de Azure Migrate, en Azure y en las herramientas de ISV.

**ISV** | **Característica**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Evaluación
[Device 42](https://docs.device42.com/) | Evaluación
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Evaluación
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Evaluación
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Valoración y migración
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migrar

### <a name="selecting-an-isv-tool"></a>Seleccionar una herramienta de ISV

Después de agregar una herramienta de ISV a un proyecto de Azure Migrate, comience con la herramienta. Para ello, obtenga una licencia o regístrese para obtener una evaluación gratuita de acuerdo con la directiva de ISV. En cada herramienta, existe una opción para conectarse a Azure Migrate. Siga las instrucciones y la documentación de la herramienta para conectar la herramienta con Azure Migrate.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate Server Assessment

Azure Migrate Server Assessment detecta y evalúa máquinas virtuales de VMware locales y máquinas virtuales de Hyper-V para la migración a Azure. Le ayuda a identificar lo siguiente:

- **Preparación para Azure:** Evalúe si las máquinas locales están listas para la migración a Azure.
- **Ajuste de tamaño de Azure:** Calcule el tamaño de las máquinas virtuales de Azure después de la migración.
- **Estimación de costos de Azure:** Calcule el costo de la ejecución de servidores locales en Azure.
- **Visualización de dependencias:** Identifique dependencias entre servidores y la mejor manera de trasladar los servidores dependientes a Azure. 

Server Assessment usa un dispositivo ligero que se implementa en el entorno local y se registra con Server Assessment.

- El dispositivo detecta máquinas locales, se conecta a Server Assessment y envía continuamente metadatos y datos relacionados con el rendimiento a Azure Migrate.
- La detección es sin agente. No es necesario instalar nada en las máquinas virtuales detectadas.
- Una vez detectadas las máquinas, las recopila en grupos que normalmente se componen de las máquinas virtuales que quiere migrar juntas.
- Cree una evaluación para un grupo. A continuación, puede analizar la valoración para diseñar su estrategia de migración.

## <a name="azure-migrate-server-migration"></a>Azure Migrate Server Migration

Azure Migrate Server Migration le ayuda a migrar máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtualizadas y máquinas virtuales de nube pública a Azure. Puede migrar máquinas después de evaluarlas o sin una valoración. 

## <a name="azure-migrate-database-assessment"></a>Azure Migrate Database Assessment

Azure Migrate se integra con Data Migration Assistant (DMA) para evaluar las bases de datos de SQL Server locales para la migración a Azure SQL DB, Instancia administrada de Azure SQL o máquinas virtuales de Azure que ejecutan SQL Server. DMA proporciona información sobre los posibles problemas de bloqueo de la migración. Identifica características no admitidas, así como nuevas características de las que se puede aprovechar después de la migración, y ayuda a identificar la ruta de acceso correcta para la migración de la base de datos. [Más información](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).


## <a name="azure-migrate-database-migration"></a>Azure Migrate Database Migration

Azure Migrate se integra con Azure Database Migration Service (DMS) para migrar bases de datos locales a Azure. Use DMS para migrar bases de datos locales a máquinas virtuales de Azure que ejecutan SQL, Azure SQL DB e instancias administradas de Azure SQL. [Más información](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-assessment-and-migration"></a>Valoración y migración de aplicaciones web

En el centro de Azure Migrate, puede evaluar y migrar aplicaciones web locales a Azure.

- **Evaluar aplicaciones web en línea**: Use Azure App Service Migration Assistant para evaluar sitios web locales para la migración a Azure App Service.
- **Migrar aplicaciones web**: Migre aplicaciones web .NET y PHP a Azure con Azure App Service Migration Assistant.

[Más información.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Migración de datos sin conexión

Puede usar la familia de productos Data Box sin conexión para trasladar grandes cantidades de datos a Azure. [Más información](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Pasos siguientes

- Pruebe los tutoriales para evaluar [máquinas virtuales de VMware](tutorial-assess-vmware.md) y [máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md).
- [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
