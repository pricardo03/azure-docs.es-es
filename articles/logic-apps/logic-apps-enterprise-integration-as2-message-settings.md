---
title: 'Configuración de mensaje de AS2: Azure Logic Apps'
description: Guía de referencia para AS2 enviar y recibir la configuración en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: ead92094b9af1dff56ff68e1ff58a3a4cdd9dca5
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769457"
---
# <a name="reference-for-as2-message-settings-in-azure-logic-apps-with-enterprise-integration-pack"></a>Referencia de configuración del mensaje AS2 en Azure Logic Apps con Enterprise Integration Pack

Esta referencia describe las propiedades que se pueden establecer para especificar cómo un acuerdo AS2 controla los mensajes enviados y recibidos entre socios comerciales. Configure estas propiedades en función del contrato con el socio comercial que intercambia mensajes con usted.

<a name="AS2-incoming-messages"></a>

## <a name="as2-receive-settings"></a>Configuración de recepción de AS2

![Seleccione "Configuración de recepción"](./media/logic-apps-enterprise-integration-as2-message-settings/receive-settings.png)

| Propiedad | Obligatorio | DESCRIPCIÓN |
|----------|----------|-------------|
| **Invalidar propiedades de mensajes** | Sin  | Reemplaza las propiedades en los mensajes entrantes con los valores de propiedad. |
| **Debe firmarse el mensaje** | Sin  | Especifica si todos los mensajes entrantes deben estar firmados digitalmente. Si necesita iniciar sesión, desde el **certificado** lista, seleccione un existente socio certificado público del invitado para validar la firma de los mensajes. Si no tiene un certificado, obtenga más información sobre [agregar certificados](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Debe cifrarse el mensaje** | Sin  | Especifica si se deben cifrar todos los mensajes entrantes. Los mensajes sin cifrado se rechazan. Si necesita el cifrado, desde el **certificado** lista, seleccione un host partner private certificado existente para descifrar los mensajes entrantes. Si no tiene un certificado, obtenga más información sobre [agregar certificados](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Debe comprimirse el mensaje** | Sin  | Especifica si se deben comprimir todos los mensajes entrantes. Los mensajes sin comprimir se rechazan. |
| **No permitir duplicados del Id. de mensaje** | Sin  | Especifica si se debe permitir que los mensajes con identificadores duplicados. Si prohíbe identificadores duplicados, seleccione el número de días entre comprobaciones. También puede elegir si se deben suspender los duplicados. |
| **Texto de MDN** | Sin  | Especifica la predeterminada mensaje disposition notificación (MDN) que desea que se envía al remitente del mensaje. |
| **Enviar MDN** | Sin  | Especifica si se debe enviar MDN sincrónicos para los mensajes recibidos.  |
| **Enviar MDN firmado** | Sin  | Especifica si se debe enviar MDN firmadas para los mensajes recibidos. Si necesita iniciar sesión, desde el **algoritmo MIC** lista, seleccione el algoritmo que se usará para firmar los mensajes. |
| **Enviar MDN asincrónico** | Sin  | Especifica si se debe enviar MDN de forma asincrónica. Si selecciona MDN asincrónica, en el **URL** cuadro, especifique la dirección URL donde se enviarán las MDN. |
||||

<a name="AS2-outgoing-messages"></a>

## <a name="as2-send-settings"></a>Configuración de envío de AS2

![Seleccione "Configuración de envío"](./media/logic-apps-enterprise-integration-as2-message-settings/send-settings.png)

| Propiedad | Obligatorio | DESCRIPCIÓN |
|----------|----------|-------------|
| **Habilitar la firma del mensaje** | Sin  | Especifica si deben firmarse digitalmente todos los mensajes salientes. Si necesita iniciar sesión, seleccione estos valores: <p>-Desde el **algoritmo de firma** lista, seleccione el algoritmo que se usará para firmar los mensajes. <br>-Desde el **certificado** lista, seleccione un host partner private certificado existente para firmar los mensajes. Si no tiene un certificado, obtenga más información sobre [agregar certificados](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Habilitar el cifrado de mensajes** | Sin  | Especifica si se deben cifrar todos los mensajes salientes. Si necesita el cifrado, seleccione estos valores: <p>-Desde el **algoritmo de cifrado** lista, seleccione el algoritmo de certificado público del asociado invitado que se usará para cifrar los mensajes. <br>-Desde el **certificado** lista, seleccione un existente invitado certificado privado del asociado para cifrar los mensajes salientes. Si no tiene un certificado, obtenga más información sobre [agregar certificados](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Habilitar la compresión de mensajes** | Sin  | Especifica si se deben comprimir todos los mensajes salientes. |
| **Expandir encabezados HTTP** | Sin  | Coloca el HTTP `content-type` encabezado en una sola línea. |
| **Transmitir nombre de archivo en encabezado MIME** | Sin  | Especifica si se debe incluir el nombre de archivo en el encabezado MIME. |
| **Solicitar MDN** | Sin  | Especifica si desea recibir notificaciones de disposición de mensaje (MDN) para todos los mensajes salientes. |
| **Solicitar MDN firmado** | Sin  | Especifica si se debe recibir MDN firmadas para todos los mensajes salientes. Si necesita iniciar sesión, desde el **algoritmo MIC** lista, seleccione el algoritmo que se usará para firmar los mensajes. |
| **Solicitar MDN asíncrono** | Sin  | Especifica si se recibe MDN de forma asincrónica. Si selecciona MDN asincrónica, en el **URL** cuadro, especifique la dirección URL donde se enviarán las MDN. |
| **Habilitar NRR** | Sin  | Especifica si se requiere la recepción sin repudio (NRR). Este atributo de comunicación proporciona evidencia que se recibieron los datos según la dirección indicada. |
| **Formato del algoritmo SHA2** | Sin  | Especifica el formato de algoritmo MIC que se usará para iniciar sesión en los encabezados de los mensajes AS2 salientes o MDN |
||||

## <a name="next-steps"></a>Pasos siguientes

[Intercambio de mensajes AS2](../logic-apps/logic-apps-enterprise-integration-as2.md)
