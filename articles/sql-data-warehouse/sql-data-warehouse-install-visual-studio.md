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
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: e08d0a52903fb488359356707ecedfd81d49084a
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165067"
---
# <a name="getting-started-with-visual-studio-2019-for-sql-data-warehouse"></a>Introducción a Visual Studio 2019 para SQL Data Warehouse
SQL Server Data Tools (SSDT) para Visual Studio **2019** es una herramienta única que permite hacer lo siguiente:

- Conectarse a aplicaciones para SQL Data Warehouse, consultarlas y desarrollarlas 
- Aprovechar un explorador de objetos para examinar de forma visual todos los objetos del modelo de datos, lo que incluye las tablas, las vistas, los procedimientos almacenados, etc.
- Generar scripts de lenguaje de definición de datos (DDL) de T-SQL para los objetos
- Desarrollar el almacenamiento de datos mediante un enfoque basado en el estado con proyectos de base de datos de SSDT
- Integrar el proyecto de base de datos con sistemas de control de código fuente como Git con repositorios de Azure DevOps
- Configurar canalizaciones de integración e implementación continuas con servidores de automatización como Azure DevOps [próximamente]

> [!NOTE]
> Actualmente, los proyectos de base de datos de SSDT para Visual Studio se encuentran en versión preliminar. Para recibir actualizaciones periódicas sobre esta característica, vote en [UserVoice].

## <a name="install-visual-studio-2019-preview"></a>Instalación de Visual Studio 2019 Preview
Consulte [Descargar Visual Studio 2019 Preview][] para descargar e instalar Visual Studio. Durante la instalación, seleccione la carga de trabajo de almacenamiento de datos y procesamiento. La instalación independiente de SSDT ya no es necesaria en Visual Studio 2019.

## <a name="reporting-issues-with-ssdt-visual-studio-2019-preview"></a>Notificación de problemas con SSDT para Visual Studio 2019 (versión preliminar)

Para notificar problemas al usar SSDT con SQL Data Warehouse, envíe un correo electrónico a la siguiente lista de distribución de correo electrónico: <sqldwssdtpreview@service.microsoft.com>

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene la versión más reciente de SSDT, ya puede [conectarse][connect] a su instancia de SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Descargar Visual Studio 2019 Preview]: https://visualstudio.microsoft.com/vs/preview/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
