---
title: Roles y requisitos de Azure Data Share (versión preliminar)
description: Roles y requisitos de Azure Data Share (versión preliminar)
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: a5d70b9aa611b4f939cb46b5d25655edd818cb35
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807536"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Roles y requisitos de Azure Data Share (versión preliminar)

En este artículo se describen los roles necesarios para compartir, aceptar y recibir datos mediante Azure Data Share (versión preliminar). 

## <a name="roles-and-requirements"></a>Roles y requisitos

Para compartir o recibir datos mediante Azure Data Share, la cuenta de usuario que utiliza para iniciar sesión en Azure debe poder conceder permisos a Data Share en la cuenta de Storage desde la que se están compartiendo los datos o en la que se recibirán estos. Normalmente, este permiso existe en el rol **propietario** o en un rol personalizado con el permiso Microsoft.Authorization/role assignments/write asignado. 

Para compartir o recibir datos desde una cuenta de Azure Storage o en ella, debe ser propietario de la cuenta de almacenamiento. Aunque haya creado la cuenta de Azure Storage, ello no le convierte automáticamente en propietario de dicha cuenta. Para agregarse a sí mismo al rol de propietario de la cuenta de Azure Storage, siga estos pasos.

1. Vaya a la cuenta de Azure Storage en Azure Portal
1. Seleccione **Access Control (IAM)**
1. Haga clic en **Agregar**.
1. Agréguese como propietario.

En Azure Portal, seleccione su nombre de usuario en la esquina superior derecha y, después, seleccione **Permisos** para ver los permisos que tiene en la suscripción. Si tiene acceso a varias suscripciones, elija la correspondiente. 

## <a name="resource-provider-registration"></a>Registro del proveedor de recursos 

Al aceptar una invitación de Azure Data Share, deberá registrar manualmente el proveedor de recursos Microsoft.DataShare en la suscripción. Siga estos pasos para registrar el proveedor de recursos Microsoft.DataShare en la suscripción de Azure. 

1. En Azure Portal, vaya a **Suscripciones**
1. Seleccione la suscripción que va a usar para Azure Data Share
1. Haga clic en **Proveedores de recursos**
1. Busque Microsoft.DataShare
1. Haga clic en **Registrar**.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de los roles en Azure - [Descripción de definiciones de roles](../role-based-access-control/role-definitions.md)

