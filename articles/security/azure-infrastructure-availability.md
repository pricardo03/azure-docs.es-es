---
title: Disponibilidad de la infraestructura de Azure
description: En este artículo se describen los niveles de redundancia para proporcionar la máxima disponibilidad de los datos de los clientes.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: a9a55f61f032512be032897d5f21ece405844634
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105882"
---
# <a name="azure-infrastructure-availability"></a>Disponibilidad de la infraestructura de Azure
Azure proporciona una disponibilidad sólida basada en la amplia redundancia lograda con la tecnología de virtualización. Azure proporciona numerosos niveles de redundancia para proporcionar la máxima disponibilidad de los datos de los clientes.

## <a name="temporary-outages-and-natural-disaster"></a>Interrupciones temporales y desastres naturales
El equipo de infraestructura y operaciones de Microsoft Cloud diseña, compila, opera y mejora la infraestructura en la nube. Este equipo garantiza que la infraestructura de Azure ofrece alta disponibilidad y confiabilidad, elevada eficiencia y escalabilidad inteligente. El equipo proporciona una nube más segura, privada y de confianza.

Los sistemas de alimentación ininterrumpida y los grandes bancos de baterías garantizan que la electricidad siga siendo continua si se produce un corte de energía a corto plazo. Los generadores de emergencia suministran energía de reserva para apagones prolongados y mantenimiento planeado. Si se produce un desastre natural, el centro de datos puede usar las reservas de combustible in situ.

Las redes de fibra óptica sólidas y de alta velocidad conectan los centros de datos con otros centros importantes y usuarios de Internet. Calcule las cargas de trabajo de host de los nodos más cerca de los usuarios para reducir la latencia, proporcionar redundancia geográfica y aumentar la resistencia general del servicio. Un equipo de ingenieros trabaja las 24 horas del día para garantizar que los servicios estén siempre disponibles.

Microsoft garantiza una alta disponibilidad mediante la supervisión avanzada y la respuesta a incidentes, el soporte técnico de servicios y la funcionalidad de conmutación por error de copia de seguridad. Los centros de operaciones de Microsoft distribuidos geográficamente operan ininterrumpidamente todos los días del año. La red de Azure es una de las más grandes del mundo. La red de distribución de fibra óptica y contenido conecta los centros de datos y los nodos perimetrales para garantizar un alto rendimiento y una gran confiabilidad.

## <a name="disaster-recovery"></a>Recuperación ante desastres
Azure mantiene los datos durables en dos ubicaciones. Puede elegir la ubicación del sitio de copia de seguridad. En ambas ubicaciones, Azure mantiene constantemente tres réplicas en buen estado de los datos.

## <a name="database-availability"></a>Disponibilidad de la base de datos
Azure garantiza que una base de datos sea accesible desde Internet mediante una puerta de enlace de Internet con disponibilidad sostenida de la base de datos. La supervisión evalúa el mantenimiento y el estado de las bases de datos activas en intervalos de tiempo de cinco minutos.

## <a name="storage-availability"></a>Disponibilidad de almacenamiento
Azure ofrece almacenamiento mediante un servicio de almacenamiento muy durable y escalable que proporciona puntos de conexión de conectividad. Esto significa que una aplicación puede acceder directamente al servicio de almacenamiento. El servicio de almacenamiento procesa las solicitudes de almacenamiento entrantes de forma eficiente, con integridad transaccional.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que Microsoft hace para ayudar a proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](azure-physical-security.md)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](azure-infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](azure-protection-of-customer-data.md)
