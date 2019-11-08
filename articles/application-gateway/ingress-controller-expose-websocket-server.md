---
title: Exponer un servidor WebSocket a Application Gateway
description: En este artículo se proporciona información sobre cómo exponer un servidor de WebSocket a una instancia de Application Gateway con el controlador de entrada para los clústeres de AKS.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 01fde82e69917f59f6519524c4c8828feb84a4f9
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795963"
---
# <a name="expose-a-websocket-server-to-application-gateway"></a>Exponer un servidor WebSocket a Application Gateway

Tal como se describe en la documentación de Application Gateway v2, [proporciona compatibilidad nativa con los protocolos WebSocket y HTTP/2](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic). Tenga en cuenta que, tanto en Application Gateway como en la entrada de Kubernetes, no hay ninguna opción configurable por el usuario para habilitar o deshabilitar de forma selectiva la compatibilidad con WebSocket.

En el YAML de implementación de Kubernetes siguiente se muestra la configuración mínima que se usa para implementar un servidor de WebSocket, que es igual que la implementación de un servidor Web normal:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: websocket-server
spec:
  selector:
    matchLabels:
      app: ws-app
  replicas: 2
  template:
    metadata:
      labels:
        app: ws-app
    spec:
      containers:
        - name: websocket-app
          imagePullPolicy: Always
          image: your-container-repo.azurecr.io/websockets-app
          ports:
            - containerPort: 8888
      imagePullSecrets:
        - name: azure-container-registry-credentials

---

apiVersion: v1
kind: Service
metadata:
  name: websocket-app-service
spec:
  selector:
    app: ws-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8888

---

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-repeater
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: ws.contoso.com
      http:
        paths:
          - backend:
              serviceName: websocket-app-service
              servicePort: 80
```

Dado que se cumplen todos los requisitos previos y que tiene una instancia de Application Gateway controlada por una entrada de Kubernetes en AKS, la implementación anterior daría lugar a un servidor WebSocket expuesto en el puerto 80 de la IP pública de su instancia de Application Gateway y el dominio `ws.contoso.com`.

El siguiente comando cURL probaría la implementación del servidor de WebSocket:
```sh
curl -i -N -H "Connection: Upgrade" \
        -H "Upgrade: websocket" \
        -H "Origin: http://localhost" \
        -H "Host: ws.contoso.com" \
        -H "Sec-Websocket-Version: 13" \
        -H "Sec-WebSocket-Key: 123" \
        http://1.2.3.4:80/ws
```

## <a name="websocket-health-probes"></a>Sondeos de estado de WebSocket

Si la implementación no define explícitamente los sondeos de estado, Application Gateway intentará realizar una solicitud HTTP GET en el punto de conexión del servidor de WebSocket.
En función de la implementación del servidor ([aquí hay uno que nos encanta](https://github.com/gorilla/websocket/blob/master/examples/chat/main.go)) se pueden requerir encabezados específicos de WebSocket (`Sec-Websocket-Version` por ejemplo).
Dado que Application Gateway no agrega encabezados de WebSocket, lo más probable es que la respuesta de sondeo de estado de Application Gateway del servidor de WebSocket sea `400 Bad Request`.
Como resultado, Application Gateway marcará los pods como incorrectos, lo que finalmente producirá un `502 Bad Gateway` para los consumidores del servidor de WebSocket.
Para evitar esto, puede que tenga que agregar un controlador de solicitudes HTTP GET para una comprobación de estado en el servidor (`/health` por ejemplo, que devuelve `200 OK`).
