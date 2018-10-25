---
title: Búsqueda de sitio, uso de la interfaz de usuario hospedada de Bing Custom Search
titlesuffix: Azure Cognitive Services
description: Describe cómo configurar la interfaz de usuario hospedada de Bing Custom Search.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 77845cb60dac707326acdb08b0198f8725a36f62
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48813981"
---
# <a name="configure-your-hosted-ui-experience"></a>Configure su experiencia de interfaz de usuario hospedada.

Después de configurar la instancia de búsqueda personalizada, puede llamar a Custom Search API para obtener los resultados de búsqueda y mostrarlos en la aplicación. O bien, si la aplicación es una aplicación web, puede usar la interfaz de usuario hospedada que proporciona Custom Search.   

## <a name="configure-custom-hosted-ui"></a>Configuración de la interfaz de usuario hospedada personalizada

Para configurar una interfaz de usuario hospedada para la aplicación web, siga estos pasos:

1. Inicie sesión en el [portal](https://customsearch.ai) de Custom Search.  
  
2. Haga clic en una instancia de Custom Search. Para crear una instancia, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).  

3. Haga clic en la pestaña **Hosted UI** (UI hospedada).  
  
4. Seleccione un diseño.
  
  - Barra de búsqueda y resultados (valor predeterminado): este diseño es la página de búsqueda tradicional con el cuadro de búsqueda y los resultados de búsqueda.
  - Solo resultados: este diseño muestra solo los resultados de búsqueda. Este diseño no muestra un cuadro de búsqueda. Debe proporcionar la consulta de búsqueda agregando el parámetro de consulta (&q=\<query string>) a la dirección URL de solicitud en el fragmento de código de JavaScript o el vínculo del punto de conexión HTML.
  - Ventana emergente: este diseño proporciona un cuadro de búsqueda y muestra los resultados de búsqueda en una superposición deslizante.
      
5. Seleccione un tema de color. Los temas posibles son: 
  
  - Clásico
  - Oscuro
  - Skyline Blue

  Haga clic en cada uno de los temas para ver qué tema funciona mejor con la aplicación web. Si necesita ajustar el tema de color para integrar mejor su aplicación web, haga clic en **Personalizar tema**. No todas las configuraciones de color se aplican a todos los temas de diseño. Para cambiar un color, escriba el valor HEXADECIMAL RGB del color (por ejemplo, #366eb8) en el cuadro de texto correspondiente. O bien, haga clic en el botón de color y después en el tono que le convenga. 
  
  Después de cambiar un color, compruebe cómo afecta el cambio al ejemplo de vista previa de la derecha. Siempre puede hacer clic en **Restablecer valores predeterminados** para volver a los colores predeterminados del tema seleccionado.

  > [!NOTE]
  > Cuando cambie el tema de color, tenga en cuenta la accesibilidad a la hora de elegir los colores.

5. En **Configuraciones adicionales**, proporcione los valores según corresponda para la aplicación. Estos parámetros son opcionales. Para ver el efecto de aplicarlos o eliminarlos, consulte el panel de vista previa de la derecha. Las opciones de configuración disponibles son las siguientes:  
  
  - Configuraciones de búsqueda web:
    - Resultados web habilitados: determina si está habilitada la búsqueda web (verá una pestaña Web en la parte superior de la página).
    - Habilitar las sugerencias automáticas: determina si están habilitadas las sugerencias automáticas personalizadas (consulte [precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) para conocer los posibles costos adicionales).
    - Resultados web por página: número de resultados de la búsqueda web para mostrar a la vez (el máximo son 50 resultados por página).
    - Leyenda de imagen &mdash; Determina si las imágenes se muestran con resultados de búsqueda.
  
    Las configuraciones siguientes se mostrarán si hace clic en **Mostrar configuraciones avanzadas**.  
  
    - Resaltar palabras: determina si se muestran los resultados con los términos de búsqueda en negrita. 
    - Destino del vínculo: determina si la página web se abre en una nueva pestaña del explorador (en blanco) o en la misma pestaña del explorador (suya propia) cuando el usuario hace clic en un resultado de búsqueda. 

  - Configuraciones de búsqueda de imágenes:
    - Resultados de imagen habilitados: determina si está habilitada la búsqueda de imágenes (verá una pestaña Imágenes en la parte superior de la página).   
    - Resultados de imágenes por página: número de resultados de la búsqueda de imágenes para mostrar a la vez (el máximo son 150 resultados por página).  
  
    La configuración siguiente se mostrará si hace clic en **Mostrar configuraciones avanzadas**.  
  
    - Habilitar filtros: agrega filtros que el usuario puede utilizar para filtrar las imágenes que devuelve Bing. Por ejemplo, el usuario puede filtrar los resultados para que aparezcan solo GIF animados.

  - Configuraciones de búsqueda de vídeos:
    - Resultados de vídeo habilitados: determina si está habilitada la búsqueda de vídeos (verá una pestaña Vídeos en la parte superior de la página).  
    - Resultados de vídeos por página: número de resultados de la búsqueda de vídeos para mostrar a la vez (el máximo son 150 resultados por página).
  
    La configuración siguiente se mostrará si hace clic en **Mostrar configuraciones avanzadas**.  
  
    - Habilitar filtros: agrega filtros que el usuario puede utilizar para filtrar los vídeos que devuelve Bing. Por ejemplo, el usuario puede filtrar los resultados de vídeos con una resolución específica o vídeos detectados en las últimas 24 horas.

  - Configuraciones diversas:
    - Título de página: el texto mostrado en el área de título de la página de resultados de búsqueda (no para el diseño de ventana emergente).
    - Tema de la barra de herramientas: determina el color de fondo del área de título de la página de resultados de la búsqueda.  
  
    Las configuraciones siguientes se mostrarán si hace clic en **Mostrar configuraciones avanzadas**.  
  
    - Marcador de posición de texto del cuadro de búsqueda: texto mostrado en el cuadro de búsqueda antes de la entrada.
    - Dirección URL del vínculo de título: destino para el vínculo del título.
    - URL del logotipo: imagen que se muestra junto al título. 
    - Dirección URL del icono de favoritos: icono que se muestra en la barra de título del explorador.  

    Las siguientes configuraciones se aplican solo si usa la interfaz de usuario hospedada mediante el punto de conexión HTML (no se aplican si usa el fragmento de código de JavaScript).
    
    - Título de la página
    - Tema de la barra de herramientas
    - Dirección URL del vínculo de título
    - URL del logotipo
    - Dirección URL del icono de favoritos  
  
6. Escriba la clave de suscripción de búsqueda o elija una en la lista desplegable. La lista desplegable se completa con las claves de las suscripciones de su cuenta de Azure. Consulte [Creación de una cuenta de Cognitive Services APIs en Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

7. Si habilita las sugerencias automáticas, escriba la clave de suscripción de sugerencias automáticas o elija una en la lista desplegable. La lista desplegable se completa con las claves de las suscripciones de su cuenta de Azure. Custom Autosuggest requiere un nivel de suscripción específico, consulte [precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Cuando realice cambios en la configuración de IU hospedada personalizada, el panel de la derecha proporciona una referencia visual para los cambios realizados. Los resultados de búsqueda mostrados no son los resultados reales de la instancia.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Uso de la IU personalizada

Para utilizar la IU hospedada, elija una de estas dos opciones: 

- Incluir el script en la página web  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- O bien, puede usar la siguiente dirección URL en un explorador web.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Agregue los siguientes parámetros de consulta a la dirección URL según sea necesario. Para más información acerca de estos parámetros, consulte la referencia de [Custom Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).
  >
  > - q
  > - mkt
  > - safesearch
  > - setlang

  > [!IMPORTANT]
  > La página no puede mostrar su declaración de privacidad ni otros avisos y términos. La idoneidad de su uso puede variar.  

Para obtener más información, incluido el identificador de configuración personalizada, vaya a **Puntos de conexión** en la pestaña **Producción**.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)