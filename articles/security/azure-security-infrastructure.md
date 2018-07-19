---
title: Seguridad de la infraestructura de Azure | Microsoft Docs
description: En este artículo se describe cómo Microsoft garantiza la seguridad de nuestros centros de datos de Azure.
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
ms.openlocfilehash: 397bd1f904b676a6ba020ec78fb1cad05c460be1
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903863"
---
# <a name="azure-infrastructure-security"></a>Seguridad de la infraestructura de Azure
Microsoft Azure se ejecuta en centros de datos administrados y operados por Microsoft. Estos centros de datos geográficamente dispersos cumplen los estándares del sector en materia de seguridad y confiabilidad, como ISO/IEC 27001:2013 y NIST SP 800-53. El personal de operaciones de Microsoft administra y supervisa los centros de datos. El personal de operaciones tiene años de experiencia en la prestación de servicios en línea más grandes del mundo con continuidad ininterrumpida durante el día.

Esta serie de artículos proporciona información sobre lo que Microsoft hace para proteger la infraestructura de Azure. Los artículos abordan lo siguiente:

- [Seguridad física](azure-physical-security.md)
- [Disponibilidad](azure-infrastructure-availability.md)
- [Componentes y límites](azure-infrastructure-components.md)
- [Arquitectura de red](azure-infrastructure-network.md)
- [Red de producción](azure-production-network.md)
- [SQL Database](azure-infrastructure-sql.md)
- [Operaciones](azure-infrastructure-operations.md)
- [Supervisión](azure-infrastructure-monitoring.md)
- [Integridad](azure-infrastructure-integrity.md)
- [Protección de datos](azure-protection-of-customer-data.md)

## <a name="shared-responsibility-model"></a>Modelo de responsabilidad compartida
Es importante comprender la división de responsabilidad entre usted y Microsoft. De forma local, es propietario de la pila completa, pero a medida que se pasa a la nube, algunas responsabilidades se transfieren a Microsoft. La matriz de responsabilidad siguiente muestra las áreas de la pila en una implementación de software como servicio (SaaS), plataforma como servicio (PaaS) e infraestructura como servicio (IaaS) de la que tanto usted como Microsoft son responsables.

![Responsabilidad compartida][1]

Las responsabilidades que siempre tiene, con independencia del tipo de implementación, son:

- Datos
- Puntos de conexión
- Cuenta
- administración de acceso

Asegúrese de que comprende la división de responsabilidades entre usted y Microsoft en una implementación SaaS, PaaS e IaaS. Consulte [Shared Responsibilities for Cloud Computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) (Responsabilidades compartidas de la informática en la nube) para más detalles.

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
- [Integridad de la infraestructura de Azure](azure-infrastructure-integrity.md)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-security-infrastructure/responsibility-zones.png
