---
title: 'Mensajes AS2 para la integración empresarial B2B: Azure Logic Apps'
description: Intercambio de mensajes de AS2 en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: b494f6524e5105a95bc8a24a6fa2521abcca3f7b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729399"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Intercambio de mensajes AS2 para la integración empresarial B2B en Azure Logic Apps con Enterprise Integration Pack

Para trabajar con mensajes de AS2 en Azure Logic Apps, puede usar el conector AS2, que proporciona los desencadenadores y acciones para la administración de comunicaciones de AS2. Por ejemplo, para garantizar la seguridad y confiabilidad al transmitir mensajes, puede utilizar estas acciones:

* [**Codificar en mensaje AS2** acción](#encode) para proporcionar cifrado, firma digital y confirmaciones a través de las notificaciones de disposición del mensaje (MDN), que ayudan a admitir sin repudio. Por ejemplo, esta acción aplica a los encabezados AS2/HTTP y realiza estas tareas cuando se configura:

  * Firma mensajes salientes.
  * Cifra los mensajes salientes.
  * Comprime el mensaje.
  * Transmite el nombre de archivo en el encabezado MIME.

* [**Descodificar mensaje AS2** acción](#decode) para proporcionar el descifrado, firma digital y confirmaciones a través de las notificaciones de disposición del mensaje (MDN). Por ejemplo, esta acción lleva a cabo estas tareas: 

  * Procesa encabezados AS2/HTTP.
  * Reconcilia un MDN recibido con los mensajes de salida originales.
  * Actualiza y correlaciona registros en la base de datos sin repudio.
  * Escribe los registros para los informes de estado de AS2.
  * Contenido de carga de las salidas como codificada en base64.
  * Determina si el MDN se necesitan. En función de AS2 acuerdo, determina si el MDN deben ser sincrónico o asincrónico.
  * Genera el MDN sincrónica o asincrónica según el acuerdo AS2.
  * Establece las propiedades y los tokens de correlación en los MDN.

  Esta acción también realiza estas tareas cuando se configura:

  * Comprueba la firma.
  * Descifra los mensajes.
  * Descomprime el mensaje. 
  * Compruebe y no permitir duplicados del Id. de mensaje.

En este artículo se muestra cómo agregar la codificación de AS2 y acciones de descodificación para una aplicación lógica existente.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La aplicación lógica desde donde desea usar el conector AS2 y un desencadenador que inicia el flujo de trabajo de la aplicación lógica. El conector AS2 proporciona únicamente las acciones, no los desencadenadores. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Un [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) que ha asociado con su suscripción de Azure y vinculado a la aplicación lógica donde va a usar el conector AS2. Tanto la cuenta de integración y la aplicación de lógica debe existir en la misma ubicación o región de Azure.

* Al menos dos [socios comerciales](../logic-apps/logic-apps-enterprise-integration-partners.md) que ya se ha definido en la cuenta de integración utilizando el calificador de identidad de AS2.

* Para poder usar el conector AS2, debe crear un AS2 [acuerdo](../logic-apps/logic-apps-enterprise-integration-agreements.md) entre los socios comerciales y el almacén de ese contrato en la cuenta de integración.

* Si usas [Azure Key Vault](../key-vault/key-vault-overview.md) para la administración de certificados, compruebe que se permiten las claves del almacén la **Encrypt** y **descifrar** operaciones. En caso contrario, la codificación y descodificación acciones producirá un error.

  En el portal de Azure, vaya al almacén de claves, ver su clave de almacén **operaciones permitidas**y confirme que la **Encrypt** y **descifrar** operaciones están seleccionadas.

  ![Compruebe las operaciones de clave de almacén](media/logic-apps-enterprise-integration-as2/vault-key-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>Codificación de mensajes AS2

1. Si no lo ha hecho ya, en el [portal Azure](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

1. En el diseñador, agregue una nueva acción a la aplicación lógica. 

1. En **elegir una acción** y la búsqueda, seleccione **todas**. En el cuadro de búsqueda, escriba "codificar as2" y seleccione esta acción: **Codificar en mensaje AS2**.

   ![Seleccione "Codificar en mensaje AS2"](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Si no tiene una conexión existente a la cuenta de integración, se le pedirá que cree ahora esa conexión. Nombre de la conexión, seleccione la cuenta de integración que desee conectarse y elija **crear**.

   ![Crear conexión con la cuenta de integración](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  
 
1. Ahora se proporciona información de las siguientes propiedades:

   | Propiedad | DESCRIPCIÓN |
   |----------|-------------|
   | **AS2-From** | El identificador de remitente del mensaje según lo especificado por su contrato de AS2 |
   | **AS2-To** | El identificador para el receptor del mensaje según lo especificado por su contrato de AS2 |
   | **body** | La carga del mensaje |
   |||

   Por ejemplo: 

   ![Propiedades de codificación de mensajes](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

<a name="decode"></a>

## <a name="decode-as2-messages"></a>Descodificación de mensajes AS2

1. Si no lo ha hecho ya, en el [portal Azure](https://portal.azure.com), abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

1. En el diseñador, agregue una nueva acción a la aplicación lógica. 

1. En **elegir una acción** y la búsqueda, seleccione **todas**. En el cuadro de búsqueda, escriba "descodificar as2" y seleccione esta acción: **Descodificar mensaje AS2**

   ![Seleccione "Decode AS2 message"](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. Si no tiene una conexión existente a la cuenta de integración, se le pedirá que cree ahora esa conexión. Nombre de la conexión, seleccione la cuenta de integración que desee conectarse y elija **crear**.

   ![Crear conexión con la cuenta de integración](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  

1. Para **cuerpo** y **encabezados**, seleccione estos valores desde los resultados de acción o desencadenador anteriores.

   Por ejemplo, suponga que la aplicación lógica recibe mensajes a través de un desencadenador de solicitud. Puede seleccionar las salidas de ese desencadenador.

   ![Seleccione el cuerpo y los encabezados de las salidas de la solicitud](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png) 

## <a name="sample"></a>Muestra

Para intentar implementar una aplicación lógica totalmente operativa y conocer el escenario de ejemplo de AS2, consulte el artículo sobre [escenario y plantilla de aplicaciones lógicas de AS2](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos, como desencadenadores, acciones y los límites, como se describe en OpenAPI del conector (anteriormente Swagger) de archivos, consulte el [página de referencia del conector](/connectors/as2/).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
