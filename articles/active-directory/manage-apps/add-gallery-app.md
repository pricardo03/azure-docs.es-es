---
title: Adición de una aplicación de la galería en Azure Active Directory | Microsoft Docs
description: Aprenda a agregar una aplicación de la galería de Azure AD a las aplicaciones empresariales de Azure.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 06/18/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1bdf275bbafa9c46cfc4577ac2843da0be74c7ef
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477279"
---
# <a name="add-a-gallery-app-to-your-azure-ad-organization"></a>Adición de una aplicación de la galería a la organización de Azure AD

Azure Active Directory (Azure AD) dispone de una galería que contiene miles de aplicaciones previamente integradas que se habilitan con el inicio de sesión único empresarial. En este artículo se describen los pasos generales para agregar una aplicación de la galería a la organización de Azure AD.

> [!IMPORTANT]
> Primero, busque la aplicación en la [lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/). Probablemente encontrará instrucciones paso a paso para agregar y configurar la aplicación de la galería que quiere agregar.

## <a name="add-a-gallery-application"></a>Adición de una aplicación de la galería

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global del inquilino de Azure AD, administrador de aplicaciones en la nube o administrador de aplicaciones.

1. En [Azure Portal](https://portal.azure.com), en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales**.

    ![Apertura de las aplicaciones empresariales](media/add-application-portal/open-enterprise-apps.png)

1. Seleccione **Nueva aplicación**.

    ![Nueva aplicación](media/add-application-portal/new-application.png)

1. En **Agregar desde la galería**, en el cuadro de búsqueda, escriba el nombre del aplicación que quiere agregar. 

    ![Buscar por nombre o categoría](media/add-application-portal/categories.png)

1. Seleccione la aplicación en los resultados.

1. (Opcional) En el formulario específico de la aplicación, puede editar el nombre de la aplicación para que se ajuste a las necesidades de su organización.

1. Seleccione **Agregar**. Se abre la página **Información general** de la aplicación.

## <a name="configure-user-sign-in-properties"></a>Configuración de las propiedades de inicio de sesión de usuario

1. Seleccione **Propiedades** para abrir el panel de propiedades para editarlas.

    ![Panel Editar propiedades](media/add-application-portal/edit-properties.png)

1. Establezca las siguientes opciones para determinar cómo los usuarios que están asignados o no asignados a la aplicación pueden iniciar sesión en la aplicación y si un usuario puede ver la aplicación en el panel de acceso.

    - **Enabled for users to sign-in** (Habilitado para que los usuarios inicien sesión) determina si los usuarios asignados a la aplicación pueden iniciar sesión.
    - **Asignación de usuarios necesaria** determina si los usuarios que no están asignados a la aplicación pueden iniciar sesión.
    - **Visible to user** (Visible para el usuario) determina si los usuarios asignados a una aplicación pueden verlo en el panel de acceso y el iniciador de Office 365.

      Comportamiento para los usuarios **asignados**:

       | Configuración de propiedades de la aplicación | | | Experiencia del usuario asignado | |
       |---|---|---|---|---|
       | ¿Habilitado para que los usuarios inicien sesión? | ¿Se requiere la asignación de usuarios? | ¿Es visible para los usuarios? | ¿Pueden los usuarios asignados iniciar sesión? | ¿Pueden ver los usuarios asignados la aplicación?* |
       | Sí | Sí | Sí | Sí | Sí  |
       | Sí | Sí | no  | Sí | no   |
       | Sí | no  | Sí | Sí | Sí  |
       | Sí | no  | no  | Sí | no   |
       | no  | Sí | Sí | no  | no   |
       | no  | Sí | no  | no  | no   |
       | no  | no  | Sí | no  | no   |
       | no  | no  | no  | no  | no   |

      Comportamiento para los usuarios **no asignados**:

       | Configuración de propiedades de la aplicación | | | Experiencia del usuario no asignado | |
       |---|---|---|---|---|
       | ¿Está habilitado para que los usuarios inicien sesión? | ¿Se requiere la asignación de usuarios? | ¿Es visible para los usuarios? | ¿Pueden iniciar sesión los usuarios no asignados? | ¿Pueden ver la aplicación los usuarios no asignados?* |
       | Sí | Sí | Sí | no  | no   |
       | Sí | Sí | no  | no  | no   |
       | Sí | no  | Sí | Sí | no   |
       | Sí | no  | no  | Sí | no   |
       | no  | Sí | Sí | no  | no   |
       | no  | Sí | no  | no  | no   |
       | no  | no  | Sí | no  | no   |
       | no  | no  | no  | no  | no   |

     ¿*Puede el usuario ver la aplicación en el panel de acceso y el iniciador de aplicaciones de Office 365?

1. Para usar un logotipo personalizado, cree uno de 215 x 215 píxeles y guárdelo en formato PNG. A continuación, vaya a su logotipo y cárguelo.

    ![Cambio del logotipo](media/add-application-portal/change-logo.png)

1. Cuando haya terminado, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha agregado la aplicación a la organización de Azure AD, [elija el método de inicio de sesión único](what-is-single-sign-on.md#choosing-a-single-sign-on-method) que quiera usar y consulte a continuación el artículo adecuado:

- [Configuración del inicio de sesión único basado en SAML](configure-single-sign-on-non-gallery-applications.md)
- [Configuración del inicio de sesión único con contraseña](configure-password-single-sign-on-non-gallery-applications.md)
- [Configuración del inicio de sesión vinculado](configure-linked-sign-on.md)

