---
title: 'Bing Custom Search: creación de una página web de búsqueda personalizada | Microsoft Docs'
description: Se describe cómo configurar una instancia de búsqueda personalizada e integrarla en una página web.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: 1f9b689ac6127bc2f7d1e810356ae9a23b8e0996
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162400"
---
# <a name="build-a-custom-search-web-page"></a>Creación de una página web de Custom Search
Bing Custom Search le permite crear experiencias de búsqueda a medida de los temas que le interesan. Por ejemplo, si tiene un sitio web de artes marciales que proporciona una experiencia de búsqueda, puede especificar los dominios, subsitios y páginas web en las que busque Bing. Los usuarios ven los resultados de búsqueda adaptados al contenido que les interesa en lugar de recorrer las páginas de resultados de búsqueda generales que pueden contener contenido carente de interés. 

En este tutorial se muestra cómo configurar una instancia de búsqueda personalizada e integrarla en una nueva página web.

Las tareas que se tratan son:

> [!div class="checklist"]
> - Crear una instancia de búsqueda personalizada
> - Agregar entradas activas
> - Agregar entradas bloqueadas
> - Agregar entradas ancladas
> - Integrar la búsqueda personalizada en una página web

## <a name="prerequisites"></a>Requisitos previos
- Para seguir con el tutorial, necesita una clave de suscripción para Bing Custom Search API.  Para obtener una clave, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Si aún no tiene Visual Studio de 2017 instalado, puede descargar y usar la versión **gratis** de [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).

## <a name="create-a-custom-search-instance"></a>Creación de una instancia de búsqueda personalizada
Para crear una instancia de Bing Custom Search:

1.  Abra un explorador de Internet.
2.  Navegue al [portal](https://customsearch.ai) de Custom Search.
3.  Inicie sesión en el portal mediante una cuenta Microsoft (MSA). Si no tiene una MSA, haga clic en **Create a Microsoft account** (Crear una cuenta Microsoft). Si es la primera vez que usa el portal, se le pedirán los permisos para acceder a los datos. Haga clic en **Sí**.
4.  Después de iniciar sesión, haga clic en **New custom search** (Nueva búsqueda personalizada). En la ventana **Create a new custom search instance** (Crear una instancia de búsqueda personalizada), escriba un nombre que sea significativo y que describa el tipo de contenido que devuelve la búsqueda. Puede cambiar el nombre en cualquier momento.
 
    ![Captura de pantalla del cuadro de creación de una instancia de búsqueda personalizada](../media/newCustomSrch.png)

5.  Haga clic en OK (Aceptar), especifique una dirección URL e indique si se incluirán subpáginas en la página base:

    ![Captura de pantalla de la página de definición de dirección URL](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Agregar entradas activas
Para incluir resultados de determinados sitios o direcciones URL, agréguelos a la pestaña **Active** (Activo).

1.  En **Definition Editor** (Editor de definiciones), haga clic en la pestaña **Active** (Activo) y escriba la dirección URL de uno o varios sitios que quiera incluir en la búsqueda.

    ![Captura de pantalla de la pestaña elementos activos del editor de definiciones](../media/customSrchEditor.png)

2.  Para confirmar que la instancia devuelve resultados, escriba una consulta en el panel de vista previa de la derecha. Bing devuelve resultados solo de sitios públicos que se han indexado.

## <a name="add-blocked-entries"></a>Agregar entradas bloqueadas
Para excluir resultados de determinados sitios o direcciones URL, agréguelos a la pestaña **Blocked** (Bloqueado).

1. En **Definition Editor** (Editor de definiciones), haga clic en la pestaña **Blocked** (Bloqueado) y escriba la dirección URL de uno o varios sitios que quiera excluir de la búsqueda.

    ![Captura de pantalla de la pestaña de elementos bloqueados del editor de definiciones](../media/blockedCustomSrch.png)


2. Para confirmar que la instancia no devuelve resultados de los sitios bloqueados, escriba una consulta en el panel de vista previa de la derecha. 

## <a name="add-pinned-entries"></a>Agregar entradas ancladas
Para anclar una página web específica a la parte superior de los resultados de búsqueda, agregue la página web y el término de consulta a la pestaña **Pinned** (Anclado).  La pestaña **Pinned** (Anclado) contiene una lista de pares de término de consulta y página web que especifican la página web que aparece como mejor resultado para una consulta específica. El término de consulta del usuario debe coincidir exactamente con el término de consulta anclado para la página web que se va a anclar al principio.

1. En **Definition Editor** (Editor de definiciones), haga clic en la pestaña **Pinned** (Anclado) y escriba la página web y el término de consulta de la página web que se debe devolver como mejor resultado.

    ![Captura de pantalla de la pestaña de elementos anclados del editor de definiciones](../media/pinnedCustomSrch.png)

2. Para confirmar que la instancia devuelve la página web especificada como mejor resultado, escriba el término de consulta que ancló en el panel de vista previa de la derecha.

## <a name="configure-hosted-ui"></a>Configuración de la interfaz de usuario hospedada
Custom Search proporciona una interfaz de usuario hospedada para presentar la respuesta JSON de la instancia de búsqueda personalizada.  Para definir la experiencia de interfaz de usuario:

1. Haga clic en la pestaña **Hosted UI** (UI hospedada).
2. Seleccione un diseño.

    ![Captura de pantalla del paso de selección de usuario de la UI hospedada](./media/custom-search-hosted-ui-select-layout.png)

3. Seleccione un tema de color.

    ![Captura de pantalla del paso de selección de usuario de la UI hospedada](./media/custom-search-hosted-ui-select-color-theme.png)

4. Especifique opciones de configuración adicionales.

    ![Captura de pantalla del paso de configuraciones adicionales de la UI hospedada](./media/custom-search-hosted-ui-additional-configurations.png)

5. Pegue la **clave de suscripción**. Consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Captura de pantalla del paso de configuraciones adicionales de la UI hospedada](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE [publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Consumo de la UI hospedada
Hay dos maneras de consumir la interfaz de usuario hospedada.  

- Opción 1: Integrar el fragmento de código de JavaScript proporcionado en la aplicación.
- Opción 2: Usar el punto de conexión HTML proporcionado.

En el resto del tutorial se ilustra la **Opción 1: Fragmento de código Javascript**.  

## <a name="set-up-your-visual-studio-solution"></a>Configuración de la solución de Visual Studio
1. Abra **Visual Studio** en el equipo.
2. En el menú **Archivo**, seleccione **Nuevo** y elija **Proyecto**.
3. En la ventana **Nuevo proyecto**, seleccione **Visual C#/Web/Aplicación web ASP.NET Core**, nombre el proyecto y, luego, haga clic en **OK** (Aceptar).
   
    ![Captura de pantalla de la ventana Nuevo proyecto](./media/custom-search-new-project.png)

4. En la ventana **Nueva aplicación web ASP.NET Core**, seleccione **Aplicación web** y haga clic en **Aceptar**.
    
    ![Captura de pantalla de la ventana Nuevo proyecto](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Edite index.cshtml.
1. En el **Explorador de soluciones**, expanda **Páginas** y haga doble clic en **index.cshtml** para abrir el archivo.

    ![Captura de pantalla del Explorador de soluciones con páginas expandidas e index.cshtml seleccionado](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. En index.cshtml, elimine todo desde la línea 7 en adelante.
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Agregue un elemento de salto de línea y una etiqueta div para que actúe como contenedor.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. En la pestaña **Hosted UI** (UI hospedada), desplácese a la sección llamada **Consuming the UI** (Consumo de UI). Copie el fragmento de código de JavaScript.

    ![Captura de pantalla del botón para guardar de la interfaz de usuario hospedada](./media/custom-search-hosted-ui-consuming-ui.png)

5. Pegue el elemento de script en el contenedor que agregó.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch">
        <script type="text/javascript"
                id="bcs_js_snippet"
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">
        </script>
    </div>
    ```

6. En el **Explorador de soluciones**, haga clic con el botón derecho en **wwwroot** y haga clic en **View in Browser** (Ver en el explorador).

    ![Captura de pantalla del Explorador de soluciones al seleccionar la vista en el explorador en el menú contextual de wwwroot](./media/custom-search-webapp-view-in-browser.png)

La nueva página web de búsqueda personalizada debe parecerse a esta:

![Captura de pantalla de página web de búsqueda personalizada](./media/custom-search-webapp-browse-index.png)

Al realizar una búsqueda, los resultados se presentan de la manera siguiente.

![Captura de pantalla de resultados de búsqueda personalizada](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha hecho lo siguiente:

> [!div class="checklist"]
> - Ha creado una instancia de búsqueda personalizada
> - Ha agregado entradas activas
> - Ha agregado entradas bloqueadas
> - Ha agregado entradas ancladas
> - Ha integrado una búsqueda personalizada en una página web

Ahora puede pasar a llamar a los puntos de conexión de Bing Custom Search mediante programación.

> [!div class="nextstepaction"]
> [Llamada a un punto de conexión de Bing Custom Search (C#)](../call-endpoint-csharp.md)