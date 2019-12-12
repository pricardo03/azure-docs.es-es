---
title: 'Inicio de sesión con clave de seguridad sin contraseña en Windows: Azure Active Directory'
description: Habilitar el inicio de sesión con clave de seguridad sin contraseña para Azure AD mediante llaves de seguridad FIDO2 (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce2b420c2124c86610058ce2f31cd6d7bf620a97
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848466"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-preview"></a>Habilitación del inicio de sesión con clave de seguridad sin contraseña en dispositivos Windows 10 (versión preliminar)

Este documento se centra en la habilitación de la autenticación sin contraseña basada en claves de seguridad FIDO2 para dispositivos Windows 10. Al final de este artículo, será capaz de iniciar sesión en aplicaciones basadas en web y los dispositivos Windows 10 unidos a Azure AD con la cuenta de Azure AD mediante una clave de seguridad FIDO2.

|     |
| --- |
| Las claves de seguridad FIDO2 son una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="requirements"></a>Requisitos

- [Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
- [Registro de información de seguridad combinado (vista preliminar)](concept-registration-mfa-sspr-combined.md)
- [Claves de seguridad FIDO2](concept-authentication-passwordless.md#fido2-security-keys) compatibles
- WebAuthN requiere Windows 10 versión 1809 o superior
- [Los dispositivos unidos a Azure AD](../devices/concept-azure-ad-join.md) requieren Windows 10 versión 1809 o superior
- [Microsoft Intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (opcional)
- Paquete de aprovisionamiento (opcional)

### <a name="unsupported-scenarios"></a>Escenarios no admitidos

- **No se admite** la implementación de Windows Server Active Directory Domain Services (AD DS) unido a un dominio (solo en dispositivos locales).
- Los escenarios de RDP, VDI y Citrix **no se admiten** con la clave de seguridad.
- S/MIME **no se admite** con la clave de seguridad.
- "Ejecutar como" **no se admite** con la clave de seguridad.
- El inicio de sesión en un servidor con la clave de seguridad **no se admite**.
- Si no ha usado la clave de seguridad para iniciar sesión en el dispositivo mientras está conectado, no podrá usarla para iniciar sesión o desbloquearlo sin conexión.
- Inicie sesión o desbloquee un dispositivo de Windows 10 con una clave de seguridad que contenga varias cuentas de Azure AD. En este escenario se utilizará la última cuenta agregada a la clave de seguridad. WebAuthN permitirá a los usuarios elegir la cuenta que desean usar.

## <a name="prepare-devices-for-preview"></a>Preparar dispositivos para la versión preliminar

Los dispositivos unidos a Azure AD que va a probar en uso piloto deben ejecutar Windows 10 versión 1809 o superior. La mejor experiencia se logra con Windows 10 versión 1903 o superior.

## <a name="enable-security-keys-for-windows-sign-in"></a>Habilitación de claves de seguridad para el inicio de sesión de Windows

Las organizaciones pueden optar por usar uno o varios de los métodos siguientes para habilitar el uso de claves de seguridad para el inicio de sesión de Windows en función de sus requisitos.

- [Habilitación con Intune](#enable-with-intune)
- [Implementación de Intune dirigida](#targeted-intune-deployment)
- [Habilitación con un paquete de aprovisionamiento](#enable-with-a-provisioning-package)

### <a name="enable-with-intune"></a>Habilitación con Intune

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **Microsoft Intune** > **Inscripción de dispositivos** > **Inscripción de Windows** > **Windows Hello para empresas** > **Propiedades**.
1. En **Configuración**, establezca **Utilice las claves de seguridad para el inicio de sesión** en **Habilitado**.

La configuración de claves de seguridad para el inicio de sesión no depende de la configuración de Windows Hello para empresas.

### <a name="targeted-intune-deployment"></a>Implementación de Intune dirigida

Para dirigirse a grupos de dispositivos específicos a fin de habilitar el proveedor de credenciales, use la siguiente configuración personalizada a través de Intune.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **Microsoft Intune** > **Configuración del dispositivo** > **Perfiles** > **Crear perfil**.
1. Configure el nuevo perfil con los valores siguientes
   1. Nombre: Claves de seguridad para inicio de sesión de Windows
   1. Description: Habilita claves de seguridad FIDO que se van a usar durante el inicio de sesión de Windows
   1. Plataforma: Windows 10 y versiones posteriores
   1. Tipo de perfil: Personalizado
   1. Configuración OMA-URI personalizada:
      1. Nombre: Habilitar claves de seguridad FIDO para inicio de sesión de Windows
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Tipo de datos: Entero
      1. Valor: 1
1. Esta directiva se puede asignar a usuarios, dispositivos o grupos específicos. Se puede encontrar más información en el artículo [Asignación de perfiles de dispositivo en Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

![Creación de directiva de configuración de dispositivos personalizada de Intune](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>Habilitación con un paquete de aprovisionamiento

En el caso de los dispositivos no administrados por Intune, se puede instalar un paquete de aprovisionamiento para habilitar la funcionalidad. La aplicación Diseñador de configuración de Windows puede instalarse desde [Microsoft Store](https://www.microsoft.com/en-us/p/windows-configuration-designer/9nblggh4tx22).

1. Inicie el Diseñador de configuración de Windows.
1. Seleccione **Archivo** > **Nuevo proyecto**.
1. Asigne un nombre al proyecto y tome nota de la ruta de acceso donde se ha creado.
1. Seleccione **Next** (Siguiente).
1. Deje **Paquete de aprovisionamiento** seleccionado como **Flujo de trabajo de proyecto seleccionados** y seleccione **Siguiente**.
1. Seleccione **Todas las ediciones del escritorio de Windows** en **Elige los valores que se deben ver y configurar** y luego **Siguiente**.
1. Seleccione **Finalizar**.
1. En el proyecto recién creado, vaya a **Configuración de tiempo de ejecución** > **WindowsHelloForBusiness** > **SecurityKeys** > **UseSecurityKeyForSignIn**.
1. Establezca **UseSecurityKeyForSignIn** en **Habilitado**.
1. Seleccione **Exportar** > **Paquete de aprovisionamiento**.
1. Deje los valores predeterminados en la ventana **Compilar** en **Describe el paquete de aprovisionamiento** y seleccione **Siguiente**.
1. Deje los valores predeterminados en la ventana **Compilar** en **Selecciona los detalles de seguridad del paquete de aprovisionamiento** y seleccione **Siguiente**.
1. Anote o cambie la ruta de acceso de la ventana **Compilar** en **Selecciona el lugar donde guardar el paquete de aprovisionamiento** y seleccione **Siguiente**.
1. Seleccione **Compilar** en la página **Compilar el paquete de aprovisionamiento**.
1. Guarde los dos archivos creados (ppkg y cat) en una ubicación donde pueda aplicarlos a los equipos más adelante.
1. Siga las instrucciones del artículo [Aplicación de un paquete de aprovisionamiento](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package) para aplicar el paquete de aprovisionamiento que ha creado.

> [!NOTE]
> Los dispositivos que ejecutan Windows 10 versión 1809 también deben habilitar el modo de PC compartido (EnableSharedPCMode). Puede encontrar información sobre cómo habilitar esta funcionalidad en el artículo [Configuración de un equipo compartido o invitado con Windows 10](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc).

## <a name="sign-in-to-windows-with-a-fido2-security-key"></a>Inicio de sesión en Windows con una clave de seguridad FIDO2

En el ejemplo siguiente, el usuario Bala Sandhu ya ha aprovisionado su clave de seguridad FIDO2 mediante los pasos del artículo anterior, [Habilitación del inicio de sesión con la clave de seguridad sin contraseña](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys). Bala puede elegir el proveedor de credenciales de clave de seguridad desde la pantalla de bloqueo de Windows 10 e insertar la clave de seguridad para iniciar sesión en Windows.

![Inicio de sesión de clave de seguridad en la pantalla de bloqueo de Windows 10](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Administrar biométrica, PIN o restablecer clave de seguridad

* Windows 10 versión 1903 o posterior
   * Los usuarios pueden abrir **Configuración de Windows** en el dispositivo > **Cuentas** > **Clave de seguridad**
   * Los usuarios pueden cambiar el PIN, actualizar la biométrica o restablecer la clave de seguridad

## <a name="troubleshooting-and-feedback"></a>Solución de problemas y comentarios

Si quiere compartir comentarios o detectar problemas mientras usa la versión preliminar de esta característica, compártalos mediante la aplicación Centro de opiniones sobre Windows.

1. Abra el **Centro de opiniones** y asegúrese de que ha iniciado sesión.
1. Envíe los comentarios bajo la categorización siguiente:
   1. Categoría: Seguridad y privacidad
   1. Subcategoría: FIDO
1. Para capturar registros, use la opción: **Volver a crear mi problema**

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="does-this-work-in-my-on-premises-environment"></a>¿Esto funciona en mi entorno local?

Esta característica no funciona para un entorno de Active Directory Domain Services (AD DS) local puro.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>Mi organización requiere la autenticación en dos fases para acceder a los recursos. ¿Qué puedo hacer para admitir este requisito?

Las claves de seguridad tienen varios factores de forma. Póngase en contacto con el fabricante del dispositivo concreto para analizar cómo se pueden habilitar sus dispositivos con un PIN o reconocimiento biométrico como segundo factor.

### <a name="can-admins-set-up-security-keys"></a>¿Los administradores pueden configurar claves de seguridad?

Estamos trabajando en esta funcionalidad para la disponibilidad general (GA) de esta característica.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>¿Dónde puedo encontrar claves de seguridad compatibles?

[Llaves de seguridad FIDO2](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>¿Qué hago si pierdo mi clave de seguridad?

Para quitar las claves desde Azure Portal si va a la página de información de seguridad y quita la clave de seguridad.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre el registro de dispositivos](../devices/overview.md)

[Más información sobre Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
