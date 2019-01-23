---
title: Eliminación de un directorio de inquilino en Azure Active Directory | Microsoft Docs
description: Explica cómo preparar un directorio de inquilino de Azure AD para eliminarlo
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/07/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 39682ac04b914809aec36f46889feb5c4b59af51
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243889"
---
# <a name="delete-an-azure-active-directory-tenant"></a>Eliminación de un inquilino de Azure Active Directory

Cuando se elimina un inquilino, también se eliminan todos los recursos que contiene. Debe preparar el inquilino, reduciendo sus recursos asociados antes de eliminarlo. Solo un administrador global de Azure Active Directory (Azure AD) puede eliminar a un inquilino de Azure AD desde el portal.

## <a name="prepare-the-tenant-for-deletion"></a>Preparación del inquilino para eliminarlo

No puede eliminar un inquilino de Azure AD hasta que pase varias comprobaciones. Estas comprobaciones reducen el riesgo de que la eliminación de un inquilino afecte desfavorablemente a los usuarios, por ejemplo, en la capacidad de iniciar sesión en Office 365 o de tener acceso a los recursos de Azure. Por ejemplo, si se elimina de forma involuntaria un inquilino con una suscripción, los usuarios no pueden acceder a los recursos de Azure de esa suscripción. A continuación se explican las condiciones que se comprueban:

* No puede haber ningún usuario en el inquilino, excepto un administrador global que eliminará al inquilino. Se deben eliminar los demás usuarios para poder eliminar el inquilino. Si los usuarios se sincronizan localmente, se deberá desactivar la sincronización y eliminar los usuarios del inquilino en la nube mediante Azure Portal o cmdlets de Azure PowerShell. 
* No puede haber aplicaciones en el inquilino. Las aplicaciones se deben quitar para poder eliminar el inquilino.
* No puede haber proveedores de Multi-Factor Authentication vinculados al inquilino.
* No puede haber suscripciones a ningún servicio de Microsoft Online Services, como Microsoft Azure, Office 365 o Azure AD Premium, asociadas al inquilino. Por ejemplo, si se ha creado un inquilino predeterminado en Azure en su nombre, no puede eliminarlo si la suscripción a Azure aún se basa en él para la autenticación. De igual forma, no puede eliminar un inquilino si otro usuario le ha asociado una suscripción. 

## <a name="delete-an-azure-ad-tenant"></a>Eliminación de un inquilino de Azure AD

1. Inicie sesión en el [Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador global del inquilino.

2. Seleccione **Azure Active Directory**.

3. Cambie al inquilino que desea eliminar.
  
  ![botón Eliminar directorio](./media/directory-delete-howto/delete-directory-command.png)

4. Seleccione **Eliminar directorio**.
  
  ![botón Eliminar directorio](./media/directory-delete-howto/delete-directory-list.png)

5. Si el inquilino no supera una o varias comprobaciones, se le proporcionará un vínculo para obtener más información sobre cómo pasar. Una vez que pase todas las comprobaciones, seleccione **Eliminar** para completar el proceso.

## <a name="i-have-an-expired-subscription-but-i-cant-delete-the-tenant"></a>Tengo una suscripción caducada, pero no puedo eliminar el inquilino

Al configurar el inquilino de Azure Active Directory, es posible que también activara suscripciones basadas en licencias para su organización, como Azure Active Directory Premium P2, Office 365 Business Premium o Enterprise Mobility + Security E5. Estas suscripciones bloquean la eliminación de directorio hasta que se eliminan por completo, para evitar la pérdida accidental de datos. Las suscripciones deben estar en estado **Deprovisioned** (Desaprovisionado) para permitir la eliminación del inquilino. Una suscripción en estado **Expirado** o **Cancelado** se pasa al estado **Deshabilitado** y la fase final es el estado **Deprovisioned** (Desaprovisionado). 

Para saber qué se puede esperar cuando una suscripción de Office 365 de prueba expira (exceptuando el caso de licencias por volumen, el contrato para empresas o de CSP o asociados de pago), consulte la tabla siguiente. Para obtener más información sobre la retención de datos de Office 365 y el ciclo de vida de la suscripción, consulte [¿Qué pasa con mis datos y mi acceso cuando termina mi suscripción de Office 365 para empresas?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Estado de la suscripción | Datos | Acceso a datos
----- | ----- | -----
Activo (30 días para evaluación)  | Datos accesibles a todos    | <li>Los usuarios tienen acceso normal a los archivos o aplicaciones de Office 365<li>Los administradores tienen acceso normal al centro de administración y los recursos de Office 365 
Expirado (30 días)   | Datos accesibles a todos    | <li>Los usuarios tienen acceso normal a los archivos o aplicaciones de Office 365<li>Los administradores tienen acceso normal al centro de administración y los recursos de Office 365
Deshabilitado (30 días) | Datos accesibles solo para administradores  | <li>Los usuarios no pueden obtener acceso a archivos o aplicaciones de Office 365<li>Los administradores pueden acceder al centro de administración de Office 365, pero no pueden asignar licencias a los usuarios ni actualizarlos
Desaprovisionado (30 días tras la deshabilitación) | Los datos se eliminan (automáticamente si no hay otros servicios en uso) | <li>Los usuarios no pueden obtener acceso a archivos o aplicaciones de Office 365<li>Los administradores pueden tener acceso al centro de administración de Office 365 para adquirir y administrar otras suscripciones

Puede colocar una suscripción en un estado **Deprovisoned** (Desaprovisionado) para ser eliminados a los 3 días mediante el centro de administración de Microsoft Store para Empresas. Esta funcionalidad estará disponible próximamente en el centro de administración de Office 365.

1. Inicie sesión en el [centro de administración de Microsoft Store para Empresas](https://businessstore.microsoft.com/manage/) con una cuenta que sea de administrador global en el inquilino. Si está intentando eliminar el inquilino "Contoso" que tiene el dominio predeterminado inicial "contoso.onmicrosoft.com", inicie sesión con un UPN como admin@contoso.onmicrosoft.com.

2. Vaya a la pestaña **Administrar** y seleccione **Productos y servicios**, y, después, elija la suscripción que quiera cancelar y seleccione **Eliminar**.
  
  ![Eliminación del vínculo para eliminar la suscripción](./media/directory-delete-howto/delete-command.png)
  
3. Seleccione **Eliminar suscripción** para aceptar los términos y condiciones, y eliminar la suscripción. Todos los datos se eliminarán permanentemente a los tres días. Puede reactivar la suscripción durante los próximos tres días, si cambia de opinión.
  
  ![términos y condiciones](./media/directory-delete-howto/delete-terms.png)

4. Ahora que ha cambiado el estado de la suscripción, esta se marca para eliminarla. La suscripción entra en el estado **Desaprovisionado** 72 horas más tarde.

5. Una vez que haya eliminado una suscripción en el inquilino y hayan transcurrido 72 horas, puede iniciar sesión de nuevo en el centro de administración de Azure AD y no debería requerirse ninguna acción ni haber ninguna suscripción que bloquee la eliminación de inquilinos. Debe ser capaz de eliminar correctamente el inquilino de Azure AD.
  
  ![pasar la comprobación de suscripción en la pantalla de eliminación](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="next-steps"></a>Pasos siguientes
[Documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
