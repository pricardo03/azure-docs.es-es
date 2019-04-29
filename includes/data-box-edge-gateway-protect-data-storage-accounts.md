---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 653c175a559f5c0b7dc551b396e91276332df20a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754318"
---
El dispositivo está asociado con una cuenta de almacenamiento que se utiliza como un destino para los datos en Azure. Acceso a la cuenta de almacenamiento se controla mediante la suscripción y el almacenamiento de 512 bits dos asociadas con esa cuenta de almacenamiento de claves de acceso.

Una de las claves se usa para la autenticación cuando el dispositivo de borde del cuadro de datos tiene acceso a la cuenta de almacenamiento. El resto de las claves se mantiene en reserva, para que pueda girar las claves periódicamente.

Por motivos de seguridad, muchos centros de datos requieren la rotación de claves. Se recomienda seguir estos procedimientos recomendados para la rotación de claves:

- La clave de la cuenta de almacenamiento es similar a la contraseña raíz de la cuenta de almacenamiento. Proteja cuidadosamente la clave de cuenta. No distribuya la contraseña a otros usuarios, duro codificarlo o guardarlo en cualquier lugar en texto sin formato que sea accesible a otros usuarios.
- [Volver a generar la clave de cuenta](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) a través de Azure portal si cree podría estar en peligro.
- El Administrador de Azure periódicamente debe cambiar o volver a generar la clave principal o secundaria mediante la sección de almacenamiento de Azure portal para acceder directamente a la cuenta de almacenamiento.