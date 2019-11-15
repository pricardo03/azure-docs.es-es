---
title: Introducción al portal para desarrolladores de Azure API Management - Azure API Management | Microsoft Docs
description: Obtenga información sobre el portal para desarrolladores de API Management.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 1eef7b6505c4800acbe8aa69cf6f17eecc503aed
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796017"
---
# <a name="azure-api-management-developer-portal-overview"></a>Introducción al portal para desarrolladores de Azure API Management

El portal para desarrolladores es un sitio web totalmente personalizable que se genera automáticamente con la documentación de las API. Aquí, los consumidores de API pueden detectar las API, aprender a usarlas, solicitar acceso y probarlas.

En este artículo se describen las diferencias entre las versiones autohospedadas y administradas del portal para desarrolladores de API Management. También se explica su arquitectura y se proporcionan respuestas a las preguntas más frecuentes.

> [!WARNING]
> El nuevo portal para desarrolladores se está implementando actualmente en los servicios de API Management.
> Si el servicio se ha creado recientemente o es uno de nivel de desarrollador, ya debería tener la versión más reciente. De lo contrario, podría experimentar problemas (por ejemplo, con la funcionalidad de publicación). El lanzamiento de características se completará el lunes 11 de noviembre de 2019.
>
> [Obtenga información sobre cómo migrar desde la versión preliminar a la versión disponible con carácter general](#preview-to-ga) del portal para desarrolladores.

![API Management developer portal](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="managed-vs-self-hosted"></a> Versiones administradas y autohospedadas

Puede crear su portal para desarrolladores de dos maneras:

- **Versión administrada**: mediante la edición y la personalización del portal, que se integra en la instancia de API Management y es accesible a través de la dirección URL `<your-api-management-instance-name>.developer.azure-api.net`. Consulte [este artículo de la documentación](api-management-howto-developer-portal-customize.md) para aprender a acceder al portal administrado y a personalizarlo.
- **Versión autohospedada**: mediante la implementación y autohospedaje del portal fuera de una instancia de API Management. Este enfoque le permite editar el código base del portal y ampliar la funcionalidad principal proporcionada. También debe actualizar manualmente el portal a la versión más reciente. Para información detallada e instrucciones, consulte el [repositorio de GitHub con el código fuente del portal][1]. En el [tutorial de la versión administrada](api-management-howto-developer-portal-customize.md) se recorre el panel administrativo del portal, que también se incluye en la versión autohospedada.

## <a name="portal-architectural-concepts"></a>Conceptos arquitectónicos del portal

Los componentes del portal se pueden dividir lógicamente en dos categorías: *código* y *contenido*.

El *código* se mantiene en [el repositorio de GitHub][1] e incluye lo siguiente:

- Widgets: representan elementos visuales y combinan HTML, JavaScript, estilos, configuración y asignación de contenido. Por ejemplo, una imagen, un párrafo de texto, un formulario o una lista de API.
- Definiciones de estilos: especifican cómo se pueden aplicar estilos a los widgets.
- Motor: genera páginas web estáticas a partir del contenido del portal y está escrito en JavaScript.
- Editor visual: habilita funcionalidades de creación y personalización en el explorador.

El *contenido* se divide en dos subcategorías: *contenido del portal* y *contenido de API Management*.

El *contenido del portal* es específico del portal e incluye lo siguiente:

- Páginas: por ejemplo, página de aterrizaje, tutoriales de API o entradas de blog.
- Elementos multimedia: imágenes, animaciones y otros contenidos basados en archivos.
- Diseños: plantillas, que se comparan con una dirección URL y definen cómo se muestran las páginas.
- Estilos: valores para definiciones de estilo; por ejemplo, fuentes, colores o bordes.
- Ajustes: configuración, por ejemplo, iconos de favoritos (favicon) y metadatos de sitios web.

El *contenido del portal*, a excepción de los elementos multimedia, se expresa como documentos JSON.

El *contenido de API Management* incluye entidades como API, operaciones, productos y suscripciones.

El portal se basa en una bifurcación adaptada del [marco de Paperbits](https://paperbits.io/). La funcionalidad de Paperbits original se ha ampliado para proporcionar widgets específicos de API Management (por ejemplo, una lista de API, una lista de productos, etc.) y un conector al servicio de API Management para guardar y recuperar contenido.

## <a name="faq"></a> Preguntas más frecuentes

En esta sección se responde a preguntas comunes sobre el nuevo portal para desarrolladores, que son de naturaleza general. Si tiene preguntas específicas de la versión autohospedada, consulte la [sección wiki del repositorio de GitHub](https://github.com/Azure/api-management-developer-portal/wiki).

### <a name="a-idpreview-to-ga-how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"/> ¿Cómo puedo migrar desde la versión preliminar del portal?

Al usar la versión preliminar del portal para desarrolladores, se aprovisiona el contenido de versión preliminar en el servicio de API Management. El contenido predeterminado se ha modificado significativamente en la versión disponible con carácter general para mejorar la experiencia del usuario. También se incluyen nuevos widgets.

Si usa la versión administrada, restablezca el contenido del portal; para ello, haga clic en **Restablecer contenido** en la sección del menú **Operaciones**. La confirmación de esta operación quitará todo el contenido del portal y aprovisionará el nuevo contenido predeterminado. El motor del portal se ha actualizado automáticamente en el servicio de API Management.

![Restablecimiento del contenido del portal](media/api-management-howto-developer-portal/reset-content.png)

Si usa la versión autohospedada, use los archivos `scripts/cleanup.bat` y `scripts/generate.bat` del repositorio de GitHub para quitar el contenido existente y aprovisionar el nuevo. No se olvide de actualizar con antelación el código del portal a la versión más reciente del repositorio de GitHub.

Si no desea restablecer el contenido del portal, puede usar los widgets más recientes en todas las páginas. Los widgets existentes se han actualizado automáticamente a las últimas versiones.

Si el portal se aprovisionó después del anuncio de disponibilidad general, ya debería ofrecer el nuevo contenido predeterminado. No es necesario que realice ninguna acción.

### <a name="how-can-i-migrate-from-the-old-developer-portal-to-the-new-developer-portal"></a>¿Cómo puedo migrar el portal para desarrolladores anterior al nuevo?

Los portales son incompatibles y debe migrar el contenido manualmente.

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>¿El nuevo portal tiene todas las características del portal anterior?

El nuevo portal para desarrolladores no es compatible ni con las *aplicaciones* ni con las *incidencias*. Si ha usado *incidencias* en el portal anterior y las necesita en el nuevo, publique un comentario en [una incidencia de GitHub específica](https://github.com/Azure/api-management-developer-portal/issues/122).

### <a name="has-the-old-portal-been-deprecated"></a>¿El portal antiguo quedó en desuso?

Los antiguos portales para desarrolladores y anunciantes ahora son características *heredadas*: solo recibirán actualizaciones de seguridad. Las nuevas características se implementarán solo en el nuevo portal para desarrolladores.

La retirada de los portales heredados se anunciará por separado. Si tiene preguntas, problemas o comentarios, plantéelos [en una incidencia de GitHub específica](https://github.com/Azure/api-management-developer-portal/issues/121).

### <a name="how-can-i-automate-portal-deployments"></a>¿Cómo puedo automatizar las implementaciones del portal?

Puede acceder al contenido del portal para desarrolladores y administrarlo mediante programación a través de la API de REST, independientemente de si está usando una versión administrada o autohospedada.

La API se documenta en [la sección wiki del repositorio de GitHub][2]. También se puede usar para automatizar las migraciones del contenido del portal entre entornos, por ejemplo, de un entorno de prueba al de producción. Puede obtener más información sobre este proceso [en este artículo de la documentación](https://aka.ms/apimdocs/migrateportal) en GitHub.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>¿Admite el portal plantillas de Azure Resource Manager o es compatible con el kit de recursos de DevOps de API Management?

No.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>¿Debo habilitar la conectividad VNET adicional para las dependencias del portal administrado?

No.

### <a name="im-getting-a-cors-error-when-using-the-interactive-console-what-should-i-do"></a>Obtengo un error de CORS al usar la consola interactiva. ¿Cuál debo hacer?

La consola interactiva realiza una solicitud de API del lado cliente desde el explorador. Para resolver el problema de CORS, agregue [una directiva de CORS](https://docs.microsoft.com/azure/api-management/api-management-cross-domain-policies#CORS) en las API. Puede especificar todos los parámetros manualmente o usar valores `*` comodín. Por ejemplo:

```XML
<cors>
    <allowed-origins>
        <origin>*</origin>
    </allowed-origins>
    <allowed-methods>
        <method>GET</method>
        <method>POST</method>
        <method>PUT</method>
        <method>DELETE</method>
        <method>HEAD</method>
        <method>OPTIONS</method>
        <method>PATCH</method>
        <method>TRACE</method>
    </allowed-methods>
    <allowed-headers>
        <header>*</header>
    </allowed-headers>
    <expose-headers>
        <header>*</header>
    </expose-headers>
</cors>
```

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del nuevo portal para desarrolladores:

- [Acceso a la versión administrada del portal para desarrolladores y su personalización](api-management-howto-developer-portal-customize.md)
- [Configuración de la versión autohospedada del portal][2]

Examine otros recursos:

- [Repositorio de GitHub con el código fuente][1]
- [Hoja de ruta pública del proyecto][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects