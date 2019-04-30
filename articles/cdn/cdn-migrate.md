---
title: Migrar un perfil de Azure CDN de Verizon estándar a Verizon premium | Microsoft Docs
description: Obtenga información sobre cómo migrar un perfil de Verizon estándar a Verizon premium.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: 7768dde424aedc295b53512db50c9dfc9db9ab8c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635642"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Migrar un perfil de Azure CDN de Verizon estándar a Verizon premium

Cuando crea un perfil de Azure Content Delivery Network (CDN) para administrar sus puntos de conexión, Azure CDN le ofrece cuatro productos diferentes entre los que elegir. Para obtener información sobre los diferentes productos y sus características disponibles, consulte [Comparación de las características de los productos de Azure CDN](cdn-features.md).

Si ha creado un perfil **estándar de Azure CDN de Verizon** y lo está usando para administrar sus puntos de conexión de CDN, tiene la opción de actualizarlo a un perfil **premium de Azure CDN de Verizon** . Cuando lo actualice, se conservarán sus puntos de conexión de CDN y todos sus datos. 

> [!IMPORTANT]
> Una vez que haya actualizado a un perfil **premium de Azure CDN de Verizon**, no podrá volver a la versión **estándar de Azure CDN de Verizon**.
> 

Para actualizar un perfil **estándar de Azure CDN de Verizon**, póngase en contacto con el [soporte técnico de Microsoft ](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Comparación de perfiles
Los perfiles **premium de Azure CDN de Verizon** tienen las siguientes diferencias clave, en comparación con los perfiles **estándar de Azure CDN de Verizon**:
- En determinadas características de Azure CDN, como la [compresión](cdn-improve-performance.md), las [reglas de almacenamiento en caché](cdn-caching-rules.md) y el [filtrado geográfico](cdn-restrict-access-by-country.md), no puede usar la interfaz de Azure CDN, si no que debe usar el portal de Verizon mediante el botón **Administrar**.
- API: A diferencia de con Verizon estándar, no se puede usar la API para controlar las características que se obtiene acceso desde el portal de Verizon Premium. Sin embargo, puede usar la API para controlar otras características comunes, como crear o eliminar un punto de conexión, purgar o cargar activos en la caché y habilitar o deshabilitar un dominio personalizado.
- Precios: Verizon Premium tiene una estructura de precios diferente para las transferencias de datos de Verizon estándar. Para más información, consulte [Precios de Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

Los perfiles **premium de Azure CDN de Verizon** tienen las siguientes características adicionales:
- [Autenticación de token](cdn-token-auth.md): Permite a los usuarios obtener y usar un token para recuperar recursos seguros.
- [Motor de reglas](cdn-rules-engine.md): Le permite personalizar cómo se controlan las solicitudes HTTP.
- Herramientas de análisis avanzadas:
   - [Análisis HTTP detallado](cdn-advanced-http-reports.md)
   - [Análisis de rendimiento de Edge ](cdn-edge-performance.md)
   - [Análisis en tiempo real](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el motor de reglas, consulte la [referencia del motor de reglas de Azure CDN](cdn-rules-engine-reference.md).

