---
title: 'Colaboración con otros usuarios: LUIS'
titleSuffix: Azure Cognitive Services
description: El propietario de una aplicación puede agregar colaboradores a la aplicación. Estos colaboradores pueden modificar el modelo, entrenar y publicar la aplicación.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 2aaedb27d6875335b34c757505981f1a58462541
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932820"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Administración de creadores y colaboradores 

El propietario de una aplicación puede agregar colaboradores a la aplicación. Estos colaboradores pueden modificar el modelo, entrenar y publicar la aplicación. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Agregar un colaborador

Una aplicación tiene un único creador, el propietario, pero puede tener muchos colaboradores. Para permitir que los colaboradores editen la aplicación de LUIS, debe agregar el correo electrónico que usan para acceder al portal de LUIS a la lista de colaboradores. Una vez agregados, la aplicación se muestra en su portal de LUIS.

1. Seleccione **Administrar** en el menú superior derecho y, a continuación, seleccione **Colaboradores** en el menú izquierdo.

2. Seleccione **Agregar colaborador** en la barra de herramientas.

    [![Agregar colaborador](./media/luis-how-to-collaborate/add-collaborator.png "Add collaborator")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Escriba la dirección de correo electrónico que usa el colaborador para iniciar sesión en el portal de LUIS.

    ![Adición de la dirección de correo electrónico del colaborador](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Transferencia de propiedad

Aunque LUIS no admite actualmente la transferencia de propiedad, puede exportar la aplicación y otro usuario de LUIS puede importarla. Puede haber pequeñas diferencias en las puntuaciones de LUIS entre las dos aplicaciones. 

## <a name="azure-active-directory-resources"></a>Recursos de Azure Active Directory

Si usa [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (Azure AD) en su organización, Language Understanding (LUIS) necesita permiso para obtener acceso a la información sobre sus usuarios cuando quieran usar LUIS. Los recursos que requiere LUIS son mínimos. 

Podrá ver la descripción detallada cuando intente registrarse con una cuenta que tenga consentimiento del administrador o no requiera consentimiento del administrador: Ejemplos de consentimiento del administrador serían los siguientes:

* Permite iniciar sesión en la aplicación con su cuenta profesional y dejar que la aplicación lea su perfil. También permite que la aplicación lea información básica de la empresa. Esta opción concede permiso de LUIS para leer datos de perfil básicos, como el id. de usuario, el correo electrónico o el nombre.
* Permite que la aplicación vea y actualice los datos, incluso cuando no se esté usando la aplicación. El permiso es necesario para actualizar el token de acceso del usuario.


## <a name="azure-active-directory-tenant-user"></a>Usuario inquilino de Azure Active Directory

LUIS usa el flujo de consentimiento de Azure Active Directory (Azure AD) estándar. 

El administrador de inquilinos debe trabajar directamente con el usuario que requiera que se le conceda acceso para usar LUIS en Azure AD. 

* En primer lugar, el usuario inicia sesión en LUIS y ve el cuadro de diálogo emergente que necesita aprobación de administrador. El usuario se pone en contacto con el administrador de inquilinos antes de continuar. 
* En segundo lugar, el administrador de inquilinos inicia sesión en LUIS y ve un cuadro de diálogo emergente de flujo de consentimiento. Este es el cuadro de diálogo que debe usar el administrador para conceder permiso al usuario. Una vez que el administrador acepte el permiso, el usuario podrá continuar con LUIS. Si el administrador de inquilinos no inicia sesión en LUIS, el administrador puede obtener acceso a la opción de [consentimiento](https://account.activedirectory.windowsazure.com/r#/applications) de LUIS, que se muestra en la siguiente captura de pantalla. Tenga en cuenta que la lista tiene filtrados los elementos que incluyen el nombre `LUIS`.

![Permiso de Azure Active Directory por sitio web de aplicación](./media/luis-how-to-collaborate/tenant-permissions.png)

Si el administrador de inquilinos desea solo que determinados usuarios puedan usar LUIS, hay un par de soluciones posibles:
* dar el "consentimiento del administrador" (dar el consentimiento a todos los usuarios de Azure AD), pero, luego, establecer "Asignación de usuarios necesaria" en "Sí", en Propiedades de la aplicación de empresa y, por último, asignar o agregar solo los usuarios deseados a la aplicación. Con este método, el administrador aún está proporcionando "consentimiento del administrador" a la aplicación; sin embargo, es posible controlar los usuarios que pueden acceder a ella.
* Una segunda solución es usar la [API de Azure AD Graph](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) para proporcionar consentimiento a cada usuario específico. 

Más información sobre el consentimiento y los usuarios de Azure Active Directory: 
* [Restricción de la aplicación](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) a un conjunto de usuarios

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Cuentas de usuario con varios mensajes de correo electrónico para colaboradores

Si agrega colaboradores a una aplicación de LUIS, especifique la dirección de correo electrónico exacta que necesita un colaborador para usar LUIS como colaborador. Si bien Azure Active Directory (Azure AD) permite que un solo usuario tenga más de una cuenta de correo electrónico intercambiables, LUIS requiere que el usuario inicie sesión con la dirección de correo electrónico especificada en la lista de colaboradores.

