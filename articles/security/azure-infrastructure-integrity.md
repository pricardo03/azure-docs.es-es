---
title: Integridad de la infraestructura de Azure
description: En este artículo se aborda la integridad de la infraestructura de Azure.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 867bc66a68bec662153d8336e649cf46df02f101
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901323"
---
# <a name="azure-infrastructure-integrity"></a>Integridad de la infraestructura de Azure

## <a name="software-installation"></a>Instalación de software
Todos los componentes de la pila de software que se instalan en el entorno de Azure se crean de forma personalizada siguiendo el proceso Ciclo de vida de desarrollo de seguridad (SDL) de Microsoft. Todos los componentes de software (incluidas las imágenes del sistema operativo y SQL Database) se implementan como parte del proceso de administración de versiones y de cambios. El sistema operativo que se ejecuta en todos los nodos es una versión personalizada de Windows Server 2008 o Windows Server 2012. La versión exacta la elige el controlador de tejido de acuerdo con el rol que pretende que desempeñe el sistema operativo. Además, el sistema operativo del host no permite la instalación de ningún componente de software no autorizado.

Algunos componentes de Microsoft Azure (por ejemplo, RDFE o el portal para desarrolladores) se implementan como clientes de Azure en la máquina virtual invitada que se ejecuta en el sistema operativo invitado.

## <a name="virus-scans-on-builds"></a>Análisis de virus en las compilaciones
Las compilaciones de los componentes de software de Azure (incluido el sistema operativo) tienen que pasar por un análisis de virus mediante la herramienta antivirus Microsoft Endpoint Protection (MEP). Cada análisis de virus creará un registro dentro del directorio de compilación asociado, detallando lo que se examinó y los resultados del examen. El análisis de virus forma parte del código fuente de la compilación de todos los componentes de Azure. El código no pasara a producción sin tener un análisis de virus limpio y correcto. Si se detecta algún problema, la compilación se bloquea y, después, se envía a los equipos de seguridad de Microsoft Security para identificar dónde entró el código "falso" en la compilación.

## <a name="closedlocked-environment"></a>Entorno cerrado o bloqueado
De forma predeterminada, los nodos de infraestructura Azure y las máquinas virtuales invitadas no tienen ninguna cuenta de usuario creada en ellas. Además, las cuentas de administrador predeterminadas de Windows también están deshabilitadas. Los administradores de Microsoft Azure Live Support (WALS) pueden, con la autenticación adecuada, iniciar sesión en estas máquinas y administrar la red de producción de Azure para reparaciones de emergencia.

## <a name="microsoft-azure-sql-database-authentication"></a>Autenticación de Microsoft Azure SQL Database
Al igual que con cualquier implementación de SQL Server, la administración de cuentas de usuario debe estar estrictamente controlada. Microsoft Azure SQL Database solo admite la autenticación de SQL Server. Las cuentas de usuario con contraseñas seguras y configuradas con derechos específicos deben utilizarse también para complementar el modelo de seguridad de datos del cliente.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACL entre MSFT CorpNet y el clúster de Microsoft Azure
Las ACL/Firewall entre la plataforma de servicios y la red corporativa de MS protegen Microsoft Azure SQL Database de accesos no autorizados. Además, solo los usuarios de los intervalos de direcciones IP de Microsoft CorpNet pueden acceder al punto de conexión de administración de la plataforma WinFabric.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACL entre los nodos de un clúster de Azure SQL DB
Como protección adicional, como parte de la estrategia de defensa en profundidad, se han implementado ACL/Firewall entre los nodos de un clúster de Microsoft Azure SQL DB. Toda la comunicación dentro del clúster de plataformas WinFabric, así como todo el código en ejecución, es de confianza.

## <a name="custom-mas-watchdogs"></a>Agentes de administración personalizados (guardianes)
Microsoft Azure SQL Database emplea agentes de administración personalizados denominados guardianes para supervisar el estado del clúster de Microsoft Azure SQL DB.

## <a name="web-protocols"></a>Protocolos web

### <a name="role-instance-monitoring-and-restart"></a>Supervisión y reinicio de instancias de rol
Azure se asegura de que todos los roles en ejecución (web orientada a Internet, o roles de trabajo de procesamiento de back-end) implementados se sometan a una supervisión sostenida de su estado para garantizar que están prestando de manera efectiva y eficiente los servicios en los que han sido aprovisionados. En el caso de que un rol se vuelva incorrecto, ya sea por un error crítico en la aplicación hospedada o por un problema de configuración subyacente dentro de la propia instancia del rol, el controlador de tejido de Microsoft Azure detectará el problema dentro de la instancia de rol e iniciará un estado de corrección.

### <a name="compute-connectivity"></a>Conectividad de proceso
Azure garantiza que la aplicación o el servicio implementado sea accesible a través de protocolos estándar basados en web. Las instancias virtuales de los roles web orientados a Internet tendrán conectividad externa a Internet y los usuarios de la web podrán acceder a ellas directamente. Las instancias virtuales de rol de trabajo de procesamiento de back-end tienen conectividad externa a Internet, pero el usuario web externo no puede acceder directamente a ellas, con el fin de proteger la confidencialidad e integridad de las operaciones que los roles de trabajo realizan en nombre de las instancias virtuales del rol web de acceso público.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](azure-physical-security.md)
- [Disponibilidad de la infraestructura de Azure](azure-infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)
