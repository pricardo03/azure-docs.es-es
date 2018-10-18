---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de LinkedIn mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de LinkedIn en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1719da96a849bb5390745ec3df3ed11374bb8700
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47180502"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de LinkedIn mediante Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Creación de una aplicación de LinkedIn

Para usar una cuenta de LinkedIn como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de LinkedIn, puede obtenerla en [https://www.linkedin.com/](https://www.linkedin.com/).

1. Inicie sesión en el [sitio web para desarrolladores de LinkedIn](https://www.developer.linkedin.com/) con las credenciales de su cuenta de LinkedIn.
2. Seleccione **My Apps** (Mis aplicaciones) y, a continuación, haga clic en **Create Application** (Crear aplicación).
3. Escriba la información de **Company Name** (Nombre de la compañía), **Application Name** (Nombre de la aplicación), **Application Description** (Descripción de la aplicación), **Application Logo** (Logotipo de la aplicación), **Application Use** (Uso de la aplicación), **Website URL** (Dirección URL del sitio web), **Business Email** (Correo electrónico de la empresa) y **Business Phone** (Teléfono de la empresa).
4. Acepte las **condiciones de uso de API de LinkedIn** y haga clic en **Submit** (Enviar).
5. Copie los valores de **Client ID** y **Client Secret**. Los encontrará en **Authentication Keys** (Claves de autenticación). Necesitará ambos para configurar LinkedIn como proveedor de identidades de su inquilino. **secreto de cliente** es una credencial de seguridad importante.
6. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Authorized Redirect URLs** (Direcciones URL de redirección autorizadas). Reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C. Seleccione **Add** (Agregar) y después haga clic en **Update** (Actualizar).

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Configuración de una cuenta de LinkedIn como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *LinkedIn*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), seleccione **LinkedIn** y haga clic en **Aceptar**.
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de cliente que anotó anteriormente como el valor de **Client ID**, y el secreto de cliente que registró como **Client Secret** de la aplicación de LinkedIn que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y en **Create** (Crear) para guardar la configuración de la cuenta de LinkedIn.

