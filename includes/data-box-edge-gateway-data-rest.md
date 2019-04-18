---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: d8570c0be6b64d4e289575ce3f3f1721c4a65074
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684502"
---
Para lo datos en reposo:

- Para lo datos en reposo, cifrado de BitLocker XTS AES-256 se utiliza para proteger los datos locales.
- Para los datos que residen en recursos compartidos, se restringe el acceso a los recursos compartidos.

    - Para los clientes SMB que tienen acceso a los datos del recurso compartido, necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen en el momento de creación del recurso compartido.
    - Para los clientes NFS que tienen acceso a los recursos compartidos, las direcciones IP de los clientes deben agregarse en el momento de creación del recurso compartido.