---
title: Elaboración de la acústica con Elementos acústicos de un proyecto
titlesuffix: Azure Cognitive Services
description: En este documento se describe el proceso de realizar una elaboración de la acústica mediante la extensión del editor Unity.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: f376b6bcb0238f96b4f5bb35d898cc600e108c65
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169763"
---
# <a name="bake-acoustics"></a>Elaboración de la acústica

En este documento se describe el proceso de realizar una elaboración de la acústica mediante la extensión del editor Unity. Para obtener más información sobre la acústica, consulte [What is Acoustics](what-is-acoustics.md) (¿Qué es la acústica?). Para obtener una guía de inicio rápido, consulte [Getting Started](getting-started.md) (Introducción).

## <a name="import-the-plugin"></a>Importar el complemento

Importe el paquete de complemento Acoustics al proyecto. A continuación, abra la interfaz de usuario de Acoustics eligiendo **Window > Acoustics**  (Ventana > Acústica) en el menú de Unity:

![Abrir la ventana Acoustics (Acústica)](media/WindowAcoustics.png)

## <a name="principles"></a>Principios

La ventana de la herramienta Acoustics (Acústica) recopila la información que el motor de acústica necesita para calcular la acústica de la escena. Los pasos necesarios para realizar la elaboración son cinco:

1. Crear o marcar la malla de navegación del reproductor
2. Marcar la geometría de la acústica
3. Asignar propiedades de materiales acústicos a la geometría
4. Vista previa de la ubicación de los sondeos
5. Elaboración

Una vez completada la elaboración, consulte [Design Process Overview for Acoustics](design-process.md) (Introducción al proceso de diseño para la acústica) para ver los pasos de diseño opcionales posteriores a la elaboración.

## <a name="create-or-mark-a-navigation-mesh"></a>Crear o marcar una malla de navegación

Una malla de navegación se usa para colocar puntos de sondeo para la simulación. Puede usar el [flujo de trabajo de la malla de navegación](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) de Unity o bien puede especificar su propia malla de navegación. El sistema de acústica tomará las mallas de navegación creadas con el flujo de trabajo de Unity. Para usar sus propias mallas, márquelas en la pestaña **Objects** (Objetos), tal como se describe en el paso siguiente.

## <a name="objects-tab"></a>Pestaña Objects (Objetos)

Abra la pestaña **Objects** (Objetos) de la ventana **Acoustics** (Acústica). Use esta pestaña para marcar objetos de la escena, que simplemente agrega los componentes **AcousticsGeometry** o **AcousticsNavigation** al objeto. También puede usar el [flujo de trabajo de los componentes estándar de Unity](https://docs.unity3d.com/Manual/UsingComponents.html) para marcar o desmarcar los objetos.

Seleccione uno o más objetos en la ventana o la jerarquía de la escena y, a continuación, márquelos o desmárquelos para **AcousticsGeometry** o **AcousticsNavigation**, tal como se describe a continuación. Si no hay nada seleccionado, puede marcar o desmarcar todos los elementos de la escena a la vez.

Solo se pueden marcar representadores de malla y terrenos. Se omitirán todos los demás tipos de objeto. Las casillas marcarán o desmarcarán todos los objetos afectados.

Si no hay nada seleccionado en la escena, será similar a la siguiente imagen:

![Pestaña Objects (Objetos) sin ninguna selección](media/ObjectsTabNoSelectionDetail.png)

Si ha seleccionado algún elemento en la ventana o la jerarquía de la escena, será similar a la siguiente imagen:

![Pestaña Objects (Objetos) sin ninguna selección](media/ObjectsTabWithSelectionDetail.png)

### <a name="objects-tab-parts"></a>Partes de la pestaña Objects (Objetos)

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

Si hay algunos objetos marcados y otros no, la casilla correspondiente mostrará un valor "mixto":

![Casilla de valor mixto](media/MixedObjectSelectionDetail.png)

Al hacer clic en la casilla obligará a marcar todos los objetos y al hacer clic de nuevo se desmarcarán todos los objetos.

Los objetos se pueden marcar para la geometría y la navegación.

### <a name="guidelines-for-marking-objects"></a>Directrices para marcar objetos

Asegúrese de marcar los objetos como **Acoustics Geometry** (Geometría acústica) si deben bloquear, reflejar o absorber el sonido. La geometría acústica puede incluir elementos como suelo, paredes, techos, ventanas y cristales de ventana, alfombras y muebles grandes. No pasa nada si se incluyen objetos de menor tamaño, como lámparas, elementos decorativos, apliques de luz, ya que no aumentan de manera apreciable el costo de la elaboración. Es importante que no falten los elementos principales, como el suelo o el techo. Además, no incluya elementos que no afecten a la acústica, como las mallas de colisión.

La transformación de un objeto en el momento del cálculo del sondeo (a través de la pestaña **Probes** siguiente) se corrige en los resultados de la elaboración. Si se mueve alguno de los objetos marcados en la escena se deberá rehacer el cálculo de sondeo y volver a elaborar la escena.

## <a name="materials-tab"></a>Pestaña Materials (Materiales)

Una vez marcados los objetos, haga clic en el botón **Materials** (Materiales) para ir a la pestaña Materials (Materiales).

### <a name="parts-of-the-materials-tab"></a>Partes de la pestaña Materials (Materiales)

![Detalle de la pestaña Materials (Materiales)](media/MaterialsTabDetail.png)

1. El botón de la pestaña **Materials** (Materiales), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Cuando está activada, solo se mostrarán los materiales usados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). En caso contrario, se enumerarán todos los materiales usados en la escena.
4. Use estas opciones para cambiar el orden del menú desplegable que se muestra al hacer clic en un menú desplegable de la columna Acoustics (Acústica) (número 6). **Name** (Nombre) ordena los materiales acústicos por nombre. "Absorptivity" (Absorción) los ordena por capacidad de absorción, de baja a alta.
5. La lista de materiales usados en la escena, ordenados alfabéticamente. Si la casilla **Show Marked Only** (Mostrar solo los marcados) está activada (número 3), solo se muestran los materiales usados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). Al hacer clic en un material, se seleccionarán todos los objetos de la escena que usan ese material.
6. Se muestra el material acústico al que se ha asignado el material de la escena. Haga clic en un menú desplegable para volver a asignar un material de la escena a un material acústico diferente. Puede cambiar el criterio de ordenación del menú que se muestra al hacer clic en un elemento mediante las opciones **Sort Acoustics By:** anteriores (número 4).
7. Muestra el coeficiente de absorción acústica del material seleccionado en la columna anterior. Un valor de cero significa totalmente reflectante (ninguna absorción), mientras que un valor de 1 significa totalmente absorbente (sin reflexión). No se puede cambiar el coeficiente de absorción, a menos que el material seleccionado sea "Custom" (Personalizado).
8. Para un material asignado a "Custom" (Personalizado), el control deslizante ya no está deshabilitado y puede elegir el coeficiente de absorción con el control deslizante o escribiendo un valor. Para obtener más información sobre las propiedades de los materiales, vea [Design Process Overview for Acoustics](design-process.md) (Introducción al proceso de diseño para la acústica).

### <a name="guidelines-for-assigning-materials-or-absorption-values"></a>Directrices para asignar materiales (o valores de absorción)

En esta pestaña se intenta adivinar la absorción de los materiales en función del nombre. Por ejemplo, si el nombre del material de la escena es Sala_MesaMadera, el material acústico inicial asignado será "madera". Los materiales para los que no se puede determinar un material conocido se asignan al material "Default" (Predeterminado), que tiene una absorción similar al hormigón.

Puede reasignar materiales acústicos a cada material de la escena. Por ejemplo, si en una sala se produce demasiada reverberación, cambie el material acústico de las paredes, suelo o techo por otro con mayor absorción. La asignación de material acústico se aplica a todos los objetos que usan ese material de la escena.

## <a name="probes-tab"></a>Pestaña Probes (Sondeos)

Después de asignar los materiales, cambie a la pestaña **Probes** (Sondeos).

### <a name="parts-of-the-probes-tab"></a>Partes de la pestaña Probes (Sondeos)

![Detalle de la pestaña Probes (Sondeos)](media/ProbesTabDetail.png)

1. El botón de la pestaña **Probes** (Sondeos), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Use esto para elegir una resolución de simulación gruesa o fina. Gruesa es más rápido, pero tiene algunos inconvenientes. Consulte ["Resolución gruesa frente a fina"](#Coarse-vs-Fine-Resolution) a continuación para ver los detalles.
4. Elija la ubicación donde se deben colocar los archivos de datos acústicos con este campo. Haga clic en el botón "..." para usar un selector de carpetas. El valor predeterminado es **Assets/AcousticsData**. También se creará una subcarpeta **Editor** en esta ubicación. Para obtener más información sobre los archivos de datos, vea ["Archivos de datos"](#Data-Files) más abajo.
5. El nombre de los archivos de datos para esta escena llevará el prefijo proporcionado aquí. El valor predeterminado es "Acoustics_ [nombre de la escena]".
6. Una vez se hayan calculado los sondeos, se deshabilitarán los controles anteriores. Haga clic en el botón **Clear** (Borrar) para borrar los cálculos y habilitar los controles de modo que pueda realizar otro cálculo con otros valores.
7. Haga clic en el botón **Calculate...**  (Calcular...) para voxelizar la escena y calcular las ubicaciones de puntos de sondeo. Esto se realiza localmente en el equipo y debe hacerse antes de una elaboración.

En esta versión de Project Acoustics, los sondeos no se pueden colocar manualmente y se deben colocar a través del proceso automatizado proporcionado en la pestaña **Probes** (Sondeos).

### <a name="what-the-calculate-button-calculates"></a>Lo que calcula el botón "Calculate..." (Calcular...)

El botón **Calculate...**  (Calcular...) toma todos los datos que proporcionados hasta el momento (geometría, navegación, materiales y configuración gruesa o fina) y realiza varios pasos:

1. Toma la geometría de las mallas de la escena y calcula un volumen de vóxel. El volumen de vóxel es un volumen de 3 dimensiones que rodea toda la escena y se compone de pequeños "vóxeles" cúbicos. El tamaño de los vóxeles viene determinado por la frecuencia de la simulación, que se establece mediante la configuración **Simulation Resolution** (Resolución de configuración). Se marca cada vóxel como "cielo abierto" o que contiene geometría de la escena. Si un vóxel contiene geometría, se etiqueta con el coeficiente de absorción del material asignado a esa geometría.
2. A continuación, usa los datos de navegación para calcular las ubicaciones acústicamente interesantes donde es posible que el jugador vaya. Intenta encontrar un conjunto relativamente pequeño de estas ubicaciones que incluya áreas más pequeñas, como entradas y vestíbulos y luego salas y espacios abiertos. Para las escenas pequeñas suele ser de menos de 100 ubicaciones, mientras que las escenas de gran tamaño pueden tener hasta mil.
3. Para cada una de las ubicaciones de escucha final que calcula, determina varios parámetros, como cuán "abierto" es el espacio, el tamaño de la sala en que se encuentra, etcétera.
4. Los resultados de estos cálculos se almacenan en archivos en la ubicación que especifique (vea ["Archivos de datos"](#Data-Files) más abajo).

Según el tamaño de la escena y la velocidad del equipo, estos cálculos pueden tardar varios minutos.

Una vez completados estos cálculos, puede obtener una vista previa de los datos de vóxel y las ubicaciones de puntos de sondeo para garantizar que la elaboración ofrecerá buenos resultados. Normalmente, casos como una malla de navegación incorrecta o que falte o sobre una geometría se verán rápidamente en la vista previa, por lo que podrá corregirlos.

### <a name="scene-rename"></a>Cambio de nombre de la escena

El nombre de la escena se usa para conectar la escena a los archivos que almacenan la colocación de puntos de sondeo y la voxelización. Si se cambia el nombre de la escena una vez calculados los puntos de sondeo, se pierden los datos de asignación y la colocación del material y se debe volver a realizar la ejecución.

## <a name="debug-display-through-gizmos"></a>Depurar la pantalla a través de Gizmos

De forma predeterminada, los gizmos **Probes** (Sondeos) y **Voxels** (Vóxeles) están activados. Le mostrarán los vóxeles y las ubicaciones de puntos de sondeo que se calcularon. Se pueden habilitar o deshabilitar con el menú Gizmos:

![Menú Gizmos](media/GizmosMenu.png)

### <a name="voxels"></a>Vóxeles

Los vóxeles se muestran en la ventana de la escena como cubos verdes alrededor de la geometría participante. No se muestran los vóxeles que contienen solo aire. Hay un gran recuadro verde alrededor de toda la escena que indica el volumen de vóxel completo que se usará en la simulación.
Muévase por la escena y compruebe que todo lo que debe ser geometría tiene vóxeles. La cámara de la escena debe estar dentro de aproximadamente 5 metros del objeto para que se muestren los vóxeles.

Si se comparan los vóxeles creados con resolución gruesa con los creados con resolución fina, verá que los vóxeles gruesos son el doble de grandes.

![Vista previa de vóxel](media/VoxelCubesPreview.png)

### <a name="probe-points"></a>Puntos de sondeo

Los puntos de sondeo son sinónimos de ubicaciones posibles del jugador (escucha). Cuando se realiza una elaboración, se calcula la acústica de un origen de sonido en cualquier parte de la escena para cada uno de estos puntos de sondeo. Si el jugador no se encuentra directamente en una ubicación de punto de sondeo, se usan los datos de los puntos de sondeo más cercanos al jugador para interpolar los parámetros en esa ubicación.

Por lo tanto, es importante asegurarse de que existen puntos de sondeo en cualquier lugar al que se espera que vaya el jugador en la escena y que las áreas pequeñas y entradas estén representadas correctamente. El motor de Project Acoustics coloca los puntos de sondeo según la interpretación de la geometría de la escena y no se pueden mover ni editar en esta versión preliminar del diseñador. Úselos para comprobar la exactitud de las marcas de geometría y de los datos de sugerencias de navegación.

![Vista previa de sondeos](media/ProbesPreview.png)

### <a name="Coarse-vs-Fine-Resolution"></a>Resolución gruesa frente a fina

La única diferencia entre la configuración de resolución gruesa y la fina es la frecuencia con la que se realiza la simulación. La fina usa una frecuencia el doble de alta que la gruesa.
Aunque puede parecer simple, tiene varias implicaciones en la simulación acústica:

* La longitud de onda de la gruesa es el doble de larga que la fina y, por lo tanto, los vóxeles son el doble de grandes.
* El tiempo de simulación está directamente relacionado con el tamaño de vóxel, lo que hace que la elaboración gruesa sea 16 veces más rápida que una elaboración fina.
* Los portales (por ejemplo, puertas o ventanas) más pequeños que el tamaño de vóxel no se pueden simular. La configuración gruesa puede hacer que algunos de estos portales más pequeños no se simulen; por lo tanto, no pasarán sonidos en tiempo de ejecución. Para ver si sucede esto, puede visualizar los vóxeles.
* La frecuencia de simulación inferior da como resultado menos difracción en torno a los bordes y esquinas.
* Los orígenes del sonido no se pueden ubicar dentro de vóxeles "rellenos", que son vóxeles que contienen la geometría, ya que el resultado sería ningún sonido. Es más difícil localizar los orígenes de sonido si no están dentro de vóxeles más grandes de la configuración gruesa que cuando se usa la configuración fina.
* Los vóxeles más grandes se meten más en los portales, como se muestra a continuación. La primera imagen se creó con la configuración gruesa, mientras que la segunda es la misma entrada con resolución fina. Como indican las marcas de color rojo, hay mucha menos intrusión en la entrada cuando se usa la configuración fina. La línea azul es la entrada, como se define en la geometría, mientras que la línea roja es el portal acústico efectivo definido por el tamaño de vóxel. La evolución esta intrusión en una situación determinada dependerá completamente de cómo los vóxeles se alineen con la geometría del portal, que viene determinado por el tamaño y las ubicaciones de los objetos en la escena.

![Entrada gruesa](media/CoarseVoxelDoorway.png)

![Entrada fina](media/FineVoxelDoorway.png)

## <a name="bake-tab"></a>Pestaña Bake (Elaborar)

Cuando esté satisfecho con los datos de la vista previa, use la pestaña **Bake** (Elaborar) para enviar la elaboración a la nube.

### <a name="parts-of-the-bake-tab"></a>Partes de la pestaña Bake (Elaborar)

![Detalle de la pestaña Bake (Elaborar)](media/BakeTabDetails.png)

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
14. Haga clic en el botón Bake (Elaborar) para enviar la elaboración a la nube. Mientras se ejecuta un trabajo, se muestra **Cancel Job** (Cancelar trabajo) en su lugar.
15. Se prepara para el procesamiento de la simulación acústica en un equipo local. Vea [Elaboración local](#Local-bake) para obtener más información.  
16. En esta área se muestra el estado de la elaboración. Cuando se haya completado, debe mostrar **Downloaded** (Descargado).

Siempre puede obtener la información completa sobre los trabajos activos, los grupos de proceso y el almacenamiento en [Azure Portal](https://portal.azure.com).

Mientras un trabajo está en ejecución, el botón **Bake** (Elaborar) cambia a **Cancel Job** (Cancelar trabajo). Use este botón para cancelar el trabajo en curso. Se le pedirá confirmación antes de cancelar el trabajo. La cancelación de un trabajo no se puede deshacer, no estará disponible ningún resultado y se le cobrará por el tiempo de proceso de Azure usado.

Una vez iniciada una elaboración, puede cerrar Unity. Según el proyecto, el tipo de nodo y el número de nodos, una elaboración en la nube puede tardar varias horas. Se actualizará el estado del trabajo de elaboración al recargar el proyecto y abrir la ventana Acoustics (Acústica). Si se ha completado el trabajo, se descargará el archivo de salida.

Las credenciales de Azure se almacenan de forma segura en el equipo local y están asociadas al editor Unity. Se usan únicamente para establecer una conexión segura con Azure.

### <a name="Estimating-bake-cost"></a> Estimación del costo de elaboración de Azure

Para estimar lo que costará una elaboración determinada, tome el valor que se muestra para **Estimated Compute Cost** (Costo de proceso estimado), que es una duración, y multiplíquelo por el costo por hora en su moneda local del **VM Node Type** (Tipo de nodo VM) que ha seleccionado. El resultado no incluirá el tiempo de nodo necesario para que los nodos entren en ejecución. Por ejemplo, si selecciona **Standard_F8s_v2** para el tipo de nodo, que tiene un costo de 0,40 USD/hora y el costo estimado de proceso es de 3 horas y 57 minutos, el costo estimado para la ejecución del trabajo será 0,40 USD * ~ 4 horas = ~ 1,60 USD. El costo real probablemente será un poco mayor debido al tiempo adicional para iniciar los nodos. Puede encontrar el costo de nodo por hora en la página [Azure Batch Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (Precios de Azure Batch) (seleccione "Proceso optimizado" o "Proceso de alto rendimiento" para la categoría).

### <a name="reviewing-the-bake-results"></a>Revisión de los resultados de la elaboración

Una vez finalizada la elaboración, compruebe que los vóxeles y los puntos de sondeo estén en las ubicaciones esperadas, ejecutando el complemento de tiempo de ejecución. Encontrará más información disponible en [Design Process Overview for Acoustics](design-process.md) (Introducción al proceso de diseño para la acústica).

## <a name="Local-bake"></a>Elaboración local
La elaboración local ejecuta la simulación acústica en su PC en lugar de descargarla en el clúster de proceso de Azure Batch. Esto puede ser una buena opción para experimentar con la acústica sin necesidad de una suscripción de Azure. Tenga en cuenta que la simulación acústica conlleva muchos cálculos y puede tardar mucho tiempo en función del tamaño de la escena, la configuración de la simulación y la capacidad de proceso del equipo que realiza el procesamiento.

### <a name="minimum-hardware-requirements"></a>Requisitos mínimos de hardware
Procesador Intel de 64 bits con al menos 8 núcleos y 32 GB de RAM o superior.

Por ejemplo, en un equipo de 8 núcleos con Intel Xeon E5-1660 @ 3 GHz y 32 GB de RAM:
* Un escena pequeña con 100 sondeos tarda unas 2 horas para una elaboración gruesa y unas 32 para una de resolución fina.
* Una escena de mayor con 1000 sondeos puede tardar hasta 20 horas para una elaboración gruesa y 21 días para una de resolución fina.

### <a name="setup-docker"></a>Instalación de Docker
Instale y configure Docker en el equipo en el que se va a procesar la simulación:
1. Instale el [conjunto de herramientas de Docker](https://www.docker.com/products/docker-desktop).
2. Inicie la configuración de Docker, vaya a las opciones "Avanzadas" y configure los recursos para tener al menos 8 GB de RAM. Cuantas más CPU pueda asignar a Docker, más rápidamente se completará la elaboración. ![Configuración de Docker de ejemplo](media/DockerSettings.png)
3. Vaya a "Unidades compartidas" y active el uso compartido de la unidad que se usa para el procesamiento.![Uso compartido de la unidad Docker](media/DockerSharedDrives.png)

### <a name="run-local-bake"></a>Ejecución de la elaboración local
1. Haga clic en el botón "Prepare Local Bake" (Preparar la elaboración local) en la pestaña Bake (Elaboración) y seleccione una carpeta para guardar los archivos de entrada y los scripts de ejecución. Después, puede ejecutar la elaboración en cualquier equipo, siempre que cumpla los requisitos mínimos de hardware y tenga Docker instalado si copia la carpeta en ese equipo.
2. Inicie la simulación mediante el script "runlocalbake.bat". Este script capturará la imagen de Docker de Elementos acústicos de un proyecto con el conjunto de herramientas necesarias para el procesamiento de la simulación e iniciará la simulación. 
3. Cuando haya finalizado la simulación, copie el archivo .ace resultante al proyecto Unity. Para asegurarse de que Unity lo reconoce como un archivo binario, anexe ".bytes" a la extensión de archivo (por ejemplo, "Scene1.ace.bytes"). Los registros detallados para la simulación se almacenan en "AcousticsLog.txt". Si experimenta algún problema, comparta este archivo para ayudar con el diagnóstico.

## <a name="Data-Files"></a>Archivos de datos

Este complemento crea cuatro archivos de datos en varios puntos. Solo uno de ellos es necesario en tiempo de ejecución, por lo tanto, los otros tres están dentro de carpetas denominadas "Editor", por lo que no se compilan en el proyecto.

* **Assets/Editor/[nombreEscena]\_AcousticsParameters.asset**: este archivo almacena los datos que se especifiquen en los campos de la interfaz de usuario de Acoustics. No se puede cambiar la ubicación y el nombre de este archivo. Otros valores almacenados en este archivo afectan a la elaboración, pero son para usuarios avanzados y no deben cambiarse.
* **Assets/AcousticsData/Acoustics\_[nombreEscena].ace.bytes**: este archivo es lo que se crea durante la simulación mediante "bake" y contiene los datos de búsqueda que usa el runtime para representar la acústica de la escena. La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).
* **Assets/AcousticsData/Editor/Acoustics_[nombreEscena].vox**: Este archivo almacena la geometría de acústica convertida en vóxeles y las propiedades del material. Se calcula con el botón **Calculate...**  (Calcular...) de la pestaña Probes (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).
* **Assets/AcousticsData/Editor/Acoustics\_[nombreEscena]\_config.xml**: este archivo almacena los parámetros calculados mediante el botón **Calculate...** (Calcular...) de la pestaña **Probes** (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).

Tenga cuidado de no para eliminar el archivo *.ace.bytes descargado desde la elaboración. Este archivo no es recuperable, excepto si se vuelve a elaborar la escena.

## <a name="next-steps"></a>Pasos siguientes
* Aplicar los resultados de la elaboración a los orígenes de sonido en [Design Process Overview for Acoustics](design-process.md) (Introducción al proceso de diseño para la acústica).

