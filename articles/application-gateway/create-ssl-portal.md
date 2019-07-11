---
title: 'Tutorial: Configuración de una puerta de enlace de aplicaciones con terminación SSL (Azure Portal)'
description: En este tutorial, aprenderá a configurar una puerta de enlace de aplicaciones y a agregar un certificado para la terminación SSL mediante Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/17/2019
ms.author: victorh
ms.openlocfilehash: ed4230969e81eee0d77b7e4b69eac3a264068388
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449165"
---
# <a name="tutorial-configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Tutorial: Configuración de una puerta de enlace de aplicaciones con terminación SSL mediante de Azure Portal

Puede usar Azure Portal para configurar una [puerta de enlace de aplicaciones](overview.md) con un certificado para terminación SSL que use máquinas virtuales para servidores back-end.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de un certificado autofirmado
> * Crear una puerta de enlace de aplicaciones con el certificado
> * Crear las máquinas virtuales que se utilizan como servidores back-end
> * Prueba de la puerta de enlace de aplicaciones

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-self-signed-certificate"></a>Creación de un certificado autofirmado

En esta sección, usará [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) para crear un certificado autofirmado. Cargará el certificado en Azure Portal al crear el agente de escucha para la puerta de enlace de aplicaciones.

En el equipo local, abra una ventana de Windows PowerShell como administrador. Ejecute el siguiente comando para crear el certificado:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

Debería ver algo parecido a esta respuesta:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

Use [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) con la huella digital que se devolvió al exportar un archivo pfx del certificado:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Se necesita una red virtual para la comunicación entre los recursos que se crean. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones.

1. Seleccione **Nuevo** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Application Gateway** en la lista de destacados.
3. Escriba *myAppGateway* para el nombre de la puerta de enlace de aplicaciones y *myResourceGroupAG* para el nuevo grupo de recursos.
4. Acepte los valores predeterminados para las demás opciones y seleccione **Aceptar**.
5. Seleccione **Elegir una red virtual**, **Crear nueva** y, después, especifique estos valores para la red virtual:

   - *myVNet*: como nombre de la red virtual.
   - *10.0.0.0/16*: como espacio de direcciones de la red virtual.
   - *myAGSubnet*: como nombre de subred.
   - *10.0.0.0/24*: como espacio de direcciones de la subred.

     ![Creación de una red virtual](./media/create-ssl-portal/application-gateway-vnet.png)

6. Seleccione **Aceptar** para crear la red virtual y la subred.
7. Seleccione **Elegir una dirección IP pública**, **Crear nueva** y, luego, escriba el nombre de la dirección IP pública. En este ejemplo, la dirección IP pública se llama *myAGPublicIPAddress*. Acepte los valores predeterminados para las demás opciones y seleccione **Aceptar**.
8. Seleccione **HTTPS** como protocolo del agente de escucha y asegúrese de que el puerto esté definido como **443**.
9. Seleccione el icono de la carpeta y busque el certificado *appgwcert.pfx* que creó anteriormente para cargarlo.
10. Escriba *mycert1* para el nombre del certificado y *Azure123456!* para la contraseña y, después, seleccione **Aceptar**.

    ![Creación de una nueva puerta de enlace de aplicaciones](./media/create-ssl-portal/application-gateway-create.png)

11. Revise la configuración en la página de resumen y, a continuación, seleccione **Aceptar** para crear los recursos de red y la puerta de enlace de aplicaciones. La creación de la puerta de enlace de aplicaciones puede tardar varios minutos, espere a que finalice correctamente la implementación antes de pasar a la sección siguiente.

### <a name="add-a-subnet"></a>Incorporación de una subred

1. Seleccione **Todos los recursos** en el menú izquierdo y, después, seleccione **myVNet** en la lista de recursos.
2. Seleccione **Subredes** y, después, **Subred**.

    ![Creación de una subred](./media/create-ssl-portal/application-gateway-subnet.png)

3. Escriba *myBackendSubnet* como nombre de la subred y, luego, seleccione **Aceptar**.

## <a name="create-backend-servers"></a>Creación de servidores back-end

En este ejemplo, se crearán dos máquinas virtuales que se usarán como servidores back-end para la puerta de enlace de aplicaciones. También puede instalar IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.

### <a name="create-a-virtual-machine"></a>de una máquina virtual

1. Seleccione **Nuevo**.
2. Haga clic en **Compute** y, a continuación, seleccione **Windows Server 2016 Datacenter** en la lista de destacados.
3. Especifique estos valores para la máquina virtual:

    - *myVM*: como nombre de la máquina virtual.
    - *azureuser*: como nombre del usuario administrador.
    - *Azure123456!* como contraseña.
    - Seleccione **Usar existente** y *myResourceGroupAG*.

4. Seleccione **Aceptar**.
5. Seleccione **DS1_V2** como tamaño de la máquina virtual y haga clic en **Seleccionar**.
6. Asegúrese de que **myVNet** está seleccionada como red virtual y que la subred es **myBackendSubnet**. 
7. Seleccione **Deshabilitado** para deshabilitar los diagnósticos de arranque.
8. Seleccione **Aceptar**, revise la configuración en la página de resumen y seleccione **Crear**.

### <a name="install-iis"></a>Instalación de IIS

1. Abra el shell interactivo y asegúrese de que está establecido en **PowerShell**.

    ![Instalación de la extensión personalizada](./media/create-ssl-portal/application-gateway-extension.png)

2. Ejecute el siguiente comando para instalar IIS en la máquina virtual: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Cree una segunda máquina virtual e instale IIS según los pasos que acaba de finalizar. Escriba *myVM2* como su nombre y como valor de VMName en Set-AzVMExtension.

### <a name="add-backend-servers"></a>Incorporación de servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.
1. Seleccione **Grupos de back-end**. Con la puerta de enlace de aplicaciones se crea un grupo predeterminado. Seleccione **appGatewayBackendPool**.
1. Seleccione **Agregar destino** para agregar las máquinas virtuales creadas al grupo back-end.

    ![Incorporación de servidores back-end](./media/create-ssl-portal/application-gateway-backend.png)

1. Seleccione **Guardar**.

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

1. Seleccione **Todos los recursos** y, después, **myAGPublicIPAddress**.

    ![Registro de la dirección IP pública de la puerta de enlace de aplicaciones](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador. Para aceptar la advertencia de seguridad si usó un certificado autofirmado, seleccione Detalles y, a continuación, Acceder a la página web:

    ![Advertencia de seguridad](./media/create-ssl-portal/application-gateway-secure.png)

    El sitio web IIS protegido se muestra ahora como en el ejemplo siguiente:

    ![Prueba de la dirección URL base en la puerta de enlace de aplicaciones](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre lo que se puede hacer con Azure Application Gateway](application-gateway-introduction.md)
