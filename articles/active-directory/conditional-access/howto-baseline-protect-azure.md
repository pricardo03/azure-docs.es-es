---
title: 'Directiva de base de referencia Requerir MFA para la administración de servicios (versión preliminar): Azure Active Directory'
description: Directiva de acceso condicional para requerir MFA para Azure Resource Manager
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: aab2aa4415345747a0e87b90ef0a7ee770ef3465
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608125"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Directiva de base de referencia: Requerir MFA para la administración de servicios (versión preliminar)

Puede que esté usando una variedad de servicios de Azure en su organización. Estos servicios se pueden administrar a través de la API de Azure Resource Manager:

* Portal de Azure
* Azure PowerShell
* CLI de Azure

El uso de Azure Resource Manager para administrar los servicios es una acción con privilegios elevados. Azure Resource Manager puede modificar las configuraciones de todo el inquilino, como la configuración del servicio y la facturación de la suscripción. La autenticación de factor único es vulnerable a una variedad de ataques, como la suplantación de identidad (phishing) y la difusión de contraseña. Por lo tanto, es importante comprobar la identidad de los usuarios que quieren acceder a Azure Resource Manager y actualizar las configuraciones. Para ello, se debe requerir la autenticación multifactor antes de permitir el acceso.

**Requerir MFA para la administración de servicios** es un [directiva de base de referencia](concept-baseline-protection.md) que requerirá MFA para que cualquier usuario acceda a Azure Portal, Azure PowerShell o la CLI de Azure. Esta directiva se aplica a todos los usuarios que acceden a Azure Resource Manager, independientemente de si son administradores.

Una vez que esta directiva está habilitada en un inquilino, todos los usuarios que inician sesión en los recursos de administración de Azure se enfrentan al desafío de la autenticación multifactor. Si el usuario no está registrado para MFA, deberá registrarse con la aplicación Microsoft Authenticator para poder continuar.

Para el inicio de sesión interactivo mediante [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps), utilice el cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) .

```PowerShell
Connect-AzAccount
```

Cuando se ejecuta, este cmdlet presentará una cadena de token. Para iniciar sesión, copie esta cadena y péguela en [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)  en un explorador. La sesión de PowerShell se autenticará para conectarse a Azure.

Para el inicio de sesión interactivo mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest), ejecute el comando  [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) .

```azurecli
az login
```

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, deberá abrir una página del explorador y seguir las instrucciones de la línea de comandos para especificar un código de autorización después de ir a [https://aka.ms/devicelogin](https://aka.ms/devicelogin) en el explorador. A continuación, inicie sesión con las credenciales de su cuenta en el explorador.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que la directiva **Requerir MFA para la administración de servicios** se aplica a todos los usuarios de Azure Resource Manager, deben realizarse varias consideraciones para garantizar una implementación sin problemas. Estas consideraciones incluyen la identificación de usuarios y entidades de servicio de Azure AD que no pueden o no deben ejecutar MFA, así como aplicaciones y clientes que se usan en la organización y que no admiten la autenticación moderna.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de base de referencia

La **Directiva de base de referencia: Requerir MFA para la administración de servicios (versión preliminar)** viene previamente configurada y se mostrará al principio cuando navegue a la hoja Acceso condicional en Azure Portal.

Para habilitar esta directiva y proteger a los administradores:

1. Inicie sesión en  **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. En la lista de directivas, seleccione **Directiva de línea base: Requerir MFA para la administración de servicios (versión preliminar)** .
1. Establezca **Habilitar directiva** en **Usar la directiva inmediatamente**.
1. Haga clic en  **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de protección de base de referencia de acceso condicional](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/fundamentals/steps-secure-identity.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)