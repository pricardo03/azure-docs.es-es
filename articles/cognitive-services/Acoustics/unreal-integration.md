---
title: Integración de Wwise y proyecto acústica Unreal
titlesuffix: Azure Cognitive Services
description: Este procedimiento describe la integración del proyecto acústica Unreal y Wwise complementos en el proyecto.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 0baaf31386e1155dee6ca2bbfda6827ca3fc36fe
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313454"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Integración de Wwise y proyecto acústica Unreal
Este procedimiento proporciona pasos de integración detallada del paquete de complemento acústica del proyecto en el proyecto existente de juegos de Unreal y Wwise. 

Requisitos de software:
* [Motor Unreal](https://www.unrealengine.com/) 4.21
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1. +
* [Complemento de Wwise para Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Si usa una integración directa de Wwise SDK en lugar de usar los complementos Wwise Unreal, consulte el complemento de Project acústica Unreal y ajustar las llamadas API Wwise.

Si desea usar acústica del proyecto con un motor de audio que no sea Wwise, póngase en contacto con nosotros en el [foros del proyecto acústica](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics). Puede usar el complemento de Project acústica Unreal para consultar datos acústica y, a continuación, realizar llamadas de API a su motor.

## <a name="download-project-acoustics"></a>Descargar proyecto acústica
Si no lo ha hecho ya, descargue el [paquete de complemento de Project acústica Unreal & Wwise](https://www.microsoft.com/download/details.aspx?id=58090)). 

Hemos incluido un complemento de Unreal Engine y un complemento de mixer Wwise en el paquete. El complemento de Unreal proporciona integración de editor y en tiempo de ejecución. Durante el juego, el complemento de Project acústica Unreal calcula parámetros como oclusión para cada objeto de juego cada fotograma. Estos parámetros se convierten en llamadas API Wwise.

## <a name="review-integration-steps"></a>Revise los pasos de integración

Existen estos pasos principales para instalar el paquete e implementarlo en su juego.
1. Instalar el complemento de Project acústica Wwise mixer
2. (Re) implementar Wwise en su juego. Este paso propaga el complemento de mixer en su proyecto de juego.
3. Agregue el complemento de Project acústica Unreal a tu juego
4. Ampliar la funcionalidad de Wwise complemento Unreal
5. Compile juegos y compruebe que Python está habilitado
6. Configurar el proyecto Wwise para usar proyecto acústica
7. Configuración de audio en Unreal

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. Instalar el complemento de mixer acústica del proyecto
* Abrir selector Wwise, a continuación, en el **complementos** ficha **instalar nuevos complementos**, seleccione **agregar desde el directorio**. 

    ![Captura de pantalla de la instalación de un complemento en el iniciador de Wwise](media/wwise-install-new-plugin.png)

* Elija la `AcousticsWwisePlugin\ProjectAcoustics` directorio que se incluyó en el paquete descargado. Contiene el paquete de complemento de mixer Wwise.

* Wwise se instalará el complemento. Proyecto acústica debería aparecer ahora en la lista de complementos instalados en Wwise.
![Lista de complementos de captura de pantalla de Wwise instalado después de la instalación de proyecto acústica](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. (Re) implementar Wwise en tu juego
Volver a implementar Wwise en su juego, incluso si ya ha integrado Wwise. Selecciona el complemento de Project acústica Wwise.

* **Complemento de motor:** Si tiene Wwise instalado como un complemento de juego en un proyecto de C++ Unreal, omita este paso. Si está instalado en su lugar como un complemento de motor de instancia porque el proyecto Unreal es única, implementación de Wwise Blueprint con nuestro complemento mezclador es más compleja. Crear un proyecto de C++ Unreal ficticio y vacío, ciérrela si abre el editor de Unreal y siga el procedimiento para implementar Wwise en este proyecto ficticio restante. A continuación, copie el complemento Wwise implementado.
 
* Desde el selector de Wwise, haga clic en el **Unreal Engine** y, después, haga clic en el menú de hamburguesa junto a **proyectos recientes de Unreal Engine** y seleccione **Buscar proyecto**. Abrir proyecto Unreal tu juego `.uproject` archivo.

    ![Pestaña de captura de pantalla de Wwise del iniciador Unreal](media/wwise-unreal-tab.png)

* A continuación, haga clic en **Wwise integrar en el proyecto** o **Wwise modificar en el proyecto**. Este paso (re) integra Wwise los archivos binarios en el proyecto, ahora incluye el complemento de mixer acústica del proyecto.

* **Complemento de motor:** Si usa Wwise como un complemento de motor y creó el proyecto ficticio que antes, copie la carpeta Wwise implementado: `[DummyUProject]\Plugins\Wwise` y péguela sobre `[UESource]\Engine\Plugins\Wwise`. `[DummyUProject]` es la ruta de proyecto vacía de C++ Unreal, y `[UESource]` es donde tiene instalados los orígenes de Unreal Engine. Cuando haya terminado copiar, puede eliminar el proyecto ficticio.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. Agregue el complemento de Project acústica Unreal a tu juego
 
* Copia el `Unreal\ProjectAcoustics` carpeta en el complemento del paquete y cree una nueva carpeta `[UProjectDir]\Plugins\ProjectAcoustics`, donde `UProjectDir` es la carpeta de proyecto de tu juego que contiene el `.uproject` archivo.
  * **Complemento de motor**: Si usa Wwise como un complemento de motor, debe usar acústica del proyecto como un complemento de motor Unreal también. En lugar del directorio de destino anterior, use: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Confirme que aparece un `Wwise` carpeta junto con el `ProjectAcoustics` carpeta. Contiene el complemento Wwise junto con los archivos binarios para el complemento de mixer que (re-) implementado en el paso 2 anterior.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. Ampliar la funcionalidad de Wwise complemento Unreal
* El complemento de Project acústica Unreal requiere un comportamiento adicional se pueden exponer desde el complemento de Wwise Unreal API por [estas directrices](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). Hemos incluido un archivo por lotes para automatizar el procedimiento de la aplicación de revisiones. 
* Dentro de `Plugins\ProjectAcoustics\Resources`, ejecute `PatchWwise.bat`. La imagen de ejemplo siguiente usa nuestro proyecto de ejemplo AcousticsGame.

    ![Resaltado de la ventana en la captura de pantalla del explorador de Windows proporciona el script para la revisión Wwise](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX, que necesitará para marcar como comentario la línea que contiene DXSDK_DIR en `[UProject]\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`

    ![Captura de pantalla del editor de código que muestra DXSDK comentada](media/directx-sdk-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Compile juegos y compruebe que Python está habilitado

* Compile su juego y asegúrese de que se compila correctamente. En caso contrario, compruebe los pasos anteriores con cuidado antes de continuar. 
* Abra el proyecto en el Editor de Unreal. 
* **Complemento de motor:** Si usa ProjectAcoustics como complemento de motor, también Asegúrese de que está habilitada, aparece en "integrados" complementos.
* Debería ver un nuevo modo, que indica que se ha integrado acústica del proyecto.

    ![Captura de pantalla de Unreal mostrando acústica modo completo](media/acoustics-mode-full.png)

* Confirme que tiene el complemento de Python para Unreal habilitado. Esto es necesario para la integración del editor funcionar correctamente.

    ![Captura de pantalla de habilitación de extensiones de Python en el editor de Unreal](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. Configuración del proyecto Wwise

Un proyecto de ejemplo Wwise se incluye con la descarga de ejemplos. Se recomienda mirar junto con estas instrucciones. Las capturas de pantalla siguiente se toman de este proyecto.

### <a name="bus-setup"></a>Configuración de bus
* El complemento de Project acústica Unreal buscará el complemento de mixer asociado en un bus con este ***exacta*** nombre: `Project Acoustics Bus`. Cree un nuevo bus de audio con este nombre. El complemento de mixer puede funcionar en diversas configuraciones, pero por ahora, se supone que se usará para la reverberación solo procesamiento. Este bus transmitirá la señal de reverberación mixto para todos los orígenes que usan acústica. Puede mezclar ascendente en cualquier bus de mezclar la estructura, se muestra un ejemplo siguiente, procedente de nuestro proyecto de ejemplo Wwise incluido en la descarga de ejemplo.

    ![Buses de captura de pantalla de Wwise mostrando proyecto acústica Bus](media/acoustics-bus.png)

* La configuración del canal en el bus debe establecerse en uno de: `1.0, 2.0, 4.0, 5.1 or 7.1`. Otras configuraciones se producirá ningún resultado en este bus.

    ![Captura de pantalla de opciones de configuración de canal para el proyecto acústica Bus](media/acoustics-bus-channel-config.png)

* Ahora, vaya en la acústica proyecto bus detalles y asegúrese de que puede ver la pestaña complemento Mixer

    ![Captura de pantalla de Wwise que muestra cómo habilitar la ficha de complemento de Mixer para proyecto acústica Bus](media/mixer-tab-enable.png)

* A continuación, vaya a la pestaña complemento Mixer y agregue el Mezclador de acústica de proyecto complemento en el bus

    ![Bus de la opción de Wwise que muestra cómo agregar el complemento de Project acústica Mixer](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Configuración de la jerarquía Mezclador de actor
* Por motivos de rendimiento, proyecto acústica se aplica DSP de audio a todos los orígenes al mismo tiempo. Esto requiere el complemento para que funcione como un complemento de mixer. Wwise requiere Mezclador de complementos en el bus de salida, aunque el bus de salida normalmente lleva la señal de salida seco. Proyecto acústica requiere la señal seca enrutarse a través de buses aux mientras se lleva a la señal húmeda en el `Project Acoustics Bus`. El siguiente proceso admite la migración gradual a este flujo de señal.

* Supongamos que tiene un proyecto existente con una jerarquía de mezclador de actor que contiene la iniciativa, armas y otros en el nivel superior. Cada uno tiene el bus de salida correspondientes para su sopa. Permite que desea migrar la iniciativa para usar acústica. En primer lugar, cree un bus aux correspondiente para llevar a cabo su submezcla seca que es un elemento secundario del bus de salida de iniciativa. Por ejemplo, hemos usado un prefijo "Seco" en la imagen siguiente para organizar estos, aunque no es importante el nombre exacto. Los medidores o efectos que tenía en el bus de iniciativa seguirán funcionando como antes.

    ![Captura de pantalla del programa de instalación de combinación de Dry de Wwise recomendada](media/wwise-dry-mix-setup.png)

* A continuación, modifique la estructura de salida de bus para el Mezclador de actor iniciativa como sigue, con proyecto acústica Bus establecer como el Bus de salida y Dry_Footsteps establecido como un bus aux definido por el usuario.

    ![Captura de pantalla de configuración de Bus de Mixer de Actor Wwise recomendada](media/actor-mixer-bus-settings.png)

* Ahora todos los pasos obtención tratamiento acústica y su reverberación en el Bus acústica de proyecto de salida. La señal seca se enruta a través de Dry_Footsteps y spatialized como de costumbre.

* Proyecto acústica solo se aplica a los sonidos que tengan una ubicación 3D del mundo. Siga [Wwise documentación](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), las propiedades de posición deben establecerse como se muestra. La configuración de "espacialización 3D, dejen" puede ser "Posición" o "Posición + Orientation" según sea necesario.

    ![Captura de pantalla de la configuración recomendada de posicionamiento de Actor de Wwise](media/wwise-positioning.png)

* Si se establece el Bus de salida en algunos otros bus que mezcla de nivel superior en **proyecto acústica Bus** no funcionará. Wwise impone este requisito en complementos de mixer.

* Si desea que un elemento secundario en la jerarquía de actor Mezclador de iniciativa para no usar acústica, puede usar siempre "invalidar a primario" en él para dejar de participar.

* Si usa envíos definido por el usuario o el juego para reverberación en cualquier Mezclador de actor en el juego, desactive los en este Mezclador de actor para evitar aplicar reverberación dos veces.

### <a name="spatialization"></a>Spatialization
De forma predeterminada, el complemento de mixer proyecto acústica Wwise aplica reverberación convolución, dejando Wwise para hacer spatialization panorámica. Cuando se usa el proyecto acústica en esta configuración predeterminada, solo reverberación, es libre de usar cualquier método de configuración y spatialization de canal en su combinación seco, lo que le permite mezclar y combinar casi cualquier espacial con Reverberación de proyecto acústica. Existen varias opciones [spatializers biauriculares basado en Ambisonics](https://www.audiokinetic.com/products/ambisonics-in-wwise/) y [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Proyecto acústica incluye un espacial opcional que admite la representación HRTF alta resolución basada en objetos y movimiento panorámico. Active la casilla "Realizar Spatialization" en las opciones del complemento mixer y elegir entre la HRTF o movimientos panorámicos y deshabilitar envía aux definido por el usuario configurar anteriormente a la de buses de dry para evitar spatializing dos veces, tanto con proyecto acústica mezclador complemento y Wwise. El modo spatialization no se puede cambiar en tiempo real, ya que requiere una regeneración del banco de sonido. Debe reiniciar Unreal y luego volver a generar los bancos de sonido antes de alcanzar las play para recoger los cambios de configuración del complemento de mezclador como la casilla 'Realizar Spatialization'.

![Configuración de captura de pantalla de Wwise Mixer complemento Spatialization](media/mixer-spatial-settings.png)

Lamentablemente, no se admiten otros complementos espacial basado en objetos en este momento tal como se implementan como complementos de mixer y Wwise actualmente no permite que varios complementos de mixer asignado a un único Mezclador de actor.  

## <a name="7-audio-setup-in-unreal"></a>7. Configuración de audio en Unreal
* En primer lugar, deberá integrar su nivel de juego para producir un activo acústica, que se colocará en `Content\Acoustics`. Consulte la [Unreal Tutorial platos preparados](unreal-baking.md) y reanudar aquí. Algunos niveles previamente al horno se incluyen en el paquete de ejemplo.
* Creación de un actor acústica espacio en la escena. Sólo crear uno de estos actores en un nivel que representa la acústica para el nivel de todo. 

    ![Captura de pantalla de Unreal editor que muestra la creación de actor acústica espacio](media/create-acoustics-space.png)

* Ahora puede asignar al recurso de datos acústicos al horno a la ranura de datos de acústica en el actor acústica espacio. ¡La escena ahora tiene acústica!

    ![Captura de pantalla de Unreal acústica de howing editor s asignación de recursos](media/acoustics-asset-assign.png)

* Ahora agregue un actor vacío y haga lo siguiente:

    ![Captura de pantalla de Unreal editor que muestra el uso del componente acústica en un actor vacío](media/acoustics-component-usage.png)

1. Agregue un componente Audio acústica al actor. Este componente amplía el componente audio Wwise con funcionalidad para acústica del proyecto.
2. La reproducción en el cuadro de inicio está activada de forma predeterminada, lo que desencadenará el evento Wwise asociado al inicio de nivel.
3. Utilice la casilla de verificación Mostrar acústica parámetros para imprimir en la pantalla información de depuración sobre el origen.
    ![Captura de pantalla de Unreal panel de acústica del editor de origen de sonido con valores de la depuración habilitada](media/debug-values.png)
4. Asignar un evento Wwise por el flujo de trabajo habitual de Wwise
5. Asegúrese de que Use espacial Audio está desactivado. En este momento, si usas acústica de proyecto para un determinado componente audio, no puede simultáneamente usar motor de Audio espaciales del Wwise para acústica.

Está listo. ¡Moverse por la escena y explorar los efectos acústicos!

## <a name="next-steps"></a>Pasos siguientes
* [Diseño](unreal-workflow.md) tutorial para el proyecto acústica en Unreal/Wwise
* [Aprenda a realizar prepara](unreal-baking.md) para sus escenas de juegos 
