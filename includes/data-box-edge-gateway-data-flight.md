---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: bea14544949f90290bf3878295b49f1e08be9eea
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684481"
---
Los datos de vuelos:

- Para los datos que pasan entre el dispositivo y Azure, se usa el estándar de TLS 1.2. No hay ninguna reserva para TLS 1.1 y versiones anteriores. Si no se admite TLS 1.2, se bloqueará la comunicación del agente. También es necesario para las operaciones de administración de SDK y portal de la TLS 1.2.
- Cuando los clientes acceder al dispositivo a través de la interfaz de usuario web local en un explorador, estándar de TLS 1.2 se utiliza como el protocolo seguro de forma predeterminada.

    - El procedimiento recomendado consiste en configurar el explorador para usar TLS 1.2.
    - Si el explorador no es compatible con TLS 1.2, puede usar TLS 1.1 o TLS 1.0.
- Para proteger los datos al copiar desde los servidores de datos, se recomienda el uso de SMB 3.0 con cifrado.
