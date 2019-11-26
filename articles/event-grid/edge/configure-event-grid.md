---
title: 'Configuración: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configuración en Event Grid en IoT Edge
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: a9bac9a84e1ba034b011691ae82a1cb67eb71af0
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991817"
---
# <a name="event-grid-configuration"></a>Configuración de Event Grid

Event Grid proporciona muchas configuraciones que se pueden modificar en función del entorno. La siguiente sección es una referencia a todas las opciones disponibles y sus valores predeterminados.

## <a name="tls-configuration"></a>Configuración de TLS

Para más información sobre la autenticación de cliente en general, consulte [Seguridad y autenticación](security-authentication.md). Puede encontrar ejemplos de su uso en [este artículo](configure-api-protocol.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
|`inbound:serverAuth:tlsPolicy`| Directiva de TLS del módulo de Event Grid. El valor predeterminado es solo HTTPS.
|`inbound:serverAuth:serverCert:source`| El origen del certificado de servidor utilizado por el módulo de Event Grid para su configuración de TLS. El valor predeterminado es IoT Edge.

## <a name="incoming-client-authentication"></a>Autenticación de cliente entrante

Para más información sobre la autenticación de cliente en general, consulte [Seguridad y autenticación](security-authentication.md). Se pueden encontrar ejemplos [en este artículo](configure-client-auth.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
|`inbound:clientAuth:clientCert:enabled`| Para activar/desactivar la autenticación de cliente basada en certificados. El valor predeterminado es true.
|`inbound:clientAuth:clientCert:source`| Origen para validar los certificados de cliente. El valor predeterminado es IoT Edge.
|`inbound:clientAuth:clientCert:allowUnknownCA`| Directiva para permitir un certificado de cliente autofirmado. El valor predeterminado es true.
|`inbound:clientAuth:sasKeys:enabled`| Para activar/desactivar la autenticación de cliente basada en clave SAS. Está desactivado de manera predeterminada.
|`inbound:clientAuth:sasKeys:key1`| Uno de los valores para validar las solicitudes entrantes.
|`inbound:clientAuth:sasKeys:key2`| Segundo valor opcional para validar las solicitudes entrantes.

## <a name="outgoing-client-authentication"></a>Autenticación de cliente saliente
Para más información sobre la autenticación de cliente en general, consulte [Seguridad y autenticación](security-authentication.md). Se pueden encontrar ejemplos [en este artículo](configure-identity-auth.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
|`outbound:clientAuth:clientCert:enabled`| Para activar o desactivar la asociación de un certificado de identidad para las solicitudes salientes. El valor predeterminado es true.
|`outbound:clientAuth:clientCert:source`| Origen para recuperar el certificado saliente del módulo de Event Grid. El valor predeterminado es IoT Edge.

## <a name="webhook-event-handlers"></a>Controladores de eventos de webhook

Para más información sobre la autenticación de cliente en general, consulte [Seguridad y autenticación](security-authentication.md). Se pueden encontrar ejemplos [en este artículo](configure-webhook-subscriber-auth.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
|`outbound:webhook:httpsOnly`| Directiva para controlar si solo se permitirán suscriptores HTTPS. El valor predeterminado es true (solo HTTPS).
|`outbound:webhook:skipServerCertValidation`| Marca para controlar si se debe validar el certificado del suscriptor. El valor predeterminado es true.
|`outbound:webhook:allowUnknownCA`| Directiva para controlar si un certificado autofirmado puede presentarse por un suscriptor. El valor predeterminado es true. 

## <a name="delivery-and-retry"></a>Entrega y nuevo intento

Para obtener información sobre esta característica en general, vea [Entrega y reintento](delivery-retry.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
| `broker:defaultMaxDeliveryAttempts` | Número máximo de intentos de entrega de un evento. El valor predeterminado es 30.
| `broker:defaultEventTimeToLiveInSeconds` | Período de vida (TTL) del evento en segundos transcurrido el cual se descartará un evento si no se entrega. El valor predeterminado es **7200** segundos.

## <a name="output-batching"></a>Procesamiento por lotes de salida

Para obtener información sobre esta característica en general, vea [Entrega y procesamiento por lotes de salida](delivery-output-batching.md).

| Nombre de propiedad | DESCRIPCIÓN |
| ---------------- | ------------ |
| `api:deliveryPolicyLimits:maxBatchSizeInBytes` | Valor máximo permitido para el control `ApproxBatchSizeInBytes`. El valor predeterminado es `1_058_576`.
| `api:deliveryPolicyLimits:maxEventsPerBatch` | Valor máximo permitido para el control `MaxEventsPerBatch`. El valor predeterminado es `50`.
| `broker:defaultMaxBatchSizeInBytes` | Tamaño máximo de la solicitud de entrega cuando solo se especifica `MaxEventsPerBatch`. El valor predeterminado es `1_058_576`.
| `broker:defaultMaxEventsPerBatch` | Número máximo de eventos para agregar a un lote cuando solo se especifica `MaxBatchSizeInBytes`. El valor predeterminado es `10`.
