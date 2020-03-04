---
title: Administración de incidentes de seguridad en Azure Security Center | Microsoft Docs
description: Este documento le ayuda a usar Azure Security Center para administrar incidentes de seguridad.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: memildin
ms.openlocfilehash: 1a6dbaeac5355d50edb93a7f215d7f8e88231e98
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615967"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Administración de incidentes de seguridad en Azure Security Center

La clasificación e investigación de las alertas de seguridad puede ser un proceso largo incluso para los analistas de seguridad más cualificados, y para muchos es difícil saber incluso por dónde empezar. Con el [análisis](security-center-detection-capabilities.md) para conectar la información entre las distintas [alertas de seguridad](security-center-managing-and-responding-alerts.md), Security Center le proporcionará una vista única de una campaña de ataque y de todas las alertas relacionadas; de esta forma podrá comprender rápidamente las acciones que realizó el atacante y qué recursos se vieron afectados.

En este tema se explican los incidentes de Security Center y cómo se usa este servicio para corregir alertas.

## <a name="what-is-a-security-incident"></a>¿Qué es un incidente de seguridad?

En Security Center, un incidente de seguridad es la suma de todas las alertas de un recurso que se alinean con patrones de [cadenas de eliminación](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Los incidentes aparecen en la lista [Alertas de seguridad](security-center-managing-and-responding-alerts.md). Haga clic en un incidente para ver la lista de alertas relacionadas, lo que permite obtener más información sobre cada repetición.

## <a name="managing-security-incidents"></a>Administración de incidentes de seguridad

1. En el panel de Security Center, haga clic en el icono **Alertas de seguridad**. Se enumerarán los incidentes y alertas. Observe que la descripción de incidentes de seguridad tiene un icono diferente si se compara con otras alertas.

    ![Visualización de los incidentes de seguridad](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Para ver detalles, haga clic en un incidente. La hoja **Security incident detected** (Incidente de seguridad detectado) muestra más detalles. La sección **Información general** puede ofrecer una visión general de lo que desencadenó la alerta de seguridad. Muestra información como el recurso de destino o la dirección IP de origen (si es aplicable) si la alerta sigue activa, así como recomendaciones sobre cómo solucionarla.  

    ![Respuesta a incidentes de seguridad en Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Para más información sobre cada alerta, haga clic en una alerta. La corrección sugerida por Security Center varía según la alerta de seguridad.

   > [!NOTE]
   > La misma alerta puede existir como parte de un incidente y estar visible como alerta independiente.

    ![Detalles de alertas](./media/security-center-incident/security-center-incident-alert.png)

1. Siga los pasos de corrección proporcionados para cada alerta.

Para más información sobre las alertas, vea [Administración y respuesta a las alertas de seguridad](security-center-managing-and-responding-alerts.md).

Los temas siguientes le guiarán a través de las diferentes alertas según los tipos de recursos:

* [Alertas de máquinas Windows con IaaS](threat-protection.md#windows-machines)
* [Alertas de máquinas Linux con IaaS](threat-protection.md#linux-machines)
* [Alertas de Azure App Service](threat-protection.md#app-services)
* [Alertas de contenedores de Azure](threat-protection.md#azure-containers)
* [Alertas de SQL Database y SQL Data Warehouse](threat-protection.md#data-sql)
* [Alertas de Azure Storage](threat-protection.md#azure-storage)
* [Alertas de Cosmos DB](threat-protection.md#cosmos-db)

Los temas siguientes explican cómo Security Center usa los diferentes datos de telemetría que recopila de la integración con la infraestructura de Azure a fin de aplicar capas de protección adicionales para los recursos implementados en Azure:

* [Alertas de la capa de administración de Azure (Azure Resource Manager) (versión preliminar)](threat-protection.md#management-layer)
* [Alertas de Azure Key Vault (versión preliminar)](threat-protection.md#azure-keyvault)
* [Alertas de la capa de red de Azure](threat-protection.md#network-layer)
* [Alertas de otros servicios](threat-protection.md#alerts-other)

## <a name="see-also"></a>Consulte también
En este documento, aprendió a usar la funcionalidad de incidente de seguridad en el Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Alertas de seguridad en Azure Security Center](security-center-alerts-overview.md)
* [Administración de alertas de seguridad](security-center-managing-and-responding-alerts.md)
* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md)
