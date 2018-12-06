---
title: 'Precios y facturación: Azure Logic Apps | Microsoft Docs'
description: Descubra cómo funcionan los precios y la facturación en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
manager: carmonm
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 10/16/2018
ms.openlocfilehash: 04fb86f9b8f8be2c013f9bd7449dd5a4b2bcf90c
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "51854125"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Modelo de precios de Azure Logic Apps

Puede crear y ejecutar flujos de trabajo de integración automatizados en la nube al usar Azure Logic Apps. Estos son los detalles sobre cómo funcionan los precios y facturación para Logic Apps. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modelo de precios de consumo

Para las nuevas aplicaciones lógicas que se ejecuten en el servicio de Logic Apps "global" o público, paga solo lo que usa. Estas aplicaciones lógicas usan un plan de consumo y el modelo de precios. En la definición de la aplicación lógica, cada paso es una acción. Las acciones incluyen el desencadenador, los pasos del flujo de control, acciones integradas y llamadas de conector. Logic Apps mide todas las acciones que se ejecutan en la aplicación lógica.  
Para más información, consulte [Precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Modelo de precios fijos

Para las nuevas aplicaciones lógicas que se ejecutan en un [*entorno del servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), se paga un precio mensual fijo por las acciones integradas y los conectores con la etiqueta ISE. Un ISE es una manera de crear y ejecutar aplicaciones lógicas aisladas que puedan acceder a recursos de una red virtual de Azure.  

El ISE incluye un conector empresarial gratuito, con tantas conexiones como desee. El uso de los conectores empresariales adicionales se carga en función del precio de consumo empresarial. 

> [!NOTE]
> El entorno de servicio de integración se encuentra en *versión preliminar privada*. Para solicitar acceso, [cree su solicitud para unirse aquí](https://aka.ms/iseprivatepreview). Para más información, consulte [Precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Desencadenadores

Los desencadenadores son acciones especiales que crean una instancia de aplicación lógica cuando se produce un evento específico. Los desencadenadores actúan de formas distintas, que afectan al modo en que la aplicación lógica se mide.

* **Desencadenador de sondeo**: sondea continuamente un punto de conexión en busca de mensajes que satisfacen los criterios para crear una instancia de una aplicación lógica e iniciar el flujo de trabajo. Logic Apps mide cada solicitud de sondeo como una ejecución, incluso cuando no se crea ninguna instancia de aplicación lógica. Para especificar el intervalo de sondeo, configure el desencadenador a través del Diseñador de aplicaciones lógicas.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Desencadenador de webhook**: este desencadenador espera a que un cliente envíe una solicitud a un punto de conexión determinado. Cada solicitud enviada al punto de conexión de webhook se considera una ejecución de acción. Por ejemplo, el desencadenador de solicitud y de webhook HTTP son ambos desencadenadores de webhook.

* **Desencadenador de periodicidad**: este desencadenador crea una instancia de la aplicación lógica según el intervalo de periodicidad configurado en el desencadenador. Por ejemplo, puede establecer un desencadenador de periodicidad que se ejecute cada tres días o según una programación más compleja.

## <a name="actions"></a>Acciones

Logic Apps mide las acciones integradas como acciones nativas. Por ejemplo, las acciones integradas incluyen las llamadas a través de HTTP, las llamadas desde Azure Functions o API Management y los pasos del flujo de control, como los bucles y las condiciones, 
- cada uno con su propio tipo de acción. Las acciones que llaman a [conectores](https://docs.microsoft.com/connectors) tienen el tipo "ApiConnection". Estos conectores se clasifican como standard o enterprise, y se miden con sus [precios][pricing] respectivos. Los conectores empresariales en *versión preliminar* se cobran como conectores estándar.

Logic Apps mide todas las acciones ejecutadas correcta o incorrectamente como ejecuciones de acción. Logic Apps no mide estas acciones: 

* Acciones que se omitan debido a condiciones no satisfechas
* Acciones que no se ejecuten porque la aplicación lógica se detuviera antes de finalizar

Las aplicaciones lógicas deshabilitadas no se cobran, ya que no se crean instancias.

> [!NOTE]
> Después de deshabilitar una aplicación lógica, las instancias que se estén ejecutando pueden tardar un tiempo antes de detenerse por completo.

Para las acciones que se ejecutan dentro de bucles, Logic Apps cuenta las acciones por cada ciclo del bucle. Por ejemplo, supongamos que tiene un bucle "for each" que procesa una lista. Logic Apps mide una acción de ese bucle al multiplicar el número de elementos de lista por el número de acciones del bucle, y agrega la acción que inicia el bucle. El cálculo para una lista de 10 elementos es (10 * 1) + 1, lo que da como resultado 11 ejecuciones de acción.

## <a name="integration-account-usage"></a>Uso de la cuenta de integración

El uso basado en el consumo se aplica a las [cuentas de integración](logic-apps-enterprise-integration-create-integration-account.md) en las que se puede explorar, desarrollar y realizar pruebas de las características [B2B/EDI](logic-apps-enterprise-integration-b2b.md) y las características de [procesamiento XML](logic-apps-enterprise-integration-xml.md) de Logic Apps sin costo adicional. Puede tener una cuenta de integración por región. Cada cuenta de integración puede almacenar [números específicos de artefactos](../logic-apps/logic-apps-limits-and-config.md), como entidades, contratos, mapas, esquemas, ensamblados, certificados y configuraciones por lotes, etc.

Azure Logic Apps también ofrece cuentas de integración básicas y estándar con Acuerdos de Nivel de Servicio de Logic Apps admitidos. Puede utilizar cuentas de integración básicas cuando desee utilizar solo el control de mensajes o actuar como un asociado de pequeña empresa que tenga una relación empresarial con una entidad empresarial mayor. Las cuentas de integración estándar admiten relaciones más complejas entre empresas y permiten aumentar el número de entidades que se pueden administrar. Para obtener más información, consulte [Precios de Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de Logic Apps][whatis]
* [Creación de una nueva aplicación lógica mediante la conexión de servicios de SaaS][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

