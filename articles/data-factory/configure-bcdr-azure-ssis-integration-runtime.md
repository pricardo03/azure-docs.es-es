---
title: Configurar Azure-SSIS Integration Runtime para la conmutación por error de SQL Database | Microsoft Docs
description: En este artículo se describe cómo configurar Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de Azure SQL Database para la base de datos SSISDB.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/14/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: dea0153b9ca6d8e751fd94cc558abd44b2591907
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66120438"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Configurar Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de Azure SQL Database

En este artículo se describe cómo configurar Azure-SSIS Integration Runtime con la replicación geográfica de Azure SQL Database para la base de datos SSISDB. Cuando se produce una conmutación por error, puede asegurarse de que Azure-SSIS IR sigue funcionando con la base de datos secundaria.

Para obtener más información acerca de la replicación geográfica y conmutación por error de SQL Database, consulte [información general: Grupos de conmutación por error automática y replicación geográfica activos](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

### <a name="conditions"></a>Condiciones

Esta sección se aplica cuando se cumplen las condiciones siguientes:

- Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura del grupo de conmutación por error.

  Y

- El servidor de SQL Database *no* está configurado con la regla de punto de conexión de servicio de red virtual.

### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, es transparente para Azure-SSIS IR. Azure-SSIS IR se conecta automáticamente a la nueva instancia principal del grupo de conmutación por error.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

### <a name="conditions"></a>Condiciones

Esta sección se aplica cuando se cumple una de las condiciones siguientes:

- Azure-SSIS IR señala al punto de conexión del servidor principal del grupo de conmutación por error. Este punto de conexión cambia cuando se produce una conmutación por error.

  OR

- El servidor de Azure SQL Database está configurado con la regla de punto de conexión de servicio de red virtual.

  OR

- El servidor de base de datos es una Instancia administrada SQL Database configurada con una red virtual.

### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga la instancia de Integration Runtime de SSIS de Azure.

2. Vuelva a configurar IR para que señale al nuevo punto de conexión principal y a una red virtual en la nueva región.

3. Reinicie IR.

En las siguientes secciones se describen estos pasos con más detalle.

### <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que ha habilitado la recuperación ante desastres para el servidor de Azure SQL Database en caso de que el servidor tenga una interrupción del servicio al mismo tiempo. Para más información, vea [Introducción a la continuidad empresarial con Azure SQL Database](../sql-database/sql-database-business-continuity.md).

- Si usa una red virtual en la región actual, tiene que usar otra red virtual en la nueva región para conectarse a un entorno de ejecución para la integración de SSIS en Azure. Para más información, vea [Unión de un entorno de ejecución para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Si utiliza una instalación personalizada, puede que tenga que preparar otro URI de SAS para el contenedor de blobs que almacena el script de instalación personalizada y los archivos asociados, de modo que siga siendo accesible durante una interrupción. Para más información, vea [Configure a custom setup on an Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Configuración de una instalación personalizada en un entorno de ejecución para la integración de SSIS en Azure).

### <a name="steps"></a>Pasos

Siga estos pasos para detener el entorno de ejecución de integración de SSIS en Azure, cambie el entorno de ejecución de integración a otra región y vuelva a iniciarlo.

1. Detenga la instancia del entorno de ejecución de integración en la región original.

2. Llame al comando siguiente en PowerShell para actualizar IR con la nueva configuración.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Para más información sobre este comando de PowerShell, vea [Creación de un entorno de ejecución para la integración de SSIS en Azure en Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. Vuelva a iniciar el entorno de ejecución de integración.

## <a name="next-steps"></a>Pasos siguientes

Tenga en cuenta estas otras opciones de configuración para el entorno de ejecución de integración de SSIS en Azure:

- [Configuración de una instancia de Integration Runtime para la integración de SSIS en Azure para conseguir un alto rendimiento](configure-azure-ssis-integration-runtime-performance.md)

- [Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Aprovisionamiento de Enterprise Edition en la instancia de Integration Runtime para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-enterprise-edition.md)
