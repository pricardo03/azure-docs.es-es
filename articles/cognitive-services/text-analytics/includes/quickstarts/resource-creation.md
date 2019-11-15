---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/28/2019
ms.author: aahi
ms.openlocfilehash: 2fe3104d61b5fe2fbf9624ed2fd4fdb2de5686a2
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73750128"
---
Obtenga una clave y un punto de conexión para autenticar las aplicaciones. Cree un recurso para Text Analytics mediante [Azure Portal](../../../cognitive-services-apis-create-account.md) o la [CLI de Azure](../../../cognitive-services-apis-create-account-cli.md) en la máquina local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services/#decision) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver este recurso en [Azure Portal](https://portal.azure.com/).

Después de obtener una clave y un punto de conexión del recurso o la suscripción de evaluación, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication). Una llamada `TEXT_ANALYTICS_SUBSCRIPTION_KEY` para la clave, y otra denominada `TEXT_ANALYTICS_ENDPOINT` para el punto de conexión.