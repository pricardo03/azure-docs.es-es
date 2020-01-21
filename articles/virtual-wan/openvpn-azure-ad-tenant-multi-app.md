---
title: 'VPN Gateway: inquilino de Azure AD para distintos grupos de usuarios: Autenticación de Azure AD'
description: Puede usar la P2S VPN para conectarse a la red virtual con la autenticación de Azure AD
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: alzam
ms.openlocfilehash: b390b5f8b00f61994db820a3af7bce26a3e0a30d
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934889"
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
> [!NOTE]
> Do not use the Azure VPN client's application ID in the commands above as it will grant all users access to the VPN gateway. Use the ID of the application(s) you registered.

## <a name="hub"></a>7. Edit hub assignment

1. Navigate to the **Hubs** blade under the virtual WAN.
2. Select the hub that you want to associate the vpn server configuration to and click the ellipsis (...).

   ![new site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Click **Edit virtual hub**.
4. Check the **Include point-to-site gateway** check box and pick the **Gateway scale unit** that you want.

   ![new site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Enter the **Address pool** from which the VPN clients will be assigned IP addresses.
6. Click **Confirm**.
7. The operation will can take up to 30 minutes to complete.

## <a name="device"></a>8. Download VPN profile

Use the VPN profile to configure your clients.

1. On the page for your virtual WAN, click **User VPN configurations**.
2. At the top of the  page, click **Download user VPN config**.
3. Once the file has finished creating, you can click the link to download it.
4. Use the profile file to configure the VPN clients.

4. Extract the downloaded zip file.

5. Browse to the unzipped “AzureVPN” folder.

6. Make a note of the location of the “azurevpnconfig.xml” file. The azurevpnconfig.xml contains the setting for the VPN connection and can be imported directly into the Azure VPN Client application. You can also distribute this file to all the users that need to connect via e-mail or other means. The user will need valid Azure AD credentials to connect successfully.
## Configure user VPN clients

To connect, you need to download the Azure VPN Client (Preview) and import the VPN client profile that was downloaded in the previous steps on every computer that wants to connect to the VNet.

> [!NOTE]
> Azure AD authentication is supported only for OpenVPN® protocol connections.
>

#### To download the Azure VPN client

Use this [link](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) to download the Azure VPN Client (Preview).

#### <a name="import"></a>To import a client profile

1. On the page, select **Import**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Browse to the profile xml file and select it. With the file selected, select **Open**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Specify the name of the profile and select **Save**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Select **Connect** to connect to the VPN.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Once connected, the icon will turn green and say **Connected**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>To delete a client profile

1. Select the ellipsis (...) next to the client profile that you want to delete. Then, select **Remove**.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Select **Remove** to delete.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Diagnose connection issues

1. To diagnose connection issues, you can use the **Diagnose** tool. Select the ellipsis (...) next to the VPN connection that you want to diagnose to reveal the menu. Then select **Diagnose**.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. On the **Connection Properties** page, select **Run Diagnosis**.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Sign in with your credentials.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. View the diagnosis results.

    ![diagnose](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>View your virtual WAN

1. Navigate to the virtual WAN.
2. On the Overview page, each point on the map represents a hub. Hover over any point to view the hub health summary.
3. In the Hubs and connections section, you can view hub status, site, region, VPN connection status, and bytes in and out.

## <a name="viewhealth"></a>View your resource health

1. Navigate to your WAN.
2. On your WAN page, in the **SUPPORT + Troubleshooting** section, click **Health** and view your resource.


## <a name="cleanup"></a>Clean up resources

When you no longer need these resources, you can use [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) to remove the resource group and all of the resources it contains. Replace "myResourceGroup" with the name of your resource group and run the following PowerShell command:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).