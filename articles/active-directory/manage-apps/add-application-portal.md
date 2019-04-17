---
title: Incorporación de una aplicación a un inquilino de Azure Active Directory | Microsoft Docs
description: Esta guía de inicio rápido usa Azure Portal para agregar una aplicación de la galería a su inquilino de Azure Active Directory (Azure AD).
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 04/09/2019
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: b15842d7157e8e5a691f37f846dd424bf308eae3
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565209"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Inicio rápido: Incorporación de una aplicación al inquilino de Azure Active Directory

Azure Active Directory (Azure AD) incluye una galería que contiene miles de aplicaciones previamente integradas. Algunas de las aplicaciones que su organización usa probablemente estén en la galería. Esta guía de inicio rápido usa Azure Portal para agregar una aplicación de la galería a su inquilino de Azure Active Directory (Azure AD).

Después de agregar una aplicación a su inquilino de Azure AD, puede:

- Administrar el acceso de usuario a la aplicación con una directiva de acceso condicional.
- Configurar los usuarios para el inicio de sesión único a la aplicación con sus cuentas de Azure AD.

## <a name="before-you-begin"></a>Antes de empezar

Para agregar una aplicación a su inquilino, necesita:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único para la aplicación

Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global del inquilino de Azure AD, administrador de aplicaciones en la nube o administrador de aplicaciones.

Para probar los pasos de este tutorial, se recomienda usar un entorno que no sea de producción. Si no dispone de un entorno de Azure AD que no sea de producción, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Incorporación de una aplicación a un inquilino de Azure AD

Para agregar una aplicación de la galería a su inquilino de Azure AD:

1. En [Azure Portal](https://portal.azure.com), en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales**.

    ![Apertura de las aplicaciones empresariales](media/add-application-portal/open-enterprise-apps.png)

1. Se abre el panel **Todas las aplicaciones**, en el que se ve una muestra aleatoria de las aplicaciones que hay en su inquilino de Azure AD. Seleccione **Nueva aplicación** en la parte superior del panel **Todas las aplicaciones**.

    ![Nueva aplicación](media/add-application-portal/new-application.png)

1. En el panel **Categorías**, verá iconos en el área **Aplicaciones destacadas** que son una muestra aleatoria de las aplicaciones de la galería.  Para ver más aplicaciones, puede seleccionar **Mostrar más**. Sin embargo, no es aconsejable buscar de este modo porque hay miles de aplicaciones en la galería.

    ![Buscar por nombre o categoría](media/add-application-portal/categories.png)

1. Para buscar una aplicación, en **Agregar desde la galería**, escriba el nombre de la aplicación que desea agregar. Seleccione la aplicación en los resultados y, después, **Agregar**. El ejemplo siguiente muestra el formulario **Agregar aplicación** que aparece después de buscar github.com.

    ![Adición de una aplicación](media/add-application-portal/add-an-application.png)

1. En el formulario específico de la aplicación, puede cambiar la información de la propiedad. Por ejemplo, puede editar el nombre de la aplicación para que coincida con las necesidades de su organización. Este ejemplo usa el nombre **GitHub-test**.

1. Cuando haya terminado de realizar cambios en las propiedades, seleccione **Agregar**.

1. Aparece una página de introducción con las opciones para configurar la aplicación para su organización.

Ha terminado de agregar la aplicación. Puede tomarse un descanso. Las secciones siguientes muestran cómo cambiar el logotipo y modificar otras propiedades de la aplicación.

## <a name="find-your-azure-ad-tenant-application"></a>Búsqueda de su aplicación de inquilino de Azure AD

Supongamos que tuvo que salir y ahora vuelve para continuar con la configuración de la aplicación. Lo primero que debe hacer es encontrar la aplicación.

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales**.

1. En el menú desplegable **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**. Para obtener más información acerca de las opciones de presentación, consulte [Visualización de las aplicaciones de inquilino](view-applications-portal.md).

1. Ahora puede ver una lista de todas las aplicaciones en su inquilino de Azure AD. La lista es una muestra aleatoria. Para ver más aplicaciones, seleccione **Mostrar más** una o varias veces.

1. Para encontrar rápidamente una aplicación en el inquilino, escriba el nombre de la misma en el cuadro de búsqueda y seleccione **Aplicar**. Este ejemplo encuentra la aplicación de prueba de GitHub que ha agregado anteriormente.

    ![Búsqueda de una aplicación](media/add-application-portal/find-application.png)


## <a name="configure-user-sign-in-properties"></a>Configuración de las propiedades de inicio de sesión de usuario

Ahora que ha encontrado la aplicación, puede abrirla y configurar sus propiedades.

Para editar las propiedades de la aplicación:

1. Seleccione la aplicación para abrirla.
1. Seleccione **Propiedades** para abrir el panel de propiedades para editarlas.

    ![Panel Editar propiedades](media/add-application-portal/edit-properties.png)

1. Dedique un momento para conocer las opciones de inicio de sesión. Las opciones determinan la forma en que los usuarios que están asignados, o no, a la aplicación pueden iniciar sesión en ella. Y las opciones también determinan si los usuarios pueden ver la aplicación en el panel de acceso.

    - **Enabled for users to sign-in** (Habilitado para que los usuarios inicien sesión) determina si los usuarios asignados a la aplicación pueden iniciar sesión.
    - **Asignación de usuarios necesaria** determina si los usuarios que no están asignados a la aplicación pueden iniciar sesión.
    - **Visible to user** (Visible para el usuario) determina si los usuarios asignados a una aplicación pueden verlo en el panel de acceso y el iniciador de Office 365.

1. Use las siguientes tablas para ayudarle a elegir las opciones que más se ajusten a sus necesidades.

   - Comportamiento para los usuarios **asignados**:

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

   - Comportamiento para los usuarios **no asignados**:

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

## <a name="use-a-custom-logo"></a>Uso de un logotipo personalizado

Para usar un logotipo personalizado:

1. Cree un logotipo de 215 por 215 píxeles y guárdelo en formato PNG.
1. Puesto que ya ha encontrado la aplicación, selecciónela.
1. Seleccione **Propiedades** en el panel izquierdo.
1. Cargue el logotipo.
1. Cuando haya terminado, seleccione **Guardar**.

    ![Cambio del logotipo](media/add-application-portal/change-logo.png)


## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a agregar una aplicación de la galería a su inquilino de Azure AD. Ha aprendido a modificar las propiedades de una aplicación.

Ahora, está preparado para configurar la aplicación para el inicio de sesión único.

> [!div class="nextstepaction"]
> [Configuración del inicio de sesión único](configure-single-sign-on-portal.md)


