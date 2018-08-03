---
title: Administración de la configuración de su cuenta en LUIS | Microsoft Docs
description: Utilice el sitio web de LUIS para administrar la configuración de su cuenta.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 963a7f8c196702ea899ddfe31e6187a15eb5f683
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223215"
---
# <a name="manage-account-and-authoring-key"></a>Administración de cuenta y clave de creación
Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. La información de inicio de sesión se administra en [account.microsoft.com](https://account.microsoft.com). La clave de creación se administra en la página **Settings** (Configuración) del sitio web de [LUIS](luis-reference-regions.md). 

## <a name="authoring-key"></a>Clave de creación

Esta clave de creación única y específica de la región, de la página **Settings** (Configuración), le permite crear todas sus aplicaciones desde el sitio web de [LUIS](luis-reference-regions.md) y las [API de creación](https://aka.ms/luis-authoring-api). Para su comodidad, la clave de creación puede realizar un número [limitado](luis-boundaries.md) de consultas de punto de conexión al mes. 

![Página de configuración de LUIS](./media/luis-how-to-account-settings/account-settings.png)

La clave de creación se usa para cualquier aplicación de su propiedad, así como para cualquier aplicación en la que aparezca como colaborador.

## <a name="authoring-key-regions"></a>Regiones de las claves de creación
La clave de creación es específica de la [región de creación](luis-reference-regions.md#publishing-regions). La clave no funciona en una región distinta. 

## <a name="reset-authoring-key"></a>Restablecimiento de la clave de creación
Si su clave de creación está en peligro, restablézcala. La clave se restablece en todas las aplicaciones en el sitio web de [LUIS](luis-reference-regions.md). Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de `Ocp-Apim-Subscription-Key` por la clave nueva. 

## <a name="delete-account"></a>Eliminación de cuenta
Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta. 

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


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [clave de creación](luis-concept-keys.md#authoring-key). 

