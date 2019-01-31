---
title: 'Emparejamiento y alineación de oraciones: Custom Translator'
titleSuffix: Azure Cognitive Services
description: 'Durante la ejecución de un entrenamiento, las oraciones disponibles en los documentos paralelos se emparejan o se alinean. Custom Translator aprende las traducciones una a la vez: primero, lee una oración y, después, lee la traducción de dicha oración. A continuación, alinea entre sí las palabras y frases presentes en ambas oraciones.'
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 663dba72619a14c66fdafe083454d0df9b3f6216
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209934"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Emparejamiento y alineación de oraciones en documentos paralelos

Durante el entrenamiento, las oraciones disponibles en los documentos paralelos se emparejan o se alinean. Custom Translator notifica el número de oraciones que pudo emparejar con el nombre de Oraciones alineadas en cada uno de los conjuntos de datos.

## <a name="pairing-and-alignment-process"></a>Proceso de emparejamiento y alineación

Custom Translator aprende la traducción de las oraciones una a la vez. Primero, lee una oración de origen y, después, la traducción de esa misma oración en el idioma de destino. A continuación, alinea entre sí las palabras y frases presentes en ambas oraciones. Este proceso le permite asignar las palabras y frases en una oración a las palabras y frases equivalentes en la traducción de esa oración. La alineación intenta asegurarse de que el sistema se entrena con oraciones que se corresponden entre sí como traducciones.

## <a name="pre-aligned-documents"></a>Documentos alineados previamente

Si sabe que tiene documentos paralelos, puede invalidar la alineación de oraciones al proporcionar los archivos de texto alineados previamente. Puede extraer todas las oraciones de ambos documentos en un archivo de texto, organizar una oración por línea y cargarlas con la extensión `.align`. La extensión `.align` indica a Custom Translator que debe omitir la alineación de oraciones.

Para obtener mejores resultados, intente asegurarse de que hay una oración por línea en sus archivos. No incluya caracteres de nueva línea en una oración, ya que esto ocasionará alineaciones deficientes.

## <a name="suggested-minimum-number-of-extracted-and-aligned-sentences"></a>Número mínimo sugerido de oraciones extraídas y alineadas

Para que un entrenamiento se realice correctamente, la tabla que se muestra a continuación muestra el número mínimo de oraciones extraídas y alineadas necesario en cada conjunto de datos. El número mínimo sugerido de oraciones extraídas es mucho mayor que el número mínimo sugerido de oraciones alineadas para considerar el hecho de que la alineación de oraciones tal vez no pueda alinear correctamente todas las oraciones extraídas.

| Conjunto de datos   | Número mínimo sugerido de oraciones extraídas | Número mínimo sugerido de oraciones alineadas | Número máximo de oraciones alineadas |
|------------|--------------------------------------------|------------------------------------------|--------------------------------|
| Cursos   | 10 000                                     | 2.000                                    | No hay límite superior                 |
| Ajuste     | 2.000                                      | 500                                      | 2500                          |
| Prueba    | 2.000                                      | 500                                      | 2500                          |
| Diccionario | 0                                          | 0                                        | No hay límite superior                 |

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar un [diccionario](what-is-dictionary.md) en Custom Translator.
