---
title: Seguridad en Azure Database for PostgreSQL con un único servidor
description: Información general sobre las características de seguridad de Azure Database for PostgreSQL con un único servidor.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: a1bd9b8cbcbc785425c2d1870dc555ff91f695f7
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484801"
---
# <a name="security-in-azure-database-for-postgresql---single-server"></a>Seguridad en Azure Database for PostgreSQL con un único servidor

Existen varios niveles de seguridad disponibles para proteger los datos en el servidor de Azure Database for PostgreSQL. En este artículo se describen esas opciones de seguridad.

## <a name="information-protection-and-encryption"></a>Protección y cifrado de información

### <a name="in-transit"></a>En tránsito
Azure Database for PostgreSQL protege los datos mediante el cifrado de datos en tránsito con Seguridad de la capa de transporte. El cifrado (SSL/TLS) se aplica de forma predeterminada.

### <a name="at-rest"></a>En reposo
El servicio Azure Database for PostgreSQL usa el módulo criptográfico con validación FIPS 140-2 para el cifrado del almacenamiento de los datos en reposo. Los datos (copias de seguridad incluidas) se cifran en el disco, a excepción de los archivos temporales creados por el motor durante la ejecución de consultas. El servicio usa el cifrado AES de 256 bits que se incluye en el cifrado de almacenamiento de Azure y las claves las administra el sistema. El cifrado de almacenamiento siempre está activado y no se puede deshabilitar.


## <a name="network-security"></a>Seguridad de las redes
Las conexiones a un servidor de Azure Database for PostgreSQL se enrutan primero a través de una puerta de enlace regional. La puerta de enlace tiene una dirección IP accesible públicamente, mientras que las direcciones IP del servidor están protegidas. Para más información sobre la puerta de enlace, vea el [artículo sobre arquitectura de conectividad](concepts-connectivity-architecture.md).  

Un servidor de Azure Database for PostgreSQL recién creado tiene un firewall que bloquea todas las conexiones externas. Aunque lleguen a la puerta de enlace, no tienen permiso para conectarse al servidor. 

### <a name="ip-firewall-rules"></a>Reglas de firewall de IP
Las reglas de firewall de IP otorgan acceso a los servidores según la dirección IP de origen de cada solicitud. Vea la [información general sobre las reglas de firewall](concepts-firewall-rules.md) para más información.

### <a name="virtual-network-firewall-rules"></a>Reglas de firewall de red virtual
Los puntos de conexión de servicio de red virtual amplían la conectividad de la red virtual a través de la red troncal de Azure. Cuando se usan reglas de red virtual, el servidor de Azure Database for PostgreSQL se puede habilitar para permitir conexiones desde subredes seleccionadas en una red virtual. Para más información, vea la [información general sobre los puntos de conexión de servicio de red virtual](concepts-data-access-and-security-vnet.md).


## <a name="access-management"></a>administración de acceso

Al crear el servidor de Azure Database for PostgreSQL, se deben proporcionar las credenciales de un rol de usuario administrador. Este rol de administrador se puede usar para crear más [roles de MySQL](https://www.postgresql.org/docs/current/user-manag.html).

También se puede conectar al servidor a través de la [autenticación de Azure Active Directory (AAD)](concepts-aad-authentication.md).


## <a name="threat-protection"></a>Protección contra amenazas

Puede optar por usar [Advanced Threat Protection](concepts-data-access-and-security-threat-protection.md), que detecta actividades anómalas que indican intentos poco habituales y posiblemente dañinos de acceder a sus servidores o de aprovechar sus vulnerabilidades.

Hay un [registro de auditoría](concepts-audit.md) disponible para realizar un seguimiento de la actividad en las bases de datos. 


## <a name="next-steps"></a>Pasos siguientes
- Habilite reglas de firewall de [direcciones IP](concepts-firewall-rules.md) o [redes virtuales](concepts-data-access-and-security-vnet.md).
- Obtenga más información sobre la [autenticación de Azure Active Directory](concepts-aad-authentication.md) en Azure Database for PostgreSQL.
