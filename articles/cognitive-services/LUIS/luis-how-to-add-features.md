---
title: Listas de frases para mejorar la detección de entidades
titleSuffix: Azure Cognitive Services
description: Uso de Language Understanding (LUIS) para agregar características de aplicación que pueden mejorar la detección o predicción de intenciones y entidades de categorías y patrones.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 6831f67a33d114ca5c42ddacf8ef4de704e21711
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036963"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Uso de listas de frases para aumentar la señal de la lista de palabras

Puede agregar características a la aplicación de LUIS para mejorar su precisión. Las características ayudan a LUIS al proporcionar sugerencias sobre ciertas palabras y frases que forman parte de un vocabulario de dominio de aplicación. 

## <a name="add-phrase-list"></a>Incorporación de una lista de frases

1. Abra la aplicación con un clic en su nombre en la página **Mis aplicaciones**, haga clic en **Compilar** y luego en **Listas de frases** en el panel izquierdo de la aplicación. 

2. En la página **Listas de frases**, haga clic en **Create new phrase list** (Crear lista de frases). 
 
3. En el cuadro de diálogo **Add Phrase List** (Agregar lista de frases), escriba "Ciudades" como el nombre de la lista de frases. En el cuadro **Value** (Valor), escriba los valores de la lista de frases. Puede escribir un valor a la vez o un conjunto de valores separados por comas y luego presione **ENTRAR**.

    ![Agregar la lista de frases Ciudades](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS puede proponer valores relacionados para agregarlos a la lista de frases. Haga clic en **Recommend** (Recomendar) para obtener un grupo de valores propuestos que están relacionados semánticamente con los valores agregados. Puede haga clic en cualquiera de los valores propuestos, o bien hacer clic en **Add All** (Agregar todo) para agregarlos todos.

    ![Valores propuestos de la lista de frases](./media/luis-add-features/related-values.png)

5. Haga clic en **These values are interchangeable** (Estos valores son intercambiables) si los valores agregados de la lista de frases son alternativas que se pueden usar indistintamente.

    ![Valores propuestos de la lista de frases](./media/luis-add-features/interchangeable.png)

6. Haga clic en **Save**(Guardar). La lista de frases "Ciudades" se agrega a la página **Listas de frases**.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Puede editar, eliminar o desactivar una lista de frases desde el botón de puntos suspensivos (***...***) situado al final de la fila de cada lista de frases.

## <a name="pattern-regular-expression-feature"></a>Característica de patrón (expresión regular) 
**Esta característica está en desuso**. No se pueden agregar características de patrón nuevas a LUIS. Las características de patrón existentes se admiten hasta mayo de 2018. Contribuya a la coincidencia de expresión regular de LUIS estándar con una solicitud al [repositorio Recognizers-Text de Github](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Pasos siguientes

Después de agregar, editar, eliminar o desactivar una lista de frases, vuelva a [entrenar y probar la aplicación](luis-interactive-test.md) para ver si mejora el rendimiento.
