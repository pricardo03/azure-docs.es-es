---
title: ¿Cuándo y cómo usar un proveedor de Azure Multi-factor Authentication?
description: ¿Cuándo se debe usar un proveedor de autenticación con Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: b601a3d23b23faa16925881a54e2ceba85c800f8
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669072"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Cuándo usar un proveedor de Azure Multi-Factor Authentication

La verificación en dos pasos está disponible de forma predeterminada para los administradores globales que tienen usuarios de Azure Active Directory y Office 365. Sin embargo, si desea aprovechar las [características avanzadas](howto-mfa-mfasettings.md), debe adquirir la versión completa de Azure Multi-Factor Authentication (MFA).

Un proveedor de Azure Multi-Factor Authentication se usa para aprovechar las características que Azure Multi-factor Authentication proporciona para los usuarios que **no tienen licencias**. 

Si tiene licencias que cubren a todos los usuarios de la organización, no necesita un Proveedor de Azure Multi-Factor Authentication. Solo cree un proveedor de Azure Multi-Factor Authentication si también tiene que ptoporcionar verificación en dos pasos para algunos usuarios que no tienen licencia.

> [!NOTE]
> A partir del 1 de septiembre de 2018, no se pueden crear nuevos clientes de autenticación. Los proveedores de autenticación existentes se pueden seguir usando y actualizando. La autenticación multifactor seguirá siendo una característica disponible en las licencias de Azure AD Premium.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Advertencias relacionadas con el SDK de Azure MFA

Se necesita un proveedor de Azure Multi-Factor Authentication para descargar el SDK. Tenga en cuenta que el SDK ha quedado en desuso y ya no se admite para los nuevos clientes y solo continuará funcionando hasta el 14 de noviembre de 2018. Después de ese momento, se producirá un error en las llamadas al SDK.

Para descargar el SDK, cree un proveedor de Azure Multi-Factor Authentication aunque tenga licencias de Azure MFA, AAD Premium u otras licencias agrupadas. Si crea un proveedor de Azure Multi-Factor Authentication para este propósito y ya tiene licencias, asegúrese de crear el proveedor con el modelo **Por usuario habilitado**. A continuación, vincule el proveedor al directorio que contiene las licencias de Azure MFA, Azure AD Premium u otras licencias agrupadas. Gracias a esta configuración, se asegura de que no se le cobrará, salvo que tenga más usuarios únicos ejecutando la verificación en dos pasos que el número de licencias que posee.

## <a name="what-is-an-mfa-provider"></a>¿Qué es un proveedor de MFA?

Si no tiene licencias de Azure Multi-Factor Authentication, puede crear un proveedor de autenticación que exija verificación en dos pasos para los usuarios.

Hay dos tipos de proveedores de autenticación y la diferencia radica en cómo se aplicarán cargos a su suscripción de Azure. La opción "por autenticación" calcula el número de autenticaciones que se realizan en el inquilino en un mes. Esta opción es la más adecuada si tiene un número de usuarios que se autentican solo en algunas ocasiones. La opción "por usuario" calcula el número de personas en el inquilino que realizan la verificación en dos pasos en un mes. Esta opción es mejor si tiene algunos usuarios con licencias pero necesita ampliar MFA a más usuarios de los permitidos por su licencia.

## <a name="create-an-mfa-provider"></a>Creación de un proveedor de MFA

Use los pasos siguientes para crear un proveedor de Azure Multi-Factor Authentication en Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global.
2. Seleccione **Azure Active Directory** > **MFA Server** (Servidor MFA)  > **Proveedores**.

   ![Proveedores][Providers]

3. Seleccione **Agregar**.
4. Rellene los campos siguientes y seleccione **Agregar**.
   - **Nombre**: el nombre del proveedor.
   - **Modelo de uso**: puede elegir una de las dos opciones a continuación:
      * Por autenticación: modelo de adquisición que se carga por autenticación. Normalmente se usa para escenarios que utilizan Azure Multi-Factor Authentication en una aplicación orientada al consumidor.
      * Por usuario habilitado: modelo de adquisición que se carga por usuario habilitado. Suele utilizarse para el acceso de los empleados a aplicaciones como Office 365. Elija esta opción si tiene algunos usuarios que ya tienen una licencia de Azure MFA.
   - **Suscripción**: la suscripción de Azure en la que se cobra la actividad de verificación en dos pasos a través del proveedor.
   - **Directorio**: el inquilino de Azure Active Directory al que está asociado el proveedor.
      * No es necesario tener un directorio de Azure AD para crear un proveedor. Si solo planea descargar el Servidor Azure Multi-Factor Authentication, deje este cuadro en blanco.
      * El proveedor debe estar asociado a un directorio de Azure AD para aprovechar las características avanzadas.
      * Solo un proveedor puede estar asociado a un directorio de Azure AD.

## <a name="manage-your-mfa-provider"></a>Administración del proveedor de MFA

No se puede cambiar el modelo de uso (por usuario habilitado o por autenticación) después de crear un proveedor de MFA. Sin embargo, puede eliminar el proveedor de MFA y crear otro con un modelo de uso distinto.

Si el proveedor de Multi-Factor Authentication actual está asociado a un directorio de Azure AD (también conocido como un inquilino de Azure AD), puede eliminar el proveedor de MFA con seguridad y crear uno que esté vinculado al mismo inquilino de Azure AD. Como alternativa, si compró licencias suficientes para cubrir todos los usuarios que están habilitados para MFA, puede eliminar por completo el proveedor de MFA.

Si el proveedor de MFA no está vinculado a un inquilino de Azure AD o si vincula el nuevo proveedor de MFA a un inquilino de Azure AD diferente, las opciones de configuración y parámetros de usuario no se transferirán. Además, los servidores Azure MFA se tendrán que reactivar mediante las credenciales de activación generadas a través del nuevo proveedor de MFA. La reactivación de los servidores MFA para vincularlos al nuevo proveedor de MFA no afecta a la autenticación por llamada telefónica y mensaje de texto, pero las notificaciones de aplicación móvil dejarán de funcionar para todos los usuarios hasta que se reactive la aplicación móvil.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "Agregar proveedores de MFA"
