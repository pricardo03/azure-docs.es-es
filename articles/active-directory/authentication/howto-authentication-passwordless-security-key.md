---
title: Habilitar el inicio de sesión con clave de seguridad sin contraseña para Azure AD (versión preliminar) - Azure Active Directory
description: Habilitar el inicio de sesión con clave de seguridad sin contraseña para Azure AD mediante claves de seguridad FIDO2 (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 316a523a6216354ae5b6166be55e183a4e050766
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305076"
---
# <a name="enable-passwordless-security-key-sign-in-for-azure-ad-preview"></a>Habilitar el inicio de sesión con clave de seguridad sin contraseña para Azure AD (versión preliminar)

## <a name="requirements"></a>Requisitos

* Azure Multi-Factor Authentication
* Versión preliminar del registro combinado con usuarios habilitados para SSPR
* La versión preliminar de la clave de seguridad FIDO2 requiere claves de seguridad FIDO2 compatibles
* WebAuthN requiere Microsoft Edge en Windows 10 versión 1809 o superior
* El inicio de sesión de Windows basado en FIDO2 requiere la versión 1809 o superior de Windows 10 unida a Azure AD

## <a name="prepare-devices-for-preview"></a>Preparar dispositivos para la versión preliminar

Los dispositivos que va a probar en uso piloto deben ejecutar Windows 10 versión 1809 o superior. La mejor experiencia se logra con Windows 10 versión 1903 o superior.

## <a name="enable-security-keys-for-windows-sign-in"></a>Habilitar claves de seguridad para inicio de sesión de Windows

Las organizaciones pueden optar por usar uno o varios de los métodos siguientes para habilitar el uso de claves de seguridad para el inicio de sesión de Windows.

### <a name="enable-credential-provider-via-intune"></a>Habilitar proveedor de credenciales a través de Intune

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **Microsoft Intune** > **Inscripción de dispositivos** > **Inscripción de Windows** > **Windows Hello para empresas** > **Propiedades**.
1. En **Configuración**, establezca **Utilice las claves de seguridad para el inicio de sesión** en **Habilitado**.

La configuración de claves de seguridad para el inicio de sesión no depende de la configuración de Windows Hello para empresas.

#### <a name="enable-targeted-intune-deployment"></a>Habilitar implementación de Intune dirigida

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
      1. Tipo de datos: Integer
      1. Valor: 1 
1. Esta directiva se puede asignar a usuarios, dispositivos o grupos específicos. Se puede encontrar más información en el artículo [Asignación de perfiles de dispositivo en Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

![Creación de directiva de configuración de dispositivos personalizada de Intune](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Habilitar un proveedor de credenciales a través de un paquete de aprovisionamiento

En el caso de los dispositivos no administrados por Intune, se puede instalar un paquete de aprovisionamiento para habilitar la funcionalidad. La aplicación Diseñador de configuración de Windows puede instalarse desde [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

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

## <a name="obtain-fido2-security-keys"></a>Obtener claves de seguridad FIDO2

Vea la sección Claves de seguridad FIDO2 del artículo [¿Qué quiere decir sin contraseña?](concept-authentication-passwordless.md) para obtener más información sobre las claves y los fabricantes compatibles.

> [!NOTE]
> Si adquiere y planea usar claves de seguridad basadas en NFC, necesita un lector NFC compatible.

## <a name="enable-passwordless-authentication-method"></a>Habilitar métodos de autenticación sin contraseña

### <a name="enable-the-combined-registration-experience"></a>Habilitar la experiencia de registro combinado

Las características de registro de los métodos de autenticación sin contraseña se basan en la versión preliminar de registro combinado. Siga los pasos del artículo [Habilitar el registro de información de seguridad combinado (vista preliminar)](howto-registration-mfa-sspr-combined.md) para habilitar la versión preliminar del registro combinado.

### <a name="enable-new-passwordless-authentication-method"></a>Habilitar nuevos métodos de autenticación sin contraseña

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)
1. Vaya a **Azure Active Directory** > **Seguridad** > **Métodos de autenticación** > **Directiva de métodos de autenticación (versión preliminar)** .
1. En cada **Método**, seleccione las siguientes opciones
   1. **Habilitar**: Sí o No
   1. **Destino**: Todos los usuarios o Seleccionar usuarios
1. **Guarde** cada método.

> [!WARNING]
> Las "directivas de restricción de claves" FIDO2 aún no funcionan. Esta funcionalidad va a estar disponible antes de la disponibilidad general, así que no cambie el valor predeterminado de estas directivas.

> [!NOTE]
> No es necesario seleccionar ambos métodos sin contraseña (si solo quiere obtener la versión preliminar de un método sin contraseña, puede habilitar solo ese método). Se recomienda probar ambos métodos, ya que cada uno tiene sus propias ventajas.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Registro de usuario y administración de claves de seguridad FIDO2

1. Vaya a [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Inicie sesión si aún no lo ha hecho.
1. Haga clic en **Información de seguridad**.
   1. Si el usuario ya tiene al menos un método de Azure Multi-Factor Authentication registrado, puede registrar de inmediato una clave de seguridad FIDO2.
   1. Si no tiene registrado al menos un método de Azure Multi-Factor Authentication, debe agregar uno.
1. Agregue una clave de seguridad FIDO2 al hacer clic en **Agregar método** y seleccionar **Clave de seguridad**.
1. Seleccione **Dispositivo USB** o **Dispositivo NFC**.
1. Tenga preparada la clave y seleccione **Siguiente**.
1. Aparece un cuadro donde se le pide que cree o escriba un PIN para la clave de seguridad y luego que realice el gesto necesario para la clave, ya sea biométrico o toque.
1. Se le devuelve a la experiencia de registro combinado y se le pide que proporcione un nombre descriptivo para el token para que pueda identificar cuál es si tiene varios. Haga clic en **Next**.
1. Haga clic en **Listo** para finalizar el proceso.

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Administrar biométrica, PIN o restablecer clave de seguridad

* Windows 10 versión 1809
   * Se requiere software complementario del proveedor de claves de seguridad
* Windows 10 versión 1903 o posterior
   * Los usuarios pueden abrir **Configuración de Windows** en el dispositivo > **Cuentas** > **Clave de seguridad**
   * Los usuarios pueden cambiar el PIN, actualizar la biométrica o restablecer la clave de seguridad

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Registro de usuario y administración de la aplicación Microsoft Authenticator

Para configurar la aplicación Microsoft Authenticator para inicio de sesión telefónico, siga las instrucciones del artículo [Inicio de sesión en sus cuentas mediante la aplicación Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md).

## <a name="sign-in-with-passwordless-credential"></a>Iniciar sesión con credenciales sin contraseña

### <a name="sign-in-at-the-lock-screen"></a>Iniciar sesión en la pantalla de bloqueo

En el ejemplo siguiente, el usuario Bala Sandhu ya ha aprovisionado su clave de seguridad FIDO2. Bala puede elegir el proveedor de credenciales de clave de seguridad desde la pantalla de bloqueo de Windows 10 e insertar la clave de seguridad para iniciar sesión en Windows.

![Inicio de sesión de clave de seguridad en la pantalla de bloqueo de Windows 10](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Iniciar sesión en Internet

En el ejemplo siguiente, un usuario ya ha aprovisionado su clave de seguridad FIDO2. El usuario puede optar por iniciar sesión en Internet con su clave de seguridad FIDO2 dentro del explorador Microsoft Edge en Windows 10 versión 1809 o posterior.

![Inicio de sesión de clave de seguridad en Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="known-issues"></a>Problemas conocidos

### <a name="security-key-provisioning"></a>Aprovisionamiento de claves de seguridad

El aprovisionamiento y desaprovisionamiento de administrador de claves de seguridad no está disponible en la versión preliminar pública.

### <a name="hybrid-azure-ad-join"></a>Unión a Azure AD híbrido

Los usuarios que dependen del SSO basado en WIA y que usan credenciales administradas como claves de seguridad FIDO2 o inicio de sesión sin contraseña con la aplicación Microsoft Authenticator deben unirse a Azure AD híbrido en Windows 10 para obtener las ventajas del SSO. Pero, por ahora, las claves de seguridad solo funcionan con equipos unidos a Azure Active Directory. Se recomienda probar claves de seguridad FIDO2 para la pantalla de bloqueo de Windows en equipos unidos a Azure Active Directory puro únicamente. Esta limitación no se aplica a Internet.

### <a name="upn-changes"></a>Cambios de UPN

Se trabaja en la admisión de una característica que permite el cambio de UPN en dispositivos AADJ y AADJ híbridos. Si cambia el UPN de un usuario, ya no puede modificar las claves de seguridad FIDO2 en consecuencia. Así, la única solución es restablecer el dispositivo y que el usuario vuelva a registrarse.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre el registro de dispositivos](../devices/overview.md)

[Más información sobre Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
