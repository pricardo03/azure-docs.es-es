---
title: 'Integración de las operaciones de seguridad con Microsoft Graph Security: Azure Logic Apps'
description: Mejore las funcionalidades de protección, detección y respuesta frente amenazas mediante la administración de las operaciones de seguridad con Microsoft Graph Security y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: preetikr
ms.author: preetikr
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/30/19
tags: connectors
ms.openlocfilehash: 647df9e73804c8f261b58d5ede7c4b030d448fed
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513310"
---
# <a name="improve-threat-protection-by-integrating-security-operations-with-microsoft-graph-security--azure-logic-apps"></a>Mejore la protección frente a amenazas mediante la integración de las operaciones de seguridad con Microsoft Graph Security y Azure Logic Apps.

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el conector [Microsoft Graph Security](https://docs.microsoft.com/graph/security-concept-overview), puede mejorar cómo la aplicación detecta, protege y responde frente a las amenazas mediante la creación de flujos de trabajo automatizados para la integración de productos, servicios y asociados de seguridad de Microsoft. Por ejemplo, puede crear [cuadernos de estrategias de Azure Security Center](../security-center/security-center-playbooks.md) que supervisen y administren las entidades de seguridad de Microsoft Graph, como las alertas. Estos son algunos de los escenarios admitidos por el conector Microsoft Graph Security:

* Obtención de alertas basadas en consultas o por identificador de alerta. Por ejemplo, puede obtener una lista que incluya alertas de gravedad alta.
* Actualización de alertas. Por ejemplo, puede actualizar asignaciones de alertas, agregar comentarios a alertas o etiquetar alertas.
* Supervisión de cuándo se crean las alertas o se cambian mediante la creación de [suscripciones de alertas (webhooks)](https://docs.microsoft.com/graph/api/resources/webhooks).
* Administración de las suscripciones de alertas. Por ejemplo, puede obtener las suscripciones activas, ampliar el tiempo de expiración de una suscripción o eliminar suscripciones.

El flujo de trabajo de la aplicación lógica puede usar acciones que obtengan respuestas del conector Microsoft Graph Security y permitir que esa salida esté disponible para otras acciones del flujo de trabajo. También puede tener otras acciones en el flujo de trabajo mediante la salida de las acciones del conector Microsoft Graph Security. Por ejemplo, si recibe alertas de gravedad alta a través del conector Microsoft Graph Security, puede enviar esas alertas en un mensaje de correo electrónico mediante el conector de Outlook. 

Para más información sobre Microsoft Graph Security, consulte la [introducción a Microsoft Graph Security API](https://aka.ms/graphsecuritydocs). Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) Si busca Microsoft Flow o PowerApps, consulte [¿Qué es Flow?](https://flow.microsoft.com/) o [¿Qué es PowerApps?](https://powerapps.microsoft.com/)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* Para usar el conector Microsoft Graph Security, debe haber *otorgado explícitamente* consentimiento al administrador de inquilinos de Azure Active Directory (AD), que es parte de los [requisitos de autenticación de Microsoft Graph Security](https://aka.ms/graphsecurityauth). Este consentimiento requiere el identificador y el nombre de la aplicación del conector Microsoft Graph Security, que también se puede encontrar en [Azure Portal](https://portal.azure.com):

   | Propiedad | Valor |
   |----------|-------|
   | **Nombre de la aplicación** | `MicrosoftGraphSecurityConnector` |
   | **Identificador de la aplicación** | `c4829704-0edc-4c3d-a347-7c4a67586f3c` |
   |||

   Para conceder consentimiento para el conector, el administrador de inquilinos de Azure AD puede seguir cualquiera de estos pasos:

   * [Conceda al administrador el consentimiento para aplicaciones de Azure AD](../active-directory/develop/v2-permissions-and-consent.md).

   * Durante la primera ejecución de la aplicación lógica, la aplicación puede solicitar el consentimiento del administrador de inquilinos de Azure AD mediante la [experiencia de consentimiento de aplicación](../active-directory/develop/application-consent-experience.md).
   
* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica a la que quieren acceder las entidades de Microsoft Graph Security, como las alertas. Actualmente, este conector no tiene desencadenadores. Por consiguiente, para usar una acción de Microsoft Graph Security, inicie la aplicación lógica con un desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="connect-to-microsoft-graph-security"></a>Conexión con Microsoft Graph Security 

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. En el caso de aplicaciones lógicas en blanco, agregue el desencadenador y todas las demás acciones que desee antes de agregar una acción de Microsoft Graph Security.

   O bien

   Con aplicaciones lógicas existentes, en el último paso donde quiera agregar una acción de Microsoft Graph Security, elija **New step** (Nuevo paso).

   O bien

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
   Elija el signo más (+) que aparece y seleccione **Add an action** (Agregar una acción).

1. En el cuadro de búsqueda, escriba "microsoft graph security" como filtro. En la lista de acciones, seleccione la acción que desee.

1. Inicie sesión con sus credenciales de Microsoft Graph Security.

1. Proporcione los detalles necesarios para la acción seleccionada y continúe con la creación del flujo de trabajo de la aplicación lógica.

## <a name="add-actions"></a>Incorporación de una acción

Estos son detalles más específicos sobre el uso de las distintas acciones disponibles con el conector Microsoft Graph Security.

### <a name="manage-alerts"></a>Administrar alertas

Para filtrar, ordenar, u obtener los resultados más recientes, *solo* proporcione los [parámetros de consulta de ODATA admitidos por Microsoft Graph](https://docs.microsoft.com/graph/query-parameters). *No especifique* la dirección URL base completa o la acción HTTP, por ejemplo, `https://graph.microsoft.com/v1.0/security/alerts`, ni la operación `GET` o `PATCH`. Este es un ejemplo específico que muestra los parámetros para una acción **Obtener alertas** cuando quiere una lista con alertas de gravedad alta:

`Filter alerts value as Severity eq 'high'`

Para más información sobre las consultas que puede usar con este conector, consulte la [documentación de referencia de alertas de Microsoft Graph Security](https://docs.microsoft.com/graph/api/alert-list). Para crear experiencias mejoradas con este conector, aprenda más sobre las [alertas de propiedades de esquema](https://docs.microsoft.com/graph/api/resources/alert) que admite el conector.

| . | DESCRIPCIÓN |
|--------|-------------|
| **Obtener alertas** | Obtenga alertas filtrada por una o varias [propiedades de alerta](https://docs.microsoft.com/graph/api/resources/alert), por ejemplo: <p>`Provider eq 'Azure Security Center' or 'Palo Alto Networks'` | 
| **Obtener alerta por identificador** | Obtenga una alerta específica según el identificador de alerta. | 
| **Actualizar alerta** | Actualice una alerta específica según el identificador de alerta. <p>Para asegurarse de que pasa las propiedades obligatorias y modificables en la solicitud, consulte las [propiedades editables para alertas](https://docs.microsoft.com/graph/api/alert-update). Por ejemplo, para asignar una alerta a un analista de seguridad para que puede investigarla, puede actualizar la propiedad **Assigned to** (Asignado a) de la alerta. |
|||

### <a name="manage-alert-subscriptions"></a>Administración de suscripciones de alertas

Microsoft Graph admite [*suscripciones*](https://docs.microsoft.com/graph/api/resources/subscription) o [*webhooks*](https://docs.microsoft.com/graph/api/resources/webhooks). Para obtener, actualizar, o eliminar suscripciones, proporcione los [parámetros de consulta de ODATA admitidos por Microsoft Graph](https://docs.microsoft.com/graph/query-parameters) a la construcción de entidad de Microsoft Graph e incluya `security/alerts` seguido de la consulta de ODATA. 
*No incluya* la URL base, por ejemplo, `https://graph.microsoft.com/v1.0`. En cambio, use el formato de este ejemplo:

`security/alerts?$filter=status eq 'New'`

| . | DESCRIPCIÓN |
|--------|-------------|
| **Creación de suscripciones** | [Crea una suscripción](https://docs.microsoft.com/graph/api/subscription-post-subscriptions) que le informe sobre cualquier cambio. Puede filtrar esta suscripción por los tipos de alerta específicos que desee. Por ejemplo, puede crear una suscripción que le informe sobre las alertas de gravedad alta. |
| **Obtener suscripciones activas** | [Obtiene las suscripciones no expiradas](https://docs.microsoft.com/graph/api/subscription-list). | 
| **Actualizar suscripción** | [Actualiza una suscripción](https://docs.microsoft.com/graph/api/subscription-update) cuando se proporciona el identificador de suscripción. Por ejemplo, para ampliar su suscripción, puede actualizar la propiedad `expirationDateTime` de la suscripción. | 
| **Eliminar suscripción** | [Elimina una suscripción](https://docs.microsoft.com/graph/api/subscription-delete) cuando se proporciona el identificador de la suscripción. | 
||| 

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](https://aka.ms/graphsecurityconnectorreference) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
