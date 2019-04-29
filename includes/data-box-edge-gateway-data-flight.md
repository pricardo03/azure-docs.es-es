---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754335"
---
Para los datos en tránsito:

- Estándar de TLS 1.2 se usa para los datos que se transmite entre el dispositivo y Azure. No hay ninguna reserva para TLS 1.1 y versiones anteriores. Si no se admite TLS 1.2, se bloqueará la comunicación del agente. También es necesario para la administración de SDK y portal de TLS 1.2.
- Cuando los clientes acceder al dispositivo a través de la IU de un explorador web local, estándar de TLS 1.2 se utiliza como el protocolo seguro de forma predeterminada.

    - El procedimiento recomendado consiste en configurar el explorador para usar TLS 1.2.
    - Si el explorador no es compatible con TLS 1.2, puede usar TLS 1.1 o TLS 1.0.
- Se recomienda usar SMB 3.0 con cifrado para proteger los datos al copiar desde los servidores de datos.
