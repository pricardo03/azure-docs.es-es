---
title: 'Tutorial: Creación de una puerta de enlace de aplicaciones con reglas de enrutamiento basadas en rutas de dirección URL con Azure Portal'
description: En este tutorial, aprenderá a crear reglas de enrutamiento basadas en rutas de dirección URL para una puerta de enlace de aplicaciones y un conjunto de escalado de máquinas virtuales mediante Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 5307f7674635fd33241e1faba9bb0b7c0432d10b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134889"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Tutorial: Creación de una puerta de enlace de aplicaciones con reglas de enrutamiento basadas en rutas de dirección URL con Azure Portal

Puede usar Azure Portal para configurar [reglas de enrutamiento basadas en rutas de dirección URL](url-route-overview.md) cuando se crea una [puerta de enlace de aplicaciones](overview.md). En este tutorial, creará grupos de back-end mediante el uso de máquinas virtuales. A continuación, creará reglas de enrutamiento que garanticen que el tráfico web llega a los servidores adecuados de los grupos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una puerta de enlace de aplicaciones
> * Crear máquinas virtuales para servidores back-end
> * Crear grupos de back-end con los servidores back-end
> * Crear un agente de escucha de back-end
> * Crear una regla de enrutamiento basada en la ruta de acceso

![Ejemplo de enrutamiento de direcciones URL](./media/create-url-route-portal/scenario.png)

Si lo prefiere, puede completar este tutorial con la [CLI de Azure](tutorial-url-route-cli.md) o [Azure PowerShell](tutorial-url-route-powershell.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Se necesita una red virtual para la comunicación entre los recursos que se crean. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones.

1. Seleccione **Nuevo** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Redes** y **Application Gateway** en la lista de destacados.
3. Especifique estos valores para la puerta de enlace de aplicaciones:

   - *myAppGateway*: como nombre de la puerta de enlace de aplicaciones.
   - *myResourceGroupAG*: como nuevo grupo de recursos.

     ![Creación de una nueva puerta de enlace de aplicaciones](./media/create-url-route-portal/application-gateway-create.png)

4. Acepte los valores predeterminados para las demás opciones y seleccione **Aceptar**.
5. Seleccione **Elegir una red virtual**, **Crear nueva** y, después, especifique estos valores para la red virtual:

   - *myVNet*: como nombre de la red virtual.
   - *10.0.0.0/16*: como espacio de direcciones de la red virtual.
   - *myAGSubnet*: como nombre de subred.
   - *10.0.0.0/24*: como espacio de direcciones de la subred.

     ![Creación de una red virtual](./media/create-url-route-portal/application-gateway-vnet.png)

6. Seleccione **Aceptar** para crear la red virtual y la subred.
7. Seleccione **Elegir una dirección IP pública**, **Crear nueva** y, luego, escriba el nombre de la dirección IP pública. En este ejemplo, la dirección IP pública se llama *myAGPublicIPAddress*. Acepte los valores predeterminados para las demás opciones y seleccione **Aceptar**.
8. Acepte los valores predeterminados para la configuración del agente de escucha, deje el firewall de aplicaciones web deshabilitado y seleccione **Aceptar**.
9. Revise la configuración en la página de resumen y, a continuación, seleccione **Aceptar** para crear los recursos de red y la puerta de enlace de aplicaciones. La creación de la puerta de enlace de aplicaciones puede tardar varios minutos, espere a que finalice correctamente la implementación antes de pasar a la sección siguiente.

### <a name="add-a-subnet"></a>Incorporación de una subred

1. Seleccione **Todos los recursos** en el menú izquierdo y, después, seleccione **myVNet** en la lista de recursos.
2. Seleccione **Subredes** y, después, **Subred**.

    ![Creación de una subred](./media/create-url-route-portal/application-gateway-subnet.png)

3. Escriba *myBackendSubnet* como nombre de la subred y, luego, seleccione **Aceptar**.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

En este ejemplo, se crean tres máquinas virtuales que se usarán como servidores back-end para la puerta de enlace de aplicaciones. También puede instalar IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.

1. Seleccione **Nuevo**.
2. Haga clic en **Compute** y, a continuación, seleccione **Windows Server 2016 Datacenter** en la lista de destacados.
3. Especifique estos valores para la máquina virtual:

    - *myVM1*: para el nombre de la máquina virtual.
    - *azureuser*: como nombre del usuario administrador.
    - *Azure123456!* como contraseña.
    - Seleccione **Usar existente** y *myResourceGroupAG*.

4. Seleccione **Aceptar**.
5. Seleccione **DS1_V2** como tamaño de la máquina virtual y haga clic en **Seleccionar**.
6. Asegúrese de que **myVNet** está seleccionada como red virtual y que la subred es **myBackendSubnet**. 
7. Seleccione **Deshabilitado** para deshabilitar los diagnósticos de arranque.
8. Seleccione **Aceptar**, revise la configuración en la página de resumen y seleccione **Crear**.

### <a name="install-iis"></a>Instalación de IIS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Abra el shell interactivo y asegúrese de que está establecido en **PowerShell**.

    ![Instalación de la extensión personalizada](./media/create-url-route-portal/application-gateway-extension.png)

2. Ejecute el siguiente comando para instalar IIS en la máquina virtual: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Cree dos máquinas virtuales más e instale IIS siguiendo los pasos que acaba de finalizar. Escriba los nombres de *myVM2* y *myVM3* para los nombres y los valores de VMName en Set-AzVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Creación de grupos de servidores back-end con las máquinas virtuales

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.
2. Seleccione **Grupos de back-end**. Con la puerta de enlace de aplicaciones se crea un grupo predeterminado. Seleccione **appGatewayBackendPool**.
3. Seleccione **Agregar destino** para agregar *myVM1* a appGatewayBackendPool.

    ![Incorporación de servidores back-end](./media/create-url-route-portal/application-gateway-backend.png)

4. Seleccione **Guardar**.
5. Seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.
6. Escriba un nombre para *imagesBackendPool* y agregue *myVM2* con **Agregar destino**.
7. Seleccione **Aceptar**.
8. Seleccione de nuevo **Agregar** para agregar otro grupo de servidores back-end con el nombre *videoBackendPool* y agregue *myVM3* a dicho grupo.

## <a name="create-a-backend-listener"></a>Crear un agente de escucha de back-end

1. Seleccione **Agentes de escucha** y, a continuación, seleccione **Básico**.
2. Escriba *myBackendListener* para el nombre, *myFrontendPort* para el nombre del puerto de front-end y, a continuación, *8080* como el puerto para el agente de escucha.
3. Seleccione **Aceptar**.

## <a name="create-a-path-based-routing-rule"></a>Crear una regla de enrutamiento basada en la ruta de acceso

1. Seleccione **Reglas** y, a continuación, seleccione **Basada en ruta de acceso**.
2. Escriba *rule2* para el nombre.
3. Escriba *Images* para el nombre de la primera ruta de acceso. Escriba */images /*\* para la ruta de acceso. Seleccione **imagesBackendPool** para el grupo de servidores back-end.
4. Escriba *Video* para el nombre de la segunda ruta de acceso. Escriba */video/*\* para la ruta de acceso. Seleccione **videoBackendPool** para el grupo de servidores back-end.

    ![Creación de una regla basada en ruta de acceso](./media/create-url-route-portal/application-gateway-route-rule.png)

5. Seleccione **Aceptar**.

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

1. Seleccione **Todos los recursos** y, después, **myAGPublicIPAddress**.

    ![Registro de la dirección IP pública de la puerta de enlace de aplicaciones](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador. Por ejemplo, http://40.121.222.19.

    ![Prueba de la dirección URL base en la puerta de enlace de aplicaciones](./media/create-url-route-portal/application-gateway-iistest.png)

3. Cambie la dirección URL a http://&lt;ip-address&gt;:8080/images/test.htm, sustituyendo &lt;ip-address&gt; por su dirección IP y verá algo similar al ejemplo siguiente:

    ![Prueba de la dirección URL de imágenes en la puerta de enlace de aplicaciones](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Cambie la dirección URL a http://&lt;ip-address&gt;:8080/video/test.htm, sustituyendo &lt;ip-address&gt; por su dirección IP y verá algo similar al ejemplo siguiente:

    ![Prueba de la dirección URL de vídeo en la puerta de enlace de aplicaciones](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con la puerta de enlace de aplicaciones, elimine el grupo de recursos. Mediante la eliminación del grupo de recursos también elimina la puerta de enlace de aplicaciones y todos sus recursos relacionados. 

Para eliminar el grupo de recursos:

1. En el menú de la izquierda de Azure Portal, seleccione **Grupos de recursos**.
2. En la página **Grupos de recursos**, busque **myResourceGroupAG** en la lista y selecciónelo.
3. En la **página del grupo de recursos**, seleccione **Eliminar grupo de recursos**.
4. Escriba *myResourceGroupAG* en **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre lo que se puede hacer con Azure Application Gateway](application-gateway-introduction.md)
