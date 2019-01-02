---
title: Listas de frases
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uso de Language Understanding (LUIS) para agregar características de aplicación que pueden mejorar la detección o predicción de intenciones y entidades de categorías y patrones.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 39a6ead041b1783a3effcb9659c59ea788cd2bf6
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139510"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Uso de listas de frases para aumentar la señal de la lista de palabras

Puede agregar características a la aplicación de LUIS para mejorar su precisión. Las características ayudan a LUIS al proporcionar sugerencias sobre ciertas palabras y frases que forman parte de un vocabulario de dominio de aplicación. 

Una [lista de frases](luis-concept-feature.md) incluye un grupo de valores (palabras o frases) que pertenecen a la misma clase y que se deben tratar de forma similar (por ejemplo, nombres de ciudades o productos). Lo que LUIS aprende sobre una de ellas se aplica automáticamente al resto. Esta lista no es una entidad de lista cerrada (coincidencias de texto exactas) de palabras coincidentes.

Una lista de frases se agrega al vocabulario del dominio de aplicación como una segunda señal para LUIS sobre esas palabras.

## <a name="add-phrase-list"></a>Incorporación de una lista de frases

1. Abra la aplicación con un clic en su nombre en la página **Mis aplicaciones**, haga clic en **Compilar** y luego en **Listas de frases** en el panel izquierdo de la aplicación. 

2. En la página **Listas de frases**, haga clic en **Create new phrase list** (Crear lista de frases). 
 
3. En el cuadro de diálogo **Add Phrase List** (Agregar lista de frases), escriba "Ciudades" como el nombre de la lista de frases. En el cuadro **Value** (Valor), escriba los valores de la lista de frases. Puede escribir un valor a la vez o un conjunto de valores separados por comas y luego presione **ENTRAR**.

    ![Agregar la lista de frases Ciudades](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS puede proponer valores relacionados para agregarlos a la lista de frases. Haga clic en **Recommend** (Recomendar) para obtener un grupo de valores propuestos que están relacionados semánticamente con los valores agregados. Puede haga clic en cualquiera de los valores propuestos, o bien hacer clic en **Add All** (Agregar todo) para agregarlos todos.

    ![Valores propuestos de la lista de frases: agregar todos](./media/luis-add-features/related-values.png)

5. Haga clic en **These values are interchangeable** (Estos valores son intercambiables) si los valores agregados de la lista de frases son alternativas que se pueden usar indistintamente.

    ![Valores propuestos de la lista de frases: seleccionar casilla intercambiable ](./media/luis-add-features/interchangeable.png)

6. Haga clic en **Save**(Guardar). La lista de frases "Ciudades" se agrega a la página **Listas de frases**.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Puede eliminar o desactivar una lista de frases desde la barra de herramientas contextual de la página **Lista de frases**.

## <a name="next-steps"></a>Pasos siguientes

Después de agregar, editar, eliminar o desactivar una lista de frases, vuelva a [entrenar y probar la aplicación](luis-interactive-test.md) para ver si mejora el rendimiento.
