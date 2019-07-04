---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: d322aa27c7c9dcf4ad365ec22f8074aeacc85d3b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502845"
---
Cuando se le conceda acceso para usar Form Recognizer, recibirá un correo electrónico de bienvenida con varios vínculos y recursos. Use el vínculo de "Azure Portal" de dicho mensaje para abrir Azure Portal y crear un recurso de Form Recognizer. En el panel **Crear**, proporcione la siguiente información:

|    |    |
|--|--|
| **Nombre** | Un nombre descriptivo para su recurso. Se recomienda usar un nombre descriptivo, como *MyNameFormRecognizer*. |
| **Suscripción** | Seleccione la suscripción de Azure a la que se le ha concedido acceso. |
| **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
| **Plan de tarifa** | El costo del recurso depende el plan de tarifa elegido y del uso. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

> [!IMPORTANT]
> Normalmente, al crear un recurso de Cognitive Services en Azure Portal, tiene la opción de crear una clave de suscripción para varios servicios (que se usa en varias instancias de Cognitive Services) o una clave de suscripción para un solo servicio (que se usa solo con una instancia específica de Cognitive Services). Sin embargo, dado que Form Recognizer se encuentra en versión preliminar, no se incluye en la suscripción para varios servicios, y no puede crear la suscripción para un solo servicio a menos que use el vínculo proporcionado en el correo electrónico de bienvenida.

Cuando el recurso de Form Recognizer termine la implementación, búsquelo y selecciónelo en la lista **Todos los recursos** del portal. A continuación, seleccione la pestaña **Claves** para ver las claves de la suscripción. Cualquiera de las claves dará a la aplicación acceso al recurso. Copie el valor de **CLAVE 1**. La usará en la sección siguiente.