---
title: Expresiones condicionales del motor de reglas de red CDN de Azure de Verizon Premium | Microsoft Docs
description: Las características y condiciones de coincidencia del motor de reglas de documentación de referencia para Azure CDN de Verizon Premium.
services: cdn
author: mdgattuso
ms.service: cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: c6e49e6fbc0c541ce9a8cd903eb313d61413257c
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481540"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Azure CDN de expresiones condicionales del motor de reglas de Verizon Premium

En este artículo se muestra descripciones detalladas de las expresiones condicionales para Azure Content Delivery Network (CDN) [motor de reglas](cdn-verizon-premium-rules-engine.md).

La primera parte de una regla es la expresión condicional.

Expresión condicional | DESCRIPCIÓN
-----------------------|-------------
IF | Una expresión IF siempre es una parte de la primera instrucción en una regla. Al igual que las demás expresiones condicionales, esta instrucción IF debe asociarse a una coincidencia. Si no se ha definido ninguna expresión condicional adicional, esta coincidencia determina los criterios que deben cumplirse antes de que se puede aplicar un conjunto de características a una solicitud.
AND IF | Una expresión AND IF solo puede agregarse después de los siguientes tipos de expresiones condicionales: IF, AND IF. Indica que hay otra condición que debe cumplirse para la instrucción IF inicial.
ELSE IF| Una expresión ELSE IF especifica una condición alternativa que debe cumplirse antes de que se realice un conjunto de funciones específicas de esta instrucción ELSE IF. La presencia de una instrucción IF ELSE indica el final de la instrucción anterior. La única expresión condicional que se puede colocar después de una instrucción ELSE IF es otra instrucción ELSE IF. Esto significa que una instrucción ELSE IF solo puede usarse para especificar una única condición adicional que debe cumplirse.

**Ejemplo**: ![Condición de coincidencia de red CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Una regla posterior puede invalidar las acciones especificadas por una regla anterior.
   > Ejemplo: Una regla comodín protege todas las solicitudes mediante la autenticación basada en Token. Puede crearse otra regla directamente debajo de ella para realizar una excepción para ciertos tipos de solicitudes.

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Azure CDN](cdn-overview.md)
- [Referencia del motor de reglas](cdn-verizon-premium-rules-engine-reference.md)
- [Condiciones de coincidencia del motor de reglas](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Características del motor de reglas](cdn-verizon-premium-rules-engine-reference-features.md)
- [Invalidación del comportamiento HTTP predeterminado mediante el motor de reglas](cdn-verizon-premium-rules-engine.md)