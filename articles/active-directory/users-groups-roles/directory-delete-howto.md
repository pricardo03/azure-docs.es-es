---
title: Eliminar un directorio de Azure AD - Azure Active Directory | Microsoft Docs
description: Se explica cómo preparar un directorio de Azure AD para su eliminación, incluidos los directorios de autoservicio
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91ac6b4530414850c52605bac8cb701aa2b877d4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473196"
---
# <a name="delete-a-directory-in-azure-active-directory"></a>Eliminar un directorio en Azure Active Directory

Cuando se elimina un directorio de Azure AD, también se eliminan todos los recursos que se encuentran en el directorio. Prepare su organización, ya que minimiza sus recursos asociados antes de eliminar. Solo un administrador global de Azure Active Directory (Azure AD) puede eliminar un directorio de Azure AD desde el portal.

## <a name="prepare-the-directory"></a>Preparar el directorio

No se puede eliminar un directorio de Azure AD hasta que pase varias comprobaciones. Estas comprobaciones reducen el riesgo de que al eliminar un directorio de Azure AD negativamente afecta al acceso de usuario, como la capacidad de iniciar sesión en Office 365 o acceder a recursos en Azure. Por ejemplo, si se elimina el directorio asociado con una suscripción de forma involuntaria, a continuación, los usuarios no pueden acceder a los recursos de Azure para esa suscripción. Se comprueban las condiciones siguientes:

* No puede haber ningún usuario en el directorio excepto un administrador global que eliminará el directorio. Se deben eliminar los otros usuarios antes de poder eliminar el directorio. Si los usuarios se sincronizan de forma local, a continuación, en primer lugar se debe desactivar sincronización y los usuarios deben eliminarse en el directorio en la nube mediante Azure portal o los cmdlets de PowerShell de Azure.
* No puede haber aplicaciones en el directorio. Las aplicaciones deben quitarse antes de poder eliminar el directorio.
* No puede haber ningún proveedor de autenticación multifactor vinculado en el directorio.
* No puede haber suscripciones a ningún servicio de Microsoft Online Services, como Microsoft Azure, Office 365 o Azure AD Premium, asociadas al directorio. Por ejemplo, si se ha creado un directorio predeterminado en Azure en su nombre, no puede eliminar este directorio si la suscripción a Azure aún se basa en este directorio para la autenticación. De igual forma, no puede eliminar un directorio si otro usuario le ha asociado una suscripción.

## <a name="delete-the-directory"></a>Eliminar el directorio

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que es el administrador Global para su organización.

2. Seleccione **Azure Active Directory**.

3. Cambie al directorio que desea eliminar.
  
   ![Confirme la organización antes de eliminar](./media/directory-delete-howto/delete-directory-command.png)

4. Seleccione **Eliminar directorio**.
  
   ![Seleccione el comando para eliminar la organización](./media/directory-delete-howto/delete-directory-list.png)

5. Si el directorio no supera una o más comprobaciones, le proporcionará un vínculo para obtener más información sobre cómo pasar. Una vez que pase todas las comprobaciones, seleccione **Eliminar** para completar el proceso.

## <a name="if-you-cant-delete-the-directory"></a>Si no se puede eliminar el directorio

Cuando se configura el directorio de Azure AD, es posible que también activó basados en licencias de suscripciones para su organización, como Azure AD Premium P2, Office 365 empresa Premium, o Enterprise Mobility + Security E5. Para evitar la pérdida accidental de datos, no se puede eliminar un directorio hasta que las suscripciones se eliminen por completo. Las suscripciones deben estar en un **Desaprovisionado** estado para permitir la eliminación del directorio. Un **expirado** o **Canceled** suscripción se mueve a la **deshabilitado** estado y la fase final es el **Desaprovisionado** estado.

Para saber qué se puede esperar cuando una suscripción de Office 365 de prueba expira (exceptuando el caso de licencias por volumen, el contrato para empresas o de CSP o asociados de pago), consulte la tabla siguiente. Para obtener más información sobre la retención de datos de Office 365 y el ciclo de vida de la suscripción, consulte [¿Qué pasa con mis datos y mi acceso cuando termina mi suscripción de Office 365 para empresas?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Estado de la suscripción | Datos | Acceso a datos
----- | ----- | -----
Activo (30 días para evaluación) | Datos accesibles a todos | Los usuarios tienen acceso normal a los archivos o aplicaciones de Office 365<br>Los administradores tienen acceso normal a centro de administración de Microsoft 365 y recursos 
Expirado (30 días) | Datos accesibles a todos| Los usuarios tienen acceso normal a los archivos o aplicaciones de Office 365<br>Los administradores tienen acceso normal a centro de administración de Microsoft 365 y recursos
Deshabilitado (30 días) | Datos accesibles solo para administradores | Los usuarios no pueden obtener acceso a archivos o aplicaciones de Office 365<br>Los administradores pueden acceder al centro de administración de Microsoft 365 pero no se puede asignar licencias a o actualizar los usuarios
Desaprovisionado (30 días tras la deshabilitación) | Los datos se eliminan (automáticamente si no hay otros servicios en uso) | Los usuarios no pueden obtener acceso a archivos o aplicaciones de Office 365<br>Los administradores pueden tener acceso al centro de administración de Microsoft 365 para adquirir y administrar otras suscripciones

## <a name="delete-a-subscription"></a>Eliminar una suscripción

Puede colocar una suscripción al estado Desaprovisionado eliminarse en tres días mediante el centro de administración de Microsoft 365.

1. Inicie sesión en el [centro de administración de Microsoft 365](https://admin.microsoft.com) con una cuenta que sea un administrador global de su organización. Si está intentando eliminar el directorio "Contoso" que tiene el dominio predeterminado inicial, "contoso.onmicrosoft.com", inicie sesión con un UPN como admin@contoso.onmicrosoft.com.

2. Seleccione **facturación** y seleccione **suscripciones**, a continuación, elija la suscripción que desea cancelar. Tras hacer clic en **Cancelar**, actualice la página.
  
   ![Eliminación del vínculo para eliminar la suscripción](./media/directory-delete-howto/delete-command.png)
  
3. Seleccione **Eliminar** para eliminar la suscripción y acepte los términos y condiciones. Todos los datos se eliminarán permanentemente a los tres días. Puede reactivar la suscripción durante el período de tres días, si cambia de opinión.
  
   ![Lea detenidamente los términos y condiciones](./media/directory-delete-howto/delete-terms.png)

4. Ahora que ha cambiado el estado de la suscripción, esta se marca para eliminarla. La suscripción entra en el estado **Desaprovisionado** 72 horas más tarde.

5. Una vez que se ha eliminado una suscripción en su directorio y ha transcurrido estas 72 horas, puede iniciar nuevo en el centro de administración de Azure AD nuevo y haya debe ser ninguna acción necesaria y no bloquea la eliminación del directorio de suscripciones. Debe ser capaz de eliminar correctamente el directorio de Azure AD.
  
   ![pasar la comprobación de suscripción en la pantalla de eliminación](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="i-have-a-trial-subscription-that-blocks-deletion"></a>Tengo una suscripción de prueba que bloquea la eliminación

Hay [autoservicio registro productos](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide) como usuarios individuales de Microsoft Power BI, Rights Management Services, Microsoft Power Apps o Dynamics 365, pueden registrarse a través de Office 365, que también crea un usuario invitado para la autenticación en directorio de Azure AD. Estos productos autoservicio bloquean las eliminaciones del directorio hasta que se eliminen por completo desde el directorio, para evitar la pérdida de datos. Se puede eliminar solo por el Administrador de Azure AD si el usuario suscrito individualmente o se le asignó el producto.

Hay dos tipos de productos registro de autoservicio en cómo se asignan: 

* Asignación de nivel de organización: Un administrador de Azure AD asigna el producto en toda la organización y un usuario puede estar usando activamente el servicio con esta asignación de nivel de organización incluso si no tienen licencia individualmente.
* Asignación de nivel de usuario: Un usuario individual durante el registro de autoservicio asigna básicamente el producto a sí mismos sin un administrador. Una vez que la organización esté administrada por un administrador (consulte [adquisición de administrador de un directorio no administrado](domains-admin-takeover.md), a continuación, el administrador puede asignar directamente el producto a los usuarios sin registro de autoservicio.  

Al comenzar la eliminación del producto de suscripción de autoservicio, la acción elimina los datos de permanentemente y elimina todos los accesos de usuario para el servicio. Cualquier usuario que se asignó la oferta individualmente o en el nivel de organización, a continuación, no podrá iniciar sesión o acceder a los datos existentes. Si desea evitar la pérdida de datos con el producto de suscripción de autoservicio como [paneles de Microsoft Power BI](https://docs.microsoft.com/power-bi/service-export-to-pbix) o [configuración de directiva de Rights Management Services](https://docs.microsoft.com/azure/information-protection/configure-policy#how-to-configure-the-azure-information-protection-policy), asegúrese de que los datos es una copia de seguridad y Guardar en cualquier lugar.

Para más información sobre los productos disponibles actualmente registro autoservicio y servicios, consulte [programas disponibles autoservicio](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide#available-self-service-programs).

Para saber qué se puede esperar cuando una suscripción de Office 365 de prueba expira (exceptuando el caso de licencias por volumen, el contrato para empresas o de CSP o asociados de pago), consulte la tabla siguiente. Para obtener más información sobre Office 365 datos retención y la suscripción del ciclo de vida, consulte [¿qué ocurre con mis datos y el acceso cuando termina mi Office 365 para la suscripción de empresa?](https://docs.microsoft.com/office365/admin/subscriptions-and-billing/what-if-my-subscription-expires?view=o365-worldwide).

Estado del producto | Datos | Acceso a datos
------------- | ---- | --------------
Activo (30 días para evaluación) | Datos accesibles a todos | Los usuarios tienen acceso normal a autoservicio suscripción a un producto, archivos o aplicaciones<br>Los administradores tienen acceso normal a centro de administración de Microsoft 365 y recursos
Deleted | Datos eliminados | Los usuarios no pueden tener acceso de autoservicio de suscripción a un producto, archivos o aplicaciones<br>Los administradores pueden tener acceso al centro de administración de Microsoft 365 para adquirir y administrar otras suscripciones

## <a name="how-can-i-delete-a-self-service-sign-up-product-in-the-azure-portal"></a>¿Cómo se puede eliminar un producto de suscripción de autoservicio en el portal de Azure?

Puede colocar un producto de suscripción de autoservicio como Microsoft Power BI o Azure Rights Management Services en un **eliminar** estado se eliminarán inmediatamente en el portal de Azure AD.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con una cuenta que sea un administrador Global de la organización. Si está intentando eliminar el directorio "Contoso" que tiene el dominio predeterminado inicial, "contoso.onmicrosoft.com", inicie sesión con un UPN como admin@contoso.onmicrosoft.com.

2. Seleccione **licencias**y, a continuación, seleccione **autoservicio registro productos**. Puede ver todos los autoservicio registro productos por separado de las suscripciones basadas en puestos. Elija el producto que quiere eliminar permanentemente. Este es un ejemplo en Microsoft Power BI:

    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/directory-delete-howto/licenses-page.png)

3. Seleccione **eliminar** para eliminar el producto y acepte los términos de esos datos se eliminan inmediatamente y de forma irrevocable. Esta acción de eliminación quitará todos los usuarios y quitar el acceso de la organización para el producto. Haga clic en Sí para continuar con la eliminación.  

    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/directory-delete-howto/delete-product.png)

4. Al seleccionar **Sí**, se iniciará la eliminación del producto de autoservicio. Hay una notificación que le informará de la eliminación en curso.  

    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/directory-delete-howto/progress-message.png)

5. Ahora que ha cambiado el estado de autoservicio de suscripción a un producto a **Deleted**. Cuando actualice la página, el producto debe quitarse la **autoservicio registro productos** página.  

    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/directory-delete-howto/product-deleted.png)

6. Una vez que haya eliminado todos los productos, puede iniciar sesión en el centro de administración de Azure AD nuevo y debería haber ninguna acción necesaria y no bloquea la eliminación del directorio de productos. Debe ser capaz de eliminar correctamente el directorio de Azure AD.

    ![se escribió incorrectamente o no se encuentra el nombre de usuario](./media/directory-delete-howto/delete-organization.png)

## <a name="next-steps"></a>Pasos siguientes

[Documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
