---
title: Prevención de costos inesperados y administración de la facturación en Azure
description: Obtenga información acerca de cómo evitar cargos inesperados en la factura de Azure. Use las características de administración y seguimiento del costo de las suscripciones a Azure.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: d6c287d5ead0095a4f7bb5ad754212b134f7103c
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719818"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Prevención de cargos inesperados con la administración de costos y facturación de Azure

Al suscribirse a Azure, hay varias cosas que puede hacer para obtener una idea más clara de los gastos:

- La [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) puede proporcionar una estimación de los costos antes de crear un recurso de Azure. 

- [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) proporciona el desglose de costos actual y la previsión correspondientes a la suscripción. 

- Si desea agrupar y comprender los costos para distintos proyectos o equipos, consulte el [etiquetado de recursos](../azure-resource-manager/resource-group-using-tags.md). Si prefiere usar el sistema de informes del que dispone su organización, compruebe las [API de facturación](billing-usage-rate-card-overview.md).

- Si la suscripción se creó a partir de un Contrato Enterprise (EA), puede ver los costos en Azure Portal. Si realiza la suscripción con un Proveedor de soluciones en la nube (CSP) o Patrocinio de Azure, no podrá aplicar algunas de las siguientes características. Para más información, consulte [Recursos adicionales para EA, CSP y Patrocinio](#other-offers).

- Si la suscripción es una oferta de evaluación gratuita, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure bajo licencia Open (AIO) o BizSpark, la suscripción se deshabilitará automáticamente cuando se usen todos los créditos. Obtenga más información acerca de los [límites de gasto](#spending-limit) para evitar que la suscripción se deshabilite inesperadamente.

- Si se ha registrado para obtener una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), [puede utilizar algunos de los servicios de Azure más populares de forma gratuita durante 12 meses](billing-create-free-services-included-free-account.md). Junto con las recomendaciones que se enumeran a continuación, consulte [Evitar cargos en su cuenta gratuita de Azure](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Obtención de costos estimados antes de agregar servicios de Azure

A continuación se ofrece información adicional sobre la estimación de costos mediante las siguientes herramientas:
- Calculadora de precios de Azure
- Portal de Azure
- Límite de gasto

Las imágenes de las secciones siguientes muestran ejemplos de precios en dólares estadounidenses.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Estimación del coste en línea con la calculadora de precios

Consulte la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para obtener un costo mensual estimado del servicio que le interesa. Puede agregar cualquier recurso de Azure propio para obtener un costo estimado. En la calculadora de precios, puede cambiar el tipo de moneda.

![Captura de pantalla del menú de la calculadora de precios](./media/billing-getting-started/pricing-calc.png)

Por ejemplo, en la calculadora de precios, se estima que una máquina virtual Windows A1 cuesta una determinada cantidad al mes en horas de proceso si la deja ejecutándose todo el tiempo:

![Captura de pantalla de la calculadora de precios en la que se muestra un costo estimado de la máquina virtual Windows A1 al mes](./media/billing-getting-started/pricing-calcvm.png)

Para más información sobre los precios, consulte [Preguntas más frecuentes sobre los precios de Azure](https://azure.microsoft.com/pricing/faq/). Si desea comunicarse con un vendedor de Azure, llame al número de teléfono que se muestra en la parte superior de la página de preguntas más frecuentes.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Revisión de los costos estimados en Azure Portal

Normalmente, cuando se agrega un servicio en Azure Portal, hay una vista que le muestra un costo estimado al mes en la moneda de facturación. Por ejemplo, al elegir el tamaño de la máquina virtual Windows verá el costo mensual estimado de las horas de proceso:

![Ejemplo: una máquina virtual Windows A1 que muestra el costo estimado al mes](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a> Comprobación para saber si tiene un límite de gasto activado

Si tiene una suscripción que usa créditos, el límite de gasto estará activo de forma predeterminada. De esta manera, cuando gasta todos sus créditos, no se le cobra en su tarjeta de crédito. Consulte la [lista completa de ofertas de Azure y la disponibilidad del límite de gasto](https://azure.microsoft.com/support/legal/offer-details/).

Sin embargo, si se alcanza el límite de gasto, se deshabilitarán sus servicios. Esto significa que se desasignarán sus máquinas virtuales. Para evitar el tiempo de inactividad del servicio debe desactivar el límite de gasto. Cualquier uso por encima del límite se cargará en su tarjeta de crédito guardada.

Para ver si tiene activo un límite de gasto, vaya a la [vista de suscripciones del Centro de cuentas](https://account.windowsazure.com/Subscriptions). Aparecerá un mensaje emergente si su límite de gasto está activo, similar al siguiente:

![Captura de pantalla que muestra una advertencia sobre un límite de gasto que está activo en el Centro de cuentas](./media/billing-getting-started/spending-limit-banner.png)

Haga clic en el mensaje emergente y siga las indicaciones para quitar el límite de gasto. Si no facilitó la información de la tarjeta de crédito cuando se suscribió, deberá facilitarla ahora para quitar el límite de gasto. Para más información, consulte [Límite de gasto de Azure: cómo funciona y cómo habilitarlo o quitarlo](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Uso de presupuestos y alertas de costos

Puede crear [presupuestos](../cost-management/tutorial-acm-create-budgets.md) para administrar los costos y crear [alertas](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo.

## <a name="monitor-costs-when-using-azure-services"></a>Supervisión de los costos al usar los servicios de Azure
Puede supervisar los costos con las siguientes herramientas:

- Etiquetas
- Desglose de costos y tasa de evolución
- Análisis de costos

### <a name="tags"></a> Adición de etiquetas a los recursos para agrupar los datos de facturación

Para los servicios compatibles, puede usar etiquetas a fin de agrupar los datos de facturación. Por ejemplo, si ejecuta varias máquinas virtuales para los distintos equipos, puede utilizar etiquetas para clasificar los costos por centro de costo (por ejemplo: Recursos humanos, marketing, finanzas, etc.) o por entorno (por ejemplo: producción, preproducción, prueba).

![Captura de pantalla que muestra la configuración de etiquetas en el portal](./media/billing-getting-started/tags.png)

Las etiquetas se mostrarán a lo largo de las diferentes vistas de informe de costes. Por ejemplo, son visibles directamente en la [vista de análisis de costos](#costs) y en los archivos CSV de uso detallados después del primer período de facturación.

Para obtener más información, vea [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Supervisión del análisis de costos y de la tasa de evolución

Cuando ya estén todos los servicios de Azure en funcionamiento, compruebe los cargos con regularidad. Puede ver el gasto actual y la tasa de evolución en Azure Portal.

1. Vaya a [Suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) y seleccione una suscripción.

2. Si es compatible con su suscripción, verá el desglose de costos y la tasa de evolución.

    ![Captura de pantalla de la tasa de evolución y el análisis de coste en Azure Portal](./media/billing-getting-started/burn-rate.PNG)

3. Haga clic en [Análisis de costos](../cost-management/quick-acm-cost-analysis.md) en la lista de la izquierda para ver el análisis de costos por recursos. Después de agregar un servicio, espere 24 horas para que se muestren los datos.

    ![Captura de pantalla de la vista de análisis de costes en Azure Portal](./media/billing-getting-started/cost-analysis.png)

4. Puede filtrar por diferentes propiedades como [etiquetas](#tags), tipo de recurso, grupo de recursos e intervalo de tiempo. Haga clic en **Aplicar** para confirmar los filtros y en **Descargar** si desea exportar la vista a un archivo de valores separados por comas (.csv).

5. Además, puede hacer clic en un recurso para ver el historial diario de gastos y cuánto cuesta cada día el recurso.

    ![Captura de pantalla de la vista de historial de gastos en Azure Portal](./media/billing-getting-started/costhistory.png)

Compare los costos que ve con las estimaciones que ha visto al seleccionar los servicios. Si los costos difieren significativamente de los valores estimados, compruebe el plan de precios que ha seleccionado para los recursos.

## <a name="optimize-and-reduce-costs"></a>Optimización y reducción de costos
Si no está familiarizado con los principios de la administración de costos, consulte el artículo [Optimización de la inversión en la nube con Azure Cost Management](../cost-management/cost-mgt-best-practices.md).

En Azure Portal, también puede optimizar y reducir los costos de Azure con el apagado automático de las máquinas virtuales y las recomendaciones de Advisor.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Consideración de características de reducción de costos como el apagado automático de las máquinas virtuales

Según el escenario, puede configurar el apagado automático de las VM en Azure Portal. Para más información, consulte [Auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Apagado automático de VM mediante Azure Resource Manager).

![Captura de pantalla de la opción de apagado automático en el portal](./media/billing-getting-started/auto-shutdown.png)

El apagado automático no es lo mismo que cuando apaga la VM desde dentro mediante las opciones de energía. El apagado automático detiene y desasigna las VM para impedir gastos por uso adicionales. Para más información, consulte las preguntas más frecuentes sobre precios de [máquinas virtuales Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [máquinas virtuales Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) y sobre los estados de máquinas virtuales.

Para conocer más medidas de reducción de costes de los entornos de desarrollo y pruebas, visite [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Activación y comprobación de las recomendaciones de Azure Advisor

[Azure Advisor](../advisor/advisor-overview.md) ayuda a reducir los costos al identificar los recursos que se usan poco. Visite Advisor en Azure Portal:

![Captura de pantalla del botón de Azure Advisor en Azure Portal](./media/billing-getting-started/advisor-button.png)

Puede obtener recomendaciones prácticas en la pestaña **Cost** (Costo) del panel de Advisor:

![Captura de pantalla de ejemplo de recomendación de coste de Advisor](./media/billing-getting-started/advisor-action.png)

Consulte el tutorial guiado [Optimización de costos a partir de las recomendaciones](../cost-management/tutorial-acm-opt-recommendations.md) para conocer las recomendaciones de Advisor sobre el ahorro de costos.

## <a name="review-costs-against-your-latest-invoice"></a>Revisión de los costos con la factura más reciente

Al final del ciclo de facturación, la última factura está disponible. También puede [descargar facturas y archivos de uso detallados](billing-download-azure-invoice-daily-usage-date.md) para garantizar que se le cobró correctamente. Para más información sobre cómo comparar el uso diario con la factura, consulte [Entienda su factura de Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>API de facturación

Utilice la API de facturación de Azure para obtener mediante programación los datos de uso. Use la API de RateCard y la API de uso juntas para obtener el uso facturado. Para más información, consulte [Obtención de información sobre el consumo de recursos de Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Recursos adicionales y casos especiales

### <a name="ea-csp-and-sponsorship-customers"></a>Clientes patrocinadores, CSP y EA
Hable con el administrador de la cuenta o el asociado de Azure para conocer más información.

| Oferta | Recursos |
|-------------------------------|-----------------------------------------------------------------------------------|
| Contrato Enterprise (EA) | [Portal EA](https://ea.azure.com/), [documentos de ayuda](https://ea.azure.com/helpdocs) e [informe de Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Proveedor de soluciones en la nube (CSP) | Hable con el proveedor |
| Patrocinio de Azure | [Portal de patrocinio](https://www.microsoftazuresponsorships.com/) |

Si es el administrador de una infraestructura de TI para una organización grande, es recomendable que vea las [plantillas scaffold empresariales de Azure](/azure/architecture/cloud-adoption-guide/subscription-governance) y las [notas del producto TI empresarial](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (descarga en .pdf, solo en inglés).

#### <a name="EA"></a> Vistas de los costos de Contrato Enterprise en Azure Portal

Las vistas de los costos de Enterprise están actualmente en la versión preliminar pública. Elementos que tener en cuenta:

- Los costos de suscripción se basan en el uso y no incluyen los importes de prepago, uso por encima del límite, cantidades incluidas, ajustes ni impuestos. Los cargos reales se calculan en el nivel de inscripción.
- Los importes que se muestran en Azure Portal podrían ser diferentes de lo que está en Enterprise Portal. Las actualizaciones de Enterprise Portal pueden tardar unos minutos antes de que se muestran los cambios en Azure Portal.
- Si no ve los costos, puede deberse a uno de los siguientes motivos:
    - No tiene permisos en el nivel de suscripción. Para ver las vistas de costos de la empresa, debe ser un lector de facturación, un lector, un colaborador o un propietario en el nivel de suscripción.
    - Es el propietario de la cuenta y su administrador de inscripción ha deshabilitado la opción para que el propietario de la cuenta vea los cargos.  Póngase en contacto con el administrador de inscripción para obtener acceso a los costos.
    - Es el administrador del departamento y su administrador de inscripción ha deshabilitado la opción para que el **administrador del departamento vea los cargos**.  Póngase en contacto con el administrador de inscripciones para acceder.
    - Compró Azure a través de un asociado de canal y el asociado no publicó la información sobre precios.  
- Si actualiza la configuración relacionada con el acceso a los costos en Enterprise Portal, hay un retraso de unos minutos antes de que los cambios se muestren en Azure Portal.
- El límite de gasto y la orientación de factura no se aplican a las suscripciones de EA.

### <a name="check-your-subscription-and-access"></a>Comprobación de la suscripción y el acceso

Para ver los costos, debe tener [acceso de nivel de suscripción a la información de facturación](billing-manage-access.md). Solo el administrador de la cuenta puede acceder al [Centro de cuentas](https://account.azure.com/Subscriptions), cambiar la información de facturación y administrar las suscripciones. El administrador de la cuenta es la persona que ha completado el proceso de suscripción. Para más información, consulte [Adición o cambio de roles de administrador de Azure que administran la suscripción o servicios](billing-add-change-azure-subscription-administrator.md).

Para ver si es el administrador de la cuenta, vaya a [Suscripciones en Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Consulte la lista de suscripciones y busque **Mi rol**. Si *Administrador de cuenta* es el rol, tendrá privilegios completos. Si dice algo más, como *Propietario*, no tiene privilegios completos.

![Captura de pantalla de su rol en la vista de suscripciones de Azure Portal](./media/billing-getting-started/sub-blade-view.PNG)

Para administrar las suscripciones y cambiar la información de facturación, [busque al administrador de la cuenta](billing-subscription-transfer.md#whoisaa). Póngase en contacto con el administrador de la cuenta para completar las tareas o que le [transfiera la suscripción](billing-subscription-transfer.md).

Si el administrador de la cuenta ya no está en su organización y necesita administrar la facturación, [póngase en contacto con nosotros](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Solicitud de un crédito del Acuerdo de Nivel de Servicio por un incidente de servicio

El contrato de nivel de servicio explica los compromisos de Microsoft en cuanto a tiempo de actividad y conectividad. Se notifica de un incidente de servicio cuando los servicios de Azure experimentan un problema que afecta al tiempo de actividad o a la conectividad, lo que se suele conocer como *interrupción*. Si no alcanzamos ni mantenemos los niveles de servicio para cada servicio, como se describe en el Acuerdo de Nivel de Servicio, podría optar a un crédito para una parte de las tarifas mensuales por servicio.

Para solicitar un crédito:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/). Si tiene varias cuentas, asegúrese de usar la que resultó afectada por el tiempo de inactividad de Azure. 
2. Cree una nueva solicitud de soporte técnico.
3. En **Tipo de problema** como **Facturación**.
4. En **Tipo de problema**, seleccione **Solicitud de reembolso**.
5. Agregue detalles para especificar que está solicitando un crédito de Acuerdo de Nivel de Servicio, mencione la fecha, hora y zona horaria, así como los servicios afectados (máquinas virtuales, sitios web, etc.).
6. Compruebe los detalles de contacto y seleccione **Crear** para enviar la solicitud.

Los umbrales del Acuerdo de Nivel de Servicio varían según el servicio. Por ejemplo, el nivel web de SQL tiene un Acuerdo de Nivel de Servicio del 99,9 %, las máquinas virtuales tienen un Acuerdo de Nivel de Servicio del 99,95 % y el nivel Estándar de SQL tiene un Acuerdo de Nivel de Servicio del 99,99 %.

Para algunos servicios, hay requisitos previos para que se aplique el Acuerdo de Nivel de Servicio. Por ejemplo, las máquinas virtuales deben tener dos o más instancias implementadas en el mismo conjunto de disponibilidad.

Para más información, consulte [Acuerdos de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/) y la documentación [Resumen de Acuerdos de Nivel de Servicio para servicios de Azure](https://azure.microsoft.com/support/legal/sla/summary/).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre los [límites de gasto](billing-spending-limit.md) para evitar gastos excesivos.
- Comience a [analizar los costos de Azure](../cost-management/quick-acm-cost-analysis.md).
