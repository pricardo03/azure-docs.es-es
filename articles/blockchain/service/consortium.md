---
title: Servicio de Azure Blockchain Consortium
description: ''
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e745a4ee4789ef46a61b5cb0bbf806c41ef631ec
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027921"
---
# <a name="azure-blockchain-service-consortium"></a>Servicio de Azure Blockchain Consortium

Mediante el servicio de la cadena de bloques de Azure, puede crear privada consortium blockchain redes donde cada cadena de bloques red puede estar limitado a participantes específicos en la red. Sólo los participantes en la red de privada consortium blockchain pueden ver e interactuar con la cadena de bloques. Redes de consorcio en Azure Blockchain Service pueden contener dos tipos de miembro participante roles:

* **Administrador** -con privilegios a los participantes que pueden realizar acciones de administración de consorcio y pueden participar en transacciones de la cadena de bloques.

* **Usuario** -participantes que no se pueden realizar ninguna acción de administración de consorcio pero pueden participar en transacciones de la cadena de bloques.

Redes de consorcio pueden ser una combinación de roles participantes y pueden tener un número arbitrario de cada tipo de rol. Debe haber al menos un administrador.

El siguiente diagrama muestra una red de consorcio con varios participantes:

![Diagrama de red de consorcio privada](./media/consortium/network-diagram.png)

Con la administración de consorcio en Azure Blockchain Service, puede administrar a los participantes en la red de consorcio. Administración del consorcio se basa en el modelo de consenso de la red. En la versión preliminar actual, Azure Blockchain Service proporciona un modelo de consenso centralizada para la administración del consorcio. Cualquier participante con privilegios con un rol de administrador puede realizar acciones como agregar o quitar a participantes de una red de administración que consortium.

## <a name="roles"></a>Roles

Los participantes en un consorcio pueden ser personas u organizaciones y se pueden asignar un rol de usuario o un rol de administrador. En la tabla siguiente se enumera las diferencias de alto nivel entre los dos roles:

| . | Rol de usuario | Rol de administrador
|--------|:----:|:------------:|
| Crear nuevo miembro | Sí | Sí |
| Invitar a nuevos miembros | Sin  | Sí |
| Establecer o cambiar la función de miembro participante | Sin  | Sí |
| Cambiar nombre de miembro para mostrar | Solo para el propio miembro | Solo para el propio miembro |
| Eliminación de miembros | Solo para el propio miembro | Sí |
| Participar en transacciones de la cadena de bloques | Sí | Sí |

### <a name="user-role"></a>Rol de usuario

Los usuarios van a ser participantes de consorcio con ninguna capacidad de administrador. No podrán participar en la administración de miembros relacionados con el consorcio. Los usuarios pueden cambiar su nombre para mostrar miembro y pueden quitarse de un consorcio.

### <a name="administrator"></a>Administrador

Un administrador puede administrar a los miembros dentro del consorcio. Un administrador puede invitar a miembros, quitar a los miembros o actualizar los roles de los miembros en el consorcio.
Siempre debe haber al menos un administrador dentro un consorcio. El último administrador debe especificar a otro participante como un rol de administrador antes de abandonar un consorcio.

## <a name="managing-members"></a>Administración de miembros

Solo los administradores pueden invitar a otros participantes en el consorcio. Los administradores invitar a los participantes con su identificador de suscripción de Azure.

Una vez que el invitado, los participantes pueden unir el consorcio de la cadena de bloques mediante la implementación de un nuevo miembro en el servicio de Azure Blockchain. Para ver y unir el consorcio invitado, debe especificar el mismo identificador de suscripción de Azure usado el Administrador de red en la invitación.

Los administradores pueden quitar a cualquier participante del consortium, incluidos otros administradores. Los miembros solo pueden borrarse de un consorcio.

## <a name="consortium-management-smart-contract"></a>Contrato de consorcio administración inteligente

Administración de consorcio en Azure Blockchain Service se realiza a través de los contratos inteligentes de administración consortium. Los contratos inteligentes se implementan automáticamente en los nodos al implementar un nuevo miembro de la cadena de bloques.

La dirección del contrato inteligente de administración de consorcio de raíz puede verse en el portal de Azure. El **RootContract dirección** está en la sección de información general del miembro de la cadena de bloques.

![Dirección RootContract](./media/consortium/rootcontract-address.png)

Puede interactuar con el contrato inteligente de administración de consorcio con la administración de consorcio [módulo de PowerShell](manage-consortium-powershell.md), Azure portal, o directamente a través del contrato inteligente mediante el servicio de Azure Blockchain genera Ethereum cuenta.

## <a name="ethereum-account"></a>Ethereum cuenta

Cuando se crea un miembro, se crea una clave de cuenta Ethereum. Azure Blockchain Service usa la clave para crear las transacciones relacionadas con la administración del consorcio. La clave de cuenta Ethereum se administra mediante el servicio de Azure Blockchain automáticamente.

La cuenta de miembro puede verse en el portal de Azure. La cuenta de miembro está en la sección de información general del miembro de la cadena de bloques.

![Cuenta de miembro](./media/consortium/member-account.png)

Puede restablecer su cuenta de Ethereum haciendo clic en su cuenta de miembro y escribir una contraseña nueva. La dirección de la cuenta Ethereum y la contraseña se restablecerá.  

## <a name="next-steps"></a>Pasos siguientes

[Cómo administrar a miembros en Azure Blockchain Service mediante PowerShell](manage-consortium-powershell.md)
