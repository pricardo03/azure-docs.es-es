---
title: Acceso y personalización del nuevo portal para desarrolladores en Azure API Management | Microsoft Docs
description: Aprenda a usar el nuevo portal para desarrolladores de API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: deb3fdf3069aaad4982d71806c209fe516e3c3d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66743581"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Acceso y personalización del nuevo portal para desarrolladores en Azure API Management

Este artículo muestra cómo acceder al nuevo portal para desarrolladores de Azure API Management. Le guiará a través de la experiencia del editor visual: incorporación y edición de contenido, así como personalización del aspecto del sitio web.

![Nuevo portal para desarrolladores de API Management](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Requisitos previos

- Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
- Importe y publique una instancia de Azure API Management. Para más información, consulte [Importación y publicación](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> El nuevo portal para desarrolladores está actualmente en versión preliminar.

## <a name="managed-and-self-hosted-versions"></a>Versiones administradas y autohospedadas

Puede crear su portal para desarrolladores de dos maneras:

- **Versión administrada**: mediante la edición y personalización del portal integrado en la instancia de API Management y accesible a través de la dirección URL `<your-api-management-instance-name>.developer.azure-api.net`.
- **Versión autohospedada**: mediante la implementación y autohospedaje del portal fuera de una instancia de API Management. Este enfoque le permite editar el código base del portal y ampliar la funcionalidad principal proporcionada. Para obtener información detallada e instrucciones, consulte el [repositorio de GitHub con el código fuente del portal][1].

## <a name="access-the-managed-version-of-the-portal"></a>Acceso a la versión administrada del portal

Siga los pasos siguientes para acceder a la versión administrada del portal.

1. Vaya a la instancia de servicio de API Management en Azure Portal.
1. Haga clic en el botón **New developer portal (preview)** (Nuevo portal para desarrolladores: versión preliminar) en la barra de navegación superior. Se abrirá una nueva pestaña del explorador con una versión administrativa del portal. Si accede al portal por primera vez, se aprovisionará automáticamente el contenido predeterminado.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Edición y personalización de la versión administrada del portal

En el siguiente vídeo se muestra cómo editar el contenido del portal, personalizar el aspecto del sitio web y publicar los cambios.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del nuevo portal para desarrolladores:

- [Repositorio de GitHub con el código fuente][1]
- [Instrucciones sobre el autohospedaje del portal][2]
- [Hoja de ruta pública del proyecto][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects