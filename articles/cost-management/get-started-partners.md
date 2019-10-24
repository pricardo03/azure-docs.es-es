---
title: Introducción a Azure Cost Management para los asociados
description: En este artículo se explica cómo los asociados usan las características de Azure Cost Management y cómo permiten a sus clientes el acceso a Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: 9d95e23cf92c7ee98291831d60088d610c3e5c52
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377433"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>Introducción a Azure Cost Management para los asociados

Azure Cost Management está disponible de forma nativa para los asociados que hayan incorporado a sus clientes a un contrato de cliente de Microsoft. En este artículo se explica cómo los asociados usan las características de [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/). También se describe cómo los asociados permiten a sus clientes el acceso a Cost Management. Los clientes de CSP pueden usar las características de Cost Management cuando su asociado de CSP lo permita.

Los asociados de CSP usan Cost Management para:

- Comprender los costos facturados y asociarlos al cliente, las suscripciones, los grupos de recursos y los servicios.
- Gracias a las funcionalidades para analizar los costos por cliente, suscripción, grupo de recursos, recurso, medidor, servicio y muchas otras dimensiones, puede tener una vista intuitiva de los costos de Azure en el [análisis de costos](quick-acm-cost-analysis.md).
- Vea los costos de los recursos que tienen aplicado un crédito ganado por el asociado (PEC) en el análisis de costos.
- Configure notificaciones y automatización con [presupuestos](tutorial-acm-create-budgets.md) creados mediante programación, junto con alertas cuando los costos superen los presupuestos.
- Habilite la directiva de Azure Resource Manager que proporciona a los clientes acceso a los datos de Cost Management. De este modo, los clientes pueden ver los datos de los costos de consumo de sus suscripciones con las [tarifas de pago por uso](https://azure.microsoft.com/pricing/calculator/).

Toda la funcionalidad disponible en Azure Cost Management también lo está con las API REST. Use las API para automatizar las tareas de administración de costos.

## <a name="prerequisites"></a>Requisitos previos

Azure Cost Management requiere acceso de lectura a la cuenta de facturación o suscripción. El acceso se puede conceder en cualquier nivel por encima de los recursos, desde la cuenta de facturación o un grupo de administración hasta grupos de recursos individuales en los que se administran aplicaciones. Para que los usuarios de la suscripción puedan ver los precios y los costos, el acceso a los cargos debe estar habilitado para la cuenta de facturación. Para más información sobre cómo permitir y asignar el acceso a Azure Cost Management, consulte [Asignación de acceso a los datos](assign-access-acm-data.md). Para ver una lista completa de tipos de cuenta admitidos, consulte [Descripción de los datos de Cost Management](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Uso de los ámbitos por parte de Cost Management

En los ámbitos es donde puede administrar los datos de facturación, tener roles específicos para pagos, ver facturas y realizar la administración de cuentas generales. Los roles de cuenta y facturación se administran de forma independiente de los ámbitos que se usan en la administración de recursos, que emplean RBAC. Para distinguir claramente la finalidad de los distintos ámbitos, incluidas las diferencias en el control de acceso, se hace referencia a ellos como ámbitos de facturación y ámbitos de RBAC, respectivamente.

Para comprender los ámbitos de facturación y los ámbitos de RBAC y cómo funciona la administración de costos con los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>Administración de costos con ámbitos de facturación de inquilinos de asociados

Después de incorporar a los clientes a un contrato de cliente de Microsoft, están disponibles los siguientes _ámbitos de facturación_ en su inquilino. Use los ámbitos para administrar los costos en Cost Management.

### <a name="billing-account-scope"></a>Ámbito de la cuenta de facturación

Use el ámbito de la cuenta de facturación para ver los costos antes de los impuestos de todos los clientes y perfiles de facturación. También puede ver los costos de las facturas de los productos basados en el consumo de los clientes del acuerdo de cliente de Microsoft. Asimismo, se muestran los costos de las facturas de los productos de compra relativos a los clientes del contrato de cliente de Microsoft y de la oferta de CSP. Actualmente, la moneda predeterminada para ver los costos en el ámbito es dólares estadounidenses. Los presupuestos establecidos para el ámbito también están en USD.

Con independencia de la moneda de facturación del cliente, los asociados usan el ámbito para establecer presupuestos y administrar los costos en USD de los clientes, las suscripciones, los recursos y los grupos de recursos.

Los asociados también filtran los costos en una moneda de facturación específica para todos los clientes de la vista de análisis de costos. Seleccione la lista **Costo real** para ver los costos en las monedas de facturación del cliente admitidas.

![Ejemplo que muestra la selección de costos reales para las monedas](./media/get-started-partners/actual-cost-selector.png)

Use la [vista de costo amortizado](quick-acm-cost-analysis.md#customize-cost-views) en los ámbitos de facturación para ver los costos amortizados de instancias reservadas en un plazo de reserva.

### <a name="billing-profile-scope"></a>Ámbito del perfil de facturación

Use el ámbito del perfil de facturación para ver los costos antes de los impuestos en la moneda de facturación relativos a todos los clientes de todos los productos y suscripciones incluidos en una factura. Puede filtrar los costos de un perfil de facturación de una factura específica mediante el filtro **InvoiceID**. El filtro muestra el consumo y los costos de compra de productos de una factura específica. También se pueden filtrar la factura de un cliente concreto para ver los costos antes de los impuestos.

Después de incorporar a los clientes a un contrato de cliente de Microsoft, recibirá una factura de cliente que muestra los cargos por derechos y productos adquiridos, como SaaS, Azure Marketplace y reservas. Cuando se factura en la misma moneda de facturación, la factura también muestra los cargos del cliente que no están incluidos en el nuevo contrato de cliente de Microsoft.

Para conciliar los cargos con la factura del cliente, el ámbito del perfil de facturación le permite ver todos los costos que se acumulan en una factura para sus clientes. Al igual que la factura, el ámbito muestra los costos de cada cliente en el nuevo contrato de cliente de Microsoft. El ámbito también muestra todos los cargos de los productos de derechos de cliente que todavía están en la oferta de CSP actual.

Los ámbitos de perfil de facturación y cuenta de facturación son los únicos que muestran los cargos por derechos y productos de compra.

Los perfiles de facturación definen las suscripciones que se incluyen en una factura. Son el equivalente funcional de una inscripción de contrato Enterprise. Una inscripción es el ámbito en el que se generan las facturas. Igualmente, las compras que no están basadas en el uso, como las de Azure Marketplace y las reservas, solo están disponibles en el ámbito del perfil de facturación.

Actualmente, la moneda de facturación del cliente es la moneda predeterminada cuando se ven los costos en el ámbito del perfil de facturación. Los presupuestos establecidos en este ámbito se encuentran en la moneda de facturación.

Los asociados pueden usar el ámbito para conciliar las facturas. Además, usan el ámbito para establecer los presupuestos en la moneda de facturación de:

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

El ámbito del cliente no incluye a los clientes que se encuentran en la oferta de CSP actual. Los costos de derechos, no los del uso de Azure, de la oferta de CSP actual están disponibles en los ámbitos de la cuenta de facturación y del perfil de facturación cuando se aplica el filtro de cliente.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Acceso de los asociados a los ámbitos de facturación en Cost Management

Solo los usuarios con los roles de **administrador global** y **agente de administrador** pueden administrar y ver los costos de las cuentas de facturación, los perfiles de facturación y los clientes directamente en el inquilino de Azure del asociado. Para más información sobre los roles del Centro de partners, consulte [Asignar roles y permisos de usuario](/partner-center/permissions-overview).

### <a name="enable-cost-management-in-the-customer-tenant"></a>Habilitación de la administración de costos en el inquilino del cliente

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
Ahora, los ámbitos RBAC de grupo de recursos y suscripción tienen a su disposición análisis de costos, presupuestos y alertas según los costos basados en la tarifa de pago por uso.

![Visualización del análisis de costos como cliente ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

Las vistas amortizadas y los costos reales de las instancias reservadas de los ámbitos RBAC muestran cero cargos. Los costos de las instancias reservadas solo se muestran en los ámbitos de facturación en los que se realizaron las compras.

## <a name="analyze-costs-in-cost-analysis"></a>Análisis de los costos en el análisis de costos

Los asociados pueden explorar y analizar los costos del análisis de costos de los clientes y centrarse en un cliente o una factura específicos. Las características de filtro y agrupar por permiten analizar los costos por varios campos, como:

| **Campo** | **Descripción** | **Columna equivalente en el Centro de partners** |
| --- | --- | --- |
| PartnerTenantID | Identificador del inquilino de Azure Active Directory del asociado | TenantID de Azure Active Directory del asociado, conocido como Id. de partner. En formato de GUID. |
| PartnerName | Nombre del inquilino de Azure Active Directory del asociado | Nombre del asociado |
| CustomerTenantID | Identificador del inquilino de Azure Active Directory de la suscripción del cliente | Identificador de la organización del cliente. Por ejemplo, TenantID de Azure Active Directory del cliente. |
| CustomerName | Nombre del inquilino de Azure Active Directory que contiene la suscripción del cliente | Nombre de la organización del cliente, según figura en el Centro de partners. Es importante para la conciliación de la factura con la información del sistema. |
| ResellerMPNID | MPNID del revendedor asociado a la suscripción | Identificador de MPN del revendedor del registro de la suscripción. No está disponible para la actividad actual. |
| Id. de suscripción | Identificador único generado por Microsoft para la suscripción de Azure | N/D |
| subscriptionName | Nombre de la suscripción de Azure | N/D |
| billingProfileID | Identificador del perfil de facturación. Agrupa los costos de las facturas en una única moneda de facturación para todos los clientes. | Identificador del grupo de facturación de asociados de MCAPI. Se usa en las solicitudes de API, pero no se incluye en las respuestas. |
| invoiceID | Identificador de factura en la factura en la que aparece la transacción específica | Número de factura donde aparece la transacción especificada. |
| resourceGroup | Nombre del grupo de recursos de Azure. Se usa para la administración del ciclo de vida de los recursos. | Nombre del grupo de recursos. |
| partnerEarnedCreditRate | Tarifa reducida aplicada si hay un crédito ganado por el asociado (PEC) basado en el acceso al vínculo de administrador del asociado. | La tasa de créditos ganados por el asociado (PEC). Por ejemplo, 0 % o 15 %. |
| partnerEarnedCreditApplied | Indica si se ha aplicado el crédito ganado por el asociado. | N/D |

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

Los asociados, los proveedores indirectos y los clientes pueden usar las API de Cost Management que se describen en las siguientes secciones para tareas comunes.

### <a name="azure-cost-management-apis-for-partners"></a>API de Azure Cost Management para asociados

Los asociados y los usuarios con acceso a los ámbitos de facturación del inquilino de un asociado pueden usar las siguientes API:

#### <a name="to-get-a-list-of-billing-accounts"></a>Para obtener una lista de cuentas de facturación

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>Para obtener una lista de clientes

```
armclient get "providers/Microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers?api-version=2019-10-01-preview"
```
#### <a name="to-get-a-list-of-subscriptions"></a>Para obtener una lista de suscripciones

```
armclient get "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/billingSubscriptions?api-version=2019-10-01-preview"
```

#### <a name="to-create-new-subscription"></a>Para crear suscripciones

```
armclient post "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub.json -verbose
```

#### <a name="to-get-or-download-usage-for-azure-services"></a>Para obtener o descargar el uso de servicios de Azure

```
armclient GET /providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-get-a-list-of-billing-profiles"></a>Para obtener una lista de perfiles de facturación

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/billingProfiles?api-version=2019-10-01-preview
```

#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>Para obtener o descargar la hoja de precios de los servicios de Azure consumidos

```
armclient post "/providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/BillingProfiles/JUT6-EU3Q-BG7-TGB/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

#### <a name="to-get-customer-costs-for-the-last-two-months-sorted-by-month"></a>Para obtener los costos del cliente durante los últimos dos meses, ordenados por mes

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryCustomer.json
```

#### <a name="to-get-azure-subscription-costs-for-the-last-two-months-sorted-by-month"></a>Para obtener los costos de las suscripciones de Azure durante los dos últimos meses, ordenados por mes

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQuerySubscription.json
```

#### <a name="to-get-daily-costs-for-the-current-month"></a>Para obtener los costos diarios del mes en curso

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>Para obtener la directiva para que los clientes vean los costos

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview"
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>Para establecer la directiva para que los clientes vean los costos

```
armclient put "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview" @policy.json
```

### <a name="azure-cost-management-apis-for-indirect-providers"></a>API de Azure Cost Management para proveedores indirectos

Los proveedores indirectos con acceso a los ámbitos RBAC en el inquilino de un cliente pueden usar las siguientes API. Para empezar, inicie sesión con un usuario o con una entidad de servicio.

#### <a name="to-get-the-billing-account-information"></a>Para obtener la información de las cuentas de facturación

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>Para obtener una lista de clientes

```
armclient get "providers/Microsoft.billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-resellers-associated-with-the-customer"></a>Para obtener una lista de los revendedores asociados al cliente

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db?api-version=2019-10-01-preview&$expand=resellers
```

#### <a name="to-get-a-list-of-subscriptions-with-reseller-information"></a>Para obtener una lista de suscripciones con información del revendedor

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-create-a-subscription"></a>Para crear una suscripción

```
armclient post "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub_reseller.json
```

### <a name="azure-cost-management-apis-for-customers"></a>API de Azure Cost Management para clientes

Los clientes usan la siguiente información para acceder a las API. Para empezar, inicie sesión con un usuario.

#### <a name="to-get-or-download-azure-consumption-usage-information-with-retail-rates"></a>Para obtener o descargar información de uso de consumo de Azure con tarifas comerciales

```
armclient post /subscriptions/66bada28-271e-4b7a-aaf5-c0ead63923d7/providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

## <a name="next-steps"></a>Pasos siguientes
- [Inicio del análisis de costos](quick-acm-cost-analysis.md) en Cost Management
- [Creación y administración de presupuestos](tutorial-acm-create-budgets.md) en Cost Management
