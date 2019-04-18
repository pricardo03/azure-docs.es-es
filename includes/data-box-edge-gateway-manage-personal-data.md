---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: fa8340ec6f555258f159e14133305b847e2e3cc3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684494"
---
- **Detalles de pedidos**: una vez creado el pedido, la dirección de envío, el correo electrónico y la información de contacto de los usuarios se almacenan en Azure Portal. Entre la información guardada se incluyen los siguientes datos:
  - Nombre de contacto
  - Número de teléfono
  - Email
  - Dirección
  - City
  - Código postal
  - Estado
  - País/región/provincia
  - Número de seguimiento del envío

    Los detalles del pedido se cifran y almacenan en el servicio. El servicio conserva la información hasta que se elimine el recurso o el orden explícitamente. Además, se bloquea la eliminación de recursos y el orden correspondiente desde el momento en que el dispositivo se envía hasta que el dispositivo vuelve a Microsoft.

- **Dirección de envío** – después de que se realiza el pedido, el servicio de Data Box proporciona la dirección de envío a las compañías aéreas de terceros como SAI (UPS).

- **Compartir usuarios** -los usuarios en el dispositivo también pueden tener acceso a los datos que residen en los recursos compartidos. Se muestra y se puede ver una lista de los usuarios que pueden acceder a los datos del recurso compartido. Esta lista también se elimina cuando se eliminan los recursos compartidos.

