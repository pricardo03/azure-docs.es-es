---
title: Obtener acceso y personalizar el nuevo portal para desarrolladores - Azure API Management | Microsoft Docs
description: Obtenga información sobre cómo usar el nuevo portal de desarrollador en API Management.
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
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743581"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Obtener acceso y personalizar el nuevo portal para desarrolladores en Azure API Management

Este artículo muestra cómo acceder al nuevo portal para desarrolladores de Azure API Management. Le guiará a través de la experiencia del editor visual - agregar y editar contenido, así como personalizar el aspecto del sitio Web.

![Nuevo portal para desarrolladores de API Management](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Requisitos previos

- Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
- Importe y publique una instancia de Azure API Management. Para más información, consulte [Importación y publicación](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> El nuevo portal para desarrolladores está actualmente en versión preliminar.

## <a name="managed-and-self-hosted-versions"></a>Versiones autohospedadas y no administrados

Puede crear su portal para desarrolladores de dos maneras:

- **Versión administrada** - de edición y personalización del portal integrada en la instancia de API Management y accesible a través de la dirección URL `<your-api-management-instance-name>.developer.azure-api.net`.
- **Versión autohospedado** : implementando y autoalojamiento su portal fuera de una instancia de API Management. Este enfoque le permite editar el portal de la base de código y ampliar la funcionalidad principal proporcionado. Para obtener información detallada e instrucciones, consulte el [repositorio de GitHub con el código fuente del portal][1].

## <a name="access-the-managed-version-of-the-portal"></a>Obtener acceso a la versión administrada del portal

Siga los pasos siguientes para tener acceso a la versión administrada del portal.

1. Vaya a la instancia del servicio API Management en Azure portal.
1. Haga clic en el **nuevo portal para desarrolladores (versión preliminar)** botón en la barra de navegación superior. Se abrirá una nueva pestaña del explorador con una versión del portal de administración. Si tiene acceso al portal por primera vez, se aprovisionará automáticamente el contenido predeterminado.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Editar y personalizar la versión administrada del portal

En el siguiente vídeo se muestra cómo editar el contenido del portal, personalizar el aspecto del sitio Web y publicar los cambios.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el nuevo portal para desarrolladores:

- [Repositorio de GitHub con el código fuente][1]
- [Obtener instrucciones sobre cómo autoalojamiento del portal][2]
- [Guía básica pública del proyecto][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects