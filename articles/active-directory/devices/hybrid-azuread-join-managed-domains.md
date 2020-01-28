---
title: Configuración de la unión a Azure Active Directory híbrido para dominios administrados | Microsoft Docs
description: Aprenda a configurar la unión a Azure Active Directory híbrido para dominios administrados.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17bfbc29f38230dc2533c9ccc63cdee4fc776717
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76512115"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Tutorial: Configuración de dispositivos híbridos unidos a Azure Active Directory para dominios administrados

Al igual que un usuario de su organización, un dispositivo es una identidad principal que desea proteger. Puede usar una identidad de dispositivo para proteger los recursos en cualquier momento y ubicación. Para lograr este objetivo, puede llevar las identidades de los dispositivos a Azure Active Directory (Azure AD) y administrarlas ahí mediante uno de los métodos siguientes:

- Unión a Azure AD
- Unión a Azure AD híbrido
- Registro de Azure AD

Al traer sus dispositivos a Azure AD, está maximizando la productividad de los usuarios mediante un inicio de sesión único (SSO) en los recursos de nube y del entorno local. Puede proteger el acceso a los recursos de nube y del entorno local con [acceso condicional](../active-directory-conditional-access-azure-portal.md) al mismo tiempo.

En este tutorial, aprenderá a configurar la unión a Azure AD híbrido para equipos unidos a un dominio de Active Directory en un entorno administrado. 

Un entorno administrado se puede implementar mediante la [sincronización de hash de contraseña (PHS)](../hybrid/whatis-phs.md) o la [autenticación de paso a través (PTA)](../hybrid/how-to-connect-pta.md) con [inicio de sesión único de conexión directa](../hybrid/how-to-connect-sso.md). Estos escenarios no requieren que se configure un servidor de federación para la autenticación.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración de la unión a Azure AD híbrido
> * Habilitación de dispositivos de Windows de nivel inferior
> * Comprobación dispositivos unidos
> * Solución de problemas

## <a name="prerequisites"></a>Prerequisites

En este tutorial se da por supuesto que está familiarizado con estos artículos:

- [¿Qué es una identidad de dispositivo?](overview.md)
- [Planeación de la implementación de la unión a Azure AD híbrido](hybrid-azuread-join-plan.md)
- [Validación controlada de la unión a Azure AD híbrido](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD no admite tarjetas inteligentes ni certificados en dominios administrados.

Para configurar el escenario en este artículo, necesita tener instalada la [versión más reciente de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 o posterior).

Compruebe que Azure AD Connect ha sincronizado con Azure AD los objetos de equipo de los dispositivos que desea que estén unidos a Azure AD híbrido. Si los objetos de equipo pertenecen a unidades organizativas (OU) específicas, debe configurarlas también para su sincronización en Azure AD Connect. Para más información acerca de cómo sincronizar objetos de equipo con Azure AD Connect, consulte el artículo [Configuración del filtrado con Azure AD Connect](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Desde la versión 1.1.819.0, Azure AD Connect incluye un asistente que puede utilizar para configurar la unión a Azure AD híbrido. El asistente simplifica considerablemente el proceso de configuración. El asistente configura los puntos de conexión de servicio (SCP) para el registro de dispositivos.

Los pasos de configuración en este artículo se basan en el uso del asistente de Azure AD Connect.

La unión a Azure AD híbrido requiere que los dispositivos tengan acceso a los siguientes recursos de Microsoft desde dentro de la red de su organización:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (si usa o planea usar SSO de conexión directa)

Si su organización necesita acceso a Internet mediante un proxy de salida, Microsoft recomienda [implementar la detección automática de proxy web (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) para permitir que los equipos con Windows 10 realicen el registro de dispositivos con Azure AD. Si tiene problemas de configuración y administración de WPAD, consulte [Troubleshoot automatic detection](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)) (Solución de problemas de detección automática). 

Si no usa WPAD y necesita configurar el proxy en el equipo, puede hacerlo desde la versión Windows 10 1709. Para más información, consulte [Configure WinHTTP settings using a group policy object (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/) [Configuración de WinHTTP mediante un objeto de directiva de grupo (GPO)].

> [!NOTE]
> Si configura el proxy en el equipo mediante WinHTTP, todos los equipos que no se puedan conectar al proxy configurado no podrán conectarse a Internet.

Si su organización requiere acceso a Internet mediante un servidor proxy saliente autenticado, tiene que asegurarse de que los equipos de Windows 10 pueden autenticarse correctamente en el proxy de salida. Debido a que los equipos de Windows 10 ejecutan el registro de dispositivos utilizando el contexto del equipo, debe configurar la autenticación del proxy de salida mediante el contexto del equipo. Realice un seguimiento con su proveedor de proxy de salida en relación a los requisitos de configuración.

Para comprobar si el dispositivo puede acceder a los recursos de Microsoft anteriores en la cuenta del sistema, puede usar el script para [probar la conectividad del Registro de dispositivos](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0).

## <a name="configure-hybrid-azure-ad-join"></a>Configuración de la unión a Azure AD híbrido

Para configurar una unión a Azure AD híbrido utilizando Azure AD Connect, necesita:

- Las credenciales de administrador global para el inquilino de Azure AD
- Las credenciales de administrador de empresa para cada uno de los bosques

**Para configurar una unión a Azure AD híbrido mediante Azure AD Connect:**

1. Inicie Azure AD Connect y, a continuación, seleccione **Configurar**.

   ![Bienvenido](./media/hybrid-azuread-join-managed-domains/11.png)

1. En la página **Tareas adicionales**, seleccione **Configurar opciones de dispositivo** y luego **Siguiente**.

   ![Tareas adicionales](./media/hybrid-azuread-join-managed-domains/12.png)

1. En la página **Información general**, seleccione **Siguiente**.

   ![Información general](./media/hybrid-azuread-join-managed-domains/13.png)

1. En la página **Conectar con Azure AD**, proporcione las credenciales de administrador global para el inquilino de Azure AD.  

   ![Conectarse a Azure](./media/hybrid-azuread-join-managed-domains/14.png)

1. En la página **Opciones de dispositivos**, seleccione **Configurar la combinación de Azure AD híbrido** y después **Siguiente**.

   ![Opciones de dispositivos](./media/hybrid-azuread-join-managed-domains/15.png)

1. En la página **SCP**, para cada bosque en el que desee que Azure AD Connect configure el SCP, complete los pasos siguientes y, a continuación, seleccione **Siguiente**:

   ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

   1. Seleccione el bosque.
   1. Seleccione el servicio de autenticación.
   1. Seleccione **Agregar** para especificar las credenciales de administrador de empresa.

1. En la página **Sistemas operativos de los dispositivos**, seleccione los sistemas operativos que los dispositivos en el entorno de Active Directory utilizan y luego **Siguiente**.

   ![Sistema operativos del dispositivo](./media/hybrid-azuread-join-managed-domains/17.png)

1. En la página **Listo para configurar**, seleccione **Configurar**.

   ![Listo para configurar](./media/hybrid-azuread-join-managed-domains/19.png)

1. En la página **Configuración completa**, seleccione **Salir**.

   ![Configuración completada](./media/hybrid-azuread-join-managed-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Habilitación de dispositivos de Windows de nivel inferior

Si algunos de los dispositivos unidos a un dominio son dispositivos de Windows de nivel inferior, tiene que:

- Configurar los valores de la intranet local para el registro de dispositivos
- Configurar SSO de conexión directa
- Instalar Microsoft Workplace Join for Windows en equipos de nivel inferior

> [!NOTE]
> La compatibilidad con Windows 7 finalizó el 14 de enero de 2020. Para más información, consulte el artículo sobre el [fin de la compatibilidad con Windows 7](https://support.microsoft.com/en-us/help/4057281/windows-7-support-ended-on-january-14-2020).

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Configurar los valores de la intranet local para el registro de dispositivos

Para completar con éxito la unión de Azure AD híbrido de los dispositivos de Windows de nivel inferior y para evitar las peticiones de certificados cuando los dispositivos se autentican en Azure AD, puede insertar una directiva en los dispositivos unidos a un dominio para agregar las siguientes direcciones URL a la zona de intranet local en Internet Explorer:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

También tiene que habilitar **Permitir actualizaciones en la barra de estado a través de script** en la zona de intranet local del usuario.

### <a name="configure-seamless-sso"></a>Configurar SSO de conexión directa

Para completar correctamente la unión a Azure AD híbrido de los dispositivos de nivel inferior de Windows en un dominio administrado que usa [PHS](../hybrid/whatis-phs.md) o [PTA](../hybrid/how-to-connect-pta.md) como método de autenticación en la nube de Azure AD, también debe [configurar SSO de conexión directa](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature).

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Instalar Microsoft Workplace Join for Windows en equipos de nivel inferior

Para registrar dispositivos de nivel inferior de Windows, las organizaciones deben instalar [Microsoft Workplace Join para equipos sin Windows 10](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join para equipos sin Windows 10 está disponible en el Centro de descarga de Microsoft.

El paquete se puede implementar mediante un sistema de distribución de software como  [Microsoft Endpoint Configuration Manager](https://docs.microsoft.com/configmgr/). El paquete admite las opciones de instalación silenciosa estándar mediante el parámetro `quiet`. La rama actual de Configuration Manager ofrece ventajas adicionales sobre las versiones anteriores, como la posibilidad de realizar el seguimiento de los registros completados.

El instalador crea una tarea programada en el sistema que se ejecuta en el contexto del usuario. La tarea se desencadena cuando el usuario inicia sesión en Windows. La tarea une de forma silenciosa el dispositivo con Azure AD mediante las credenciales de usuario después de que se autentique con Azure AD.

## <a name="verify-the-registration"></a>Comprobación del registro

Para comprobar el estado de registro del dispositivo en su inquilino de Azure, puede usar el cmdlet **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** en el [módulo de PowerShell de Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Cuando utiliza el cmdlet **Get-MSolDevice** para comprobar los detalles de servicio:

- Tiene que existir un objeto con el **Identificador de dispositivo** que coincida con el identificador en el cliente de Windows.
- El valor de **DeviceTrustType** tiene que ser **Unido a dominio**. Este valor es equivalente al estado **Hybrid Azure AD joined** en la página **Dispositivos** del portal de Azure AD.
- En el caso de los dispositivos que se usan en el acceso condicional, el valor de **Habilitado** tiene que ser **True** y el de **DeviceTrustLevel** tiene que ser **Administrado**.

**Para comprobar los detalles del servicio:**

1. Abra Windows PowerShell como administrador.
1. Escriba `Connect-MsolService` para conectarse a su inquilino de Azure.  
1. Escriba `get-msoldevice -deviceId <deviceId>`.
1. Compruebe que **Habilitado** está establecido en **True**.

## <a name="troubleshoot-your-implementation"></a>Solución de problemas de la implementación

Si tiene problemas para completar la unión a Azure AD híbrido para los dispositivos de Windows unidos a un dominio, consulte:

- [Solución de problemas de la unión a Azure AD híbrido para dispositivos actuales de Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solución de problemas de la unión a Azure AD híbrido para dispositivos de nivel inferior de Windows](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [administrar identidades de dispositivos con Azure Portal](device-management-azure-portal.md).
