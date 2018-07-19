---
title: Características de seguridad de Azure SQL Database
description: En este artículo se proporciona una descripción general de Azure SQL Database para proteger los datos de los clientes en Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: cca8febb004029b13b0df09a047da701c4528e8e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102036"
---
# <a name="microsoft-azure-sql-database-security-features"></a>Características de seguridad de Microsoft Azure SQL Database    
Microsoft Azure SQL Database ofrece un servicio de base de datos relacional en Azure. Para proteger los datos de los clientes y proporcionar características sólidas de seguridad que los clientes esperan de un servicio de base de datos relacional, SQL Database tiene sus propios conjuntos de funcionalidades de seguridad. Estas funcionalidades se basan en los controles que se heredan de Azure.

## <a name="security-capabilities"></a>Funcionalidades de seguridad

### <a name="usage-of-tabular-data-stream-tds-protocol"></a>Uso del protocolo de flujo de datos tabulares (TDS)
Microsoft Azure SQL Database solo admite el protocolo TDS, que requiere que la base de datos sea accesible solo a través del puerto predeterminado de TCP/1433.

### <a name="microsoft-azure-sql-database-firewall"></a>Firewall de Microsoft Azure SQL Database
Para ayudar a proteger los datos de los clientes, Microsoft Azure SQL Database incluye una funcionalidad de firewall que impide de forma predeterminada todo acceso al servidor de SQL Database, como se muestra a continuación.

![Firewall de Azure SQL Database][1]

El firewall de la puerta de enlace proporciona la capacidad de limitar las direcciones, lo que permite un control granular para que los clientes especifiquen los rangos de direcciones IP aceptables. El firewall otorga acceso según la dirección IP de origen de cada solicitud.

La configuración del firewall puede lograrse mediante un Portal de administración o mediante programación con la API de REST de administración de Microsoft Azure SQL Database. De forma predeterminada, el firewall de la puerta de enlace de Microsoft Azure SQL Database impide todo el acceso de TDS de los clientes a Microsoft Azure SQL Database. El acceso debe configurarse mediante ACL para permitir conexiones de Microsoft Azure SQL Database por números de puerto, protocolos y direcciones de Internet de origen y destino.

### <a name="dosguard"></a>DoSGuard
Los ataques de denegación de servicio (DoS) se reducen mediante un servicio de puerta de enlace de SQL Database denominado DoSGuard. DoSGuard realiza un seguimiento activo de inicios de sesión erróneos desde direcciones IP. Si hay varios inicios de sesión erróneos desde una dirección IP específica dentro de un período, se bloquea la dirección IP y no podrá acceder a ningún recurso en el servicio durante un período definido previamente.

Además de esto, la puerta de enlace de Microsoft Azure SQL Database también lleva a cabo:

- Negociaciones de capacidad de canal seguro para implementar conexiones cifradas validadas por TDS FIPS 140-2 al conectarse a los servidores de bases de datos.
- Inspección de paquetes TDS con estado al aceptar conexiones de los clientes. La puerta de enlace valida la información de conexión y pasa los paquetes TDS al servidor físico adecuado según el nombre de la base de datos especificado en la cadena de conexión.

Es el principio general de seguridad de red de la oferta de Microsoft Azure SQL Database es solo permitir la conexión y la comunicación que sean necesarias para permitir que el servicio funcione. Todos los otros puertos, protocolos y conexiones se bloquean de manera predeterminada. Se utilizan VLAN y ACL para restringir las comunicaciones de red mediante redes de origen y destino, protocolos y números de puerto.

Los mecanismos aprobados para implementar ACL basadas en red incluyen: ACL en enrutadores y equilibradores de carga. Estos se administran mediante reglas de firewall de puerta de enlace de Microsoft Azure SQL Database, que están configuradas por el cliente, firewall de VM invitada y redes de Azure.

## <a name="data-segregation-and-customer-isolation"></a>Segregación de datos y aislamiento de clientes
La red de producción de Azure está estructurada de modo que los componentes del sistema accesibles públicamente estén separados de los recursos internos. Existen límites físicos y lógicos entre los servidores web, lo que proporciona acceso a Azure Portal orientado al público y la infraestructura virtual subyacente de Azure, donde residen las instancias de las aplicaciones de los clientes y los datos de los clientes.

Toda la información accesible públicamente se administra dentro de la red de producción de Azure. La red de producción está sujeta a la autenticación en dos fases y a los mecanismos de protección de límites, usa el firewall y el conjunto de características de seguridad descritos en la sección anterior, y utiliza las funciones de aislamiento de datos, como se indica a continuación.

### <a name="unauthorized-systems-and-isolation-of-fc"></a>Sistemas no autorizados y aislamiento de FC
Puesto que el controlador de tejido (FC) es el orquestador central del tejido de Microsoft Azure, existen controles importantes para mitigar las amenazas, sobre todo desde FA potencialmente comprometidos dentro de las aplicaciones de los clientes. FC no reconoce ningún hardware cuya información de dispositivo (por ejemplo, la dirección MAC) no esté cargada previamente en FC. Los servidores DHCP en FC tienen listas configuradas de direcciones MAC de los nodos que están dispuestas a arrancar. Incluso si se conectan sistemas no autorizados, no se incorporan al inventario del tejido y, por lo tanto, no se conectan ni están autorizados para comunicarse con ningún sistema en el inventario del tejido. Esto reduce el riesgo de que sistemas no autorizados se comuniquen con el FC y obtengan acceso a la VLAN y Azure.

### <a name="vlan-isolation"></a>Aislamiento de VLAN
La red de producción de Azure se divide lógicamente en tres redes VLAN principales:

- La red VLAN principal: interconecta nodos de cliente que no son de confianza.
- La red VLAN FC: contiene controladores de tejido (FC) de confianza y sistemas auxiliares
- La red VLAN de dispositivo: contiene dispositivos de red y otra infraestructura de confianza

### <a name="packet-filtering"></a>Filtrado de paquetes
El IPFilter y los firewalls de software implementados en el sistema operativo raíz y el sistema operativo invitado de los nodos aplican restricciones de conectividad y evitan el tráfico no autorizado entre las VM.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hipervisor, sistema operativo raíz y VM invitadas
El hipervisor y el sistema operativo raíz administran el aislamiento del sistema operativo raíz de las VM invitadas y de las VM invitadas entre sí.

### <a name="types-of-rules-on-firewalls"></a>Tipos de reglas en firewalls
Una regla se define como:

{IP de Security Response Center (Src), puerto de Src, IP de destino, puerto de destino, protocolo de destino, entrada/salida, con estado/sin estado, tiempo de espera de flujo con estado}.

La entrada y salida de paquetes SYN solo se permiten si lo permite alguna de las reglas. Para TCP, Microsoft Azure usa reglas sin estado, donde el principio es que solo permite que todos los paquetes que no sean SYN entren o salgan de la VM. La premisa de seguridad es que toda pila de host sea resistente de omitir un paquete que no sea SYN si no ha visto anteriormente un paquete SYN. El propio protocolo TCP es con estado y, en combinación con la regla sin estado basada en SYN, logra un comportamiento general de una implementación con estado.

Para el Protocolo de datagramas de usuario (UDP), Microsoft Azure usa una regla con estado. Cada vez que un paquete UDP coincide con una regla, se crea un flujo inverso en la otra dirección. Este flujo tiene un tiempo de espera integrado.

Los clientes son responsables de configurar sus propios firewalls además de lo que ofrece Microsoft Azure. Aquí los clientes pueden definir las reglas para el tráfico entrante y saliente.

### <a name="production-configuration-management"></a>Administración de la configuración de producción
Los equipos de operaciones respectivos en Azure y Microsoft Azure SQL Database mantienen configuraciones de seguridad estándar. Todos los cambios de configuración en los sistemas de producción se documentan y registran a través de un sistema de seguimiento central. Los cambios de hardware y software se registran a través del sistema de seguimiento central. Los cambios de red relacionados con ACL se registran mediante un servicio de administración de ACL (AMS).

Todos los cambios de configuración en Microsoft Azure se desarrollan y prueban en el entorno de ensayo y, más adelante, se implementan en el entorno de producción. Las compilaciones de software se revisan como parte de las pruebas. Las comprobaciones de seguridad y privacidad se revisan como parte de los criterios de la lista de comprobación de entrada. El equipo de implementación correspondiente implementa los cambios en intervalos programados. El personal del equipo de implementación correspondiente revisa y aprueba las versiones antes de implementarlas en producción.

Los cambios se supervisan para comprobar que sean correctos. En un escenario de error, el cambio se revierte a su estado anterior o se implementa una revisión para corregir el error con la aprobación del personal designado. Source Depot, Git, TFS, MDS, Runners, supervisión de seguridad de Azure (ASM), FC y la plataforma de WinFabric se utilizan para administrar, aplicar y comprobar de manera centralizada las opciones de configuración en el entorno virtual de Azure.

De forma similar, los cambios de hardware y de red cuentan con pasos de validación establecidos para evaluar el cumplimiento de los requisitos de compilación. Las versiones se revisan y autorizan a través de un comité de evaluación de cambios (CAB) coordinado de los respectivos grupos a través de la pila.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Azure facilities, premises, and physical security](azure-physical-security.md) (Instalaciones de Azure, locales y seguridad física)
- [Availability of Azure infrastructure](azure-infrastructure-availability.md) (Disponibilidad de la infraestructura de Azure)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure](azure-infrastructure-integrity.md) (Integridad de la infraestructura de Azure)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
