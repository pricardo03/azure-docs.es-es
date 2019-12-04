---
title: Novedades de Azure Migrate
description: Obtenga información sobre las novedades y actualizaciones recientes del servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 11/18/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b3b8e24e7d84e492aeaba64573b6e909707bc5a5
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185750"
---
# <a name="whats-new-in-azure-migrate"></a>Novedades de Azure Migrate

[Azure Migrate](migrate-services-overview.md) le ayuda a detectar, evaluar y migrar servidores, aplicaciones y datos locales a la nube de Microsoft Azure. En este artículo se resumen las nuevas características de Azure Migrate.



## <a name="update-november-2019"></a>Actualización (noviembre de 2019)

Se han agregado varias características nuevas a Azure Migrate:

- **Evaluación de servidores físicos**. Ahora se admite la evaluación de servidores físicos locales, además de la migración de servidores físicos que ya se admite.
- **Evaluación basada en la importación**. Ahora se admite la evaluación de máquinas con metadatos y datos de rendimiento proporcionados en un archivo CSV.
- **Detección de aplicaciones**: Azure Migrate admite ahora la detección en el nivel de aplicación de aplicaciones, roles y características mediante el uso del dispositivo Azure Migrate. Actualmente solo se admite para máquinas virtuales de VMware y solo se limita a la detección (actualmente no se admite la evaluación). [Más información](how-to-discover-applications.md)
- **Visualización de dependencias sin agentes**: Ya no necesita instalar agentes explícitamente para la visualización de dependencias. Ahora se admiten tanto sin agente como basados en agente.
- **Virtual Desktop**: Use herramientas de ISV para evaluar y migrar la infraestructura de escritorio virtual (VDI) local a Windows Virtual Desktop en Azure.
- **Aplicación web**: Migration Assistant de Azure App Service, que se usa para evaluar y migrar aplicaciones web, ahora se integra en Azure Migrate.

Se han agregado a Azure Migrate nuevas herramientas para la evaluación y la migración:

- **Rackware**: Migración a la nube de ofertas.
- **Movere**: Evaluación de ofertas.

[Más información](migrate-services-overview.md) sobre el uso de herramientas y ofertas de ISV para la evaluación y la migración en Azure Migrate.

## <a name="release-version-july-2019"></a>Versión de lanzamiento (julio de 2019)

En julio de 2019 se publicó la versión actual de Azure Migrate.

- **Versión actual**: use esta versión para crear proyectos de Azure Migrate, detectar máquinas locales y organizar valoraciones y migraciones.
- **Versión anterior**: Para clientes que usaban la versión anterior de Azure Migrate (solo se admitía la evaluación de VM de VMware locales), ahora debería usar la versión actual. En la versión anterior, ya no puede crear nuevos proyectos de Azure Migrate ni realizar nuevas detecciones. Todavía puede obtener acceso a los proyectos existentes. Para hacerlo, en Azure Portal > **Todos los servicios**, busque **Azure Migrate**. En las notificaciones de Azure Migrate, hay un vínculo para acceder a antiguos proyectos de Azure Migrate.


### <a name="azure-migrate-features"></a>Características de Azure Migrate

La versión actual de Azure Migrate proporciona varias características nuevas:


- **Plataforma de migración unificada**: Ahora, Azure Migrate ofrece un único portal para centralizar, administrar y realizar el seguimiento del proceso de migración a Azure, con un flujo de implementación mejorado y una experiencia de portal.
- **Herramientas de evaluación y migración**: Azure Migrate proporciona herramientas nativas y se integra con otros servicios de Azure, así como con herramientas de fabricante de software independiente (ISV). [Más información](migrate-services-overview.md#isv-integration) acerca de la integración de ISV.
- **Evaluación de Azure Migrate**: Puede usar la herramienta de evaluación de servidores de Azure Migrate para evaluar VM de VMware y de Hyper-V para la migración a Azure. También puede evaluar la migración con otros servicios de Azure y herramientas de ISV.
- **Migración de Azure Migrate**: La herramienta de migración de servidores de Azure Migrate permite migrar VM de VMware locales y VM de Hyper-V a Azure, así como servidores físicos, otros servidores virtualizados y VM de nube privada o pública. Además, puede migrar a Azure mediante herramientas de ISV.
- **Dispositivo con Azure Migrate**: Azure Migrate implementa un dispositivo ligero para la detección y evaluación de VM de VMware locales y VM de Hyper-V.
    - La evaluación y la migración de servidores de Azure Migrate usan este dispositivo para realizar una migración sin agente.
    - El dispositivo detecta continuamente los metadatos de servidor y los datos de rendimiento con fines de evaluación y migración.  
- **Migración de VM de VMware**:  La migración de servidores de Azure Migrate proporciona un par de métodos para migrar VM de VMware locales a Azure.  Una migración sin agente mediante el dispositivo de Azure Migrate y una migración basada en agente que usa un dispositivo de replicación e implementa un agente en cada VM que quiere migrar. [Más información](server-migrate-overview.md)
 - **Valoración y migración de bases de datos**: desde Azure Migrate, puede evaluar las bases de datos locales para la migración a Azure mediante Azure Database Migration Assistant. Puede migrar bases de datos con Azure Database Migration Service.
- **Migración de aplicaciones web**: puede evaluar las aplicaciones web con una dirección URL de punto de conexión pública mediante Azure App Service. Para la migración de aplicaciones de .NET internas, puede descargar y ejecutar App Service Migration Assistant.
- **Data Box**: importe grandes cantidades de datos sin conexión a Azure mediante Azure Data Box en Azure Migrate.


## <a name="next-steps"></a>Pasos siguientes

- [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
- Pruebe nuestros tutoriales para evaluar las [VM de VMware](tutorial-assess-vmware.md) y [las VM de Hyper-V](tutorial-assess-hyper-v.md).
