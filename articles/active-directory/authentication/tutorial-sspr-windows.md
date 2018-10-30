---
title: Autoservicio de restablecimiento de contraseña de Azure AD desde la pantalla de inicio de sesión de Windows 10
description: En este tutorial va a habilitar el restablecimiento de contraseña en la pantalla de inicio de sesión de Windows 10 para reducir el número de llamadas al departamento de soporte técnico.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 79a6636043499cffb7eded409cdc27c56de98e33
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430228"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Tutorial: Restablecimiento de contraseña de Azure AD desde la pantalla de inicio de sesión

En este tutorial va a permitir a los usuarios restablecer sus contraseñas desde la pantalla de inicio de sesión de Windows 10. Con la nueva actualización de Windows 10 de abril de 2018, los usuarios con dispositivos **unidos a Azure AD** o **unidos a Azure AD híbrido** pueden usar un vínculo "Restablecer contraseña" en la pantalla de inicio de sesión. Al hacer clic en este vínculo, se incorporan a la misma experiencia de autoservicio de restablecimiento de contraseña (SSPR) con la que están familiarizados.

> [!div class="checklist"]
> * Configuración del vínculo de restablecimiento de contraseña con Intune
> * Configuración opcional mediante el registro de Windows
> * Sepa lo que sus usuarios verán.

## <a name="prerequisites"></a>Requisitos previos

* Actualización de Windows del 10 de abril de 2018 o un cliente más reciente que esté:
   * [Unido a Azure AD](../device-management-azure-portal.md) o 
   * [Unido a Azure AD híbrido](../device-management-hybrid-azuread-joined-devices-setup.md)
* El autoservicio de restablecimiento de contraseña de Azure AD debe estar habilitado.

## <a name="configure-reset-password-link-using-intune"></a>Configuración del vínculo de restablecimiento de contraseña con Intune

La implementación de cambios de configuración para habilitar el restablecimiento de contraseña desde la pantalla de inicio de sesión mediante Intune es el método más flexible. Intune le permite implementar el cambio de configuración en un grupo específico de máquinas que defina. Este método requiere la inscripción en Intune del dispositivo.

### <a name="create-a-device-configuration-policy-in-intune"></a>Creación de una directiva de configuración de dispositivo en Intune

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Intune**.
2. Cree un nuevo perfil de configuración de dispositivo; para ello, vaya a **Configuración del dispositivo** > **Perfiles** > **Crear perfil**
   * Escriba un nombre descriptivo para el perfil
   * Opcionalmente, proporcione una descripción detallada del perfil
   * Plataforma **Windows 10 y versiones posteriores**
   * Tipo de perfil **Personalizado**

3. Configuración de los **valores**
   * **Agregue** la siguiente configuración de OMA-URI para habilitar el vínculo de restablecimiento de contraseña
      * Proporcione un nombre descriptivo para explicar lo que hace el valor de configuración
      * Opcionalmente, proporcione una descripción detallada del valor
      * **OMA-URI** establecido en `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **Tipo de datos** establecido en **Entero**
      * **Valor** establecido en **1**
      * Haga clic en **Aceptar**
   * Haga clic en **Aceptar**
4. Haga clic en **Crear**

### <a name="assign-a-device-configuration-policy-in-intune"></a>Asignación de una directiva de configuración de dispositivo en Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Cree un grupo para aplicar la directiva de configuración de dispositivo y haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Azure Active Directory**.
2. Vaya a **Usuarios y grupos** > **Todos los grupos** > **Nuevo grupo**
3. Proporcione un nombre para el grupo y, en **Tipo de pertenencia** elija **Asignado**
   * En **Miembros**, elija los dispositivos con Windows 10 unidos a Azure AD a los que desea aplicar la directiva.
   * Haga clic en **Seleccionar**
4. Haga clic en **Crear**

Para más información sobre cómo crear grupos, consulte el artículo [Administración del acceso a recursos con grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Asignación de la directiva de configuración de dispositivo al grupo de dispositivos

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Intune**.
2. Identifique el perfil de configuración de dispositivo creado anteriormente; para ello, vaya a **Configuración del dispositivo** > **Perfiles** > haga clic en el perfil que creó anteriormente
3. Asigne el perfil a un grupo de dispositivos 
   * Haga clic en **Asignaciones** > en **Incluir** > **Seleccionar grupos para incluir**
   * Seleccione el grupo que creó anteriormente y haga clic en **Seleccionar**
   * Haga clic en **Guardar**

   ![Asignación][Assignment]

Ahora ha creado y asignado una directiva de configuración de dispositivo para habilitar el vínculo de restablecimiento de contraseña en la pantalla de inicio de sesión con Intune.

## <a name="configure-reset-password-link-using-the-registry"></a>Configuración del vínculo de restablecimiento de contraseña con el registro

1. Inicie sesión en el equipo Windows con credenciales administrativas
2. Ejecute **regedit** como administrador
3. Establezca la siguiente clave del Registro
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Ahora que la directiva está configurada y asignada, ¿qué cambia para el usuario? ¿Cómo sabe que puede restablecer su contraseña en la pantalla de inicio de sesión?

![LoginScreen][LoginScreen]

Cuando los usuarios intenten iniciar sesión verán un vínculo de restablecimiento de contraseña que abre la experiencia de autoservicio de restablecimiento de contraseña en la pantalla de inicio de sesión. Esta funcionalidad permite a los usuarios restablecer su contraseña sin tener que usar otro dispositivo para acceder a un explorador web.

Los usuarios encontrarán instrucciones para usar esta característica en el artículo sobre el [restablecimiento de la contraseña profesional o educativa](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)

## <a name="common-issues"></a>Problemas comunes

Al probar esta funcionalidad con Hyper-V, el vínculo "Restablecer contraseña" no aparece.

* Vaya a la máquina virtual que usa para la prueba, haga clic en **Ver** y desactive la opción **Sesión mejorada**.

Al probar esta funcionalidad con Escritorio remoto, el vínculo "Restablecer contraseña" no aparece.

* El restablecimiento de contraseña no se admite actualmente desde Escritorio remoto.

Si se ha deshabilitado la pantalla de bloqueo al usar una clave del Registro de Windows o una directiva de grupo, **Restablecer contraseña** no estará disponible.

Si la directiva requiere Ctrl + Alt + Supr o si las notificaciones de pantalla de bloqueo están desactivadas, **Restablecer contraseña** no funcionará. Windows 10 19H 1 hará que se cumpla este requisito.

El registro de auditoría de Azure AD incluirá información sobre la dirección IP y el ClientType donde se produjo el restablecimiento de contraseña.

![Ejemplo de pantalla de inicio de sesión con restablecimiento de contraseña en el registro de auditoría de Azure AD](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

Si las máquinas de Windows 10 se encuentran detrás de un servidor proxy o firewall, se debe permitir el tráfico HTTPS (443) para passwordreset.microsoftonline.com y ajax.aspnetcdn.com.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si decide que ya no desea utilizar la funcionalidad que se ha configurado como parte de este tutorial, elimine el perfil de configuración del dispositivo Intune que ha creado o la clave del registro.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha permitido a los usuarios restablecer sus contraseñas desde la pantalla de inicio de sesión de Windows 10. Continúe con el siguiente tutorial para ver cómo se puede integrar Azure Identity Protection en las experiencias de Multi-Factor Authentication y autoservicio de restablecimiento de contraseñas.

> [!div class="nextstepaction"]
> [Evaluación del riesgo en el inicio de sesión](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Asignación de la directiva de configuración de dispositivo de Intune a un grupo de dispositivos con Windows 10"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Vínculo de restablecimiento de contraseña en la pantalla de inicio de sesión de Windows 10"
