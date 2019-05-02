---
title: Tutorial de simulación mediante "bake" de Project Acoustics con Unity
titlesuffix: Azure Cognitive Services
description: Este tutorial describe la simulación acústica mediante "bake" de Project Acoustics con Unity.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 2f0fcdcdf781c86179b67eeef0223d46da0fc65b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916995"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Tutorial de simulación mediante "bake" de Project Acoustics con Unity
Este tutorial describe la simulación acústica mediante "bake" de Project Acoustics con Unity.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows
* [Complemento de Project Acoustics integrado en el proyecto de Unity](unity-integration.md) o [contenido de ejemplo de Project Acoustics con Unity](unity-quickstart.md)
* Opcional: Una [cuenta de Azure Batch](create-azure-account.md) para acelerar la simulación mediante "bake" utilizando la informática en la nube

## <a name="open-the-project-acoustics-bake-window"></a>Apertura de la ventana de simulación mediante "bake" de Project Acoustics
Elija **Ventana > Acoustics** en el menú de Unity:

![Captura de pantalla del editor de Unity con la opción del menú de ventana Acoustics resaltada](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Creación de una malla de navegación
Project Acoustics utiliza una malla de navegación para colocar puntos de sondeo del oyente para la simulación. Puede usar [el flujo de trabajo de la malla de navegación](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) de Unity, u otro paquete de modelado 3D para diseñar su propia malla. 

## <a name="mark-acoustic-scene-objects"></a>Marcado de objetos de la escena acústica
Project Acoustics se basa en dos tipos de objetos de escena para la simulación: los objetos que reflejan y bloquean el sonido en la simulación, y la malla de navegación del jugador que restringe los puntos de sondeo del oyente en la simulación. Ambos tipos de objetos se marcan mediante la pestaña **Objects** (Objetos). 

Dado que marcar objetos simplemente agrega los componentes **AcousticsGeometry** o **AcousticsNavigation** al objeto, también puede usar el [flujo de trabajo estándar del componente Unity](https://docs.unity3d.com/Manual/UsingComponents.html) para marca o desmarca los objetos. Solo se pueden marcar representadores de malla y terrenos. Se omitirán todos los demás tipos de objeto. Las casillas marcarán o desmarcarán todos los objetos afectados.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Marcado de oclusión acústica y geometría de reflexión
Abra la pestaña **Objects** (Objetos) de la ventana **Acoustics** (Acústica). Marque los objetos como **Acoustics Geometry** (Geometría acústica) si deben bloquear, reflejar o absorber el sonido. La geometría acústica puede incluir elementos como suelo, paredes, techos, ventanas y cristales de ventana, alfombras y muebles grandes. Puede usar cualquier nivel de complejidad arbitrario para estos objetos. Dado que la escena se voxelizará antes de la simulación, las mallas muy detalladas, como árboles con muchas hojas pequeñas, no son más costosas para simular mediante "bake" que los objetos simplificados.

No incluya elementos que no afecten a la acústica, como las mallas de colisión invisibles.

La transformación de un objeto en el momento del cálculo del sondeo (a través de la pestaña **Probes** siguiente) se corrige en los resultados de la elaboración. Si se mueve alguno de los objetos marcados en la escena se deberá rehacer el cálculo de sondeo y volver a elaborar la escena.

### <a name="mark-the-navigation-mesh"></a>Marca de una malla de navegación
El sistema de acústica tomará las mallas de navegación creadas con el flujo de trabajo de Unity. Para usar sus propias mallas, márquelas en la pestaña **Objects** (Objetos).

### <a name="for-reference-the-objects-tab-parts"></a>Para referencia: Partes de la pestaña Objects (Objetos)
Las partes de la ficha son:

1. Los botones de selección de la pestaña [pestaña **Objects** (Objetos) seleccionada]. Use estos botones para desplazarse por los distintos pasos de la elaboración de la acústica, de izquierda a derecha.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Filtros disponibles para la ventana de la jerarquía. Úselos para filtrar la ventana de la jerarquía con los objetos del tipo especificado, de modo que pueda marcarlos más fácilmente. Si aún no ha marcado nada para la acústica, la selección de las dos últimas opciones no mostrará nada. Sin embargo, pueden ser útiles para buscar objetos marcados una vez que lo ha hecho.
4. Si no se ha seleccionado ningún objeto, en esta sección se muestra el estado de todos los objetos de la escena:
    * Total: el número total de objetos activos no ocultos en la escena.
    * Ignored (Omitidos): el número de objetos que no son representadores de malla ni terrenos.
    * Mesh (Malla): el número de objetos de representador de malla de la escena.
    * Terrain (Terreno): el número de objetos de terreno de la escena.
    * Geometry (Geometría): el número de objetos de malla o terreno de la escena marcados como "Acoustics Geometry" (Geometría de acústica).
    * Geometry (Geometría): el número de objetos Mesh o Terrain en la escena marcados como "Acoustics Geometry" (Geometría de acústica). En este número no se incluye NavMesh de Unity.
5. Se muestra el número total de objetos "que se pueden marcar" de la escena, que son solo representadores de malla y terrenos. Se muestran las casillas que se pueden usar para marcar (agregar el componente adecuado a) los objetos como geometría o navegación para la acústica.
6. Cuando no hay nada seleccionado, esta nota recuerda que seleccione los objetos que se deben marcar si es necesario. También puede activar una o ambas casillas para marcar todos los objetos de la escena sin seleccionar nada.
7. Cuando se han seleccionado objetos, en esta sección se muestra únicamente el estado de los objetos seleccionados.
8. Se muestra el número total de objetos seleccionados "que se pueden marcar". Al activar o desactivar las casillas se marcarán o desmarcarán solo los objetos seleccionados.

Si no hay nada seleccionado en la escena, la pestaña Object (Objetos) será similar a la siguiente imagen:

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics sin ninguna selección](media/objects-tab-no-selection-detail.png)

Si ha seleccionado algún elemento en la ventana o la jerarquía de la escena, será similar a la siguiente imagen:

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics con la selección mostrada](media/objects-tab-selection-detail.png)

Si hay algunos objetos marcados y otros no, la casilla correspondiente mostrará un valor "mixto":

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics con un icono de selección mixta resaltado](media/mixed-object-selection-detail.png)

Al hacer clic en la casilla obligará a marcar todos los objetos y al hacer clic de nuevo se desmarcarán todos los objetos.

Los objetos se pueden marcar para la geometría y la navegación.

## <a name="select-acoustic-materials"></a>Selección de materiales acústicos
Una vez marcados los objetos, haga clic en el botón **Materials** (Materiales) y realice la asignación de materiales acústicos. El sistema de materiales de Project Acoustics está vinculado al sistema de materiales visual de Unity: para que dos objetos tengan materiales acústicos independientes, tienen que tener materiales visuales independientes.

Los materiales acústicos controlan la cantidad de energía sonora que se refleja desde cada superficie. El material acústico predeterminado tiene una absorción similar a la del hormigón. Project Acoustics sugiere materiales en base al nombre de material visual. Puede asignar el material acústico "Custom" (Personalizado) a un material para habilitar un control deslizante de coeficiente de absorción.

El tiempo de reverberación de un material determinado en una habitación está inversamente relacionado con su coeficiente de absorción y la mayoría de los materiales tiene valores de absorción en un rango que va desde 0,01 a 0,20. Los materiales con coeficientes de absorción que quedan fuera de este rango son muy absorbentes.

![Gráfico que muestra una correlación negativa de tiempo de reverberación con el coeficiente de absorción](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Como referencia: Partes de la pestaña Materials (Materiales)
![Captura de pantalla de la pestaña Materials (Materiales) de Acoustics en Unity](media/materials-tab-detail.png)

1. El botón de la pestaña **Materials** (Materiales), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Cuando está activada, solo se mostrarán los materiales usados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). En caso contrario, se enumerarán todos los materiales usados en la escena.
4. Use estas opciones para cambiar el orden del menú desplegable que se muestra al hacer clic en un menú desplegable de la columna Acoustics (Acústica) (número 6). **Name** (Nombre) ordena los materiales acústicos por nombre. "Absorptivity" (Absorción) los ordena por capacidad de absorción, de baja a alta.
5. La lista de materiales usados en la escena, ordenados alfabéticamente. Si la casilla **Show Marked Only** (Mostrar solo los marcados) está activada (número 3), solo se muestran los materiales usados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). Al hacer clic en un material, se seleccionarán todos los objetos de la escena que usan ese material.
6. Se muestra el material acústico al que se ha asignado el material de la escena. Haga clic en un menú desplegable para volver a asignar un material de la escena a un material acústico diferente. Puede cambiar el criterio de ordenación del menú que se muestra al hacer clic en un elemento mediante las opciones **Sort Acoustics By:** anteriores (número 4).
7. Muestra el coeficiente de absorción acústica del material seleccionado en la columna anterior. Un valor de cero significa totalmente reflectante (ninguna absorción), mientras que un valor de 1 significa totalmente absorbente (sin reflexión). No se puede cambiar el coeficiente de absorción, a menos que el material seleccionado sea "Custom" (Personalizado).
8. Para un material asignado a "Custom" (Personalizado), el control deslizante ya no está deshabilitado y puede elegir el coeficiente de absorción con el control deslizante o escribiendo un valor.

## <a name="calculate-and-review-listener-probe-locations"></a>Calculo y revisión de las ubicaciones de sondeo del oyente
Después de asignar los materiales, cambie a la pestaña **Probes** (Sondeos) y haga clic en **Calculate** (Calcular) para colocar puntos de sondeo del oyente para la simulación.

### <a name="what-the-calculate-button-calculates"></a>Lo que calcula el botón "Calculate..." (Calcular...)
El botón **Calculate...**  (Calcular) usa la geometría de escena acústica que ha seleccionado para preparar la escena de simulación:

1. Toma la geometría de las mallas de la escena y calcula un volumen de vóxel. El volumen de vóxel es un volumen de 3 dimensiones que rodea toda la escena y se compone de pequeños "vóxeles" cúbicos. El tamaño de los vóxeles viene determinado por la frecuencia de la simulación, que se establece mediante la configuración **Simulation Resolution** (Resolución de configuración). Se marca cada vóxel como "cielo abierto" o que contiene geometría de la escena. Si un vóxel contiene geometría, se etiqueta con el coeficiente de absorción del material asignado a esa geometría.
2. Usa una o más mallas de navegación para colocar puntos de sondeo del oyente. El algoritmo equilibra los asuntos en competencia de la cobertura espacial y el tiempo y tamaño de archivo de la simulación, al tiempo que asegura que los pasillos estrechos y los pequeños espacios tengan siempre algo de cobertura. El recuento típico de puntos de sondeo va desde 100 para escenas pequeñas a varios miles para escenas grandes.

Según el tamaño de la escena y la velocidad del equipo, estos cálculos pueden tardar varios minutos.

### <a name="review-voxel-and-probe-placement"></a>Revisión de la ubicación de vóxeles y puntos de sondeo
Obtenga una vista previa de los datos de vóxel y de la ubicación de los puntos de sondeo para asegurarse de que está listo para realizar la simulación mediante "bake" de la escena. Una malla de navegación incompleta o una geometría acústica de más o de menos suelen ser rápidamente visibles en la vista previa. La ubicación de vóxeles y puntos de sondeo se puede habilitar o deshabilitar con el menú Gizmos:

![Captura de pantalla del menú Gizmos en Unity](media/gizmos-menu.png)

Los vóxeles que contienen la geometría acústica se muestran como cubos verdes. Explore la escena y compruebe que todo lo que debe ser geometría tiene vóxeles. La cámara de la escena debe estar dentro de aproximadamente 5 metros del objeto para que se muestren los vóxeles.

Si se comparan los vóxeles creados con resolución gruesa con los creados con resolución fina, verá que los vóxeles gruesos son el doble de grandes.

![Captura de pantalla de la vista previa de vóxeles gruesos en el editor de Unity](media/voxel-cubes-preview.png)

Los resultados de la simulación se interpolan entre ubicaciones de puntos de sondeo del oyente en tiempo de ejecución. Compruebe que haya puntos de sondeo cerca de cualquier lugar al que el jugador pueda ir dentro de la escena.

![Captura de pantalla de la vista previa de sondeo en el editor de Unity](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Tenga cuidado con los cambios de nombre de las escenas
El nombre de la escena se usa para conectar la escena a los archivos que almacenan la colocación de puntos de sondeo y la voxelización. Si se cambia el nombre de la escena una vez calculados los puntos de sondeo, se pierden los datos de asignación y la colocación del material y se debe volver a realizar la ejecución.

### <a name="for-reference-parts-of-the-probes-tab"></a>Para referencia: Partes de la pestaña Probes (Sondeos)
![Captura de pantalla de la pestaña Probes (Sondeos) de Acoustics en Unity](media/probes-tab-detail.png)

1. El botón de la pestaña **Probes** (Sondeos), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Use esto para elegir una resolución de simulación gruesa o fina. Gruesa es más rápido, pero tiene algunos inconvenientes. Para más información, consulte [Resolución de simulación mediante "bake"](bake-resolution.md).
4. Elija la ubicación donde se deben colocar los archivos de datos acústicos con este campo. Haga clic en el botón "..." para usar un selector de carpetas. El valor predeterminado es **Assets/AcousticsData**. También se creará una subcarpeta **Editor** en esta ubicación. Para más información acerca de los archivos de datos, consulte más abajo la sección de [Archivos de datos](#Data-Files).
5. El nombre de los archivos de datos para esta escena llevará el prefijo proporcionado aquí. El valor predeterminado es "Acoustics_ [nombre de la escena]".
6. Una vez se hayan calculado los sondeos, se deshabilitarán los controles anteriores. Haga clic en el botón **Clear** (Borrar) para borrar los cálculos y habilitar los controles de modo que pueda realizar otro cálculo con otros valores.
7. Haga clic en el botón **Calculate...**  (Calcular...) para voxelizar la escena y calcular las ubicaciones de puntos de sondeo. Esto se realiza localmente en el equipo y debe hacerse antes de una elaboración.

En esta versión de Project Acoustics, los sondeos no se pueden colocar manualmente y se deben colocar a través del proceso automatizado proporcionado en la pestaña **Probes** (Sondeos).

Para más información acerca de la diferencia entre baja y alta resolución, consulte [Resolución de simulación mediante "bake"](bake-resolution.md).

## <a name="bake-your-scene-using-azure-batch"></a>Simulación mediante "bake" de una escena con Azure Batch
Se puede simular mediante "bake" la escena con un clúster de proceso en la nube usando el servicio Azure Batch. El complemento de Project Acoustics para Unitiy se conecta directamente a Azure Batch para crear instancias, administrar y anular un clúster de Azure Batch para cada simulación mediante "bake". En la pestaña **Bake**, escriba sus credenciales de Azure, seleccione un tamaño y tipo de máquina del clúster y haga clic en **Bake**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Para referencia: Partes de la pestaña Bake
![Captura de pantalla de la pestaña Bake (Elaborar) de Acoustics en Unity](media/bake-tab-details.png)

1. El botón de la pestaña Bake (Elaborar) se usa para abrir esta página.
2. Una breve descripción de qué hacer en esta página.
3. Campos para especificar las credenciales de Azure una vez creada la cuenta de Azure. Para más información, vea [Create an Azure Batch Account](create-azure-account.md) (Crear una cuenta de Azure Batch).
4. Etiqueta de imagen de Docker para el conjunto de herramientas acústicas.
5. Abra Azure Portal para administrar las suscripciones, supervisar el uso, ver información de facturación, etc. 
6. Tipo de nodo de proceso de Azure Batch que se usará para el cálculo. El tipo de nodo debe ser compatible con la ubicación del centro de datos de Azure. Si no está seguro, deje **Standard_F8s_v2**.
7. Número de nodos que se usarán para este cálculo. El número que especifique aquí afectará al tiempo en que se completará la elaboración y está limitado por la asignación de núcleos de Azure Batch. La asignación predeterminada solo permite dos nodos de 8 núcleos o un nodo de 16 núcleos, pero se puede expandir. Para obtener más información sobre las restricciones de la asignación de núcleos, consulte [Create an Azure Batch Account](create-azure-account.md) (Crear una cuenta de Azure Batch).
8. Active esta casilla para configurar el grupo de proceso para que use [nodos de prioridad baja](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Los nodos de proceso de prioridad baja tienen un costo mucho menor, pero no siempre están disponibles o se pueden reemplazar en cualquier momento.
9. El recuento del sondeo de la escena tal como se ha calculado en la pestaña **Probes** (Sondeos). El número de sondeos determina el número de simulaciones que se deben ejecutar en la nube. No se pueden especificar más nodos que sondeos.
10. La cantidad de tiempo transcurrido que se espera que tarde el trabajo en ejecutarse en la nube. Esto no incluye el tiempo de inicio del nodo. Una vez que el trabajo comienza a ejecutarse, es el tiempo que tardará en obtener los resultados. Tenga en cuenta que esto es solo una estimación.
11. La cantidad total de tiempo de proceso necesario para ejecutar las simulaciones. Se trata de la cantidad total de tiempo de proceso del nodo que se usará en Azure. Consulte [Estimación de costos de la elaboración](#Estimating-bake-cost) más abajo para obtener más información sobre el uso de este valor.
12. Este mensaje indica dónde se guardarán los resultados de la elaboración una vez completado el trabajo.
13. (Solo para uso avanzado) Si por algún motivo tiene que forzar que Unity olvide una elaboración enviada (por ejemplo, ha descargado los resultados con otro equipo), haga clic en el botón **Clear State** (Borrar estado) para que se olvide del trabajo enviado. Tenga en cuenta que esto significa que el archivo de resultados, cuando esté listo, **no** se descargará y esto **no es igual a cancelar el trabajo**. El trabajo, si está en ejecución, se seguirá ejecutando en la nube.
14. Haga clic en el botón **Bake** para enviar la simulación mediante "bake" a la nube. Mientras se ejecuta un trabajo, se muestra **Cancel Job** (Cancelar trabajo) en su lugar.
15. Se prepara para el procesamiento de la [simulación acústica en el PC](#Local-bake).
16. En esta área se muestra el estado de la elaboración. Cuando se haya completado, debe mostrar **Downloaded** (Descargado).

Siempre puede obtener la información completa sobre los trabajos activos, los grupos de proceso y el almacenamiento en [Azure Portal](https://portal.azure.com).

Mientras un trabajo está en ejecución, el botón **Bake** (Elaborar) cambia a **Cancel Job** (Cancelar trabajo). Use este botón para cancelar el trabajo en curso. Se le pedirá confirmación antes de cancelar el trabajo. La cancelación de un trabajo no se puede deshacer, no estará disponible ningún resultado y se le cobrará por el tiempo de proceso de Azure usado.

Una vez iniciada una elaboración, puede cerrar Unity. Según el proyecto, el tipo de nodo y el número de nodos, una elaboración en la nube puede tardar varias horas. Se actualizará el estado del trabajo de elaboración al recargar el proyecto y abrir la ventana Acoustics (Acústica). Si se ha completado el trabajo, se descargará el archivo de salida.

Las credenciales de Azure se almacenan de forma segura en el equipo local y están asociadas al editor Unity. Se usan únicamente para establecer una conexión segura con Azure.

### <a name="Estimating-bake-cost"></a> Estimación del costo de elaboración de Azure

Para estimar lo que costará una elaboración determinada, tome el valor que se muestra para **Estimated Compute Cost** (Costo de proceso estimado), que es una duración, y multiplíquelo por el costo por hora en su moneda local del **VM Node Type** (Tipo de nodo VM) que ha seleccionado. El resultado no incluirá el tiempo de nodo necesario para que los nodos entren en ejecución. Por ejemplo, si selecciona **Standard_F8s_v2** para el tipo de nodo, que tiene un costo de 0,40 USD/hora y el costo estimado de proceso es de 3 horas y 57 minutos, el costo estimado para la ejecución del trabajo será 0,40 USD * ~ 4 horas = ~ 1,60 USD. El costo real probablemente será un poco mayor debido al tiempo adicional para iniciar los nodos. Puede encontrar el costo de nodo por hora en la página [Azure Batch Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (Precios de Azure Batch) (seleccione "Proceso optimizado" o "Proceso de alto rendimiento" para la categoría).

## <a name="Local-bake"></a>Simulación mediante "bake" de una escena en el PC
Puede simular mediante "bake" una escena en su propio PC. Esto puede ser útil para experimentar con la acústica en escenas pequeñas antes de crear una cuenta de Azure Batch. Tenga en cuenta que la simulación acústica puede tardar mucho tiempo según el tamaño de la escena.

### <a name="minimum-hardware-requirements"></a>Requisitos mínimos de hardware
* Procesador x86-64 con al menos 8 núcleos y 32 GB de RAM

Por ejemplo, en las pruebas realizadas en una máquina de 8 núcleos con Intel Xeon E5-1660 @ 3 GHz y 32 GB de RAM
* Una escena pequeña con 100 sondeos tarda unas 2 horas para una simulación mediante "bake" gruesa, y unas 32 horas para una de resolución fina.
* Un escena de tamaño mediano con 1000 sondeos puede llevar unas 20 horas para una simulación mediante "bake" gruesa, y unos 21 días para una de resolución fina.

### <a name="setup-docker"></a>Instalación de Docker
Instale y configure Docker en el equipo en el que se va a procesar la simulación:
1. Instale el [conjunto de herramientas de Docker](https://www.docker.com/products/docker-desktop).
2. Inicie la configuración de Docker, vaya a las opciones "Avanzadas" y configure los recursos para tener al menos 8 GB de RAM. Cuantas más CPU pueda asignar a Docker, más rápidamente se completará la elaboración. ![Captura de pantalla de configuración de Docker de ejemplo](media/docker-settings.png)
3. Vaya a "Unidades compartidas" y active el uso compartido de la unidad que se usa para el procesamiento.![Captura de pantalla de las opciones de las unidades compartidas de Docker](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Ejecución de la elaboración local
1. Haga clic en el botón "Prepare Local Bake" (Preparar la simulación mediante "bake" local) en la pestaña **Bake** y seleccione una carpeta para guardar los archivos de entrada y los scripts de ejecución. Después, puede ejecutar la elaboración en cualquier equipo, siempre que cumpla los requisitos mínimos de hardware y tenga Docker instalado si copia la carpeta en ese equipo.
2. Inicie la simulación mediante el script "runlocalbake.bat". Este script capturará la imagen de Docker de Elementos acústicos de un proyecto con el conjunto de herramientas necesarias para el procesamiento de la simulación e iniciará la simulación. 
3. Cuando haya finalizado la simulación, copie el archivo .ace resultante al proyecto Unity. Para asegurarse de que Unity lo reconoce como un archivo binario, anexe ".bytes" a la extensión de archivo (por ejemplo, "Scene1.ace.bytes"). Los registros detallados para la simulación se almacenan en "AcousticsLog.txt". Si experimenta algún problema, comparta este archivo para ayudar con el diagnóstico.

## <a name="Data-Files"></a> Archivos de datos agregados por el proceso de simulación mediante "bake"

Durante el proceso de simulación mediante "bake" se crean cuatro archivos de datos. Uno contiene los resultados de la simulación y se incluye con el título. Los demás almacenan datos relacionados con el editor de Unity.

Resultado de la simulación:
* **Assets/AcousticsData/Acoustics\_[nombreEscena].ace.bytes**: Esta es la tabla de búsqueda en tiempo de ejecución y contiene los resultados de la simulación y los elementos de la escena acústica voxelizados. La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).

Tenga cuidado de no eliminar el archivo de resultados de la simulación. Este archivo no es recuperable, excepto si se vuelve a simular la escena mediante "bake".

Archivos de datos del editor:
* **Assets/Editor/[nombreEscena]\_AcousticsParameters.asset**: este archivo almacena los datos que se especifiquen en los campos de la interfaz de usuario de Acoustics. No se puede cambiar la ubicación y el nombre de este archivo.
* **Assets/AcousticsData/Editor/Acoustics_[nombreEscena].vox**: Este archivo almacena la geometría de acústica voxelizada y las propiedades de material que se calculan utilizando el botón **Calculate...**  (Calcular) en la pestaña Probes (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).
* **Assets/AcousticsData/Editor/Acoustics\_[nombreEscena]\_config.xml**: Este archivo almacena los parámetros de simulación calculados mediante el botón **Calculate...** (Calcular...) de la pestaña **Probes** (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).

## <a name="set-up-the-acoustics-lookup-table"></a>Configuración de la tabla de búsqueda acústica
Arrastre y coloque el objeto prefabricado **Project Acoustics** desde el panel del proyecto en la escena:

![Captura de pantalla de objeto prefabricado de Acoustics en Unity](media/acoustics-prefab.png)

Haga clic en el objeto de juego **ProjectAcoustics** y vaya al panel del inspector. Especifique la ubicación del resultado de la elaboración (el archivo .ACE, en **Assets/AcousticsData**) mediante la función de arrastrarlo y colocarlo en el script del administrador de acústica o con un clic en el botón de círculo situado junto al cuadro de texto.

![Captura de pantalla de objeto prefabricado del administrador de acústica en Unity](media/acoustics-manager.png)  

## <a name="next-steps"></a>Pasos siguientes
* Explore los [controles de diseño para Unity](unity-workflow.md)
* Explore los [conceptos de diseño de Project Acoustics](design-process.md)

