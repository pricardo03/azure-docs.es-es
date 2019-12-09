---
title: Pasos para importar una aplicación
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 12/03/2019
ms.author: diberry
ms.openlocfilehash: c0253360c66ef6fd995f65e8a83ba5adcdf19543
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806427"
---
1. En el [portal de LUIS en versión preliminar](https://preview.luis.ai), en la página **Mis aplicaciones**, seleccione **Importar** y, a continuación, **Import as JSON** (Importar como JSON). Busque el archivo JSON guardado en el paso anterior. No es necesario cambiar el nombre de la aplicación. Seleccione **Listo**

1. Desde la sección **Administrar**, en la pestaña **Versiones**, seleccione la versión, luego seleccione **Clonar** para clonar la versión y asígnele un nombre de 10 caracteres. A continuación, seleccione **Listo** para finalizar el proceso de clonación. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

    > [!TIP]
    > La clonación de una versión en una nueva versión es un procedimiento recomendado antes de modificar la aplicación. Cuando finalice una versión, exporte la versión (como archivo .json o .lu), y compruebe el archivo en el sistema de control de código fuente.

1. Seleccione **Compilar** y después **Intenciones** para ver las intenciones, los principales bloques de creación de una aplicación de LUIS.

    ![Cambie de la página Versiones a la página Intenciones.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)