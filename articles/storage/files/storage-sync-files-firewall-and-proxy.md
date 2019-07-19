---
title: Configuración del proxy y el firewall locales de Azure File Sync | Microsoft Docs
description: Configuración de la red local de Azure File Sync
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/24/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: fdbbea2d32762d2d4030ec3a10826595dadd371c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449828"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Configuración del proxy y el firewall de Azure File Sync
Azure File Sync conecta los servidores locales a Azure Files, lo que permite sincronizar las características de niveles de nube y de sincronización multisitio. Por lo tanto, un servidor local debe estar conectado a Internet. Un administrador de TI tiene que decidir cuál es la mejor ruta de acceso para que el servidor acceda a los servicios en la nube de Azure.

Este artículo ofrece un resumen de las opciones y los requisitos específicos disponibles para conectar el servidor a Azure File Sync de forma correcta y segura.

> [!Important]
> Azure File Sync aún no admite firewalls ni redes virtuales para una cuenta de almacenamiento.

## <a name="overview"></a>Información general
Azure File Sync funciona como un servicio de orquestación entre el servidor Windows, el recurso compartido de archivos de Azure y otros servicios de Azure con el objetivo de sincronizar los datos tal y como se describe en el grupo de sincronización. Para que Azure File Sync funcione correctamente, debe configurar los servidores para que se comuniquen con los siguientes servicios de Azure:

- Azure Storage
- Azure File Sync
- Azure Resource Manager
- Servicios de autenticación

> [!Note]  
> El agente de Azure File Sync en Windows Server inicia todas las solicitudes a servicios en la nube, por lo que solo hay que tener en cuenta el tráfico saliente en el firewall. <br /> Ningún servicio de Azure inicia una conexión con el agente de Azure File Sync.

## <a name="ports"></a>Puertos
Azure File Sync mueve los metadatos y los datos de archivo exclusivamente a través de HTTPS y requiere que el puerto 443 esté abierto al tráfico saliente.
Como resultado, se cifra todo el tráfico.

## <a name="networks-and-special-connections-to-azure"></a>Redes y conexiones especiales a Azure
El agente de Azure File Sync no tiene ningún requisito en lo que respecta a canales especiales como [ExpressRoute](../../expressroute/expressroute-introduction.md), etc., en Azure.

Azure File Sync funciona con cualquier medio disponible que permita conectarse con Azure, de modo que se adapta automáticamente a diversas características de red, como el ancho de banda, la latencia y la posibilidad de usar el control de administración para realizar ajustes. No todas las características están disponibles en este momento. Si desea configurar un comportamiento específico, comuníquelo en [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
Azure File Sync admite la configuración del proxy específico de aplicación en el nivel de máquina.

De este modo, se permite la **configuración de un proxy específico de la aplicación**, específicamente para el tráfico de Azure File Sync. La configuración de un proxy específico de la aplicación es compatible con la versión 4.0.1.0 o versiones posteriores, y se puede configurar durante la instalación del agente o mediante el cmdlet Set-StorageSyncProxyConfiguration de PowerShell.

Comandos de PowerShell para configurar el proxy específico de la aplicación:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
**Esta configuración de proxy en el nivel de máquina** es transparente para el agente de Azure File Sync, ya que todo el tráfico del servidor se enruta a través de este proxy.

Para configurar los valores de proxy en el nivel de máquina, siga estos pasos: 

1. Configuración de valores de proxy para aplicaciones de .NET 

   - Edite estos dos archivos:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - Agregue la sección <system.net> en los archivos machine.config (debajo de la sección <system.serviceModel>).  Cambie 127.0.01:8888 a la dirección IP y puerto del servidor proxy. 
     ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
     ```

2. Establecimiento de la configuración del proxy WinHTTP 

   - Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados o PowerShell para ver la configuración del proxy existente:   

     netsh winhttp show proxy

   - Ejecute el siguiente comando desde un símbolo del sistema con privilegios elevados o PowerShell para establecer la configuración de proxy (cambie 127.0.01:8888 a la dirección IP y puerto del servidor proxy):  

     netsh winhttp set proxy 127.0.0.1:8888

3. Reinicie el servicio del agente de sincronización de Azure Storage ejecutando el comando siguiente desde un símbolo del sistema con privilegios elevados o PowerShell: 

      net stop filesyncsvc

      Nota: El servicio del agente de sincronización de Storage (filesyncsvc) se iniciará automáticamente una vez detenido.

## <a name="firewall"></a>Firewall
Como se mencionó en una sección anterior, el puerto 443 tiene que abrirse al tráfico saliente. En función de las directivas del centro de datos, la rama o la región, puede ser recomendable u obligatorio restringir aún más el tráfico a través de este puerto a dominios concretos.

En la tabla siguiente se describen los dominios necesarios para la comunicación:

| Servicio | Punto de conexión de la nube pública | Punto de conexión de Azure Government | Uso |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | https://management.usgovcloudapi.net | Todas las llamadas de usuario (como PowerShell) van a esta URL, incluida la llamada de registro inicial del servidor. |
| **Azure Active Directory** | https://login.windows.net | https://login.microsoftonline.us | Las llamadas de Azure Resource Manager deben realizarlas usuarios autenticados. Para que el proceso se complete correcta, esta URL se utiliza para la autenticación de usuarios. |
| **Azure Active Directory** | https://graph.windows.net/ | https://graph.windows.net/ | Como parte de la implementación de Azure File Sync, se crea una entidad de servicio en la instancia de Azure Active Directory de la suscripción. Esta URL se utiliza con ese fin. Esta entidad de seguridad se usa para delegar un conjunto mínimo de derechos en el servicio Azure File Sync. El usuario que realiza la configuración inicial de Azure File Sync debe ser un usuario autenticado con privilegios de propietario en la suscripción. |
| **Azure Storage** | &ast;.core.windows.net | &ast;.core.usgovcloudapi.net | Cuando el servidor descarga un archivo, realiza el movimiento de datos de forma más eficaz al comunicarse directamente con el recurso compartido de archivos de Azure en la cuenta de almacenamiento. El servidor tiene una clave SAS que solo se permite el acceso al recurso compartido de archivos de destino. |
| **Azure File Sync** | &ast;.one.microsoft.com | &ast;. afs.azure.us | Después del registro inicial del servidor, el servidor recibe una URL regional para la instancia de servicio de Azure File Sync en dicha región. El servidor puede utilizar la URL para comunicarse de forma directa y eficaz con la instancia que controla la sincronización. |
| **Microsoft PKI** | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | Una vez instalado el agente Azure File Sync, la dirección URL de PKI se utiliza para descargar los certificados intermedios necesarios para comunicarse con el servicio Azure File Sync y el recurso compartido de archivos de Azure. La dirección URL de OCSP se utiliza para comprobar el estado de un certificado. |

> [!Important]
> Al permitir el tráfico a &ast;.one.microsoft.com, es posible dirigir el tráfico del servidor a más recursos que simplemente el servicio de sincronización. Existen muchos otros servicios de Microsoft en subdominios.

Si &ast;.one.microsoft.com es demasiado extensa, puede limitar la comunicación del servidor permitiendo solo la comunicación con instancias regionales explícitas del servicio Azure Files Sync. La elección de las instancias dependerá de la región del servicio de sincronización de almacenamiento en el que haya implementado y registrado el servidor. Esa región se denomina "Dirección URL del punto de conexión principal" en la tabla siguiente.

Por razones de continuidad empresarial y recuperación ante desastres (BCDR), es posible que haya especificado los recursos compartidos de archivos de Azure en una cuenta de almacenamiento globalmente redundante (GRS). Si es así, los recursos compartidos de archivos de Azure conmutarán por error a la región emparejada en caso de una interrupción regional duradera. Azure File Sync usa los mismos emparejamientos regionales que el almacenamiento. Por lo tanto, si usa cuentas de almacenamiento GRS, deberá habilitar direcciones URL adicionales para permitir que el servidor se comunique con la región emparejada para Azure File Sync. En la tabla siguiente, se denomina "Región emparejada". Además, hay una dirección URL del perfil de Traffic Manager que debe habilitarse también. Esto asegurará que el tráfico de red se pueda redirigir sin problemas a la región emparejada en caso de que se produzca una conmutación por error y se denomina "URL de detección" en la tabla siguiente.

| Nube  | Region | Dirección URL del punto de conexión principal | Región emparejada | Dirección URL de detección |
|--------|--------|----------------------|---------------|---------------|
| Público |Este de Australia | https://kailani-aue.one.microsoft.com | Sudeste de Australia | https://tm-kailani-aue.one.microsoft.com |
| Público |Sudeste de Australia | https://kailani-aus.one.microsoft.com | Este de Australia | https://tm-kailani-aus.one.microsoft.com |
| Público | Sur de Brasil | https://brazilsouth01.afs.azure.net | Centro-Sur de EE. UU | https://tm-brazilsouth01.afs.azure.net |
| Público | Centro de Canadá | https://kailani-cac.one.microsoft.com | Este de Canadá | https://tm-kailani-cac.one.microsoft.com |
| Público | Este de Canadá | https://kailani-cae.one.microsoft.com | Centro de Canadá | https://tm-kailani.cae.one.microsoft.com |
| Público | India Central | https://kailani-cin.one.microsoft.com | Sur de la India | https://tm-kailani-cin.one.microsoft.com |
| Público | Centro de EE. UU. | https://kailani-cus.one.microsoft.com | Este de EE. UU. 2 | https://tm-kailani-cus.one.microsoft.com |
| Público | Asia oriental | https://kailani11.one.microsoft.com | Sudeste asiático | https://tm-kailani11.one.microsoft.com |
| Público | Este de EE. UU | https://kailani1.one.microsoft.com | Oeste de EE. UU. | https://tm-kailani1.one.microsoft.com |
| Público | Este de EE. UU. 2 | https://kailani-ess.one.microsoft.com | Centro de EE. UU. | https://tm-kailani-ess.one.microsoft.com |
| Público | Este de Japón | https://japaneast01.afs.azure.net | Oeste de Japón | https://tm-japaneast01.afs.azure.net |
| Público | Oeste de Japón | https://japanwest01.afs.azure.net | Este de Japón | https://tm-japanwest01.afs.azure.net |
| Público | Corea Central | https://koreacentral01.afs.azure.net/ | Corea del Sur | https://tm-koreacentral01.afs.azure.net/ |
| Público | Corea del Sur | https://koreasouth01.afs.azure.net/ | Corea Central | https://tm-koreasouth01.afs.azure.net/ |
| Público | Centro-Norte de EE. UU | https://northcentralus01.afs.azure.net | Centro-Sur de EE. UU | https://tm-northcentralus01.afs.azure.net |
| Público | Europa del Norte | https://kailani7.one.microsoft.com | Europa occidental | https://tm-kailani7.one.microsoft.com |
| Público | Centro-Sur de EE. UU | https://southcentralus01.afs.azure.net | Centro-Norte de EE. UU | https://tm-southcentralus01.afs.azure.net |
| Público | Sur de la India | https://kailani-sin.one.microsoft.com | India Central | https://tm-kailani-sin.one.microsoft.com |
| Público | Sudeste asiático | https://kailani10.one.microsoft.com | Asia oriental | https://tm-kailani10.one.microsoft.com |
| Público | Sur de Reino Unido 2 | https://kailani-uks.one.microsoft.com | Oeste de Reino Unido | https://tm-kailani-uks.one.microsoft.com |
| Público | Oeste de Reino Unido | https://kailani-ukw.one.microsoft.com | Sur de Reino Unido 2 | https://tm-kailani-ukw.one.microsoft.com |
| Público | Centro occidental de EE.UU. | https://westcentralus01.afs.azure.net | Oeste de EE. UU. 2 | https://tm-westcentralus01.afs.azure.net |
| Público | Europa occidental | https://kailani6.one.microsoft.com | Europa del Norte | https://tm-kailani6.one.microsoft.com |
| Público | Oeste de EE. UU. | https://kailani.one.microsoft.com | Este de EE. UU | https://tm-kailani.one.microsoft.com |
| Público | Oeste de EE. UU. 2 | https://westus201.afs.azure.net | Centro occidental de EE.UU. | https://tm-westus201.afs.azure.net |
| Government | Gobierno de EE. UU.: Arizona | https://usgovarizona01.afs.azure.us | Gobierno de EE. UU.: Texas | https://tm-usgovarizona01.afs.azure.us |
| Government | Gobierno de EE. UU.: Texas | https://usgovtexas01.afs.azure.us | Gobierno de EE. UU.: Arizona | https://tm-usgovtexas01.afs.azure.us |

- Si utiliza cuentas de almacenamiento con redundancia local (LRS) o de almacenamiento con redundancia de zona (ZRS), solo tiene que habilitar la URL que aparece en "Dirección URL del punto de conexión principal".

- Si usa cuentas de almacenamiento con redundancia geográfica (GRS), habilite tres direcciones URL.

**Ejemplo:** Implemente un servicio de sincronización de almacenamiento en `"West US"` y registre el servidor allí. Las direcciones URL que permiten que el servidor se comunique en este caso son:

> - https://kailani.one.microsoft.com (punto de conexión principal: Oeste de EE. UU.)
> - https://kailani1.one.microsoft.com (región emparejada de conmutación por error: Este de EE. UU.)
> - https://tm-kailani.one.microsoft.com (URL de detección de la región principal)

## <a name="summary-and-risk-limitation"></a>Resumen y limitación de riesgos
Las listas que aparecen anteriormente en este documento contienen las URL con las que actualmente se comunica Azure File Sync. Los firewalls deben poder permitir el tráfico de salida a estos dominios. Microsoft se esfuerza por mantener esta lista actualizada.

Configurar reglas de firewall que restrinjan dominios puede ser una medida para mejorar la seguridad. Si se usan estas configuraciones de firewall, hay que tener en cuenta que se agregarán direcciones URL que incluso podrían cambiar con el tiempo. Compruebe periódicamente este artículo.

## <a name="next-steps"></a>Pasos siguientes
- [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
- [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
- [Supervisión de Azure File Sync](storage-sync-files-monitoring.md)
