---
title: Actualizar código a la plataforma más reciente | Microsoft Docs
description: Este artículo explica cómo actualizar su versión de la plataforma Microsoft Dynamics 365 for Operations la versión de la plataforma más reciente
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 787d9ef509bcafeb6a21d4b0ec952ea1eb4fd52b
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808013"
---
# <a name="upgrading-code-to-the-latest-platform"></a>Actualizar código a la plataforma más reciente

Este artículo explica cómo actualizar su versión de la plataforma Microsoft Dynamics 365 for Operations la versión de la plataforma más reciente.

## <a name="overview"></a>Información general

La plataforma Microsoft Dynamics 365 for Operations consta de los siguientes componentes:

Los binarios de la plataforma Dynamics 365 for Operations como Application Object Server (AOS), el marco de trabajo de administración de datos, el marco de trabajo de informes y e inteligencia empresarial (BI), las herramientas de desarrollo y los servicios de análisis. Los siguientes paquetes de árbol de objetos de aplicación (AOT):

1. Plataforma de aplicaciones
2. Application Foundation
3. Test Essentials

**Importante**: para mover a la plataforma de Dynamics 365 for Operations más reciente, su implementación de Dynamics 365 for Operations no debe tener personalizaciones (conflicto de superposición) de cualquiera de los paquetes AOT que pertenecen a la plataforma. Esta restricción se introdujo en la actualización de la plataforma 3, para que se puedan hacer actualizaciones continuas sin problemas a la plataforma. Si está ejecutando en una plataforma que es anterior a la actualización de la plataforma 3, consulte sección Actualizar a la plataforma de la actualización 3 desde una de compilación anterior al final de este artículo.

Para obtener más información sobre la actualización del código, consulte [aquí](https://docs.microsoft.com/dynamics365/operations/dev-itpro/migration-upgrade/upgrade-latest-platform-update).