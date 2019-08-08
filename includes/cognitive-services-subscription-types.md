---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 674dd30ff3e493ec4c4036f032f82624a6ca5749
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334231"
---
## <a name="azure-cognitive-service-resource-types"></a>Tipos de recursos de Azure Cognitive Services

> [!NOTE]
> Los propietarios de las suscripciones pueden deshabilitar las opciones de creación de recursos de Cognitive Services para los grupos de recursos y las suscripciones; para ello, deben aplicar la [directiva de Azure](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), asignar una definición de directiva "Tipos de recursos no permitidos" y especificar **Microsoft.CognitiveServices/accounts** como el tipo de recurso de destino.

Puede acceder a Azure Cognitive Services mediante dos recursos distintos: Un recurso de varios servicios o uno de un único servicio. Estas suscripciones le permiten conectarse a un solo servicio o a varios servicios a la vez.

### <a name="multi-service-resource"></a>Recurso de varios servicios

>[!WARNING]
> En este momento, estos servicios **no** admiten las claves de varios servicios: QnA Maker, servicios de Voz, Custom Vision y Anomaly Detector.

Suscripción a un recurso de Cognitive Services de varios servicios:
* Permite usar un único recurso de Azure para la mayoría de los servicios de Azure Cognitive Services.
* Consolida la facturación de los servicios que usa. Consulte [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) para más información.

### <a name="single-service-resource"></a>Recurso de un único servicio

Los recursos de un único servicio (como Computer Vision o Servicios de voz) están restringidos a su servicio especificado.