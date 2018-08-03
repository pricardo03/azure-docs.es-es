---
title: Incorporación de características en aplicaciones de LUIS | Microsoft Docs
description: Uso de Language Understanding (LUIS) para agregar características de aplicación que pueden mejorar la detección o predicción de intenciones y entidades de categorías y patrones.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/30/2018
ms.author: diberry
ms.openlocfilehash: 5ec75436c7df5c08f5507794229bec1f9adb2804
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222960"
---
# <a name="use-features-to-improve-your-luis-apps-performance"></a>Uso de características para mejorar el rendimiento de la aplicación de LUIS  

Puede agregar características a la aplicación de LUIS para mejorar su precisión. Las características ayudan a LUIS al proporcionar sugerencias sobre que ciertas palabras y frases forman parte de una categoría. Si LUIS aprende a reconocer a un miembro de la categoría, puede tratar a los demás de manera similar.

## <a name="add-phrase-list"></a>Incorporación de una lista de frases

1. Abra la aplicación con un clic en su nombre en la página **Mis aplicaciones**, haga clic en **Compilar** y luego en **Listas de frases** en el panel izquierdo de la aplicación. 

    ![Navegación de la lista de frases](./media/luis-add-features/phrase-list-nav.png)

2. En la página **Listas de frases**, haga clic en **Create new phrase list** (Crear lista de frases). 
 
    ![Crear lista de frases](./media/luis-add-features/create-new-phrase-list.png)
    
3. En el cuadro de diálogo **Add Phrase List** (Agregar lista de frases), escriba "Ciudades" como el nombre de la lista de frases. En el cuadro **Value** (Valor), escriba los valores de la lista de frases. Puede escribir un valor a la vez o un conjunto de valores separados por comas y luego presione **ENTRAR**.

    ![Agregar la lista de frases Ciudades](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS puede proponer valores relacionados para agregarlos a la lista de frases. Haga clic en **Recommend** (Recomendar) para obtener un grupo de valores propuestos que están relacionados semánticamente con los valores agregados. Puede haga clic en cualquiera de los valores propuestos, o bien hacer clic en **Add All** (Agregar todo) para agregarlos todos.

    ![Valores propuestos de la lista de frases](./media/luis-add-features/related-values.png)

5. Haga clic en **These values are interchangeable** (Estos valores son intercambiables) si los valores agregados de la lista de frases son alternativas que se pueden usar indistintamente.

    ![Valores propuestos de la lista de frases](./media/luis-add-features/interchangeable.png)

6. Haga clic en **Save**(Guardar). La lista de frases "Ciudades" se agrega a la página **Listas de frases**.

    ![Lista de frases agregada](./media/luis-add-features/phrase-list-cities.png)

## <a name="edit-phrase-list"></a>Edición de la lista de frases

Haga clic en el nombre de la lista de frases en la página **Listas de frases**. En el cuadro de diálogo **Edit Phrase List** (Editar lista de frases) que aparece, haga todos los cambios de edición que sean necesarios y haga clic en **Save** (Guardar).

 ![Lista de frases agregada](./media/luis-add-features/edit-phrase-list.png)

## <a name="delete-phrase-list"></a>Eliminación de la lista de frases 

Haga clic en el botón de puntos suspensivos (***...***) que se encuentra al final de la fila y seleccione **Delete** (Eliminar).

 ![Eliminar lista agregada](./media/luis-add-features/delete-phrase-list.png)

## <a name="deactivate-phrase-list"></a>Desactivación de la lista de frases 

Haga clic en el botón de puntos suspensivos (***...***) que se encuentra al final de la fila y seleccione **Deactivate** (Desactivar).

 ![Desactivar lista agregada](./media/luis-add-features/deactivate-phrase-list.png)

## <a name="pattern-regular-expression-feature"></a>Característica de patrón (expresión regular) 
**Esta característica está en desuso**. No se pueden agregar características de patrón nuevas a LUIS. Las características de patrón existentes se admiten hasta mayo de 2018. Contribuya a la coincidencia de expresión regular de LUIS estándar con una solicitud al [repositorio Recognizers-Text de Github](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Pasos siguientes

Después de agregar, editar, eliminar o desactivar una lista de frases, vuelva a [entrenar y probar la aplicación](luis-interactive-test.md) para ver si mejora el rendimiento.
