---
title: Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure CDN | Microsoft Docs
description: Si utiliza su propio certificado para habilitar HTTPS en un dominio personalizado, debe utilizar una entidad de certificación permitida (CA) para crearlo.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 754941163ddce9512870f0b76a96207472e5b2aa
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593359"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure CDN

Para un dominio personalizado de Azure Content Delivery Network (CDN) en un punto de conexión de **Azure CDN estándar de Microsoft**, al [habilitar la característica HTTPS mediante su propio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), debe usar una entidad de certificación (CA) permitida para crear un certificado SSL. De lo contrario, si usa una entidad de certificación no permitida o un certificado autofirmado, la solicitud se rechazará.

> [!NOTE]
> La opción de usar su propio certificado para habilitar HTTPS personalizado solo está disponible con los perfiles de **Azure CDN estándar de Microsoft**. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

