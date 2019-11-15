---
title: Introducción a Azure Cost Management para los asociados
description: En este artículo se explica cómo los asociados usan las características de Azure Cost Management y cómo permiten a sus clientes el acceso a Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: 611b3e608d9b0de9423c861ec70e9fc2e7ad67d5
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720756"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>Introducción a Azure Cost Management para los asociados

Azure Cost Management está disponible de forma nativa para los asociados que hayan incorporado a sus clientes a un contrato de cliente de Microsoft. En este artículo se explica cómo los asociados usan las características de [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/). También se describe cómo los asociados permiten a sus clientes el acceso a Cost Management. Los clientes pueden usar las características de Cost Management cuando su asociado de CSP lo permita.

Los asociados de CSP usan Cost Management para:

- Comprender los costos facturados y asociarlos al cliente, las suscripciones, los grupos de recursos y los servicios.
- Gracias a las funcionalidades para analizar los costos por cliente, suscripción, grupo de recursos, recurso, medidor, servicio y muchas otras dimensiones, puede tener una vista intuitiva de los costos de Azure en el [análisis de costos](quick-acm-cost-analysis.md).
- Vea los costos de los recursos que tienen aplicado un crédito ganado por el asociado (PEC) en el análisis de costos.
- Configure notificaciones y automatización con [presupuestos](tutorial-acm-create-budgets.md) creados mediante programación, junto con alertas cuando los costos superen los presupuestos.
- Habilite la directiva de Azure Resource Manager que proporciona a los clientes acceso a los datos de Cost Management. De este modo, los clientes pueden ver los datos de los costos de consumo de sus suscripciones con las [tarifas de pago por uso](https://azure.microsoft.com/pricing/calculator/).

Este es un ejemplo que muestra los costos para todos los clientes.
![Ejemplo que muestra los costos para todos los clientes](./media/get-started-partners/customer-costs1.png)

Este es un ejemplo que muestra los costos para un solo cliente.
![Ejemplo que muestra los costos para un solo cliente](./media/get-started-partners/customer-costs2.png)

Toda la funcionalidad disponible en Azure Cost Management también lo está con las API REST. Use las API para automatizar las tareas de administración de costos.

## <a name="prerequisites"></a>Requisitos previos

Azure Cost Management requiere acceso de lectura a la cuenta de facturación o suscripción. El acceso se puede conceder en cualquier nivel por encima de los recursos, desde la cuenta de facturación o un grupo de administración hasta grupos de recursos individuales en los que se administran aplicaciones. Para más información sobre cómo permitir y asignar el acceso a Azure Cost Management para una cuenta de facturación, consulte [Asignar roles y permisos de usuarios](/partner-center/permissions-overview). Los roles de **administrador global** y **agente de administración** pueden administrar los costos de una cuenta de facturación.

Para ver una lista completa de tipos de cuenta admitidos, consulte [Descripción de los datos de Cost Management](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Uso de los ámbitos por parte de Cost Management

En los ámbitos es donde puede administrar los datos de facturación, tener roles específicos para pagos, ver facturas y realizar la administración de cuentas generales. Los roles de cuenta y facturación se administran de forma independiente de los ámbitos que se usan en la administración de recursos, que emplean RBAC. Para distinguir claramente la finalidad de los distintos ámbitos, incluidas las diferencias en el control de acceso, se hace referencia a ellos como ámbitos de facturación y ámbitos de RBAC, respectivamente.

Para comprender los ámbitos de facturación y los ámbitos de RBAC y cómo funciona la administración de costos con los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>Administración de costos con ámbitos de facturación de inquilinos de asociados

Después de incorporar a los clientes a un contrato de cliente de Microsoft, están disponibles los siguientes _ámbitos de facturación_ en su inquilino. Use los ámbitos para administrar los costos en Cost Management.

### <a name="billing-account-scope"></a>Ámbito de la cuenta de facturación

Use el ámbito de la cuenta de facturación para ver los costos antes de los impuestos de todos los clientes y perfiles de facturación. Los costos de las facturas solo se muestran para los productos basados en el consumo de los clientes que forman parte del contrato de cliente de Microsoft. Sin embargo, se muestran los costos de las facturas de los productos de compra relativos a los clientes tanto del contrato de cliente de Microsoft como de la oferta de CSP. Actualmente, la moneda predeterminada para ver los costos en el ámbito es dólares estadounidenses. Los presupuestos establecidos para el ámbito también están en USD.

Con independencia de las distintas monedas de facturación del cliente, los asociados usan el ámbito de la cuenta de facturación para establecer presupuestos y administrar los costos en USD de los clientes, las suscripciones, los recursos y los grupos de recursos.

Los asociados también filtran los costos en una moneda de facturación específica para todos los clientes de la vista de análisis de costos. Seleccione la lista **Costo real** para ver los costos en las monedas de facturación del cliente admitidas.

![Ejemplo que muestra la selección de costos reales para las monedas](./media/get-started-partners/actual-cost-selector.png)

Use la [vista de costo amortizado](quick-acm-cost-analysis.md#customize-cost-views) en los ámbitos de facturación para ver los costos amortizados de instancias reservadas en un plazo de reserva.

### <a name="billing-profile-scope"></a>Ámbito del perfil de facturación

Use el ámbito del perfil de facturación para ver los costos antes de los impuestos en la moneda de facturación relativos a todos los clientes de todos los productos y suscripciones incluidos en una factura. Puede filtrar los costos de un perfil de facturación de una factura específica mediante el filtro **InvoiceID**. El filtro muestra el consumo y los costos de compra de productos de una factura específica. También se pueden filtrar la factura de un cliente concreto para ver los costos antes de los impuestos.

Una vez que haya incorporado clientes a un contrato de cliente de Microsoft, recibirá una factura que incluirá todos los cargos de todos los productos (consumos, compras y derechos) de estos clientes en relación con el contrato de cliente de Microsoft. Cuando se facturan en la misma moneda, estas facturas también incluyen los cargos por los derechos y productos adquiridos como SaaS, Azure Marketplace, así como las reservas para los clientes que todavía están en la oferta de CSP.

Para conciliar los cargos con la factura del cliente, el ámbito del perfil de facturación le permite ver todos los costos que se acumulan en una factura para sus clientes. Al igual que la factura, el ámbito muestra los costos de cada cliente en el nuevo contrato de cliente de Microsoft. El ámbito también muestra todos los cargos de los productos de derechos de cliente que todavía están en la oferta de CSP actual.

Los ámbitos de perfil de facturación y cuenta de facturación son los únicos ámbitos aplicables que muestran los cargos por derechos y productos de compra relativos, como Azure Marketplace y las compras de reservas.

Los perfiles de facturación definen las suscripciones que se incluyen en una factura. Son el equivalente funcional de una inscripción de contrato Enterprise. Un perfil de facturación es el ámbito en el que se generan las facturas.

Actualmente, la moneda de facturación del cliente es la moneda predeterminada cuando se ven los costos en el ámbito del perfil de facturación. Los presupuestos establecidos en este ámbito se encuentran en la moneda de facturación.

Los asociados pueden usar el ámbito para conciliar las facturas. Además, usan el ámbito para establecer los presupuestos en la moneda de facturación para los siguientes elementos:

- Una factura filtrada específica
- Cliente
- Subscription
- Resource group
- Resource
- Servicio de Azure
- Medidor
- ResellerMPNID

### <a name="customer-scope"></a>Ámbito del cliente

Los asociados usan el ámbito para administrar los costos relacionados con los clientes que se incorporan al contrato de cliente de Microsoft. El ámbito permite a los asociados ver los costos antes de los impuestos de un cliente específico. También se pueden ver los costos antes de los impuestos de una suscripción, un grupos de recursos o un recurso específicos.

El ámbito del cliente no incluye a los clientes que se encuentran en la oferta de CSP actual. El ámbito solo incluye a los clientes que tienen un contrato de cliente de Microsoft. Los costos de derechos, no los del uso de Azure, de la oferta de CSP actual están disponibles en los ámbitos de la cuenta de facturación y del perfil de facturación cuando se aplica el filtro de cliente.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Acceso de los asociados a los ámbitos de facturación en Cost Management

Solo los usuarios con los roles de **administrador global** y **agente de administrador** pueden administrar y ver los costos de las cuentas de facturación, los perfiles de facturación y los clientes directamente en el inquilino de Azure del asociado. Para más información sobre los roles del Centro de partners, consulte [Asignar roles y permisos de usuario](/partner-center/permissions-overview).

## <a name="enable-cost-management-in-the-customer-tenant"></a>Habilitación de la administración de costos en el inquilino del cliente

Los asociados pueden permitir el acceso a Cost Management una vez que los clientes se incorporan a un contrato de cliente de Microsoft. Después, los asociados pueden habilitar una directiva que permita a los clientes ver sus costos calculados según las tarifas para minoristas de pago por uso. Los costos muestran el uso consumido en los ámbitos de suscripción y grupos de recursos de RBAC en la moneda de facturación del cliente.

Cuando el asociado habilita la directiva de visibilidad de costos, cualquier usuario con acceso de Azure Resource Manager a la suscripción puede administrar y analizar los costos según las tarifas de pago por uso. De hecho, los revendedores y los clientes que tienen el acceso RBAC adecuado a las suscripciones de Azure pueden ver los costos.

Con independencia de la directiva, los asociados también pueden ver los costos si tienen acceso a la suscripción y al grupo de recursos.

### <a name="enable-the-policy-to-view-azure-usage-charges"></a>Habilitación de la directiva para ver los cargos de uso de Azure

Los asociados usan la siguiente información para habilitar la directiva con el fin de ver los cargos de uso de Azure de sus clientes.

En Azure Portal, inicie sesión en el inquilino del asociado y haga clic en **Administración de costos + facturación**. Seleccione una cuenta de facturación y, luego, haga clic en **Clientes**. La lista de clientes está asociada a la cuenta de facturación.

En la lista de clientes, seleccione el que quiere que vea los costos.

![Selección de clientes en Cost Management](./media/get-started-partners/customer-list.png)

En **Configuración**, haga clic en **Directivas**.

La directiva actual de visibilidad de los costos se muestra para los cargos de **uso de Azure** asociados a las suscripciones del cliente seleccionado.
![Directiva para permitir que los clientes vean los cargos de pago por uso](./media/get-started-partners/cost-management-billing-policies.png)

Cuando la directiva se establece en **No**, Azure Cost Management no está disponible para los usuarios de suscripciones asociados al cliente. A menos que la habilite un asociado, la directiva de visibilidad de los costos está deshabilitada de forma predeterminada para todos los usuarios de suscripciones.

Cuando esta directiva está establecida en **Sí**, los usuarios de suscripciones asociados al inquilino del cliente pueden ver los cargos de uso según las tarifas de pago por uso.

Cuando la directiva de visibilidad de los costos está habilitada, todos los servicios que tienen la opción de uso de la suscripción muestran los costos según las tarifas de pago por uso. El uso de reservas aparece con cero cargos para los costos reales y amortizados. Las compras y los derechos no están asociados a una suscripción específica. Por lo tanto, las compras no se muestran en el ámbito de la suscripción.

Para ver los costos del inquilino del cliente, abra Cost Management + facturación y haga clic en Cuentas de facturación. En la lista de cuentas de facturación, haga clic en una de ellas.

![Selección de una cuenta de facturación](./media/get-started-partners/select-billing-account.png)

En **Facturación**, haga clic en **Suscripciones de Azure** y, luego, haga clic en un cliente.

![Selección de un cliente de una suscripción de Azure](./media/get-started-partners/subscriptions-select-customer.png)

Haga clic en **Análisis de costos** y comience a revisar los costos.
Los ámbitos RBAC de grupo de recursos y suscripción tienen a su disposición análisis de costos, presupuestos y alertas según los costos basados en la tarifa de pago por uso.

![Visualización del análisis de costos como cliente ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

Las vistas amortizadas y los costos reales de las instancias reservadas de los ámbitos RBAC muestran cero cargos. Los costos de las instancias reservadas solo se muestran en los ámbitos de facturación en los que se realizaron las compras.

## <a name="analyze-costs-in-cost-analysis"></a>Análisis de los costos en el análisis de costos

Los asociados pueden explorar y analizar los costos del análisis de costos de los clientes y centrarse en un cliente o una factura específicos.

Los siguientes campos se encuentran en archivos de detalles de uso y API de Cost Management. Puede usar características de filtro y agrupar por en el análisis de costos para analizar los costos por varios campos. Para ver una lista completa de los campos, consulte [Campo de datos de Cost Management](understand-cost-mgt-data.md#cost-management-data-fields).

| Nombre del campo | DESCRIPCIÓN |
| --- | --- |
| CustomerTenantID | Identificador del inquilino de Azure Active Directory de la suscripción del cliente. |
| CustomerName | Nombre del inquilino de Azure Active Directory de la suscripción del cliente. |
| CustomerTenantDomainName | Nombre de dominio del inquilino de Azure Active Directory de la suscripción del cliente. |
| PartnerTenantID | Identificador del inquilino de Azure Active Directory del asociado. |
| PartnerName | Nombre del inquilino de Azure Active Directory del asociado. |
| ResellerMPNID | MPNID del revendedor asociado a la suscripción. |
| costinUSD | Costo combinado o ExtendedCost estimado antes de impuestos en USD. |
| paygCostInBillingCurrency | Muestra los costos si los precios están en los precios minoristas. Muestra los precios de pago por uso en la moneda de facturación. Solo está disponible en los ámbitos de RBAC. |
| paygCostInUSD | Muestra los costos si los precios están en los precios minoristas. Muestra los precios de pago por uso en USD. Solo está disponible en los ámbitos de RBAC. |
| partnerEarnedCreditRate | Tarifa de descuento aplicada si hay un crédito ganado por el asociado (PEC) basado en el acceso al vínculo de administrador del asociado. |
| partnerEarnedCreditApplied | Indica si se ha aplicado el crédito ganado por el asociado. |

En la vista de [análisis de costos](quick-acm-cost-analysis.md), también puede [guardar las vistas](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) y exportar datos a archivos [CSV y PNG](quick-acm-cost-analysis.md#automation-and-offline-analysis).

## <a name="view-partner-earned-credit-pec-resource-costs"></a>Visualización de los costos de recursos de créditos ganados por el asociado (PEC)

En Azure Cost Management, los asociados pueden usar el análisis de costos para ver los costos que recibieron las ventajas de PEC.

En Azure Portal, inicie sesión en el inquilino del asociado y seleccione **Administración de costos + facturación**. En **Administración de costos**, haga clic en **Análisis de costos**.

La vista de análisis de costos muestra los costos de la cuenta de facturación del asociado. Seleccione el **ámbito** que sea necesario para el asociado, un cliente específico o un perfil de facturación para conciliar las facturas.

En un gráfico de anillos, haga clic en la lista desplegable y seleccione **PartnerEarnedCreditApplied** para profundizar en los costos de PEC.

![Ejemplo que muestra cómo ver el crédito ganado por el asociado](./media/get-started-partners/cost-analysis-pec1.png)

Cuando la propiedad **PartnerEarnedCreditApplied** es _True_, el costo asociado tiene la ventaja del acceso de administrador ganado por el asociado.

Cuando la propiedad **PartnerEarnedCreditApplied** es _False_, el costo asociado no ha satisfecho los requisitos necesarios para el crédito. O bien, el servicio adquirido no reúne los requisitos para ganar créditos de asociado.

Lo habitual es que los datos de uso del servicio tarden entre 8 y 24 horas en aparecer en Cost Management. Para más información, consulte [La frecuencia de actualización de los datos de uso varía](understand-cost-mgt-data.md#usage-data-update-frequency-varies). Los créditos PEC aparecen a las 48 horas desde el momento del acceso en Azure Cost Management.


También puede agrupar y filtrar por la propiedad **PartnerEarnedCreditApplied** mediante las opciones **Agrupar por**. Use las opciones para examinar los costos que tienen y no tienen PEC.

![Agrupación o filtrado por el crédito ganado por el asociado](./media/get-started-partners/cost-analysis-pec2.png)

## <a name="cost-management-rest-apis"></a>API REST de Cost Management

Los asociados y los clientes pueden usar las API de Cost Management que se describen en las siguientes secciones para tareas comunes.

### <a name="azure-cost-management-apis---direct-and-indirect-providers"></a>API de Azure Cost Management: proveedores directos e indirectos

Los asociados con acceso a los ámbitos de facturación en el inquilino de un asociado pueden usar las siguientes API para ver los costos facturados.

Un asociado puede llamar a las API en el ámbito de la suscripción (independientemente de la directiva de costos) si tiene acceso a la suscripción. Otros usuarios con acceso a la suscripción, como el cliente o el revendedor, pueden llamar a las API solo después de que el asociado habilite la directiva de costos para el inquilino del cliente.


#### <a name="to-get-a-list-of-billing-accounts"></a>Para obtener una lista de cuentas de facturación

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-customers"></a>Para obtener una lista de clientes

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-subscriptions"></a>Para obtener una lista de suscripciones

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-invoices-for-a-period-of-time"></a>Para obtener una lista de las facturas de un período de tiempo

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/invoices?api-version=2019-10-01-preview&periodStartDate={periodStartDate}&periodEndDate={periodEndDate}
```

La llamada API devuelve una matriz de facturas que tiene elementos similares al siguiente código JSON.

```
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/{billingAccountID}/billingProfiles/{BillingProfileID}/invoices/{InvoiceID}",
      "name": "{InvoiceID}",
      "properties": {
        "amountDue": {
          "currency": "USD",
          "value": x.xx
        },
        ...
    }
```

Use el valor de campo de identificador devuelto anterior y reemplácelo en el ejemplo siguiente como ámbito para consultar los detalles de uso.

```
GET https://management.azure.com/{id}/providers/Microsoft.Consumption/UsageDetails?api-version=2019-10-01
```

En el ejemplo se devuelven los registros de uso asociados a la factura específica.


#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Para obtener la directiva para que los clientes vean los costos

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Para establecer la directiva para que los clientes vean los costos

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-get-azure-service-usage-for-a-billing-account"></a>Para obtener el uso del servicio de Azure de una cuenta de facturación

```
GET https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-download-a-customers-azure-service-usage"></a>Para descargar el uso del servicio de Azure de un cliente

La siguiente llamada GET es una operación asincrónica.

```
GET https://management.azure.com/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/usageDetails/download?api-version=2019-10-01 -verbose
```

Llame al URI `Location` devuelto en la respuesta para comprobar el estado de la operación. Cuando el estado es *Completado*, la propiedad `downloadUrl` contiene un vínculo que puede usar para descargar el informe generado.


#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Para obtener o descargar la hoja de precios de los servicios de Azure consumidos

En primer lugar, use la siguiente entrada.

```
POST https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

A continuación, llame al valor de la propiedad de operación asincrónica. Por ejemplo:

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheetDownloadOperations/{operation}?sessiontoken=0:11186&api-version=2019-10-01-preview
```
La llamada GET anterior devuelve el vínculo de descarga que contiene la hoja de precios.


#### <a name="to-get-aggregated-costs"></a>Para obtener los costos agregados

```
POST https://management.azure.com/providers/microsoft.billing/billingAccounts/{billingAccountName}/providers/microsoft.costmanagement/query?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-partner"></a>Creación de un presupuesto para un asociado

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/providers/Microsoft.CostManagement/budgets/partnerworkshopbudget?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-customer"></a>Creación de un presupuesto para un cliente

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2019-10-01
```

#### <a name="delete-a-budget"></a>Eliminación de un presupuesto

```
PUT
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/budgets/{budgetName}?api-version=2019-10-01
```


## <a name="next-steps"></a>Pasos siguientes
- [Inicio del análisis de costos](quick-acm-cost-analysis.md) en Cost Management
- [Creación y administración de presupuestos](tutorial-acm-create-budgets.md) en Cost Management
