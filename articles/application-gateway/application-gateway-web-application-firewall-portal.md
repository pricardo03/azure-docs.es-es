---
title: 'Tutorial: Creación de una puerta de enlace de aplicaciones con un firewall de aplicaciones web mediante Azure Portal| Microsoft Docs'
description: En este tutorial, aprenda a crear una puerta de enlace de aplicaciones con un firewall de aplicaciones web mediante Azure Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/16/2019
ms.author: victorh
ms.openlocfilehash: 206895768ea48e352e4f7fe90ab597f3756586dd
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682855"
---
# <a name="tutorial-create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Tutorial: Creación de una puerta de enlace de aplicaciones con un firewall de aplicaciones web mediante Azure Portal

En este tutorial se muestra cómo usar Azure Portal para crear una [puerta de enlace de aplicaciones](application-gateway-introduction.md) con un [firewall de aplicaciones web](application-gateway-web-application-firewall-overview.md) (WAF). WAF usa reglas de [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) para proteger la aplicación. Estas reglas incluyen protección frente a ataques, como la inyección de SQL, ataques de scripts entre sitios y apropiaciones de sesión. Después de crear la puerta de enlace de aplicaciones, pruébela para asegurarse de que funciona correctamente. Con Azure Application Gateway, puede dirigir el tráfico web de la aplicación a recursos específicos mediante la asignación de agentes de escucha a los puertos, la creación de reglas y la adición de recursos a un grupo de back-end. Para simplificar, en este tutorial se usa una configuración sencilla con una dirección IP de front-end pública, un cliente de escucha básico para hospedar un único sitio en esta puerta de enlace de aplicaciones, dos máquinas virtuales que se usan con el grupo de back-end y una regla de enrutamiento de solicitudes básica.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una puerta de enlace de aplicaciones con WAF habilitado
> * Crear las máquinas virtuales que se utilizan como servidores back-end
> * Crear una cuenta de almacenamiento y configurar los diagnósticos
> * Prueba de la puerta de enlace de aplicaciones

![Ejemplo de firewall de aplicaciones web](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Si lo prefiere, puede completar este tutorial con [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md) o la [CLI de Azure](tutorial-restrict-web-traffic-cli.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Para que Azure se comunique entre los recursos, se necesita una red virtual. Puede crear una red virtual o usar una existente. En este ejemplo, creará una red virtual. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones. Se crean instancias de Application Gateway en subredes independientes. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end.

Seleccione **Crear un recurso** en el menú de la izquierda de Azure Portal. Aparece la ventana **Nuevo**.

Seleccione **Redes** y **Application Gateway** en la lista **Destacados**.

### <a name="basics-page"></a>Página Datos básicos

1. En la página **Datos básicos**, especifique estos valores para la siguiente configuración de puerta de enlace de aplicaciones:
   - **Nombre**: Escriba *myAppGateway* como nombre de la puerta de enlace de aplicaciones.
   - **Grupo de recursos**: Seleccione **myResourceGroupAG** como grupo de recursos. Si no existe, seleccione **Crear nuevo** para crearlo.
   - Seleccione *WAF* para el nivel de la puerta de enlace de aplicaciones.![Crear nueva puerta de enlace de aplicaciones](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

Acepte los valores predeterminados para las demás opciones y haga clic en **Aceptar**.

### <a name="settings-page"></a>Página Configuración

1. En la página **Configuración**, en **Configuración de subred**, seleccione **Elegir una red virtual**. <br>
2. En la página **Elegir una red virtual**, seleccione **Crear nuevo** y, a continuación, escriba valores para los siguientes valores de la red virtual:
   - **Nombre**: Escriba *myVnet* como nombre de la red virtual.
   - **Espacio de direcciones**: Especifique *10.0.0.0/16* como espacio de direcciones de la red virtual.
   - **Nombre de subred**: Especifique *myAGSubnet* como nombre de subred.<br>La subred de la puerta de enlace de aplicaciones solo puede contener puertas de enlace de aplicaciones. No se permite ningún otro recurso.
   - **Rango de direcciones de subred**: Escriba *10.0.0.0/24* para el intervalo de direcciones de subred.![Crear red virtual](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)
3. Haga clic en **Aceptar** para crear la red virtual y la subred.
4. Elija la **configuración IP de front-end**. En **Configuración de IP de front-end**, compruebe que **Tipo de dirección IP** está establecido en **Público**. En **Dirección IP pública**, compruebe que la opción **Crear nueva** está seleccionada. <br>Puede configurar la dirección IP de front-end para que sea pública o privada, según el caso de uso. Aquí puede elegir una dirección IP de front-end pública.
5. Escriba *myAGPublicIPAddress* para el nombre de la dirección IP pública.
6. Acepte los valores predeterminados para las demás opciones y seleccione **Aceptar**.<br>Por simplicidad, en este tutorial se elegirán valores predeterminados, pero pueden configurarse valores personalizados para las demás opciones según el caso de uso.

### <a name="summary-page"></a>Página de resumen

Revise la configuración en la página de **resumen** y seleccione **Aceptar** para crear la red virtual, la dirección IP pública y la puerta de enlace de aplicaciones. Azure puede tardar varios minutos en crear la puerta de enlace de aplicaciones. Espere hasta que finalice la implementación correctamente antes de continuar con la siguiente sección.

## <a name="add-backend-pool"></a>Agregar grupo de back-end

El grupo de back-end se usa para enrutar las solicitudes a los servidores back-end, que atienden la solicitud. Los grupos de back-end pueden constar de NIC, conjuntos de escalado de máquinas virtuales, direcciones IP públicas e internas, nombres de dominio completos (FQDN) y servidores back-end multiinquilino, como Azure App Service. Agregue los destinos de back-end a un grupo de back-end.

En este ejemplo, se usan máquinas virtuales como back-end de destino. Pueden usarse máquinas virtuales existentes o crear otras nuevas. En este ejemplo, se crean dos máquinas virtuales que Azure usa como servidores back-end para la puerta de enlace de aplicaciones. Para ello, puede:

1. Crear una subred, *myBackendSubnet*, en la que se crearán las máquinas virtuales. 
2. Crear dos VM, *myVM* y *myVM2*, que se usarán como servidores back-end.
3. Instalar IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.
4. Agregar los servidores back-end al grupo de back-end.

### <a name="add-a-subnet"></a>Incorporación de una subred

Agregue una subred a la red virtual que creó siguiendo estos pasos:

1. Seleccione **Todos los recursos** en el menú de la izquierda en Azure Portal, escriba *myVNet* en el cuadro de búsqueda y, a continuación, seleccione **myVNet** en los resultados de búsqueda.

2. Seleccione **Subredes** en el menú de la izquierda y seleccione **+ Subred**. 

   ![Creación de una subred](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. En la página **Agregar subred**, escriba *myBackendSubnet* como el **nombre** de la subred y, después, seleccione **Aceptar**.

### <a name="create-a-virtual-machine"></a>de una máquina virtual

1. En Azure Portal, seleccione **Crear un recurso**. Aparece la ventana **Nuevo**.
2. Haga clic en **Compute** y, a continuación, seleccione **Windows Server 2016 Datacenter** en la lista **Destacados**. Aparecerá la página **Creación de una máquina virtual**.<br>Application Gateway puede enrutar el tráfico a cualquier tipo de máquina virtual que se use en el grupo de back-end. En este ejemplo se usa un Windows Server 2016 Datacenter.
3. Especifique estos valores en la pestaña **Datos básicos** de la siguiente configuración de máquina virtual:
   - **Grupo de recursos**: Seleccione **myResourceGroupAG** como nombre del grupo de recursos.
   - **Nombre de la máquina virtual**: Especifique *myVM* como nombre de la máquina virtual.
   - **Nombre de usuario**: Escriba *azureuser* como nombre del usuario administrador.
   - **Contraseña**: Escriba *Azure123456!* como la contraseña de administrador.
4. Acepte los valores predeterminados y haga clic en **Siguiente: Discos**.  
5. Acepte los valores predeterminados de la pestaña **Discos** y seleccione **Siguiente: Redes**.
6. En la pestaña **Redes**, compruebe que **myVNet** está seleccionada como **red virtual** y que la **subred** es **myBackendSubnet**. Acepte los valores predeterminados y haga clic en **Siguiente: Administración**.<br>Application Gateway puede comunicarse con instancias fuera de la red virtual en la que se encuentra, pero hay que comprobar que haya conectividad IP.
7. En la pestaña **Administración**, establezca **Diagnósticos de arranque** en **Desactivado**. Acepte los demás valores predeterminados y seleccione **Revisar y crear**.
8. En la pestaña **Revisar y crear**, revise la configuración, corrija los errores de validación y, después, seleccione **Crear**.
9. Espere a que se complete la creación de la máquina virtual antes de continuar.

### <a name="install-iis-for-testing"></a>Instalación de IIS para pruebas

En este ejemplo se instala IIS en las máquinas virtuales con el único fin de comprobar que Azure creó correctamente la puerta de enlace de aplicaciones. 

1. Abra [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Para ello, seleccione **Cloud Shell** en la barra de navegación superior de Azure Portal y, a continuación, seleccione **PowerShell** en la lista desplegable. 

   ![Instalación de la extensión personalizada](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

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

3. Cree una segunda máquina virtual e instale IIS con los pasos que acaba de finalizar. Use *myVM2* como nombre de la máquina virtual y como valor de **VMName** para el cmdlet **Set-AzVMExtension**.

### <a name="add-backend-servers-to-backend-pool"></a>Incorporación de servidores back-end a un grupo de back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione los **grupos back-end** en el menú de la izquierda. Azure creó automáticamente un grupo predeterminado, **appGatewayBackendPool** cuando creó la puerta de enlace de aplicaciones. 

3. Seleccione **appGatewayBackendPool**.

4. En **Destinos**, seleccione **Máquina virtual** de la lista desplegable.

5. En **MÁQUINA VIRTUAL** e **INTERFACES DE RED**, seleccione las máquinas virtuales **myVM** y **myVM2**, y sus interfaces de red asociadas de las listas desplegables.

   ![Incorporación de servidores back-end](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Seleccione **Guardar**.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Crear una cuenta de almacenamiento y configurar los diagnósticos

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

En este tutorial, la puerta de enlace de aplicaciones usa una cuenta de almacenamiento para almacenar datos con fines de detección y prevención. También puede usar los registros de Azure Monitor o una instancia de Event Hubs para registrar los datos.

1. Haga clic en **Nuevo** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Almacenamiento** y, a continuación, seleccione **Cuenta de almacenamiento: blob, archivo, tabla, cola**.
3. Escriba el nombre de la cuenta de almacenamiento, seleccione **Usar existente** para el grupo de recursos y, a continuación, seleccione **myResourceGroupAG**. En este ejemplo, el nombre de la cuenta de almacenamiento es *myagstore1*. Acepte los valores predeterminados para las demás opciones y haga clic en **Crear**.

### <a name="configure-diagnostics"></a>Configuración de diagnóstico

Configure los diagnósticos para registrar datos en los registros ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog y ApplicationGatewayFirewallLog.

1. En el menú izquierdo, haga clic en **Todos los recursos** y, a continuación, seleccione *myAppGateway*.
2. En Supervisión, haga clic en **Registros de diagnóstico**.
3. Haga clic en **Agregar configuración de diagnóstico**.
4. Escriba *myDiagnosticsSettings* como el nombre de la configuración de diagnóstico.
5. Seleccione **Archivar en una cuenta de almacenamiento** y, a continuación, haga clic en **Configurar** para seleccionar la cuenta de almacenamiento *myagstore1* que creó anteriormente.
6. Seleccione los registros de la puerta de enlace de aplicaciones que se van a recopilar y conservar.
7. Haga clic en **Save**(Guardar).

    ![Configuración de diagnóstico](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

No es necesario instalar IIS para crear la puerta de enlace de aplicaciones, pero se instaló en este inicio rápido para comprobar si la puerta de enlace de aplicaciones se creó correctamente. Use IIS para probar la puerta de enlace de aplicaciones:

1. Busque la dirección IP pública de la puerta de enlace de aplicaciones en la página de **información general**.![ Registre la dirección IP pública de la puerta de enlace de aplicaciones](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png). O bien, puede seleccionar **Todos los recursos**, escribir *myAGPublicIPAddress* en el cuadro de búsqueda y luego seleccionarlo en los resultados de la búsqueda. Azure muestra la dirección IP pública en la página de **información general**.
2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador.
3. Compruebe la respuesta. Una respuesta válida corrobora que la puerta de enlace de aplicaciones se ha creado correctamente y puede conectarse correctamente con el back-end.![Prueba de la puerta de enlace de aplicaciones](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con la puerta de enlace de aplicaciones, elimine el grupo de recursos. Mediante la eliminación del grupo de recursos también elimina la puerta de enlace de aplicaciones y todos sus recursos relacionados. 

Para eliminar el grupo de recursos:

1. En el menú de la izquierda de Azure Portal, seleccione **Grupos de recursos**.
2. En la página **Grupos de recursos**, busque **myResourceGroupAG** en la lista y selecciónelo.
3. En la **página del grupo de recursos**, seleccione **Eliminar grupo de recursos**.
4. Escriba *myResourceGroupAG* en **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre lo que se puede hacer con Azure Application Gateway](application-gateway-introduction.md)
