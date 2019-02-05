---
title: 'Inicio rápido: Instancia administrada de Azure SQL Database | Microsoft Docs'
description: Introducción a Azure SQL Database (Instancia administrada)
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 77deed43c106a451d3de768989233c749e1280e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468179"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Introducción a Instancia administrada de Azure SQL Database

[Instancia administrada de Azure SQL Database](sql-database-managed-instance-index.yml) es una versión de PaaS totalmente administrada de SQL Server que se hospeda en la nube de Azure y se encuentra en la red virtual propia con una dirección IP privada. En esta sección aprenderá a crear y configurar rápidamente Instancia administrada y a migrar las bases de datos.

## <a name="quickstart-overview"></a>Introducción al artículo de inicio rápido

En esta sección verá información general de los artículos disponibles que le ayudarán a empezar a trabajar rápidamente con las instancias administradas. La manera más fácil de crear la primera instancia administrada es usar [Azure Portal](sql-database-managed-instance-get-started.md), donde puede configurar los parámetros necesarios (nombre de usuario/contraseña, número de núcleos o almacenamiento máximo) y crear automáticamente el entorno de red de Azure sin necesidad de conocer los detalles de red ni los requisitos de infraestructura. Asegúrese de que tiene un [tipo de suscripción](sql-database-managed-instance-resource-limits.md#supported-subscription-types) con permiso para crear la instancia.

Si tiene una red propia que desee utilizar o so desea personalizar la red, consulte cómo [configurar el entorno de red](#configure-network-environment) para Instancia administrada.

Al crear la instancia administrada necesitará conectarse a ella con uno de los siguientes métodos:

* Cree una [máquina virtual de Azure](sql-database-managed-instance-configure-vm.md) con SQL Server Management Studio instalado y otras aplicaciones que se pueden usar para acceder a la instancia administrada en una subred de la misma red virtual donde se encuentra la instancia administrada. La máquina virtual no puede estar en la misma subred de las instancias administradas.
* Configure una [conexión de punto a sitio](sql-database-managed-instance-configure-p2s.md) en el equipo que le permita unir el equipo a la red virtual donde se coloca la instancia administrada y utilice esta última como cualquier otro servidor de SQL Server de la red.

Como alternativa, puede usar ExpressRoute o una conexión de sitio a sitio desde la red local, pero estos métodos están fuera del ámbito de estos artículos de inicio rápido.

Al crear una instancia administrada y configurar el acceso, puede iniciar la migración de las bases de datos de SQL Server local o de máquinas virtuales de Azure. Tenga en cuenta que la migración producirá un error si tiene características no admitidas en la base de datos de origen que desee migrar. Para evitar errores y comprobar la compatibilidad, puede instalar [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595), que analizará las bases de datos de SQL Server y detectará cualquier problema que pudiera bloquear la migración a instancia administrada, como la existencia de secuencias de a archivos o varios archivos de registro. Si resuelve estos problemas, las bases de datos estarán preparadas para pasar a Instancia administrada. El [Asistente para experimentación con bases de datos](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) es otra herramienta útil que puede registrar la carga de trabajo de SQL Server y reproducirla en Instancia administrada para que pueda determinar si va a haber problemas de rendimiento si se realiza la migración a Instancia administrada.

Una vez que esté seguro de que puede migrar la base de datos a Instancia administrada, puede usar la funcionalidad [RESTORE nativa](sql-database-managed-instance-get-started-restore.md), que permite crear una copia de seguridad de la base de datos mediante el comando de Transact-SQL, cargarla en Azure Blob Storage y restaurar la base de datos de Blob Storage mediante el comando de Transact-SQL.

Estos artículos de inicio rápido permiten configurar, crear y colocar rápidamente las bases de datos en las instancias administradas. En algunos escenarios, necesitaría personalizar o automatizar la implementación de Instancia administrada y el entorno de red necesario. Estos escenarios se describen a continuación.

## <a name="customizing-network-environment"></a>Personalización del entorno de red

Aunque la red virtual o subred se pueden configurar automáticamente al crearse la instancia mediante [Azure Portal](sql-database-managed-instance-get-started.md), podría ser conveniente crearla antes de iniciar la creación de instancias administradas, ya que podrá configurar los parámetros de la red virtual y de la subred. La manera más fácil de crear y configurar el entorno de red es usar la plantilla de [implementación de recursos de Azure](sql-database-managed-instance-create-vnet-subnet.md), que le ayudará a crear y configurar la red y la subred donde se colocará la instancia. Basta con presionar el botón de implementación de Azure Resource Manager y rellenar el formulario con los parámetros. Como alternativa, puede usar el [script de PowerShell](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) para automatizar la creación de la red.

Si ya tiene una red virtual y una subred donde le gustaría implementar la instancia administrada, deberá asegurarse de que estas satisfagan los [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Debe usar el [script de PowerShell para verificar que la subred está correctamente configurada](sql-database-managed-instance-configure-vnet-subnet.md). Este script no solo validará la red y notificará cualquier problema: le indicará lo que debe cambiarse y también le ofrecerá realizar los cambios necesarios en la red virtual o la subred. Ejecute este script si no desea configurar manualmente la red virtual o la subred. También debe ejecutarlo tras cualquier reconfiguración importante de la infraestructura de la red. Si desea crear y configurar su propia red, consulte la [documentación de Instancia administrada](sql-database-managed-instance-connectivity-architecture.md) y [esta guía](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-managed-instance"></a>Automatización de la creación de Instancia administrada

 Si no ha creado el entorno de red como se describe en el paso anterior, Azure Portal puede hacerlo por usted, el único inconveniente es el hecho de que se configurará con algunos parámetros predeterminados que no podrá modificar más adelante. Como alternativa, puede usar [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), [PowerShell con la plantilla de Resource Manager](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) o la [CLI de Azure](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).

## <a name="migrating-to-managed-instance-with-minimal-downtime"></a>Migración a Instancia administrada con tiempo de inactividad mínimo

Estos artículos de inicio rápido permiten configurar Instancia administrada y mover las bases de datos rápidamente. Sin embargo, con la restauración nativa deberá esperar a que se restauren las bases de datos, lo que podría suponer tiempo de inactividad de la aplicación, en particular, con bases de datos mayores. Si va a mover la base de datos de producción, probablemente necesite una mejor migración, que garantice un tiempo de inactividad mínimo. [Data Migration Service](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) es un servicio de migración que puede migrar la base de datos con tiempo de inactividad mínimo mediante la inserción incremental de los cambios realizados en la base de datos de origen en una base de datos que esté restaurando en la instancia administrada. De este modo, puede cambiar rápidamente la aplicación de la base de datos de origen a la de destino con tiempo de inactividad mínimo.

## <a name="next-steps"></a>Pasos siguientes

* Buscar una [lista detallada de las características admitidas en Instancia administrada](sql-database-features.md), y [detalles y problemas conocidos](sql-database-managed-instance-transact-sql-information.md). 
* Información sobre las [características técnicas de Instancia administrada](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
* Buscar tutoriales más avanzados en la [sección de procedimientos](sql-database-howto-managed-instance.md). 
