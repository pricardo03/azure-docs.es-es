---
title: 'Configuración de identidad: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configuración de la identidad del módulo de Event Grid
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 336b6157128468169264d6ffa9564da4d9338aae
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991821"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Configuración de la identidad para el módulo de Event Grid

En este artículo se proporcionan ejemplos de las posibles configuraciones de identidad para un módulo de Event Grid. De forma predeterminada, el módulo de Event Grid presentará su certificado de identidad tal y como lo configuró el demonio de seguridad de IoT. El módulo de Event Grid presenta un certificado de identidad en sus llamadas salientes, es decir, cuando entrega eventos. Un suscriptor a un evento de Event Grid puede elegir entonces validar que es realmente el módulo de Event Grid que envió el evento antes de aceptar el evento.

Consulte la guía [Seguridad y autenticación](security-authentication.md) para ver todas las configuraciones posibles.

## <a name="always-present-identity-certificate"></a>Presentar siempre certificado de identidad
A continuación se muestra un ejemplo de configuración para presentar siempre un certificado de identidad en llamadas salientes. 

```json
 {
  "Env": [
    "outbound:clientAuth:clientCert:enabled=true",
    "outbound:clientAuth:clientCert:source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>No presentar certificado de identidad
A continuación se muestra un ejemplo de configuración para no presentar un certificado de identidad en llamadas salientes. 

```json
 {
  "Env": [
    "outbound:clientAuth:clientCert:enabled=false"
  ]
}
 ```
