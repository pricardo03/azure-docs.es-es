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
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5e50c814fef24aa799549d055ad6496f5bdf05e0
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961501"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Compatibilidad de idioma del contenido de la base de conocimiento para QnA Maker

QnA Maker admite el contenido de la base de conocimiento en varios idiomas. Sin embargo, cada servicio QnA Maker se debe reservar para un único idioma. La primera base de conocimiento creada para un servicio QnA Maker particular establece el idioma de dicho servicio. Consulte [aquí](../Overview/languages-supported.md) la lista de idiomas admitidos.

El idioma se reconoce automáticamente a partir del contenido de los orígenes de datos que se van a extraer. Tras crear un servicio QnA Maker y una base de conocimiento en dicho servicio, puede verificar que el idioma se ha establecido correctamente.

1. Acceda a [Azure Portal](https://portal.azure.com/).

1. Seleccione **grupos de recursos** y vaya al grupo de recursos en que se implementa el servicio QnA Maker y seleccione el recurso **Azure Search**.

    ![Selección del recurso Azure Search](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. Seleccione **Índices** y, a continuación, seleccione el índice **testkb**. Este es el primer índice de Azure Search que se crea y contiene el contenido guardado de todas las bases de conocimiento de dicho servicio. 

1. Seleccione **Campos** para ver los campos en el índice.

1. La columna _Analizador_ de los campos `questions` y `answer` está establecida en un idioma específico. Este idioma se detecta automáticamente durante el paso de creación de la base de conocimiento a partir de las direcciones URL y los archivos importados. Este idioma no se puede cambiar una vez creado el recurso.

    ![Analizador seleccionado](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un bot QnA con Azure Bot Service](../Tutorials/create-qna-bot.md)
