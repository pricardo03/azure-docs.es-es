---
title: 'VPN Gateway: inquilino de Azure AD para distintos grupos de usuarios: Autenticación de Azure AD'
description: Puede usar la P2S VPN para conectarse a la red virtual con la autenticación de Azure AD
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: alzam
ms.openlocfilehash: 73a47b20010be0b4eadec790dd8fb254f912ff2a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75474122"
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

## <a name="enable-authentication"></a>6. Habilitación de la autenticación de Azure AD

1. Habilite la autenticación Azure AD en la VPN Gateway con la ejecución de los siguientes comandos y asegúrese de modificar el comando para que refleje su propio entorno:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
> [!NOTE]
> No use el identificador de la aplicación del cliente de VPN de Azure en los comandos anteriores, ya que se concederá acceso a todos los usuarios a la puerta de enlace de VPN. Use el identificador de las aplicaciones que registró.

2. Ejecute los siguientes comandos para crear y descargar el perfil. Cambie los valores de-ResourcGroupName y-Name para que coincidan con los suyos.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. Después de ejecutar los comandos, verá un resultado similar al siguiente. Copie la dirección URL del resultado en el explorador para descargar el archivo zip del perfil.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. Extraiga el archivo zip descargado.

5. Busque la carpeta "AzureVPN" descomprimida.

6. Anote la ubicación del archivo "azurevpnconfig. xml". Azurevpnconfig. xml contiene la configuración de la conexión VPN y se puede importar directamente en la aplicación del cliente VPN Azure. También puede distribuir este archivo a todos los usuarios que necesiten conectarse a través del correo electrónico u otros medios. El usuario necesitará credenciales de Azure AD válidas para conectarse correctamente.

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a la red virtual, debe crear y configurar un perfil de cliente de VPN. Consulte [Configurar un cliente VPN para conexiones P2S VPN](openvpn-azure-ad-client.md).
