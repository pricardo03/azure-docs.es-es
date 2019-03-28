---
title: Azure App Service para la compatibilidad con tiempo de ejecución de Java de Windows
description: En este tema se proporciona la instrucción en tiempo de ejecución de Java de soporte técnico para Azure App Service en Windows.
author: bmitchell287
ms.author: brendm; joshuapa
ms.date: 3/22/2019
ms.topic: article
ms.service: app-service
manager: dougE
ms.openlocfilehash: ab0e9b9f272108745b629d2cb284f6c5ab52c76f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58523046"
---
# <a name="java-runtime-statement-of-support-for-app-service-on-windows"></a>Instrucción de tiempo de ejecución de Java de la compatibilidad con App Service en Windows

## <a name="jdk-versions-and-maintenance"></a>Mantenimiento y versiones de JDK

El JDK (Java Development Kit) compatible con Azure es [Zulu](https://www.azul.com/downloads/azure-only/zulu/) y se distribuye a través de [Azul Systems](https://www.azul.com/).

Las actualizaciones de versión principal se proporcionará a través de nuevas opciones de tiempo de ejecución en Azure App Service para Windows. Para actualizar a estas versiones más recientes, los clientes deben configurar su implementación de App Service. Asimismo, son responsables de probar y garantizar que la actualización principal satisface sus necesidades.

Los JDK compatibles se revisarán trimestralmente de manera automática en enero, abril, julio y octubre de cada año.

## <a name="security-updates"></a>Actualizaciones de seguridad

Las revisiones y correcciones de las vulnerabilidades de seguridad principales se publicarán en cuanto estén disponibles a través de Azul Systems. Una vulnerabilidad "principal" se define con una puntuación total de 9.0 o superior en el [CVSS (Common Vulnerability Scoring System), versión 2 del NIST](https://nvd.nist.gov/cvss.cfm).

## <a name="deprecation-and-retirement"></a>Desuso y retirada

Si un entorno compatible de Java Runtime se va a retirar, los desarrolladores de Azure que lo usen recibirán un aviso de desuso al menos seis meses antes de su retirada.

## <a name="local-development"></a>Desarrollo local

Los desarrolladores pueden descargar el JDK de Azul Zulu Enterprise Production Edition para el desarrollo local desde el [sitio de descarga de Azul](https://www.azul.com/downloads/azure-only/zulu/).

## <a name="development-support"></a>Soporte para el desarrollo

Soporte técnico para el [compatibles con Azure Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) está disponible a través de Microsoft al desarrollar para Azure o [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) con un [completo plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).

## <a name="runtime-support"></a>Compatibilidad con el tiempo de ejecución

Los desarrolladores pueden [abrir una incidencia](/azure/azure-supportability/how-to-create-azure-support-request) con los JDK de Azul Zulu a través de soporte técnico de Azure si tienen un [plan de soporte técnico completo](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Pasos siguientes

En este tema se proporciona la instrucción en tiempo de ejecución de Java de soporte técnico para Azure App Service en Windows.
- Para más información sobre el hospedaje de aplicaciones web con el servicio de aplicación de Azure, consulte [Introducción a App Service](overview.md).
- Para obtener información acerca de Java en el desarrollo de Azure, consulte [Azure para el centro de desarrollo de Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
