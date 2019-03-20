---
ms.openlocfilehash: 430302d8b1b2a01febbbe2f11057bb331ec80c28
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907813"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Busque un anclaje espacial en la nube

Para buscar delimitadores espacial de la nube, necesitará saber sus identificadores. Los identificadores de delimitador pueden ser accesible para todos los dispositivos que pueden autenticar correctamente a ella y almacenados en el servicio de back-end de la aplicación. Para obtener un ejemplo de esta, consulte [Tutorial: Comparta delimitadores espaciales entre dispositivos](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Crear una instancia de un objeto AnchorLocateCriteria, establezca los identificadores que está buscando e invocar el método CreateWatcher en la sesión proporcionando su AnchorLocateCriteria.
