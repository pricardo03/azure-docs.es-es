---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 7ac98f608cb1ca5bab590714623bc78036d46b81
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74093001"
---
## <a name="configure-the-cloud-anchor-session"></a>Configuración de la sesión de delimitador de la nube

A continuación, va a configurar la sesión de delimitador de nube. En la primera línea, se establece el proveedor del sensor en la sesión. A partir de ahora, todos los delimitadores que se creen en la sesión se asociarán a un conjunto de lecturas del sensor. A continuación, se instancian los criterios de búsqueda de dispositivos cercanos y se inicializan para que coincidan con los requisitos de la aplicación. Por último, se indica a la sesión que use los datos del sensor al localizar los delimitadores mediante la creación de un monitor a partir de nuestros criterios de dispositivos cercanos.