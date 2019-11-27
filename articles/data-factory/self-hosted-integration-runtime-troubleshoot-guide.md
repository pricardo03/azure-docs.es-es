---
title: Solución de problemas de un entorno de ejecución de integración autohospedado en Azure Data Factory
description: Conozca cómo solucionar problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: 9adbc3d7d30aeb8c7cb2b89c326ac2b39a2e8d2b
ms.sourcegitcommit: 6dec090a6820fb68ac7648cf5fa4a70f45f87e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2019
ms.locfileid: "73907224"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Solución de problemas del entorno de ejecución de integración autohospedado

En este artículo se exploran los métodos comunes de solución de problemas del entorno de ejecución de integración autohospedado en Azure Data Factory.

## <a name="common-errors-and-resolutions"></a>Errores comunes y soluciones

### <a name="error-message-self-hosted-integration-runtime-is-unable-to-connect-to-cloud-service"></a>Mensaje de error: El entorno de ejecución de integración autohospedado no puede conectarse al servicio en la nube.

- **Síntoma**: 

    ![problema de conexión de IR autohospedado](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

- **Causa**: el entorno de ejecución de integración autohospedado no puede conectarse al servicio Data Factory (back-end). Lo normal es que no se deba a la configuración de red del firewall.

- **Resolución**: 

    1. compruebe si el servicio de Windows "Integration Runtime Service" se está ejecutando.
    
        ![Estado de ejecución del servicio IR autohospedado](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
    2. Si el servicio de Windows, tal como se muestra en [1], se está ejecutando, siga las instrucciones siguientes según corresponda:

        1. Si el proxy no está configurado en el entorno de ejecución de integración autohospedado (la configuración predeterminada es sin proxy), ejecute el siguiente comando de PowerShell en la máquina donde está instalado el entorno de ejecución de integración autohospedado: 
            
            ```powershell
            (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
            ```
            > [!NOTE] 
            > La dirección URL del servicio puede variar en función de la ubicación de la factoría de datos. Puede encontrar la dirección URL del servicio en la interfaz de usuario de ADF: Connections -> Integration runtimes -> Edit Self-hosted IR -> Nodes -> View Service URLs (Conexiones > Entornos de ejecución de integración > Editar IR autohospedado > Nodos > Ver direcciones URL del servicio).
            
            Esta es la respuesta esperada:
            
            ![Respuesta del comandos de PowerShell](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
            Si la respuesta es diferente, siga las instrucciones que se indican a continuación, según corresponda:
            
            * Si recibe el error "No se pudo resolver el nombre remoto", hay un problema con DNS. Póngase en contacto con el equipo de red para solucionarlo. 
            * Si recibe el error "El certificado SSL/TLS no es de confianza", compruebe si el certificado de "https://wu2.frontend.clouddatahub.net/" es de confianza en la máquina e instale el certificado público mediante el administrador de certificados. Lo normal es que el problema se mitigue.
            * Compruebe si hay errores en Windows -> Visor de eventos (registros) -> Registros de aplicaciones y servicios -> Integration Runtime; la mayoría de ellos se deben a DNS, la regla de firewall y la configuración de red de la empresa, que obligan a cerrar la conexión. Para este problema, póngase en contacto con su equipo de red para buscar otras soluciones, ya que cada empresa tiene una configuración de red personalizada.

        2. Si el proxy se ha configurado en el entorno de ejecución de integración autohospedado, compruebe si el servidor proxy puede acceder a nuestro punto de conexión de servicio. Para ver un comando de ejemplo, consulte [aquí](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
            ```powershell
            $user = $env:username
            $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
            Settings').ProxyServer
            $pwd = Read-Host "Password?" -assecurestring
            $proxy = new-object System.Net.WebProxy
            $proxy.Address = $webproxy
            $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
            $proxy.credentials = $account
            $url = "https://wu2.frontend.clouddatahub.net/"
            $wc = new-object system.net.WebClient
            $wc.proxy = $proxy
            $webpage = $wc.DownloadData($url)
            $string = [System.Text.Encoding]::ASCII.GetString($webpage)
            $string
            ```

            Esta es la respuesta esperada:
            
            ![Respuesta del comando de PowerShell 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

            > [!NOTE] 
            > Consideraciones del proxy:
            > * Compruebe si el servidor proxy requiere la inclusión en la lista de permitidos. En ese caso, incluya [estos dominios](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) en la lista de permitidos.
            > * Compruebe que el certificado TLS/SSL de "wu2.frontend.clouddatahub.net/" es de confianza en el servidor proxy.
            > * Si usa la autenticación de Active Directory en el proxy, cambie la cuenta de servicio por la cuenta de usuario que pueda acceder al proxy como "Integration Runtime Service".

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Mensaje de error: El nodo del entorno de ejecución de integración autohospedado/SHIR lógico está en estado inactivo/"en ejecución (limitado)"

- **Causa**: puede ver el nodo de IR autohospedado en estado inactivo, tal como se muestra en la siguiente captura de pantalla:

    ![Nodo de IR autohospedado inactivo](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

    Este problema se produce cuando los nodos no pueden comunicarse entre sí. 

- **Resolución**: 

    inicie sesión en la máquina virtual hospedada en el nodo y abra la vista de eventos. En Registros de aplicaciones y servicios-> Integration Runtime, filtre todos los registros de errores. 

     1. Si el registro de errores contiene: 
    
        **Registro de errores**: System.ServiceModel.EndpointNotFoundException: No se pudo conectar a net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. La duración del intento de conexión fue de 00:00:00.9940994. Código de error TCP 10061: No se estableció ninguna conexión porque el equipo de destino la rechazó 10.2.4.10:8060.  ---> System.Net.Sockets.SocketException: No se estableció ninguna conexión porque el equipo de destino la rechazó 10.2.4.10:8060
    
           en System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
           
           en System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
           
           en System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    
        **Solución:** inicie la línea de comandos: telnet 10.2.4.10 8060
        
        Si recibe el siguiente error, póngase en contacto con el personal de TI para que le ayude a solucionar este problema. Una vez que haya conseguido la comunicación con Telnet, póngase en contacto con el servicio de soporte técnico de Microsoft si todavía tiene problemas con el estado del nodo de IR.
        
        ![Error de la línea de comandos](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
     2. Si el registro de errores contiene:
     
        **Registro de errores:** No se puede conectar con el administrador del trabajo: net.tcp://xxxxxx:8060/ExternalService.svc/ No existen entradas DNS para el host azranlcir01r1. Host desconocido. Detalle de la excepción: System.ServiceModel.EndpointNotFoundException: No existen entradas DNS para el host xxxxx. ---> System.Net.Sockets.SocketException: Host desconocido en System.Net.Dns.GetAddrInfo(String name) en System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) en System.Net.Dns.GetHostEntry(String hostNameOrAddress) en System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- Fin del seguimiento de la pila de la excepción interna --- Seguimiento de la pila del servidor: en System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) 
    
        **Solución:** una de las dos acciones siguientes puede ayudar a resolver el problema:
         1. Colocar todos los nodos en el mismo dominio
         2. Agregar la asignación de IP a host en todo el archivo de hosts de la máquina virtual hospedada


## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Guía de optimización y rendimiento de Mapping Data Flows](concepts-data-flow-performance.md)
