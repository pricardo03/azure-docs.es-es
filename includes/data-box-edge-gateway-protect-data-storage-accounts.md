---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 23587e617c62cfe4aa24256330c5f7673dd1c674
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684487"
---
El dispositivo está asociado con una cuenta de almacenamiento que se utiliza como un destino para los datos en Azure. Acceso a la cuenta de almacenamiento se controla mediante la suscripción y el almacenamiento de 512 bits dos asociadas con esa cuenta de almacenamiento de claves de acceso.

Una de las claves se usa para la autenticación cuando el dispositivo de borde del cuadro de datos tiene acceso a la cuenta de almacenamiento. La otra clave se mantiene en reserva, lo que le permite rotar periódicamente las claves.

Por motivos de seguridad, muchos centros de datos requieren la rotación de claves. Se recomienda seguir estos procedimientos recomendados para la rotación de claves:

- La clave de la cuenta de almacenamiento es similar a la contraseña raíz de la cuenta de almacenamiento. Proteja cuidadosamente la clave de cuenta. No distribuya la contraseña a otros usuarios, disco duro codificarlo o guardarlo en cualquier lugar en texto sin formato que sea accesible a otros usuarios.
- [Volver a generar la clave de cuenta](../articles/storage/common/storage-account-manage.md#regenerate-access-keys) mediante Azure portal si cree puede verse comprometida.
- De forma regular, el administrador de Azure debe cambiar o volver a generar la clave principal o secundaria mediante la sección Storage de Azure Portal para acceder directamente a la cuenta de almacenamiento.