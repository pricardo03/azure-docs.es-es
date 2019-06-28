---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8a6ceee0f7db4b7b368dce1a28c8edc0b2b11180
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186957"
---
Para los datos en reposo:

- El cifrado de BitLocker XTS-AES de 256 bits se usa para proteger los datos locales.
- El acceso a los datos almacenados en recursos compartidos está restringido.

    - Los clientes SMB que tienen acceso a datos del recurso compartido necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen cuando se crea el recurso compartido.
    - Las direcciones IP de los clientes NFS con acceso a un recurso compartido deben agregarse cuando se crea el recurso compartido.