---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: c2d3119283b5b75eeca6b0e0737f62eee00e81c6
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72237455"
---
Cuando se le concede acceso para usar Form Recognizer, recibe un correo electrónico de bienvenida con varios vínculos y recursos. Use el vínculo de "Azure Portal" de dicho mensaje para abrir Azure Portal y crear un recurso de Form Recognizer. En el panel **Crear**, proporcione la siguiente información:

|    |    |
|--|--|
| **Nombre** | Un nombre descriptivo para su recurso. Se recomienda usar un nombre descriptivo, como *MyNameFormRecognizer*. |
| **Suscripción** | Seleccione la suscripción de Azure a la que se le ha concedido acceso. |
| **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
| **Plan de tarifa** | El costo del recurso depende el plan de tarifa elegido y del uso. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) que contendrá su recurso. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

> [!IMPORTANT]
> Normalmente, al crear un recurso de Cognitive Services en Azure Portal, tiene la opción de crear una clave de suscripción para varios servicios (que se usa en varias instancias de Cognitive Services) o una clave de suscripción para un solo servicio (que se usa solo con una instancia específica de Cognitive Services). Sin embargo, dado que Form Recognizer se encuentra en versión preliminar, no se incluye en la suscripción para varios servicios, y no puede crear la suscripción para un solo servicio a menos que use el vínculo proporcionado en el correo electrónico de bienvenida.

Cuando el recurso de Form Recognizer termine la implementación, búsquelo y selecciónelo en la lista **Todos los recursos** del portal. A continuación, seleccione la pestaña **Claves** para ver las claves de la suscripción. Cualquiera de las claves dará a la aplicación acceso al recurso. Copie el valor de **CLAVE 1**.