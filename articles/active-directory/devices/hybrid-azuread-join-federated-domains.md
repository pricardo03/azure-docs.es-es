---
title: Configuración de dispositivos híbridos unidos a Azure Active Directory | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/07/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: e40c18edadebae1f92cf811ea054503b9cd6b1ae
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277977"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Configuración de la unión a Azure Active Directory híbrido para dominios federados

De forma similar a un usuario, un dispositivo se está convirtiendo en otra identidad que debe proteger y usar también para proteger los recursos en cualquier momento y lugar. Puede lograr este objetivo incluyendo las identidades de los dispositivos en Azure AD mediante uno de los métodos siguientes:

- Unión a Azure AD
- Unión a Azure AD híbrido
- Registro de Azure AD

Al traer sus dispositivos a Azure AD, está maximizando la productividad de los usuarios mediante un inicio de sesión único (SSO) en los recursos de nube y del entorno local. Al mismo tiempo, puede proteger el acceso a los recursos de nube y de entorno local con [acceso condicional](../active-directory-conditional-access-azure-portal.md).

En este tutorial aprenderá a configurar la unión a Azure AD híbrido para dispositivos federados con ADFS.

> [!div class="checklist"]
> * Configuración de la unión a Azure AD híbrido
> * Habilitación de dispositivos de Windows de nivel inferior
> * Comprobación del registro
> * Solución de problemas


## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por supuesto que está familiarizado con:

-  [Introducción a la administración de dispositivos en Azure Active Directory](../device-management-introduction.md)

-  [Planeación de la implementación de dispositivos híbridos unidos a Azure Active Directory](hybrid-azuread-join-plan.md)

-  [Control de la unión de los dispositivos híbridos a Azure AD](hybrid-azuread-join-control.md)


Para configurar el escenario de este tutorial necesita:

- Windows Server 2012 R2 con AD FS

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versión 1.1.819.0 o superior. 
 

Desde la versión 1.1.819.0, Azure AD Connect proporciona un asistente para configurar la unión a Azure AD híbrido. El asistente permite simplificar considerablemente el proceso de configuración. El asistente relacionado:

- Configura los puntos de conexión de servicio (SCP) para el registro de dispositivos

- Hace una copia de seguridad de la relación de confianza para usuario autenticado de Azure AD existente

- Actualiza las reglas de notificación en la confianza de Azure AD

Los pasos de configuración en este artículo se basan en este asistente. Si tiene una versión anterior de Azure AD Connect instalada, tiene que actualizarla a 1.1.819 o posterior. Si no le es posible instalar la versión más reciente de Azure AD Connect, consulte la información acerca de [cómo configurar manualmente el registro de dispositivos](../device-management-hybrid-azuread-joined-devices-setup.md).

La unión a Azure AD híbrido requiere que los dispositivos tengan acceso a los siguientes recursos de Microsoft desde dentro de la red de su organización:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- STS de su organización (dominios federados)
- https://autologon.microsoftazuread-sso.com (Si está utilizando o planeando usar SSO de conexión directa)

A partir de Windows 10 1803, si se produce un error en la unión a Azure AD híbrido para un dominio federado como AD FS, contamos con Azure AD Connect para sincronizar el objeto de equipo en Azure AD que se usa posteriormente para completar el registro del dispositivo para la unión a Azure AD híbrido.

Si su organización requiere acceso a Internet a través de un proxy de salida, a partir de Windows 10 1709, puede [configurar los valores del proxy en el equipo utilizando un objeto de directiva de grupo (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Si el equipo está ejecutando versiones anteriores a Windows 10 1709, tiene que implementar la detección automática de proxy web (WPAD) para permitir que los equipos de Windows 10 realicen el registro de dispositivos con Azure AD. 

Si su organización requiere acceso a Internet a través de un servidor proxy saliente autenticado, tiene que asegurarse de que los equipos de Windows 10 pueden autenticarse correctamente en el proxy de salida. Debido a que los equipos de Windows 10 ejecutan el registro de dispositivos utilizando el contexto del equipo, es necesario configurar la autenticación de proxy de salida utilizando el contexto del equipo. Realice un seguimiento con su proveedor de proxy de salida en relación a los requisitos de configuración. 


## <a name="configure-hybrid-azure-ad-join"></a>Configuración de la unión a Azure AD híbrido

Para configurar una unión a Azure AD híbrido utilizando Azure AD Connect, necesita:

- Las credenciales de administrador global para el inquilino de Azure AD.  

- Las credenciales de administrador de empresa para cada uno de los bosques.

- Las credenciales del administrador de AD FS. 


**Para configurar una unión a Azure AD híbrido usando Azure AD Connect:**

1. Inicie Azure AD Connect y, a continuación, haga clic en **Configurar**.

    ![Bienvenido](./media/hybrid-azuread-join-federated-domains/11.png)

2. En la página **Tareas adicionales** seleccione **Configurar opciones de dispositivo** y haga clic en **Siguiente**. 

    ![Tareas adicionales](./media/hybrid-azuread-join-federated-domains/12.png)

3. En la página **Información general**, haga clic en **Siguiente**. 

    ![Información general](./media/hybrid-azuread-join-federated-domains/13.png)

4. En la página **Conectar con Azure AD**, proporcione las credenciales de administrador global para el inquilino de Azure AD y haga clic en **Siguiente**.   

    ![Conectarse a Azure](./media/hybrid-azuread-join-federated-domains/14.png)

5. En la página **Opciones de dispositivos**, seleccione **Configurar la combinación de Azure AD híbrido**y haga clic en **Siguiente**. 

    ![Opciones de dispositivos](./media/hybrid-azuread-join-federated-domains/15.png)

6. En la página **SCP**, realice los pasos siguientes y luego haga clic en **Siguiente**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Seleccione el bosque.

    b. Seleccione el servicio de autenticación. Debe seleccionar un servidor de AD FS, salvo que su organización tenga exclusivamente clientes de Windows 10 y haya configurado la sincronización de equipos o dispositivos, o bien su organización use SeamlessSSO.

    c. Haga clic en **Agregar** para especificar las credenciales de administrador de empresa.


7. En la página **Sistemas operativos de los dispositivos**, seleccione los sistemas operativos utilizados por los dispositivos en el entorno de Active Directory y luego haga clic en **Siguiente**. 

    ![Sistema operativos del dispositivo](./media/hybrid-azuread-join-federated-domains/17.png)

8. En la página **Configuración de federación**, especifique las credenciales de su administrador de AD FS y después haga clic en **Siguiente**. 

    ![Configuración de federación](./media/hybrid-azuread-join-federated-domains/18.png)

9. En la página **Listo para configurar**, haga clic en **Configurar**. 

    ![Listo para configurar](./media/hybrid-azuread-join-federated-domains/19.png)

10. En la página **Configuración completa**, haga clic en **Salir**. 

    ![Configuración completada](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Habilitación de dispositivos de Windows de nivel inferior

Si algunos de los dispositivos unidos a un dominio son dispositivos de Windows de nivel inferior, tendrá que:

- Actualizar configuración del dispositivo
 
- Configurar los valores de la intranet local para el registro de dispositivos

- Control de dispositivos de Windows de nivel inferior 


### <a name="update-device-settings"></a>Actualizar configuración del dispositivo 

Para registrar dispositivos de Windows de nivel inferior, es preciso asegurarse de que están establecidos los valores de dispositivo que permiten a los usuarios registrar dispositivos en Azure AD. En Azure Portal, esta configuración se encuentra en:

`Home > [Name of your tenant] > Devices - Device settings`  


    
En la siguiente directiva se debe elegir **Todos**: **Los usuarios pueden registrar sus dispositivos con Azure AD**

![Registro de dispositivos](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Configurar los valores de la intranet local para el registro de dispositivos

Para completar con éxito la combinación de Azure AD híbrido de los dispositivos de Windows de nivel inferior, y para evitar las peticiones de certificados cuando los dispositivos se autentican en Azure AD, se puede insertar una directiva en los dispositivos unidos a un dominio para agregar las siguientes direcciones URL a la zona de intranet local en Internet Explorer:

- `https://device.login.microsoftonline.com`

- El servicio de token de seguridad (STS - dominios federados) de su organización

- `https://autologon.microsoftazuread-sso.com` (para el SSO de conexión directa).

Además, tiene que habilitar **Permitir actualizaciones en la barra de estado a través de script** en la zona de intranet local del usuario.



### <a name="control-windows-down-level-devices"></a>Control de dispositivos de Windows de nivel inferior 

Para registrar dispositivos de nivel inferior de Windows, debe descargar e instalar un paquete de Windows Installer (.msi) desde el Centro de descarga. Para más información, haga clic [aquí](hybrid-azuread-join-control.md#control-windows-down-level-devices). 

## <a name="verify-the-registration"></a>Comprobación del registro

Para comprobar el estado de registro del dispositivo en su inquilino de Azure, puede usar el cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** en el **[módulo de PowerShell de Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**.

Cuando se usa el cmdlet **Get-MSolDevice** para comprobar los detalles de servicio:

- Tiene que existir un objeto con el **Identificador de dispositivo** que coincida con el identificador en el cliente de Windows.
- El valor de **DeviceTrustType** tiene que ser **Unido a dominio**. Esto es equivalente al estado **Unido a Azure AD híbrido** en la página de dispositivos en el portal de Azure AD.
- En el caso de los dispositivos que se usan en el acceso condicional, el valor de **Habilitado** tiene que ser **True** y el de **DeviceTrustLevel** tiene que ser **Administrado**. 


**Para comprobar los detalles del servicio:**

1. Abra **Windows PowerShell** como administrador.

2. Escriba `Connect-MsolService` para conectarse a su inquilino de Azure.  

3. Escriba `get-msoldevice -deviceId <deviceId>`.

6. Compruebe que **Habilitado** está establecido en **True**.





## <a name="troubleshoot-your-implementation"></a>Solución de problemas de la implementación

Si tiene problemas para completar la unión a Azure AD híbrido para los dispositivos de Windows unidos a un dominio, consulte:

- [Solución de problemas de la unión a Azure AD híbrido para dispositivos actuales de Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solución de problemas de la unión a Azure AD híbrido para dispositivos de nivel inferior de Windows](troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la unión a Azure Active Directory híbrido para dominios administrados](hybrid-azuread-join-managed-domains.md)
> [Configuración de la unión a Azure Active Directory híbrido de forma manual](hybrid-azuread-join-manual-steps.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
