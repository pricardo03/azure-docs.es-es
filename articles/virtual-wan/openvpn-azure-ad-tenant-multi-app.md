---
title: 'VPN Gateway: inquilino de Azure AD para distintos grupos de usuarios: Autenticación de Azure AD'
description: Puede usar la P2S VPN para conectarse a la red virtual con la autenticación de Azure AD
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: alzam
ms.openlocfilehash: 90244b9dcf30c2ef01d4e57c9d8e35fa1d71f434
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985652"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Creación de un inquilino de Azure Active Directory para conexiones del protocolo P2S OpenVPN

Al conectarse a la red virtual, puede usar la autenticación basada en certificados o la autenticación RADIUS. Sin embargo, cuando use el protocolo de VPN abierto, también puede usar la autenticación de Azure Active Directory. Si quiere que un conjunto de usuarios diferente pueda conectarse a diferentes puertas de enlace de VPN, puede registrar varias aplicaciones en AD y vincularlas a diferentes puertas de enlace de VPN. Este artículo le ayuda a configurar un inquilino de Azure AD para la autenticación de OpenVPN de P2S y a crear y registrar varias aplicaciones en Azure AD a fin de permitir un acceso distinto a los diferentes usuarios y grupos.

> [!NOTE]
> La autenticación de Azure AD solo se admite para las conexiones de protocolo de OpenVPN®.
>

## <a name="tenant"></a>1. Cree un inquilino Azure AD

Cree un inquilino de Azure AD con los pasos del artículo [Crear un nuevo inquilino](../active-directory/fundamentals/active-directory-access-create-new-tenant.md):

* Nombre organizativo
* Nombre de dominio inicial

Ejemplo:

   ![Nuevo inquilino Azure AD](./media/openvpn-azure-ad-tenant-multi-app/newtenant.png)

## <a name="users"></a>2. Crear usuario de inquilino Azure AD

Después, cree dos cuentas de usuario. Cree una cuenta de Administrador global y una cuenta de usuario principal. La cuenta de usuario principal se usa como cuenta de inserción maestra (cuenta de servicio). Al crear una cuenta de usuario de inquilino Azure AD, ajuste el rol de directorio para el tipo de usuario que desea crear.

Siga los pasos descritos en [este artículo](../active-directory/fundamentals/add-users-azure-active-directory.md) para crear al menos dos usuarios para su inquilino de Azure AD. Asegúrese de cambiar el **rol de directorio** para crear los tipos de cuenta:

* Administrador global
* Usuario

## <a name="enable-authentication"></a>3. Registrar un cliente de VPN de Azure en el inquilino de Azure AD

1. Busque el ID. de directorio del directorio que desea utilizar para la autenticación. Aparece en la sección propiedades de la página Active Directory.

    ![Identificador de directorio](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

2. Copie el ID. del directorio.

3. Inicie sesión en el Azure Portal como un usuario al que se le ha asignado el rol de **administrador global**.

4. Después, ceda el consentimiento del administrador. Copie y pegue la dirección URL que pertenece a la ubicación de implementación en la barra de direcciones del explorador:

    Público

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Germany

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. Seleccione la cuenta del **Administrador global** si se le solicita.

    ![Identificador de directorio](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. Seleccione **Aceptar** cuando se le solicite.

    ![Accept](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. En Azure AD, en **Aplicaciones empresariales**, verá la **VPN de Azure** en la lista.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/azurevpn.png)

## <a name="enable-authentication"></a>4. Registrar aplicaciones adicionales para varios usuarios o grupos

1. En la página Azure Active Directory, haga clic en **Registros de aplicaciones** y en **+ Nuevo registro**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. En la hoja **Registrar una aplicación**, escriba el **nombre**, seleccione los **tipos de cuenta admitidos** que quiera y haga clic en **Registrar**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Una vez que se haya registrado la nueva aplicación, haga clic en **Exponer una API** en la hoja de la aplicación.

4. Haga clic en **+ Agregar un ámbito**.
5. Deje el **URI de identificador de aplicación** predeterminado y haga clic en **Guardar y continuar**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app3.png)
6. Rellene los campos obligatorios y asegúrese de que la opción **Estado** está **habilitada**. Haga clic en **Agregar ámbito**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app4.png)
7. Haga clic en **Exponer una API** y, a continuación, **+ Agregar una aplicación cliente**.  En **Id. de cliente**, escriba los valores siguientes en función de la nube:
    -   Escriba **41b23e61-6c1e-4545-B367-cd054e0ed4b4** para Azure **público**.
    -   Escriba **51bb15d4-3a4f-4ebf-9dca-40096fe32426** para Azure **Government**.
    -   Escriba **538ee9e6-310a-468d-afef-ea97365856a9** para Azure **Alemania**.
    -   Escriba **49f817b6-84ae-4cc0-928c-73f27289b3aa** para Azure **China 21Vianet**.


8. Haga clic en **Agregar aplicación**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app5.png)
9. En la página **Introducción**, copie el **identificador de aplicación (cliente)** . Lo necesitará para configurar las puertas de enlace de VPN.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Repita los pasos de esta sección (4) para crear tantas aplicaciones como sea necesario para su requisito de seguridad. Cada aplicación se asociará a una puerta de enlace de VPN y puede tener un conjunto diferente de usuarios. Solo se puede asociar una aplicación a una puerta de enlace.

## <a name="enable-authentication"></a>5. Asignar usuarios a aplicaciones

1. En Azure AD, **Aplicaciones empresariales**, seleccione la aplicación recién registrada y haga clic en **Propiedades**. Asegúrese de que la opción **¿Asignación de usuarios?** esté establecida en **Sí**. Haga clic en **Guardar**

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. En la página de la aplicación, haga clic en **Usuarios y grupos** y, después, en **Agregar usuario**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user3.png)
3. En **Agregar asignación**, haga clic en **Usuarios y grupos**. Seleccione los usuarios que quiere que puedan tener acceso a esta aplicación de VPN. Haga clic en **Seleccionar**

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user4.png)

## <a name="site"></a>6. Creación de una nueva configuración de P2S

Una configuración de P2S define los parámetros para conectarse a los clientes remotos.

1. Establezca las siguientes variables y reemplace los valores según sea necesario para su entorno.

   ```powershell
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Ejecute los siguientes comandos para crear la configuración:

   ```powershell
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
   ```

> [!NOTE]
> No use el identificador de la aplicación del cliente de VPN de Azure en los comandos anteriores, ya que se concederá acceso a todos los usuarios a la puerta de enlace de VPN. Use el identificador de las aplicaciones que registró.

## <a name="hub"></a>7. Edición de la asignación del concentrador

1. Vaya a la hoja **Centros de conectividad** de la red virtual WAN.
2. Seleccione el concentrador al que desea asociar la configuración del servidor VPN y haga clic en los puntos suspensivos (...).

   ![nuevo sitio](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Haga clic en **Editar centro de conectividad virtual**.
4. Active la casilla **Incluir puerta de enlace de punto a sitio** y elija la **unidad de escalado de puerta de enlace** que quiera.

   ![nuevo sitio](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Especifique el **Grupo de direcciones** del que se asignarán direcciones IP a los clientes VPN.
6. Haga clic en **Confirmar**.
7. La operación puede tardar hasta 30 minutos en completarse.

## <a name="device"></a>8. Descarga del perfil de VPN

Use el perfil de VPN para configurar los clientes.

1. En la página de su red WAN virtual, haga clic en **Configuraciones de VPN de usuario**.
2. En la parte superior de la página, haga clic en **Download user VPN config** (Descargar configuración de VPN de usuario).
3. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.
4. Use el archivo de perfil para configurar los clientes de VPN.

4. Extraiga el archivo zip descargado.

5. Busque la carpeta "AzureVPN" descomprimida.

6. Anote la ubicación del archivo "azurevpnconfig. xml". Azurevpnconfig. xml contiene la configuración de la conexión VPN y se puede importar directamente en la aplicación del cliente VPN Azure. También puede distribuir este archivo a todos los usuarios que necesiten conectarse a través del correo electrónico u otros medios. El usuario necesitará credenciales de Azure AD válidas para conectarse correctamente.
## <a name="configure-user-vpn-clients"></a>Configuración de clientes de VPN de usuario

Para conectarse, debe descargar el cliente de VPN de Azure (versión preliminar) e importar el perfil de cliente de VPN que descargó en los pasos anteriores en todos los equipos que quieran conectarse a la red virtual.

> [!NOTE]
> La autenticación de Azure AD solo se admite para las conexiones de protocolo de OpenVPN®.
>

#### <a name="to-download-the-azure-vpn-client"></a>Para descargar el cliente VPN de Azure

Use este [vínculo](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) para descargar el cliente VPN de Azure (versión preliminar).

#### <a name="import"></a>Para importar un perfil de cliente

1. En la página, seleccione **Importar**.

    ![importación](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Busque el archivo xml de perfil y selecciónelo. Con el archivo seleccionado, seleccione **Abrir**.

    ![importación](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Especifique el nombre del perfil y seleccione **Guardar**.

    ![importación](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Seleccione **Conectar** para conectarse a la VPN.

    ![importación](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Una vez conectado, el icono se volverá verde y dirá**Conectado**.

    ![importación](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>Para eliminar un perfil de cliente

1. Seleccione los puntos suspensivos junto al perfil de cliente que quiera eliminar. Después, seleccione **Quitar**.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Seleccione **Quitar** para eliminar.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Diagnóstico de problemas de conexión

1. Para diagnosticar problemas de conexión, puede usar la herramienta **Diagnosticar**. Seleccione los puntos suspensivos (...) junto a la conexión de VPN que desea diagnosticar para que se muestre el menú. Después, seleccione **Diagnosticar**.

    ![diagnóstico](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. En la página **Propiedades de conexión**, seleccione **Ejecutar diagnóstico**.

    ![diagnóstico](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Inicie sesión con sus credenciales.

    ![diagnóstico](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Ver los resultados del diagnóstico.

    ![diagnóstico](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>Visualización de la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, cada punto del mapa representa un concentrador. Mantenga el mouse sobre cualquier punto para ver el resumen de estado del concentrador.
3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="viewhealth"></a>Visualización del estado de los recursos

1. Vaya a su red WAN.
2. En la página de la WAN, en la sección de **Soporte técnico y solución de problemas**, haga clic en **Mantenimiento** y visualice los recursos.


## <a name="cleanup"></a>Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
