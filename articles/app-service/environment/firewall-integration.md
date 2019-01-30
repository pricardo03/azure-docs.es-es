---
title: 'Bloqueo del tráfico de salida de Environment: Azure'
description: Se describe cómo realizar la integración con Azure Firewall para proteger el tráfico saliente.
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 87331ed0d9e5a4ff51e3669390d1b40dea58574a
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389245"
---
# <a name="locking-down-an-app-service-environment"></a>Bloqueo de una instancia de App Service Environment

App Service aislado tiene varias dependencias externas a las que requiere acceso para que funcionen correctamente. El plan de App Service aislado reside en la red de Azure Virtual Network (VNet) del cliente. Los clientes deben permitir el tráfico de dependencia de App Service aislado, que es un problema para aquellos que desean bloquear todo el tráfico de salida de su red virtual.

Una instancia de App Service aislado tiene una serie de dependencias de entrada. El tráfico entrante de administración no se puede enviar a través de un dispositivo de firewall. Las direcciones de origen de este tráfico se conocen y están publicadas en el documento [Direcciones de administración de App Service aislado](https://docs.microsoft.com/azure/app-service/environment/management-addresses). Puede crear Reglas de grupo de seguridad de red con esa información para proteger el tráfico entrante.

Las dependencias de salida de App Service aislado se definen casi por completo con los FQDN, que no tienen direcciones estáticas tras ellos. La falta de direcciones estáticas significa que no se pueden usar grupos de seguridad de red (NSG) para bloquear el tráfico saliente de una instancia de App Service aislado. Las direcciones cambian con tal frecuencia que no se pueden configurar reglas en función de la resolución actual ni usarlas para crear grupos de seguridad de red. 

La solución para proteger las direcciones de salida se encuentra en el uso de un dispositivo de firewall que puede controlar el tráfico saliente basándose en los nombres de dominio. Azure Firewall puede restringir el tráfico saliente HTTP y HTTPS basándose en el FQDN de destino.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Configuración de Azure Firewall con App Service aislado 

A continuación, se indican los pasos para bloquear la salida de la instancia existente de App Service Environment con Azure Firewall:

1. Habilite puntos de conexión de servicio para SQL, Event Hub y Storage en la subred de ASE. Para ello, vaya al portal de red > subredes y seleccione Microsoft.EventHub, Microsoft.SQL y Microsoft.Storage en la lista desplegable de puntos de conexión de servicio. Cuando tenga puntos de conexión de servicio habilitados para Azure SQL, todas las dependencias de Azure SQL que tengan sus aplicaciones deben configurarse también como puntos de conexión de servicio. 

   ![selección de puntos de conexión de servicio][2]
  
1. Cree una subred denominada AzureFirewallSubnet en la red virtual donde exista su ASE. Siga las instrucciones de la [documentación de Azure Firewall](https://docs.microsoft.com/azure/firewall/) para crear su instancia de Azure Firewall.
1. En la interfaz de usuario de Azure Firewall, vaya a Reglas > Recopilación de reglas de aplicación y seleccione Agregar una colección de reglas de aplicación. Proporcione un nombre y una prioridad y establezca Permitir. En la sección de etiquetas FQDN, proporcione un nombre, establezca las direcciones de origen en * y seleccione la etiqueta FQDN de App Service Environment y la actualización de Windows. 
   
   ![Adición de regla de aplicación][1]
   
1. En la interfaz de usuario de Azure Firewall, vaya a Reglas > Recopilación de reglas de aplicación y seleccione Agregar una colección de reglas de aplicación. Proporcione un nombre y una prioridad y establezca Permitir. En la sección de reglas, proporcione un nombre, seleccione **Cualquiera**, establezca * como direcciones de origen y destino y defina los puertos como 123. Esta regla permite que el sistema realice la sincronización del reloj mediante NTP. Cree otra regla de la misma manera para el puerto 12000 para ayudar a evaluar los problemas del sistema.

   ![Adición de regla de red NTP][3]

1. Cree una tabla de rutas con las direcciones de administración de [Direcciones de administración de App Service aislado]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) con un próximo salto de Internet. Las entradas de la tabla de rutas son necesarias para evitar problemas de enrutamiento asimétrico. Agregue rutas a las dependencias de dirección IP que se indican a continuación en las dependencias de dirección IP con un próximo salto de Internet. Agregue una ruta de aplicación virtual a la tabla de rutas para 0.0.0.0/0 con el próximo salto como dirección IP privada de Azure Firewall. 

   ![Creación de una tabla de rutas][4]
   
1. Asigne la tabla de rutas que creó a la subred de ASE.

#### <a name="deploying-your-ase-behind-a-firewall"></a>Implementación de ASE tras un firewall

Los pasos para implementar su ASE tras un firewall son iguales que los pasos para configurar el ASE existente con una instancia de Azure Firewall, con la salvedad de que necesitará crear una subred de ASE y, a continuación, seguir los pasos anteriores. Para crear su ASE en una subred existente, deberá usar una plantilla de Resource Manager, como se describe en el documento sobre cómo [crear su ASE con una plantilla de Resource Manager](https://docs.microsoft.com/azure/app-service/environment/create-from-template).

## <a name="application-traffic"></a>Tráfico de la aplicación 

Los pasos anteriores permitirán que la instancia de App Service aislado funcione sin problemas. Deberá configurar todo para adaptarse a las necesidades de la aplicación. Hay dos problemas para las aplicaciones de una instancia de App Service aislado configurada con Azure Firewall.  

- Las dependencias de la aplicación deben agregarse a Azure Firewall o a la tabla de rutas. 
- Se deben crear rutas para el tráfico de la aplicación para evitar problemas de enrutamiento asimétrico.

Si las aplicaciones tienen dependencias, deben agregarse a Azure Firewall. Crear reglas de aplicación para permitir el tráfico HTTP/HTTPS y reglas de red para todo lo demás. 

Si conoce el intervalo de direcciones del que provendrá el tráfico de la solicitud de la aplicación, puede agregarlo a la tabla de rutas asignada a la subred de App Service aislado. Si el intervalo de direcciones es grande o no se especifica, puede usar un dispositivo de red, como Application Gateway, para proporcionarle una dirección para agregar a la tabla de rutas. Para obtener más información sobre cómo configurar una instancia de Application Gateway con su instancia de App Service aislado con ILB, lea [Integración de App Service aislado con ILB con Azure Application Gateway](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)

![ASE con flujo de conexión de Azure Firewall][5]

Este uso de Application Gateway es solo un ejemplo de cómo configurar el sistema. Si ha seguido este procedimiento, deberá agregar una ruta a la tabla de rutas de subred de ASE para que el tráfico de respuesta enviado a Application Gateway vaya allí directamente. 

## <a name="logging"></a>Registro 

Azure Firewall puede enviar registros a Azure Storage, Event Hub o Log Analytics. Para integrar la aplicación con cualquier destino admitido, vaya al portal de Azure Firewall > Registros de diagnóstico y habilite los registros para el destino deseado. Si integra con Log Analytics, podrá ver el registro de todo el tráfico enviado a Azure Firewall. Para ver el tráfico que se va a denegar, abra el portal de Log Analytics > Registros y escriba una consulta como: 

    AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
 
La integración de Azure Firewall con Log Analytics resulta muy útil la primera vez que se pone una aplicación en funcionamiento si no conoce todas las dependencias de la aplicación. Puede obtener más información acerca de Log Analytics en [Análisis de los datos de Log Analytics en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).
 
## <a name="dependencies"></a>Dependencias

La siguiente información solo es necesaria si desea configurar un dispositivo de firewall distinto a Azure Firewall. 

- Los servicios compatibles con puntos de conexión de servicio deben configurarse con puntos de conexión de servicio.
- Las dependencias de dirección IP son para tráfico que no sea HTTP/HTTPS.
- Los puntos de conexión HTTP/HTTPS de FQDN se pueden colocar en el dispositivo de firewall.
- Los puntos de conexión HTTP/HTTPS de carácter comodín son dependencias que pueden variar con App Service aislado en función del número de calificadores. 
- Las dependencias de Linux solo son un problema si va a implementar aplicaciones de Linux en App Service aislado. No es necesario agregar estas direcciones al firewall si no va a implementar aplicaciones de Linux en App Service aislado. 


#### <a name="service-endpoint-capable-dependencies"></a>Dependencias compatibles con los puntos de conexión de servicio 

| Punto de conexión |
|----------|
| Azure SQL |
| Azure Storage |
| Centro de eventos de Azure |

#### <a name="ip-address-dependencies"></a>Dependencias de dirección IP

| Punto de conexión | Detalles |
|----------| ----- |
| \*:123 | Comprobación de reloj NTP. El tráfico se comprueba en varios puntos de conexión en el puerto 123. |
| \*:12000 | Este puerto se usa para algunas tareas de supervisión del sistema. Si se bloquea, algunos problemas serán más difíciles de evaluar, pero el ASE seguirá funcionando. |

Con una instancia de Azure Firewall, se configurará automáticamente todo lo siguiente con etiquetas FQDN. 

#### <a name="fqdn-httphttps-dependencies"></a>Dependencias HTTP/HTTPS de FQDN 

| Punto de conexión |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|login.windows.com:443 |
|login.windows.net:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.thawte.com:443 |
|crl3.digicert.com:80 |
|ocsp.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|crl.verisign.com:80 |
|ocsp.verisign.com:80 |
|cacerts.digicert.com:80 |
|azperfcounters1.blob.core.windows.net:443 |
|azurewatsonanalysis-prod.core.windows.net:443 |
|global.metrics.nsatc.net:80   |
|az-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.net:443 |
|azglobal black.azglobal.metrics.nsatc.net:443 |
|azglobal-red.azglobal.metrics.nsatc.net:443 |
|antares-black.antares.metrics.nsatc.net:443 |
|antares-red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|management.core.windows.net:443 |
|management.core.windows.net:80 |
|management.azure.com:443 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|validation-v2.sls.microsoft.com:443 |
|flighting.cp.wd.microsoft.com:443 |
|dmd.metaservices.microsoft.com:80 |
|admin.core.windows.net:443 |
|azureprofileruploads.blob.core.windows.net:443 |
|azureprofileruploads2.blob.core.windows.net:443 |
|azureprofileruploads3.blob.core.windows.net:443 |
|azureprofileruploads4.blob.core.windows.net:443 |
|azureprofileruploads5.blob.core.windows.net:443 |

#### <a name="wildcard-httphttps-dependencies"></a>Dependencias HTTP/HTTPS de carácter comodín 

| Punto de conexión |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*.update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*.servicebus.windows.net:443 |
|grmdsprod\*lini\*.servicebus.windows.net:443 |
|grsecprod\*mini\*.servicebus.windows.net:443 |
|grsecprod\*lini\*.servicebus.windows.net:443 |
|graudprod\*mini\*.servicebus.windows.net:443 |
|graudprod\*lini\*.servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Dependencias de Linux 

| Punto de conexión |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|registry-1.docker.io:443 |
|auth.docker.io:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.mono-project.com:80 |
|packages.treasuredata.com:80|
|security.ubuntu.com:80 |

<!--Image references-->
[1]: ./media/firewall-integration/firewall-apprule.png
[2]: ./media/firewall-integration/firewall-serviceendpoints.png
[3]: ./media/firewall-integration/firewall-ntprule.png
[4]: ./media/firewall-integration/firewall-routetable.png
[5]: ./media/firewall-integration/firewall-topology.png
