---
title: 'Firewall de las tablas de Azure - solución de VMware mediante CloudSimple:'
description: Obtenga información sobre las tablas de firewall de la nube privada CloudSimple y reglas de firewall.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 861c2e86d623c46c14366f19457d1f689386a316
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577351"
---
# <a name="firewall-tables-overview"></a>Información general de tablas de Firewall

Una tabla de firewall enumera las reglas para filtrar el tráfico de red hacia y desde recursos de nube privada. Se puede aplicar a una subred o VLAN. Las reglas, a continuación, controlan el tráfico de red entre una red de origen o la dirección IP y una red de destino o dirección IP.

## <a name="firewall-rules"></a>Reglas de firewall

En la tabla siguiente se describe los parámetros en una regla de firewall.

| Propiedad | Detalles |
| ---------| --------|
| **Nombre** | Un nombre que identifica de forma única la regla de firewall y su finalidad. |
| **Prioridad** | Un número entre 100 y 4096, con la prioridad más alta de 100. Las reglas se procesan en el orden de prioridad. Cuando trata de tráfico a través de una coincidencia de regla, se detiene el procesamiento de la regla. Como resultado, no se procesan las reglas que existen con prioridades inferiores que tienen los mismos atributos que las reglas con prioridades más altas.  Tenga cuidado para evitar reglas en conflicto. |
| **Seguimiento de estado** | Seguimiento puede ser sin estado (nube privada, Internet o VPN) o con estado (IP pública).  |
| **Protocolo** | Algunas opciones son cualquiera, TCP o UDP. Si necesita usarlo, puede usar cualquiera. |
| **Dirección** | Si la regla se aplica al tráfico entrante o al saliente. |
| **Acción** | Permitir o denegar para el tipo de tráfico definido en la regla. |
| **Origen** | Una dirección IP, bloque de enrutamiento de interdominios sin clases (CIDR) (por ejemplo, 10.0.0.0/24) o ninguno.  Especificar un intervalo, una etiqueta de servicio o grupo de seguridad de la aplicación le permite crear menos reglas de seguridad. |
| **Puerto de origen** | Puerto de red que el tráfico se origina.  Puede especificar un puerto individual o un intervalo de puertos, por ejemplo, 443 o 8000-8080. La especificación de intervalos le permite crear menos reglas de seguridad. |
| **Destino** | Una dirección IP, bloque de enrutamiento de interdominios sin clases (CIDR) (por ejemplo, 10.0.0.0/24) o ninguno.  Especificar un intervalo, una etiqueta de servicio o grupo de seguridad de la aplicación le permite crear menos reglas de seguridad.  |
| **Puerto de destino** | Puerto a la que fluye el tráfico de red.  Puede especificar un puerto individual o un intervalo de puertos, por ejemplo, 443 o 8000-8080. La especificación de intervalos le permite crear menos reglas de seguridad.|

### <a name="stateless"></a>Sin estado

Una regla sin estado solo examina los paquetes individuales y las filtra basándose en la regla.  
Reglas adicionales pueden ser necesarias para el flujo de tráfico en la dirección inversa.  Usar reglas de estado para el tráfico entre los siguientes puntos:

* Subredes de las nubes privadas
* Subred local y una nube privada
* Tráfico de Internet de las nubes privadas

### <a name="stateful"></a>Con estado

 Una regla con estado es consciente de las conexiones que lo atraviesan. Se crea un registro de flujo para las conexiones existentes. Se permite o deniega la comunicación en función del estado de conexión del registro de flujo.  Use este tipo de regla para las direcciones IP públicas para filtrar el tráfico de Internet.

### <a name="default-rules"></a>Reglas predeterminadas

Siguientes reglas predeterminadas se crean en cada tabla de firewall.

|Prioridad|NOMBRE|Seguimiento de estado|Dirección|Tipo de tráfico|Protocol|Origen|Puerto de origen|Destino|Puerto de destino|.|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|allow-all-to-internet|Con estado|Salida|Tráfico de internet o IP público|Todo|Cualquiera|Cualquiera|Cualquiera|Cualquiera|PERMITIR|
|65001|deny-all-from-internet|Con estado|Entrada|Tráfico de internet o IP público|Todo|Cualquiera|Cualquiera|Cualquiera|Cualquiera|Denegar|
|65002|allow-all-to-intranet|Sin estado|Salida|Nube privada interna o tráfico VPN|Todo|Cualquiera|Cualquiera|Cualquiera|Cualquiera|PERMITIR|
|65003|allow-all-from-intranet|Sin estado|Entrada|Nube privada interna o tráfico VPN|Todo|Cualquiera|Cualquiera|Cualquiera|Cualquiera|PERMITIR|

## <a name="next-steps"></a>Pasos siguientes

* [Configurar las reglas y las tablas de firewall](https://docs.azure.cloudsimple.com/firewall/)