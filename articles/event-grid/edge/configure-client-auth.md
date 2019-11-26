---
title: 'Configuración de la autenticación de cliente de llamadas entrantes: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configure los protocolos de API expuestos por Event Grid en IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: b5456130e89bf77e2c2ba41880323e38f6b27f4c
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991841"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Configuración de la autenticación de cliente de las llamadas entrantes

En esta guía se proporcionan ejemplos de las posibles configuraciones de autenticación de cliente para el módulo de Event Grid. El módulo de Event Grid admite dos tipos de autenticación de cliente:

* Basada en claves de firma de acceso compartido (SAS).
* Basada en certificados.

Consulte la guía [Seguridad y autenticación](security-authentication.md) para ver todas las configuraciones posibles.

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Habilitación de la autenticación de cliente basada en certificados, sin certificados autofirmados

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Habilitación de la autenticación de cliente basada en certificados, permitir certificados autofirmados

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=false",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Establezca la propiedad **inbound:clientAuth:clientCert:allowUnknownCA** en **true** solo en entornos de prueba, ya que normalmente usará certificados autofirmados. En las cargas de trabajo de producción, se recomienda establecer esta propiedad en **false** y que los certificados provengan de una entidad de certificación (CA).

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Habilitación de la autenticación de cliente basada en certificados y claves SAS

```json
 {
  "Env": [
    "inbound:clientAuth:sasKeys:enabled=true",
    "inbound:clientAuth:sasKeys:key1=<some-secret1-here>",
    "inbound:clientAuth:sasKeys:key2=<some-secret2-here>",
    "inbound:clientAuth:clientCert:enabled=true",
    "inbound:clientAuth:clientCert:source=IoTEdge",
    "inbound:clientAuth:clientCert:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>La autenticación de cliente basada en claves SAS permite que un módulo que no sea de IoT Edge realice operaciones de administración y tiempo de ejecución, suponiendo que se pueda tener acceso a los puertos de API fuera de la red de IoT Edge.
