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
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: da75ca43a2576e3214d4b67f9eb61c7bad3bd5cc
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70073514"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Acceso y personalización del nuevo portal para desarrolladores en Azure API Management

Este artículo muestra cómo acceder al nuevo portal para desarrolladores de Azure API Management. Le guiará a través de la experiencia del editor visual: incorporación y edición de contenido, así como personalización del aspecto del sitio web.

![Nuevo portal para desarrolladores de API Management](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a> Requisitos previos

- Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
- Importe y publique una instancia de Azure API Management. Para más información, consulte [Importación y publicación](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> El nuevo portal para desarrolladores está actualmente en versión preliminar.

## <a name="managed-vs-self-hosted"></a> Versiones administradas y autohospedadas

Puede crear su portal para desarrolladores de dos maneras:

- **Versión administrada**: mediante la edición y la personalización del portal, que se integra en la instancia de API Management y es accesible a través de la dirección URL `<your-api-management-instance-name>.developer.azure-api.net`.
- **Versión autohospedada**: mediante la implementación y autohospedaje del portal fuera de una instancia de API Management. Este enfoque le permite editar el código base del portal y ampliar la funcionalidad principal proporcionada. Para información detallada e instrucciones, consulte el [repositorio de GitHub con el código fuente del portal][1].

## <a name="managed-access"></a> Acceso a la versión administrada del portal

Siga los pasos siguientes para acceder a la versión administrada del portal.

1. Vaya a la instancia de servicio de API Management en Azure Portal.
1. Haga clic en el botón **New developer portal (preview)** (Nuevo portal para desarrolladores: versión preliminar) en la barra de navegación superior. Se abrirá una nueva pestaña del explorador con una versión administrativa del portal. Si accede al portal por primera vez, se aprovisionará automáticamente el contenido predeterminado.

## <a name="managed-tutorial"></a> Edición y personalización de la versión administrada del portal

En el siguiente vídeo se muestra cómo editar el contenido del portal, personalizar el aspecto del sitio web y publicar los cambios.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="faq"></a> Preguntas más frecuentes

En esta sección se responde a preguntas comunes sobre el nuevo portal para desarrolladores, que son de naturaleza general. Si tiene preguntas específicas de la versión autohospedada, consulte la [sección wiki del repositorio de GitHub](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="how-can-i-migrate-content-from-the-old-developer-portal-to-the-new-one"></a>¿Cómo puedo migrar contenido del portal para desarrolladores anterior al nuevo?

No puede. Los portales no son compatibles.

### <a name="when-will-the-portal-become-generally-available"></a>¿Cuándo estará disponible el portal con carácter general?

El portal se encuentra actualmente en versión preliminar y estará disponible con carácter general al final del año natural (2019). La versión preliminar no debe usarse con fines de producción.

### <a name="will-the-old-portal-be-deprecated"></a>¿El portal antiguo quedará en desuso?

Sí, quedará en desuso después de que el nuevo esté disponible con carácter general. Si tiene problemas, puede plantearlos [en un problema de GitHub dedicado](https://github.com/Azure/api-management-developer-portal/issues/121).

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>¿El nuevo portal tiene todas las características del portal anterior?

El objetivo de la disponibilidad general es proporcionar una paridad de características basada en escenarios con el portal anterior. Hasta entonces, la versión preliminar podría no tener las características seleccionadas implementadas.

Las excepciones son las *aplicaciones* y los *problemas* del portal anterior, que no estarán disponibles en el nuevo portal. Si usa los *problemas* del portal anterior y los necesita en el nuevo, publique un comentario en [un problema de GitHub dedicado](https://github.com/Azure/api-management-developer-portal/issues/122).

### <a name="ive-found-bugs-andor-id-like-to-request-a-feature"></a>He encontrado errores o me gustaría solicitar una característica.

Estupendo. Puede proporcionarnos comentarios, enviar una solicitud de característica o archivar un informe de errores mediante la [sección de problemas del repositorio de GitHub](https://github.com/Azure/api-management-developer-portal/issues). Mientras visita esa sección, también nos gustaría recibir sus comentarios sobre los problemas marcados con la etiqueta `community`.

### <a name="i-want-to-move-the-content-of-the-new-portal-between-environments-how-can-i-do-that-and-do-i-need-to-go-with-the-self-hosted-version"></a>Quiero trasladar el contenido del nuevo portal entre entornos. ¿Cómo puedo hacerlo?, ¿tengo que usar la versión autohospedada?

Puede hacerlo en ambas versiones del portal: administrada y autohospedada. El nuevo portal para desarrolladores admite la extracción de contenido a través de la API de administración del servicio API Management. Las API se documentan [en la sección wiki del repositorio de GitHub](https://github.com/Azure/api-management-developer-portal/wiki/). También hemos escrito [un script](https://github.com/Azure/api-management-developer-portal/blob/master/scripts/migrate.bat), que puede ayudarle a comenzar.

Todavía estamos trabajando para alinear este proceso con el kit de recursos de DevOps de API Management.

### <a name="how-can-i-select-a-layout-when-creating-a-new-page"></a>¿Cómo se puede seleccionar un *diseño* al crear una *página*?

Para aplicar un *diseño* a una página, se hace coincidir la plantilla de dirección URL con la dirección URL de la *página*. Por ejemplo, el *diseño* con una plantilla de dirección URL de `/wiki/*` se aplicará a cada *página* con el segmento `/wiki/`: `/wiki/getting-started`, `/wiki/styles`, y así sucesivamente.

### <a name="why-doesnt-the-interactive-developer-console-work"></a>¿Por qué no funciona la consola interactiva para desarrolladores?

Probablemente esté relacionado con CORS. La consola interactiva realiza una solicitud de API del lado cliente desde el explorador. Para resolver el problema de CORS, agregue [una directiva de CORS](https://docs.microsoft.com/azure/api-management/api-management-cross-domain-policies#CORS) en las API. Puede especificar todos los parámetros manualmente (por ejemplo, origen como https://contoso.com) o usar un valor comodín `*` ).

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del nuevo portal para desarrolladores:

- [Repositorio de GitHub con el código fuente][1]
- [Instrucciones sobre el autohospedaje del portal][2]
- [Hoja de ruta pública del proyecto][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects