---
title: 'Migrar de contrato Enterprise a la API de acuerdo al cliente de Microsoft: Azure | Microsoft Docs'
description: En este artículo le ayudará a comprender las consecuencias de la migración de un contrato Enterprise (EA) a un contrato de cliente de Microsoft.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 283808c0bd3f5297011b25619d6f978c99d4dc32
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439233"
---
# <a name="migrate-from-enterprise-agreement-to-microsoft-customer-agreement-apis"></a>Migrar de contrato Enterprise a las API de acuerdo al cliente de Microsoft

En este artículo le ayudará a comprender la estructura de datos, API y otras diferencias de integración de sistema entre cuentas de contrato Enterprise (EA) y contrato de cliente de Microsoft (MCA). Azure Cost Management es compatible con las API para ambos tipos de cuenta. Revise el [configurar la cuenta de facturación para](../billing/billing-mca-setup-account.md) artículo de contrato de cliente de Microsoft antes de continuar.

Las organizaciones con una cuenta EA existente deben revisar este artículo, junto con la configuración de una cuenta MCA. Anteriormente, era necesario algún trabajo mínimo para pasar de una inscripción anterior a una nueva renovación de una cuenta EA. Sin embargo, la migración a una cuenta MCA requiere un esfuerzo adicional. Esfuerzo adicional es debido a cambios en el subsistema de facturación subyacente, lo que afecta a todas las API relacionadas con el costo y las ofertas de servicio.

## <a name="mca-apis-and-integration"></a>Las API de MCA e integración

Las API de MCA y nueva integración le permiten:

- Tener disponibilidad completa de API a través de las API nativas de Azure.
- Configurar varias facturas en una sola cuenta de facturación.
- Tener acceso a una API combinada con el uso del servicio de Azure, el uso de Marketplace de terceros y las compras de Marketplace.
- Ver los costos a través de perfiles (igual que las inscripciones) de facturación con Azure Cost Management.
- Nuevas API de acceso para mostrar los costos y obtener notificaciones cuando los costos superan los umbrales predefinidos y exportación datos sin procesar de forma automática.

## <a name="migration-checklist"></a>Lista de comprobación de migración

La Ayuda de los elementos siguientes que realiza la transición a las API de MCA.

- Familiarícese con la nueva [cuenta de facturación de contrato de cliente de Microsoft](../billing/billing-mca-overview.md).
- Determine qué API usar y vea cuáles se reemplazan en la sección siguiente.
- Familiarícese con [API de REST de Azure Resource Manager](/rest/api/azure).
- Si ya no usa las API de Azure Resource Manager, [registrar la aplicación cliente con Azure AD](/rest/api/azure/#register-your-client-application-with-azure-ad).
- Actualice cualquier código de programación para [usar autenticación de Azure AD](/rest/api/azure/#create-the-request).
- Actualice cualquier código de programación para reemplazar las llamadas de API de EA con llamadas a API MCA.
- Actualizar para usar los nuevos códigos de error de control de errores.
- Revise las ofertas de integración adicionales, como la acción de necesitan Cloudyn y Power BI, para otros.

## <a name="ea-apis-replaced-with-mca-apis"></a>Las API de EA se reemplazan por las API de MCA

Las API de EA usan una clave de API para la autenticación y autorización. Las API de MCA usan autenticación de Azure AD.

| Propósito | EA API | MCA API |
| --- | --- | --- |
| Saldo y créditos | [/balancesummary](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) | Microsoft.Billing/billingAccounts/billingProfiles/availableBalanceussae |
| Uso (JSON) | [/usagedetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)[/usagedetailsbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format) | [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Uso (CSV) | [descarga/usagedetails/](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format) [ /usagedetails/enviar](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Uso de Marketplace (CSV) | [/marketplacecharges](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)[/marketplacechargesbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Períodos de facturación | [/billingperiods](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Hoja de precios | [/pricesheet](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) | Microsoft.Billing/billingAccounts/billingProfiles/pricesheet/default/download format=json|CSV Microsoft.Billing/billingAccounts/.../billingProfiles/.../invoices/... formato /pricesheet/default/download = json|CSV Microsoft.Billing/billingAccounts/... / billingProfiles /.. /Providers/Microsoft.Consumption/pricesheets/download  |
| Compras de reserva | [/reservationcharges](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) | Microsoft.Billing/billingAccounts/billingProfiles/transactions |
| Recomendaciones de reserva | [/SharedReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)[/](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)[SingleReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations) | [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) |
| Uso de reserva | [/reservationdetails](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)[/reservationsummaries](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary) | [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails)[Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) |

<sup>1</sup> servicio de azure y el uso de Marketplace de terceros están disponibles con el [API de detalles de uso](/rest/api/consumption/usagedetails).

Las API siguientes están disponibles para las cuentas de facturación de MCA:

| Propósito | API de cliente de Microsoft (MCA) de acuerdo |
| --- | --- |
| Las cuentas de facturación<sup>2</sup> | Microsoft.Billing/billingAccounts |
| Perfiles de facturación<sup>2</sup> | Microsoft.Billing/billingAccounts/billingProfiles |
| Secciones de factura<sup>2</sup> | Microsoft.Billing/billingAccounts/invoiceSections |
| Facturas | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Facturación de suscripciones | {scope}/billingSubscriptions |

<sup>2</sup> API devuelven listas de objetos, que son ámbitos, donde las experiencias de administración de costos en el portal de Azure y las API funcionan. Para obtener más información sobre los ámbitos de Cost Management, consulte [entender y trabajar con los ámbitos](understand-work-scopes.md).

Si usa las API de EA existente, deberá actualizarlos para admitir cuentas de facturación MCA. En la tabla siguiente se muestra otros cambios de integración:

| Propósito | Oferta anterior | Nueva oferta |
| --- | --- | --- |
| Cloudyn | [Cloudyn.com](https://www.cloudyn.com) | [Azure Cost Management](https://azure.microsoft.com/services/cost-management/) |
| Power BI | [Microsoft Consumption Insights](/power-bi/desktop-connect-azure-consumption-insights) pack y el conector de contenido | [Aplicación de Power BI de Microsoft Azure consumo Insights](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview) y [ conector Azure Consumption Insights](/power-bi/desktop-connect-azure-consumption-insights) |

## <a name="apis-to-get-balance-and-credits"></a>API para obtener el saldo y créditos

El [obtener el resumen de saldo](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) API le proporciona un resumen mensual de:

- Saldos
- Nuevas compras
- Gastos de servicios de Azure Marketplace
- Ajustes
- Cargos de uso del servicio por encima del límite

Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request).

La API de resumen del saldo Get se sustituye por la API Microsoft.Billing/billingAccounts/billingProfiles/availableBalance.

Para obtener los saldos disponibles con la API de saldo disponible:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/availableBalances?api-version=2018-11-01-preview` |

## <a name="apis-to-get-cost-and-usage"></a>API para obtener el costo y uso

Obtener un desglose diario de los costos de uso del servicio de Azure, el uso de Marketplace de terceros y otras las compras de Marketplace con las API siguientes. Las siguientes API independientes se combinaron para servicios de Azure y el uso de Marketplace de terceros. Las API anteriores se reemplazan por el [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails) API. Agrega las compras de Marketplace, que estaban previamente solo se muestra en el saldo resumen hasta la fecha.

- [Obtener detalles de uso y descarga](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Obtener detalles de uso o enviar](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Obtener detalles de uso/usagedetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Obtener detalles de uso/usagedetailsbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Obtener almacén cargo/marketplacecharges de marketplace](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)
- [Obtener almacén cargo/marketplacechargesbycustomdate de marketplace](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)

Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request).

Todas las API anteriores se reemplazan por la API de detalles de uso y el consumo.

Para obtener detalles de uso con la API de detalles de uso:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?api-version=2019-01-01` |

La API de detalles de uso, al igual que con todas las API de administración de costos, está disponible en varios ámbitos. Costos facturados, como se recibiría tradicionalmente en el nivel de inscripción, use el ámbito de perfil de facturación.  Para obtener más información sobre los ámbitos de Cost Management, consulte [entender y trabajar con los ámbitos](understand-work-scopes.md).

| Type | Formato de Id. |
| --- | --- |
| Cuenta de facturación | `/Microsoft.Billing/billingAccounts/{billingAccountId}` |
| Perfil de facturación | `/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}` |
| Subscription | `/subscriptions/{subscriptionId}` |
| Grupos de recursos | `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}` |

Utilice los siguientes parámetros de cadena de consulta para actualizar el código de programación.

| Parámetros antiguos | Nuevos parámetros |
| --- | --- |
| `billingPeriod={billingPeriod}` | No compatible |
| `endTime=yyyy-MM-dd` | `endDate=yyyy-MM-dd` |
| `startTime=yyyy-MM-dd` | `startDate=yyyy-MM-dd` |

También puede cambiar el cuerpo de la respuesta.

Cuerpo de la respuesta anterior:

```
{
  "id": "string",
  "data": [{...}, ...],
  "nextLink": "string"
}
```

Nuevo cuerpo de respuesta:

```
{
  "value": [{
    "id": "{scope}/providers/Microsoft.Consumption/usageDetails/###",
    "name": "###",
    "type": "Microsoft.Consumption/usageDetails",
    "tags": {...},
    "properties": [{...}, ...],
    "nextLink": "string"
  }, ...]
}
```

Cambiar el nombre de propiedad que contiene la matriz de registros de uso de datos a _valores_. Cada registro que se utiliza para tener una lista plana de propiedades detalladas. Sin embargo, cada registro ahora todos los detalles están ahora en una propiedad anidada denominada _propiedades_, excepto las etiquetas. La nueva estructura es coherente con otras API de Azure. Algunos nombres de propiedad han cambiado. La siguiente tabla muestra las propiedades correspondientes.

| Propiedad anterior | Nueva propiedad | Notas |
| --- | --- | --- |
| Id. de la cuenta | N/D | El creador de la suscripción no está registrado. Utilice invoiceSectionId (igual que departmentId). |
| AccountNameAccountOwnerId y AccountOwnerEmail | N/D | El creador de la suscripción no está registrado. Utilice invoiceSectionName (igual que departmentName). |
| Información adicional | additionalInfo | &nbsp;  |
| ChargesBilledSeparately | isAzureCreditEligible | Tenga en cuenta que estas propiedades son opuestos. Si es true isAzureCreditEnabled, ChargesBilledSeparately sería false. |
| ConsumedQuantity | quantity | &nbsp; |
| Servicio consumido | consumedService | Pueden diferir los valores de cadena exacto. |
| Id. del servicio consumido | None | &nbsp; |
| CostCenter | costCenter | &nbsp; |
| Fecha y usageStartDate | fecha | &nbsp;  |
| Día | None | Analiza el día de la fecha. |
| Id. de departamento | invoiceSectionId | Difieren de los valores exactos. |
| DepartmentName | invoiceSectionName | Pueden diferir los valores de cadena exacto. Configure las secciones de la factura para que coincida con los departamentos de TI, si es necesario. |
| ExtendedCost y costo | costInBillingCurrency | &nbsp;  |
| InstanceId | ResourceId | &nbsp;  |
| Es un cargo periódico | None | &nbsp;  |
| Ubicación | location | &nbsp;  |
| Categoría del medidor | meterCategory | Pueden diferir los valores de cadena exacto. |
| Id. del medidor | meterId | Difieren de los valores de cadena exacto. |
| Nombre del medidor | meterName | Pueden diferir los valores de cadena exacto. |
| Región del medidor | meterRegion | Pueden diferir los valores de cadena exacto. |
| Subcategoría del medidor | meterSubCategory | Pueden diferir los valores de cadena exacto. |
| Mes | None | Analiza el mes de fecha. |
| Nombre de la oferta | None | Use publisherName y productOrderName. |
| OfferId | None | &nbsp;  |
| Número de pedido | None | &nbsp;  |
| PartNumber | None | Use meterId y productOrderName para identificar de forma exclusiva los precios. |
| Nombre del plan | productOrderName | &nbsp;  |
| Producto | Producto |   |
| ProductId | productId | Difieren de los valores de cadena exacto. |
| Nombre de publicador | publisherName | &nbsp;  |
| ResourceGroup | resourceGroupName | &nbsp;  |
| ResourceGuid | meterId | Difieren de los valores de cadena exacto. |
| Ubicación de los recursos | resourceLocation | &nbsp;  |
| Id. de ubicación de recursos | None | &nbsp;  |
| Tasa de recursos | effectivePrice | &nbsp;  |
| Id. de administrador de servicio | N/D | &nbsp;  |
| Información del servicio 1 | serviceInfo1 | &nbsp;  |
| Información del servicio 2 | serviceInfo2 | &nbsp;  |
| ServiceName | meterCategory | Pueden diferir los valores de cadena exacto. |
| ServiceTier | meterSubCategory | Pueden diferir los valores de cadena exacto. |
| Identificador del servicio del almacén | N/D | &nbsp;  |
| Ubicación de los recursos | subscriptionId | &nbsp;  |
| SubscriptionId | subscriptionId | &nbsp;  |
| SubscriptionName | subscriptionName | &nbsp;  |
| Etiquetas | etiquetas | La propiedad tags se aplica al objeto raíz, no a la propiedad de las propiedades anidadas. |
| Unidad de medida | unitOfMeasure | Difieren de los valores de cadena exacto. |
| usageEndDate | fecha | &nbsp;  |
| Year | None | Analiza el año de fecha. |
| (nueva) | billingCurrency | Moneda usada para la carga. |
| (nueva) | billingProfileId | Identificador único para el perfil de facturación (igual que la inscripción). |
| (nueva) | billingProfileName | Nombre del perfil de facturación (igual que la inscripción). |
| (nueva) | chargeType | Se usa para diferenciar el uso del servicio de Azure, el uso de Marketplace y compras. |
| (nueva) | invoiceId | Identificador único de la factura. Vacío para el mes actual, abra. |
| (nueva) | publisherType | Tipo de publicador para las compras. Vacío para el uso. |
| (nueva) | serviceFamily | Tipo de la compra. Vacío para el uso. |
| (nueva) | servicePeriodEndDate | Fecha de finalización para el servicio adquirido. |
| (nueva) | servicePeriodStartDate | Fecha inicial para el servicio comprado. |

## <a name="billing-periods-api-replaced-by-invoices-api"></a>Reemplazado por la API de facturas de API de períodos de facturación

Las cuentas de facturación de MCA no usan los períodos de facturación. En su lugar, usan las facturas de los costos de ámbito para los períodos de facturación específicos. El [API de períodos de facturación](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) se sustituye por la API de facturas. Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request).

Para obtener las facturas con la API de facturas:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/invoices?api-version=2018-11-01-preview` |

## <a name="price-sheet-apis"></a>API de hoja de precios

En esta sección se describe las API de hoja de precios existente y ofrece recomendaciones para desplazarse a la API de hoja de precios para los contratos de cliente de Microsoft. También se describe la API de hoja de precios para los contratos de cliente de Microsoft y explica los campos en las hojas de precios. El [hoja de precios Enterprise obtener](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) y [Enterprise obtener períodos de facturación](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) API se ha reemplazado por la API de hoja de precios para los contratos de cliente de Microsoft (Microsoft.Billing/billingAccounts/billingProfiles / pricesheet). La nueva API admite formatos CSV y JSON, en formatos asincrónicos de REST. Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request).

### <a name="billing-enterprise-apis"></a>API de facturación de Enterprise

Usar API de facturación de Enterprise con inscripciones Enterprise para obtener información del periodo de facturación y el precio. Autenticación y autorización usan tokens web de Azure Active Directory.

Para obtener los precios aplicables para la inscripción Enterprise especificada con la hoja de precios y la API de período de facturación:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet` |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet` |

### <a name="price-sheet-api-for-microsoft-customer-agreements"></a>API de hoja de precios para los contratos de cliente de Microsoft

Utilice la API de hoja de precios para los contratos de cliente de Microsoft para ver los precios de todos los servicios de consumo de consumo de Azure y Marketplace. Los precios que se muestra para el perfil de facturación se aplican a todas las suscripciones que pertenecen al perfil de facturación.

Usar la API de hoja de precios para ver todos los datos de hoja de precios de servicios de consumo de Azure en formato CSV:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=csv` |

Usar la API de hoja de precios para ver todos los datos de hoja de precios de servicios de consumo de Azure en formato JSON:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=json` |

Mediante la API devuelve la hoja de precios para toda la cuenta. Sin embargo, también puede obtener una versión comprimida de la hoja de precios en formato PDF. El resumen incluye servicios de consumo de consumo de Azure y Marketplace que se facturan según una factura concreta. La factura se identifica mediante el {invoiceId}, que es el mismo que el **número de factura** se muestra en los archivos PDF de resumen de factura. Este es un ejemplo.

![Imagen de ejemplo que muestra el número de factura que corresponde a la InvoiceId](./media/migrate-cost-management-api/invoicesummary.png)

Para ver información de la factura con la API de hoja de precios en formato CSV:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

Para ver información de la factura con la API de hoja de precios en formato JSON:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

También puede ver los precios estimados para cualquier servicio de consumo de consumo de Azure o de Marketplace en el ciclo de facturación actual abierto o el período de servicio.

Para ver los precios estimados de servicios de consumo con la API de hoja de precios en formato CSV:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

Para ver los precios estimados de servicios de consumo con la API de hoja de precios en formato JSON:

| Método | URI de solicitud |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

Las API de hoja de Microsoft al cliente acuerdo precio son *asincrónica de las API de REST*. Puede cambiar las respuestas para las API desde las API sincrónicas antiguas. También puede cambiar el cuerpo de la respuesta de API.

#### <a name="old-response-body"></a>Cuerpo de la respuesta anterior

Este es un ejemplo de la respuesta sincrónica de la API de REST:

```
[
        {
            "id": "enrollments/573549891/billingperiods/2016011/products/343/pricesheets",
            "billingPeriodId": "201704",
            "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
            "meterName": "A1 VM",
            "unitOfMeasure": "100 Hours",
            "includedQuantity": 0,
            "partNumber": "N7H-00015",
            "unitPrice": 0.00,
            "currencyCode": "USD"
        },
        {
    ]
```

#### <a name="new-response-body"></a>Nuevo cuerpo de respuesta

Las API admiten la [asincrónicas de REST de Azure](../azure-resource-manager/resource-manager-async-operations.md) formato. Llame a la API de uso de GET y recibe la respuesta siguiente:

```
No Response Body

HTTP Status 202 Accepted
```

Los siguientes encabezados se envían con la ubicación de la salida:

```
Location:https://management.azure.com/providers/Microsoft.Consumption/operationresults/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Azure-AsyncOperation:https://managment.azure.com/providers/Microsoft.Consumption/operationStatus/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Retry-After: 10

OData-EntityId: {operationId}

```

Llamar a otro GET a la ubicación. La respuesta a la llamada GET es la misma hasta que la operación llega a un estado de finalización o el error. Cuando haya completado, la respuesta a la ubicación de la llamada GET devuelve la dirección URL de descarga. Como si la operación se ha ejecutado al mismo tiempo. Este es un ejemplo:

```
HTTP Status 200

                                    {
                            “id”: “providers/Microsoft.Consumption/operationresults/{operationId}”,
                            “name”: {operationId},
                           “type”: “Microsoft.Consumption/operationResults”,
                           “properties” : {
                                  “downloadUrl”: {urltoblob},
                                  “vaildTill”: “Date”
}
                     }
```

El cliente también puede realizar una llamada GET el `Azure-AsyncOperation`. El punto de conexión devuelve el estado de la operación.

La siguiente tabla muestra los campos de la API de hoja de precios Enterprise obtener anterior. Incluye los campos correspondientes en la nueva hoja de precios para los contratos de cliente de Microsoft:

| Propiedad anterior | Nueva propiedad | Notas |
| --- | --- | --- |
| billingPeriodId  | _No aplicable_ | No aplicable. Los contratos de cliente de Microsoft, la factura y la hoja de precios asociado reemplazan el concepto de billingPeriodId. |
| meterId  | meterId | &nbsp;  |
| unitOfMeasure  | unitOfMeasure | Pueden diferir los valores de cadena exacto. |
| includedQuantity  | includedQuantity | No es aplicable para los servicios en los contratos de cliente de Microsoft. |
| partNumber  | _No aplicable_ | En su lugar, use una combinación de productOrderName (igual que offerId) y meterid. |
| unitPrice  | unitPrice | Precio unitario es aplicable para los servicios que se utilizan en los contratos de cliente de Microsoft. |
| currencyCode  | pricingCurrency | Los contratos de cliente de Microsoft tienen representaciones de precio en moneda precios y facturación. El currencyCode corresponde a la pricingCurrency en contratos de cliente de Microsoft. |
| offerId | productOrderName | En lugar de OfferId, puede usar productOrderName pero no es el mismo que OfferId. Sin embargo, productOrderName y medidor determinan un precio en los contratos de cliente de Microsoft relacionado con meterId y Offerid en inscripciones heredadas. |

## <a name="consumption-price-sheet-api-operations"></a>Operaciones de API de hoja de precios de consumo

Para contratos Enterprise, usa la API de hoja de precios de consumo [obtener](/rest/api/consumption/pricesheet/get) y [obtener por período de facturación](/rest/api/consumption/pricesheet/getbybillingperiod) operaciones para un ámbito por un período de facturación o subscriptionId. La API usa la autenticación de Azure Resource Manager.

Para obtener la información de la hoja de precios para un ámbito con la API de hoja de precios:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

Para obtener información de la hoja de precios por período con la API de hoja de precios de facturación:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodName}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

En lugar de los puntos de conexión de API anteriores, use los siguientes contratos de Microsoft al cliente:

**API de hoja de precios para los contratos de cliente de Microsoft (API de REST asincrónica)**

Esta API es para los contratos de cliente de Microsoft y ofrece atributos adicionales.

**Hoja de precios para un ámbito de perfil de facturación en una cuenta de facturación**

Esta API es la API existente. Se actualizó para proporcionar la hoja de precios para un perfil de facturación en una cuenta de facturación.

## <a name="price-sheet-for-a-scope-by-billing-account"></a>Hoja de precios para un ámbito mediante la cuenta de facturación

Cuando llegue la hoja de precios en el ámbito de la inscripción en una cuenta de facturación, se usa la autenticación de Azure Resource Manager.

Para obtener la hoja de precios en la cuenta de inscripción en una cuenta de facturación:

| Método | URI de solicitud |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/65085863/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

Para un contrato de cliente de Microsoft, use la información en la sección siguiente. Proporciona las propiedades del campo usadas para contratos de Microsoft Customer.

### <a name="price-sheet-for-a-billing-profile-scope-in-a-billing-account"></a>Hoja de precios para un ámbito de perfil de facturación en una cuenta de facturación

La hoja de precios actualizada por cuenta de facturación API Obtiene la hoja de precios en formato CSV. Para obtener la hoja de precios en el ámbito de perfil de facturación para una MCA:

| Método | URI de solicitud |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/28ae4b7f-41bb-581e-9fa4-8270c857aa5f/billingProfiles/ef37facb-cd6f-437a-9261-65df15b673f9/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

En el ámbito de la inscripción de EA, la respuesta de la API y las propiedades son idénticas. Las propiedades corresponden a las mismas propiedades MCA.

Las propiedades anteriores para [API de hoja de Azure Resource Manager precio](/rest/api/consumption/pricesheet) y son las mismas propiedades nuevo en la tabla siguiente.

| Antigua propiedad Azure Resource Manager precio hoja API  | Nueva propiedad de la API de hoja de precios de Microsoft al cliente acuerdo   | DESCRIPCIÓN |
| --- | --- | --- |
| Id. de medidor | _meterId_ | Identificador único para el medidor. Igual que meterId. |
| Nombre del medidor | meterName | Nombre del medidor. Medidor representa el recurso que se puede implementar servicios de Azure. |
| Categoría del medidor  | service | Nombre de la categoría de clasificación para el medidor. Igual que el servicio en la hoja de precios de contrato de cliente de Microsoft. Difieren de los valores de cadena exacto. |
| Subcategoría del medidor | meterSubCategory | Nombre de la categoría de medidor subclasificación. Según la clasificación de diferenciación de conjunto de características de alto nivel en el servicio. Por ejemplo, básica SQL DB frente a la base de datos de SQL Standard. |
| Región del medidor | meterRegion | &nbsp;  |
| Unidad | _No aplicable_ | Se puede analizar desde la unidad de medida. |
| Unidad de medida | unitOfMeasure | &nbsp;  |
| Número de componente | _No aplicable_ | En lugar de NúmeroPieza, utilice productOrderName y MeterId para identificar de forma única el precio de un perfil de facturación. Se enumeran los campos en la factura MCA en lugar del número de artículo en las facturas MCA. |
| Precio unitario | unitPrice | Precio unitario de contrato de cliente de Microsoft. |
| Código de moneda | pricingCurrency | Los contratos de cliente de Microsoft representan los precios de la moneda de precios y facturación de moneda. Código de divisa es el mismo que el pricingCurrency en contratos de cliente de Microsoft. |
| Cantidad incluida | includedQuantity | No es aplicable a los servicios de los contratos de cliente de Microsoft. Mostrar con los valores de cero. |
|  Id. de oferta  | productOrderName | En lugar de OfferId, use productOrderName. No es el mismo que OfferId, sin embargo el productOrderName y medidor determinar un precio en los contratos de cliente de Microsoft. Relacionado con meterId y Offerid en inscripciones heredadas. |

El precio para los contratos de cliente de Microsoft se define de forma diferente a los contratos Enterprise. El precio para los servicios en la inscripción Enterprise es único para el producto, NúmeroPieza, medidor y oferta. El número de artículo no se usa en los contratos de cliente de Microsoft.

El precio del servicio de consumo de Azure que forma parte de un contrato de cliente de Microsoft es único para productOrderName y meterId. Representan el medidor de servicio y el plan del producto.

Para conciliar entre la hoja de precios y el uso de la API de detalles de uso, puede usar el productOrderName y meterId.

Los usuarios que tienen la facturación de propietario, Colaborador, lector de perfil y derechos de administrador de facturas pueden descargar la hoja de precios.

La hoja de precios incluye los precios de servicios cuyo precio se basa en uso. Los servicios incluyen el consumo de Azure y Marketplace. El precio más reciente al final de cada período de servicio está bloqueado y se aplican a uso en un período de servicio único. Para los servicios de consumo de Azure, el período de servicio es normalmente un mes natural.

### <a name="retired-price-sheet-api-fields"></a>Retirada de los campos de API de hoja de precios

Los campos siguientes no están disponibles en las API de hoja de Microsoft al cliente acuerdo precio o tienen los mismos campos.

|Campo retirados| DESCRIPCIÓN|
|---|---|
| billingPeriodId | No es aplicable. Corresponde a InvoiceId MCA. |
| offerId | No aplicable. Corresponde a productOrderName en MCA. |
| meterCategory  | No aplicable. Corresponde al servicio de MCA. |
| unit | No aplicable. Se puede analizar desde la unidad de medida. |
| currencyCode | Igual que el pricingCurrency en MCA. |
| meterLocation | Igual que el meterRegion en MCA. |
| número de artículo del número de artículo | No es aplicable porque el número de pieza no aparece en las facturas MCA. En lugar del número de artículo, use la combinación de meterId y productOrderName para identificar de forma exclusiva los precios. |
| totalIncludedQuantity | No aplicable. |
| pretaxStandardRate  | No aplicable. |

## <a name="reservation-instance-charge-api-replaced"></a>API de gastos de instancia de reserva reemplazado

Puede obtener facturación transacciones para las compras de reserva con la [API de gastos de instancia reservada](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). La nueva API incluye todas las compras, incluidas las ofertas de Marketplace de terceros. Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request). La API de cargo de instancia reservada se sustituye por la API de transacciones.

Para obtener las transacciones de compra con la API de transacciones de reserva:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/transactions?api-version=2018-11-01-preview` |

## <a name="recommendations-apis-replaced"></a>Reemplaza las API de recomendaciones

API de recomendaciones de compra de instancia reservada proporcionan el uso de máquinas virtuales durante los últimos 7, 30 o 60 días. API también proporcionan recomendaciones de compra de reserva. Entre ellos se incluyen los siguientes:

- [Compartir API de instancia reservada de recomendación](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)
- [API de recomendaciones de única instancia reservada](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)

Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request). Las recomendaciones de reserva, las API enumeradas anteriormente se reemplazan por el [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) API.

Para obtener recomendaciones de reserva con la API de recomendaciones de reserva:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationRecommendations?api-version=2019-01-01` |

## <a name="reservation-usage-apis-replaced"></a>Reemplaza la API de uso de reserva

Puede obtener el uso de reserva en una inscripción con la API de uso de instancia reservada. Si hay más de una instancia reservada de una inscripción, también puede obtener el uso de todas las compras de la instancia reservada mediante esta API.

Entre ellos se incluyen los siguientes:

- [Detalles de uso de instancias reservadas](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)
- [Resumen de uso de instancias reservadas](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary)

Todas las API de consumo se reemplazan por las API nativas de Azure que usan Azure AD para la autenticación y autorización. Para obtener más información sobre cómo llamar a las API de REST de Azure, consulte [Introducción a REST](/rest/api/azure/#create-the-request). Las recomendaciones de reserva, las API enumeradas anteriormente se reemplazan por el [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails) y [Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) API.

Para obtener detalles de la reserva con la API de detalles de reserva:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationDetails?api-version=2019-01-01` |

Para obtener los resúmenes de reserva con la API de resúmenes de reserva:

| Método | URI de solicitud |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationSummaries?api-version=2019-01-01` |



## <a name="move-from-cloudyn-to-cost-management"></a>Mover de Cloudyn para administración de costos

Las organizaciones que usan [Cloudyn](https://cloudyn.com) debería empezar a usar [Azure Cost Management](https://azure.microsoft.com/services/cost-management/) para las necesidades de administración de costos. Administración de costos está disponible en el portal de Azure con una latencia de ocho horas y ninguna incorporación. Para obtener más información, consulte el [documentación de administración de costos](index.yml).

Con Azure Cost Management, puede:

- Ver los costos con el tiempo con un presupuesto predefinido. Analizar diariamente los patrones de costo para identificar y detener las anomalías de gastos. Desglosar los costos por etiquetas, servicio, grupo de recursos y ubicación.
- Cree los presupuestos para definir los límites de uso y los costos y recibir una notificación cuando se abordó umbrales importantes. Configurar automation con grupos de acciones para desencadenar eventos personalizados y exigir límites estrictos en sus propios términos.
- Optimizar el costo y el uso con las recomendaciones de Azure Advisor. Descubra las optimizaciones de compra con reservas, reducir el tamaño de máquinas virtuales infrautilizadas y eliminar recursos no utilizados para mantenerse dentro de los presupuestos.
- Programar una exportación de datos de uso y costo para publicar un archivo CSV en la cuenta de almacenamiento diario. Automatizar la integración con sistemas externos para mantener los datos de facturación sincronizados y al día.

## <a name="power-bi-integration"></a>Integración con Power BI

Si usa Power BI para los informes de costo, es preciso realizar la transición a la siguiente:

- Microsoft Azure consumo Insights aplicación de Power BI
- Conector de escritorio de Azure Consumption Insights


El conector se recomienda para las organizaciones que desean la máxima flexibilidad. Sin embargo, la aplicación Power BI también está disponible para la configuración rápida.

- Instalar el [Microsoft Azure Consumption Insights mejorar una aplicación de BI](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview)
- [Conectar con el conector Azure Consumption Insights](/power-bi/desktop-connect-azure-consumption-insights)

El paquete de contenido de la información de consumo y el conector antiguo trabajaron en un nivel de inscripción. Requiere al menos acceso de lectura. La nueva aplicación de consumo Insights Power BI y el nuevo conector de Azure Consumption Insights están disponibles para usuarios de perfil de facturación. Los equipos que necesitan opciones adicionales para la revisión de los costos o ver los costos a través de perfiles de facturación deben usar en [análisis de costos](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/costanalysis) el portal de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Leer el [documentación de administración de costos](index.yml) para obtener información sobre cómo supervisar y controlar el gasto de Azure. O bien, si desea optimizar el uso de recursos con Cost Management.
