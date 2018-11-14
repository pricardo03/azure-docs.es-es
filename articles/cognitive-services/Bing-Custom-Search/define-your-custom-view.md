---
title: 'Definición de una vista personalizada: Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Describe cómo crear servicios de búsqueda de sitio y verticales.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 5e168608202f81f698221b52315de83e9713feab
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282931"
---
# <a name="configure-your-custom-search-experience"></a>Configuración de la experiencia de búsqueda personalizada

Una instancia de Custom Search le permite adaptar la experiencia de búsqueda para incluir contenido solo de sitios web que interesan a los usuarios. En lugar de realizar una búsqueda en toda la Web, Bing solo busca el sector de la Web que le interesa. Para crear la vista personalizada de la Web, use el [portal](https://customsearch.ai) de Bing Custom Search. Para información sobre cómo iniciar sesión en el portal, consulte [Create your first Bing Custom Search instance](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start) (Creación de la primera instancia de Bing Custom Search). 

El portal le permite crear una instancia de búsqueda que especifica los dominios, las subpáginas y las páginas web en los que quiere que busque Bing y aquellos en los que no quiere que busque. Además de especificar las direcciones URL del contenido que conoce, también puede solicitar al portal que sugiera contenido que quizás quiera agregar a la vista. 

Estas son las maneras en que puede definir un sector de la Web: 

1.  Dominio. Un sector de dominio incluye todo el contenido que se encuentra en un dominio de Internet. Por ejemplo, www.microsoft.com. Si se omite "www", Bing también busca en los subdominios del dominio. Por ejemplo, si especifica microsoft.com, Bing también devuelve resultados de support.microsoft.com o technet.microsoft.com.  
  
2.  Subpágina. Un sector de subpágina incluye todo el contenido encontrado en la subpágina y las rutas de acceso que hay debajo de ella. Puede especificar como máximo dos subpáginas en la ruta de acceso. Por ejemplo, www.microsoft.com/en-us/windows/  
  
3.  Página web. Un sector de página web puede incluir solo esa página web en una búsqueda personalizada. Opcionalmente, puede especificar si se deben incluir subpáginas.

Todos los dominios, subpáginas y páginas web que especifique deben ser públicos y estar indexados por Bing. Si dispone de un sitio público que quiere incluir en la búsqueda, y Bing no lo ha indexado, consulte la [documentación del administrador web](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) para más información sobre cómo obtener Bing para indexarlo. Además, consulte la documentación del administrador web para más información sobre cómo obtener Bing para actualizar su sitio rastreado si el índice no está actualizado.

## <a name="adding-slices-to-your-custom-search"></a>Adición de sectores a la búsqueda personalizada

Al definir la instancia de búsqueda personalizada, especifica los dominios, las páginas web y las subpáginas, activos y bloqueados, en los que quiere buscar o no buscar.  

- Activo: una lista de dominios, subpáginas o páginas web que se incluirán en la búsqueda.  
  
- Bloqueado: una lista de dominios, subpáginas o páginas web que se excluirán de la búsqueda. Los elementos que bloquea deben ser contenido encontrado en los dominios y las subpáginas enumerados en la lista de activos.

Para acceder a cada lista, haga clic en las pestañas Active (Activo) y Blocked (Bloqueado) en la instancia de búsqueda personalizada. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Listas de activos y bloqueados 

Para especificar un sector de la Web en el que quiere que busque Bing, haga clic en la pestaña **Active** (Activo) y muestre los dominios, las subpáginas y las páginas web donde buscar. Puede agregar un sector directamente a la lista o agregar más de un sector mediante la carga de un archivo de texto con el icono de carga.

Detalles de carga de archivos: 

- La carga de archivos solo está disponible para agregar sectores a la lista de activos; no se puede usar para agregar sectores a la lista de bloqueados.  
  
- Cree un archivo de texto y especifique un único dominio, subpágina o página web por línea. La carga completa se rechaza si se produce un error.  
  
- Si la lista de bloqueados contiene el dominio, la subpágina o la página web que especificó en el archivo de carga, el servicio los quita de la lista de bloqueados y los agrega a la lista de activos.  
  
- El servicio omite los elementos duplicados del archivo de carga.

Para editar o eliminar sectores, use las opciones de la columna **Controls** (Controles). 

De forma similar, puede agregar sectores a la lista de bloqueados (excepto que no puede usar un archivo de carga para especificar los sectores).

## <a name="pinned-list"></a>Lista de anclados

El portal también le permite anclar una página web a la parte superior del resultado de búsqueda si el usuario escribe un término de búsqueda específico. La pestaña **Pinned** (Anclado) contiene una lista de pares de término de consulta y página web que especifican la página web que aparece como mejor resultado para una consulta específica. Además, se mostrarán solo las páginas web indexadas en búsquedas. Para información acerca de cómo anclar los resultados, consulte [Ajuste de la clasificación](#adjustrank). 

El anclaje de los resultados no está disponible para las experiencias Image Search ni Video Search.

## <a name="website-suggestions"></a>Sugerencias de sitio web

Después de agregar sectores a la lista de activos, el servicio genera sugerencias de sitios web y subpáginas que podría querer agregar a la búsqueda. La sección **You might want to add** (Puede que quiera agregar) contiene las sugerencias. La página de configuración de instancias incluye esta sección solo si hay sugerencias disponibles. 

Para agregar sugerencias a la lista de activos, haga clic en el icono +.  Como el servicio genera sugerencias basadas en la configuración, asegúrese de hacer clic en **Refresh**  (Actualizar) después de agregar las sugerencias. 

## <a name="preview-pane"></a>Panel de vista previa

Puede probar la instancia de búsqueda mediante el panel de vista previa de la derecha para enviar consultas de búsqueda y ver los resultados. 

1. Seleccione **My Instance** (Mi instancia)
2. Seleccione un filtro de búsqueda seguro y el mercado en el que buscar (consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) [Parámetros de consulta]).
3. Escriba una consulta y presione Entrar o haga clic en el icono de búsqueda para ver los resultados de la configuración actual. 

Para seleccionar el tipo de búsqueda que realizar, haga clic en **Web** para obtener los resultados web, **Imagen** para obtener los resultados de imagen o **Vídeo**, para los de vídeo. 

Mediante el panel de vista previa, puede también revisar los resultados de Bing si selecciona **Bing** en lugar de **My instance** (Mi instancia). El panel de vista previa puede ser útil para comparar los resultados de su experiencia de búsqueda con los resultados devueltos por Bing.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Ajuste de la clasificación

El portal le permite ajustar la clasificación para manipular los resultados que devuelve Bing. En el panel de vista previa, escriba un término de búsqueda y ejecute la consulta. En el panel de vista previa se muestra el resultado de búsqueda de la consulta. A la derecha de cada resultado está la lista de ajustes que puede realizar. 

- Bloquear. Mueve el dominio, la subpágina o la página web a la lista de bloqueados. Puede seleccionar el nivel de bloqueo. Bing excluye contenido del sitio seleccionado en los resultados de búsqueda.  
  
- Aumentar de nivel. Coloca el contenido del dominio o la subpágina en un nivel más alto en los resultados de búsqueda. Seleccione si desea aumentar de nivel el contenido del dominio o la subpágina a los que pertenece la página web. [Más información](#boosting-and-demoting)  
  
- Reducir de nivel. Coloca el contenido del dominio o la subpágina en un nivel inferior en los resultados de búsqueda. Seleccione si desea reducir de nivel el contenido del dominio o la subpágina a los que pertenece la página web. [Más información](#boosting-and-demoting).  
  
- Anclar al principio. Define la página web que aparece en la parte superior de los resultados si coincide la cadena de consulta del usuario con las ancladas en la condición de coincidencia de anclaje. No es necesario que la lista de activos contenga la página web para que la pueda anclar. [Más información](#pin-to-top).

El ajuste de la clasificación no está disponible para las experiencias Image Search ni Video Search.

## <a name="boosting-and-demoting"></a>Aumento y disminución de nivel

Puede aplicar aumento, aumento avanzado o disminución de nivel a cualquier dominio o subpágina de la lista de activos. De forma predeterminada, todos los sectores se agregan con el mismo peso. Los elementos con aumento o aumento avanzado se clasifican mejor en los resultados de búsqueda (teniendo el aumento avanzado una clasificación más alta que el aumento solo). Los elementos que disminuyen de nivel se clasifican por detrás en los resultados de búsqueda.

Es importante advertir que el aumento avanzado, el aumento y la disminución de nivel proporcionan variantes de peso respectivas para los dominios o las subpáginas. Esta es solo una de las muchas señales que usa el clasificador para determinar el orden de los resultados. Esto significa que su efecto en una consulta específica no se garantiza, dado que otros muchos factores pueden influir en la clasificación general de los resultados web.  Para determinar el posible efecto que puede tener sobre el clasificador aumentar o disminuir de nivel, pruebe la experiencia de búsqueda mediante el panel de vista previa.

Para aplicar aumento, aumento avanzado o disminución de nivel a los elementos, puede usar los controles de ajuste de clasificación de la lista de activos o los controles de aumento y reducción de nivel del panel de vista previa. El servicio agrega el sector a la lista de activos y ajusta la clasificación según corresponda.

Los ajustes de clasificación de aumento avanzado, aumento y disminución no están disponibles para la experiencia Image Search ni Video Search.

## <a name="pin-to-top"></a>Anclar al principio

Para anclar una página web al principio de los resultados de búsqueda de una consulta específica, elija una de las siguientes opciones:

1.  En la pestaña  **Anclado** , escriba la dirección URL de la página web para anclar al principio de los resultados y la consulta que desencadena el anclaje.  
  
2.  En el panel **Vista previa**, escriba un término de consulta y haga clic en el botón de búsqueda. A continuación, identifique la página web en los resultados que quiere anclar al principio si el usuario escribe la misma consulta. Seguidamente, haga clic en  **Anclar al principio**. El servicio agrega la página web y la consulta a la lista **Anclado**. 

Puede realizar un seguimiento de los elementos anclados en la pestaña  **Anclado** . Los elementos anclados se muestran como pares de "\<consulta\>, \<página web\>". 

Para una consulta concreta, puede anclar una página web como máximo al principio de los resultados.

Los elementos anclados no están disponibles para las experiencias Image Search ni Video Search.

### <a name="specifying-the-pins-match-condition"></a>Especificación de la condición de coincidencia de anclaje

La página web se ancla al principio de los resultados solo si la cadena de consulta del usuario coincide con la de anclaje en función de la condición de coincidencia de anclaje. De forma predeterminada, el anclaje coincide solo si la cadena de consulta de anclaje y la del usuario son exactamente iguales. Puede cambiar el comportamiento predeterminado al especificar una de las siguientes condiciones de coincidencia.

- Empieza por &mdash; El anclaje coincide si la cadena de consulta del usuario empieza por lo mismo que la de anclaje.
- Termina por &mdash; El anclaje coincide si la cadena de consulta del usuario termina por lo mismo que la de anclaje.
- Contiene &mdash; El anclaje coincide si la cadena de consulta del usuario contiene la de anclaje.

Para cambiar la condición de coincidencia de anclaje, haga clic en el icono de edición del anclaje (parece un lápiz). En la columna **Query match condition** (Condición de coincidencia de la consulta), haga clic en la lista desplegable y seleccione la nueva condición para usarla. Después, haga clic en el icono para guardar el cambio.

En las comparaciones se distingue entre mayúsculas y minúsculas.

### <a name="changing-the-order-of-the-pins"></a>Cambio del orden de los elementos anclados

Para cambiar el orden de los elementos anclados, puede arrastrarlos y colocarlos o modificarlos y cambiar el número por el que se ordenan. Para arrastrar y colocar un elemento anclado, haga clic en él en la lista para moverlo, mantenga el botón del mouse presionado y arrástrelo al lugar del elemento que desee remplazar; después, suelte el botón del mouse. Tenga en cuenta que número de orden del elemento anclado cambiará en consecuencia.

También puede editar el número de orden del elemento. En la columna **Controls** (Controles), haga clic en el icono de edición del elemento anclado (parece un lápiz). Escriba el número de orden donde desea que aparezca el elemento anclado en la lista y presione Entrar o haga clic en el icono de guardar. Por ejemplo, si es actualmente el sexto en la lista y quiere que sea el segundo, cambie el número a dos (2).

Si varios elementos anclados cumplen la condición de coincidencia, su orden determinará el que use Bing. Por ejemplo, si los elementos dos y tres cumplen la condición de coincidencia, Bing usará el número dos.

## <a name="use-bing-to-specify-slices"></a>Uso de Bing para especificar sectores

Existen un par de maneras de especificar los sectores de la Web que constituyen su búsqueda personalizada. Si conoce los sectores que quiere incluir en la instancia, agréguelos a la lista **Active** (Activos) de esta. Para información sobre cómo agregar elementos a la lista **Active** (Activos) por su cuenta, consulte [Listas de activos y bloqueados](#active-and-blocked-lists).

Pero si no está seguro de qué sectores incluir, puede ejecutar consultas de Bing en el panel **Vista previa** y ver qué devuelve Bing. A continuación, puede seleccionar los sectores que quiere incluir en la búsqueda personalizada. Es probable que deba ejecutar varios términos de consulta para asegurarse de que se identifican todos los sectores que quiere para la instancia. 

Siga estos pasos para usar Bing para agregar sectores a la instancia de búsqueda personalizada. 

1.  Inicie sesión en el [portal](https://customsearch.ai) de Bing Custom Search.
2.  Cree una instancia o seleccione una instancia para actualizar.
3.  En el panel de vista previa de la parte derecha, seleccione Bing en la lista desplegable.
4.  En el cuadro de búsqueda, escriba un término de consulta que sea pertinente para la instancia.
5.  Haga clic en **Add site** (Agregar sitio) junto al resultado que quiere incluir.
6.  Haga clic en el botón **Ok** (Aceptar).

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Visualización de estadísticas

Si se ha suscrito a Custom Search en el nivel adecuado (consulte los [planes de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), se agrega una pestaña **Statistics** (Estadísticas) a las instancias de producción. En la pestaña de estadísticas se muestra información detallada sobre cómo se usan los puntos de conexión de Custom Search, como el volumen de llamadas, las mejores consultas, la distribución geográfica, los códigos de respuesta y la búsqueda segura. Puede filtrar esta información mediante los controles proporcionados.

## <a name="understanding-quota"></a>Cuota

- En cada instancia de búsqueda personalizada, el número máximo de ajustes de clasificación que puede realizar en los sectores **Active** (Activo) y **Blocked** (Bloqueado) se limita a 400.
- Agregar un sector a las pestañas Active (Activo) o Blocked (Bloqueado) cuenta como un ajuste de clasificación.
- Aumentar y disminuir de nivel cuenta como dos ajustes de clasificación.
- En cada instancia de búsqueda personalizada, el número máximo de elementos anclados que se pueden crear está limitado a 200.

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom search](./search-your-custom-view.md) (Llamada a la búsqueda personalizada)
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)