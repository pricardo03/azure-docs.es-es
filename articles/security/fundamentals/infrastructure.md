---
title: Seguridad de la infraestructura de Azure | Microsoft Docs
description: En el artículo se describe cómo trabaja Microsoft para proteger nuestros centros de datos de Azure.
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
ms.openlocfilehash: 1dd9d9d4fc47f13f275e3be87ad282640840f873
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68611623"
---
# <a name="azure-infrastructure-security"></a>Seguridad de la infraestructura de Azure
Microsoft Azure se ejecuta en centros de datos administrados y operados por Microsoft. Estos centros de datos geográficamente dispersos cumplen los estándares del sector en materia de seguridad y confiabilidad, como ISO/IEC 27001:2013 y NIST SP 800-53. El personal de operaciones de Microsoft administra y supervisa los centros de datos. El personal de operaciones tiene años de experiencia en la prestación de servicios en línea más grandes del mundo con continuidad ininterrumpida durante el día.

Esta serie de artículos proporciona información sobre lo que Microsoft hace para proteger la infraestructura de Azure. Los artículos abordan lo siguiente:

- [Seguridad física](physical-security.md)
- [Disponibilidad](infrastructure-availability.md)
- [Componentes y límites](infrastructure-components.md)
- [Arquitectura de red](infrastructure-network.md)
- [Red de producción](production-network.md)
- [SQL Database](infrastructure-sql.md)
- [Operaciones](infrastructure-operations.md)
- [Supervisión](infrastructure-monitoring.md)
- [Integridad](infrastructure-integrity.md)
- [Protección de datos](protection-customer-data.md)

## <a name="shared-responsibility-model"></a>Modelo de responsabilidad compartida
Es importante comprender la división de responsabilidad entre usted y Microsoft. De forma local, es propietario de la pila completa, pero a medida que se pasa a la nube, algunas responsabilidades se transfieren a Microsoft. En el siguiente gráfico se ilustran las áreas de responsabilidad, según el tipo de implementación de la pila (software como servicio [SaaS], plataforma como servicio [PaaS], infraestructura como servicio [IaaS] y entono local).

![Gráfico que muestra las responsabilidades](./media/infrastructure/responsibility-zones.png)

Sin importar el tipo de implementación, siempre es responsable de lo siguiente:

- Datos
- Puntos de conexión
- Cuenta
- administración de acceso

Asegúrese de que comprende la división de responsabilidades entre usted y Microsoft en una implementación SaaS, PaaS e IaaS. Para más información, consulte [Shared responsibilities for cloud computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) (Responsabilidades compartidas en la informática en la nube).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que Microsoft hace para ayudar a proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Disponibilidad de la infraestructura de Azure](infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](infrastructure-components.md)
- [Arquitectura de red de Azure](infrastructure-network.md)
- [Red de producción de Azure](production-network.md)
- [Características de seguridad de Azure SQL Database](infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)


