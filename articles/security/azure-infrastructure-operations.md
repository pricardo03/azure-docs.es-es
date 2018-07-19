---
title: Operaciones de producción y administración de Azure
description: En este artículo se proporciona una descripción general de la administración y el funcionamiento de la red de producción de Azure.
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
ms.openlocfilehash: dc389f5f5c155555deb860f041b15b0ea49ee416
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101947"
---
# <a name="azure-production-operations-and-management"></a>Operaciones de producción y administración de Azure    
La administración y el funcionamiento de la red de producción de Azure es un esfuerzo coordinado entre los equipos de operaciones de Azure y Azure SQL Database. En el entorno se usan varias herramientas de supervisión de rendimiento de sistemas y de aplicaciones. Los dispositivos de red, servidores, servicios y procesos de aplicación se supervisan con las herramientas adecuadas.

Se implementan varios niveles de supervisión, registro y generación de informes para garantizar la ejecución segura de los servicios que se ejecutan en el entorno de Microsoft Azure, incluidas las siguientes acciones:

- Principalmente, Monitoring Agent (MA) de Microsoft Azure recopila información de registros de supervisión y diagnóstico desde muchos lugares, incluido el controlador de tejido (FC) y el sistema operativo raíz, y la escribe en los archivos de registro. Finalmente, inserta un subconjunto reducido de la información en una cuenta de almacenamiento de Azure configurada previamente. Además, el servicio de supervisión y diagnóstico (MDS) es un servicio independiente que lee distintos datos de registro de supervisión y diagnóstico y se resume la información. MDS escribe la información en un registro integrado. Azure usa la supervisión de seguridad de Azure (ASM) personalizada, que es una extensión del sistema de supervisión de Azure. Tiene componentes que observan, analizan y notifican eventos relativos a la seguridad desde varios lugares de la plataforma.
- La plataforma WinFabric de Microsoft Azure SQL Database proporciona servicios de administración, implementación, desarrollo y control operativo de Microsoft Azure SQL Database. Ofrece servicios de implementación distribuidos y de varios pasos, supervisión del mantenimiento, reparaciones automáticas y cumplimiento de las versiones de servicio. Ofrece los siguientes servicios:

   - Funcionalidades de modelado de servicios con entorno de desarrollo de alta fidelidad (los clústeres de centros de datos son escasos y caros).
   - Flujos de trabajo de implementación y actualización de un solo clic para el arranque y el mantenimiento del servicio.
   - Informes de mantenimiento con flujos de trabajo de reparación automáticos para habilitar la recuperación automática.
   - Supervisión, alertas y utilidades de depuración en tiempo real en todos los nodos de un sistema distribuido.
   - Recopilación centralizada de datos operativos y métricas para análisis distribuido de la causa principal e información del servicio.
   - Herramientas operativas para implementación, administración de cambios y supervisión.
   - Los scripts de plataforma WinFabric y guardián de Microsoft Azure SQL Database se ejecutan continuamente y realizan la supervisión en tiempo real.

Si se produce cualquier anomalía, se activa el proceso de respuesta ante incidentes, seguido por el equipo de evaluación de incidentes de Azure. Se notifica al personal de soporte técnico de Azure adecuado para responder al incidente. El seguimiento y la resolución de problemas se documentan y administran en un sistema centralizado de vales. Las métricas de tiempo de actividad del sistema están disponibles en el acuerdo de confidencialidad (NDA) y a petición.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Red corporativa y acceso multifactor a producción
La base de usuarios de la red corporativa incluye a personal de soporte técnico de Microsoft Azure. La red corporativa admite funciones corporativas internas e incluye el acceso a las aplicaciones internas que se usan para la asistencia al cliente de Azure. La red corporativa está separada tanto de forma lógica como física de la red de producción de Azure. El personal de Microsoft Azure tiene acceso a la red corporativa con equipos portátiles y estaciones de trabajo de Microsoft Azure. Todos los usuarios deben tener una cuenta de Active Directory (AD), incluido un nombre de usuario y una contraseña, para tener acceso a los recursos de la red corporativa. El acceso a la red corporativa usa cuentas de AD, que se emiten a todo el personal, los contratistas y los proveedores de Microsoft, y se administra mediante MSIT. Los identificadores de usuario únicos distinguen al personal según su estado de empleo en Microsoft.

El acceso a las aplicaciones internas de Azure se controla mediante la autenticación con Servicios de federación de Active Directory (AD FS). AD FS es un servicio hospedado por MSIT que proporciona autenticación de los usuarios de la red corporativa a través de la aplicación un token seguro y notificaciones de usuario. AD FS permite que las aplicaciones internas de Microsoft Azure autentiquen usuarios en el dominio de AD corporativo de Microsoft. Para obtener acceso a la red de producción desde el entorno de la red corporativa, el usuario debe autenticarse con autenticación multifactor.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Azure facilities, premises, and physical security](azure-physical-security.md) (Instalaciones de Azure, locales y seguridad física)
- [Availability of Azure infrastructure](azure-infrastructure-availability.md) (Disponibilidad de la infraestructura de Azure)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integrity of Azure infrastructure](azure-infrastructure-integrity.md) (Integridad de la infraestructura de Azure)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)
