---
title: Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure Front Door Service | Microsoft Docs
description: Si utiliza su propio certificado para habilitar HTTPS en un dominio personalizado, debe utilizar una entidad de certificación permitida (CA) para crearlo.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: e2bab9f9e1ae099952b34e66f7250e7ecbc8e689
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330749"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure Front Door Service

Para un dominio personalizado de Azure Front Door Service, al [habilitar la característica HTTPS mediante su propio certificado](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), debe usar una entidad de certificación (CA) permitida para crear un certificado SSL. De lo contrario, si usa una entidad de certificación no permitida o un certificado autofirmado, la solicitud se rechazará.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
