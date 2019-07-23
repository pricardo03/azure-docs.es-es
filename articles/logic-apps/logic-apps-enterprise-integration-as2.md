---
title: 'Mensajes AS2 para la integración empresarial B2B: Azure Logic Apps'
description: Intercambio de mensajes AS2 en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: b494f6524e5105a95bc8a24a6fa2521abcca3f7b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64729399"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Intercambio de mensajes AS2 para la integración empresarial B2B en Azure Logic Apps con Enterprise Integration Pack

Para trabajar con mensajes AS2 en Azure Logic Apps, puede usar el conector AS2, que proporciona los desencadenadores y acciones necesarios para administrar las comunicaciones AS2. Por ejemplo, para garantizar la seguridad y confiabilidad al transmitir mensajes, puede utilizar estas acciones:

* La acción [**Codificar en mensaje AS2**](#encode) para proporcionar firmas digitales, cifrado y confirmaciones mediante las notificaciones de disposición del mensaje (MDN), lo que ayuda a admitir la recepción sin rechazo. Por ejemplo, esta acción se aplica a los encabezados AS2/HTTP y realiza estas tareas cuando se configura:

  * Firma los mensajes salientes.
  * Cifra los mensajes salientes.
  * Comprime el mensaje.
  * Transmite el nombre de archivo en el encabezado MIME.

* La acción [**Descodificar mensaje AS2**](#decode) para proporcionar firmas digitales, descifrado y confirmaciones a través de las notificaciones de disposición de mensajes (MDN). Por ejemplo, esta acción lleva a cabo estas tareas: 

  * Procesa los encabezados AS2/HTTP.
  * Reconcilia los MDN recibidos con los mensajes de salida originales.
  * Actualiza y correlaciona registros en la base de datos sin rechazo.
  * Escribe registros para los informes de estado de AS2.
  * Devuelve el contenido de la carga útil con codificación base64.
  * Determina si el MDN es necesario. En función del contrato AS2, determina si el MDN deben ser sincrónico o asincrónico.
  * Genera el MDN sincrónico o asincrónico según el contrato AS2.
  * Establece las propiedades y los token de correlación en los MDN.

  Esta acción también realiza estas tareas cuando se configura:

  * Comprueba la firma.
  * Descifra los mensajes.
  * Descomprime el mensaje. 
  * Comprueba y prohíbe duplicados de identificador de mensaje.

En este artículo se muestra cómo agregar la codificación AS2 y las acciones de descodificación a una aplicación lógica existente.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La aplicación lógica desde la que quiere usar el conector AS2 y un desencadenador que inicie el flujo de trabajo de la aplicación lógica. El conector AS2 proporciona únicamente las acciones, no los desencadenadores. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) asociada con la suscripción a Azure y vinculada a las aplicaciones lógicas en las que planea usar el conector AS2. Tanto la cuenta de integración como la aplicación de lógica deben existir en la misma ubicación o región de Azure.

* Al menos dos [entidades](../logic-apps/logic-apps-enterprise-integration-partners.md) que ya haya creado en su cuenta de integración mediante el calificador de identidad AS2.

* Para poder usar el conector AS2, debe crear un [contrato](../logic-apps/logic-apps-enterprise-integration-agreements.md) AS2 entre las entidades y almacenarlo en la cuenta de integración.

* Si usa [Azure Key Vault](../key-vault/key-vault-overview.md) para la administración de certificados, compruebe que las claves de almacén permitan las operaciones **Cifrar** y **Descifrar**. En caso contrario, las acciones de codificación y descodificación producirán un error.

  En Azure Portal, vaya al almacén de claves, consulte las **operaciones permitidas** de la clave de almacén y confirme que las operaciones **Cifrar** y **Descifrar** están seleccionadas.

  ![Comprobar las operaciones con claves de almacén](media/logic-apps-enterprise-integration-as2/vault-key-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>Codificación de mensajes AS2

1. Si aún no lo ha hecho, en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicación lógica.

1. En el diseñador, agregue una nueva acción a la aplicación lógica. 

1. En **Elegir una acción** y en el cuadro de búsqueda, seleccione **Todas**. En el cuadro de búsqueda, escriba "encode as2" y seleccione esta acción: **Codificar en mensaje AS2**.

   ![Seleccionar "Codificar en mensaje AS2"](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Si no existe una conexión a la cuenta de integración, se le pedirá que cree una ahora. Asigne un nombre a la conexión, seleccione la cuenta de integración que quiera conectar y elija **Crear**.

   ![Crear la conexión con la cuenta de integración](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  
 
1. Ahora, proporcione la información de las siguientes propiedades:

   | Propiedad | DESCRIPCIÓN |
   |----------|-------------|
   | **AS2-From** | Identificador del remitente del mensaje según lo especificado por su contrato AS2. |
   | **AS2-To** | Identificador del receptor del mensaje según lo especificado por su contrato AS2. |
   | **body** | Carga útil del mensaje. |
   |||

   Por ejemplo:

   ![Propiedades de codificación del mensaje.](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

<a name="decode"></a>

## <a name="decode-as2-messages"></a>Descodificación de mensajes AS2

1. Si aún no lo ha hecho, en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicación lógica.

1. En el diseñador, agregue una nueva acción a la aplicación lógica. 

1. En **Elegir una acción** y en el cuadro de búsqueda, seleccione **Todas**. En el cuadro de búsqueda, escriba "decode as2" y seleccione esta acción: **Descodificar mensaje AS2**.

   ![Seleccionar "Descodificar mensaje AS2"](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. Si no existe una conexión a la cuenta de integración, se le pedirá que cree una ahora. Asigne un nombre a la conexión, seleccione la cuenta de integración que quiera conectar y elija **Crear**.

   ![Crear la conexión con la cuenta de integración](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  

1. Para **body** y **Headers**, seleccione estos valores de las salidas anteriores de las acciones o desencadenadores.

   Por ejemplo, suponga que la aplicación lógica recibe mensajes a través de un desencadenador de solicitud. Puede seleccionar las salidas de ese desencadenador.

   ![Seleccione el cuerpo y los encabezados de las salidas de la solicitud](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png) 

## <a name="sample"></a>Muestra

Para intentar implementar una aplicación lógica totalmente operativa y conocer el escenario de ejemplo de AS2, consulte el artículo sobre [escenario y plantilla de aplicaciones lógicas de AS2](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo OpenAPI (antes Swagger) del conector, consulte la [página de referencia del conector](/connectors/as2/).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
