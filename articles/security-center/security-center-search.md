---
title: Búsqueda de registros en Azure Security Center | Microsoft Docs
description: Obtenga información acerca de cómo Azure Security Center usa la búsqueda de registros de Azure Monitor para recuperar y analizar los datos de seguridad.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: c88e2025444aa2fd83e05fdff97ea640ceefaa04
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662926"
---
# <a name="azure-security-center-search-retired"></a>Búsqueda en Azure Security Center (retirada)

> [!NOTE]
> El panel de búsqueda de Security Center se retiró el 31 de julio de 2019. Para obtener más información y servicios alternativos, consulte [Retirada de las características de Security Center (julio de 2019)](security-center-features-retirement-july2019.md#menu_search).

Azure Security Center usa la [búsqueda de registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md) para recuperar y analizar los datos de seguridad. Los registros de Azure Monitor incluyen un lenguaje de consulta para recuperar y consolidar rápidamente los datos. Desde Security Center, puede utilizar la búsqueda de registros de Azure Monitor para construir consultas y analizar los datos recopilados.

Las búsquedas están disponibles en el nivel Gratis y en el nivel Estándar de Security Center.  Los datos disponibles en las búsquedas de registros dependen del nivel que se aplica al área de trabajo.  Para más información, consulte la [página de precios](../security-center/security-center-pricing.md) de Security Center.


> [!NOTE]
> Security Center no guarda los datos de seguridad de un área de trabajo en el nivel Gratis. Puede enviar diversos registros a un área de trabajo en el nivel Gratis y realizar búsquedas en esos datos, pero los resultados de la búsqueda no incluyen datos de Security Center. Security Center solo guarda los datos en un área de trabajo en el nivel Estándar.
>
>

## <a name="access-search"></a>Acceso a las búsquedas
1. En el menú principal de Security Center, seleccione **Buscar**.

   ![Seleccionar Búsqueda de registros][1]

2. Security Center enumera todas las áreas de trabajo de sus suscripciones de Azure. Seleccione un área de trabajo. (Si solo tiene un área de trabajo, este selector de área de trabajo no aparecerá).

   ![Seleccionar un área de trabajo][2]

3. Se abre la opción **Búsqueda de registros**. Para consultar más datos en el área de trabajo seleccionada, escriba esta consulta de ejemplo:

   SecurityEvent | where EventID == 4625 | summarize count() by TargetAccount

   Los resultados muestran todas las cuentas en las que no se pudo iniciar sesión (evento 4625).

   ![Search Results][3]

Consulte [Lenguaje de consulta Kusto](../log-analytics/log-analytics-search-reference.md) para más información sobre cómo consultar los datos del área de trabajo seleccionada.

## <a name="next-steps"></a>Pasos siguientes
En este artículo ha descubierto cómo acceder a las búsquedas de registros en Security Center. Security Center usa la búsqueda de registros de Azure Monitor. Más información sobre la búsqueda de registros de Azure Monitor, consulte:

- [¿Qué son los registros de Azure Monitor?](../log-analytics/log-analytics-overview.md) – Información general de los registros de Azure Monitor
- [Descripción de las búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-search-new.md): en este artículo se describe cómo se usan las búsquedas de registros en los registros de Azure Monitor y se proporcionan los conceptos que debe comprender antes de crear una.
- [Búsqueda de datos mediante búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md): tutorial sobre cómo usar la búsqueda de registros.
- [Referencia de búsqueda de Kusto](../log-analytics/log-analytics-search-reference.md): describe el lenguaje de consultas de los registros de Azure Monitor.

Para más información sobre Security Center, consulte:

- [Introducción a Security Center](security-center-intro.md): aquí se describen las principales funciones de Security Center.

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
