---
title: 'Ejemplo de red perimetral: proteger redes con la red perimetral formada por un firewall, UDR y NSG | Microsoft Docs'
description: Crear una red perimetral (también conocida como DMZ) con un firewall, enrutamiento definido por el usuario (UDR) y grupos de seguridad de red (NSG).
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 104eae7c8d18c32fd5b9b84147df2bf121345c22
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59277429"
---
# <a name="example-3-build-a-perimeter-network-to-protect-networks-with-a-firewall-udr-and-nsgs"></a>Ejemplo 3: Creación de una red perimetral para proteger las redes con un firewall, UDR y NSG

[Volver a la página de procedimientos recomendados de límites de seguridad][HOME]

En este ejemplo, creará una red perimetral (también conocido como una DMZ, zona desmilitarizada y subred filtrada). El ejemplo implementa un firewall, cuatro servidores de Windows, el enrutamiento definido por el usuario (UDR), el reenvío de IP y grupos de seguridad de red (NSG). Este artículo le guiará a través de cada uno de los comandos pertinentes para proporcionar una comprensión más profunda de cada paso. La sección de escenario de tráfico también explica detalladamente cómo pasa el tráfico a través de las capas de defensa de la red perimetral. Por último, la sección de referencias contiene todo el código y las instrucciones para crear este entorno para que pueda probar y experimentar con diferentes escenarios.

![Red perimetral bidireccional con dispositivo de red virtual, grupo de seguridad de red y enrutamiento definido por el usuario][1]

## <a name="environment-setup"></a>Configuración del entorno

Este ejemplo usa una suscripción que contiene los siguientes componentes:

* Tres servicios en la nube: SecSvc001 FrontEnd001 y BackEnd001
* Una red virtual (CorpNetwork) con tres subredes: SecNet, FrontEnd y BackEnd
* Una aplicación virtual de red: un firewall conectado a la subred SecNet
* Un servidor de Windows que representa un servidor de aplicaciones web: IIS01
* Dos servidores de Windows que representan servidores back-end de aplicación: AppVM01, AppVM02
* Un servidor de Windows que representa un servidor DNS: DNS01

El [hace referencia a la sección](#references) contiene un script de PowerShell que compila la mayor parte del entorno descrito aquí. En este artículo en caso contrario, no le ofrece instrucciones detalladas para crear las máquinas virtuales (VM) y redes virtuales.

Para crear el entorno, siga estos pasos:

1. Guarde el archivo XML de configuración de red incluido en el [sección referencia](#references). Deberá actualizarlo con nombres, ubicación y las direcciones IP para que coincida con un escenario determinado.
1. Actualice las variables de usuario en el script completo para que coincida con su entorno específico (por ejemplo, las suscripciones, los nombres de servicio y así sucesivamente).
1. Ejecute el script de PowerShell.

> [!NOTE]
> La región especificada en el script de PowerShell debe coincidir con la región especificada en el archivo XML de configuración de red.

Después de la secuencia de comandos se ejecuta correctamente, siga estos pasos:

1. Configurar las reglas de firewall. Consulte la [reglas de firewall](#firewall-rules) sección.
1. Si lo desea, utilice las dos secuencias de comandos en la sección de referencias para configurar una aplicación web en el servidor web y el servidor de aplicaciones para permitir las pruebas de esta configuración de red Perimetral.

## <a name="user-defined-routing"></a>Enrutamiento definido por el usuario

De forma predeterminada, las siguientes rutas de sistema se definen como:

    Effective routes :
     Address Prefix    Next hop type    Next hop IP address Status   Source
     --------------    -------------    ------------------- ------   ------
     {10.0.0.0/16}     VNETLocal                            Active   Default
     {0.0.0.0/0}       internet                             Active   Default
     {10.0.0.0/8}      Null                                 Active   Default
     {100.64.0.0/10}   Null                                 Active   Default
     {172.16.0.0/12}   Null                                 Active   Default
     {192.168.0.0/16}  Null                                 Active   Default

VNETLocal es siempre el prefijo de dirección definido para esa red virtual específica. Por ejemplo, cambiará de red virtual a red virtual según cómo se defina cada red virtual específica. Las rutas de sistema restantes son estáticas y predeterminado tal como se muestra.

En cuanto a la prioridad, las rutas se procesan a través del método de coincidencia de prefijo más larga (LPM). Por lo que la ruta más específica en la tabla se aplica a una dirección de destino especificado.

Por lo tanto, el tráfico destinado a un servidor como DNS01 (10.0.2.4) en el equipo local (10.0.0.0/16) de red se enruta a través de la red virtual debido a la ruta 10.0.0.0/16.  Para 10.0.2.4, la ruta 10.0.0.0/16 es la ruta más específica. Esta regla se aplica incluso aunque 10.0.0.0/8 y 0.0.0.0/0 también sean aplicables. Sin embargo, son menos específicos, por lo que no afectan a este tráfico. El tráfico a 10.0.2.4 tiene la red virtual local como su próximo salto, por lo que se enruta al destino.

Por ejemplo, la ruta 10.0.0.0/16 no se aplica al tráfico que está destinado a 10.1.1.1. La ruta de sistema 10.0.0.0/8 es la más específica, por lo que se quita el tráfico o "negro enviando a un agujero" porque el próximo salto es Null.

Si el destino no se aplica a cualquiera de los prefijos Null o los prefijos VNETLocal, el tráfico sigue la ruta menos específica (0.0.0.0/0). Se enruta a internet como el próximo salto y fuera del perímetro de internet de Azure.

Si hay dos prefijos idénticos en la tabla de rutas, el orden de preferencia se basa en el atributo de origen de la ruta:

1. VirtualAppliance: Una ruta definida por el usuario agregado manualmente a la tabla.
1. VPNGateway: Agrega una ruta dinámica (BGP cuando se usa con redes híbridas) mediante un protocolo de red dinámica. Estas rutas pueden cambiar con el tiempo como el protocolo dinámico refleja automáticamente los cambios de red emparejada.
1. Valor predeterminado: Las rutas del sistema, la red virtual local y las entradas estáticas como se muestra en la tabla de enrutamiento anterior.

> [!NOTE]
> Ahora puede usar el enrutamiento definido por el usuario (UDR) con ExpressRoute y puertas de enlace de VPN para forzar el tráfico entrante y saliente entre locales se enrute a un dispositivo virtual de red (NVA).

### <a name="create-local-routes"></a>Crear rutas locales

Este ejemplo utiliza dos tablas de enrutamiento, uno para las subredes front-end y back-end. Cada tabla se carga con rutas estáticas adecuadas para la subred indicada. En este ejemplo, cada tabla tiene tres rutas:

1. Tráfico de subred local sin próximo salto definido. Esta ruta permite el tráfico de subred local por alto el firewall.
2. Tráfico de red virtual con el próximo salto definido como firewall. Esta ruta reemplaza la regla predeterminada que permite el tráfico de red virtual local se enrute directamente.
3. Todo el tráfico restante (0/0) con el próximo salto definido como firewall.

Una vez creadas las tablas de enrutamiento, se enlazan a sus subredes. La tabla de enrutamiento de la subred front-end debe ser similar:

    Effective routes :
     Address Prefix    Next hop type       Next hop IP address  Status   Source
     --------------    ------------------  -------------------  ------   ------
     {10.0.1.0/24}     VNETLocal                                Active
     {10.0.0.0/16}     VirtualAppliance    10.0.0.4             Active
     {0.0.0.0/0}       VirtualAppliance    10.0.0.4             Active

En este ejemplo usa los siguientes comandos para crear la tabla de rutas, agregue una ruta definida por el usuario y, a continuación, enlazar la tabla de rutas a una subred. Los elementos que comienzan por `$`, tales como `$BESubnet`, son variables definidas por el usuario de la secuencia de comandos en la sección de referencia.

1. En primer lugar, cree la tabla de enrutamiento base. El fragmento de código siguiente crea la tabla para la subred back-end. El script completo también crea una tabla para la subred front-end correspondiente.

   ```powershell
   New-AzureRouteTable -Name $BERouteTableName `
       -Location $DeploymentLocation `
       -Label "Route table for $BESubnet subnet"
   ```

1. Después de crear la tabla de rutas, puede agregar rutas específicas definidas por el usuario. El fragmento de código siguiente especifica que todo el tráfico (0.0.0.0/0) se enruta a través de la aplicación virtual. Una variable `$VMIP[0]` se usa para pasar la dirección IP asignada cuando el dispositivo virtual se creó anteriormente en la secuencia de comandos. El script completo también crea una regla correspondiente en la tabla front-end.

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. La entrada de ruta anterior reemplaza la ruta de "0.0.0.0/0" de forma predeterminada, pero la regla predeterminada 10.0.0.0/16 aún permite el tráfico dentro de la red virtual para enrutar directamente en el destino y no en el dispositivo virtual de red. Para corregir este comportamiento, deberá agregar la siguiente regla:

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. En este punto, tendrá que tomar una decisión. Las dos reglas anteriores enrutan todo el tráfico al firewall para su evaluación, incluido el tráfico dentro de una sola subred. Es posible que quiera este comportamiento. Sin embargo, si no lo hace, puede permitir el tráfico dentro de una subred se enrute localmente sin la participación del firewall. Agregue una tercera regla concreta que se enruta directamente cualquier dirección destinado a la subred local (NextHopType = VNETLocal).

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
           -NextHopType VNETLocal
   ```

1. Por último, después de la tabla de enrutamiento se crean y rellenan con las rutas definidas por el usuario, deberá enlazar la tabla a una subred. El siguiente fragmento de código enlaza la tabla para la subred back-end. El script completo también enlaza a la tabla de rutas de front-end a la subred front-end.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
       -SubnetName $BESubnet `
       -RouteTableName $BERouteTableName
   ```

## <a name="ip-forwarding"></a>Reenvío IP

Reenvío IP es una característica complementaria UDR. Esta configuración en un dispositivo virtual le permite recibir tráfico no dirigido al dispositivo y, a continuación, reenviar ese tráfico a su destino final.

Por ejemplo, si el tráfico de AppVM01 realiza una solicitud al servidor DNS01, UDR enruta el tráfico al firewall. Con el reenvío IP habilitado, el tráfico con el destino de DNS01 (10.0.2.4) está aceptado por el dispositivo de firewall (10.0.0.4) y, a continuación, se reenvía a su destino final (10.0.2.4). Sin el reenvío de IP habilitado en el servidor de seguridad, el tráfico no está aceptado por el dispositivo, aunque la tabla de rutas tiene el firewall como próximo salto.

> [!IMPORTANT]
> No olvide habilitar el reenvío IP junto con el enrutamiento definido por el usuario.

Reenvío de IP puede habilitarse con un solo comando durante la creación de la máquina virtual. Se llama a la instancia de máquina virtual que es la aplicación virtual de firewall y habilitar el reenvío IP. Tenga en cuenta que los elementos en rojo que comienzan por `$`, como `$VMName[0]`, son variables definidas por el usuario de la secuencia de comandos en la sección de referencia de este documento. El cero entre corchetes, `[0]`, representa la primera máquina virtual en la matriz de máquinas virtuales. Para que el script funcione sin modificaciones, la primera máquina virtual (VM 0) debe ser el servidor de seguridad. En el script completo, el fragmento de código relevante se agrupa con los comandos de Enrutamiento cerca del final.

```powershell
Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
    Set-AzureIPForwarding -Enable
```

## <a name="network-security-groups"></a>Grupos de seguridad de red

En este ejemplo, cree un grupo de seguridad de red (NSG) y, a continuación, cargarla con una sola regla. A continuación, en el ejemplo se enlaza el NSG solo a las subredes front-end y back-end (no el SecNet). La regla que se cargan en el NSG es como sigue:

* Se deniega cualquier tráfico (todos los puertos) desde internet a toda la red virtual (todas las subredes)

Aunque en este ejemplo se usan los NSG, su principal objetivo es constituir un segundo nivel de defensa frente a errores de configuración manual. Desea bloquear todo el tráfico entrante desde internet a las subredes front-end o back-end. El tráfico solo debe fluir a través de la subred SecNet al firewall, después del cual obtener debería enrutar el tráfico solo adecuado sesión en las subredes front-end o back-end. Además, las reglas de Enrutamiento enrutan todo el tráfico que llegue a las subredes front-end o back-end para el servidor de seguridad. El firewall lo considera como un flujo asimétrico y descarta el tráfico saliente.

Tres niveles de seguridad que protección las subredes front-end y back-end:

1. Ningún extremo abierto en los servicios en la nube FrontEnd001 y BackEnd001
1. Los NSG denegación el tráfico desde internet
1. El firewall quita tráfico asimétrico

Un punto interesante sobre lo NSG en este ejemplo es que contiene solo una regla, que se muestra a continuación. Esta regla deniega el tráfico de internet a toda la red virtual, incluida la subred de seguridad.

```powershell
Get-AzureNetworkSecurityGroup -Name $NSGName | `
    Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
    from the internet" `
    -Type Inbound -Priority 100 -Action Deny `
    -SourceAddressPrefix INTERNET -SourcePortRange '*' `
    -DestinationAddressPrefix VIRTUAL_NETWORK `
    -DestinationPortRange '*' `
    -Protocol *
```

Dado que solo está enlazado el NSG a las subredes front-end y back-end, no se aplica la regla para el tráfico entrante a la subred de seguridad. Como resultado, el tráfico fluye a la subred de seguridad.

```powershell
Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $FESubnet -VirtualNetworkName $VNetName

Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $BESubnet -VirtualNetworkName $VNetName
```

## <a name="firewall-rules"></a>Reglas de firewall

Debe crear reglas de reenvío en el firewall. Dado que el firewall bloquea o reenvía todo el tráfico entrante, saliente e intra-familiares virtuales de red, necesita muchas reglas de firewall. Además, el firewall tiene que procesar todo el tráfico entrante a la dirección IP pública de servicio de seguridad (en puertos diferentes). Para evitar la repetición del trabajo más adelante, siga el procedimiento recomendado creando diagramas de los flujos lógicos antes de configurar las reglas de firewall y las subredes. La siguiente ilustración es una vista lógica de las reglas de firewall para este ejemplo:

![Vista lógica de las reglas de firewall][2]

> [!NOTE]
> Los puertos de administración varían según el dispositivo virtual de red. En este ejemplo se muestra un Firewall Barracuda NextGen que usa los puertos 22, 801 y 807. Consulte la documentación del proveedor del dispositivo para buscar los puertos exactos para administrar el dispositivo.

### <a name="logical-rule-description"></a>Descripción de la lógica de regla

El diagrama lógico anterior no muestra la subred de seguridad porque el servidor de seguridad es el único recurso de la subred. Este diagrama muestra las reglas de firewall, cómo permiten o deniegan lógicamente los flujos de tráfico, pero no la real enruta la ruta de acceso. Además, los puertos externos seleccionados para el tráfico de protocolo de escritorio remoto (RDP) son puertos intervalos más alto (8014 – 8026) elegidos para que facilitar la legibilidad para alinearse con los dos últimos octetos de las direcciones IP locales. Por ejemplo, la dirección de servidor local 10.0.1.4 está asociada con el puerto externo 8014. Sin embargo, puede utilizar cualquier puerto superior no conflictivos.

En este ejemplo, necesita los siguientes tipos de reglas:

* Reglas externas para el tráfico entrante:
  1. Regla de Firewall de administración: permite el tráfico pase a los puertos de administración de dispositivo de red virtual.
  2. Reglas de RDP para cada servidor de windows: permite la administración de los servidores individuales mediante RDP.  Según las capacidades de su dispositivo de red virtual, puede agrupar las reglas en uno.
  3. Las reglas de tráfico de aplicación: uno para el tráfico web front-end y otro para el tráfico de back-end (por ejemplo, el servidor web a capa de datos). La configuración de estas reglas depende de la arquitectura de red y el tráfico de flujos.

     * La primera regla permite el tráfico de aplicación real llegue al servidor de la aplicación. A diferencia de las reglas de seguridad, administración y así sucesivamente, las reglas de aplicación permiten usuarios externos o servicios tener acceso a las aplicaciones. En este ejemplo tiene un único servidor web en el puerto 80, lo que permite una única aplicación regla de firewall redirigir el tráfico destinado a una dirección IP externa enrutar en su lugar a la dirección IP interna del servidor de web. La sesión de tráfico redirigido se reasigna NAT al servidor interno.
     * La segunda regla de tráfico de aplicación es la regla de back-end para permitir que el servidor web para utilizar cualquier puerto para enrutar el tráfico al servidor AppVM01, pero no al servidor AppVM02.

* Reglas internas para el tráfico de red-intra-familiares-virtuales:
  1. Salida a la regla de internet: permite el tráfico desde cualquier red pase a las redes seleccionadas. Esta regla suele ser un valor predeterminado en el firewall, pero en un estado deshabilitado. Habilite esta regla para este ejemplo.
  2. Regla DNS: solo permite el tráfico DNS (puerto 53) pasar al servidor DNS. Para este entorno, se bloquea la mayor parte del tráfico de front-end a back-end. Esta regla permite específicamente DNS desde cualquier subred local.
  3. Regla de subred a subred: permite que cualquier servidor en la subred de back-end para conectarse a cualquier servidor en la subred front-end, pero no a la inversa.

* Regla para notificaciones de error para el tráfico que no cumple alguno de los criterios mencionados anteriormente:
  1. Denegar todas las reglas de tráfico: siempre la última regla en términos de prioridad. Si el flujo de tráfico no coincide con ninguna de las reglas anteriores, esta regla lo bloquea. Es una regla predeterminada. Ya que normalmente se activa, no se necesitan modificaciones.

> [!TIP]
> En la segunda regla de tráfico de aplicación, se permite cualquier puerto para simplificar este ejemplo. En un escenario real, debe usar un puerto específico e intervalos de direcciones para reducir la superficie de ataque de esta regla.


> [!IMPORTANT]
> Después de haber creado las reglas, es importante revisar la prioridad de cada regla para asegurarse de que el tráfico se permitirá o denegará según sea necesario. En este ejemplo, las reglas están en orden de prioridad. Es fácil quedar bloqueado fuera del firewall si las reglas están mal ordenadas. Asegúrese de establecer la regla de firewall de administración como la máxima prioridad absoluta.

### <a name="rule-prerequisites"></a>Requisitos previos de la regla

Puntos de conexión públicos son necesarios para la máquina virtual que ejecuta el firewall. Estos puntos de conexión públicos deben estar abiertos para que el firewall pueda procesar el tráfico. Hay tres tipos de tráfico en este ejemplo:

1. Tráfico de administración para controlar el firewall y reglas de firewall
1. Tráfico RDP para controlar los servidores de windows
1. Tráfico de la aplicación

Los tipos de tráfico aparecen en la mitad superior de lo firewall reglas diagrama lógico anterior.

> [!IMPORTANT]
> Recuerde que *todas* tráfico procede a través del firewall. Escritorio remoto en el servidor IIS01, deberá conectarse al servidor de seguridad en el puerto 8014 y, a continuación, permitir que el firewall enrute la solicitud RDP internamente al puerto RDP de IIS01. El portal de Azure **Connect** botón no funcionará porque no hay ninguna ruta de acceso directo de RDP a IIS01 como puede ver el portal. Todas las conexiones desde internet están en el servicio de seguridad y un puerto (por ejemplo, secscv001.cloudapp). Hacer referencia en el diagrama anterior para la asignación de puerto externo e interno IP y puerto.

Puede abrir un punto de conexión en el momento de la creación de la máquina virtual o después de la compilación. El script de ejemplo y el fragmento de código siguiente abren un punto de conexión una vez creada la máquina virtual.

Tenga en cuenta que los elementos que comienzan por `$`, tales como `$VMName[$i]`, son variables definidas por el usuario de la secuencia de comandos en la sección de referencia. El `[$i]` representa el número de la matriz de una máquina virtual específica en una matriz de máquinas virtuales.

```powershell
Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
    -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
    Update-AzureVM
```

Aunque no se muestra aquí debido a las variables, sólo debe abrir los puntos de conexión en el servicio de seguridad en la nube. Esta precaución le ayuda a asegurarse de que el firewall controla todo el tráfico entrante, independientemente de que se enrutan, reasignar NAT o quitar.

Instale a un cliente de administración en el equipo para administrar el firewall y crear las configuraciones necesarias. Para obtener más información sobre cómo administrar el firewall u otro NVA, consulte la documentación del fabricante. El resto de esta sección, así como la **creación de reglas de Firewall** sección describe la configuración del firewall. Utilice el cliente de administración del proveedor, no el portal de Azure o PowerShell.

Vaya a [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin) para descargar el cliente de administración y obtenga información sobre cómo conectar con el firewall de Barracuda.

Una vez iniciado en el servidor de seguridad, definir objetos de red y el servicio antes de crear las reglas de firewall. Estas dos clases de requisitos previos del objeto pueden realizar la creación de las reglas.

En este ejemplo, defina tres objetos de red con nombre para cada uno:

* Subred de front-end
* Subred de back-end
* Dirección IP del servidor DNS

Para crear una red con nombre desde el panel de cliente de Barracuda NG Admin:

1. Vaya a la **ficha configuración**.
1. Seleccione **Ruleset** en el **configuración operativa** sección
1. Seleccione **redes** bajo el **Firewall objetos** menú.
1. Seleccione **New** en el **editar redes** menú.
1. Crear el objeto de red agregando el nombre y el prefijo:

   ![Crear un objeto de red de front-ed][3]

Los pasos anteriores crean una red con nombre para la subred front-end. Cree un objeto similar para la subred de back-end también. Ahora se puede hacer referencia a las subredes más fácilmente por nombre en las reglas de firewall.

Para el objeto de servidor DNS:

![Crear un objeto de servidor DNS][4]

Esta referencia de dirección IP solo se usa en una regla DNS más adelante en el documento.

La clase del objeto incluye los objetos de servicios, que representan los puertos de conexión de RDP para cada servidor. El objeto de servicio RDP existente está enlazado al puerto RDP predeterminado, 3389. Por lo tanto, puede crear nuevos servicios para permitir el tráfico de los puertos externos (8014-8026). También puede agregar los nuevos puertos para el servicio RDP existente. Sin embargo, para facilitar la demostración, puede realizar una de las reglas para cada servidor. Para crear una nueva regla RDP para un servidor desde el panel de cliente de Barracuda NG Admin:

1. Vaya a la **ficha configuración**.
1. Seleccione **Ruleset** en el **configuración operativa** sección.
1. Seleccione **servicios** bajo el **Firewall objetos** menú.
1. Desplácese hacia abajo en la lista de servicios y seleccione **RDP**.
1. Haga clic en y seleccione Copiar, a continuación, haga clic en y seleccione Pegar.
1. Ahora es un objeto de servicio RDP-Copy1 que se puede editar. Haga clic en **RDP-Copy1** y seleccione **editar**.
1. El **Editar objeto de servicio** ventana aparece como se muestra aquí:

   ![Copia de la regla predeterminada de RDP][5]

1. Edite los valores para representar el servicio RDP para un servidor específico. Para AppVM01, la regla predeterminada de RDP debe modificarse para reflejar un nuevo servicio **nombre**, **descripción**y el puerto RDP externo usado en el diagrama de la figura 8. Tenga en cuenta que los puertos se cambian el valor predeterminado RDP 3389 al puerto externo para este servidor específico. Por ejemplo, el puerto para AppVM01 externo es 8025. La regla de servicio modificado se muestra aquí:

   ![Regla de AppVM01][6]

Repita este proceso para crear servicios RDP para el resto de servidores: AppVM02, DNS01 e IIS01. Estos servicios hacen las reglas en la siguiente sección más obvios y más fácil de crear.

> [!NOTE]
> Un servicio RDP para el firewall no es necesario porque el firewall de máquina virtual es una imagen basada en Linux, por lo que se usa SSH en el puerto 22 para la administración de la máquina virtual en lugar de RDP. Además, el puerto 22 y otros dos puertos se permiten para la conectividad de administración. Consulte la **regla de Firewall de administración** en la sección siguiente.

### <a name="firewall-rules-creation"></a>Creación de reglas de Firewall

Hay tres tipos de reglas de firewall en este ejemplo, todo ello con iconos distintos:

La regla de redirección de aplicación: ![Icono de redirección de aplicación][7]

La regla NAT de destino: ![Icono de NAT de destino][8]

La regla paso: ![Icono de paso][9]

Para obtener más información sobre estas reglas se puede encontrar en el sitio Web de Barracuda.

Para crear las siguientes reglas o comprobar las reglas predeterminadas existentes:

1. En el panel de cliente de Barracuda NG Admin, vaya a la **configuración** ficha.
1. En el **configuración operativa** sección, seleccione **Ruleset**.
1. El **Main reglas** cuadrícula muestra existente activa y desactiva las reglas en este servidor de seguridad. Seleccionar el verde **+** en la esquina superior derecha para crear una nueva regla. Si el firewall está bloqueado para cambios, verá un botón marcado **bloqueo** y no se puede crear o editar reglas. Seleccione el **bloqueo** botón desbloquear el conjunto de reglas y permitir la edición. Haga clic en una regla que desea editar y seleccione **Editar regla**.

Después de crear o modificar las reglas, insertarlos en el firewall y activarlas. En caso contrario, los cambios de regla no surtirán efecto. Se describe en el proceso de activación e inserción [activación de regla](#rule-activation).

Estos son los aspectos específicos de cada regla necesarios para completar este ejemplo:

* **Regla de Firewall de administración**: Esta regla de redirección de aplicación permite el tráfico pase a los puertos de administración de la aplicación virtual de red, en este ejemplo, un Firewall Barracuda NextGen. Los puertos de administración son 801, 807 y, opcionalmente, 22. Los puertos internos y externos son los mismos, sin traducción de puertos. Esta regla se denomina instalación-MGMT-ACCESS. Es una regla predeterminada y habilitado de forma predeterminada en Barracuda NextGen Firewall versión 6.1.
  
    ![Regla de administración de firewall][10]

  > [!TIP]
  > El espacio de direcciones de origen en esta regla es **cualquier**. Si se conocen los intervalos de direcciones IP de administración, al reducir este ámbito también reduce la superficie de ataque para los puertos de administración.

* **Reglas de RDP**:  Estas reglas NAT de destino permiten la administración de los servidores individuales mediante RDP. Los campos críticos para estas reglas son:
  * Código fuente. Para permitir RDP desde cualquier lugar, use la referencia **cualquier** en el campo de origen.
  * Servicio. Utilice el objeto de servicio RDP que creó anteriormente: **AppVM01 RDP**. Los puertos externos se redirigen a la dirección IP local del servidor y al puerto RDP predeterminado 3386. Esta regla concreta es para el acceso de RDP a AppVM01.
  * Destino. Use el puerto local en el firewall: **DCHP 1 Local IP** o **eth0** si usa direcciones IP estáticas. El número ordinal (eth0, eth1 etc.) pueden ser diferente si el dispositivo de red tiene varias interfaces locales. El servidor de seguridad utiliza este puerto para enviar y podría ser el mismo que el puerto de recepción. El destino enrutado real está en el **lista destino** campo.
  * Redirección. Configure esta opción para indicar que en última instancia redirigir este tráfico a la aplicación virtual. La redirección más sencilla consiste en colocar la dirección IP en el campo de lista de destino. También puede especificar el puerto y NAT redistribuye el puerto y la dirección IP. Si no especifica un puerto, el dispositivo virtual utiliza el puerto de destino en la solicitud entrante.

    ![Regla de RDP de firewall][11]

    Cree cuatro reglas de RDP:

    | Nombre de la regla | Server | Servicio | Lista de destinos |
    | --- | --- | --- | --- |
    | RDP a IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
    | RDP a DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
    | RDP a AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
    | RDP a AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

  > [!TIP]
  > El ámbito de los campos de origen y el servicio de restricción reduce la superficie de ataque. Use el ámbito más limitado que permite la funcionalidad.

* **Las reglas de tráfico de aplicación**: Hay dos reglas de tráfico de aplicación. Uno es para el tráfico web front-end. El otro trata de tráfico de back-end como servidor web a capa de datos. Estas reglas dependen de la arquitectura de red y flujos de tráfico.
  
  * La regla front-end para el tráfico web:
  
    ![Regla web de firewall][12]
  
    Esta regla NAT de destino permite el tráfico de aplicación real llegue al servidor de la aplicación. A diferencia de las reglas de seguridad, administración y etc., las reglas de aplicación permiten usuarios externos o servicios tener acceso a las aplicaciones. En este ejemplo tiene un único servidor web en el puerto 80, lo que permite una única aplicación regla de firewall redirigir el tráfico destinado a una dirección IP externa enrutar en su lugar a la dirección IP interna del servidor de web. La sesión de tráfico redirigido se reasigna NAT al servidor interno.

    > [!NOTE]
    > No hay ningún puerto asignado en el **lista destino** campo. Por lo tanto, el puerto de entrada 80 (o 443 para el servicio seleccionado) se usa en la redirección del servidor web. Si el servidor web está escuchando en un puerto diferente, como 8080, puede actualizar el campo de lista de destino a 10.0.1.4:8080 para permitir también la redirección de puertos.
  
  * La regla back-end permite que el servidor web para comunicarse con el servidor AppVM01, pero no AppVM02, a través de **cualquier** servicio:
  
    ![Regla de AppVM01 de firewall][13]
  
    Esta regla de paso permite que cualquier servidor IIS en la subred front-end para llegar a AppVM01 (10.0.2.5) en cualquier puerto mediante cualquier protocolo, por lo que pueden tener acceso a datos por la aplicación web.
  
    En esta captura de pantalla, `<explicit-dest>` se utiliza en el **destino** campo para indicar 10.0.2.5 como destino. Puede especificar la dirección IP explícitamente como se muestra en la captura de pantalla. También puede usar un objeto con nombre de red, como en los requisitos previos para el servidor DNS. El administrador del servidor de seguridad puede elegir qué método usar. Para agregar 10.0.2.5 como destino explícito, haga doble clic en la primera fila vacía debajo `<explicit-dest>` y escriba la dirección en el cuadro de diálogo que aparece.
  
    Con esta regla de paso, no haya una NAT es necesario porque lleva a cabo el tráfico interno. Puede establecer el **método de conexión** a `No SNAT`.
  
    > [!NOTE]
    > La red de origen en esta regla es cualquier recurso en la subred front-end si solo hay una. Si la arquitectura especifica un número limitado de servidores web, puede crear un recurso de objeto de red para que sea más específicas para las direcciones IP exactas en lugar de toda la subred front-end.

    > [!TIP]
    > Esta regla usa el servicio **cualquier** para facilitar la instalación y uso de la aplicación de ejemplo. Permite ICMPv4 (ping) en una sola regla. Sin embargo, para reducir el ataque expuesta a través de este límite, se recomienda limitar los puertos y protocolos de servicios al mínimo posible que permitir la operación de la aplicación.

    > [!TIP]
    > Aunque esta regla de ejemplo usa `<explicit-dest>` referencia, debe usar un enfoque coherente en toda la configuración del firewall. Se recomienda usar un objeto con nombre de red para facilitar la legibilidad y compatibilidad. El `<explicit-dest>` mostrado aquí es solo para mostrar un método alternativo de referencia. Por lo general se no recomienda, especialmente para configuraciones complejas.

* **Regla de saliente a internet**: Esta regla de paso permite el tráfico de red de cualquier origen para pasar a las redes de destino seleccionadas. El firewall Barracuda NextGen normalmente tiene esto esta regla de "on" de forma predeterminada, pero en un estado deshabilitado. Haga doble clic en esta regla para tener acceso a la **activar regla** comando. Modificar la regla que se muestra en la captura de pantalla para agregar los objetos de red para las subredes back-end y front-end para el atributo de origen de esta regla. Estos objetos de red que creó en la sección Requisitos previos de este artículo.
  
    ![Regla de salida de firewall][14]

* **Regla DNS**: Esta regla de paso permite solo el tráfico DNS (puerto 53) pasar al servidor DNS. Para este entorno, mayor cantidad de tráfico desde el front-end para el back-end está bloqueado por lo que esta regla permite específicamente el tráfico DNS.
  
    ![Regla de DNS de firewall][15]
  
    > [!NOTE]
    > El **método de conexión** está establecido en `No SNAT` porque esta regla es para la dirección IP interna a tráfico de la dirección IP interno y no reenrutamiento a través de NAT es obligatorio.

* **Regla de subred a subred**: Esta regla de paso predeterminada se ha activado y modificado para permitir que a cualquier servidor en la subred de back-end para conectarse a cualquier servidor en la subred front-end. Esta regla solo coves tráfico interno por lo que la **método de conexión** se puede establecer en `No SNAT`.

    ![Regla entre redes virtuales de firewall][16]
  
    > [!NOTE]
    > El **bidireccionales** casilla no está activada aquí, por lo que esta regla se aplica sólo en una dirección. Desde la subred back-end, se puede iniciar una conexión a la red front-end, pero no a la inversa. Si se selecciona esa casilla, esta regla permitiría el tráfico bidireccional, que hemos especificado como no deseables en nuestro diagrama lógico.

* **Denegar todas las reglas de tráfico**: Esta regla debe ser siempre la última regla en términos de prioridad. Si el flujo de tráfico no coincide con cualquiera de las reglas anteriores, esta regla provoca que se va a quitar. La regla se activa normalmente de forma predeterminada, por lo que no se necesita ninguna modificación.
  
    ![Regla de denegación de firewall][17]

> [!IMPORTANT]
> Una vez creadas todas las reglas anteriores, revise la prioridad de cada regla para asegurarse de tráfico se permitirá o denegará según sea necesario. En este ejemplo, las reglas se muestran en el orden en que aparecen en la cuadrícula principal del cliente de Barracuda administración de reglas de reenvío.

## <a name="rule-activation"></a>Activación de reglas

Una vez modificada la regla establecida para que cumplan las especificaciones del diagrama lógico, deberá cargar el conjunto de reglas al firewall y actívelo.

![Activación de reglas de firewall][18]

Buscar en la esquina superior derecha de la ventana de cliente de administración y seleccione **enviar cambios** para cargar las reglas modificadas al firewall. Seleccione **Activar**.

Al activar el conjunto de reglas de firewall, este entorno de ejemplo está completa.

## <a name="traffic-scenarios"></a>Escenarios de tráfico

> [!IMPORTANT]
> Recuerde que *todas* tráfico procede a través del firewall. Escritorio remoto en el servidor IIS01, deberá conectarse al servidor de seguridad en el puerto 8014 y, a continuación, permitir que el firewall enrute la solicitud RDP internamente al puerto RDP de IIS01. El portal de Azure **Connect** botón no funcionará porque no hay ninguna ruta de acceso directo de RDP a IIS01 como puede ver el portal. Todas las conexiones desde internet están en el servicio de seguridad y un puerto (por ejemplo, secscv001.cloudapp). Hacer referencia en el diagrama anterior para la asignación de puerto externo e interno IP y puerto.

En estos escenarios, las siguientes reglas de firewall deben estar en su lugar:

1. Administración del firewall (administración de Reenvío)
2. RDP a IIS01
3. RDP a DNS01
4. RDP a AppVM01
5. RDP a AppVM02
6. Tráfico de aplicación a web
7. Tráfico de aplicación a AppVM01
8. Saliente a internet
9. Front-end a DNS01
10. Tráfico entre subredes (solo back-end a front-end)
11. Denegar todo

El conjunto de reglas de firewall actuales probablemente requerirá más reglas que los de este ejemplo. Es probables que tengan números de prioridad diferentes también. Se debe hacer referencia a esta lista y números asociados para su prioridad relativa entre sí. Por ejemplo, la regla "RDP a IIS01" podría ser regla número 5 en el firewall real, pero siempre y cuando su siguiente la "administración de Firewall" y por encima de la "RDP a DNS01" regla, el conjunto se alinea con la intención de esta lista. Esta lista también ayuda a simplificar las instrucciones para los escenarios siguientes. Por ejemplo, "regla de Firewall 9 (DNS)." Tenga en cuenta que las cuatro reglas RDP se denominan colectivamente "reglas RDP" cuando el escenario de tráfico está relacionado con RDP.

Recuerde también que los grupos de seguridad de red (NSG) están en vigor para el tráfico entrante de internet de las subredes front-end y back-end.

### <a name="allowed-internet-to-web-server"></a>(Permitido) Internet al servidor web

1. Un usuario de internet solicita la página HTTP desde SecSvc001.CloudApp.Net (servicio de nube a través de internet).
1. El servicio en la nube pasa el tráfico a través de un extremo abierto en el puerto 80 en la interfaz de firewall en 10.0.0.4: 80.
1. Ningún NSG se asigna a la subred de seguridad de modo que las reglas NSG del sistema permiten el tráfico al firewall.
1. El tráfico llega a una dirección IP interna del firewall (10.0.1.4).
1. El firewall realiza el procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. Regla de Firewall 6 (aplicación: Aplicar Web). Se permite el tráfico. Firewall redirige el tráfico a través de NAT a 10.0.1.4 (IIS01).
1. La subred front-end realiza el procesamiento de la regla de entrada:
   1. No se aplica la regla de NSG 1 (bloquear internet). El firewall redirigir este tráfico a través de NAT, por lo que la dirección de origen ahora es el servidor de seguridad. Dado que el firewall está en la subred de seguridad y aparece como el tráfico local a la subred front-end NSG, se permite el tráfico. Mover a la regla siguiente.
   1. Reglas NSG predeterminadas permiten el tráfico de subred a subred para permitir este tráfico. Detener procesamiento de reglas NSG.
1. Iis01 escucha el tráfico de web. Recibe esta solicitud y comienza a procesar la solicitud.
1. Iis01 intenta iniciar una sesión FTP a AppVM01 en la subred back-end.
1. La ruta UDR en la subred front-end hace que el firewall del próximo salto.
1. No hay ninguna regla de salida en la subred front-end por lo que se permite el tráfico.
1. El firewall comienza el procesamiento de las reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. Regla de Firewall 6 (aplicación: No se aplica la Web). Mover a la regla siguiente.
   1. Regla de Firewall 7 (aplicación: back-end) es aplicable. Se permite el tráfico. El firewall reenvía el tráfico a 10.0.2.5 (AppVM01).
1. La subred back-end realiza el procesamiento de la regla de entrada:
    1. No se aplica la regla de NSG 1 (bloquear internet). Mover a la regla siguiente.
    1. Reglas NSG predeterminadas permiten el tráfico de subred a subred. Se permite el tráfico. Detener procesamiento de reglas NSG.
1. AppVM01 recibe la solicitud, inicia la sesión y responde.
1. La ruta UDR en la subred back-end hace que el firewall del próximo salto.
1. No hay ninguna regla de NSG de salida en la subred back-end por lo que se permite la respuesta.
1. Como lo devuelve el tráfico en una sesión establecida, el firewall devuelve la respuesta al servidor web (IIS01).
1. Subred de front-end realiza el procesamiento de la regla de entrada:
    1. No se aplica la regla de NSG 1 (bloquear internet). Mover a la regla siguiente.
    1. Las reglas NSG predeterminadas permiten el tráfico de subred a subred para permitir este tráfico. Detener procesamiento de reglas NSG.
1. El servidor IIS recibe la respuesta y completa la transacción con AppVM01. A continuación, el servidor completa la compilación de la respuesta HTTP y lo envía al solicitante.
1. No hay ninguna regla de NSG de salida en la subred front-end por lo que se permite la respuesta.
1. La respuesta HTTP llega al firewall. Dado que es una respuesta a una sesión NAT establecida, el firewall lo acepta.
1. El firewall redirige la respuesta al usuario de internet.
1. No existen reglas de NSG salida ni saltos UDR en la subred front-end por lo que se permite la respuesta. El usuario de internet recibe la página web solicitada.

### <a name="allowed-internet-rdp-to-back-end"></a>(Permitido) RDP de Internet a back-end

1. Un administrador del servidor en internet solicita una sesión RDP para AppVM01 mediante SecSvc001.CloudApp.Net:8025. 8025 es el número de puerto asignado por el usuario para la regla de firewall 4 (RDP AppVM01).
1. El servicio en la nube pasa el tráfico a través del extremo abierto en el puerto 8025 a la interfaz de firewall en 10.0.0.4:8025.
1. Ningún NSG se asigna a la subred de seguridad de modo que las reglas NSG del sistema permiten el tráfico al firewall.
1. El firewall realiza el procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 2 (RDP IIS). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 3 (RDP DNS01). Mover a la regla siguiente.
   1. Aplicar la regla de Firewall 4 (RDP AppVM01) por lo que se permite el tráfico. El firewall lo redirige a través de NAT para 10.0.2.5:3386 (puerto RDP en AppVM01).
1. La subred back-end realiza el procesamiento de la regla de entrada:
   1. No se aplica la regla de NSG 1 (bloquear internet). El firewall redirigir este tráfico a través de NAT, por lo que la dirección de origen ahora es el servidor de seguridad que se encuentra en la subred de seguridad. El NSG de subred de back-end suele ser tráfico local y se permite. Mover a la regla siguiente.
   1. Reglas NSG predeterminadas permiten el tráfico de subred a subred para permitir este tráfico. Detener procesamiento de reglas NSG.
1. AppVM01 escucha el tráfico RDP y responde.
1. No hay ninguna regla de NSG de salida por lo que se aplican las reglas predeterminadas. Se permite el tráfico de retorno.
1. UDR enruta el tráfico saliente al firewall como próximo salto.
1. Como lo devuelve el tráfico en una sesión establecida, el firewall devuelve la respuesta al usuario de internet.
1. Sesión de RDP está habilitado.
1. AppVM01 solicita la contraseña del nombre de usuario.

### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Permitido) Búsqueda DNS de servidor Web en el servidor DNS

1. Servidor Web IIS01 solicita un suministro de datos en http\:\/\/www.data.gov, pero debe resolver la dirección.
1. La configuración de red para las listas de red virtual DNS01 (10.0.2.4 en la subred back-end) como el servidor DNS principal. Iis01 envía la solicitud DNS a DNS01.
1. UDR enruta el tráfico saliente al firewall como próximo salto.
1. No hay reglas NSG de salida se enlazan a la subred front-end. Se permite el tráfico.
1. El Firewall realiza el procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall, 6 y 7 (reglas de aplicación). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 8 (a internet). Mover a la regla siguiente.
   1. Aplicar la regla de Firewall 9 (DNS). Se permite el tráfico. Firewall reenvía el tráfico a 10.0.2.4 (DNS01).
1. La subred back-end realiza el procesamiento de la regla de entrada:
   1. No se aplica la regla de NSG 1 (bloquear internet). Mover a la regla siguiente.
   1. Reglas NSG predeterminadas permiten el tráfico de subred a subred. Se permite el tráfico. Detener procesamiento de reglas NSG.
1. El servidor DNS recibe la solicitud.
1. El servidor DNS no tiene la dirección en caché y solicita a un servidor DNS raíz en internet.
1. UDR enruta el tráfico saliente al firewall como próximo salto.
1. Hay ninguna regla NSG de salida en la subred de back-end para el tráfico se permite.
1. El Firewall realiza el procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall, 6 y 7 (reglas de aplicación). Mover a la regla siguiente.
   1. Aplicar la regla de Firewall 8 (a internet). Se permite el tráfico. La sesión se vuelve a enrutar a través de SNAT al servidor DNS raíz en internet.
1. El DNS de internet responde el servidor. Esta sesión se inició desde el servidor de seguridad, por lo que la respuesta está aceptada por el firewall.
1. Esta sesión ya está establecida para que el firewall reenvía la respuesta al servidor de inicio, DNS01.
1. La subred back-end realiza el procesamiento de la regla de entrada:
    1. No se aplica la regla de NSG 1 (bloquear internet). Mover a la regla siguiente.
    1. Reglas NSG predeterminadas permiten que se permite el tráfico de subred a subred a este tráfico. Detener procesamiento de reglas NSG.
1. El servidor DNS recibe y almacena en caché la respuesta y, a continuación, responde a la solicitud inicial a IIS01.
1. La ruta UDR en la subred back-end hace que el firewall del próximo salto.
1. Por lo que se permite el tráfico no existe ninguna regla NSG saliente en la subred back-end.
1. Esta sesión ya está establecida en el firewall para que el firewall redirige la respuesta al servidor IIS.
1. La subred front-end realiza el procesamiento de la regla de entrada:
    1. No hay ninguna regla NSG para el tráfico entrante desde la subred back-end a la subred front-end de modo que ninguna de las reglas NSG se aplican.
    1. La regla del sistema predeterminada permite el tráfico entre subredes. Se permite el tráfico.
1. Iis01 recibe la respuesta de DNS01.

### <a name="allowed-back-end-server-to-front-end-server"></a>(Permitido) Servidor de back-end a servidor front-end

1. Un administrador cono sesión iniciada appvm02 mediante RDP solicita un archivo directamente desde el servidor IIS01 mediante el Explorador de archivos de windows.
1. La ruta UDR en la subred back-end hace que el firewall del próximo salto.
1. No hay ninguna regla de NSG de salida en la subred back-end por lo que se permite la respuesta.
1. El firewall realiza el procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall, 6 y 7 (reglas de aplicación). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 8 (a internet). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 9 (DNS). Mover a la regla siguiente.
   1. Aplicar la regla de Firewall 10 (entre subredes). Se permite el tráfico. El firewall pasa el tráfico a 10.0.1.4 (IIS01).
1. La subred front-end comienza el procesamiento de la regla de entrada:
   1. No se aplica, pasar a la siguiente regla NSG de la regla 1 (bloquear internet)
   1. Las reglas NSG predeterminadas permiten el tráfico de subred a subred por lo que se permite el tráfico. Detener procesamiento de reglas NSG.
1. Si la autorización y autenticación correctas, IIS01 acepta la solicitud y responde.
1. La ruta UDR en la subred front-end hace que el firewall del próximo salto.
1. No hay ninguna regla de NSG de salida en la subred front-end por lo que se permite la respuesta.
1. Esta sesión ya existe en el firewall, por lo que se permite esta respuesta. El firewall devuelve la respuesta al AppVM02.
1. La subred back-end realiza el procesamiento de la regla de entrada:
    1. No se aplica la regla de NSG 1 (bloquear internet). Mover a la regla siguiente.
    2. Las reglas NSG predeterminadas permiten el tráfico de subred a subred por lo que se permite el tráfico. Detener procesamiento de reglas NSG.
1. AppVM02 recibe la respuesta.

### <a name="denied-internet-direct-to-web-server"></a>(Denegado) Internet directo al servidor web

1. Un usuario de internet intenta tener acceso el servidor web IIS01 mediante el servicio FrontEnd001.CloudApp.Net.
1. Hay ningún extremo abierto para el tráfico HTTP para que este tráfico no pasa a través del servicio en la nube. El tráfico no llegue al servidor.
1. Si los puntos de conexión están abiertos por alguna razón, el NSG (bloquear internet) en la subred front-end bloquea este tráfico.
1. Por último, la ruta UDR de la subred front-end envía todo el tráfico saliente desde IIS01 al firewall como próximo salto. El firewall lo considera tráfico asimétrico y quita la respuesta saliente.

>Por lo tanto, hay al menos tres capas independientes de defensa entre internet e IIS01. El servicio de nube impide el acceso no autorizado o inadecuado.

### <a name="denied-internet-to-back-end-server"></a>(Denegado) Internet al servidor back-end

1. Un usuario de internet intenta acceder a un archivo en AppVM01 a través del servicio backend001.cloudapp.NET.
2. No hay ningún extremo abierto para uso compartido de archivos, por lo que esta solicitud no pasa el servicio en la nube. El tráfico no llegue al servidor.
3. Si los puntos de conexión están abiertos por alguna razón, el NSG (bloquear internet) bloquea este tráfico.
4. Por último, la ruta UDR envía todo el tráfico saliente desde AppVM01 al firewall como próximo salto. El firewall lo considera tráfico asimétrico y quita la respuesta saliente.

> Por lo tanto, hay al menos tres capas independientes de defensa entre internet y AppVM01. El servicio de nube impide el acceso no autorizado o inadecuado.

### <a name="denied-front-end-server-to-back-end-server"></a>(Denegado) Servidor front-end a servidor back-end

1. Iis01 está en peligro y se está ejecutando código malintencionado intenta analizar los servidores de la subred back-end.
1. La ruta UDR de la subred front-end envía todo el tráfico saliente desde IIS01 al firewall como próximo salto. La máquina virtual en peligro no puede modificar este enrutamiento.
1. El firewall procesa el tráfico. Si la solicitud es para AppVM01 o en el servidor DNS para las búsquedas DNS, el firewall podría permitir el tráfico debido a las reglas de Firewall 7 y 9. Todo el tráfico está bloqueado por la regla de Firewall 11 (denegar todo).
1. Si habilita la detección de amenazas avanzada en el firewall, podría evitarse el tráfico que contiene signaturas o patrones que activen una regla avanzada contra amenazas. Puede trabajar en esta medida incluso si se permite el tráfico según las reglas de reenvío básicas que se describen en este artículo. Detección de amenazas avanzadas no se trata en este documento. Consulte la documentación del proveedor para su dispositivo de red avanzadas funcionalidades de amenazas.

### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Denegado) Búsqueda de DNS de Internet en el servidor DNS

1. Un usuario de internet intenta buscar un registro DNS interno en DNS01 a través del servicio backend001.cloudapp.NET.
1. Puesto que no hay ningún extremo abierto para el tráfico DNS, este tráfico no pasa a través del servicio en la nube. Que no alcance el servidor.
1. Si los puntos de conexión están abiertos por alguna razón, la regla NSG (bloquear internet) en la subred front-end bloquea este tráfico.
1. Por último, la ruta UDR de la subred de back-end envía todo el tráfico saliente desde DNS01 al firewall como próximo salto. El firewall lo considera tráfico asimétrico y quita la respuesta saliente.

> Por lo tanto, hay al menos tres capas independientes de defensa entre internet e DNS01. El servicio de nube impide el acceso no autorizado o inadecuado.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Denegado) Internet a acceso SQL a través de firewall

1. Un usuario de internet solicita datos SQL desde el servicio de nube a través de internet SecSvc001.CloudApp.Net.
1. Hay ningún extremo abierto para SQL para que este tráfico no pasa el servicio en la nube. Que no alcance el firewall.
1. Si los puntos de conexión SQL están abiertos por alguna razón, el firewall realiza procesamiento de reglas:
   1. No se aplica la regla de Firewall 1 (administración de Reenvío). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall 2-5 (reglas de RDP). Mover a la regla siguiente.
   1. No se aplican las reglas de Firewall, 6 y 7 (reglas de aplicación). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 8 (a internet). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 9 (DNS). Mover a la regla siguiente.
   1. No se aplica la regla de Firewall 10 (entre subredes). Mover a la regla siguiente.
   1. Aplicar la regla de Firewall 11 (denegar todo). Bloquea el tráfico. Detener procesamiento de las reglas.

## <a name="references"></a>Referencias

Esta sección contiene los siguientes elementos:

* Script completo. Guárdelo en un archivo de script de PowerShell.
* Configuración de red. Guárdelo en un archivo denominado NetworkConf2.xml.

Modifique las variables definidas por el usuario en los archivos según sea necesario. Ejecute el script y, a continuación, siga las instrucciones de configuración de regla de Firewall enumeradas anteriormente en este artículo.

### <a name="full-script"></a>Script completo

Después de establecer las variables definidas por el usuario, ejecute esta secuencia de comandos:

1. Conexión a una suscripción de Azure
1. Creación de una cuenta de almacenamiento nueva
1. Crear una nueva red virtual y tres subredes, tal como se define en el archivo de configuración de red
1. Crear cinco máquinas virtuales: un firewall y cuatro máquinas virtuales de Windows Server
1. Configurar UDR:
   1. Cree dos nuevas tablas de enrutamiento
   1. Agregar rutas a las tablas
   1. Enlazar las tablas a las subredes correspondientes
1. Habilitar el reenvío IP en el dispositivo virtual de red
1. Configurar NSG:
   1. Crear un NSG
   1. Agregar una regla
   1. Enlace el NSG a las subredes adecuadas

Ejecute este PowerShell script localmente en un internet conectado a PC o servidor.

> [!IMPORTANT]
> Al ejecutar este script, advertencias u otros mensajes informativos podrían aparecer en PowerShell. Solo los mensajes de error rojo son motivo de preocupación.

```powershell
    <#
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwarding from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Variables or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) {
            Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Associate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the FrontEnd and BackEnd subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install BackEnd resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

### <a name="network-config-file"></a>Archivo de configuración de red

Guarde este archivo XML con la ubicación actualizada. Cambiar el `$NetworkConfigFile` variable en el script completo anterior para vincular al archivo de configuración de red guardada.

```xml
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>
```

## <a name="next-steps"></a>Pasos siguientes

Puede instalar una aplicación de ejemplo para ayudarle con este ejemplo de la red perimetral.

> [!div class="nextstepaction"]
> [Script de la aplicación de ejemplo](./virtual-networks-sample-app.md)

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Red perimetral bidireccional con un dispositivo de red virtual, grupos de seguridad de red y enrutamiento definido por el usuario"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Vista lógica de las reglas de firewall"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Creación de un objeto de red front-end"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Creación de un objeto de servidor DNS"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Copia de la regla predeterminada de RDP"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "Regla de AppVM01"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Icono de redirección de aplicación"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Icono de NAT de destino"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Icono de Paso"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Regla de administración de firewall"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Regla de RDP de firewall"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Regla web de firewall"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Regla de AppVM01 de firewall"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Regla de salida de firewall"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Regla de DNS de firewall"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Regla entre redes virtuales de firewall"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Regla de denegación de firewall"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Activación de reglas de firewall"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
