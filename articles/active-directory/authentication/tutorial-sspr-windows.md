---
title: Autoservicio de restablecimiento de contraseña de Azure AD desde la pantalla de inicio de sesión de Windows 10
description: En este tutorial va a habilitar el restablecimiento de contraseña en la pantalla de inicio de sesión de Windows 10 para reducir el número de llamadas al departamento de soporte técnico.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: c84d876828ac96bfb44b84e99b13489d51ae3370
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694030"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Tutorial: Restablecimiento de la contraseña de Azure AD desde la pantalla de inicio de sesión

En este tutorial va a permitir a los usuarios restablecer sus contraseñas desde la pantalla de inicio de sesión de Windows 10. Con la nueva actualización de Windows 10 de abril de 2018, los usuarios con dispositivos **unidos a Azure AD** o **unidos a Azure AD híbrido** pueden usar un vínculo "Restablecer contraseña" en la pantalla de inicio de sesión. Al hacer clic en este vínculo, se incorporan a la misma experiencia de autoservicio de restablecimiento de contraseña (SSPR) con la que están familiarizados.

> [!div class="checklist"]
> * Configuración del vínculo de restablecimiento de contraseña con Intune
> * Configuración opcional mediante el registro de Windows
> * Sepa lo que sus usuarios verán.

## <a name="prerequisites"></a>Requisitos previos

* Debe ejecutar al menos la actualización de abril de 2018 de Windows 10 y los dispositivos deben estar:
   * [Unidos a Azure AD](../device-management-azure-portal.md) o
   * [Unidos a Azure AD híbrido](../device-management-hybrid-azuread-joined-devices-setup.md), con conectividad de red a un controlador de dominio.
* Debe habilitar el autoservicio de restablecimiento de contraseña de Azure AD.
* Si los dispositivos Windows 10 se encuentran detrás de un servidor proxy o un firewall, debe agregar las direcciones URL, `passwordreset.microsoftonline.com` y `ajax.aspnetcdn.com` a la lista de direcciones URL permitidas (puerto 443) del tráfico HTTPS.
* Revise las limitaciones siguientes antes de probar esta característica en su entorno.

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

1. Inicio de sesión en el equipo Windows con credenciales administrativas
2. Ejecute **regedit** como administrador
3. Establezca la siguiente clave del Registro
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Ahora que la directiva está configurada y asignada, ¿qué cambia para el usuario? ¿Cómo sabe que puede restablecer su contraseña en la pantalla de inicio de sesión?

![LoginScreen][LoginScreen]

Cuando los usuarios intenten iniciar sesión, ahora verán un vínculo de restablecimiento de contraseña que abre la experiencia de autoservicio de restablecimiento de contraseña en la pantalla de inicio de sesión. Esta funcionalidad permite a los usuarios restablecer su contraseña sin tener que usar otro dispositivo para acceder a un explorador web.

Los usuarios encontrarán instrucciones para usar esta característica en el artículo sobre el [restablecimiento de la contraseña profesional o educativa](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)

El registro de auditoría de Azure AD incluirá información sobre la dirección IP y el ClientType donde se produjo el restablecimiento de contraseña.

![Ejemplo de pantalla de inicio de sesión con restablecimiento de contraseña en el registro de auditoría de Azure AD](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

Cuando los usuarios restablecen su contraseña desde la pantalla de inicio de sesión de un dispositivo Windows 10, se crea una cuenta temporal con pocos privilegios denominada "defaultuser1". Esta cuenta se usa para proteger el proceso de restablecimiento de contraseña. La propia cuenta tiene una contraseña generada aleatoriamente, no se muestra en el inicio de sesión del dispositivo y se elimina automáticamente después de que el usuario restablece su contraseña. Aunque pueden existir varios perfiles "defaultuser", se pueden omitir con seguridad.

## <a name="limitations"></a>Limitaciones

Al probar esta funcionalidad con Hyper-V, el vínculo "Restablecer contraseña" no aparece.

* Vaya a la máquina virtual que usa para la prueba, haga clic en **Ver** y desactive la opción **Sesión mejorada**.

Al probar esta funcionalidad con Escritorio remoto o con una sesión de máquina virtual mejorada, el vínculo "Restablecer contraseña" no aparece.

* El restablecimiento de contraseña no se admite actualmente desde Escritorio remoto.

Si en versiones de Windows 10 anteriores a la 1809 la directiva requiere Ctrl+Alt+Del, no funcionará el vínculo **Restablecer contraseña**.

Si las notificaciones de la pantalla de bloqueo están desactivadas, tampoco funcionará el vínculo **Restablecer contraseña**.

Los siguientes valores directiva se sabe que interfieren con la capacidad de restablecer contraseñas

   * HideFastUserSwitching está establecido en habilitado o 1
   * DontDisplayLastUserName está establecido en habilitado o 1
   * NoLockScreen está establecido en habilitado o 1
   * EnableLostMode se establece en el dispositivo
   * Explorer.exe se reemplaza por un shell personalizado

Esta característica no funciona para las redes con la red autenticación 802.1X implementada y la opción "Realizar inmediatamente antes de que el usuario inicie sesión". Para las redes con la autenticación de red 802.1X implementada se recomienda usar la autenticación del equipo para habilitar esta característica.

En escenarios de unión a dominios híbridos, el flujo de trabajo de SSPR se completará correctamente sin necesidad de un controlador de dominio de Active Directory. Si un usuario completa el proceso de restablecimiento de contraseña cuando la comunicación con un controlador de dominio de Active Directory no está disponible, por ejemplo, al trabajar de forma remota, el usuario no podrá iniciar sesión en el dispositivo hasta que este pueda comunicarse con un controlador de dominio y actualizar las credenciales almacenadas en caché. **Se necesita conectividad con un controlador de dominio para usar la nueva contraseña por primera vez**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si decide que ya no desea utilizar la funcionalidad que se ha configurado como parte de este tutorial, elimine el perfil de configuración del dispositivo Intune que ha creado o la clave del registro.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha permitido a los usuarios restablecer sus contraseñas desde la pantalla de inicio de sesión de Windows 10. Continúe con el siguiente tutorial para ver cómo se puede integrar Azure Identity Protection en las experiencias de Multi-Factor Authentication y autoservicio de restablecimiento de contraseñas.

> [!div class="nextstepaction"]
> [Evaluación del riesgo en el inicio de sesión](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Asignación de la directiva de configuración de dispositivo de Intune a un grupo de dispositivos con Windows 10"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Vínculo de restablecimiento de contraseña en la pantalla de inicio de sesión de Windows 10"
