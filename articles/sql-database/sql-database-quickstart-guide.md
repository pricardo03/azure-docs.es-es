---
title: 'Inicio rápido: Bases de datos únicas en Azure SQL Database | Microsoft Docs'
description: Aprenda a empezar a trabajar rápidamente con las bases de datos únicas en Azure SQL Database
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
ms.openlocfilehash: 72ffdb357b5469b71da7655104add1135bf114fb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464823"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Introducción a las bases de datos únicas en Azure SQL Database

En Azure SQL Database, una [base de datos única](sql-database-single-index.yml) es una base de datos como servicio PaaS totalmente administrada (DbaaS), el motor de almacenamiento ideal para las aplicaciones modernas creadas en la nube. En esta sección, aprenderá a crear y configurar rápidamente la base de datos SQL.

## <a name="quickstart-overview"></a>Introducción al artículo de inicio rápido

En esta sección, verá información general de los artículos disponibles que le ayudarán a empezar a trabajar rápidamente con las bases de datos únicas. Para crear su primera base de datos de SQL, lo más sencillo es usar [Azure Portal](sql-database-get-started-portal.md), donde puede configurar los parámetros necesarios.
Después de la creación, necesitaría [proteger la base de datos mediante la configuración de reglas de firewall](sql-database-get-started-portal-firewall.md). 

Si tiene una base de datos en SQL Server que desea migrar a Azure, debe instalar [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595), que analizará las bases de datos de SQL Server y buscará cualquier problema que pudiera bloquear la migración a una base de datos única. Si no se encuentra ninguno, puede exportar la base de datos como archivo `.bacpac` e [importarlo mediante Azure Portal o SqlPackage](sql-database-import.md).

Estos artículos de inicio rápido permiten configurar, crear e importar rápidamente las bases de datos en la nube de Azure.

## <a name="automating-management-operations"></a>Automatización de las operaciones de administración

Azure Portal permite crear y modificar la base de datos única fácilmente. Como alternativa, puede usar [Powershell](scripts/sql-database-create-and-configure-database-powershell.md) o la [CLI de Azure](scripts/sql-database-create-and-configure-database-cli.md) para crear bases de datos.
También puede actualizar la base de datos única y los recursos de escalado mediante [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) o la [CLI de Azure](scripts/sql-database-monitor-and-scale-database-cli.md).

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Migración a una base de datos única con tiempo de inactividad mínimo

Estos artículos de inicio rápido permiten crear o importar rápidamente la base de datos a Azure mediante `.bacpac`. Los archivos `.bacpac` y `.dacpack` están diseñados para mover rápidamente la base de datos entre las distintas versiones de SQL Server y Azure SQL Database (base de datos única, grupo elástico o instancia administrada) o para implementar la integración continua en la canalización de DevOps. Sin embargo, este método no está diseñado para la migración de las bases de datos de producción con tiempo de inactividad mínimo, ya que se necesitaría esperar la exportación de la base de datos de origen como archivo `.bacpac` y su importación a Azure SQL Database, lo que provocaría tiempo de inactividad de la aplicación, en particular con bases de datos de mayor tamaño. Si va a mover la base de datos de producción, probablemente necesite una mejor migración, que garantice un tiempo de inactividad mínimo. [Data Migration Service](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) es un servicio que permite migrar la base de datos con tiempo de inactividad mínimo. De este modo, puede cambiar rápidamente la aplicación de la base de datos de origen a la de destino con tiempo de inactividad mínimo.

## <a name="next-steps"></a>Pasos siguientes

* Buscar una [lista detallada de las características admitidas en Azure SQL Database](sql-database-features.md). 
* Aprender a [proteger mejor la base de datos](sql-database-security-tutorial.md). 
* Buscar tutoriales más avanzados en la [sección de procedimientos](sql-database-howto-single-database.md). 
* Buscar más scripts de ejemplo escritos en [PowerShell](sql-database-powershell-samples.md) y la [CLI de Azure](sql-database-cli-samples.md). 
* Obtener más información sobre la [API de administración](sql-database-single-databases-manage.md) que puede usar para configurar las bases de datos. 
