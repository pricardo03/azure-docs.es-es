---
title: Referencia del motor de reglas de Azure CDN | Microsoft Docs
description: Documentación de referencia sobre las condiciones y características de coincidencia del motor de reglas de Azure CDN.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 5fc611af75a7f733576f9343a4375fb56cacc030
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593161"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Referencia del motor de reglas de Azure CDN de Verizon Premium

En este artículo se muestran descripciones detalladas de las condiciones de coincidencia disponibles para el [motor de reglas](cdn-verizon-premium-rules-engine.md) de Azure Content Delivery Network (CDN).

El motor de reglas está diseñado para ser la entidad de certificación final sobre cómo la red CDN procesa tipos específicos de solicitudes.

**Usos habituales**:

- Invalidar o definir una directiva de memoria caché personalizada.
- Proteger o denegar las solicitudes de contenido confidencial.
- Redirigir solicitudes.
- Almacenar datos de registro personalizados.

## <a name="terminology"></a>Terminología

Una regla se define mediante el uso de [**expresiones condicionales**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**coincidencias**](cdn-verizon-premium-rules-engine-reference-match-conditions.md) y [**características**](cdn-verizon-premium-rules-engine-reference-features.md). Estos elementos se resaltan en la siguiente ilustración:

 ![Condición de coincidencia de red CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sintaxis

La manera en que se tratarán los caracteres especiales varía según la forma en la que una condición de coincidencia o función administra los valores de texto. Una condición de coincidencia o función puede interpretar el texto de una de las siguientes maneras:

1. [**Valores literales**](#literal-values)
2. [**Valores de carácter comodín**](#wildcard-values)
3. [**Expresiones regulares**](#regular-expressions)

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

### <a name="regular-expressions"></a>Expresiones regulares

Las expresiones regulares definen un patrón que se busca dentro de un valor de texto. La notación de expresiones regulares define significados concretos en una variedad de símbolos. La tabla siguiente indica cómo se tratan los caracteres especiales mediante las condiciones de coincidencia y funciones que admiten expresiones regulares.

Carácter especial | DESCRIPCIÓN
------------------|------------
\ | Una barra diagonal inversa aplica el escape al carácter que le sigue, lo que hace que ese carácter se trate como un valor literal en lugar de tomar el significado de su expresión regular. Por ejemplo, la sintaxis siguiente aplica el escape a un asterisco: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | An asterisk allows the preceding character to be matched zero or more times.
Space | A space character is typically treated as a literal character.
'value' | Single quotes are treated as literal characters. A set of single quotes does not have special meaning.

## Next steps

- [Rules engine match conditions](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Override HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)
- [Azure CDN overview](cdn-overview.mdEl significado de un símbolo de porcentaje depende de su uso. Docs
descr`%{HTTPVariable}`: esta sintaxis identifica una variable HTTP.match`%{HTTPVariable%Pattern}`: esta sintaxis utiliza un símbolo de porcentaje para identificar una variable HTTP y se usa como delimitador.1/2019`\``: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\`20`).f the available match conditions and features for the Azure Content Delivery Network (CDN) [rules engine](cdn-verizon-premium-rules-engine.md).

The rules engine is designed to be the final authority on how specific types of requests are processed by the CDN.

**Common uses**:

- Override or define a custom cache policy.
- Secure or deny requests for sensitive content.
- Redirect requests.
- Store custom log data.

## Terminology

A rule is defined through the use of [**conditional expressions**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**match conditions**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), and [**features**](cdn-verizon-premium-rules-engine-reference-features.md). These elements are highlighted in the following illustration:

 ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## Syntax

The manner in which special characters are treated varies according to how a match condition or feature handles text values. A match condition or feature may interpret text in one of the following ways:

1. [**Literal values**](#literal-values)
2. [**Wildcard values**](#wildcard-values)
3. [**Regular expressions**](#regular-expressions)

### Literal values

Text that is interpreted as a literal value treats all special characters, with the exception of the % symbol, as a part of the value that must be matched. In other words, a literal match condition set to `\'*'\` is only satisfied when that exact value (that is, `\'*'\`) is found.

A percentage symbol is used to indicate URL encoding (for example, `%20`).

### Wildcard values

Text that is interpreted as a wildcard value assigns additional meaning to special characters. The following table describes how the following set of characters is interpreted:

Character | Description
----------|------------
\ | A backslash is used to escape any of the characters specified in this table. A backslash must be specified directly before the special character that should be escaped.<br/>For example, the following syntax escapes an asterisk: `\*`
% | A percentage symbol is used to indicate URL encoding (for example, `%20`).
\* | An asterisk is a wildcard that represents one or more characters.
Space | A space character indicates that a match condition may be satisfied by either of the specified values or patterns.
'value' | A single quote does not have special meaning. However, a set of single quotes is used to indicate that a value should be treated as a literal value. It can be used in the following ways:<br><br/>- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.  For example, `'ma'` would match any of the following strings: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- It allows a special character to be specified as a literal character. For example, you may specify a literal space character by enclosing a space character within a set of single quotes (that is, `' '` or `'sample value'`).<br/>- It allows a blank value to be specified. Specify a blank value by specifying a set of single quotes (that is, '').<br /><br/>**Important:**<br/>- If the specified value does not contain a wildcard, then it is automatically considered a literal value, which means that it is not necessary to specify a set of single quotes.<br/>- If a backslash does not escape another character in this table, it is ignored when it is specified within a set of single quotes.<br/>- Another way to specify a special character as a literal character is to escape it using a backslash (that is, `\`).

### Regular expressions

Regular expressions define a pattern that is searched for within a text value. Regular expression notation defines specific meanings to a variety of symbols. The following table indicates how special characters are treated by match conditions and features that support regular expressions.

Special Character | Description
------------------|------------
\ | A backslash escapes the character the follows it, which causes that character to be treated as a literal value instead of taking on its regular expression meaning. For example, the following syntax escapes an asterisk: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | Un asterisco permite que el carácter anterior coincida con cero o más veces.
Espacio | Normalmente, un carácter de espacio se trata como un carácter literal.
'valor' | Las comillas simples se tratan como caracteres literales. Un conjunto de comillas simples no tiene un significado especial.

## <a name="next-steps"></a>Pasos siguientes

- [Condiciones de coincidencia del motor de reglas](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Expresiones condicionales del motor de reglas](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Características del motor de reglas](cdn-verizon-premium-rules-engine-reference-features.md)
- [Invalidación del comportamiento HTTP mediante el motor de reglas](cdn-verizon-premium-rules-engine.md)
- [Información general de Azure CDN](cdn-overview.md)