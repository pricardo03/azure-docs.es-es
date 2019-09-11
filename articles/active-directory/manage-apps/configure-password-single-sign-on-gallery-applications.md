---
title: Configuración del inicio de sesión único con contraseña para una aplicación de la galería de Azure AD | Microsoft Docs
description: Configuración de una aplicación para el inicio de sesión único seguro basado en contraseña cuando ya figura en la galería de aplicaciones de Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 9fb33c4110a590539c85364885da9a27853f6bd0
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70146886"
---
# <a name="configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Configuración del inicio de sesión único con contraseña para una aplicación de la galería de Azure AD

Cuando se agrega una aplicación desde la [galería de aplicaciones de Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), tiene la opción de elegir cómo desea que los usuarios inicien sesión en esa aplicación. Esta opción se puede configurar en cualquier momento si selecciona **Inicio de sesión único** en una aplicación empresarial en [Azure Portal](https://portal.azure.com/).

Una de las opciones de inicio de sesión único (SSO) disponible es el [inicio de sesión único basado en contraseña](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). Esta es una excelente manera de comenzar a integrar rápidamente las aplicaciones en Azure AD. Esta opción permite lo siguiente:

-   Almacenar y reproducir de forma segura nombres de usuario y contraseñas para la aplicación que ha integrado con Azure AD

-   Proporcionar compatibilidad con aplicaciones que requieren varios campos de inicio de sesión además de los campos de nombre de usuario y contraseña

-   Personalizar las etiquetas de los campos de entrada de nombre de usuario y contraseña que los usuarios ven en el [Panel de acceso de la aplicación](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) cuando escriben sus credenciales

-   Que los usuarios proporcionen sus propios nombres de usuario y contraseñas para las cuentas de aplicación existentes, que escriben manualmente en el [Panel de acceso de la aplicación](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Que un miembro del grupo de negocios use la característica [Acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) para especificar los nombres de usuario y las contraseñas asignados a un usuario

-   Que un administrador especifique los nombres de usuario y las contraseñas que se asignan a un usuario mediante la característica Actualizar credenciales al [asignar un usuario a una aplicación](#assign-a-user-to-an-application-directly)

-   Que un administrador use la característica Actualizar credenciales para especificar el nombre de usuario o la contraseña compartidos para un grupo de personas cuando [asignan un grupo a una aplicación](#assign-an-application-to-a-group-directly)

En la siguiente sección se describe cómo habilitar el [inicio de sesión único con contraseña](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) en una aplicación que ya está en la [galería de aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="overview-of-required-steps"></a>Información general de los cambios necesarios
Para configurar una aplicación desde la galería de Azure AD, realice los siguientes pasos:

-   [Incorporación de una aplicación desde la galería de Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Configuración de la aplicación para el inicio de sesión único con contraseña](#configure-the-application-for-password-single-sign-on)

-   Asigne la aplicación a un usuario o un grupo:

    -   [Asignar un usuario a una aplicación directamente](#assign-a-user-to-an-application-directly)

    -   [Asignar una aplicación a un grupo directamente](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Incorporación de una aplicación desde la galería de Azure AD

Para agregar una aplicación desde la galería de Azure AD, siga estos pasos:

1.  Abra [Azure Portal](https://portal.azure.com) e inicie sesión como **administrador global** o **coadministrador**.

2.  Abra la **extensión de Azure Active Directory**; para ello, seleccione **Todos los servicios** en la parte superior del menú izquierdo.

3.  Escriba **Azure Active Directory** en el cuadro de búsqueda y, luego, seleccione el elemento **Azure Active Directory**.

4.  Seleccione **Aplicaciones empresariales** en el menú de Azure AD de la izquierda.

5.  Seleccione **Agregar** en la esquina superior derecha del panel **Aplicaciones empresariales**.

6.  En el cuadro **Escriba un nombre** de la sección **Agregar desde la galería**, escriba el nombre de la aplicación.

7.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

8.  Antes de agregar la aplicación, puede cambiar su nombre en el cuadro de texto **Nombre**.

9.  Seleccione **Agregar** para agregar la aplicación.

Tras un breve período, podrá ver el panel de configuración de la aplicación.

## <a name="configure-the-application-for-password-single-sign-on"></a>Configuración de la aplicación para el inicio de sesión único con contraseña

Para configurar el inicio de sesión único para una aplicación, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

2. Abra la **extensión de Azure Active Directory**; para ello, seleccione **Todos los servicios** en la parte superior del menú izquierdo.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda y, luego, seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que desea configurar para el inicio de sesión único.

7. Después de cargar la aplicación, seleccione **Inicio de sesión único** en el menú izquierdo de la aplicación.

8. Seleccione el modo **Inicio de sesión con contraseña**.

9. [Asigne usuarios a la aplicación](#assign-a-user-to-an-application-directly).

10. También puede proporcionar credenciales en nombre de los usuarios; para ello, seleccione la fila de un usuario, seleccione **Actualizar credenciales** y, luego, escriba el nombre de usuario y la contraseña. De lo contrario, se solicitará a los usuarios que escriban sus credenciales cuando inicien la aplicación.

## <a name="assign-a-user-to-an-application-directly"></a>Asignar un usuario a una aplicación directamente

Para asignar uno o varios usuarios a una aplicación directamente, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global**.

2. Abra la **extensión de Azure Active Directory**; para ello, seleccione **Todos los servicios** en la parte superior del menú izquierdo.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda y, luego, seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que quiere asignar a un usuario.

7. Después de cargar la aplicación, seleccione **Usuarios y grupos** en el menú izquierdo de la aplicación.

8. Seleccione el botón **Agregar** en la parte superior de la lista **Usuarios y grupos** para abrir el panel **Agregar asignación**.

9. Seleccione **Usuarios y grupos** en el panel **Agregar asignación**.

10. Escriba el nombre completo o la dirección de correo electrónico del usuario en el cuadro de búsqueda **Buscar por nombre o dirección de correo**.

11. Mantenga el mouse sobre el usuario de la lista y, luego, active la casilla situada junto a la foto o el logotipo del perfil del usuario para agregarlo a la lista **Seleccionado**.

12. Opcional: si quiere agregar más de un usuario, escriba otro nombre completo o dirección de correo electrónico en el cuadro **Buscar por nombre o dirección de correo** y active la casilla correspondiente a ese usuario para agregarlo a la lista **Seleccionado**.

13. Cuando haya terminado de seleccionar usuarios, use el botón **Seleccionar** para agregarlos a la lista de usuarios y grupos que se asignarán a la aplicación.

14. Opcional: Use el comando **Seleccionar rol** del panel **Agregar asignación** para seleccionar el rol que se asignará a los usuarios que ha seleccionado.

15. Seleccione **Asignar** para asignar la aplicación a los usuarios seleccionados.

## <a name="assign-an-application-to-a-group-directly"></a>Asignar una aplicación a un grupo directamente

Para asignar uno o varios grupos a una aplicación directamente, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global**.

2. Abra la **extensión de Azure Active Directory**; para ello, seleccione **Todos los servicios** en la parte superior del menú izquierdo.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda y, luego, seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú izquierdo de Azure AD.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones.** .

6. Seleccione la aplicación que quiere asignar a un usuario.

7. Después de cargar la aplicación, seleccione **Usuarios y grupos** en el menú izquierdo de la aplicación.

8. Seleccione el botón **Agregar** en la parte superior de la lista **Usuarios y grupos** para abrir el panel **Agregar asignación**.

9. Seleccione **Usuarios y grupos** en el panel **Agregar asignación**.

10. Escriba el nombre de grupo completo que quiere asignar en el cuadro de búsqueda **Buscar por nombre o dirección de correo**.

11. Mantenga el mouse sobre el **grupo** de la lista y, luego, active la casilla situada junto a la foto o el logotipo del perfil del grupo para agregarlo a la lista **Seleccionado**.

12. Opcional: Si quiere agregar más de un grupo, escriba otro nombre completo de grupo en el cuadro de búsqueda **Buscar por nombre o dirección de correo electrónico** y, luego, active la casilla correspondiente para agregar este grupo a la lista **Seleccionado**.

13. Cuando haya terminado de seleccionar grupos, use el botón **Seleccionar** para agregarlos a la lista de usuarios y grupos que se asignarán a la aplicación.

14. Opcional: Use el comando **Seleccionar rol** en el panel **Agregar asignación** para seleccionar un rol y asignarlo a los grupos que ha seleccionado.

15. Seleccione **Asignar** para asignar la aplicación a los grupos seleccionados.

Después de un breve período, los usuarios que ha seleccionado deberían poder iniciar estas aplicaciones desde el Panel de acceso.

## <a name="next-steps"></a>Pasos siguientes
[Proporcionar un inicio de sesión único a las aplicaciones con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md)
