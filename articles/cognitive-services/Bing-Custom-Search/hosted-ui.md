---
title: 'Bing Custom Search: búsqueda de sitios | Microsoft Docs'
description: Se describe cómo configurar la interfaz de usuario hospedada.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 593ea4d23f8ddcec8efc4be632afa2aab1a5210f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381010"
---
# <a name="configure-your-hosted-ui-experience"></a>Configure su experiencia de interfaz de usuario hospedada.
Después de configurar la instancia de búsqueda personalizada, puede llamar a Custom Search API para obtener los resultados de búsqueda y mostrarlos en la aplicación. O bien, si la aplicación es una aplicación web, puede usar la interfaz de usuario hospedada que proporciona Custom Search.   

## <a name="configure-custom-hosted-ui"></a>Configuración de la interfaz de usuario hospedada personalizada
Utilice las siguientes instrucciones para configurar una interfaz de usuario hospedada para incluirla en la aplicación web.
1.  Inicie sesión en el [portal](https://customsearch.ai) de Custom Search.
2.  Haga clic en una instancia de Custom Search. Para crear una instancia, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
3.  Haga clic en la pestaña **Hosted UI** (UI hospedada).
4.  Seleccione un diseño.
    - Search bar and results (Barra de búsqueda y resultados) (valor predeterminado) &mdash; Mostrar un cuadro de búsqueda y los resultados de la búsqueda
    - Results only (Solo resultados) &mdash; No mostrar un cuadro de búsqueda, mostrar solo los resultados
    - Pop-over (Ventana emergente) &mdash; No mostrar un cuadro de búsqueda, mostrar solo resultados en una superposición deslizante
    
   > [!IMPORTANT]
   > No olvide incluir el parámetro de consulta customConfig al seleccionar el diseño **Results only** (Solo resultados), consulte [Query parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) (Parámetros de consulta).

5.  En **Configuraciones adicionales**, proporcione los valores según corresponda para la aplicación. Estos parámetros son opcionales. Para ver el efecto de aplicarlos o eliminarlos, consulte el panel de vista previa de la derecha.  Las opciones de configuración disponibles son las siguientes:
    - Configuraciones de búsqueda web:
        - Web results enabled (Resultados web habilitados) &mdash; Determina si se devuelven los resultados de la búsqueda de web
        - Enable autosuggest (Habilitar las sugerencias automáticas) &mdash; Determina si están habilitadas las sugerencias automáticas personalizadas
        - Web results per page (Resultados web por página) &mdash; Número de resultados de la búsqueda web para mostrar a la vez
        - Leyenda de imagen &mdash; Determina si las imágenes se muestran con resultados de búsqueda
        - Highlight words (Resaltar palabras) &mdash; Determina si se muestran los resultados con los términos de búsqueda en negrita
    - Configuraciones de búsqueda de imágenes:
        - Image results enabled (Resultados de imagen habilitados) &mdash; Determina si se devuelven los resultados de la búsqueda de web
    - Configuraciones diversas:
        - Título de página&mdash; Texto mostrado en el área de título de página
        - Toolbar theme (Tema de la barra de herramientas) &mdash; Determina el color de fondo del área de título de página
        - Search box text placeholder (Marcador de posición de texto del cuadro de búsqueda) &mdash; Texto mostrado en el cuadro de búsqueda antes de la entrada
        - Title link url (Dirección URL del vínculo de título) &mdash; Destino para el vínculo del título
        - URL del logotipo &mdash; Imagen que se muestra junto al título 
        - Dirección URL del icono de favoritos&mdash; Icono que se muestra en la barra de título del explorador

   > [!IMPORTANT]
   > Debe habilitarse al menos la búsqueda de imágenes o la búsqueda web.

6.  Escriba la clave de suscripción de búsqueda o elija una en la lista desplegable. La lista desplegable se completa con las claves de las suscripciones de Azure de su cuenta. Consulte [Creación de una cuenta de Cognitive Services APIs en Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Si habilita las sugerencias automáticas, escriba la clave de suscripción de sugerencias automáticas o elija una en la lista desplegable. La lista desplegable se completa con las claves de las suscripciones de Azure de su cuenta. Custom Autosuggest requiere un nivel de suscripción específico, consulte la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Cuando realice cambios en la configuración de IU hospedada personalizada, el panel de la derecha proporciona una referencia visual para los cambios realizados. Los resultados de búsqueda mostrados no son los resultados reales de la instancia.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Uso de la IU personalizada
Para utilizar la IU hospedada, elija una de estas dos opciones: 

- Incluir el script en la página web
    ``` html
    <html>
        <body>
            <script type="text/javascript"
                id="bcs_js_snippet"            
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">            
            </script>
        </body>    
    </html>
    ```

- Usar la dirección URL proporcionada `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > La página no puede mostrar su declaración de privacidad ni otros avisos y términos. La idoneidad de su uso puede variar.  

Para obtener más información, incluido el identificador de configuración personalizada, vaya a **Puntos de conexión** en la pestaña **Producción**.

## <a name="next-steps"></a>Pasos siguientes
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)