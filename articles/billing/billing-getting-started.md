---
title: Evitar costos inesperados y administrar la facturación en Azure | Microsoft Docs
description: Obtenga información acerca de cómo evitar cargos inesperados en la factura de Azure. Use características de administración y seguimiento del costo de las suscripciones a Microsoft Azure.
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2018
ms.author: banders
ms.openlocfilehash: d215e2ba5d650b532b9d7554ccca9ad5537cf4de
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842520"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Prevención de cargos inesperados con la administración de costos y facturación de Azure

Al suscribirse a Azure, hay varias cosas que puede hacer para obtener una mejor idea de los gastos. La [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) puede proporcionar una estimación de los costos antes de crear un recurso de Azure. [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) proporciona el desglose de costos actual y la previsión correspondientes a la suscripción. Si desea agrupar y comprender los costos para distintos proyectos o equipos, consulte el [etiquetado de recursos](../azure-resource-manager/resource-group-using-tags.md). Si prefiere usar el sistema de informes del que dispone su organización, compruebe las [API de facturación](billing-usage-rate-card-overview.md).

- Si la suscripción es un Contrato Enterprise (EA), la versión preliminar pública para ver los costos en Azure Portal está disponible. Si realiza la suscripción a través de un Proveedor de soluciones en la nube (CSP) o Patrocinio de Azure, no podrá aplicar algunas de las siguientes características. Vea [Recursos adicionales para EA, CSP y Patrocinio](#other-offers) para obtener más información.

- Si la suscripción es una evaluación gratuita, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure bajo licencia Open (AIO) o BizSpark, la suscripción se deshabilitará automáticamente cuando se usen todos los créditos. Obtenga más información acerca de los [límites de gasto](#spending-limit) para evitar que la suscripción se deshabilite inesperadamente.

- Si se ha registrado para obtener una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), [puede utilizar algunos de los servicios de Azure más populares de forma gratuita durante 12 meses](billing-create-free-services-included-free-account.md). Junto con las recomendaciones que se enumeran a continuación, consulte [Evitar cargos en su cuenta gratuita de Azure](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Obtención de costos estimados antes de agregar servicios de Azure

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Estimación del coste en línea con la calculadora de precios

Consulte la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para obtener un costo mensual estimado del servicio que le interesa. Puede agregar cualquier recurso de Azure propio para obtener un costo estimado.

![Captura de pantalla del menú de la calculadora de precios](./media/billing-getting-started/pricing-calc.png)

Por ejemplo, se estima que una máquina virtual Windows A1 cuesta 66,96 dólares al mes en horas de proceso si la deja ejecutándose todo el tiempo:

![Captura de pantalla de la calculadora de precios en la que se estima que una máquina virtual Windows A1 cuesta 66,96 dólares al mes](./media/billing-getting-started/pricing-calcVM.png)

Para más información sobre los precios, consulte estas [preguntas más frecuentes](https://azure.microsoft.com/pricing/faq/). O bien, si desea comunicarse con un vendedor de Azure, llame al 1-800-867-1389.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Revisión del costo estimado en Azure Portal

Normalmente, cuando se agrega un servicio en Azure Portal, hay una vista que le muestra un coste estimado similar al mes. Por ejemplo, al elegir el tamaño de la máquina virtual Windows verá el costo mensual estimado de las horas de proceso:

![Ejemplo: se estima que una máquina virtual Windows A1 cuesta 66,96 dólares al mes](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="spending-limit"></a> Comprobación para saber si tiene un límite de gasto activado

Si tiene una suscripción que usa créditos, el límite de gasto estará activo de forma predeterminada. De esta manera, cuando gasta todos sus créditos, no se le cobra en su tarjeta de crédito. Consulte la [lista completa de ofertas de Azure y la disponibilidad del límite de gasto](https://azure.microsoft.com/support/legal/offer-details/).

Sin embargo, si se alcanza el límite de gasto, se deshabilitarán sus servicios. Esto significa que se desasignarán sus máquinas virtuales. Para evitar el tiempo de inactividad del servicio debe desactivar el límite de gasto. Cualquier uso por encima del límite se cargará en su tarjeta de crédito guardada. 

Para ver si tiene activo un límite de gasto, vaya a la [vista de suscripciones del Centro de cuentas](https://account.windowsazure.com/Subscriptions). Aparecerá un mensaje emergente si su límite de gasto está activo:

![Captura de pantalla que muestra una advertencia sobre un límite de gasto que está activo en el Centro de cuentas](./media/billing-getting-started/spending-limit-banner.PNG)

Haga clic en el mensaje emergente y siga las indicaciones para quitar el límite de gasto. Si no facilitó la información de la tarjeta de crédito cuando se suscribió, deberá facilitarla ahora para quitar el límite de gasto. Para más información, consulte [Límite de gasto de Azure: cómo funciona y cómo habilitarlo o quitarlo](https://azure.microsoft.com/pricing/spending-limits/).

Puede usar el servicio [Cloudyn](https://www.cloudyn.com/) para crear alertas que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Se pueden crear alertas mediante informes que admitan alertas basadas en umbrales de presupuesto y de costo. Para obtener más información sobre el uso de Cloudyn, consulte el [Tutorial: Revisión del uso y los costos](../cost-management/tutorial-review-usage.md).

En este ejemplo se usa el informe **Actual Cost Over Time** (Costo real a lo largo del tiempo) para enviar una notificación cuando el gasto de una máquina virtual de Azure se acerca al presupuesto total. En este escenario, tiene un presupuesto total de 20 000 USD y quiere recibir una notificación cuando los costos se aproximen a la mitad del presupuesto (9000 USD) y una alerta adicional cuando los costos lleguen a 10 000 USD.

1. En el menú de la parte superior del portal de Cloudyn, seleccione **Costs** > **Cost Analysis** > **Actual Cost Over Time** (Costos > Análisis de costos > Costo real a lo largo del tiempo). 
2. Establezca **Grupos** en **Servicio** y **Filter on the service (Filtrar el servicio)** en **Azure/VM**. 
3. En la parte superior derecha del informe, seleccione **Actions** (Acciones) y, después, seleccione **Schedule report** (Programar informe).
4. Para enviarse un correo electrónico del informe a intervalos programados, seleccione la pestaña **Scheduling** (Programación) en el cuadro de diálogo **Save or Schedule this report** (Guardar o programar este informe). Asegúrese de seleccionar **Send via email** (Enviar por correo electrónico). Todas las etiquetas, la agrupación y los filtros que use se incluyen en el informe enviado por correo electrónico. 
5. Seleccione la pestaña **Threshold** (Umbral) y después seleccione **Actual Cost vs. Threshold (Costo real frente a umbral)**. 
   1. En el cuadro de umbral **Red alert** (Alerta roja) escriba 10000. 
   2. En el cuadro del umbral **Yellow alert** (Alerta amarilla) escriba 9000. 
   3. En el cuadro **Number of consecutive alerts** (Número de alertas consecutivas), escriba el número de alertas consecutivas que desea recibir. Cuando recibe el número total de alertas especificado, no se envían más alertas. 
6. Seleccione **Guardar**.

    ![Ejemplo que muestra alertas rojas y amarillas según los umbrales de gastos](./media/billing-getting-started/schedule-alert01.png)

También puede elegir la métrica de umbral **Cost Percentage vs. Budget** (Porcentaje de costo frente a presupuesto) para crear alertas. Esto le permite especificar los umbrales como porcentajes del presupuesto,en lugar de los valores de moneda.

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>Formas de supervisar los costos cuando se usan servicios de Azure

### <a name="tags"></a> Incorporación de etiquetas a los recursos para agrupar los datos de facturación

Para los servicios compatibles, puede usar etiquetas a fin de agrupar los datos de facturación. Por ejemplo, si ejecuta varias máquinas virtuales para los distintos equipos, puede utilizar etiquetas para clasificar los costes por centro de coste (Recursos humanos, Marketing, Finanzas) o por entorno (producción, preproducción, pruebas). 

![Captura de pantalla que muestra la configuración de etiquetas en el portal](./media/billing-getting-started/tags.PNG)

Las etiquetas se mostrarán a lo largo de las diferentes vistas de informe de costes. Por ejemplo, son visibles directamente en la [vista de análisis de costes](#costs) y en los archivos .csv de uso detallados después del primer período de facturación.

Para obtener más información, vea [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Visite el portal con regularidad para conocer el análisis de costes y la tasa de evolución

Después de ejecutar los servicios, compruebe regularmente el coste de estos. Puede ver el gasto actual y la tasa de evolución en Azure Portal.

1. Vaya a [Suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) y seleccione una suscripción.

2. Si es compatible con su suscripción, verá el desglose de costos y la tasa de evolución.

    ![Captura de pantalla de la tasa de evolución y el análisis de coste en Azure Portal](./media/billing-getting-started/burn-rate.PNG)

3. Haga clic en **Análisis de costes** en la lista de la izquierda para ver el análisis de costes por recursos. Espere 24 horas después de agregar un servicio para que se rellenen los datos.

    ![Captura de pantalla de la vista de análisis de costes en Azure Portal](./media/billing-getting-started/cost-analysis.PNG)

4. Puede filtrar por diferentes propiedades como [etiquetas](#tags), tipo de recurso, grupo de recursos e intervalo de tiempo. Haga clic en **Aplicar** para confirmar los filtros y en **Descargar** si desea exportar la vista a un archivo de valores separados por comas (.csv).

5. Además, puede hacer clic en un recurso para ver el historial diario de gastos y cuánto cuesta cada día el recurso.

    ![Captura de pantalla de la vista de historial de gastos en Azure Portal](./media/billing-getting-started/costhistory.PNG)

Le recomendamos que compare los costes reales con las estimaciones que vio cuando seleccionó los servicios. Si los costes difieren mucho de los valores estimados, debe comprobar el plan de precios que ha seleccionado para los recursos.

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Habilitación de medidas de reducción de costes como el apagado automático de las VM

Según el escenario, puede configurar el apagado automático de las VM en Azure Portal. Para más información, consulte [Auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Apagado automático de VM mediante Azure Resource Manager).

![Captura de pantalla de la opción de apagado automático en el portal](./media/billing-getting-started/auto-shutdown.PNG)

El apagado automático no es lo mismo que cuando apaga la VM desde dentro mediante las opciones de energía. El apagado automático detiene y desasigna las VM para impedir gastos por uso adicionales. Para más información, consulte las preguntas más frecuentes sobre precios de [máquinas virtuales Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [máquinas virtuales Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) y sobre los estados de máquinas virtuales.

Para conocer más medidas de reducción de costes de los entornos de desarrollo y pruebas, visite [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Active y compruebe las recomendaciones de Azure Advisor

[Azure Advisor](../advisor/advisor-overview.md) es una característica que ayuda a reducir los costos identificando los recursos que se usan poco. Visite Advisor en Azure Portal:

![Captura de pantalla del botón de Azure Advisor en Azure Portal](./media/billing-getting-started/advisor-button.PNG)

Puede obtener recomendaciones prácticas sobre en la pestaña **Cost** (Costo) del panel de Advisor:

![Captura de pantalla de ejemplo de recomendación de coste de Advisor](./media/billing-getting-started/advisor-action.PNG)

Para obtener más información, consulte las [recomendaciones sobre el costo de Advisor](../advisor/advisor-cost-recommendations.md).

## <a name="reviewing-costs-at-the-end-of-your-billing-cycle"></a>Revisión de los costos al final del ciclo de facturación

Después del final del ciclo de facturación, la factura estará disponible. También puede [descargar facturas anteriores y archivos de uso de detalle](billing-download-azure-invoice-daily-usage-date.md) para garantizar que se le cobró correctamente. Para más información sobre cómo comparar el uso diario con la factura, consulte [Entienda su factura de Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>API de facturación

Utilice la API de facturación para obtener mediante programación los datos de uso. Use la API de RateCard y la API de uso juntas para obtener el uso facturado. Para más información, consulte [Obtención de información sobre el consumo de recursos de Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Recursos adicionales y casos especiales

### <a name="ea-csp-and-sponsorship-customers"></a>Clientes patrocinadores, CSP y EA
Hable con el administrador de la cuenta o el asociado de Azure para conocer más información.

| Oferta | Recursos |
|-------------------------------|-----------------------------------------------------------------------------------|
| Contrato Enterprise (EA) | [Portal EA](https://ea.azure.com/), [documentos de ayuda](https://ea.azure.com/helpdocs) e [informe de Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Proveedor de soluciones en la nube (CSP) | Hable con el proveedor |
| Patrocinio de Azure | [Portal de patrocinio](https://www.microsoftazuresponsorships.com/) |

Si es el administrador de una infraestructura de TI para una organización grande, es recomendable que vea las [plantillas scaffold empresariales de Azure](/azure/architecture/cloud-adoption-guide/subscription-governance) y las [notas del producto TI empresarial](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (descarga en .pdf, solo en inglés).

#### <a name="EA"></a>Vistas de los costos del Contrato Enterprise de la versión preliminar dentro de Azure Portal 

Las vistas de los costos de Enterprise están actualmente en la versión preliminar pública. Elementos que tener en cuenta:

- Los costos de suscripción se basan en el uso y no incluyen los importes de prepago, uso por encima del límite, cantidades incluidas, ajustes ni impuestos. Los cargos reales se calculan en el nivel de inscripción.
- Los importes que se muestran en Azure Portal podrían ser diferentes de lo que está en Enterprise Portal. Las actualizaciones de Enterprise Portal pueden tardar unos minutos antes de que se muestran los cambios en Azure Portal.
- Si no ve los costos, puede deberse a uno de los siguientes motivos:
    - No tiene permisos en el nivel de suscripción. Para ver las vistas de costos de la empresa, debe ser un lector de facturación, un lector, un colaborador o un propietario en el nivel de suscripción.
    - Es el propietario de la cuenta y su administrador de inscripción ha deshabilitado la opción para que el propietario de la cuenta vea los cargos.  Póngase en contacto con el administrador de inscripción para obtener acceso a los costos. 
    - Es el administrador del departamento y su administrador de inscripción ha deshabilitado la opción para que el administrador del departamento vea los cargos.  Póngase en contacto con el administrador de inscripciones para acceder.
    - Compró Azure a través de un asociado de canal y el asociado no publicó la información sobre precios.  
- Si actualiza la configuración relacionada con el acceso a los costos en Enterprise Portal, hay un retraso de unos minutos antes de que los cambios se muestren en Azure Portal.
- El límite de gasto y la orientación de factura no se aplican a las suscripciones de EA.

### <a name="check-your-subscription-and-access"></a>Comprobación de la suscripción y el acceso

Para ver los costos, debe tener [acceso de nivel de suscripción a la información de facturación](billing-manage-access.md). Solo el administrador de la cuenta puede acceder al [Centro de cuentas](https://account.azure.com/Subscriptions), cambiar la información de facturación y administrar las suscripciones. El administrador de la cuenta es la persona que ha completado el proceso de suscripción. Para más información, consulte [Adición o cambio de roles de administrador de Azure que administran la suscripción o servicios](billing-add-change-azure-subscription-administrator.md).

Para ver si es el administrador de la cuenta, vaya a [Suscripciones en Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Eche un vistazo a la lista de suscripciones a las que tiene acceso. Busque en **Mi rol**. Si dice *Administrador de cuenta*, entonces, todo correcto. Si dice algo más como *Propietario*, entonces no tiene privilegios completos.

![Captura de pantalla de su rol en la vista de suscripciones de Azure Portal](./media/billing-getting-started/sub-blade-view.PNG)

Si no es el administrador de la cuenta, posiblemente alguien le dio acceso parcial mediante el [control de acceso basado en rol de Azure Active Directory](../role-based-access-control/role-assignments-portal.md) (RBAC). Para administrar las suscripciones y cambiar la información de facturación, [busque al administrador de la cuenta](billing-subscription-transfer.md#whoisaa). Póngase en contacto con el administrador de la cuenta para realizar las tareas o que le [transfiera la suscripción](billing-subscription-transfer.md).

Si el administrador de la cuenta ya no está en su organización y necesita administrar la facturación, [póngase en contacto con nosotros](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="how-to-request-a-service-level-agreement-credit-for-a-service-incident"></a>Cómo solicitar un crédito de contrato de nivel de servicio para un incidente de servicio

El contrato de nivel de servicio explica los compromisos de Microsoft en cuanto a tiempo de actividad y conectividad. Un incidente de servicio se comunica cuando Servicios de Azure experimentan un problema que el tiempo de actividad impactos o conectividad, a menudo se denomina "interrupción". Si no conseguir y mantener los niveles de servicio para cada servicio, como se describe en el SLA, a continuación, puede ser apto para un crédito por una parte de las tarifas de servicio mensual.

Para solicitar un crédito:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/). Si tiene varias cuentas, asegúrese de utilizar uno que se vio afectado por el tiempo de inactividad de Azure. Esto ayuda a soporte técnico a recopilar automáticamente la información necesaria en segundo plano y resolver el caso más rápido.
2. Crear una nueva solicitud de soporte técnico.
3. En **tipo de problema**, seleccione **facturación**.
4. En **tipo de problema**, seleccione **solicitud de reembolso**.
5. Agregar detalles para especificar que se está solicitando un crédito de SLA, mencione la fecha/hora/zona horaria, así como los servicios afectados (máquinas virtuales, sitios Web, etcetera.)
6. Compruebe los detalles de contacto y seleccione el **crear** botón para enviar la solicitud.

Los umbrales del SLA varían según el servicio. Por ejemplo, nivel de Web de SQL tiene un SLA del 99,9%, las máquinas virtuales tienen un SLA del 99,95% y el nivel estándar de SQL tiene un SLA del 99,99%.

Para algunos servicios, existen requisitos previos para el SLA aplicar. Por ejemplo, las máquinas virtuales debe tener dos o más instancias implementadas en el mismo conjunto de disponibilidad.

Para obtener más información, consulte el [acuerdos de nivel de servicio](https://azure.microsoft.com/en-us/support/legal/sla/) documentación y el [resumen del SLA de servicios de Azure](https://azure.microsoft.com/en-us/support/legal/sla/summary/) documentación.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
