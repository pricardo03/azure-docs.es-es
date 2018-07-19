---
title: Componentes y límites del sistema de información de Azure
description: En este artículo se proporciona una descripción general de la arquitectura y administración de Microsoft Azure.
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
ms.openlocfilehash: 8db1dce5fcc56c229d1fdd746bafbd2fae2c9bad
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101966"
---
# <a name="azure-information-system-components-and-boundaries"></a>Componentes y límites del sistema de información de Azure
En este artículo se proporciona una descripción general de la arquitectura y administración de Microsoft Azure. El entorno del sistema de Azure se compone de las siguientes redes:

- Red de producción de Microsoft Azure (red de Azure)
- Red corporativa de Microsoft (red corporativa)

Los distintos equipos de TI son responsables de las operaciones y el mantenimiento de la red de Azure y las redes corporativas.

## <a name="azure-architecture"></a>Arquitectura de Azure
Microsoft Azure es una plataforma de informática en la nube y una infraestructura para compilar, implementar y administrar aplicaciones y servicios a través de una red de centros de datos administrados por Microsoft. En función del número de recursos especificados por los clientes, Azure crea VM basadas en la necesidad de recursos. Estas VM se ejecutan en un hipervisor de Microsoft Azure, que está diseñado para usar en la nube y no es accesible para el público.

En cada nodo de servidor físico de Azure, hay un hipervisor que se ejecuta directamente sobre hardware. El hipervisor divide un nodo en un número variable máquinas virtuales (VM) invitadas. Cada nodo tiene también una VM “raíz” especial, que ejecuta el sistema operativo host. El Firewall de Windows está habilitado en cada VM. Los únicos puertos abiertos y direccionables, interna o externamente, son puertos definidos explícitamente en el archivo de definición de servicio configurado por el cliente. El hipervisor y el sistema operativo raíz intervienen en todo el tráfico y el acceso al disco y a la red.

En la capa de host, las VM de Azure ejecutan una versión personalizada y protegida de Windows Server más reciente. Microsoft Azure usa una versión reducida de Windows Server que incluye solo los componentes necesarios para hospedar las VM. Esto sirve tanto para mejorar el rendimiento como para reducir la superficie expuesta a ataques. El hipervisor, que no depende de la seguridad del sistema operativo, impone los límites de la máquina.

**Administración de Azure por controladores de tejido (FC)**: en Azure, las VM que se ejecutan en servidores físicos (hojas o nodos) se agrupan en "clústeres" de aproximadamente 1000. Un componente de software de plataforma de escalado horizontal y redundante, llamado FC, administra las VM por separado.

Cada FC administra el ciclo de vida de las aplicaciones que se ejecutan en su clúster, aprovisiona y supervisa el estado del hardware bajo su control. Ejecuta ambas operaciones autónomas, como la reencarnación de instancias de VM en servidores con estado correcto cuando determina que un servidor con estado erróneo. El FC también realiza las operaciones de administración de aplicaciones, como la implementación, actualización y escalado horizontal de las aplicaciones.

El centro de datos se divide en clústeres. Los clústeres aíslan los errores en el nivel de FC y evitan que ciertas clases de errores afecten a los servidores más allá del clúster en el que se producen. Los FC que sirven a un clúster de Azure determinado se agrupan en un clúster de FC.

**Inventario de Hardware**: durante el proceso de configuración de arranque se prepara un inventario de dispositivos de hardware y de red de Azure, y se documenta en el archivo de configuración datacenter.xml. El hardware y los componentes de red nuevos que ingresan en el entorno de producción de Azure deben seguir el proceso de configuración de arranque. El FC es responsable de administrar todo el inventario enumerado en el archivo de configuración datacenter.xml.

**Sistema operativo administrado por FC**: el equipo del sistema operativo proporciona imágenes del sistema operativo en forma de discos duros virtuales (VHD) que se implementan en todas las VM host e invitadas en el entorno de producción de Azure. El equipo del sistema operativo crea estas “imágenes base” a través de un proceso automatizado de compilación sin conexión. La imagen base es una versión del sistema operativo en el que el kernel y otros componentes principales se han modificado y optimizado para admitir el entorno de Azure.

Hay tres tipos de imágenes de sistema operativo administrado por tejido:

- Sistema operativo host: el sistema operativo host es un sistema operativo personalizado que se ejecuta en VM host
- Sistema operativo nativo: sistema operativo nativo que se ejecuta en los inquilinos (por ejemplo, Azure Storage) que no tiene ningún hipervisor
- Sistema operativo invitado: sistema operativo invitado que se ejecuta en VM invitadas

Los sistemas operativos host y nativo administrados por FC están diseñados para su uso en la nube y no son accesibles públicamente.

**Sistemas operativos host y nativo**: el sistema operativo host y el sistema operativo nativo son imágenes de sistemas operativos protegidas que hospedan los agentes de tejido (FA) y se ejecutan en un nodo de proceso (se ejecuta como la primera VM en el nodo) y en los nodos de almacenamiento. Las ventajas de usar las imágenes base optimizadas de los sistemas operativos host y nativo son que reducen la superficie expuesta por las API o por los componentes no utilizados que presentan altos riesgos de seguridad y aumentan la superficie del sistema operativo. Estos sistemas operativos de superficie reducida solo incluyen los componentes necesarios para Azure. Esto mejora el rendimiento y reduce la superficie de ataque.

**Sistema operativo invitado**: los componentes internos de Azure que se ejecutan en VM de sistema operativo invitado no tienen ninguna oportunidad de ejecutar el Protocolo de escritorio remoto (RDP), a diferencia de los clientes externos. Los cambios realizados en los valores de configuración de línea base serían necesarios para pasar por el cambio y liberar el proceso de administración.

## <a name="azure-datacenters"></a>Centros de datos de Azure
El equipo de Microsoft Cloud Infrastructure and Operations (MCIO) administra infraestructura física y las instalaciones de centros de datos de Microsoft para todos los servicios en línea de Microsoft. MCIO es principalmente responsable de administrar los controles físicos y de entorno dentro de los centros de datos, así como de administrar y brindar soporte técnico a los dispositivos de red perimetrales externos (enrutadores perimetrales y enrutadores de centros de datos). MCIO también es responsable de configurar el hardware de servidor mínimo en bastidores en el centro de datos. Los clientes no tienen ninguna interacción directa con Azure.

## <a name="service-management--service-teams"></a>Administración de servicios y equipos de servicio
El soporte técnico del servicio de Azure está a cargo de distintos grupos de ingeniería, conocidos como “Equipos de servicio”. Cada uno de los equipos de servicio es responsable de un área de soporte técnico para Azure. Cada equipo de servicio debe tener un ingeniero disponible las 24 horas del día, los 7 días de la semana, para investigar y resolver errores en el servicio. De manera predeterminada, los equipos de servicio tienen acceso físico al hardware que funciona en Azure.

Los equipos de servicio son:

- Plataforma de aplicaciones
- Azure Active Directory
- Azure Compute
- Red de Azure
- Servicios de ingeniería en la nube
- ISSD: seguridad
- Autenticación multifactor
- SQL Database
- Storage

## <a name="types-of-users"></a>Tipos de usuarios
Todos los usuarios internos de Azure tienen un estado de empleado clasificado con un nivel de confidencialidad que define su acceso a los datos del cliente (con acceso o sin acceso). Los empleados (o contratistas) de Microsoft se consideran usuarios internos. Todos los demás usuarios se consideran usuarios externos. En la tabla siguiente se describen los privilegios de usuario en Azure (permiso de autorización una vez que se realiza la autenticación):

| Rol | Interno o externo | Nivel de confidencialidad | Privilegios autorizados y funciones desempeñadas | Tipo de acceso
| --- | --- | --- | --- | --- |
| Ingeniero de centro de datos de Azure | Interno | Sin acceso a los datos de clientes | Administra la seguridad física de los locales; lleva a cabo patrullas dentro y fuera del centro de datos y supervisa todos los puntos de entrada; realiza servicios de escolta para entrar y salir del centro de datos para el personal sin autorización que proporciona servicios generales (cena, limpieza) o trabajos de TI en el centro de datos; realiza la supervisión y el mantenimiento de rutina del hardware de red; realiza la administración de incidentes y el trabajo de corrección con una variedad de herramientas; realiza la supervisión y el mantenimiento de rutina del hardware físico en los centros de datos; accede al entorno a petición de los propietarios de la propiedad. Capaz de llevar a cabo investigaciones forenses, registro de informe de incidentes, y exigir requisitos obligatorios de seguridad, aprendizaje y directivas; propiedad operativa y mantenimiento de las herramientas de seguridad críticas, como escáneres y recopilación de registros. | Acceso persistente al entorno |
| Evaluación de incidentes de Microsoft Azure (ingenieros de respuesta rápida) | Interno | Acceso a los datos de clientes | Administra las comunicaciones entre los equipos de operaciones de infraestructura, soporte técnico e ingeniería de Azure; evalúa los incidentes de la plataforma, los problemas de implementación y las solicitudes de servicio. | Acceso justo a tiempo en el entorno, con acceso persistente limitado a sistemas que no son de clientes |
| Ingenieros de implementación de Microsoft Azure | Interno | Acceso a los datos de clientes | Realiza implementaciones y actualizaciones de los componentes de la plataforma, el software y los cambios de configuración programados en apoyo de Microsoft Azure. | Acceso justo a tiempo en el entorno, con acceso persistente limitado a sistemas que no son de clientes |
| Soporte técnico de interrupción de clientes de Microsoft Azure (inquilino) | Interno | Acceso a los datos de clientes | Depura y diagnostica los errores e interrupciones de la plataforma para los inquilinos de proceso individuales y las cuentas de Microsoft Azure; analizar los errores e impulsa las correcciones críticas en la plataforma/cliente, impulsa mejoras técnicas a través de soporte técnico. | Acceso justo a tiempo en el entorno, con acceso persistente limitado a sistemas que no son de clientes |
| Ingenieros de sitio activo de Microsoft Azure (ingenieros de supervisión) e incidente | Interno | Acceso a los datos de clientes | Responsable de diagnosticar y mitigar el estado de la plataforma mediante herramientas de diagnóstico; impulsa correcciones de los controladores de volumen,repara elementos provocados por interrupciones y ayuda en las acciones de restauración de interrupciones. | Acceso justo a tiempo en el entorno, con acceso persistente limitado a sistemas que no son de clientes |
|Clientes de Microsoft Azure | Externo | N/D | N/D | N/D |

Azure utiliza identificadores únicos para autenticar a usuarios de la organización y clientes (o procesos que actúan en nombre de usuarios de la organización) para todos los recursos y dispositivos que forman parte del entorno de Azure.

**Autenticación interna de Microsoft Azure**: las comunicaciones entre los componentes internos de Azure están protegidas con cifrado TLS. En la mayoría de los casos, los certificados X.509 son autofirmados. Se establecen excepciones para certificados con conexiones a las que podría accederse desde fuera de la red de Azure y para los FC. Una entidad emisora (CA) de Microsoft, respaldada por una CA raíz de confianza, emite los certificados de los FC. Esto permite que las claves públicas de FC se sustituyan fácilmente. Además, las claves públicas de FC se utilizan en herramientas de desarrollo de Microsoft de modo que, cuando los desarrolladores envían nuevas imágenes de aplicación, se cifran con una clave pública de FC con el fin de proteger los secretos incrustados.

**Autenticación de dispositivos de hardware de Microsoft Azure**: el FC mantiene un conjunto de credenciales (claves o contraseñas) usadas para autenticarse a sí mismo ante varios dispositivos de hardware bajo su control. El sistema usado para el transporte, la persistencia y el uso de estas credenciales está diseñado para evitar que los desarrolladores, los administradores y personal/servicios de copia de seguridad de Azure tengan acceso a información confidencial o privada.

El cifrado basado en la clave pública de identidad maestra del FC se utiliza en los tiempos de configuración y reconfiguración del FC para transferir las credenciales usadas para tener acceso a dispositivos de hardware de red. El FC recupera y cifra las credenciales cuando las necesita.

**Dispositivos de red**: el equipo de red de Azure configura las cuentas de servicio de red para permitir que un cliente de Microsoft Azure se autentique en los dispositivos de red (enrutadores, conmutadores y equilibradores de carga).

## <a name="secure-service-administration"></a>Administración segura del servicio
Es necesario que el personal de operaciones de Microsoft Azure use estaciones de trabajo seguras para la administración (SAW) (los clientes pueden implementar controles similares con estaciones de trabajo de acceso con privilegios, o PAW). El enfoque de SAW es una extensión del procedimiento recomendado ya establecido de utilizar cuentas de usuario y de administración independiente para el personal administrativo. Esta práctica utiliza una cuenta administrativa asignada individualmente que es independiente de la cuenta estándar del usuario. SAW se basa en dicha práctica de separación de cuentas y proporciona una estación de trabajo de confianza para las cuentas confidenciales.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Azure facilities, premises, and physical security](azure-physical-security.md) (Instalaciones de Azure, locales y seguridad física)
- [Availability of Azure infrastructure](azure-infrastructure-availability.md) (Disponibilidad de la infraestructura de Azure)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure](azure-infrastructure-integrity.md) (Integridad de la infraestructura de Azure)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)
