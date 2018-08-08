---
title: Recomendaciones sobre continuidad empresarial y recuperación ante desastres (BCDR) con una instancia de Integration Runtime para la integración de SSIS en Azure | Microsoft Docs
description: En este artículo se describen las recomendaciones sobre continuidad empresarial y recuperación ante desastres en una instancia de Integration Runtime para la integración de SSIS en Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/26/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 37347df2d543116085f52fed76c692b60fac2ad6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39295399"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-recommendations-for-azure-ssis-integration-runtime"></a>Recomendaciones sobre continuidad empresarial y recuperación ante desastres (BCDR) en una instancia de Integration Runtime para la integración de SSIS en Azure

A efectos de recuperación ante desastres, puede detener su entorno de ejecución para la integración de SSIS en Azure en la región en la que se ejecuta actualmente y cambiar a otra región para iniciarla de nuevo. Se recomienda usar [regiones emparejadas de Azure](../best-practices-availability-paired-regions.md) para este propósito.

## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que ha habilitado la recuperación ante desastres para el servidor de Azure SQL Database en caso de que el servidor tenga una interrupción del servicio al mismo tiempo. Para más información, vea [Introducción a la continuidad empresarial con Azure SQL Database](../sql-database/sql-database-business-continuity.md).

- Si usa una red virtual en la región actual, tiene que usar otra red virtual en la nueva región para conectarse a un entorno de ejecución para la integración de SSIS en Azure. Para más información, vea [Unión de un entorno de ejecución para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Si utiliza una instalación personalizada, puede que tenga que preparar otro URI de SAS para el contenedor de blobs que almacena el script de instalación personalizada y los archivos asociados, de modo que siga siendo accesible durante una interrupción. Para más información, vea [Configure a custom setup on an Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Configuración de una instalación personalizada en un entorno de ejecución para la integración de SSIS en Azure).

## <a name="steps"></a>Pasos

Siga estos pasos para detener el entorno de ejecución de integración de SSIS en Azure, cambie el entorno de ejecución de integración a otra región y vuelva a iniciarlo.

1. Detenga la instancia del entorno de ejecución de integración en la región original.

2. Llame al comando siguiente en PowerShell para actualizar el entorno de ejecución de integración.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -Location "new region" `
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
