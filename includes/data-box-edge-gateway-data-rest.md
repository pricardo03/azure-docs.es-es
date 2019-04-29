---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8a6ceee0f7db4b7b368dce1a28c8edc0b2b11180
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754328"
---
Para los datos en reposo:

- Cifrado de BitLocker XTS-AES de 256 bits se usa para proteger los datos locales.
- Acceso a los datos almacenados en recursos compartidos está restringido.

    - Los clientes SMB que tienen acceso a datos del recurso compartido, necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen cuando se crea el recurso compartido.
    - Las direcciones IP de los clientes NFS que tienen acceso a un recurso compartido deben agregarse cuando se crea el recurso compartido.