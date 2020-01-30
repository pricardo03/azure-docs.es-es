---
title: Configuración del registro y del inicio de sesión con una cuenta de Amazon
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Amazon en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b22c4fb8f5c54437281e90a74032e8ee1d05bcb8
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850371"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Amazon mediante Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Creación de una aplicación de Amazon

Para usar una cuenta de Amazon como [proveedor de identidades](authorization-code-flow.md) en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de Amazon, puede suscribirse en [https://www.amazon.com/](https://www.amazon.com/).

1. Inicie sesión en [Amazon Developer Center](https://login.amazon.com/) con las credenciales de su cuenta de Amazon.
1. Si aún no lo ha hecho, haga clic en **Sign up**(Registro), siga los pasos de registro para desarrolladores y acepte la directiva.
1. Seleccione **Register new application**(Registrar nueva aplicación).
1. Escriba la información de **Name** (Nombre), **Description** (Descripción) y **Privacy Notice URL** (Dirección URL de aviso de privacidad) y, a continuación, haga clic en **Save** (Guardar). El aviso de privacidad es una página administrada por el usuario que proporciona información de privacidad a los usuarios.
1. En la sección **Web Settings** (Configuración web), copie los valores de **Client ID** (Identificador de cliente). Seleccione **Show secret** (Mostrar secreto) para obtener el secreto de cliente y, a continuación, cópielo. Necesitará los dos para configurar la cuenta de Amazon como proveedor de identidades de su inquilino. **secreto de cliente** es una credencial de seguridad importante.
1. En la sección **Web Settings** (Configuración Web), seleccione **Edit** (Editar) y, a continuación, escriba `https://your-tenant-name.b2clogin.com` en **Allowed JavaScript Origins** (Orígenes de JavaScript permitidos) y `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Allowed Return URLs** (Direcciones URL de devolución permitidas). Reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
1. Haga clic en **Save**(Guardar).

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Configuración de una cuenta de Amazon como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Amazon**.
1. Escriba un **nombre**. Por ejemplo, *Amazon*.
1. En **Id. de cliente**, escriba el identificador de cliente de Amazon que ha creado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado.
1. Seleccione **Guardar**.
