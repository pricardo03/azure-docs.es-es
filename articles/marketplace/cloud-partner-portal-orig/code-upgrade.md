---
title: Actualizar código a la plataforma más reciente | Azure Marketplace
description: Este artículo explica cómo actualizar su versión de la plataforma Microsoft Dynamics 365 for Operations la versión de la plataforma más reciente
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: aedc2c7474de0fe068a329eb2205e9bb08e62c3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935280"
---
# <a name="upgrading-code-to-the-latest-platform"></a>Actualizar código a la plataforma más reciente

Este artículo explica cómo actualizar su versión de la plataforma Microsoft Dynamics 365 for Operations la versión de la plataforma más reciente.

## <a name="overview"></a>Información general

La plataforma Microsoft Dynamics 365 for Operations consta de los siguientes componentes:

Los binarios de la plataforma Dynamics 365 for Operations como Application Object Server (AOS), el marco de trabajo de administración de datos, el marco de trabajo de informes y e inteligencia empresarial (BI), las herramientas de desarrollo y los servicios de análisis. Los siguientes paquetes de árbol de objetos de aplicación (AOT):

1. Plataforma de aplicaciones
2. Application Foundation
3. Test Essentials

**Importante**: Para mover a la plataforma de Dynamics 365 for Operations más reciente, su implementación de Dynamics 365 for Operations no debe tener personalizaciones (conflicto de superposición) de ninguno de los paquetes AOT que pertenecen a la plataforma. Esta restricción se introdujo en la actualización de la plataforma 3, para que se puedan hacer actualizaciones continuas sin problemas a la plataforma. Si está ejecutando en una plataforma que es anterior a la actualización de la plataforma 3, consulte sección Actualizar a la plataforma de la actualización 3 desde una de compilación anterior al final de este artículo.

Para obtener más información sobre la actualización del código, consulte [aquí](https://docs.microsoft.com/dynamics365/operations/dev-itpro/migration-upgrade/upgrade-latest-platform-update).