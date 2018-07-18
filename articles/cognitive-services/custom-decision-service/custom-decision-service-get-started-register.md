---
title: 'Registrar la aplicación: Azure Cognitive Services | Microsoft Docs'
description: Guía detallada sobre el registro de una aplicación nueva con Azure Custom Decision Service
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo;alekh;marossi
ms.openlocfilehash: 2aa8fbe77c11df4434eefa4c92d8529d5ca1d885
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382102"
---
# <a name="register-your-application"></a>Registrar su aplicación

Para utilizar Custom Decision Service para su aplicación, debe registrarlo en el portal. En este artículo se explica cómo.

1. Vaya a la [página principal](https://ds.microsoft.com/) de Custom Decision Service. En la cinta de opciones, haga clic en **My Portal** (Mi portal), como se resalta en la imagen:

    ![Mi portal](./media/portal.png)

    Si no aún ha iniciado sesión, el portal le pedirá que la inicie con su [cuenta de Microsoft](https://account.microsoft.com/account). Una vez iniciada la sesión, el portal muestra su cuenta de Microsoft en la esquina superior derecha de la página.

2. Para registrar la aplicación, haga clic en el botón **Nueva aplicación**.

3. En el cuadro de diálogo, elija un id. de aplicación para su aplicación. Custom Decision Service requiere un id. único para cada aplicación. Si alguien ya eligió este id., el sistema le pedirá que selecciona uno diferente.

4. Especificar una API de conjunto de acciones. Esta configuración es una fuente RSS o Atom que comunica el contenido disponible para su aplicación a Custom Decision Service. Escriba un nombre para la fuente y especifique la dirección URL desde la que se sirve. Para realizar este paso más adelante, haga clic en el botón **Fuentes** y, a continuación, haga clic en el botón **Fuente nueva**. Más adelante se describe un ejemplo que crea una fuente RSS.

5. Para registrar la aplicación, active la casilla **Aplicación personalizada** de la esquina inferior izquierda. Escriba un [cadena de conexión](../../storage/common/storage-configure-connection-string.md) para la cuenta de Azure Storage en la que están registrados los datos de la aplicación. Para obtener más información acerca de la creación de una cuenta de almacenamiento, consulte [Creación, administración o eliminación de una cuenta de almacenamiento](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Pasos siguientes

* Empiece a optimizar [una página web](custom-decision-service-get-started-browser.md) o [una aplicación de smartphone](custom-decision-service-get-started-app.md).
* Complete un [tutorial](custom-decision-service-tutorial-news.md) para obtener un ejemplo más detallado.
* Consulte la [referencia de la API](custom-decision-service-api-reference.md) para obtener más información sobre la funcionalidad que proporciona.