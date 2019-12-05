---
title: Seguridad en Azure Database for MySQL con un único servidor
description: Información general sobre las características de seguridad de Azure Database for MySQL con un único servidor.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: 06f895d2e8cd7936141c9f35c5ed0c81fb7e3e15
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484793"
---
# <a name="security-in-azure-database-for-mysql---single-server"></a>Seguridad en Azure Database for MySQL con un único servidor

Existen varios niveles de seguridad disponibles para proteger los datos en el servidor de Azure Database for MySQL. En este artículo se describen esas opciones de seguridad.

## <a name="information-protection-and-encryption"></a>Protección y cifrado de información

### <a name="in-transit"></a>En tránsito
Azure Database for MySQL protege los datos mediante el cifrado de datos en tránsito con Seguridad de la capa de transporte. El cifrado (SSL/TLS) se aplica de forma predeterminada.

### <a name="at-rest"></a>En reposo
El servicio Azure Database for MySQL usa el módulo criptográfico con validación FIPS 140-2 para el cifrado del almacenamiento de los datos en reposo. Los datos (copias de seguridad incluidas) se cifran en el disco, a excepción de los archivos temporales creados por el motor durante la ejecución de consultas. El servicio usa el cifrado AES de 256 bits que se incluye en el cifrado de almacenamiento de Azure y las claves las administra el sistema. El cifrado de almacenamiento siempre está activado y no se puede deshabilitar.


## <a name="network-security"></a>Seguridad de las redes
Las conexiones a un servidor de Azure Database for MySQL se enrutan primero a través de una puerta de enlace regional. La puerta de enlace tiene una dirección IP accesible públicamente, mientras que las direcciones IP del servidor están protegidas. Para más información sobre la puerta de enlace, vea el [artículo sobre arquitectura de conectividad](concepts-connectivity-architecture.md).  

Un servidor de Azure Database for MySQL recién creado tiene un firewall que bloquea todas las conexiones externas. Aunque lleguen a la puerta de enlace, no tienen permiso para conectarse al servidor. 

### <a name="ip-firewall-rules"></a>Reglas de firewall de IP
Las reglas de firewall de IP otorgan acceso a los servidores según la dirección IP de origen de cada solicitud. Vea la [información general sobre las reglas de firewall](concepts-firewall-rules.md) para más información.

### <a name="virtual-network-firewall-rules"></a>Reglas de firewall de red virtual
Los puntos de conexión de servicio de red virtual amplían la conectividad de la red virtual a través de la red troncal de Azure. Cuando se usan reglas de red virtual, el servidor de Azure Database for MySQL se puede habilitar para permitir conexiones desde subredes seleccionadas en una red virtual. Para más información, vea la [información general sobre los puntos de conexión de servicio de red virtual](concepts-data-access-and-security-vnet.md).


## <a name="access-management"></a>administración de acceso

Al crear el servidor de Azure Database for MySQL, se deben proporcionar las credenciales de un usuario administrador. Este administrador se puede usar para crear más usuarios de MySQL.


## <a name="threat-protection"></a>Protección contra amenazas

Puede optar por usar [Advanced Threat Protection](concepts-data-access-and-security-threat-protection.md), que detecta actividades anómalas que indican intentos poco habituales y posiblemente dañinos de acceder a sus servidores o de aprovechar sus vulnerabilidades.

Hay un [registro de auditoría](concepts-audit-logs.md) disponible para realizar un seguimiento de la actividad en las bases de datos. 


## <a name="next-steps"></a>Pasos siguientes
- Habilite reglas de firewall de [direcciones IP](concepts-firewall-rules.md) o [redes virtuales](concepts-data-access-and-security-vnet.md).