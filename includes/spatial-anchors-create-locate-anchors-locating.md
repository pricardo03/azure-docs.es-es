---
ms.openlocfilehash: 52dfbfca5f79a7f92848ea39eddc00aa10f05ff1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186031"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Búsqueda de un anclaje espacial de la nube

Una de las principales razones para usar la biblioteca Azure Spatial Anchors es poder localizar un anclaje espacial en la nube previamente cargado. Para buscar anclajes espaciales en la nube, necesitará conocer sus identificadores. Los identificadores de anclaje se pueden almacenar en el servicio back-end de la aplicación y son accesibles para todos los dispositivos que se puedan autenticar correctamente. Si quiere ver un ejemplo, consulte [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Crear una instancia de un objeto `AnchorLocateCriteria`, establezca los identificadores que busca e invoque el método `CreateWatcher` en la sesión mediante su `AnchorLocateCriteria`.
