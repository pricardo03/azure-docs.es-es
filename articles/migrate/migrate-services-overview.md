---
title: Acerca de Azure Migrate
description: Obtenga información acerca de la evaluación y la migración del servidor con el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 11/30/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5843e6a5669cd90cdc0572e7a104c4fdf70ac2b3
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74669126"
---
# <a name="about-azure-migrate"></a>Acerca de Azure Migrate

Este artículo proporciona una descripción general de Azure Migrate.

Azure Migrate le ayuda a migrar la empresa desde el entorno local a Azure. Azure Migrate proporciona un centro principal para realizar el seguimiento de la detección, valoración y migración de infraestructuras, aplicaciones y datos locales a Azure.  Azure Migrate proporciona:

- **Plataforma de migración unificada**: Use un único portal para iniciar, ejecutar y realizar un seguimiento del recorrido de la migración a Azure.
- **Rango de herramientas**: El centro proporciona la evaluación y la migración de herramientas de Azure Migrate y se integra con otros servicios de Azure, así como otras herramientas y ofertas de fabricantes independientes de software (ISV).
- **Cargas de trabajo**: Azure Migrate proporciona evaluaciones y migración para:
    - **Servidores**: Use la evaluación del servidor de Azure Migrate, la migración del servidor de Azure Migrate y otras herramientas para evaluar y migrar servidores a máquinas virtuales de Azure.
    - **Bases de datos**: Aproveche las herramientas de Microsoft e ISV para evaluar y migrar bases de datos locales a Azure SQL Database o a la Instancia administrada de Azure SQL.
    - **Aplicaciones web** Utilice Azure App Service Assistant para evaluar y migrar aplicaciones web locales a Azure App Service.
    - **Escritorios virtuales**: Use herramientas de ISV para evaluar y migrar la infraestructura de escritorio virtual (VDI) local a Windows Virtual Desktop en Azure.
    - **Data**: Utilice la familia de Azure Data Box de productos para migrar grandes cantidades de datos a Azure de manera rápida y rentable.

## <a name="azure-migrate-versions"></a>Versiones de Azure Migrate

Actualmente existen dos versiones del servicio Azure Migrate:

- **Versión actual**: Use esta versión para crear proyectos de Azure Migrate, detectar máquinas locales y organizar valoraciones y migraciones. [Obtenga más información](whats-new.md) sobre las novedades de esta versión.
- **Versión anterior**: Si utilizaba la versión anterior de Azure Migrate (solo se admitía la valoración de máquinas virtuales de VMware locales), ahora debería usar la versión actual. Ya no puede crear proyectos de Azure Migrate con la versión anterior y se recomienda no realizar nuevas detecciones. Para obtener acceso a los proyectos existentes, en Azure Portal > **Todos los servicios**, busque **Azure Migrate**. En el panel de Azure Migrate, hay una notificación y un vínculo para acceder a proyectos antiguos de Azure Migrate.



## <a name="isv-integration"></a>Integración de ISV

Además de las herramientas nativas de Azure, Azure Migrate se integra con varias ofertas de ISV. 

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

## <a name="azure-tool-integration"></a>Integración de herramientas de Azure

En la tabla se resumen otras herramientas que se integran en Azure Migrate.

**Herramienta** | **Detalles**
--- | ---
Azure Migrate: Server Assessment | Evaluar servidores
Azure Migrate: Server Migration | Migrar servidores
Database Migration Assistant (DMA) | Evaluar bases de datos
Database Migration Service (DMS) | Migración de bases de datos
Movere | Evaluar servidores
Migration Assistant para aplicaciones web | Evaluar y migrar aplicaciones web



### <a name="selecting-a-tool"></a>Seleccionar una herramienta

Identifique la herramienta que necesita y agréguela a un proyecto de Azure Migrate.

- Si va a agregar una herramienta de ISV o a Movere:
    - Comience mediante la obtención de una licencia o regístrese para obtener una evaluación gratuita de acuerdo con la directiva de la herramienta. La licencia para las herramientas se conceden de conformidad con el modelo de licencias de la herramienta o de ISV.
    - En cada herramienta, existe una opción para conectarse a Azure Migrate. Siga las instrucciones y la documentación de la herramienta para conectar la herramienta con Azure Migrate.
- Realice un seguimiento centralizado del recorrido de la migración desde el proyecto de Azure Migrate, en Azure y en otras herramientas.



## <a name="azure-migrate-server-assessment-tool"></a>Herramienta Server Assessment de Azure Migrate

Azure Migrate: La herramienta Server Assessment detecta y evalúa máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V y servidores físicos para la migración a Azure. Le ayuda a identificar lo siguiente:

- **Preparación para Azure:** Evalúe si las máquinas locales están listas para la migración a Azure.
- **Ajuste de tamaño de Azure:** Tamaño de las máquinas virtuales de Azure después de la migración.
- **Estimación de costos de Azure:** Costos estimados de la ejecución de servidores locales en Azure.
- **Visualización de dependencias:** Dependencias entre servidores (si está habilitada la visualización de dependencias) y las maneras óptimas de trasladar los servidores dependientes a Azure.

Server Assessment usa un dispositivo ligero que se implementa en el entorno local y se registra con Server Assessment.

- El dispositivo detecta las máquinas locales.
- Se conecta a Server Assessment y envía continuamente metadatos de las máquinas y datos relacionados con el rendimiento a Azure Migrate.
- La detección del dispositivo es sin agente. No es necesario instalar nada en las máquinas detectadas.
- Después de la detección, recopile las máquinas detectadas en grupos. Normalmente, se agrupan las máquinas que desea migrar juntas.
- Cree una evaluación para un grupo. Después, analice la valoración para diseñar su estrategia de migración.

## <a name="azure-migrate-server-migration-tool"></a>Herramienta Azure Migrate Server Migration

Azure Migrate: La herramienta Server Migration le ayuda a migrar máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtualizadas y máquinas virtuales de nube pública a Azure. Puede migrar máquinas después de evaluarlas o sin una valoración.


## <a name="database-migration-assistant"></a>Database Migration Assistant

Azure Migrate se integra con Microsoft Data Migration Assistant (DMA) para evaluar las bases de datos de SQL Server locales para la migración a Azure SQL DB, Instancia administrada de Azure SQL o máquinas virtuales de Azure que ejecutan SQL Server. DMA proporciona información sobre los posibles problemas de bloqueo de la migración. Identifica características no admitidas, así como nuevas características de las que se puede aprovechar después de la migración, y ayuda a identificar la ruta de acceso correcta para la migración de la base de datos. [Más información](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).

## <a name="database-migration-service"></a>Database Migration Service

Azure Migrate se integra con Azure Database Migration Service (DMS) para migrar bases de datos locales a Azure. Use DMS para migrar bases de datos locales a máquinas virtuales de Azure que ejecutan SQL, Azure SQL DB e instancias administradas de Azure SQL. [Más información](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="movere"></a>Movere

 
Movere es una plataforma SaaS que aumenta la inteligencia empresarial al presentar con precisión entornos de TI completos en un solo día. A medida que las organizaciones crecen, cambian y optimizan digitalmente, la solución proporciona a las empresas la confianza que necesitan para tener visibilidad y control de sus entornos, con independencia de la plataforma, aplicación o geografía. Microsoft ha [adquirido](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) Movere y ya no se vende como una oferta independiente.  Movere está disponible mediante los programas de Evaluación de soluciones y Economía de la nube de Microsoft. [Más información](https://www.movere.io) acerca de Movere. Si tiene alguna pregunta, envíela a movereq@microsoft.com o póngase en contacto con su representante de Microsoft.

Le recomendamos que consulte también Azure Migrate, nuestro servicio de migración integrado. Azure Migrate proporciona un centro para simplificar la migración a la nube. El centro ofrece compatibilidad completa con diferentes cargas de trabajo, incluidos servidores físicos y virtuales, bases de datos y aplicaciones. La visibilidad de un extremo a otro facilita el seguimiento del progreso en la detección, la evaluación y la migración. Con las herramientas de Azure y de ISV de asociados integradas, Azure Migrate también tiene una amplia gama de características, como la detección de servidores físicos y virtuales, el ajuste de tamaño correcto basado en el rendimiento, la planificación de costos, las evaluaciones basadas en la importación y análisis de dependencias de aplicaciones sin agente. Si está buscando ayuda experta para empezar, Microsoft cuenta con un [proveedor cualificado de servicios administrados por expertos de Azure](https://azure.microsoft.com/partners) para guiarle a lo largo del proceso. Consulte el [sitio web de Azure Migrate](https://azure.microsoft.com/services/azure-migrate/). 
 

## <a name="web-app-migration-assistant"></a>Migration Assistant para aplicaciones web

Azure Migrate se integra con Migration Assistant de Azure App Service. En el concentrador de Azure Migrate, puede evaluar y migrar aplicaciones web locales a Azure, mediante el asistente, de la forma siguiente.

- **Evaluar aplicaciones web en línea**: Use Azure App Service Migration Assistant para evaluar sitios web locales para la migración a Azure App Service.
- **Migrar aplicaciones web**: Migre aplicaciones web .NET y PHP a Azure con Azure App Service Migration Assistant.

[Más información](https://appmigration.microsoft.com/) acerca del asistente.



## <a name="offline-data-migration"></a>Migración de datos sin conexión

Puede usar los productos de Azure Data Box para trasladar sin conexión grandes cantidades de datos a Azure. [Más información](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Pasos siguientes

- Pruebe los tutoriales para evaluar [máquinas virtuales de VMware](tutorial-assess-vmware.md) y [máquinas virtuales de Hyper-V](tutorial-assess-hyper-v.md).
- [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
