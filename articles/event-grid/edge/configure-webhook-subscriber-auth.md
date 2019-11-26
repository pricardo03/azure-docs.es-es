---
title: 'Configuración de la autenticación del suscriptor de webhook: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configuración de la autenticación de suscriptores de webhook
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 97ed1e2ad84d895e9da0d96cd070e14acb46385d
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991833"
---
# <a name="configure-webhook-subscriber-authentication"></a>Configuración de la autenticación de suscriptores de webhook

En esta guía se proporcionan ejemplos de las posibles configuraciones de suscriptores de webhook para un módulo de Event Grid. De forma predeterminada, solo se aceptan puntos de conexión HTTPS para los suscriptores de webhook. El módulo de Event Grid provocará un rechazo si el suscriptor presenta un certificado autofirmado.

## <a name="allow-only-https-subscriber"></a>Permitir solo suscriptor HTTPS

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>Permitir suscriptor HTTPS con certificado autofirmado

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Establezca la propiedad `outbound:webhook:allowUnknownCA` en `true` solo en entornos de prueba, ya que normalmente usará certificados autofirmados. En el caso de las cargas de trabajo de producción, se recomienda que se establezcan en **false**.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>Permitir suscriptor HTTPS pero omitir la validación de certificados

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=true",
    "outbound:webhook:skipServerCertValidation=true",
    "outbound:webhook:allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Establezca la propiedad `outbound:webhook:skipServerCertValidation` en `true` solo en entornos de prueba, ya que es posible que no esté presentando un certificado que deba autenticarse. En el caso de las cargas de trabajo de producción, se recomienda que se establezcan en **false**.

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Permitir HTTP y HTTPS con certificados autofirmados

```json
 {
  "Env": [
    "outbound:webhook:httpsOnly=false",
    "outbound:webhook:skipServerCertValidation=false",
    "outbound:webhook:allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Establezca la propiedad `outbound:webhook:httpsOnly` en `false` solo en entornos de prueba, ya que es posible que desee empezar con un suscriptor HTTP. En el caso de las cargas de trabajo de producción, se recomienda que se establezcan en **true**.
