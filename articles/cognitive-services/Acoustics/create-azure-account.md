---
title: Configuración de la cuenta de Azure Batch de proyecto acústica
titlesuffix: Azure Cognitive Services
description: Este procedimiento describe cómo configurar una cuenta de Azure Batch para su uso con integraciones de motor de proyecto acústica Unity y Unreal.
services: cognitive-services
author: ashtat
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 7a7f5f6738b4bc96b6248deb062c7b3f63048148
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61335761"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Configuración de la cuenta de Azure Batch de proyecto acústica
Este procedimiento describe cómo configurar una cuenta de Azure Batch para su uso con integraciones de motor de proyecto acústica Unity y Unreal.

## <a name="get-an-azure-subscription"></a>Obtener una suscripción de Azure
Se necesita una [suscripción de Azure](https://azure.microsoft.com/free/) antes de configurar cuentas de Batch y Storage. Si se suscribe por primera vez, Azure ofrece algunos recursos gratis por un tiempo limitado y USD 200 de crédito.

## <a name="create-azure-batch-and-storage-accounts"></a>Creación de cuentas de Azure Batch y Storage
A continuación, siga [estas instrucciones](https://docs.microsoft.com/azure/batch/batch-account-create-portal) para configurar sus cuentas de Azure Batch y sus cuentas asociadas de Azure Storage.

Elija opciones predeterminadas para las cuentas de Batch y Storage:
  
  ![Captura de pantalla de Azure Batch nuevas opciones que muestra la configuración predeterminada de cuentas](media/new-batch-account-create.png)

  ![Captura de pantalla de Azure Storage nueva que muestra la configuración predeterminada de las opciones de cuentas](media/batch-storage-account-create.png)

Azure tarda unos minutos en implementar las cuentas. Busque una notificación de finalización en la esquina superior derecha del portal.
  
  ![Las cuentas de captura de pantalla de Azure implementan notificación](media/batch-accounts-deploy-notification.png)

Las cuentas ahora deberían aparecer en el panel.
  
  ![Panel de captura de pantalla de Azure portal que muestra una cuenta de Batch y Storage](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Configuración de la interfaz de usuario de elaboración con credenciales de Azure
Haga clic en el vínculo de la cuenta de Batch en el panel y luego en el vínculo **Keys** (Claves) en la página de la cuenta de Batch para acceder a sus credenciales.
  
  ![Cuenta de captura de pantalla de Azure Batch con vínculo a la página claves de resaltado](media/batch-access-keys.png)

  ![Página de claves de cuenta de captura de pantalla de Azure Batch con las teclas de acceso](media/batch-keys-info.png)

Haga clic en el vínculo de la **cuenta de Storage** en la página para acceder a las credenciales de su cuenta de Azure Storage.
  
  ![Página de claves de cuenta de captura de pantalla de Azure Storage con las teclas de acceso](media/storage-keys-info.png)

Escriba estas credenciales en el [Unity preparar complemento](unity-baking.md) o [hornearlos Unreal complemento](unreal-baking.md).

## <a name="node-types-and-region-support"></a>Compatibilidad de región y tipos de nodo
Proyecto acústica requiere que serie Fsv2 y H proceso optimizado para los nodos de máquina virtual de Azure que podrían no admitirse en todas las regiones de Azure. Revise [esta tabla](https://azure.microsoft.com/global-infrastructure/services) para asegurarse de elegir la ubicación correcta para la cuenta de Batch.
![Captura de pantalla que muestra máquinas virtuales de Azure por región](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Actualización de la cuota
Las cuentas de Azure Batch se aprovisionan al crear la cuenta con un límite de 20 núcleos de proceso. Nos conviene aumentar este límite en tiempos más rápidos de preparar, porque puede paralelizar la carga de trabajo acústica entre varios nodos, hasta el número de puntos de sondeo de la escena. Para solicitar un aumento de cuota, haga clic en el vínculo **Cuota** en la página del portal de Azure Batch y luego en **Solicitar un aumento de la cuota**:

![Página de captura de pantalla de la cuota de Azure](media/azure-quotas.png)

## <a name="next-steps"></a>Pasos siguientes
* Integrar el complemento de Project acústica en su [Unity](unity-integration.md) o [Unreal](unreal-integration.md) proyecto

