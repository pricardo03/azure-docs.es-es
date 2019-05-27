---
title: 'Directiva de línea base exigencia de MFA para la administración de servicio (versión preliminar): Azure Active Directory'
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
ms.openlocfilehash: 00ed40bef3f3cbe59825f546ffa39c3ebfb2e41f
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003182"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Directiva de línea de base: Requerir MFA para la administración de servicio (versión preliminar)

Puede que esté usando una variedad de servicios de Azure en su organización. Estos servicios se pueden administrar a través de API de Azure Resource Manager:

* Azure Portal
* Azure PowerShell
* Azure CLI

Con Azure Resource Manager para administrar los servicios es una acción con privilegios elevados. Azure Resource Manager se puede modificar las configuraciones de todos los inquilinos, como la configuración de servicio y facturación de suscripción. Autenticación de factor único es vulnerable a una variedad de ataques, como aerosol "phishing" y la contraseña. Por lo tanto, es importante comprobar la identidad de los usuarios que deseen tener acceso a Azure Resource Manager y actualizar configuraciones, al requerir autenticación multifactor antes de permitir el acceso.

**Requerir MFA para la administración de servicio** es un [directiva de línea base](concept-baseline-protection.md) que requiere MFA para cualquier usuario tenga acceso a Azure portal, Azure PowerShell o CLI de Azure. Esta directiva se aplica a todos los usuarios tener acceso a Azure Resource Manager, independientemente de si son administradores.

Una vez que esta directiva está habilitada en un inquilino, todos los usuarios iniciar sesión en los recursos de administración de Azure se se enfrentan al desafío de autenticación multifactor. Si el usuario no está registrado para MFA, le pedirá al usuario al registrarse con la aplicación Microsoft Authenticator para poder continuar.

![Requerir MFA para Azure Resource Manager](./media/howto-baseline-protect-azure/baseline-policy-require-mfa-for-service-management.png)

Para realizar interactivo inicio de sesión mediante [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps), utilice el [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet.

```PowerShell
Connect-AzAccount
```

Cuando se ejecuta, este cmdlet presentará una cadena de token. Para iniciar sesión, copie esta cadena y pegarlos en [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) en un explorador. La sesión de PowerShell se autenticará para conectarse a Azure.

Para realizar interactivo inicio de sesión mediante [CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest), ejecute el [inicio de sesión de az](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) comando.

```azurecli
az login
```

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. En caso contrario, deberá abrir una página del explorador y siga las instrucciones de la línea de comandos que escriba un código de autorización después de navegar a [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) en el explorador. Después, inicie sesión con sus credenciales de cuenta en el explorador.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que el **exigencia de MFA para la administración de servicio** directiva se aplica a todos los usuarios de Azure Resource Manager, varias consideraciones deben realizarse para garantizar una implementación sin problemas. Estas consideraciones son la identificación de usuarios y entidades de servicio de Azure AD que no se puede o no se debe realizar la MFA, así como las aplicaciones y los clientes usados por la organización que no admiten la autenticación moderna.

### <a name="user-exclusions"></a>Exclusiones de usuario

Esta directiva de línea de base proporciona la opción de excluir a los usuarios. Antes de habilitar la directiva para el inquilino, se recomienda excluir las cuentas siguientes:

* **Acceso de emergencia** o **emergencia** cuentas para evitar el bloqueo de cuentas de todos los inquilinos. En el improbable caso de que todos los administradores quedan bloqueados fuera de su inquilino, la cuenta administrativa de acceso de emergencia puede usarse para iniciar sesión en los pasos de inquilinos take para recuperar el acceso.
   * Se puede encontrar más información en el artículo, [administrar cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Las cuentas de servicio** y **principios de servicio**, como la cuenta Azure AD Connect Sync. Cuentas de servicio son cuentas no interactivas que no están asociadas a un usuario concreto. Normalmente se utilizan por servicios back-end y permitir el acceso mediante programación a las aplicaciones. Puesto que no se puede completar mediante programación MFA, se deben excluir las cuentas de servicio.
   * Si su organización tiene estas cuentas en uso en scripts o código, considere la posibilidad de reemplazarlos con [administra identidades](../managed-identities-azure-resources/overview.md). Como solución temporal, puede excluir estas cuentas específicas de la directiva de línea de base.
* Usuarios que no tiene o no podrá usar un Smartphone.
   * Esta directiva requiere que los usuarios deben registrarse en MFA mediante la aplicación Microsoft Authenticator.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de línea base

La directiva **directiva de línea base: Requerir MFA para la administración de servicio (versión preliminar)** viene previamente configurada y se mostrarán en la parte superior cuando vaya a la hoja de acceso condicional en Azure portal.

Para habilitar esta directiva y proteger a los administradores:

1. Inicie sesión en el **portal Azure** como administrador global, Administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **acceso condicional**.
1. En la lista de directivas, seleccione **directiva de línea base: Requerir MFA para la administración de servicio (versión preliminar)**.
1. Establecer **Habilitar directiva** a **usar la directiva inmediatamente**.
1. Agregar exclusiones de cualquier usuario haciendo clic en **usuarios** > **Seleccionar usuarios excluidos** y elegir los usuarios que deben excluirse. Haga clic en **seleccione** , a continuación, **realiza**.
1. Haga clic en **guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de protección de acceso condicional básicas](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)