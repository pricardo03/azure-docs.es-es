---
title: Bloqueo del tráfico saliente de App Service aislado
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
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: 5f2dd31488ae61bec061a81986a208bd328bf39b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093627"
---
# <a name="locking-down-an-app-service-environment"></a>Bloqueo de una instancia de App Service Environment

App Service aislado tiene varias dependencias externas a las que requiere acceso para que funcionen correctamente. El plan de App Service aislado reside en la red de Azure Virtual Network (VNet) del cliente. Los clientes deben permitir el tráfico de dependencia de App Service aislado, que es un problema para aquellos que desean bloquear todo el tráfico de salida de su red virtual.

Una instancia de App Service aislado tiene una serie de dependencias de entrada. El tráfico entrante de administración no se puede enviar a través de un dispositivo de firewall. Las direcciones de origen de este tráfico se conocen y están publicadas en el documento [Direcciones de administración de App Service aislado](https://docs.microsoft.com/azure/app-service/environment/management-addresses). Puede crear Reglas de grupo de seguridad de red con esa información para proteger el tráfico entrante.

Las dependencias de salida de App Service aislado se definen casi por completo con los FQDN, que no tienen direcciones estáticas tras ellos. La falta de direcciones estáticas significa que no se pueden usar grupos de seguridad de red (NSG) para bloquear el tráfico saliente de una instancia de App Service aislado. Las direcciones cambian con tal frecuencia que no se pueden configurar reglas en función de la resolución actual ni usarlas para crear grupos de seguridad de red. 

La solución para proteger las direcciones de salida se encuentra en el uso de un dispositivo de firewall que puede controlar el tráfico saliente basándose en los nombres de dominio. El equipo de redes de Azure ha puesto un nuevo dispositivo de red en versión preliminar denominado Azure Firewall. Azure Firewall es capaz de restringir el tráfico HTTP y HTTPS de salida en función del nombre DNS del destino.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Configuración de Azure Firewall con App Service aislado 

A continuación, se indican los pasos para bloquear la salida de App Service aislado con Azure Firewall:

1. Cree una instancia de Azure Firewall en la red virtual en la que se encuentra, o se encontrará, App Service aislado. [Documentación sobre Azure Firewall](https://docs.microsoft.com/azure/firewall/)
2. En la interfaz de usuario de Azure Firewall, seleccione la etiqueta de FQDN de App Service aislado.
3. Cree una tabla de rutas con las direcciones de administración de [Direcciones de administración de App Service aislado]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) con un próximo salto de Internet. Las entradas de la tabla de rutas son necesarias para evitar problemas de enrutamiento asimétrico. 
4. Agregue rutas a las dependencias de dirección IP que se indican a continuación en las dependencias de dirección IP con un próximo salto de Internet. 
5. Agregue una ruta a la tabla de rutas para 0.0.0.0/0 con el próximo salto como dispositivo de red de Azure Firewall.
6. Cree puntos de conexión de servicio para la subred de App Service aislado en Azure SQL y Azure Storage.
7. Asigne la tabla de rutas que creó a la subred de App Service aislado.  

## <a name="application-traffic"></a>Tráfico de la aplicación 

Los pasos anteriores permitirán que la instancia de App Service aislado funcione sin problemas. Deberá configurar todo para adaptarse a las necesidades de la aplicación. Hay dos problemas para las aplicaciones de una instancia de App Service aislado configurada con Azure Firewall.  

- Los FQDN de la dependencia de la aplicación deben agregarse a Azure Firewall o a la tabla de rutas.
- Se deben crear rutas para las direcciones de las que procederá el tráfico para evitar problemas de enrutamiento asimétrico.

Si las aplicaciones tienen dependencias, deben agregarse a Azure Firewall. Crear reglas de aplicación para permitir el tráfico HTTP/HTTPS y reglas de red para todo lo demás. 

Si conoce el intervalo de direcciones del que provendrá el tráfico de la solicitud de la aplicación, puede agregarlo a la tabla de rutas asignada a la subred de App Service aislado. Si el intervalo de direcciones es grande o no se especifica, puede usar un dispositivo de red, como Application Gateway, para proporcionarle una dirección para agregar a la tabla de rutas. Para obtener más información sobre cómo configurar una instancia de Application Gateway con su instancia de App Service aislado con ILB, lea [Integración de App Service aislado con ILB con Azure Application Gateway](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)


## <a name="dependencies"></a>Dependencias

Azure App Service tiene una serie de dependencias externas. Se pueden dividir de forma categórica en diversas áreas principales:

- Los servicios compatibles con los puntos de conexión de servicio deben configurarse con dichos puntos de conexión si desea bloquear el tráfico de red saliente.
- Los puntos de conexión de dirección IP no se dirigen con un nombre de dominio. Esto puede ser un problema para los dispositivos de firewall que espera que todo el tráfico HTTPS use nombres de dominio. Los puntos de conexión de dirección IP deben agregarse a la tabla de rutas que se establece en la subred de App Service aislado.
- Los puntos de conexión HTTP/HTTPS de FQDN se pueden colocar en el dispositivo de firewall.
- Los puntos de conexión HTTP/HTTPS de carácter comodín son dependencias que pueden variar con App Service aislado en función del número de calificadores. 
- Las dependencias de Linux solo son un problema si va a implementar aplicaciones de Linux en App Service aislado. No es necesario agregar estas direcciones al firewall si no va a implementar aplicaciones de Linux en App Service aislado. 


#### <a name="service-endpoint-capable-dependencies"></a>Dependencias compatibles con los puntos de conexión de servicio 

| Punto de conexión |
|----------|
| Azure SQL |
| Azure Storage |
| Azure KeyVault |


#### <a name="ip-address-dependencies"></a>Dependencias de dirección IP 

| Punto de conexión |
|----------|
| 40.77.24.27:443 |
| 13.82.184.151:443 |
| 13.68.109.212:443 |
| 13.90.249.229:443 |
| 13.91.102.27:443 |
| 104.45.230.69:443 |
| 168.62.226.198:12000 |


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

