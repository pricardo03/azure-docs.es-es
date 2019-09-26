---
title: Solución de problemas de estado del back-end en Azure Application Gateway
description: Este artículo le guía en la solución de problemas de estado del back-end en Azure Application Gateway
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: article
ms.date: 08/30/2019
ms.author: surmb
ms.openlocfilehash: 1fd4e9156e29133b1db4fe9ab9a0825eb1aa3b55
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097591"
---
<a name="troubleshoot-backend-health-issues-in-application-gateway"></a>Solución de problemas de estado del back-end en Application Gateway
==================================================

<a name="overview"></a>Información general
--------

De forma predeterminada, Application Gateway sondea los servidores back-end para comprobar su estado de mantenimiento y si están listos para atender las solicitudes. Los usuarios pueden crear sondeos personalizados también para mencionar el nombre de host, la ruta de acceso que se va a sondear y los códigos de estado que se van a aceptar como correctos. En ambos casos, si el servidor back-end no responde de forma apropiada, Application Gateway lo marcará como incorrecto y dejará de reenviarle la solicitud. Una vez que el servidor empiece a responder de forma correcta, se reanudará la entrega de solicitudes.

### <a name="how-to-check-backend-health"></a>Comprobación del estado del servidor back-end

Para comprobar el estado del grupo de servidores back-end, puede usar la página "Estado del back-end" en Azure Portal. También puede usar [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhealth?view=azps-2.6.0), la [CLI](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-show-backend-health) o la [API REST](https://docs.microsoft.com/rest/api/application-gateway/applicationgateways/backendhealth) para obtener el estado. Consulte el artículo vinculado con cada método para más información.

El estado recuperado por cualquiera de los métodos mencionados anteriormente puede ser de tres tipos, que son:

1.  Healthy

2.  Unhealthy (Incorrecto)

3.  Desconocido

Si el estado del back-end de un servidor es healthy (correcto), significa que Application Gateway reenviará las solicitudes a ese servidor. Pero si el estado de los servidores de un grupo de back-end es unhealthy (incorrecto) o unknown (desconocido), podría sufrir algunos problemas durante el acceso a la aplicación. En este documento se describe los síntomas, la causa y la solución de cada uno de los errores que se muestran cuando el estado de los servidores back-end podría ser incorrecto o desconocido.

<a name="backend-health-status-unhealthy"></a>Estado del back-end incorrecto
-------------------------------

Si el estado del back-end se muestra como incorrecto, lo verá en el portal, como en la siguiente captura de pantalla:

![Estado del back-end Application Gateway: unhealthy (incorrecto)](./media/application-gateway-backend-health-troubleshooting/appgwunhealthy.png)

O bien, si usa una consulta a la API REST de Azure, Azure PowerShell o la CLI de Azure, verá una respuesta similar a la siguiente:
```azurepowershell
PS C:\Users\testuser\> Get-AzApplicationGatewayBackendHealth -Name "appgw1" -ResourceGroupName "rgOne"
BackendAddressPools :
{Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayBackendHealthPool}
BackendAddressPoolsText : [
{
                              "BackendAddressPool": {
                                "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appgw1/b
                          ackendAddressPools/appGatewayBackendPool"
                              },
                              "BackendHttpSettingsCollection": [
                                {
                                  "BackendHttpSettings": {
                                    "TrustedRootCertificates": [],
                                    "Id": "/subscriptions/536d30b8-665b-40fc-bd7e-68c65f816365/resourceGroups/rgOne/providers/Microsoft.Network/applicationGateways/appg
                          w1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
                                  },
                                  "Servers": [
                                    {
                                      "Address": "10.0.0.5",
                                      "Health": "Healthy"
                                    },
                                    {
                                      "Address": "10.0.0.6",
                                      "Health": "Unhealthy"
                                    }
                                  ]
                                }
                              ]
                            }
                        ]
```
Cuando vea el estado del servidor back-end como incorrecto para todos los servidores de un grupo de back-end, no se les reenviarán las solicitudes y Application Gateway devolverá un error de puerta de enlace incorrecto 502 al cliente que realiza la solicitud. Para solucionar el problema, compruebe la columna Detalles de la pestaña Estado del back-end.

El mensaje que se muestra en la columna Detalles de la pestaña Estado del back-end proporciona información más detallada sobre el problema y, según dicha información, podemos empezar a solucionar el problema.

> [!NOTE]
> La solicitud de sondeo predeterminada se envía con el formato \<protocolo\>://127.0.0.1:\<puerto\>/, por ejemplo, <http://127.0.0.1/>, para un sondeo HTTP en el puerto 80, y solo considera como correcta una respuesta con los códigos de estado HTTP 200 a 399. El protocolo y el puerto de destino se heredan de la configuración de HTTP. Si desea que Application Gateway sondee en un protocolo, un nombre de host o una ruta de acceso diferentes, y acepte como correcto un código de estado distinto, configure un sondeo personalizado y asócielo con la configuración de HTTP.

<a name="error-messages"></a>Mensajes de error
------------------------
#### <a name="backend-server-timeout"></a>Tiempo de espera del servidor de back-end

**Mensaje**: Time taken by the backend to respond to application gateway\'s health probe is more than the time-out threshold in the probe setting. (El tiempo que tarda el servidor back-end en responder al sondeo de estado de Application Gateway es mayor que el umbral de tiempo de espera en la configuración de sondeo).

**Causa:** una vez que Application Gateway envía una solicitud de sondeo HTTP(S) al servidor back-end, espera su respuesta durante un determinado período de tiempo configurado. Si el servidor back-end no responde dentro del valor de tiempo de espera, se marca como incorrecto hasta que empieza a responder de nuevo en el tiempo de espera.

**Resolución:** compruebe en el servidor back-end o la aplicación el motivo por el que no responde dentro del tiempo de espera configurado, y compruebe también las dependencias de la aplicación; por ejemplo, si la base de datos tiene problemas, lo que podría provocar el retraso en la respuesta. Si tiene constancia del comportamiento de la aplicación y debe responder solo después del valor de tiempo de espera, aumente el valor de tiempo de espera en la configuración del sondeo personalizado.
Debe tener un sondeo personalizado para cambiar el valor del tiempo de espera. Si desea saber cómo configurar un sondeo personalizado, [consulte la página de documentación](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal).

Para aumentar el tiempo de espera, siga estos pasos:

1.  Acceda directamente al servidor back-end y compruebe el tiempo que tarda el servidor en responder en esa página. Puede usar cualquier herramienta para acceder, incluido un explorador con las herramientas de desarrollo.

2.  Una vez que haya averiguado el tiempo necesario para que la aplicación responda, haga clic en la pestaña Sondeos de estado y seleccione el sondeo que está asociado a la configuración de HTTP.

3.  Especifique un valor de tiempo de espera mayor que el tiempo de respuesta de la aplicación, en segundos.

4.  Guarde la configuración de sondeo personalizada y compruebe si el estado del back-end aparece ahora como correcto.

#### <a name="dns-resolution-error"></a>Error de resolución DNS

**Mensaje**: Application gateway could not create a probe for this backend. This usually happens when the FQDN of the backend has not been entered correctly. (Application Gateway no pudo crear un sondeo para este back-end. Esto suele ocurrir cuando el nombre de dominio completo del back-end no se ha escrito de forma apropiada). 

**Causa:** si el grupo de back-end es del tipo Dirección IP/Nombre de dominio completo o App Service, Application Gateway resuelve la dirección IP del nombre de dominio completo especificado mediante el servidor DNS (personalizado o predeterminado de Azure) e intenta conectarse al servidor en el puerto TCP mencionado en la configuración de HTTP. Sin embargo, si se muestra este mensaje, sugiere que Application Gateway no pudo resolver correctamente la dirección IP del nombre de dominio completo especificado.

**Resolución:**

1.  compruebe si el nombre de dominio completo especificado en el grupo de back-end es correcto y, si es un dominio público, intente resolverlo desde la máquina local.

2.  Si puede resolver la dirección IP, podría haber algún problema con la configuración DNS en la red virtual.

3.  Compruebe si la red virtual está configurada con un servidor DNS personalizado. Si es así, compruebe en el servidor DNS el motivo por el que no se puede resolver la dirección IP del nombre de dominio completo proporcionado.

4.  Si se trata de DNS predeterminado de Azure, consulte al registrador de nombres de dominio si se realizó correctamente una asignación apropiada de registro de dirección o de registro CNAME.

5.  Si el dominio es uno privado o interno, intente resolverlo desde una máquina virtual en la misma red virtual y, si así sí se resuelve, reinicie Application Gateway y vuelva a comprobarlo. Para reiniciar Application Gateway, debe [detenerlo](https://docs.microsoft.com/powershell/module/azurerm.network/stop-azurermapplicationgateway?view=azurermps-6.13.0) e [iniciarlo](https://docs.microsoft.com/powershell/module/azurerm.network/start-azurermapplicationgateway?view=azurermps-6.13.0) con los comandos de PowerShell proporcionados en los vínculos de los documentos correspondientes.

#### <a name="tcp-connect-error"></a>Error de conexión TCP

**Mensaje**: Application gateway could not connect to the backend.
Please check that the backend responds on the port used for the probe.
Also check whether any NSG/UDR/Firewall is blocking access to the Ip and port of this backend (Application Gateway no pudo conectarse al back-end. Compruebe que el back-end responde en el puerto usado para el sondeo. Compruebe también si NSG, UDR o el Firewall están bloqueando el acceso a la dirección IP y al puerto de este back-end).

**Causa:** después de la fase de resolución de DNS, Application Gateway intenta conectarse al servidor back-end en el puerto TCP establecido en la configuración de HTTP. Si Application Gateway no puede establecer una sesión TCP en el puerto especificado, el sondeo se marca como incorrecto con este mensaje.

**Solución:** si ve este error, compruebe lo siguiente:

1.  Compruebe si puede conectarse al servidor back-end en el puerto mencionado en la configuración de HTTP mediante un explorador o con PowerShell; por ejemplo, puede usar el comando: Test-NetConnection -ComputerName www.bing.com -Port 443

2.  Si el puerto mencionado no es el deseado, escriba el número de puerto correcto para que Application Gateway se conecte al servidor back-end.

3.  Si no puede conectarse tampoco en el puerto desde la máquina local, entonces:

    a.  Compruebe la configuración del grupo de seguridad de red de la subred y la NIC del servidor back-end, y si se permiten las conexiones entrantes al puerto configurado. Si no están permitidas, cree una nueva regla para permitirlas. Para aprender a crear reglas de grupo de seguridad de red, [consulte la página de documentación](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-security-rules).

    b.  Compruebe en la configuración del grupo de seguridad de red de la subred de Application Gateway si se permite el tráfico público y privado saliente, de modo que se pueda realizar la conexión. Consulte el documento vinculado en el punto (a) para obtener más información sobre cómo crear reglas de grupo de seguridad de red.
    ```azurepowershell
            $vnet = Get-AzVirtualNetwork -Name "vnetName" -ResourceGroupName "rgName"
            Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
    ```

    c.  Compruebe la configuración de UDR de la subred del servidor de back-end y Application Gateway para detectar anomalías de enrutamiento. Asegúrese de que la UDR no esté alejando el tráfico de la subred de back-end. Por ejemplo, compruebe las rutas a dispositivos virtuales de red o las rutas predeterminadas anunciadas en la subred de Application Gateway mediante ExpressRoute/VPN.

    d.  Para comprobar las rutas actuales y las reglas de una NIC, puede usar los siguientes comandos de PowerShell.
    ```azurepowershell
            Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
            Get-AzEffectiveRouteTable -NetworkInterfaceName "nic1" -ResourceGroupName "testrg"
    ```
4.  Si no encuentra ningún problema con el grupo de seguridad de red o la UDR, compruebe el servidor back-end para ver si hay problemas relacionados con la aplicación debido a que los clientes no puedan establecer una sesión TCP en los puertos configurados. Puntos que hay que comprobar:

    e.  Abra el símbolo del sistema (Win+R -\>cmd), escriba netstat y presione Entrar.

    f.  Compruebe si el servidor está escuchando en el puerto configurado.
        Por ejemplo:
    ```
            Proto Local Address Foreign Address State PID
            TCP 0.0.0.0:80 0.0.0.0:0 LISTENING 4
    ```
    g.  Si no es así, compruebe la configuración del servidor web, por ejemplo, los enlaces de sitio en IIS, el bloque de servidor en NGINX y el host virtual en Apache.

    h.  Compruebe la configuración del firewall del sistema operativo para asegurarse de que se permite el tráfico entrante al puerto.

#### <a name="http-status-code-mismatch"></a>Error de coincidencia en el código de estado HTTP

**Mensaje**: Status code of the backend\'s HTTP response did not match the probe setting. Expected:{HTTPStatusCode0} Received:{HTTPStatusCode1}. (El código de estado de la respuesta HTTP del back-end no coincidía con la configuración de sondeo. Se esperaba: {HTTPStatusCode0} Se recibió: {HTTPStatusCode1}).

**Causa:** una vez que se ha establecido la conexión TCP y se ha realizado el protocolo de enlace SSL (si SSL está habilitado), Application Gateway enviará el sondeo como una solicitud GET HTTP al servidor back-end. Como se mencionó anteriormente, el sondeo predeterminado se hará a \<protocolo\>://127.0.0.1:\<puerto\>/, y considera que los códigos de estado de respuesta en el intervalo de 200 a 399 son correctos. Si el servidor devuelve cualquier otro código de estado, se marcará como incorrecto con este mensaje.

**Solución:** según el código de respuesta del servidor back-end, puede realizar los pasos siguientes. A continuación se enumeran algunos de los códigos de estado comunes:

| **Error** | **Acciones** |
| --- | --- |
| Error de coincidencia de código de estado de sondeo: Recibido 401 | Compruebe si el servidor back-end requiere autenticación. Los sondeos de Application Gateway no pueden pasar credenciales para la autenticación en este momento. Permita \"HTTP 401\" en una coincidencia de código de estado de sondeo o sondee en una ruta de acceso en la que el servidor no requiera autenticación. | |
| Error de coincidencia de código de estado de sondeo: Recibido 403 | Acceso prohibido. Compruebe si se permite el acceso a la ruta de acceso en el servidor back-end. | |
| Error de coincidencia de código de estado de sondeo: Recibido 404 | Página no encontrada. Compruebe si la ruta de acceso del nombre de host es accesible en el servidor back-end. Cambie el parámetro de ruta de acceso o el nombre de host a un valor accesible. | |
| Error de coincidencia de código de estado de sondeo: Recibido 405 | Las solicitudes de sondeo de Application Gateway usan el método GET HTTP. Compruebe si el servidor lo permite. | |
| Error de coincidencia de código de estado de sondeo: Recibido 500 | Error interno del servidor Compruebe el estado del servidor back-end y si los servicios se están ejecutando. | |
| Error de coincidencia de código de estado de sondeo: Recibido 503 | Servicio no disponible. Compruebe el estado del servidor back-end y si los servicios se están ejecutando. | |

O bien, si cree que la respuesta es legítima y desea que Application Gateway acepte otros códigos de estado como correctos, puede crear un sondeo personalizado. Esta modificación resultará útil en situaciones en las que el sitio web de back-end necesite autenticación y, dado que las solicitudes de sondeo no incluyen ninguna credencial de usuario, se producirá un error y el servidor back-end devolverá un código de estado HTTP 401.

Para crear un sondeo personalizado, siga los pasos enumerados [aquí](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal).

#### <a name="http-response-body-mismatch"></a>Error de coincidencia del cuerpo de la respuesta HTTP

**Mensaje**: Body of the backend\'s HTTP response did not match the probe setting. Received response body does not contain {string}. (El cuerpo de la respuesta HTTP del back-end no coincidía con la configuración de sondeo. El cuerpo de respuesta recibido no contiene {string}).

**Causa:** al crear un sondeo personalizado, tiene una opción para marcar un servidor back-end con estado correcto si hace coincidir una cadena del cuerpo de la respuesta. Por ejemplo, puede configurar Application Gateway para que acepte "no autorizado" como una cadena que coincida. Si la respuesta del servidor back-end para la solicitud de sondeo contiene la cadena "no autorizado", se marcará como correcta.
De lo contrario, se marcará como incorrecta y se generará este mensaje.

**Solución:** para resolver este problema, puede seguir los pasos que se indican a continuación:

1.  Acceda al servidor back-end localmente o desde una máquina cliente en la ruta de acceso de sondeo y compruebe el cuerpo de la respuesta.

2.  Compruebe la configuración del sondeo personalizado de Application Gateway para averiguar si el cuerpo de la respuesta coincide con lo que se ha configurado.

3.  Si no coincide, cambie la configuración de sondeo por el valor de cadena correcto que aceptar.

Puede leer más sobre la coincidencia del sondeo de Application Gateway [aquí](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching).

#### <a name="backend-server-certificate-invalid-ca"></a>Entidad de certificación no válida del certificado de servidor back-end

**Mensaje**: The server certificate used by the backend is not signed by a well-known Certificate Authority (CA). Whitelist the backend on the application gateway by uploading the root certificate of the server certificate used by the backend. (La entidad de certificación conocida no firma el certificado de servidor utilizado por el back-end. Incluya en la lista de elementos permitidos el back-end en la puerta de enlace de aplicaciones mediante la carga del certificado raíz del certificado de servidor utilizado por el back-end).

**Causa:** SSL de un extremo a otro con Application Gateway v2 requiere que se compruebe el certificado del servidor back-end para que el estado del servidor se considere correcto.
Para que un certificado SSL sea de confianza, ese certificado del servidor back-end debe haber sido emitido por una entidad de certificación incluida en el almacén de confianza de Application Gateway. Si el certificado no fue emitido por una entidad de certificación de confianza, por ejemplo, los certificados autofirmados, los usuarios deben cargar el certificado del emisor en Application Gateway.

**Solución:** siga los pasos que se indican a continuación para exportar y cargar el certificado raíz de confianza en Application Gateway (para clientes Windows).

1.  Inicie sesión en la máquina donde se hospede la aplicación.

2.  Para abrir Ejecutar, presione Win+R o haga clic con el botón derecho en el botón Inicio y seleccione Ejecutar.

3.  Escriba certmgr.msc y presione Entrar. También puede buscar el Administrador de certificados en el menú Inicio.

4.  Busque el certificado; normalmente se encuentra en \'Certificados - Usuario actual\\Personal\\Certificados\'. Después, abra el certificado.

5.  En Propiedades del certificado, seleccione la pestaña Ruta de certificación.

6.  Seleccione el certificado raíz y la opción "Ver certificado".

7.  En Propiedades del certificado, cambie a la pestaña Detalles.

8.  En la pestaña Detalles, seleccione la opción "Copiar a archivo" y guarde el archivo en el formato X.509 codificada en Base 64 (.CER).

9.  Una vez guardado, abra la página Configuración HTTP de Application Gateway en Azure Portal.

10. Abra la configuración HTTP y haga clic en "Agregar certificado"; después, busque el archivo de certificado que hemos guardado recientemente.

11. Haga clic en Guardar para guardar la configuración HTTP.

Como alternativa, para exportar el certificado raíz desde una máquina cliente, puede acceder directamente al servidor (omitiendo Application Gateway) mediante un explorador y exportar el certificado raíz desde él.

Para pasos detallados sobre cómo extraer y cargar certificados raíz de confianza en Application Gateway, consulte [aquí](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).

#### <a name="trusted-root-certificate-mismatch"></a>Error de coincidencia de certificado raíz de confianza

**Mensaje**: The root certificate of the server certificate used by the backend does not match the trusted root certificate added to the application gateway. Ensure that you add the correct root certificate to whitelist the backend (El certificado raíz del certificado de servidor utilizado por el back-end no coincide con el certificado raíz de confianza que se ha agregado a Application Gateway. Asegúrese de agregar el certificado raíz correcto para incluir en la lista de elementos permitidos en el back-end).

**Causa:** SSL de un extremo a otro con Application Gateway v2 requiere que se compruebe el certificado del servidor back-end para que el estado del servidor se considere correcto.
Para que un certificado SSL sea de confianza, ese certificado del servidor back-end debe haber sido emitido por una entidad de certificación incluida en el almacén de confianza de Application Gateway. Si el certificado no fue emitido por una entidad de certificación de confianza, por ejemplo, los certificados autofirmados, los usuarios deben cargar el certificado del emisor en Application Gateway.

El certificado que se ha cargado en la configuración HTTP de Application Gateway debe coincidir con el certificado raíz del certificado del servidor back-end.

**Solución:** si ve el mensaje de error mencionado anteriormente, significa que hay un error de coincidencia entre el certificado que se ha cargado en Application Gateway y el que se ha cargado en el servidor back-end.

Siga los pasos 1 a 11 mencionados anteriormente para cargar el certificado raíz de confianza adecuado en Application Gateway.

Para pasos detallados sobre cómo extraer y cargar certificados raíz de confianza en Application Gateway, consulte [aquí](https://docs.microsoft.com/azure/application-gateway/certificates-for-backend-authentication#export-trusted-root-certificate-for-v2-sku).
> [!NOTE]
> El error mencionado también podría producirse si el servidor back-end no está intercambiando la cadena completa del certificado, incluido el elemento Raíz > Intermedio (si es aplicable) > Hoja durante el protocolo de enlace de TLS. Para comprobarlo, puede usar los comandos de OpenSSL desde cualquier cliente y conectarse al servidor back-end con los valores configurados en el sondeo de Application Gateway.

Por ejemplo,
```
OpenSSL> s_client -connect 10.0.0.4:443 -servername www.example.com -showcerts
```
Si la salida no muestra la cadena completa del certificado que se va a devolver, vuelva a exportar el certificado con la cadena completa, incluido el certificado raíz, y configúrelo en el servidor back-end. 

CONNECTED(00000188)\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=20:unable to get local issuer certificate\
verify return:1\
depth=0 OU = Domain Control Validated, CN = \*.example.com\
verify error:num=21:unable to verify the first certificate\
verify return:1\
\-\-\-\
Certificate chain\
 0 s:/OU=Domain Control Validated/CN=*.example.com\
   i:/C=US/ST=Arizona/L=Scottsdale/O=GoDaddy.com, Inc./OU=http://certs.godaddy.com/repository//CN=Go Daddy Secure Certificate Authority - G2\
\-----BEGIN CERTIFICATE-----\
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\
\-----END CERTIFICATE-----

#### <a name="backend-certificate-invalid-common-name-cn"></a>Nombre común (CN) del certificado de back-end no válido

**Mensaje**: The Common Name (CN) of the backend certificate does not match the host header of the probe. [El nombre común (CN) del certificado de back-end no coincide con el encabezado de host del sondeo].

**Causa:** Application Gateway valida si el nombre de host especificado en la configuración HTTP del back-end coincide con el del nombre común (CN) presentado por el certificado SSL del servidor de back-end. Esto es solo un comportamiento de la SKU Standard_v2 y WAF_v2. La SNI de la SKU Standard y WAF se establece como el nombre de dominio completo en la dirección del grupo de back-end.

En la versión 2 de la SKU, si hay un sondeo predeterminado (no hay ningún sondeo personalizado configurado ni asociado), la SNI se establecerá a partir del nombre de host mencionado en la configuración HTTP, o si se menciona que hay que elegir el nombre de host desde la dirección de back-end en la configuración HTTP, donde el grupo de direcciones de back-end contiene un nombre de dominio completo válido.

Si hay un sondeo personalizado asociado a la configuración HTTP, la SNI se establecerá a partir del nombre de host mencionado en la configuración de sondeo personalizado; o si la opción para elegir el nombre de host desde la configuración HTTP de back-end está activada en el sondeo personalizado, se establecerá a partir del nombre de host mencionado en la configuración HTTP.

En caso de que la opción para elegir el nombre de host desde la dirección de back-end se establezca en la configuración HTTP, el grupo de direcciones de back-end debe contener un nombre de dominio completo válido.

Si ve el mensaje de error mencionado anteriormente, significa que el nombre común (CN) del certificado de back-end no coincide con el nombre de host configurado en el sondeo personalizado o en la configuración HTTP (en caso de que se elija el nombre de host desde la configuración HTTP de back-end). Si usa un sondeo predeterminado, el nombre de host se establecería como "127.0.0.1". Si no se desea ese valor, debe crear un sondeo personalizado y asociarlo a la configuración HTTP.

**Solución:**

para solucionar el problema, siga estos pasos:

Para Windows:

1.  Inicie sesión en la máquina donde se hospede la aplicación.

2.  Para abrir Ejecutar, presione Win+R o haga clic con el botón derecho en el botón Inicio y seleccione Ejecutar.

3.  Escriba certmgr.msc y presione Entrar. También puede buscar el Administrador de certificados en el menú Inicio.

4.  Busque el certificado; normalmente se encuentra en \'Certificados - Usuario actual\\Personal\\Certificados\'. Después, abra el certificado.

5.  En la pestaña Detalles, compruebe el asunto del certificado.

6.  Compruebe el nombre común del certificado en los detalles y escriba el mismo en el campo de nombre de host del sondeo personalizado o en la configuración HTTP (en caso de que se decida elegir el nombre de host desde la configuración HTTP de back-end). Si ese no es el nombre de host que se desea para el sitio web, debe obtener un certificado para ese dominio o escribir el nombre de host correcto en la configuración del sondeo personalizado o de HTTP.

En Linux con OpenSSL

1.  Ejecute este comando en OpenSSL. 
```
openssl x509 -in certificate.crt -text -noout
```

2.  En las propiedades mostradas, busque el nombre común del certificado y escriba el mismo en el campo Nombre de host de la configuración HTTP. Si ese no es el nombre de host que se desea para el sitio web, debe obtener un certificado para ese dominio o escribir el nombre de host correcto en la configuración del sondeo personalizado o de HTTP.

#### <a name="backend-certificate-is-invalid"></a>El certificado de back-end no es válido.

**Mensaje**: Backend certificate is invalid. (El certificado de back-end no es válido). La fecha actual está dentro del intervalo de fechas \"Válido desde\" y \"Válido hasta\" en el certificado.

**Causa:** cada certificado viene con una validez y la conexión HTTPS no será segura a menos que el certificado SSL del servidor sea válido.
Los datos actuales deben estar dentro del intervalo de validez indicado antes. Si no es así, el certificado se considerará no válido y esto constituye un problema de seguridad. En este punto, Application Gateway marcará el servidor back-end como incorrecto.

**Solución:** si el certificado SSL ha expirado, renuévelo con el proveedor y actualice la configuración del servidor con el nuevo certificado. Si se trata de un certificado autofirmado, debe generar un certificado válido y cargar el certificado raíz en la configuración HTTP de Application Gateway. Para ello, siga estos pasos:

1.  Abra la configuración HTTP de Application Gateway en el portal.

2.  Seleccione la configuración HTTP que tenga el certificado expirado, seleccione Agregar certificado y abra el nuevo archivo de certificado.

3.  Elimine el certificado antiguo con el icono para eliminar situado junto a él y haga clic en Guardar.

#### <a name="certificate-verification-failed"></a>Error en la verificación del certificado

**Mensaje**: The validity of the backend certificate could not be verified. To find out the reason, check Open SSL diagnostics for the message associated with error code {errorCode}. [No se pudo comprobar la validez del certificado de back-end. Para averiguar el motivo, compruebe el diagnóstico de OpenSSL para el mensaje asociado al código de error {errorCode}]

**Causa:** este error se produce cuando Application Gateway no puede comprobar la validez del certificado.

**Solución:** para resolver este problema, compruebe el certificado en el servidor si se ha creado correctamente. Por ejemplo, puede usar [OpenSSL](https://www.openssl.org/docs/man1.0.2/man1/verify.html) para comprobar el certificado y sus propiedades e intentar volver a cargar el certificado en la configuración HTTP de Application Gateway.

<a name="backend-health-status-unknown"></a>Estado del back-end desconocido
-------------------------------
Si el estado del back-end aparece como desconocido, se mostrará en el portal, como en la siguiente captura de pantalla:

![Estado del back-end Application Gateway: unknown (desconocido)](./media/application-gateway-backend-health-troubleshooting/appgwunknown.png)

En caso de que el estado del back-end se muestre como desconocido, podría deberse a una o varias de las razones siguientes:

1.  El grupo de seguridad de red en la subred de Application Gateway está bloqueando el acceso de entrada a los puertos 65503-65534 (SKU v1) o 65200-65535 (SKU v2) desde Internet
2.  La ruta definida por el usuario en la subred de Application Gateway con la ruta predeterminada (0.0.0.0/0) con el próximo salto distinto de Internet
3.  La ruta predeterminada anunciada por la conexión VPN/ExpressRoute a la red virtual a través de BGP
4.  El servidor DNS personalizado está configurado en la red virtual y no puede resolver nombres de dominio públicos
5.  Application Gateway está en un estado incorrecto

**Solución:**

1.  compruebe si el grupo de seguridad de red está bloqueando el acceso a los puertos 65503-65534 (SKU v1) o 65200-65535 (SKU v2) desde Internet.

    a.  En la pestaña "Información general" de Application Gateway, seleccione el vínculo Red virtual/Subred.

    b.  En la pestaña Subredes de la red virtual, seleccione la subred en la que se ha implementado Application Gateway.

    c.  Compruebe si hay configurado algún grupo de seguridad de red.

    d.  Si hay alguno, busque ese recurso de grupo de seguridad de red en la pestaña de búsqueda o en todos los recursos.

    e.  En la sección Reglas de entrada, agregue una regla de entrada para permitir el intervalo de puertos de destino 65503-65534 para la SKU v1 o 65200-65535 para SKU v2 con el origen como cualquiera o como Internet.

    f.  Haga clic en Guardar y compruebe si puede ver el back-end correcto.

    g.  Como alternativa, puede hacerlo en [PowerShell o en la CLI](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

2.  Compruebe si el UDR tiene una ruta predeterminada (0.0.0.0/0) con el próximo salto distinto de Internet.
    
    a.  Siga los pasos 1.a y 1. b para determinar la subred.

    b.  Compruebe si hay configurado algún UDR. Si hay alguno, busque el recurso en la barra de búsqueda o debajo de Todos los recursos.

    c.  Compruebe si hay rutas predeterminadas (0.0.0.0/0) con el próximo salto distinto de Internet. Si se trata de un dispositivo virtual o una puerta de enlace de red virtual, debe asegurarse de que el dispositivo virtual o el dispositivo en el entorno local puedan enrutar correctamente el paquete al destino de Internet sin modificar el paquete.

    d.  En caso contrario, cambie el próximo salto a Internet, haga clic en Guardar y compruebe el estado del back-end.

3.  La ruta predeterminada anunciada por la conexión VPN/ExpressRoute a la red virtual a través de BGP.

    a.  Si tiene una conexión de ExpressRoute o VPN a la red virtual a través de BGP y está anunciando una ruta predeterminada, debe asegurarse de que el paquete se redirige al destino de Internet sin modificarlo.

    b.  Puede comprobarlo mediante la opción "Solución de problemas de conexión" en el portal de Application Gateway.

    c.  Elija manualmente como destino cualquier dirección IP enrutable a Internet, por ejemplo "1.1.1.1", y cualquier puerto de destino, y compruebe la conectividad.

    d.  Si el próximo salto es una puerta de enlace de red virtual, es posible que haya una ruta predeterminada anunciada a través de ExpressRoute o de VPN.

4.  Si hay un servidor DNS personalizado configurado en la red virtual, compruebe si los servidores pueden resolver los dominios públicos. La resolución de nombres de dominios públicos podría ser necesaria en escenarios en los que Application Gateway deba ponerse en contacto con dominios externos como los servidores OCSP o para comprobar el estado de revocación del certificado, etc.

5.  Para comprobar si Application Gateway está en un estado correcto y en ejecución, vaya a la opción Resource Health (Estado de los recursos) en el portal y compruebe si es "Healthy" (correcto). Si ve el estado Unhealthy (Incorrecto) o Degraded (Degradado), [póngase en contacto con el soporte técnico](https://azure.microsoft.com/support/options/).

<a name="next-steps"></a>Pasos siguientes
----------

Para más información sobre cómo habilitar los registros y el diagnóstico de Application Gateway, consulte [aquí](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
