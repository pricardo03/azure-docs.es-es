---
title: Exploración de los costos de Azure con Análisis de costos | Microsoft Docs
description: En este artículo se le ayuda a usar análisis de costos para explorar y analizar sus costos de organización de Azure.
services: billing
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/16/2018
ms.topic: conceptual
ms.service: billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: db06956e01ed38ce1f8ee0ce92526be16733fd73
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818131"
---
# <a name="explore-and-analyze-costs-with-cost-analysis"></a>Explore y analice los costos con Análisis de costos

Antes de que pueda controlar y optimizar correctamente sus costos de Azure, debe saber en qué lugar de su organización se originaron los costos. También resulta de utilidad saber cuánto dinero cuestan sus servicios y en apoyo a qué entornos y sistemas. La visión general de la gama de costos al completo es fundamental para conocer con precisión los patrones de gasto de la organización, que se pueden usar para aplicar mecanismos de control de costos, como los presupuestos.

En este artículo, usa análisis de costos para explorar y analizar sus costos de organización. Ve los costos agregados de su organización para saber dónde se acumulan estos e identificar las tendencias de gasto. Ve los costos acumulados con el tiempo para estimar las tendencias de costos mensual, trimestral o incluso anualmente con respecto a un presupuesto. Un presupuesto ayuda a respetar las restricciones financieras o a ver los costos diarios o mensuales para aislar las irregularidades relativas al gasto. Además, puede descargar los datos del informe actual para realizar análisis adicionales o para usarlos en un sistema externo.

## <a name="requirements"></a>Requisitos

El análisis de costos está disponible para todos los clientes con Contrato Enterprise (EA). Debe tener acceso de lectura a al menos uno de los siguientes ámbitos para ver datos de costos.

- Cuenta de facturación de Azure para Contrato Enterprise (inscripción)
- Suscripción de Azure para Contrato Enterprise
- Grupo de recursos de suscripción de Azure para Contrato Enterprise

## <a name="review-costs-in-cost-analysis"></a>Revise los costos en análisis de costos

Para revisar sus costos con análisis de costos, abra Azure Portal y, a continuación, vaya a **Administración de costos + Facturación** &gt; **Cuentas de facturación** &gt; seleccione su cuenta de facturación de Contrato Enterprise &gt; en Administración de costos, seleccione **Análisis de costos**.

La vista de análisis de costos inicial incluye las siguientes áreas:

**Total**: muestra los costos totales correspondientes al mes actual.

**Presupuesto**: muestra el límite de gasto previsto para el ámbito seleccionado, en caso de estar disponible.

**Costo acumulado**: muestra el gasto total acumulado de forma diaria desde principios de mes. Si ha [creado un presupuesto](billing-cost-management-budget-scenario.md#create-the-azure-budget) para su cuenta de facturación o suscripción, podrá ver rápidamente su tendencia de gasto en relación con el presupuesto. Mantenga el puntero sobre una fecha para obtener el costo acumulado hasta ese día.

**Gráficos dinámicos (de anillos)**: proporcione áreas dinámicas.  Desglosan el costo total por un conjunto común de propiedades estándar. En los gráficos se muestra el costo acumulado durante el mes actual, de mayor a menor. Puede cambiar los gráficos dinámicos en cualquier momento seleccionando un área dinámica diferente. De forma predeterminada, los costos se desglosan por servicio (categoría de medidor), ubicación (región) y ámbito secundario (por ejemplo, cuentas de inscripción en cuentas de facturación, grupos de recursos en suscripciones y recursos en grupos de recursos).

![Vista inicial de análisis de costos](./media/billing-cost-analysis/cost-analysis-01.png)



## <a name="customizing-cost-views"></a>Personalización de vistas de costos

La vista predeterminada proporciona respuestas rápidas a preguntas habituales como:

- ¿Cuánto he gastado?
- ¿Me mantendré dentro de mi presupuesto?

Sin embargo, hay muchos casos donde se necesitan análisis más profundos. La personalización comienza en la parte superior de la página, con la selección de fecha.

De forma predeterminada, el análisis de costos muestra los datos correspondientes al mes actual. Use el selector de fecha para cambiar rápidamente al último mes, este mes, este trimestre natural, este año natural o un intervalo de fechas personalizado que elija. Seleccionar el último mes es la forma más rápida de analizar su última factura de Azure y conciliar cargos fácilmente. Las opciones de trimestre y año actuales ayudan a hacer un seguimiento de los costos con respecto a presupuestos a más largo plazo. O bien, puede seleccionar un intervalo de fechas más amplio y detallado, de un solo día a los últimos siete días o los que sean desde un año antes del mes actual.

![Selector de fecha](./media/billing-cost-analysis/date-selector.png)

Asimismo, de forma predeterminada, el análisis de costos muestra los costos **acumulados**. Los costos acumulados incluyen todos los costos correspondientes a cada día, además de los días anteriores, a fin de obtener una vista en constante crecimiento de sus costos acumulados diarios. Esta vista está optimizada para mostrar sus tendencias con respecto a un presupuesto durante el intervalo de tiempo seleccionado.

También está la vista **diaria**. Muestra los costos correspondientes a cada día y no muestra una tendencia de crecimiento. La vista diaria está optimizada para mostrar irregularidades como la subida o bajada de los costes de un día para otro. Al seleccionar un presupuesto, la vista diaria también muestra una estimación del aspecto que puede tener su presupuesto diario. Si sus costos diarios son sistemáticamente superiores al presupuesto diario estimado, puede esperar sobrepasar su presupuesto mensual. El presupuesto diario estimado simplemente se trata de un medio para ayudarle a visualizar su presupuesto en un nivel más bajo. Cuando los costos diarios experimenten fluctuaciones, la comparación del presupuesto diario estimado con su presupuesto mensual será menos precisa.

![Vista diaria](./media/billing-cost-analysis/daily-view.png)

En **Agrupar por** puede seleccionar una categoría de grupo para cambiar los datos mostrados en el gráfico del área total superior. La agrupación le permite ver rápidamente cómo se categorizan sus gastos por tipo de recurso. Esta es una vista de los costos de servicio de Azure correspondientes al último mes.

![Vista acumulada diaria agrupada](./media/billing-cost-analysis/grouped-daily-accum-view.png)

Los gráficos dinámicos están en la vista Total superior. Úsalos para ver vistas correspondientes a diferentes categorías de filtrado y agrupación. Cuando selecciona cualquier categoría de grupo, todo el conjunto de datos correspondientes a la vista total está en la parte inferior de la vista. Este es un ejemplo de los grupos de recursos.

![Todos los datos de la vista actual](./media/billing-cost-analysis/full-data-set.png)

### <a name="download-cost-analysis-data"></a>Descargue datos de análisis de costos

Cuando **descarga** información del análisis de costos, se genera un archivo CSV para todos los datos mostrados actualmente en Azure Portal. Si ha aplicado algún filtro o agrupación, se incluyen en el archivo. Algunos datos subyacentes del gráfico Total superior que no se muestran activamente en el portal también se incluyen en el archivo CSV.

## <a name="next-steps"></a>Pasos siguientes

Consulte otra [Documentación de administración de costos y facturación de Azure](billing-cost-management-budget-scenario.md).
