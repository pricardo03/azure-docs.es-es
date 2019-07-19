---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: ad1527a5e7f1cb2ff1beb9ddace5460f41bb8a87
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461518"
---
## <a name="azure-cognitive-service-subscription-types"></a>Tipos de suscripción de Azure Cognitive Service

> [!NOTE]
> Los propietarios de las suscripciones pueden deshabilitar las opciones de creación de cuentas de Cognitive Services para los grupos de recursos y las suscripciones; para ello, deben aplicar la [directiva de Azure](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), asignar una definición de directiva "Tipos de recursos no permitidos" y especificar **Microsoft.CognitiveServices/accounts** como el tipo de recurso de destino.

Puede tener acceso a Azure Cognitive Services a través de dos suscripciones distintas: Una suscripción a varios servicios o una suscripción a un solo servicio. Estas suscripciones le permiten conectarse a un solo servicio o a varios servicios a la vez.

### <a name="multi-service-subscription"></a>Suscripción a varios servicios

>[!WARNING]
> En este momento, estos servicios **no** admiten las claves de varios servicios: QnA Maker, servicios de Voz, Custom Vision y Anomaly Detector.

Una suscripción a varios servicios para Azure Cognitive Services le permite usar una suscripción única y el recurso de Azure para la mayoría de los servicios Azure Cognitive Services y, además, consolida la facturación de los servicios que usa. Consulte [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) para más información.

### <a name="single-service-subscription"></a>Suscripción a un servicio único

Una suscripción a un servicio único, como Computer Vision o los servicios de Voz. Una suscripción a un servicio único está restringida a ese recurso. 
