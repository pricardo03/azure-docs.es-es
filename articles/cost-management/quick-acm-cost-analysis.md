---
title: 'Guía de inicio rápido: Exploración de los costos de Azure con el análisis de costos | Microsoft Docs'
description: Este guía de inicio rápido le ayudará a usar el análisis de costos para explorar y analizar sus costos de organización de Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: quickstart
ms.service: cost-management
manager: micflan
ms.custom: seodec18
ms.openlocfilehash: b4302713188237b97ffbe8473f6a37edd6741b36
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793090"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Guía de inicio rápido: Explore y analice los costos con Análisis de costos

Antes de que pueda controlar y optimizar correctamente sus costos de Azure, debe saber en qué lugar de su organización se originaron los costos. También resulta de utilidad saber cuánto dinero cuestan sus servicios y en apoyo a qué entornos y sistemas. La visión general de la gama de costos al completo es fundamental para conocer con precisión los patrones de gasto de la organización. Los patrones de gastos pueden utilizarse para aplicar mecanismos de control de costos, como los presupuestos.

En esta guía de inicio rápido, va a utilizar el análisis de costos para explorar y analizar los costos de organización. Puede ver los costos agregados de la organización para saber dónde se producen esos costos con el tiempo e identificar las tendencias de gasto. Puede ver los costos acumulados con el tiempo para estimar las tendencias de costos mensual, trimestral o incluso anualmente con respecto a un presupuesto. Un presupuesto ayuda a los proveedores a cumplir con las restricciones financieras. Y un presupuesto se utiliza para ver los costos diarios o mensuales para aislar las irregularidades de los gastos. Además, puede descargar los datos del informe actual para realizar análisis adicionales o para usarlos en un sistema externo.

En esta guía de inicio rápido, ha aprendido a hacer lo siguiente:

- Revise los costos en análisis de costos
- Personalización de vistas de costos
- Descargue datos de análisis de costos


## <a name="prerequisites"></a>Requisitos previos

El análisis de costos es compatible con varios tipos de cuenta de Azure. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure.

Los clientes con [Contrato Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) deben tener, al menos, acceso de lectura a uno o varios de los siguientes ámbitos para ver datos de costos.

- Cuenta de facturación
- department
- Cuenta de inscripción
- Grupo de administración
- Subscription
- Grupos de recursos

Para más información acerca de cómo asignar acceso a los datos de Cost Management, consulte [Asignación del acceso a los datos](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

- Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="review-costs-in-cost-analysis"></a>Revise los costos en análisis de costos

Para revisar los costos en el análisis de costos, abra el ámbito en Azure Portal y seleccione **Análisis de costos** en el menú. Por ejemplo, vaya a **Suscripciones**, seleccione una suscripción de la lista y **Análisis de costos**, en el menú. Use la píldora **Ámbito** para cambiar a un ámbito diferente en el análisis de costos. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

El ámbito que seleccione se utiliza en todo Cost Management para proporcionar la consolidación de datos y controlar el acceso a la información de costos. Cuando use ámbitos, no realice una selección múltiple de ellos. En su lugar, seleccione un ámbito mayor al que otros se acumulan y luego filtre a los ámbitos anidados que necesite. Es importante comprender este enfoque, ya que algunas personas pueden no tener acceso a un solo ámbito principal, que abarca múltiples ámbitos anidados.

La vista de análisis de costos inicial incluye las siguientes áreas:

**Total**: muestra los costos totales correspondientes al mes actual.

**Presupuesto**: muestra el límite de gasto previsto para el ámbito seleccionado, en caso de estar disponible.

**Costo acumulado**: muestra el gasto total agregado de forma diaria desde principios de mes. Después de [crear un presupuesto](tutorial-acm-create-budgets.md) para su cuenta de facturación o suscripción, podrá ver rápidamente su tendencia de gastos con el presupuesto. Mantenga el puntero sobre una fecha para ver el costo acumulado hasta ese día.

**Gráficos dinámicos (anillos)** : proporcionan gráficos dinámicos, que desglosan el costo total por un conjunto común de propiedades estándar. Muestran el costo acumulado durante el mes actual, de mayor a menor. Puede cambiar los gráficos dinámicos en cualquier momento seleccionando un área dinámica diferente. Los costos se clasifican por servicio (categoría de medidor), ubicación (región) y ámbito secundario predeterminado. Por ejemplo, cuentas de inscripción en cuentas de facturación, grupos de recursos en suscripciones y recursos en grupos de recursos.

![Vista inicial de análisis de costos en Azure Portal](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Personalización de vistas de costos

El análisis de costos tiene cuatro vistas integradas, optimizadas para los objetivos más comunes:

Ver | Responder preguntas como...
--- | ---
Costo acumulado | ¿Cuánto he gastado hasta ahora en este mes? ¿Me mantendré dentro de mi presupuesto?
Costo diario | ¿Ha habido algún aumento en los costos por día durante los últimos 30 días?
Costo por servicio | ¿Cómo ha variado mi uso mensual en las últimas 3 facturas?
Costo por recurso | ¿Qué recursos cuestan más en lo que va de mes?

![Selector de vista que muestra una selección de ejemplo de este mes](./media/quick-acm-cost-analysis/view-selector.png)

Sin embargo, hay muchos casos donde se necesitan análisis más profundos. La personalización comienza en la parte superior de la página, con la selección de fecha.

De forma predeterminada, el análisis de costos muestra los datos correspondientes al mes actual. Utilice el selector de fecha para cambiar rápidamente a los intervalos de fechas comunes. Algunos ejemplos son los últimos siete días, el mes pasado, el año actual o un intervalo de fechas personalizado. Las suscripciones de pago por uso también incluyen los intervalos de fechas según el período de facturación, que no está enlazado con el mes natural, como el período de facturación actual o la última factura. Use los vínculos  **<ANTERIOR** y  **SIGUIENTE>** en la parte superior del menú para ir al período anterior o siguiente, respectivamente. Por ejemplo, **<ANTERIOR** cambiará de los últimos siete días al período de hace 8-14 días y luego de hace 15-21 días.

![Selector de fecha que muestra una selección de ejemplo de este mes](./media/quick-acm-cost-analysis/date-selector.png)

De forma predeterminada, el análisis de costos muestra los costos **acumulados**. Los costos acumulados incluyen todos los costos correspondientes a cada día, además de los días anteriores, a fin de obtener una vista en constante crecimiento de sus costos agregados diarios. Esta vista está optimizada para mostrar sus tendencias con respecto a un presupuesto durante el intervalo de tiempo seleccionado.

También hay la vista **diaria** que muestra los costos de cada día. La vista diaria no muestra una tendencia de crecimiento. La vista está diseñada para mostrar irregularidades como picos de costos o caídas de un día para otro. Si ha seleccionado un presupuesto, la vista diaria también muestra una estimación del aspecto que puede tener su presupuesto diario. Cuando los costos diarios son sistemáticamente superiores al presupuesto diario estimado, puede esperar sobrepasar su presupuesto mensual. El presupuesto diario estimado simplemente se trata de un medio para ayudarle a visualizar su presupuesto en un nivel más bajo. Cuando los costos diarios experimenten fluctuaciones, la comparación del presupuesto diario estimado con su presupuesto mensual será menos precisa.

Por lo general, puede esperar ver datos o notificaciones para los recursos consumidos en 8-12 horas.

![Vista diaria que muestra los costos diarios de ejemplo del mes actual](./media/quick-acm-cost-analysis/daily-view.png)

**Agrupe por** propiedades comunes para desglosar los costos e identificar los principales colaboradores. Para agrupar por etiquetas de recursos, por ejemplo, seleccione la clave de etiqueta por la que desea agrupar. Los costos se desglosan por cada valor de etiqueta, con un segmento adicional para los recursos que tengan dicha etiqueta aplicada.

La mayoría de [los recursos de Azure admiten el etiquetado](../azure-resource-manager/tag-support.md); sin embargo, algunas etiquetas no están disponibles en Cost Management y facturación. Además, no se admiten las etiquetas de grupo de recursos. Cost Management solo admite etiquetas de recursos a partir de la fecha en que las etiquetas se aplican directamente al recurso. Consulte el vídeo sobre la [revisión de las directivas de etiquetas con Azure Cost Management](https://www.youtube.com/watch?v=nHQYcYGKuyw) para obtener información sobre el uso de una directiva de etiqueta de Azure para mejorar la visibilidad de los datos sobre costos.

Esta es una vista de los costos de servicio de Azure correspondientes al último mes.

![Vista acumulada diaria agrupada que muestra los costos de servicio de Azure de ejemplo correspondientes al último mes](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Los gráficos dinámicos del gráfico principal muestran distintas agrupaciones para una imagen más amplia de los costos generales durante el período seleccionado y con los filtros elegidos. Seleccione una propiedad o una etiqueta para ver los costos agregados por cualquier dimensión.


![Todos los datos de la vista actual donde se muestran los nombres de los grupos de recursos](./media/quick-acm-cost-analysis/full-data-set.png)

La imagen anterior muestra los nombres de los grupos de recursos. Aunque se puede agrupar por etiqueta para ver los costos totales por etiqueta, en ninguna de las vistas de análisis de costos está disponible la posibilidad de ver todas las etiquetas por recurso o grupo de recursos.

Al agrupar los costos por un atributo concreto, se muestran los diez mayores contribuidores a los costos, de mayor a menor. Si hay más de diez grupos, se muestran los nueve mayores contribuidores a los costos, así como un grupo **Otros**, que abarca todos los grupos restantes juntos. Al agrupar por etiquetas, también puede ver un grupo **Sin etiqueta** para los costos a los que no se ha aplicado la clave de etiqueta. El grupo **Sin etiqueta** siempre es el último, aunque haya más costos sin etiqueta que con ella. Si hay 10 o más valores de etiqueta, los costos sin etiqueta formarán parte de **Otros**.

Las máquinas virtuales, redes y recursos de almacenamiento *clásicos* no comparten datos de facturación detallados. Al agrupar los costos, se combinan como **servicios clásicos**.

Puede ver el conjunto de datos completo de cualquier vista. Todas las selecciones o los filtros que aplique afectan a los datos presentados. Para ver el conjunto de datos completo, haga clic en la lista **tipo de gráfico** y, después, haga clic en la vista **Tabla**.

![Datos de la vista actual en una vista de tabla](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="download-cost-analysis-data"></a>Descargue datos de análisis de costos

Puede **descargar** información del análisis de costos para generar un archivo CSV para todos los datos mostrados actualmente en Azure Portal. Cualquier filtro o agrupación que se aplique se incluye en el archivo. Los datos subyacentes del gráfico Total superior que no se muestran activamente se incluyen en el archivo CSV.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el primer tutorial para aprender crear y administrar presupuestos.

> [!div class="nextstepaction"]
> [Creación y administración de presupuestos](tutorial-acm-create-budgets.md)
