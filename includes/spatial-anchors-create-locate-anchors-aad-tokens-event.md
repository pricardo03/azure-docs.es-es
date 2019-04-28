---
ms.openlocfilehash: 7219a457a2631f9ff6beee06eff34bce0ff5a23f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232633"
---
Al igual que con tokens de acceso, si no se establece un token de Azure AD, debe controlar el evento TokenRequired o implementar el método tokenRequired en el protocolo de delegado.

Puede controlar el evento de forma sincrónica mediante el establecimiento de la propiedad en los argumentos del evento.
