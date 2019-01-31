---
title: 'Creación, entrenamiento y publicación de la base de conocimientos: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Puede crear una base de conocimiento (KB) de QnA Maker a partir de contenido propio, como por ejemplo preguntas más frecuentes o manuales de productos. La base de conocimiento de QnA Maker de este ejemplo se crea a partir de una página web sencilla de preguntas más frecuentes sobre la recuperación de claves de BitLocker.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 41a8f11f817ec550ef3423d85eb107796bdd1b28
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216955"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Creación, entrenamiento y publicación de la base de conocimiento de QnA Maker

Puede crear una base de conocimiento (KB) de QnA Maker a partir de contenido propio, como por ejemplo preguntas más frecuentes o manuales de productos. La base de conocimiento de QnA Maker de este ejemplo se crea a partir de una página web sencilla de preguntas más frecuentes sobre la recuperación de claves de BitLocker.

## <a name="prerequisite"></a>Requisito previo

> [!div class="checklist"]
> * Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-qna-maker-knowledge-base"></a>Creación de una base de conocimiento de QnA Maker

1. Inicie sesión en QnAMaker.ai con las credenciales de Azure.

2. En el sitio web de QnA Maker, haga clic en **Create a knowledge base** (Crear una base de conocimiento).

   ![Creación de una base de conocimiento](../media/qna-maker-create-kb.png)

3. En la página **Create** (Crear), en el paso 1, haga clic en **Create a QnA service** (Crear un servicio QnA). Se le dirigirá a [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) para configurar un servicio QnA Maker en la suscripción. Si se agota el tiempo de espera de Azure Portal, haga clic en **Intentar de nuevo** en el sitio. Después de conectarse, aparecerá el panel de Azure.

4. Después de crear correctamente un servicio QnA Maker en Azure, vuelva a qnamaker.ai/create. Seleccione el servicio QnA en las listas desplegables del paso 2. Si ha creado un servicio QnA nuevo, asegúrese de actualizar la página.

   ![Selección de una base de conocimiento de servicio QnA](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. En el paso 3, asigne el nombre **Mi KB de QnA de ejemplo** a la base de conocimiento.

6. Para agregar contenido a la KB, seleccione tres tipos de orígenes de datos. En el paso 4, en **Populate your KB** (Rellenar la base de conocimiento), agregue la dirección URL de [Preguntas más frecuentes sobre recuperación de BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) en el cuadro **URL**.

   ![Selección de una base de conocimiento de servicio QnA](../media/qnamaker-quickstart-kb/add-datasources.png)

7. En el paso 5, haga clic en **Create your KB** (Crear la base de conocimiento).

8. Mientras se crea la base de conocimiento, aparece una ventana emergente. El proceso de extracción tarda unos minutos en leer la página HTML e identificar las preguntas y respuestas.

9. Una vez que se haya creado correctamente la base de conocimiento, se abre la página **Knowledge base** (Base de conocimiento). En esta página puede modificar el contenido de la base de conocimiento.

10. En la esquina superior derecha, haga clic en **Add QnA pair** (Agregar par de QnA) para agregar una fila nueva en la sección **Editorial** de la base de conocimiento. Bajo **Question** (Pregunta), escriba **Hi.** (Hola). Bajo **Answer** (Respuesta), escriba **Hello. Ask me bitlocker questions.** (Hola. Hazme preguntas sobre BitLocker).

   ![Adición de un par de QnA](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. En la esquina superior derecha, haga clic en **Save and train** (Guardar y entrenar) para guardar las modificaciones y entrenar el modelo de QnA Maker. Las modificaciones no se conservan a menos que se guarden.

12. En la esquina superior derecha, haga clic en **Test** (Probar) para comprobar que los cambios realizados han surtido efecto. Escriba `hi there` (hola) en el cuadro y seleccione Entrar. Debería ver la respuesta que creó como respuesta.

13. Haga clic en **Inspect** (Inspeccionar) para examinar la respuesta con más detalle. La ventana de prueba se usa para probar los cambios realizados en la base de conocimiento antes de que se publiquen.

   ![Panel de prueba](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Vuelva a hacer clic en **Test** (Prueba) para cerrar el panel **Test**.

15. En el menú situado junto a **Edit** (Editar), haga clic en **Publish** (Publicar). Después, para confirmar, haga clic en **Publish** (Publicar) en la página.

16. El servicio QnA Maker se publica correctamente. Puede usar el punto de conexión del código de la aplicación o el bot.

   ![Publicar](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una base de conocimientos](../How-To/create-knowledge-base.md)
