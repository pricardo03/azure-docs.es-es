---
title: 'Guía de inicio rápido: Exploración de los costos de Azure con el análisis de costos | Microsoft Docs'
description: Este guía de inicio rápido le ayudará a usar el análisis de costos para explorar y analizar sus costos de organización de Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/10/2018
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 12b7a605350b07565660e9e4d1334b286aa5ac00
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079113"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Guía de inicio rápido: Exploración y análisis de costos con el análisis de costos

Antes de que pueda controlar y optimizar correctamente sus costos de Azure, debe saber en qué lugar de su organización se originaron los costos. También resulta de utilidad saber cuánto dinero cuestan sus servicios y en apoyo a qué entornos y sistemas. La visión general de la gama de costos al completo es fundamental para conocer con precisión los patrones de gasto de la organización. Los patrones de gastos pueden utilizarse para aplicar mecanismos de control de costos, como los presupuestos.

En esta guía de inicio rápido, va a utilizar el análisis de costos para explorar y analizar los costos de organización. Puede ver los costos agregados de la organización para saber dónde se producen esos costos con el tiempo e identificar las tendencias de gasto. Puede ver los costos acumulados con el tiempo para estimar las tendencias de costos mensual, trimestral o incluso anualmente con respecto a un presupuesto. Un presupuesto ayuda a los proveedores a cumplir con las restricciones financieras. Y un presupuesto se utiliza para ver los costos diarios o mensuales para aislar las irregularidades de los gastos. Además, puede descargar los datos del informe actual para realizar análisis adicionales o para usarlos en un sistema externo.

En esta guía de inicio rápido, ha aprendido a hacer lo siguiente:

- Revise los costos en análisis de costos
- Personalización de vistas de costos
- Descargue datos de análisis de costos


## <a name="prerequisites"></a>Requisitos previos

El análisis de costos está disponible para todos los clientes con [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Debe tener al menos acceso de lectura a uno o varios de los siguientes ámbitos para ver datos de costos.

- El ámbito de la *cuenta de facturación* se define en https://ea.azure.com y requiere acceso de administrador de organización. No se requiere ninguna configuración de EA como requisito previo. La información de facturación en el análisis de costos se consolida para todas las suscripciones del contrato Enterprise. A menudo, la cuenta de facturación se denomina *contrato Enterprise* o *inscripción*.

- El ámbito de *departamento* ámbito se define en https://ea.azure.com y requiere acceso de administrador de departamento. La opción **DA view charges** (El administrador del departamento ve los cargos) debe estar habilitada en el portal de EA. La información de facturación en el análisis de costos se consolida para todas las suscripciones que pertenecen a la cuenta de inscripción vinculadas al departamento.

- El ámbito de *cuenta de inscripción* se define en https://ea.azure.com y requiere acceso de propietario de la cuenta. La opción **AO view charges** (El propietario de la cuenta ve los cargos) debe estar habilitada en el portal de EA. La información de facturación en el análisis de costos se consolida para todas las suscripciones que pertenecen a la cuenta de inscripción. La cuenta de inscripción a menudo se denomina el *propietario de la cuenta*.

- El ámbito de *grupo de administración* se define en https://portal.azure.com y requiere acceso de lector de administración de costos (o lector). La opción **AO view charges** (El propietario de la cuenta ve los cargos) debe estar habilitada en el portal de EA. La información de facturación en el análisis de costos se consolida para todas las suscripciones debajo del grupo de administración.

- El ámbito de *suscripción* se define en https://portal.azure.com y requiere acceso de lector de administración de costos (o lector). La opción **AO view charges** (El propietario de la cuenta ve los cargos) debe estar habilitada en el portal de EA. La información de facturación en el análisis de costos se consolida para todos los recursos y grupos de recursos de la suscripción.

- El ámbito de *grupo de recursos* se define en https://portal.azure.com y requiere acceso de lector de administración de costos (o lector). La opción **AO view charges** (El propietario de la cuenta ve los cargos) debe estar habilitada en el portal de EA. La información de facturación en el análisis de costos se consolida para todos los recursos y grupos de recursos.



Para más información acerca de cómo configurar los valores de **gastos de la vista de AD** y **gastos de la vista de PC**, consulte [Habilitación del acceso a los costos](../billing/billing-enterprise-mgmt-grp-troubleshoot-cost-view.md#enabling-access-to-costs).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

- Inicie sesión en Azure Portal en http://portal.azure.com.

## <a name="review-costs-in-cost-analysis"></a>Revise los costos en análisis de costos

Para revisar los costos con el análisis de costos, en Azure Portal, vaya a **Administración de costos + Facturación**  &gt; **Administración de costos** &gt;  **Cambiar ámbito**, elija un ámbito y haga clic en **Seleccionar**.

El ámbito que seleccione se utiliza en todo Cost Management para proporcionar la consolidación de datos y controlar el acceso a la información de costos. Cuando use ámbitos, no realice una selección múltiple de ellos. En su lugar, seleccione un ámbito mayor al que otros se acumulan y luego filtre lo que desee. Es importante comprender esto porque algunas personas no deberían tener acceso a un ámbito principal al que se acumulan los ámbitos secundarios.

Haga clic en **Abrir análisis de costos**.

La vista de análisis de costos inicial incluye las siguientes áreas:

**Total**: muestra los costos totales correspondientes al mes actual.

**Presupuesto**: muestra el límite de gasto previsto para el ámbito seleccionado, en caso de estar disponible.

**Costo acumulado**: muestra el gasto total acumulado de forma diaria desde principios de mes. Después de [crear un presupuesto](tutorial-acm-create-budgets.md) para su cuenta de facturación o suscripción, podrá ver rápidamente su tendencia de gastos con el presupuesto. Mantenga el puntero sobre una fecha para ver el costo acumulado hasta ese día.

**Gráficos dinámicos (anillos)**: proporcionan gráficos dinámicos, que desglosan el costo total por un conjunto común de propiedades estándar. Muestran el costo acumulado durante el mes actual, de mayor a menor. Puede cambiar los gráficos dinámicos en cualquier momento seleccionando un área dinámica diferente. Los costos se clasifican por servicio (categoría de medidor), ubicación (región) y ámbito secundario predeterminado. Por ejemplo, cuentas de inscripción en cuentas de facturación, grupos de recursos en suscripciones y recursos en grupos de recursos.

![Vista inicial de análisis de costos](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Personalización de vistas de costos

La vista predeterminada proporciona respuestas rápidas a preguntas habituales como:

- ¿Cuánto he gastado?
- ¿Me mantendré dentro de mi presupuesto?

Sin embargo, hay muchos casos donde se necesitan análisis más profundos. La personalización comienza en la parte superior de la página, con la selección de fecha.

De forma predeterminada, el análisis de costos muestra los datos correspondientes al mes actual. Use el selector de fecha para cambiar rápidamente al último mes, este mes, este trimestre natural, este año natural o un intervalo de fechas personalizado que elija. Seleccionar el último mes es la forma más rápida de analizar su última factura de Azure y conciliar cargos fácilmente. Las opciones de trimestre y año actuales ayudan a hacer un seguimiento de los costos con respecto a presupuestos a más largo plazo. También puede seleccionar un intervalo de fechas diferente. Por ejemplo, puede seleccionar un solo día, los últimos siete días o los que sean desde un año antes del mes actual.

![Selector de fecha](./media/quick-acm-cost-analysis/date-selector.png)

De forma predeterminada, el análisis de costos muestra los costos **acumulados**. Los costos acumulados incluyen todos los costos correspondientes a cada día, además de los días anteriores, a fin de obtener una vista en constante crecimiento de sus costos acumulados diarios. Esta vista está optimizada para mostrar sus tendencias con respecto a un presupuesto durante el intervalo de tiempo seleccionado.

También hay la vista **diaria** que muestra los costos de cada día. La vista diaria no muestra una tendencia de crecimiento. La vista está diseñada para mostrar irregularidades como picos de costos o caídas de un día para otro. Si ha seleccionado un presupuesto, la vista diaria también muestra una estimación del aspecto que puede tener su presupuesto diario. Cuando los costos diarios son sistemáticamente superiores al presupuesto diario estimado, puede esperar sobrepasar su presupuesto mensual. El presupuesto diario estimado simplemente se trata de un medio para ayudarle a visualizar su presupuesto en un nivel más bajo. Cuando los costos diarios experimenten fluctuaciones, la comparación del presupuesto diario estimado con su presupuesto mensual será menos precisa.

![Vista diaria](./media/quick-acm-cost-analysis/daily-view.png)

En **Agrupar por** puede seleccionar una categoría de grupo para cambiar los datos mostrados en el gráfico del área total superior. La agrupación le permite ver rápidamente cómo se categorizan sus gastos por tipo de recurso. Esta es una vista de los costos de servicio de Azure correspondientes al último mes.

![Vista acumulada diaria agrupada](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Los gráficos dinámicos bajo la vista Total superior muestran vistas para diferentes categorías de agrupación y filtrado. Cuando selecciona cualquier categoría de grupo, todo el conjunto de datos correspondientes a la vista total está en la parte inferior de la vista. Este es un ejemplo de los grupos de recursos.

![Todos los datos de la vista actual](./media/quick-acm-cost-analysis/full-data-set.png)

La imagen anterior muestra los nombres de los grupos de recursos. Las etiquetas de visualización de recursos no están disponibles en ninguna de las vistas, filtros o agrupaciones de análisis de costos.

Al agrupar los costos por un atributo concreto, se muestran los diez mayores contribuidores a los costos, de mayor a menor. Si hay más de diez grupos, se muestran los nueve mayores contribuidores a los costos, así como un grupo **Otros**, que abarca todos los grupos restantes juntos.

Las máquinas virtuales, redes y recursos de almacenamiento *clásicos* (Azure Service Management o ASM) no comparten datos de facturación detallados. Al agrupar los costos se combinan como **servicios clásicos**.


## <a name="download-cost-analysis-data"></a>Descargue datos de análisis de costos

Puede **descargar** información del análisis de costos para generar un archivo CSV para todos los datos mostrados actualmente en Azure Portal. Cualquier filtro o agrupación que se aplique se incluye en el archivo. Los datos subyacentes del gráfico Total superior que no se muestran activamente se incluyen en el archivo CSV.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el primer tutorial para aprender crear y administrar presupuestos.

> [!div class="nextstepaction"]
> [Creación y administración de presupuestos](tutorial-acm-create-budgets.md)
