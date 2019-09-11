---
title: Configuración del inicio de sesión único federado para una aplicación de la galería de Azure AD | Microsoft Docs
description: Cómo configurar el inicio de sesión único federado para una aplicación existente de la galería de Azure AD y usar los tutoriales para empezar a trabajar rápidamente
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
ms.openlocfilehash: 8e50a495e1b0406e0c935ac31111dc6b5d0c0821
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2019
ms.locfileid: "70207119"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Configuración del inicio de sesión único federado para una aplicación de la galería de Azure AD

Hay un tutorial paso a paso disponible para todas las aplicaciones de la galería de Azure Active Directory (Azure AD) que tienen la funcionalidad de inicio de sesión único (SSO) empresarial. Para instrucciones detalladas paso a paso, acceda a la [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/).

## <a name="overview-of-steps-required"></a>Introducción a los pasos necesarios
Para configurar una aplicación desde la galería de Azure AD, realice los siguientes pasos:

-   [Incorporación de una aplicación desde la galería de Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Configuración de los valores de metadatos de la aplicación en Azure AD (URL de inicio de sesión, identificador, URL de respuesta)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selección del identificador de usuario e incorporación de los atributos de usuario para enviarlos a la aplicación](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Recuperación de los metadatos y el certificado de Azure AD](#download-the-azure-ad-metadata-or-certificate)

-   [Configuración de los valores de metadatos de Azure AD en la aplicación (URL de inicio de sesión, emisor, URL de cierre de sesión y certificado)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Asignación de usuarios a la aplicación](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Incorporación de una aplicación desde la galería de Azure AD

Para agregar una aplicación desde la galería de Azure AD, siga estos pasos:

1.  Abra [Azure Portal](https://portal.azure.com) e inicie sesión como **administrador global** o **coadministrador**.

2.  Abra la **extensión de Azure Active Directory**; para ello, haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3.  Escriba "Azure Active Directory" en el cuadro de búsqueda y seleccione **Azure Active Directory**.

4.  En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5.  Seleccione **Agregar** en la esquina superior derecha del panel **Aplicaciones empresariales**.

6.  En el cuadro **Escriba un nombre** de la sección **Agregar desde la galería**, escriba el nombre de la aplicación.

7.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

8.  Antes de agregar la aplicación, puede cambiar su nombre en el cuadro **Nombre**.

9.  Seleccione **Agregar** para agregar la aplicación.

Tras un breve período, podrá ver el panel de configuración de la aplicación.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Configuración del inicio de sesión único para una aplicación de la galería de Azure AD

Para configurar el inicio de sesión único para una aplicación, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

2. Abra la **extensión de Azure Active Directory**; para ello, haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba "Azure Active Directory" en el cuadro de búsqueda y seleccione **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   * Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que desea configurar para el inicio de sesión único.

7. Cuando se cargue la aplicación, seleccione **Inicio de sesión único** en el menú de navegación izquierdo de la aplicación.

8. En la lista desplegable **Modo**, seleccione **Inicio de sesión basado en SAML**.

9. Especifique los valores obligatorios en **Dominio y direcciones URL**. El proveedor de la aplicación le proporcionará estos valores.

   1. Para configurar la aplicación como SSO iniciado por el SP, la dirección URL de inicio de sesión es obligatoria. En algunas aplicaciones, el identificador también es obligatorio.

   2. Para configurar la aplicación como SSO iniciado por el IdP, la dirección URL de respuesta es obligatoria. En algunas aplicaciones, el identificador también es obligatorio.

10. **Opcional**: seleccione **Mostrar configuración avanzada de URL** si quiere ver los valores no obligatorios.

11. En **Atributos de usuario**, seleccione el identificador único para los usuarios de la lista desplegable **Identificador de usuario**.

12. **Opcional**: seleccione **Ver y editar todos los demás atributos de usuario** para editar los atributos que se enviarán a la aplicación en el token SAML cuando los usuarios inicien sesión.

    Para agregar un atributo:
   
    1. Seleccione **Agregar atributo**. Escriba el **Nombre** y seleccione el **Valor** de la lista desplegable.

    1. Seleccione **Guardar.** En la tabla verá el nuevo atributo.

13. Seleccione **Configurar &lt;nombre de la aplicación&gt;** para acceder a documentación sobre cómo configurar el inicio de sesión único en la aplicación. Además, tiene las direcciones URL de metadatos necesarias y el certificado para instalar SSO con la aplicación.

14. Para guardar la configuración, seleccione **Guardar**.

15. Asigne usuarios a la aplicación.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selección del identificador de usuario e incorporación de los atributos de usuario para enviarlos a la aplicación

Para seleccionar el identificador de usuario o agregar atributos de usuario, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

2. Abra la **extensión de Azure Active Directory**; para ello, haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba "Azure Active Directory" en el cuadro de búsqueda y seleccione **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   * Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que ha configurado con el inicio de sesión único.

7. Cuando se cargue la aplicación, seleccione **Inicio de sesión único** en el menú de navegación izquierdo de la aplicación.

8. En la sección **Atributos de usuario**, seleccione el identificador único para los usuarios de la lista desplegable **Identificador de usuario**. La opción seleccionada debe coincidir con el valor esperado de la aplicación para autenticar al usuario.

   >[!NOTE] 
   >Azure AD selecciona el formato del atributo NameID (identificador de usuario) en función del valor seleccionado o del formato que solicite la aplicación en el elemento AuthRequest de SAML. Para más información, consulte la sección NameIDPolicy de [Protocolo SAML de inicio de sesión único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest).
   >
   >

9. Para agregar atributos de usuario, seleccione **Ver y editar todos los demás atributos de usuario** para editar los atributos que se enviarán a la aplicación en el token SAML cuando los usuarios inicien sesión.

   Para agregar un atributo:
  
   1. Seleccione **Agregar atributo**. Escriba el **Nombre** y seleccione el **Valor** de la lista desplegable.

   2. Seleccione **Guardar**. En la tabla verá el nuevo atributo.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Descarga del certificado o los metadatos de Azure AD

Para descargar el certificado o los metadatos de la aplicación de Azure AD, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

2. Abra la **extensión de Azure Active Directory**; para ello, haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba "Azure Active Directory" en el cuadro de búsqueda y seleccione **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   *  Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que ha configurado con el inicio de sesión único.

7. Cuando se cargue la aplicación, seleccione **Inicio de sesión único** en el menú de navegación izquierdo de la aplicación.

8. Vaya a la sección **Certificado de firma de SAML** y seleccione el valor de la columna **Descargar**. Según lo que necesite la aplicación para configurar el inicio de sesión único, verá la opción para descargar el archivo XML de metadatos o el certificado.

Además, Azure AD proporciona una dirección URL para acceder a los metadatos. Use el siguiente modelo para obtener la dirección URL de metadatos específica de la aplicación: `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`.

## <a name="assign-users-to-the-application"></a>Asignación de usuarios a la aplicación

Para asignar uno o varios usuarios a una aplicación directamente, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como **administrador global**.

2. Abra la **extensión de Azure Active Directory**; para ello, haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba "Azure Active Directory" en el cuadro de búsqueda y seleccione **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   * Si no ve aquí la aplicación deseada, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6. Seleccione la aplicación que desea asignar a un usuario de la lista.

7. Cuando se cargue la aplicación, seleccione **Usuarios y grupos** en el menú de navegación izquierdo de la aplicación.

8. Seleccione el botón **Agregar** en la parte superior de la lista **Usuarios y grupos** para abrir el panel **Agregar asignación**.

9. Seleccione el selector **Usuarios y grupos** del panel **Agregar asignación**.

10. Escriba el **nombre completo** o la **dirección de correo electrónico** del usuario que quiere asignar en el cuadro de búsqueda **Buscar por nombre o dirección de correo**.

11. Mantenga el puntero sobre el **usuario** en la lista para que aparezca una **casilla**. Active la casilla situada junto a la foto o el logotipo del perfil del usuario para agregarlo a la lista **Seleccionado**.

12. **Opcional**: si desea **agregar más de un usuario**, escriba otro **nombre completo** u otra **dirección de correo electrónico** en el cuadro de búsqueda **Buscar por nombre o dirección de correo** y active la casilla para agregar ese usuario a la lista **Seleccionado**.

13. Cuando haya terminado de seleccionar usuarios, seleccione el botón **Seleccionar** para agregarlos a la lista de usuarios y grupos que se asignarán a la aplicación.

14. **Opcional**: elija el selector **Seleccionar rol** del panel **Agregar asignación** para seleccionar un rol y asignarlo a los usuarios que ha seleccionado.

15. Seleccione el botón **Asignar** para asignar la aplicación a los usuarios seleccionados.

Tras un breve período de tiempo, los usuarios seleccionados podrán iniciar estas aplicaciones mediante los métodos descritos en la sección de descripción de la solución.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Personalización de las notificaciones SAML que se han enviado a una aplicación

Para obtener información sobre cómo personalizar las notificaciones de atributo SAML que se han enviado a su aplicación, vea [Asignación de notificaciones en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping).

## <a name="next-steps"></a>Pasos siguientes
[Proporcionar un inicio de sesión único a las aplicaciones con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md)



