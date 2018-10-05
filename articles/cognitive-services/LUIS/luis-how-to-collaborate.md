---
title: Colaboración con otros colaboradores en aplicaciones de LUIS
titleSuffix: Azure Cognitive Services
description: El propietario de una aplicación puede agregar colaboradores a la aplicación. Estos colaboradores pueden modificar el modelo, entrenar y publicar la aplicación.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: b7d108bfc0c4283e7856b93daba3f4f92af4cc5b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042200"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Administración de creadores y colaboradores 

El propietario de una aplicación puede agregar colaboradores a la aplicación. Estos colaboradores pueden modificar el modelo, entrenar y publicar la aplicación. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Agregar un colaborador

Una aplicación tiene un único creador, el propietario, pero puede tener muchos colaboradores. Para permitir que los colaboradores editen la aplicación de LUIS, debe agregar el correo electrónico que usan para acceder al portal de LUIS a la lista de colaboradores. Una vez agregados, la aplicación se muestra en su portal de LUIS.

1. Seleccione **Administrar** en el menú superior derecho y, a continuación, seleccione **Colaboradores** en el menú izquierdo.

2. Seleccione **Agregar colaborador** en la barra de herramientas.

    [![](./media/luis-how-to-collaborate/add-collaborator.png "Agregar un colaborador")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Escriba la dirección de correo electrónico que usa el colaborador para iniciar sesión en el portal de LUIS.

    ![Adición de la dirección de correo electrónico del colaborador](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Transferencia de propiedad

Aunque LUIS no admite actualmente la transferencia de propiedad, puede exportar la aplicación y otro usuario de LUIS puede importarla. Puede haber pequeñas diferencias en las puntuaciones de LUIS entre las dos aplicaciones. 

## <a name="azure-active-directory-resources"></a>Recursos de Azure Active Directory

Si usa Azure Active Directory (Azure AD) en su organización, LUIS necesita permiso para obtener acceso a la información sobre sus usuarios cuando quieran usar LUIS. Los recursos que requiere LUIS son mínimos. 

Podrá ver la descripción detallada cuando intente registrarse con una cuenta que tenga consentimiento del administrador o no requiera consentimiento del administrador: Ejemplos de consentimiento del administrador serían los siguientes:

* Permite iniciar sesión en la aplicación con su cuenta profesional y dejar que la aplicación lea su perfil. También permite que la aplicación lea información básica de la empresa.
* Permite que la aplicación vea y actualice los datos, incluso cuando no se esté usando la aplicación.

El primer permiso concede permiso de LUIS para leer datos de perfil básico, como el identificador de usuario, el correo electrónico o el nombre. El segundo permiso es necesario para actualizar el token de acceso del usuario.

## <a name="azure-active-directory-tenant-user"></a>Usuario inquilino de Azure Active Directory

LUIS usa el flujo de consentimiento de Azure Active Directory (Azure AD) estándar. 

El administrador de inquilinos debe trabajar directamente con el usuario que requiera que se le conceda acceso para usar LUIS en Azure AD. 

En primer lugar, el usuario inicia sesión en LUIS y ve el cuadro de diálogo emergente que necesita aprobación de administrador. El usuario se pone en contacto con el administrador de inquilinos antes de continuar. 

En segundo lugar, el administrador de inquilinos inicia sesión en LUIS y ve un cuadro de diálogo emergente de flujo de consentimiento. Este es el cuadro de diálogo que debe usar el administrador para conceder permiso al usuario. Una vez que el administrador acepte el permiso, el usuario podrá continuar con LUIS.

Si el administrador de inquilinos no inicia sesión en LUIS, el administrador puede acceder a [consentimiento](https://account.activedirectory.windowsazure.com/r#/applications) para LUIS. 

![Permiso de Azure Active Directory por sitio web de aplicación](./media/luis-how-to-collaborate/tenant-permissions.png)

Si el administrador de inquilinos desea que solo determinados usuarios puedan usar LUIS, consulte este [blog sobre identidad](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Cuentas de usuario con varios mensajes de correo electrónico para colaboradores

Si agrega colaboradores a una aplicación de LUIS, especifique la dirección de correo electrónico exacta que necesita un colaborador para usar LUIS como colaborador. Si bien Azure Active Directory (Azure AD) permite que un solo usuario tenga más de una cuenta de correo electrónico intercambiables, LUIS requiere que el usuario inicie sesión con la dirección de correo electrónico especificada en la lista de colaboradores.

