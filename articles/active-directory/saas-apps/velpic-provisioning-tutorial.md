---
title: 'Tutorial: Configurar Velpic para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario en Velpic.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16c302fbe151d6cd8c2198240bc31a2bd69dbd7b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59270922"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Tutorial: Configurar Velpic para aprovisionar usuarios automáticamente

El objetivo de este tutorial es explicar los pasos que debe realizar en Velpic y Azure AD para aprovisionar y desaprovisionar automáticamente cuentas de usuario de Azure AD en Velpic.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure Active Directory
* Un inquilino de Velpic con el [plan empresarial](https://www.velpic.com/pricing.html) o uno superior habilitado
* Una cuenta de usuario en Velpic con permisos de administrador

## <a name="assigning-users-to-velpic"></a>Asignación de usuarios a Velpic

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento de cuentas de usuario automático, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación de Azure AD. 

Antes de configurar y habilitar el servicio de aprovisionamiento, deberá decidir qué usuarios o grupos en Azure AD representan a los usuarios que necesitan tener acceso a la aplicación Velpic. Una vez decidido, puede asignar a estos usuarios a la aplicación Velpic siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Sugerencias importantes para asignar usuarios a Velpic

* Se recomienda que un único usuario de Azure AD asignará a Velpic para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Velpic, debe seleccionar la **usuario** rol u otro válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Tenga en cuenta que el **acceso predeterminado** rol no funciona para el aprovisionamiento y estos usuarios se omitirá.

## <a name="configuring-user-provisioning-to-velpic"></a>Configuración del aprovisionamiento de usuarios en Velpic

Esta sección le guiará a través de conexión de Azure AD a la API de aprovisionamiento de cuentas de usuario de Velpic y configurar el servicio de aprovisionamiento para crear, actualizar y deshabilitar cuentas de usuario en Velpic en función de asignación de grupos en Azure AD y de usuario asignadas.

> [!TIP]
> También puede habilitar basado en SAML Single Sign-On para Velpic, siguiendo las instrucciones en [portal Azure](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Para configurar el aprovisionamiento de cuentas de usuario automático en Velpic en Azure AD:

1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Si ya ha configurado Velpic para el inicio de sesión único, busque la instancia de Velpic con el campo de búsqueda. En caso contrario, seleccione **agregar** y busque **Velpic** en la Galería de aplicaciones. Seleccione Velpic en los resultados de búsqueda y agréguelo a la lista de aplicaciones.

3. Seleccione la instancia de Velpic y luego seleccione el **aprovisionamiento** ficha.

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Velpic](./media/velpic-provisioning-tutorial/Velpic1.png)

5. En el **las credenciales de administrador** sección, entrada la **dirección URL de inquilino & Token secreto** de Velpic. () Puede encontrar estos valores en su cuenta de Velpic: **Administrar** > **integración** > **complemento** > **SCIM**)

    ![Valores de autorización](./media/velpic-provisioning-tutorial/Velpic2.png)

6. En el portal de Azure, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación Velpic. Si se produce un error en la conexión, asegúrese de que la cuenta de Velpic tiene permisos de administrador e inténtelo de nuevo el paso 5.

7. Escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error aprovisionamiento en el campo **Correo electrónico de notificación** y active la casilla que aparece a continuación.

8. Haga clic en **Save**(Guardar).

9. En la sección asignaciones, seleccione **sincronizar Active Directory usuarios de Azure en Velpic**.

10. En el **asignaciones de atributos** sección, revise los atributos de usuario que se sincronizarán de Azure AD con Velpic. Tenga en cuenta que los atributos seleccionados como **coincidencia** se usarán las propiedades para que coincida con las cuentas de usuario de Velpic para las operaciones de actualización. Seleccione el botón Guardar para confirmar los cambios.

11. Para habilitar el servicio de Velpic de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección

12. Haga clic en **Save**(Guardar).

Esta acción iniciará la sincronización inicial de todos los usuarios y grupos asignados a Velpic en la sección usuarios y grupos. Tenga en cuenta que la sincronización inicial tardará más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si se está ejecutando el servicio. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos a los informes de actividad de aprovisionamiento, que describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)