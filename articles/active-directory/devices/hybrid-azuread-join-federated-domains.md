---
title: Configuración de la unión a Azure Active Directory híbrido para dominios federados | Microsoft Docs
description: Aprenda a configurar la unión a Azure Active Directory híbrido para dominios federados.
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
ms.openlocfilehash: 600d6b9f1eb8d8073e1658dd5b8196a3d8137e42
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733719"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Configuración de dispositivos híbridos unidos a Azure Active Directory para dominios federados

De forma similar a un usuario, un dispositivo es otra identidad principal que debe proteger y usarla también para proteger los recursos en cualquier momento y lugar. Para lograr este objetivo, puede llevar las identidades de los dispositivos a Azure AD y administrarlas ahí mediante uno de los métodos siguientes:

- Unión a Azure AD
- Unión a Azure AD híbrido
- Registro de Azure AD

Al traer sus dispositivos a Azure AD, está maximizando la productividad de los usuarios mediante un inicio de sesión único (SSO) en los recursos de nube y del entorno local. Al mismo tiempo, puede proteger el acceso a los recursos de nube y de entorno local con [acceso condicional](../active-directory-conditional-access-azure-portal.md).

En este tutorial, aprenderá a configurar la unión de Azure AD híbrido para dispositivos de equipos unidos a un dominio de AD en un entorno federado mediante AD FS.

> [!NOTE]
> Si su entorno federado usa un proveedor de identidades distinto de AD FS, debe asegurarse de que el proveedor de identidades admita el protocolo WS-Trust. WS-Trust es necesario para autenticar sus actuales dispositivos unidos a un dominio de Azure AD híbrido. Además, si tiene dispositivos de nivel inferior de Windows que necesita unir a Azure AD híbrido, el proveedor de identidades deberá admitir la notificación WIAORMULTIAUTHN. 


> [!div class="checklist"]
> * Configuración de la unión a Azure AD híbrido
> * Habilitación de dispositivos de Windows de nivel inferior
> * Comprobación del registro
> * Solución de problemas

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que está familiarizado con:

- [Introducción a la administración de identidades de dispositivo en Azure Active Directory](../device-management-introduction.md)
- [Planeación de la implementación de dispositivos híbridos unidos a Azure Active Directory](hybrid-azuread-join-plan.md)
- [Validación controlada de la unión a Azure AD híbrido](hybrid-azuread-join-control.md)

Para configurar el escenario de este tutorial necesita:

- Windows Server 2012 R2 con AD FS
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versión 1.1.819.0 o superior.

Desde la versión 1.1.819.0, Azure AD Connect proporciona un asistente para configurar la unión a Azure AD híbrido. El asistente permite simplificar considerablemente el proceso de configuración. El asistente relacionado:

- Configura los puntos de conexión de servicio (SCP) para el registro de dispositivos
- Hace una copia de seguridad de la relación de confianza para usuario autenticado de Azure AD existente
- Actualiza las reglas de notificación en la confianza de Azure AD

Los pasos de configuración en este artículo se basan en este asistente. Si tiene una versión anterior de Azure AD Connect instalada, tiene que actualizarla a 1.1.819 o posterior. Si no le es posible instalar la versión más reciente de Azure AD Connect, consulte [cómo configurar manualmente la unión a Azure AD híbrido](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual).

La unión a Azure AD híbrido requiere que los dispositivos tengan acceso a los siguientes recursos de Microsoft desde dentro de la red de su organización:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- STS de su organización (dominios federados)
- [https://autologon.microsoftazuread-sso.com](`https://autologon.microsoftazuread-sso.com`)(Si está utilizando o planeando usar SSO de conexión directa)

A partir de Windows 10 1803, si se produce un error en la unión instantánea a Azure AD híbrido en un dominio federado mediante AD FS, contamos con Azure AD Connect para sincronizar el objeto de equipo en Azure AD que se usa posteriormente para completar el registro de dispositivos para la unión a Azure AD híbrido. Compruebe que Azure AD Connect ha sincronizado con Azure AD los objetos de equipo de los dispositivos que desea que sean híbridos unidos a un dominio de Azure AD. Si los objetos de equipo pertenecen a unidades organizativas (OU) específicas, estas deben configurarse también para la sincronización en Azure AD Connect. Para más información sobre cómo sincronizar objetos de equipo con Azure AD Connect, consulte el artículo [Configuración del filtrado con Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#organizational-unitbased-filtering).

Si su organización necesita acceso a Internet a través de un proxy de salida, Microsoft recomienda [implementar la detección automática de proxy web (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) para permitir que los equipos con Windows 10 realicen el registro de dispositivos con Azure AD. Si está experimentando problemas con la configuración y administración de WPAD, vaya a [solución de problemas de detección automática](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Si no usa WPAD y necesita configurar el proxy en el equipo, puede hacerlo a partir de Windows 10 1709 si [configura WinHTTP mediante un objeto de directiva de grupo (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> Si configura el proxy en el equipo mediante WinHTTP, todos los equipos que no se puedan conectar al proxy configurado no podrán conectarse a Internet.

Si su organización requiere acceso a Internet a través de un servidor proxy saliente autenticado, tiene que asegurarse de que los equipos de Windows 10 pueden autenticarse correctamente en el proxy de salida. Debido a que los equipos de Windows 10 ejecutan el registro de dispositivos utilizando el contexto del equipo, es necesario configurar la autenticación de proxy de salida utilizando el contexto del equipo. Realice un seguimiento con su proveedor de proxy de salida en relación a los requisitos de configuración.

## <a name="configure-hybrid-azure-ad-join"></a>Configuración de la unión a Azure AD híbrido

Para configurar una unión a Azure AD híbrido utilizando Azure AD Connect, necesita:

- Las credenciales de administrador global para el inquilino de Azure AD.  
- Las credenciales de administrador de empresa para cada uno de los bosques.
- Las credenciales del administrador de AD FS.

**Para configurar una unión a Azure AD híbrido usando Azure AD Connect:**

1. Inicie Azure AD Connect y, a continuación, haga clic en **Configurar**.

   ![Bienvenido](./media/hybrid-azuread-join-federated-domains/11.png)

1. En la página **Tareas adicionales** seleccione **Configurar opciones de dispositivo** y haga clic en **Siguiente**.

   ![Tareas adicionales](./media/hybrid-azuread-join-federated-domains/12.png)

1. En la página **Información general**, haga clic en **Siguiente**.

   ![Información general](./media/hybrid-azuread-join-federated-domains/13.png)

1. En la página **Conectar con Azure AD**, proporcione las credenciales de administrador global para el inquilino de Azure AD y haga clic en **Siguiente**.

   ![Conectarse a Azure](./media/hybrid-azuread-join-federated-domains/14.png)

1. En la página **Opciones de dispositivos**, seleccione **Configurar la combinación de Azure AD híbrido**y haga clic en **Siguiente**.

   ![Opciones de dispositivos](./media/hybrid-azuread-join-federated-domains/15.png)

1. En la página **SCP**, realice los pasos siguientes y luego haga clic en **Siguiente**:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Seleccione el bosque.
   1. Seleccione el servicio de autenticación. Debe seleccionar un servidor de AD FS, salvo que su organización tenga exclusivamente clientes de Windows 10 y haya configurado la sincronización de equipos o dispositivos, o bien su organización use SeamlessSSO.
   1. Haga clic en **Agregar** para especificar las credenciales de administrador de empresa.

1. En la página **Sistemas operativos de los dispositivos**, seleccione los sistemas operativos utilizados por los dispositivos en el entorno de Active Directory y luego haga clic en **Siguiente**.

   ![Sistema operativos del dispositivo](./media/hybrid-azuread-join-federated-domains/17.png)

1. En la página **Configuración de federación**, especifique las credenciales de su administrador de AD FS y después haga clic en **Siguiente**.

   ![Configuración de federación](./media/hybrid-azuread-join-federated-domains/18.png)

1. En la página **Listo para configurar**, haga clic en **Configurar**.

   ![Listo para configurar](./media/hybrid-azuread-join-federated-domains/19.png)

1. En la página **Configuración completa**, haga clic en **Salir**.

   ![Configuración completada](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-down-level-devices"></a>Habilitación de dispositivos de Windows de nivel inferior

Si algunos de los dispositivos unidos a un dominio son dispositivos de Windows de nivel inferior, tendrá que:

- Configurar los valores de la intranet local para el registro de dispositivos
- Instalar Microsoft Workplace Join for Windows en equipos de nivel inferior

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Configurar los valores de la intranet local para el registro de dispositivos

Para completar con éxito la combinación de Azure AD híbrido de los dispositivos de Windows de nivel inferior, y para evitar las peticiones de certificados cuando los dispositivos se autentican en Azure AD, se puede insertar una directiva en los dispositivos unidos a un dominio para agregar las siguientes direcciones URL a la zona de intranet local en Internet Explorer:

- `https://device.login.microsoftonline.com`
- El servicio de token de seguridad (STS - dominios federados) de su organización
- `https://autologon.microsoftazuread-sso.com` (para el SSO de conexión directa).

Además, tiene que habilitar **Permitir actualizaciones en la barra de estado a través de script** en la zona de intranet local del usuario.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Instalación de Microsoft Workplace Join for Windows en equipos de nivel inferior

Para registrar dispositivos de nivel inferior de Windows, las organizaciones deben instalar [Microsoft Workplace Join para equipos sin Windows 10](https://www.microsoft.com/download/details.aspx?id=53554) que se encuentra disponible en el Centro de descarga de Microsoft.

El paquete se puede implementar mediante un sistema de distribución de software como  [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). El paquete admite las opciones de instalación silenciosa estándar mediante el parámetro quiet. La rama actual de Configuration Manager ofrece ventajas adicionales sobre las versiones anteriores, como la posibilidad de realizar el seguimiento de los registros completados.

El instalador crea una tarea programada en el sistema que se ejecuta en el contexto del usuario. La tarea se desencadena cuando el usuario inicia sesión en Windows. La tarea une de forma silenciosa el dispositivo con Azure AD con las credenciales de usuario después de realizar la autenticación con Azure AD.

## <a name="verify-the-registration"></a>Comprobación del registro

Para comprobar el estado de registro del dispositivo en su inquilino de Azure, puede usar el cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** en el **[módulo de PowerShell de Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** .

Cuando se usa el cmdlet **Get-MSolDevice** para comprobar los detalles de servicio:

- Tiene que existir un objeto con el **Identificador de dispositivo** que coincida con el identificador en el cliente de Windows.
- El valor de **DeviceTrustType** tiene que ser **Unido a dominio**. Esto es equivalente al estado **Unido a Azure AD híbrido** en la página de dispositivos en el portal de Azure AD.
- En el caso de los dispositivos que se usan en el acceso condicional, el valor de **Habilitado** tiene que ser **True** y el de **DeviceTrustLevel** tiene que ser **Administrado**.

**Para comprobar los detalles del servicio:**

1. Abra **Windows PowerShell** como administrador.
1. Escriba `Connect-MsolService` para conectarse a su inquilino de Azure.  
1. Escriba `get-msoldevice -deviceId <deviceId>`.
1. Compruebe que **Habilitado** está establecido en **True**.

## <a name="troubleshoot-your-implementation"></a>Solución de problemas de la implementación

Si tiene problemas para completar la unión a Azure AD híbrido para los dispositivos de Windows unidos a un dominio, consulte:

- [Solución de problemas de la unión a Azure AD híbrido para dispositivos actuales de Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solución de problemas de la unión a Azure AD híbrido para dispositivos de nivel inferior de Windows](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo administrar identidades de dispositivos en el portal de Azure AD, consulte [Administración de identidades de dispositivos con Azure Portal](device-management-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
