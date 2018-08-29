---
title: 'Configuración de cuentas de Azure para la acústica: Cognitive Services'
description: Siga esta guía para configurar las cuentas de Azure Batch y Storage necesarias para trabajar con la acústica.
services: cognitive-services
author: ashtat
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: d5e78df2cb17e8275aef3694dda90a705ef4bdaa
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181375"
---
# <a name="create-an-azure-batch-account"></a>Creación de una cuenta de Azure Batch
Siga esta guía para configurar las cuentas de Azure Batch y Storage necesarias para trabajar con la acústica. Para información sobre el complemento de Unity que se desarrolló como parte de Elementos acústicos de un proyecto, consulte [¿Qué son los Elementos acústicos de un proyecto?](what-is-acoustics.md) Para información sobre cómo incorporar la acústica en un proyecto de Unity, consulte la [introducción](getting-started.md).  

## <a name="get-an-azure-subscription"></a>Obtener una suscripción de Azure
Se necesita una [suscripción de Azure](https://azure.microsoft.com/free/) antes de configurar cuentas de Batch y Storage. Si se suscribe por primera vez, Azure ofrece algunos recursos gratis por un tiempo limitado y USD 200 de crédito.

## <a name="create-azure-batch-and-storage-accounts"></a>Creación de cuentas de Azure Batch y Storage
A continuación, siga [estas instrucciones](https://docs.microsoft.com/azure/batch/batch-account-create-portal) para configurar sus cuentas de Azure Batch y sus cuentas asociadas de Azure Storage.

Elija opciones predeterminadas para las cuentas de Batch y Storage:
  
  ![Nueva cuenta de Batch](media/NewBatchAccountCreate.png)

  ![Nueva cuenta de Storage](media/BatchStorageAccountCreate.png)

Azure tarda unos minutos en implementar las cuentas. Busque una notificación de finalización en la esquina superior derecha del portal.
  
  ![Cuentas implementadas](media/BatchAccountsDeployNotification.png)

Las cuentas ahora deberían aparecer en el panel.
  
  ![Panel del portal](media/AzurePortalDashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Configuración de la interfaz de usuario de elaboración con credenciales de Azure
Haga clic en el vínculo de la cuenta de Batch en el panel y luego en el vínculo **Keys** (Claves) en la página de la cuenta de Batch para acceder a sus credenciales.
  
  ![Vínculo de claves de Batch](media/BatchAccessKeys.png)

  ![Credenciales de cuenta de Batch](media/BatchKeysInfo.png)

Haga clic en el vínculo de la **cuenta de Storage** en la página para acceder a las credenciales de su cuenta de Azure Storage.
  
  ![Credenciales de la cuenta de almacenamiento](media/StorageKeysInfo.png)

Escriba estas credenciales en la pestaña Bake (Elaborar) como se describe en el [tutorial de la interfaz de usuario de elaboración](bake-ui-walkthrough.md).

## <a name="node-types-and-region-support"></a>Compatibilidad de región y tipos de nodo
Elementos acústicos de un proyecto requiere nodos de máquinas virtuales de Azure optimizados para proceso de la serie F y H que puede que no sean compatibles con todas las regiones de Azure. Revise [esta tabla](https://azure.microsoft.com/global-infrastructure/services) para asegurarse de elegir la ubicación correcta para la cuenta de Batch. En este momento, las máquinas virtuales de la serie H son compatibles con las regiones Este de EE. UU., Centro-norte de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Europa del Norte, Europa Occidental y Oeste de Japón.

## <a name="upgrading-your-quota"></a>Actualización de la cuota
Las cuentas de Azure Batch se aprovisionan al crear la cuenta con un límite de 20 núcleos de proceso. Es posible que quiera aumentar este límite para tener tiempos de elaboración más rápido, porque puede paralelizar la carga de trabajo acústica en muchos nodos, hasta el número de puntos de sondeo de la escena. Para solicitar un aumento de cuota, haga clic en el vínculo **Cuota** en la página del portal de Azure Batch y luego en **Solicitar un aumento de la cuota**:

![Aumento de la cuota de Azure](media/azurequotas.png)

## <a name="next-steps"></a>Pasos siguientes
* Introducción a la [integración de la acústica en un proyecto de Unity](getting-started.md)
* Explore la [escena de ejemplo](sample-walkthrough.md)

