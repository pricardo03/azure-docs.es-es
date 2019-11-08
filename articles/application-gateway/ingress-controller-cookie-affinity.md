---
title: Habilitar la afinidad basada en cookies con Application Gateway
description: En este artículo se proporciona información sobre cómo habilitar la afinidad basada en cookies con una instancia de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2019
ms.author: caya
ms.openlocfilehash: cb1a403888f8dca78d1af7395095aa2bac39935e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510513"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Habilitar la afinidad basada en cookies con una instancia de Application Gateway
Como se describe en la [Documentación de Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings), Application Gateway es compatible con la afinidad basada en cookies, lo que significa que puede dirigir el tráfico posterior de una sesión de usuario hasta el mismo servidor para su procesamiento.

## <a name="example"></a>Ejemplo
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```
