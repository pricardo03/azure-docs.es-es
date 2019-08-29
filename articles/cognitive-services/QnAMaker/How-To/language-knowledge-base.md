---
title: 'Edición de una base de conocimiento en un idioma distinto del inglés: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker admite el contenido de la base de conocimiento en varios idiomas. Sin embargo, cada servicio QnA Maker se debe reservar para un único idioma. La primera base de conocimiento creada para un servicio QnA Maker particular establece el idioma de dicho servicio.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 63eb13dd131fcc1c424c02fdac10f531cc9f0282
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876619"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Compatibilidad de idioma del contenido de la base de conocimiento para QnA Maker

QnA Maker admite el contenido de la base de conocimiento en varios idiomas. Sin embargo, cada servicio QnA Maker se debe reservar para un único idioma. La primera base de conocimiento creada para un servicio QnA Maker particular establece el idioma de dicho servicio. Consulte [aquí](../Overview/languages-supported.md) la lista de idiomas admitidos.

El idioma se reconoce automáticamente a partir del contenido de los orígenes de datos que se van a extraer. Tras crear un servicio QnA Maker y una base de conocimiento en dicho servicio, puede verificar que el idioma se ha establecido correctamente.

1. Acceda a [Azure Portal](https://portal.azure.com/).

1. Seleccione **grupos de recursos** y vaya al grupo de recursos en que se implementa el servicio QnA Maker y seleccione el recurso **Azure Search**.

    ![Selección del recurso Azure Search](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. Seleccione el índice **testkb**. Este índice de Azure Search siempre es el primero que se crea y contiene el contenido guardado de todas las bases de conocimiento de dicho servicio. 

    ![Selección de la base de conocimiento de prueba](../media/qnamaker-how-to-language-kb/select-testkb.png)

1. Seleccione la sección **Campos** que muestra los detalles de _testkb_.

    ![Selección de campos](../media/qnamaker-how-to-language-kb/selectfields.png)

1. Marque la casilla del **analizador** para ver los detalles del idioma.

    ![Selección del analizador](../media/qnamaker-how-to-language-kb/select-analyzer.png)

1. Debería encontrarse con que el _analizador_ está establecido en un idioma determinado. Este idioma se detecta automáticamente durante el paso de creación de la base de conocimiento a partir de las direcciones URL y los archivos importados. Este idioma no se puede cambiar una vez creado el recurso.

    ![Analizador seleccionado](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un bot QnA con Azure Bot Service](../Tutorials/create-qna-bot.md)
