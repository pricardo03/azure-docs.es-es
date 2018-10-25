---
title: Descripción del control de versiones en LUIS
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar las versiones para administrar cambios en Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 61be39fc870eb73a5836f8e60a3495aff877d7ed
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637855"
---
# <a name="versions"></a>Versiones
Cree modelos diferentes de la misma aplicación con las [versiones](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Id. de la versión
El identificador de la versión está formado por caracteres, dígitos o ".", y no puede tener más de 10 caracteres.

## <a name="initial-version"></a>Versión inicial
La versión inicial (0.1) es la versión activa predeterminada. 

## <a name="active-version"></a>Versión activa
[Establecer una versión](luis-how-to-manage-versions.md#set-active-version) como activa significa que actualmente se edita y prueba en el sitio web de [LUIS](luis-reference-regions.md). Establezca una versión como activa para tener acceso a sus datos, efectuar actualizaciones, así como para probarla y publicarla.

El nombre de la versión activa se muestra en el panel superior izquierdo, después del nombre de la aplicación. 

[ ![Cambiar la versión activa](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Versiones y espacios de publicación
Puede publicar en los espacios de ensayo y de producción. Cada espacio puede tener otra versión o la misma. Esto es útil para comprobar los cambios entre las versiones de modelo mediante el punto de conexión, que está disponible para bots u otras aplicaciones de llamadas de LUIS. 

## <a name="clone-a-version"></a>Clonar una versión
Clone una versión para crear una copia de una versión existente y guárdela como una versión nueva. Clone una versión para usar el mismo contenido de la versión existente como punto de partida para la versión nueva. Después de clonar una versión, la versión nueva se convertirá en la versión **activa**. 

## <a name="import-and-export-a-version"></a>Importar y exportar una versión
Puede importar una versión en el nivel de la aplicación. Esa versión se convertirá en la versión activa y usará el identificador de versión en la propiedad "versionId" del archivo de la aplicación. También puede importar la versión en el nivel de versión en una aplicación existente. La versión nueva se convertirá en la versión activa. 

Puede exportar una versión en el nivel de aplicación o bien en el nivel de versión. La única diferencia es que la versión exportada en el nivel de aplicación es la versión activa, mientras que en el nivel de versión puede elegir cualquier versión para exportarla en la página **[Configuración](luis-how-to-manage-versions.md)**. 

El archivo exportado no contiene información de aprendizaje automático, puesto que la aplicación se vuelve a entrenar después de importarla. El archivo exportado no contiene colaboradores: debe volver a agregarlos cuando se haya importado la versión en la nueva aplicación.

## <a name="export-each-version-as-app-backup"></a>Exportar cada versión como una copia de seguridad de aplicación
Para poder hacer una copia de seguridad de la aplicación de LUIS, debe exportar cada versión en la página **[Configuración](luis-how-to-manage-versions.md)**.

## <a name="delete-a-version"></a>Eliminar una versión
Puede eliminar todas las versiones, excepto la versión activa, de la lista Versiones de la página Configuración. 

## <a name="version-availability-at-the-endpoint"></a>Disponibilidad de la versión en el punto de conexión
Las versiones entrenadas no están disponibles automáticamente en el [punto de conexión](luis-glossary.md#endpoint) de la aplicación. Debe [publicar](luis-how-to-publish-app.md) o volver a publicar una versión para que esté disponible en el punto de conexión de la aplicación. Puede publicar en la fase de **ensayo** o de **producción**, lo que le ofrece hasta dos versiones de la aplicación disponibles en el punto de conexión. Si necesita que haya disponibles más versiones de la aplicación en un punto de conexión, debe exportar la versión y volver a importarla a una aplicación nueva. La nueva aplicación tiene un identificador de aplicación diferente.

## <a name="collaborators"></a>Colaboradores
El propietario y todos los [colaboradores](luis-how-to-collaborate.md) tienen acceso total a todas las versiones de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Vea cómo agregar el [control de versiones](luis-how-to-manage-versions.md) en la página de configuración de la aplicación. 

Obtenga información sobre cómo diseñar [intenciones](luis-concept-intent.md) en el modelo.