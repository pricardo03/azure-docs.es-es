---
title: 'Tutorial: Previsión de gastos con Cloudyn en Azure | Microsoft Docs'
description: En este tutorial, aprenderá a pronosticar los gastos mediante el historial de uso y los datos de gastos.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: tutorial
ms.service: cost-management-billing
ms.custom: seodec18
ms.reviewer: benshy
ms.openlocfilehash: 43b48cf96baaa01408d068de509e5be9fdf950ca
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76769929"
---
# <a name="tutorial-forecast-future-spending"></a>Tutorial: Previsión de gastos futuros

Cloudyn le ayuda a predecir los gastos futuros mediante el historial de uso y los datos de gasto. Use informes de Cloudyn para ver todos los datos de proyección de costos. Los ejemplos de este tutorial le guían por la revisión de las proyecciones de costos mediante los informes. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Previsión de gastos futuros

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

- Debe tener una cuenta de Azure.
- Debe tener un registro de prueba o una suscripción de pago en Cloudyn.

## <a name="forecast-future-spending"></a>Previsión de gastos futuros

Cloudyn incluye informes de proyección de costos que le ayudarán a pronosticar los gastos según su uso con el paso del tiempo. Su objetivo principal es ayudarle a asegurarse de que sus tendencias de costo no superen las expectativas de su organización. Los informes que usará son el de costo previsto del mes en curso y el de costo previsto anual. Ambos muestran el gasto futuro previsto si su uso permanece relativamente coherente con los últimos 30 días de uso.

El informe de costo previsto del mes en curso muestra los costos de los servicios. Emplea los costos desde el comienzo del mes y el mes anterior para mostrar el costo previsto. En el menú de informes de la parte superior del portal, haga clic en **Costs** >  (Costos) **Projection and Budget** >  (Proyección y presupuesto) **Current Month Projected Cost** (Costo previsto del mes en curso). En la imagen siguiente se muestra un ejemplo:

![Información de ejemplo que se muestra en el informe de costos previstos del mes actual](./media/tutorial-forecast-spending/project-month01.png)

En el ejemplo, puede ver en qué servicios se realiza un mayor gasto. Los costos de Azure fueron más bajos que los costos de AWS. Si quiere ver los detalles de la proyección de costos para las máquinas virtuales de Azure, en la lista **Filter** (Filtro), seleccione **Azure/VM**.

![Ejemplo que muestra el costo previsto del mes actual de las máquinas virtuales de Azure](./media/tutorial-forecast-spending/project-month02.png)

Siga los mismos pasos básicos anteriores para examinar las proyecciones de costos mensuales para otros servicios que le interesen.

El informe de costo previsto anual muestra el costo extrapolado de los servicios durante los próximos 12 meses.

En el menú de informes de la parte superior del portal, haga clic en **Costs** >  (Costos) **Projection and Budget** >  (Proyección y presupuesto) **Annual Projected Cost** (Costo previsto anual). En la imagen siguiente se muestra un ejemplo:

![Ejemplo que muestra el informe de costos previstos anual](./media/tutorial-forecast-spending/project-annual01.png)

En el ejemplo, puede ver en qué servicios se realiza un mayor gasto. Al igual que en el ejemplo mensual, los costos de Azure eran más bajos que los costos de AWS. Si quiere ver los detalles de la proyección de costos para las máquinas virtuales de Azure, en la lista **Filter** (Filtro), seleccione **Azure/VM**.

![Ejemplo que muestra el costo previsto anual de máquinas virtuales](./media/tutorial-forecast-spending/project-annual02.png)

En la imagen anterior, el costo previsto anual de máquinas virtuales de Azure es 28.374 dólares USA.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Previsión de gastos futuros


Vaya al siguiente tutorial para conocer cómo administrar los costos con los informes de visualización y asignación de costos.

> [!div class="nextstepaction"]
> [Administración de costos con informes de visualización y asignación de costos](../../cost-management/tutorial-manage-costs.md)
