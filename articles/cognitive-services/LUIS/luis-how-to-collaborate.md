---
title: Colaborar con otros colaboradores en aplicaciones de LUIS en Azure | Microsoft Docs
description: Obtenga información acerca de cómo colaborar con otros colaboradores en aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/31/2018
ms.author: diberry
ms.openlocfilehash: 99f37cb6dc5e05fc5eb4bde09685435ee57fecc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397795"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Administración de creadores y colaboradores 

Puede colaborar con otros usuarios en su aplicación de LUIS. 

## <a name="owner-and-collaborators"></a>Propietario y colaboradores

Una aplicación tiene un único creador, el propietario, pero puede tener muchos colaboradores. 

## <a name="add-collaborator"></a>Agregar un colaborador

Para permitir a los colaboradores editar su aplicación de LUIS, en la página **Configuración** de la aplicación de LUIS, escriba el correo electrónico del colaborador y haga clic en **Add collaborator** (Agregar colaborador). Los colaboradores pueden iniciar sesión y editar su aplicación de LUIS mientras usted trabaja en la aplicación.

![Agregar un colaborador](./media/luis-how-to-collaborate/add-collaborator.png)

## <a name="transfer-of-ownership"></a>Transferencia de propiedad

Aunque LUIS no admite actualmente la transferencia de propiedad, puede exportar la aplicación y otro usuario de LUIS puede importarla. Puede haber pequeñas diferencias en las puntuaciones de LUIS entre las dos aplicaciones. 

## <a name="azure-active-directory-tenant-user"></a>Usuario inquilino de Azure Active Directory

LUIS usa el flujo de consentimiento de Azure Active Directory (Azure AD) estándar. 

El administrador de inquilinos debe trabajar directamente con el usuario que requiera que se le conceda acceso para usar LUIS en Azure AD. 

En primer lugar, el usuario inicia sesión en LUIS y ve el cuadro de diálogo emergente que necesita aprobación de administrador. El usuario se pone en contacto con el administrador de inquilinos antes de continuar. 

En segundo lugar, el administrador de inquilinos inicia sesión en LUIS y ve un cuadro de diálogo emergente de flujo de consentimiento. Este es el cuadro de diálogo que debe usar el administrador para conceder permiso al usuario. Una vez que el administrador acepte el permiso, el usuario podrá continuar con LUIS.

Si el administrador de inquilinos no inicia sesión en LUIS, el administrador puede acceder a [consentimiento](https://account.activedirectory.windowsazure.com/r#/applications) para LUIS. 

![Permiso de Azure Active Directory por sitio web de aplicación](./media/luis-how-to-account-settings/tenant-permissions.png)

Si el administrador de inquilinos desea que solo determinados usuarios puedan usar LUIS, consulte este [blog sobre identidad](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Cuentas de usuario con varios mensajes de correo electrónico para colaboradores

Si agrega colaboradores a una aplicación de LUIS, especifique la dirección de correo electrónico exacta que necesita un colaborador para usar LUIS como colaborador. Si bien Azure Active Directory (Azure AD) permite que un solo usuario tenga más de una cuenta de correo electrónico intercambiables, LUIS requiere que el usuario inicie sesión con la dirección de correo electrónico especificada en la lista de colaboradores.