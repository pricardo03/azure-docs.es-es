---
title: 'Guía de inicio rápido: dirigir el tráfico web con Azure Application Gateway: Azure Portal | Microsoft Docs'
description: Obtenga información acerca de cómo utilizar Azure Portal para crear una instancia de Azure Application Gateway que dirija el tráfico web a las máquinas virtuales de un grupo de back-end.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 7e11affece7e7eb133aa22e159ec07d4f15e96f7
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999597"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Inicio rápido: Dirección del tráfico web con Azure Application Gateway: Azure Portal

Con Azure Application Gateway, puede dirigir el tráfico web de la aplicación a recursos específicos mediante la asignación de agentes de escucha a los puertos, la creación de reglas y la adición de recursos a un grupo de back-end.

En esta guía de inicio rápido se muestra cómo utilizar Azure Portal para crear rápidamente la puerta de enlace de aplicaciones con dos máquinas virtuales en el grupo de back-end. A continuación, se prueba para asegurarse de que funciona correctamente.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Se necesita una red virtual para la comunicación entre los recursos que se crean. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Haga clic en **Redes** y luego en **Application Gateway** en la lista de destacados.

### <a name="basics"></a>Aspectos básicos

1. Especifique estos valores para la puerta de enlace de aplicaciones:

    - *myAppGateway*: como nombre de la puerta de enlace de aplicaciones.
    - *myResourceGroupAG*: como nuevo grupo de recursos.

    ![Creación de una nueva puerta de enlace de aplicaciones](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Acepte los valores predeterminados para las demás opciones y haga clic en **Aceptar**.

### <a name="settings"></a>Configuración

1. Haga clic en **Elegir una red virtual**, luego en **Crear nueva** y, después, especifique estos valores para la red virtual:

    - *myVNet*: como nombre de la red virtual.
    - *10.0.0.0/16*: como espacio de direcciones de la red virtual.
    - *myAGSubnet*: como nombre de subred.
    - *10.0.0.0/24*: como intervalo de direcciones de la subred.

    ![Creación de una red virtual](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Haga clic en **Aceptar** para volver a la página Configuración.
7. En **Configuración de IP de front-end**, asegúrese de que **Tipo de dirección IP** esté establecido en **Pública** y, en **Dirección IP pública**, asegúrese de que **Crear nuevo** esté seleccionado. Escriba *myAGPublicIPAddress* para el nombre de la dirección IP pública. Acepte los valores predeterminados para las demás opciones y haga clic en **Aceptar**.

### <a name="summary"></a>Resumen

Revise la configuración en la página de resumen y haga clic en **Aceptar** para crear la red virtual, la dirección IP pública y la puerta de enlace de aplicaciones. La puerta de enlace de aplicaciones puede tardar varios minutos en crearse. Espere hasta que finalice la implementación correctamente antes de continuar con la siguiente sección.

## <a name="add-a-subnet"></a>Incorporación de una subred

1. Haga clic en **Todos los recursos** en el menú izquierdo y, después, haga clic en **myVNet** en la lista de recursos.
2. Haga clic en **Subredes** y en **+Subred**.

    ![Creación de una subred](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Escriba *myBackendSubnet* como nombre de la subred y, a continuación, haga clic en **Aceptar**.

## <a name="create-backend-servers"></a>Creación de servidores back-end

En este ejemplo, se crean dos máquinas virtuales que se usan como servidores back-end para la puerta de enlace de aplicaciones. También se instala IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.

### <a name="create-a-virtual-machine"></a>de una máquina virtual

1. En Azure Portal, haga clic en **Crear un recurso**.
2. Haga clic en **Compute** y, después, seleccione **Windows Server 2016 Datacenter** en la lista de destacados.
3. Especifique estos valores para la máquina virtual:

    - *myResourceGroupAG*: como grupo de recursos.
    - *myVM*: como nombre de la máquina virtual.
    - *azureuser*: como nombre del usuario administrador.
    - *Azure123456!* como contraseña.

   Acepte los valores predeterminados y haga clic en **Siguiente: Discos**.
4. Acepte los valores predeterminados de disco y haga clic en **Siguiente: Redes**.
5. Asegúrese de que **myVNet** está seleccionada como red virtual y que la subred es **myBackendSubnet**.
6. Acepte los valores predeterminados y haga clic en **Siguiente: Administración**.
7. Haga clic en **Desactivar** para deshabilitar los diagnósticos de arranque. Acepte los otros valores predeterminados y haga clic en **Revisar y crear**.
8. Revise la configuración en la página de resumen y haga clic en **Crear**.
9. Espere a que se complete la creación de la máquina virtual antes de continuar.

### <a name="install-iis"></a>Instalación de IIS

1. Abra el shell interactivo y asegúrese de que está establecido en **PowerShell**.

    ![Instalación de la extensión personalizada](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Ejecute el siguiente comando para instalar IIS en la máquina virtual: 

    ```azurepowershell-interactive
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Cree una segunda máquina virtual e instale IIS según los pasos que acaba de finalizar. Escriba *myVM2* como nombre y como VMName en Set-AzureRmVMExtension.

### <a name="add-backend-servers"></a>Incorporación de servidores back-end

1. Haga clic en **Todos los recursos** y en **myAppGateway**.
4. Haga clic en **Grupos de back-end**. Con la puerta de enlace de aplicaciones se crea un grupo predeterminado. Haga clic en **appGatewayBackendPool**.
5. En **Destinos**, haga clic en **Dirección IP o FQDN** y seleccione **Máquina virtual**.
6. En **Máquina virtual**, agregue las máquinas virtuales myVM y myVM2 y sus interfaces de red asociadas.

    ![Incorporación de servidores back-end](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Haga clic en **Save**(Guardar).

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

1. Busque la dirección IP pública de la puerta de enlace de aplicaciones en la pantalla de información general. Haga clic en **Todos los recursos** y en **myAGPublicIPAddress**.

    ![Registro de la dirección IP pública de la puerta de enlace de aplicaciones](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador.

    ![Prueba de la puerta de enlace de aplicaciones](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, la puerta de enlace de aplicaciones y todos los recursos relacionados. Para ello, seleccione el grupo de recursos que contiene la puerta de enlace de aplicaciones y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con Application Gateway mediante la CLI de Azure](./tutorial-manage-web-traffic-cli.md)
