---
title: 'Ejemplos de la CLI de Azure: App Service | Microsoft Docs'
description: 'Ejemplos de la CLI de Azure: App Service'
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 12/12/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: dc8a08c61250994b6083e3936820e1e6025593a6
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70066703"
---
# <a name="cli-samples-for-azure-app-service"></a>Ejemplos de la CLI para Azure App Service

En la tabla siguiente se incluyen vínculos a scripts de Bash creados con la CLI de Azure.

| | |
|-|-|
|**Creación de la aplicación**||
| [Creación de una aplicación e implementación de archivos con FTP](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service e implementa un archivo mediante FTP. |
| [Creación de una aplicación e implementación de código desde GitHub](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service e implementa código proveniente de un repositorio público de GitHub. |
| [Creación de una aplicación con implementación continua desde GitHub](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una publicación de App Service con publicación continua desde un repositorio de GitHub de su propiedad. |
| [Creación de una aplicación e implementación de código desde un repositorio local de GitHub](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y configura la inserción de código desde un repositorio de Git local. |
| [Creación de una aplicación e implementación de código en un entorno de ensayo](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service con una ranura de implementación para cambios en el código de ensayo. |
| [Creación de una aplicación de ASP.NET Core en un contenedor de Docker](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service en Linux y carga una imagen de Docker desde Docker Hub. |
|**Configuración de la aplicación**||
| [Asignar un dominio personalizado a una aplicación](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service y le asigna un nombre de dominio personalizado. |
| [Enlace de un certificado SSL personalizado a una aplicación](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service y enlaza a ella el certificado SSL de un nombre de dominio personalizado. |
|**Escalado de la aplicación**||
| [Escalado manual de una aplicación](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y la escala a través de dos instancias. |
| [Escalado de una aplicación en todo el mundo con una arquitectura de alta disponibilidad](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Crea dos aplicaciones de App Service en dos regiones geográficas diferentes y hace que estén disponibles mediante un punto de conexión único con Azure Traffic Manager. |
|**Conexión de la aplicación a recursos**||
| [Conexión de una aplicación a SQL Database](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service y una base de datos SQL y, a continuación, agrega la cadena de conexión de base de datos a la configuración de la aplicación. |
| [Conexión de una aplicación a una cuenta de almacenamiento](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una aplicación de App Service y una cuenta de almacenamiento y, a continuación, agrega la cadena de conexión de almacenamiento a la configuración de la aplicación. |
| [Conexión de una aplicación a Azure Cache for Redis](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y una instancia de Azure Cache for Redis, y después agrega los detalles de conexión de Redis a la configuración de la aplicación. |
| [Conexión de una aplicación a Cosmos DB](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y una instancia de Cosmos DB y, luego, agrega los detalles de conexión de Cosmos DB a la configuración de la aplicación. |
|**Copia de seguridad y restauración de la aplicación**||
| [Copia de seguridad de una aplicación](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y una copia de seguridad única para ella. |
| [Creación de una copia de seguridad programada para una aplicación](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service y una copia de seguridad programada para ella. |
| [Restauración de una aplicación a partir de una copia de seguridad](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Restaura una aplicación de App Service a partir de una copia de seguridad. |
|**Supervisión de la aplicación**||
| [Supervisión de una aplicación con registros de servidor web](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una aplicación de App Service, habilita el registro para ella y descarga los registros en la máquina local. |
| | |