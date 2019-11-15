---
title: Referencia del motor de reglas estándar de Azure CDN | Microsoft Docs
description: Documentación de referencia sobre las condiciones y acciones de coincidencia del motor de reglas estándar de Azure CDN.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: 6fb7e704f3d33cff8c29386b8aba9d8289037cbb
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615835"
---
# <a name="azure-cdn-from-microsoft-rules-engine-reference"></a>Referencia del motor de reglas de Azure CDN de Microsoft

En este artículo se muestran descripciones detalladas de las condiciones y características de coincidencia disponibles para el [motor de reglas estándar](cdn-standard-rules-engine.md) de Azure Content Delivery Network (CDN).

El motor de reglas está diseñado para ser la entidad de certificación final sobre cómo la red CDN procesa tipos específicos de solicitudes.

**Usos habituales**:

- Invalidar o definir una directiva de memoria caché personalizada.
- Redirigir solicitudes.
- Modificación de encabezados de solicitudes y respuestas HTTP comunes

## <a name="terminology"></a>Terminología

Una regla se define mediante el uso de [**condiciones de coincidencia**](cdn-standard-rules-engine-match-conditions.md) y [**acciones**](cdn-standard-rules-engine-actions.md). Estos elementos se resaltan en la siguiente ilustración:

 ![Estructura de reglas de CDN](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Cada regla puede tener hasta 4 condiciones de coincidencia y 3 acciones. Hay un máximo de 5 reglas por cada punto de conexión de CDN. Además, hay una regla vigente de forma predeterminada llamada **regla global**. Se trata de una regla sin condiciones de coincidencia, en la que las acciones definidas dentro siempre se desencadenarán. Esta regla se incluye en el límite actual de 5 reglas.

## <a name="syntax"></a>Sintaxis

La manera en que se tratan los caracteres especiales varía según la forma en la que una condición de coincidencia o acción administra los valores de texto. Una condición de coincidencia o función puede interpretar el texto de una de las siguientes maneras:

1. [**Valores literales**](#literal-values)
2. [**Valores de carácter comodín**](#wildcard-values)


### <a name="literal-values"></a>Valores literales

El texto que se interpreta como un valor literal trata todos los caracteres especiales, excepto el símbolo %, como parte del valor que debe coincidir. En otras palabras, una condición de coincidencia literal establecida en `\'*'\` solo se puede satisfacer cuando se encuentra ese valor exacto (es decir, `\'*'\`).

Se usa un símbolo de porcentaje para indicar la codificación de direcciones URL (p. ej., `%20`).

### <a name="wildcard-values"></a>Valores de carácter comodín

El texto que se interpreta como un valor comodín asigna un significado adicional a los caracteres especiales. En la tabla siguiente se describe cómo se interpreta el siguiente conjunto de caracteres:

Character | DESCRIPCIÓN
----------|------------
\ | Se usa una barra diagonal inversa para el escape de cualquier carácter especificado en esta tabla. Debe especificarse una barra diagonal inversa directamente antes del carácter especial al que debe aplicarse el escape.<br/>Por ejemplo, la sintaxis siguiente aplica el escape a un asterisco: `\*`
% | Se usa un símbolo de porcentaje para indicar la codificación de direcciones URL (p. ej., `%20`).
\* | Un asterisco es un carácter comodín que representa uno o más caracteres.
Espacio | Un carácter de espacio indica que una condición de coincidencia debe cumplirse mediante uno de los patrones o valores especificados.
'valor' | Las comillas simples no tienen un significado especial. Sin embargo, se utiliza un conjunto de comillas simples para indicar que un valor debe tratarse como un valor literal. Puede usarse de las siguientes maneras:<br><br/>- Permite que se cumpla una condición de coincidencia siempre que el valor especificado coincida con cualquier parte del valor de comparación.  Por ejemplo, `'ma'` coincidiría con cualquiera de las siguientes cadenas: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- Permite a un carácter especial que se especifique como un carácter literal. Por ejemplo, puede especificar un carácter de espacio literal mediante la inclusión de un carácter de espacio entre un conjunto de comillas simples (es decir, `' '` o `'sample value'`).<br/>- Permite que se especifique un valor en blanco. Especifique un valor en blanco mediante la especificación de un conjunto de comillas simples (es decir, '').<br /><br/>**Importante:**<br/>-Si el valor especificado no contiene un carácter comodín, entonces se considera automáticamente un valor literal, lo que significa que no es necesario especificar un juego de comillas simples.<br/>- Si una barra diagonal inversa no aplica el escape a otro carácter de esta tabla, se omite cuando se especifique dentro de un conjunto de comillas simples.<br/>- Otra manera de especificar un carácter especial como un carácter literal es aplicarle el escape con una barra diagonal inversa (es decir, `\`).

## <a name="next-steps"></a>Pasos siguientes

- [Condiciones de coincidencia del motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md)
- [Acciones del motor de reglas estándar](cdn-standard-rules-engine-actions.md)
- [Aplicación de HTTPS mediante el motor de reglas estándar](cdn-standard-rules-engine.md)
- [Información general de Azure CDN](cdn-overview.md)
