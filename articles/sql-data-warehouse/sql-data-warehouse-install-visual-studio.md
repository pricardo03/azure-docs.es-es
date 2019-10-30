---
title: Instalación de Visual Studio 2019 para SQL Data Warehouse | Microsoft Docs
description: Instalación de herramientas de desarrollo de Visual Studio y SQL Server Data Tools (SSDT) para Azure SQL Data Warehouse
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/17/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 81c709e7705e16484438ab684a6b1591e5e624ba
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553512"
---
# <a name="getting-started-with-visual-studio-2019-for-sql-data-warehouse"></a>Introducción a Visual Studio 2019 para SQL Data Warehouse
SQL Server Data Tools (SSDT) para Visual Studio **2019** es una herramienta única que permite hacer lo siguiente:

- Conectarse a aplicaciones para SQL Data Warehouse, consultarlas y desarrollarlas 
- Aprovechar un explorador de objetos para examinar de forma visual todos los objetos del modelo de datos, lo que incluye las tablas, las vistas, los procedimientos almacenados, etc.
- Generar scripts de lenguaje de definición de datos (DDL) de T-SQL para los objetos
- Desarrollar el almacenamiento de datos mediante un enfoque basado en el estado con proyectos de base de datos de SSDT
- Integrar el proyecto de base de datos con sistemas de control de código fuente como Git con repositorios de Azure DevOps
- Configuración de canalizaciones de integración e implementación continuas con servidores de automatización como Azure DevOps

> [!NOTE]
> Actualmente, los proyectos de base de datos de SSDT para Visual Studio se encuentran en versión preliminar. Para recibir actualizaciones periódicas sobre esta característica, vote en [UserVoice].

## <a name="install-visual-studio-2019-preview"></a>Instalación de Visual Studio 2019 Preview
Consulte [Descargar Visual Studio 2019][] para descargar e instalar Visual Studio **16.3 o posteriores**. Durante la instalación, seleccione la carga de trabajo de almacenamiento de datos y procesamiento. La instalación independiente de SSDT ya no es necesaria en Visual Studio 2019.

## <a name="reporting-issues-with-ssdt-visual-studio-2019-preview"></a>Notificación de problemas con SSDT para Visual Studio 2019 (versión preliminar)
Para notificar problemas al usar SSDT con SQL Data Warehouse, envíe un correo electrónico a la siguiente lista de distribución de correo electrónico: <sqldwssdtpreview@service.microsoft.com>

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene la versión más reciente de SSDT, ya puede [conectarse][connect] a su instancia de SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Descargar Visual Studio 2019]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
