---
title: Solución de problemas de Azure Data Share
description: Obtenga información sobre cómo solucionar problemas con invitaciones y errores al crear o recibir recursos compartidos de datos con Azure Data Share.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.openlocfilehash: 6ad612d56b25da9e092070198e321e7fca8ad96b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490566"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Solución de problemas habituales de Azure Data Share 

En este artículo se muestra cómo solucionar problemas habituales de Azure Data Share. 

## <a name="azure-data-share-invitations"></a>Invitaciones de Azure Data Share 

En algunos casos, si un usuario nuevo hace clic en **Accept Invitation** (Aceptar invitación) en la invitación de correo electrónico que se envió, pueden aparecer una lista vacía de invitaciones. 

![No hay invitaciones](media/no-invites.png)

El error anterior es un problema conocido del servicio y se está solucionando. Como alternativa, siga los pasos que se indican a continuación. 

1. En Azure Portal, vaya a **Suscripciones**
1. Seleccione la suscripción que va a usar para Azure Data Share
1. Haga clic en **Proveedores de recursos**
1. Busque Microsoft.DataShare
1. Haga clic en **Registrar**.

Es preciso que tenga el [rol RBAC Colaborador de Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) para completar estos pasos. 

Si todavía no puede ver una invitación de recurso compartido de datos, póngase en contacto con el proveedor de datos y asegúrese de que ha enviado la invitación a su dirección de correo electrónico de inicio de sesión de Azure, *no* a su alias de correo electrónico. 

> [!IMPORTANT]
> Si ya ha aceptado una invitación de Azure Data Share y ha salido del servicio antes de configurar el almacenamiento, siga las instrucciones que encontrará en la guía paso a paso para [configurar una asignación de conjuntos de datos](how-to-configure-mapping.md) para aprender a finalizar la configuración del recurso compartido de datos recibidos y a empezar a recibir datos. 

## <a name="error-when-creating-or-receiving-a-new-data-share"></a>Error al crear o recibir un nuevo recurso compartido de datos

"Error: Operation returned an invalid status code "BadRequest"" (la operación devolvió el código de estado no válido "BadRequest")

"Error: AuthorizationFailed"

"Error: role assignment to storage account" (Error: asignación de rol a la cuenta de almacenamiento)

![Error de privilegios](media/error-write-privilege.png)

Si aparece cualquiera de los errores anteriores tanto al crear como al recibir un recurso compartido de datos, se debe a que no hay permisos suficientes en la cuenta de almacenamiento. El permiso requerido es *Microsoft.Authorization/role assignments/write*, que existe en el rol de propietario de almacenamiento o se puede asignar a un rol personalizado. Aunque haya creado la cuenta de Azure Storage, ello NO le convierte automáticamente en propietario de dicha cuenta. Siga estos pasos para concederse el rol de propietario de la cuenta de almacenamiento. También puede crear un rol personalizado con este permiso y, posteriormente, agregarse a él.  

1. Vaya a la cuenta de Azure Storage en Azure Portal
1. Seleccione **Access Control (IAM)**
1. Haga clic en **Agregar**.
1. Agréguese como propietario.

## <a name="troubleshooting-sql-based-sharing"></a>Solución de problemas de uso compartido basado en SQL

"Error: x conjuntos de datos no se han agregado porque no tiene los permisos necesarios para compartir."

Si recibe este error al agregar un conjunto de datos desde un origen basado en SQL, puede deberse a que no ha creado un usuario para el MSI de Azure Data Share en SQL Server.  Para resolver este problema, ejecute el siguiente script:

```sql
    create user <share_acct_name> from external provider;     
    exec sp_addrolemember db_owner, <share_acct_name>; 
```      
Tenga en cuenta que *<share_acc_name>* es el nombre de la cuenta de Data Share. Si aún no ha creado una cuenta de Data Share, puede volver a este requisito previo en otro momento.         

Asegúrese de que ha seguido todos los requisitos previos que se enumeran en el tutorial [Uso compartido de datos](share-your-data.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).

