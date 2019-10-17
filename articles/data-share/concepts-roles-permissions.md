---
title: Roles y requisitos de Azure Data Share (versión preliminar)
description: Obtenga información sobre los roles de control de acceso y los requisitos de los proveedores de datos y los consumidores de datos para compartir datos en la versión preliminar de Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: c0841f6386440776c6ea719f9932a53cada9d9c4
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166372"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Roles y requisitos de Azure Data Share (versión preliminar)

En este artículo se describen los roles necesarios para compartir, aceptar y recibir datos mediante Azure Data Share (versión preliminar). 

## <a name="roles-and-requirements"></a>Roles y requisitos

Azure Data Share utilizar identidades administradas para los servicios de Azure (conocidas anteriormente como MSI) para autenticarse en las cuentas de almacenamiento subyacentes con el fin de poder leer los datos que compartirá un proveedor de datos, así como recibir los datos compartidos como un consumidor de datos. Como resultado, no hay intercambio de credenciales entre un proveedor de datos y un consumidor de datos. 

Managed Service Identity debe tener acceso a las cuentas de almacenamiento subyacentes. El servicio Azure Data Share utiliza Managed Service Identity del recurso de Azure Data Share para leer y escribir datos. El usuario de Azure Data Share necesita la capacidad de crear una asignación de roles para Managed Service Identity en la cuenta de almacenamiento en la que están compartiendo los datos. El permiso para crear asignaciones de rol existe en el rol **propietario**, en el rol de administrador de acceso de usuario o en un rol personalizado con el permiso Microsoft.Authorization, de asignaciones de roles o de escritura asignado. 

Si no es propietario de la cuenta de almacenamiento en cuestión y no puede crear una asignación de roles para la identidad administrada del recurso de Azure Data Share, puede solicitar a un administrador de Azure que cree una asignación de roles en su nombre. 

A continuación se muestra un resumen de los roles asignados a la identidad administrada de los recursos de Data Share:

| |  |  |
|---|---|---|
|**Tipo de almacenamiento**|**Cuenta de almacenamiento de origen del proveedor de datos**|**Cuenta de almacenamiento de destino del consumidor de datos**|
|Azure Blob Storage| Lector de datos de blobs de almacenamiento | Colaborador de datos de blobs de almacenamiento
|Azure Data Lake Gen1 | Propietario | No compatible
|Azure Data Lake Gen2 | Lector de datos de blobs de almacenamiento | Colaborador de datos de blobs de almacenamiento
|
### <a name="data-providers"></a>Proveedores de datos 
Para agregar un conjunto de datos a una instancia de Azure Data Share, se debe agregar al rol de lector de datos de Storage Blob la identidad administrada de Data Share de los proveedores de datos. Esto lo hace automáticamente el servicio Azure Data Share si el usuario está agregando conjuntos de datos mediante Azure y es propietario de la cuenta de almacenamiento, o es miembro de un rol personalizado que tiene asignado el permiso Microsoft.Authorization, asignaciones de roles o de escritura. 

Como alternativa, el usuario puede hacer que un administrador de Azure agregue manualmente la identidad administrada de recursos del recurso de Data Share al rol de lector de datos de Storage Blob. La creación manual de esta asignación de roles por parte del administrador anulará la necesidad de ser propietario de la cuenta de almacenamiento o de tener una asignación de roles personalizada. Esto se aplica a los datos que se comparten desde Azure Storage o Azure Data Lake Gen2. 

Si comparte datos desde Azure Data Lake Gen1, se debe realizar la asignación de roles en el rol de propietario. 

Para crear una asignación de roles para la identidad administrada del recurso de Data Share, siga estos pasos:

1. Vaya a la cuenta de almacenamiento.
1. Seleccione **Access Control (IAM)** .
1. Seleccione **Agregar una asignación de roles**.
1. En la lista desplegable *Rol*, seleccione *Lector de datos de Storage Blob*.
1. En *Seleccionar*, escriba el nombre de la cuenta de Azure Data Share.
1. Haga clic en *Save*(Guardar).

### <a name="data-consumers"></a>Consumidores de datos
Para recibir datos, se debe agregar la identidad administrada del recurso de Data Share de los consumidores de datos al rol de colaborador de datos de blobs de almacenamiento. Este rol es necesario para permitir que el servicio Azure Data Share pueda escribir en la cuenta de almacenamiento. Esto lo hace automáticamente el servicio Azure Data Share si el usuario está agregando conjuntos de datos mediante Azure y es propietario de la cuenta de almacenamiento, o es miembro de un rol personalizado que tiene asignado el permiso Microsoft.Authorization, asignaciones de roles o de escritura. 

Como alternativa, el usuario puede hacer que un administrador de Azure agregue manualmente la identidad administrada de recursos de Data Share al rol de colaborador de datos de blobs de almacenamiento. La creación manual de esta asignación de roles por parte del administrador anulará la necesidad de ser propietario de la cuenta de almacenamiento o de tener una asignación de roles personalizada. Tenga en cuenta que esto se aplica a los datos que se comparten en Azure Storage o Azure Data Lake Gen2. No se admite la recepción de datos a Azure Data Lake Gen1. 

Para crear una asignación de roles manualmente para la identidad administrada del recurso de Data Share, siga estos pasos:

1. Vaya a la cuenta de almacenamiento.
1. Seleccione **Access Control (IAM)** .
1. Seleccione **Agregar una asignación de roles**.
1. En *Rol*, seleccione *Colaborador de datos de Storage Blob*. 
1. En *Seleccionar*, escriba el nombre de la cuenta de Azure Data Share.
1. Haga clic en *Save*(Guardar).

Si va a compartir datos con nuestras API REST, tendrá que crear estas asignaciones de roles manualmente mediante la adición de la cuenta del recurso compartido de datos en a los roles adecuados. 

Para más información sobre cómo agregar una asignación de roles, consulte [esta documentación](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) en la que se describe cómo agregar una asignación de roles a un recurso de Azure. 

## <a name="resource-provider-registration"></a>Registro del proveedor de recursos 

Al aceptar una invitación de Azure Data Share, deberá registrar manualmente el proveedor de recursos Microsoft.DataShare en la suscripción. Siga estos pasos para registrar el proveedor de recursos Microsoft.DataShare en la suscripción de Azure. 

1. En Azure Portal, vaya a **Suscripciones**.
1. Seleccione la suscripción que va a usar para Azure Data Share.
1. Haga clic en **Proveedores de recursos**
1. Busque Microsoft.DataShare.
1. Haga clic en **Registrar**.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de los roles en Azure - [Descripción de definiciones de roles](../role-based-access-control/role-definitions.md)

