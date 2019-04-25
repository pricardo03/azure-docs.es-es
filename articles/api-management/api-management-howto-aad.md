---
title: Autorización de las cuentas de desarrollador mediante Azure Active Directory en Azure API Management | Microsoft Docs
description: Aprenda a autorizar a los usuarios mediante Azure Active Directory en API Management.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: 95e501eca0f7765cc5201f7b315703c3ca43df8f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60529453"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Autorización de las cuentas de desarrollador mediante Azure Active Directory en Azure API Management

En este artículo se muestra cómo habilitar el acceso al portal para desarrolladores para usuarios de Azure Active Directory. También se muestra cómo administrar grupos de usuarios de Azure AD mediante la adición de grupos externos que contienen a los usuarios.

## <a name="prerequisites"></a>Requisitos previos

- Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
- Importe y publique una instancia de Azure API Management. Para más información, consulte [Importación y publicación](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Autorización de las cuentas de desarrollador con Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
2. Seleccionar ![arrow](./media/api-management-howto-aad/arrow.png).
3. En el cuadro de búsqueda, escriba **api**.
4. Seleccione **Servicios API Management**.
5. Seleccione su instancia de servicio API Management.
6. En **seguridad**, seleccione **identidades**.
7. Haga clic en **+ Agregar** en la parte superior.

    Aparece el panel **Agregar proveedor de identidades** a la derecha.
8. En **Tipo de proveedor**, seleccione **Azure Active Directory**.

    En el panel aparecen controles que le permiten escribir otra información necesaria. Los controles incluyen **Id. de cliente** y **Secreto de cliente**. (Más adelante en el artículo obtendrá información sobre estos controles).
9. Tome nota del contenido de **dirección URL de redireccionamiento**.
    
   ![Pasos para agregar un proveedor de identidades en Azure Portal](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. En el explorador, abra una pestaña diferente. 
11. Navegue hasta la [Azure portal: registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) para registrar una aplicación en Active Directory.
12. En **administrar**, seleccione **registros de aplicaciones**.
13. Seleccione **Nuevo registro**. En el **registrar una aplicación** página, establezca los valores como sigue:
    
* Establecer **nombre** a un nombre significativo. e.g., *developer-portal*
* Establecer **admite tipos de cuenta** a **cuentas en este directorio organizativa solo**. 
* Establecer **URI de redireccionamiento** en el valor obtenido en el paso 9. 
* Elija **registrar**. 

14.  Una vez registrada la aplicación, copie el **Id. de aplicación (cliente)** desde el **Introducción** página. 
15. Vuelva a la instancia de API Management. En el **Agregar proveedor de identidades** ventana, pegue el **Id. de aplicación (cliente)** valor en el **Id. de cliente** cuadro.
16. Volver a la configuración de Azure AD, seleccione **certificados y secretos** en **administrar**. Seleccione el **nuevo secreto de cliente** botón. Escriba un valor en **descripción**, seleccione cualquier opción de **Expires** y elija **agregar**. Copie el valor de secreto de cliente antes de salir de la página. Esta información la necesitará en el siguiente paso. 
17. Vuelva a la instancia de API Management, pegue el secreto en el **secreto de cliente** cuadro.

    > [!IMPORTANT]
    > Asegúrese de actualizar el **secreto de cliente** antes de que expire la clave. 
    >  
    >

18. La ventana **Add identity provider** (Agregar proveedor de identidades) también contiene el cuadro de texto **Allowed Tenants** (Inquilinos permitidos). Ahí, especifique los dominios de las instancias de Azure AD a las que quiere conceder acceso a las API de la instancia de servicio API Management. Puede separar varios dominios mediante nuevas líneas, espacios o comas.

> [!NOTE]
> Puede especificar varios dominios en la sección **Allowed Tenants** (Inquilinos permitidos). Para que un usuario pueda iniciar sesión desde otro dominio distinto al dominio original donde se registró la aplicación, un administrador global de ese otro dominio debe conceder antes a la aplicación permiso de acceso a los datos del directorio. Para conceder permiso, el administrador global debe: una. Ir a `https://<URL of your developer portal>/aadadminconsent` (por ejemplo, https://contoso.portal.azure-api.net/aadadminconsent)).
> b. Escribir el nombre de dominio del inquilino de Azure AD al que desea dar acceso.
> c. Seleccione **Submit** (Enviar). 

19.  Después de especificar la configuración deseada, seleccione **Agregar**.

Después de guardar los cambios, los usuarios de la instancia de Azure AD especificada pueden iniciar sesión en el portal para desarrolladores siguiendo los pasos de [Inicio de sesión en el portal para desarrolladores con una cuenta de Azure AD](#log_in_to_dev_portal).


## <a name="add-an-external-azure-ad-group"></a>Incorporación de un grupo externo de Azure AD

Después de permitir el acceso para los usuarios en una instancia de Azure AD, puede agregar grupos de Azure AD en API Management. Luego, puede administrar de manera más fácil la asociación de los desarrolladores del grupo con los productos deseados.

Para configurar un grupo externo de Azure AD, primero debe configurar la instancia de Azure AD en la pestaña **Identities** (Identidades) mediante el procedimiento de la sección anterior. 

Los grupos externos de Azure AD se agregan desde la pestaña **Grupos** de la instancia de API Management.

1. Seleccione la pestaña **Grupos** .
2. Seleccione el botón **Add AAD group** (Agregar grupo de AAD).
   ![Botón "Add AAD group" (Agregar grupo de AAD)](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Seleccione el grupo que quiere agregar.
4. Presione el botón **Select** (Seleccionar).

Después de agregar un grupo externo de Azure AD, puede revisar y configurar sus propiedades. Seleccione el nombre del grupo en la pestaña **Groups** (Grupos). Desde aquí, puede editar la información de **nombre** y **descripción** del grupo.
 
Los usuarios de la instancia de Azure AD configurada ya pueden iniciar sesión en el portal para desarrolladores. Pueden ver los grupos para los que tengan visibilidad y suscribirse a ellos.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Inicio de sesión en el portal para desarrolladores con una cuenta de Azure AD

Para iniciar sesión en el portal para desarrolladores mediante una cuenta de Azure AD que ha configurado en las secciones anteriores:

1. Abra una nueva ventana del explorador mediante la dirección URL de inicio de sesión de la configuración de aplicación de Active Directory y seleccione **Azure Active Directory**.

   ![Página de inicio de sesión][api-management-dev-portal-signin]

1. Escriba las credenciales de uno de los usuarios de Azure AD y seleccione **Iniciar sesión**.

   ![Inicio de sesión con el nombre de usuario y la contraseña][api-management-aad-signin]

1. En caso de requerirse más información, puede que se le solicite un formulario de registro. Complete el formulario de registro y seleccione **Suscribirse**.

   ![Botón Suscribirse del formulario de registro][api-management-complete-registration]

Ahora ya inició sesión en el portal para desarrolladores de la instancia del servicio API Management.

![Portal para desarrolladores una vez completado el registro][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
