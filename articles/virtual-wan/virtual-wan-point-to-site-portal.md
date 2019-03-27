---
title: Uso de Azure Virtual WAN para crear una conexión de sitio a sitio a Azure | Microsoft Docs
description: En este tutorial, aprenda a usar Azure Virtual WAN para crear una conexión VPN de punto a sitio a Azure.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 02/27/2019
ms.author: alzam
Customer intent: As someone with a networking background, I want to connect remote users to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 9fe0c7f7ae0c19833421b647449f0e4100904f5b
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226239"
---
# <a name="tutorial-create-a-point-to-site-connection-using-azure-virtual-wan-preview"></a>Tutorial: Creación de una conexión de punto a sitio mediante Azure Virtual WAN (Versión preliminar)

Este tutorial muestra cómo usar Virtual WAN para conectarse a los recursos de Azure a través de una conexión VPN de OpenVPN o IPsec/IKE (IKEv2). Este tipo de conexión requiere que se configure un cliente en el equipo cliente. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Creación de una configuración de P2S
> * Crear un concentrador
> * Aplicar la configuración de P2S a un concentrador
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de cliente VPN
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Registro de esta característica

Haga clic en **Probar** para registrar esta característica fácilmente mediante Azure Cloud Shell. Si prefiere ejecutar PowerShell localmente, asegúrese de tener la versión más reciente e inicie sesión con los comandos **Connect-AzAccount** y **Select-AzSubscription**.

>[!NOTE]
>Si no la registra, no podrá usarla ni verla en el portal.
>
>

Después de hacer clic en **Probar** para abrir Azure Cloud Shell, copie y pegue los siguientes comandos:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```
 
```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Una vez que la característica se muestre como registrada, vuelva a registrar la suscripción en el espacio de nombres Microsoft.Network.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Creación de una red virtual

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Creación de una instancia de Virtual WAN

En un explorador, vaya a [Azure Portal (versión preliminar)](https://aka.ms/azurevirtualwanpreviewfeatures) e inicie sesión con su cuenta de Azure.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Crear un concentrador

> [!NOTE]
> No seleccione la opción "Incluir puerta de enlace de punto a sitio" en este paso.
>

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="site"></a>4. Creación de una configuración de P2S

Una configuración de P2S define los parámetros para conectarse a los clientes remotos.

1. Vaya a **Todos los recursos**.
2. Haga clic en la red WAN virtual que ha creado.
3. En **Virtual WAN architecture** (Arquitectura de Virtual WAN), haga clic en **Configuraciones de punto a sitio**.
4. Haga clic en **+Agregar configuración de punto a sitio** en la parte superior de la página para abrir la página **Crear nueva configuración de punto a sitio**.
5. En la página **Crear nueva configuración de punto a sitio**, rellene los campos siguientes:

   *  **Nombre de la configuración**: nombre que desea usar para hacer referencia a su configuración.
   *  **Tipo de túnel**: el protocolo que se usará para el túnel.
   *  **Grupo de direcciones**: grupo de direcciones IP que se asignará a los clientes.
   *  **Nombre del certificado raíz**: nombre descriptivo para el certificado.
   *  **Datos del certificado raíz**: datos del certificado X.509 codificado en Base 64.

6. Haga clic en **Crear** para crear la configuración.

## <a name="hub"></a>5. Edición de la asignación del concentrador

1. En la página de la red virtual WAN, haga clic en **Centros**.
2. Seleccione el centro al que desea asignar la configuración de punto a sitio.
3. Haga clic en **"..."** y elija **Editar concentrador virtual**
4. Seleccione **Incluir puerta de enlace de punto a sitio**.
5. En la lista desplegable, seleccione las **unidades de escaña de puerta de enlace**.
6. En la lista desplegable, seleccione la **configuración de punto a sitio** que creó.
7. Configure el **grupo de direcciones** para los clientes.
8. Haga clic en **Confirmar**. La operación puede tardar un máximo de 30 minutos en completarse.

## <a name="vnet"></a>6. Conexión de la red virtual a un concentrador

En este paso, creará la conexión de emparejamiento entre una red virtual y el concentrador. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red WAN virtual, haga clic en **Conexiones de red virtual**.
2. En la página de conexión de red virtual, haga clic en **+ Agregar conexión**.
3. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.
4. Haga clic en **Aceptar** para agregar la conexión.

## <a name="device"></a>7. Descarga del perfil de VPN

Use el perfil de VPN para configurar los clientes.

1. En la página de la red virtual WAN, haga clic en **Centros**.
2. Seleccione el centro para el que desea descargar el perfil.
3. Haga clic en **"..."** y elija **Descargar perfil**. 
4. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.
4. Use el archivo de perfil para configurar los clientes de punto a sitio.

## <a name="device"></a>8. Configuración de los clientes de punto a sitio
Use el perfil descargado para configurar los clientes de acceso remoto. El procedimiento para cada sistema operativo es diferente, siga las instrucciones correctas a continuación:

### <a name="microsoft-windows"></a>Microsoft Windows
#### <a name="openvpn"></a>OpenVPN

1.  Descargue e instale el cliente OpenVPN desde el sitio web oficial.
2.  Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuraciones de punto a sitio de Azure Portal o mediante New-AzVpnClientConfiguration en PowerShell.
3.  Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN en el Bloc de notas.
4.  Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en Base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Vea aquí cómo exportar un certificado para obtener la clave pública codificada.
5.  Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en Base64. Vea aquí cómo extraer la clave privada.
6.  No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
7.  Copie el archivo vpnconfig.ovpn en la carpeta C:\Program Files\OpenVPN\config.
8.  Haga clic con el botón derecho en l icono de OpenVPN en la bandeja del sistema y, después, haga clic en Conectar.

#### <a name="ikev2"></a>IKEv2

1. Seleccione los archivos de configuración de cliente VPN que correspondan a la arquitectura del equipo Windows. Si la arquitectura de procesador es de 64 bits, elija el paquete del instalador "VpnClientSetupAmd64". En caso de que sea de 32 bits, elija el paquete del instalador "VpnClientSetupX86".
2. Haga doble clic en el paquete para instalarlo. Si ve una ventana emergente de SmartScreen, haga clic en Más información y en Ejecutar de todas formas.
3. En el equipo cliente, vaya a Configuración de red y haga clic en VPN. La conexión VPN muestra el nombre de la red virtual a la que se conecta.
4. Antes de intentar conectarse, compruebe que ha instalado un certificado de cliente en el equipo cliente. Es necesario un certificado de cliente para la autenticación al usar el tipo de autenticación de certificados nativo de Azure. Para más información acerca de cómo generar certificados, consulte Generación de certificados. Para información acerca de cómo instalar un certificado de cliente, consulte Instalación de certificados de cliente.

### <a name="mac-os-x"></a>Mac (OS X)
#### <a name="openvpn"></a>OpenVPN

1.  Descargue e instale un cliente OpenVPN, como TunnelBlik, desde https://tunnelblick.net/downloads.html 
2.  Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuraciones de punto a sitio de Azure Portal o mediante New-AzVpnClientConfiguration en PowerShell.
3.  Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN en el Bloc de notas.
4.  Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en Base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Vea aquí cómo exportar un certificado para obtener la clave pública codificada.
5.  Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en Base64. Vea aquí cómo extraer la clave privada.
6.  No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
7.  Haga doble clic en el archivo de perfil para crear el perfil en Tunnelblik.
8.  Inicie Tunnelblik desde la carpeta de aplicaciones.
9.  Haga clic en el icono de Tunneblik en la bandeja del sistema y seleccione Conectar.

#### <a name="ikev2"></a>IKEv2

Azure no proporciona el archivo mobileconfig para realizar la autenticación de certificados nativa de Azure. Tendrá que configurar manualmente el cliente de VPN nativo IKEv2 en cada equipo Mac que se conecte a Azure. La carpeta Genérico contiene toda la información necesaria para configurarlo. Si no ve la carpeta Genérico en la descarga, es posible que IKEv2 no se haya seleccionado como tipo de túnel. Una vez que se seleccione IKEv2, vuelva a generar el archivo ZIP para recuperar la carpeta Genérico. que son los archivos siguientes:

- VpnSettings.xml, con configuración importante, como el tipo de túnel y la dirección del servidor.
- VpnServerRoot.cer, con el certificado raíz necesario para validar Azure VPN Gateway durante la instalación de la conexión de punto a sitio.

## <a name="viewwan"></a>9. Visualizar la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, cada punto del mapa representa un concentrador. Mantenga el mouse sobre cualquier punto para ver el resumen de estado del concentrador.
3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="viewhealth"></a>10. Visualización del estado de los recursos

1. Vaya a su red WAN.
2. En la página de la WAN, en la sección de **Soporte técnico y solución de problemas**, haga clic en **Mantenimiento** y visualice los recursos.

## <a name="connectmon"></a>11. Supervisar una conexión

Cree una conexión para supervisar la comunicación entre una máquina virtual de Azure y un sitio remoto. Para información acerca de cómo configurar una supervisión de conexión, consulte [Supervisar la comunicación de red](~/articles/network-watcher/connection-monitor.md). El campo de origen es la IP de la máquina virtual en Azure y la dirección IP de destino es la dirección IP del sitio.

## <a name="cleanup"></a>12. Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Crear un sitio
> * Crear un concentrador
> * Conectar un concentrador con un sitio
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de dispositivo VPN
> * Visualizar la instancia de Virtual WAN
> * Visualizar el estado de los recursos
> * Supervisar una conexión

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
