---
title: Integración de Unreal y Wwise de Project Acoustics
titlesuffix: Azure Cognitive Services
description: Se describe la integración de los complementos Unreal y Wwise de Project Acoustics en el proyecto.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 47f39e8dcd96ea3bdba564df348e9b89a6b036ba
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933185"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Integración de Unreal y Wwise de Project Acoustics
Este procedimiento proporciona pasos de integración detallados del paquete de complementos Project Acoustics en el proyecto de juegos existente de Unreal y Wwise. 

Requisitos de software:
* [Unreal Engine](https://www.unrealengine.com/) 4.20+
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.\*
* [Complemento de Wwise para Unreal](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Si va a usar una integración directa del SDK de Wwise en lugar de los complementos de Unreal para Wwise, consulte el complemento Unreal de Project Acoustics y ajuste las llamadas API de Wwise.

Si quiere usar Project Acoustics con un motor de audio distinto de Wwise, envíe una solicitud de mejora en el [foro de discusión de Project Acoustics](https://github.com/microsoft/ProjectAcoustics/issues). Puede usar el complemento Unreal de Project Acoustics para consultar datos acústicos y, luego, realizar llamadas API a su motor.

## <a name="download-project-acoustics"></a>Descarga de Project Acoustics
Si no lo ha hecho ya, descargue el [paquete de complementos Unreal y Wwise de Project Acoustics](https://www.microsoft.com/download/details.aspx?id=58090)). 

Hemos incluido un complemento Unreal Engine y un complemento mezclador Wwise en el paquete. El complemento Unreal proporciona integración del editor y el tiempo de ejecución. Durante el juego, el complemento Unreal de Project Acoustics calcula parámetros como oclusión para cada objeto de juego de cada fotograma. Estos parámetros se convierten en llamadas API de Wwise.

## <a name="review-integration-steps"></a>Revisión de los pasos de integración

Existen estos pasos principales para instalar el paquete e implementarlo en el juego.
1. Instalar el complemento mezclador de Wwise de Project Acoustics
2. (Re)implementar Wwise en el juego Este paso propaga el complemento mezclador al proyecto del juego.
3. Agregar el complemento Unreal de Project Acoustics al juego
4. Ampliación de la funcionalidad del complemento Unreal de Wwise
5. Compilar el juego y comprobar que Python está habilitado
6. Configurar el proyecto Wwise para usar Project Acoustics
7. Configuración del audio en Unreal

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. Instalar el complemento mezclador de Project Acoustics
* Abra Wwise Launcher y, en la pestaña **Plugins** (Complementos), en **Install New Plugins** (Instalar nuevos complementos), seleccione **Add From Directory** (Agregar del directorio). 

    ![Captura de pantalla de la instalación de un complemento en el iniciador de Wwise](media/wwise-install-new-plugin.png)

* Elija el directorio `AcousticsWwisePlugin\ProjectAcoustics` que se incluyó en el paquete descargado. Contiene el paquete del complemento mezclador de Wwise.

* Wwise instalará el complemento. Project Acoustics debería aparecer ahora en la lista de complementos instalados en Wwise.  
![Captura de pantalla de la lista de complementos instalados de Wwise después de la instalación de Project Acoustics](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. (Re)implementar Wwise en el juego
Vuelva a implementar Wwise en su juego, aunque ya haya integrado Wwise. Esta acción selecciona el complemento Wwise de Project Acoustics.

* **Complemento de motor:** si tiene Wwise instalado como un complemento de juego en un proyecto de Unreal en C++, omita este paso. Si, por el contrario, está instalado como un complemento de motor, por ejemplo, porque el proyecto de Unreal es solo un proyecto, la implementación de Wwise con el complemento mezclador es más compleja. Cree un proyecto ficticio de Unreal en C++ que esté vacío, ciérrelo si se abre el editor de Unreal y siga el resto del procedimiento para implementar Wwise en este proyecto ficticio. A continuación, copie el complemento Wwise implementado.
 
* En Wwise Launcher, haga clic en la pestaña **Unreal Engine**, haga clic en el menú lateral junto a **Recent Unreal Engine Projects** (Proyectos de Unreal Engine recientes) y seleccione **Browse for project** (Examinar proyecto). Abra el archivo `.uproject` del proyecto de Unreal del juego.

    ![Captura de pantalla de la pestaña Unreal del iniciador de Wwise](media/wwise-unreal-tab.png)

* A continuación, haga clic en **Integrate Wwise in Project** (Integrar Wwise en el proyecto) o **Modify Wwise in Project** (Modificar Wwise en el proyecto). Este paso (re)integra los archivos binarios de Wwise en el proyecto e incluye ahora el complemento mezclador de Project Acoustics.

* **Complemento de motor:** si usa Wwise como complemento de motor y creó el proyecto ficticio como se ha indicado anteriormente, copie la carpeta de Wwise implementada: `[DummyUProject]\Plugins\Wwise` y péguela sobre `[UESource]\Engine\Plugins\Wwise`. `[DummyUProject]` es la ruta de acceso al proyecto de Unreal en C++ vacío, y `[UESource]` es donde tiene instalados los orígenes de Unreal Engine. Cuando haya terminado la copia, puede eliminar el proyecto ficticio.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. Agregar el complemento Unreal de Project Acoustics al juego
 
* Copie la carpeta `Unreal\ProjectAcoustics` en el paquete de complementos y cree una carpeta `[UProjectDir]\Plugins\ProjectAcoustics`, donde `UProjectDir` es la carpeta del proyecto del juego que contiene el archivo `.uproject`.
  * **Complemento de motor**: si usa Wwise como complemento de motor, debe usar también Project Acoustics como complemento de motor de Unreal. En lugar del directorio de destino anterior, use: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Confirme que aparece una carpeta `Wwise` junto con la carpeta `ProjectAcoustics`. Esta carpeta contiene el complemento Wwise junto con los archivos binarios del complemento mezclador que (re)implementó en el paso 2 anterior.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. Ampliación de la funcionalidad del complemento Unreal de Wwise
* El complemento Unreal de Project Acoustics requiere la exposición de comportamiento adicional de la API del complemento Unreal de Wwise según [estas instrucciones](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html). Hemos incluido un archivo por lotes para automatizar el procedimiento de la aplicación de revisiones. 
* Dentro de `Plugins\ProjectAcoustics\Resources`, ejecute `PatchWwise.bat`. En la imagen de ejemplo siguiente se usa nuestro proyecto de ejemplo AcousticsGame.

    ![Captura de pantalla de una ventana del Explorador de Windows que resalta el script proporcionado para revisión de Wwise](media/patch-wwise-script.png)

* Si no tiene instalado el SDK de DirectX, dependiendo de la versión de Wwise que utilice, es posible que tenga que convertir en comentario la línea que contiene `DXSDK_DIR` en `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`:

    ![Captura de pantalla del editor de código que muestra DXSDK convertido en comentario](media/directx-sdk-comment.png)

* Si compila con Visual Studio 2019, para solucionar un error de vinculación con Wwise, cambie manualmente el valor `VSVersion` predeterminado en `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs` por `vc150`:

    ![Captura de pantalla del editor de código que muestra el cambio de VSVersion a vc150](media/vsversion-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Compilar el juego y comprobar que Python está habilitado

* Compile su juego y asegúrese de que se compila correctamente. En caso contrario, compruebe los pasos anteriores con cuidado antes de continuar. 
* Abra el proyecto en Unreal Editor. 
* **Complemento de motor:** Si usa ProjectAcoustics como complemento de motor, asegúrese también de que está habilitado, es decir, aparece en la lista de complementos "integrados".
* Verá un nuevo modo, que indica que se ha integrado Project Acoustics.

    ![Captura de pantalla de Unreal que muestra el modo de Acoustics completo](media/acoustics-mode-full.png)

* Confirme que tiene habilitado el complemento de Python para Unreal. Es necesario para que la integración del editor funcione correctamente.

    ![Captura de pantalla de habilitación de las extensiones de Python en Unreal Editor](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. Configuración del proyecto de Wwise

Con la descarga de ejemplos se incluye un proyecto de ejemplo de Wwise. Se recomienda examinar juntas estas instrucciones. Las capturas de pantalla siguientes están tomadas de este proyecto.

### <a name="bus-setup"></a>Configuración del bus
* El complemento Unreal de Project Acoustics buscará el complemento mezclador asociado en un bus con este nombre ***exacto***: `Project Acoustics Bus`. Cree un bus de audio con este nombre. El complemento mezclador puede funcionar en diversas configuraciones, pero por ahora, se supone que se usará solo para el procesamiento de la reverberación. Este bus transmitirá la señal de reverberación mezclada de todos los orígenes que usan Acoustics. Puede mezclar de forma ascendente hasta formar cualquier estructura de mezcla de bus; a continuación se muestra un ejemplo tomado de nuestro proyecto de ejemplo de Wwise incluido en la descarga de ejemplo.

    ![Captura de pantalla de buses de Wwise que muestran el bus de Project Acoustics](media/acoustics-bus.png)

* La configuración del canal en el bus debe establecerse en uno de: `1.0, 2.0, 4.0, 5.1 or 7.1`. Otras configuraciones no generarán ningún resultado en este bus.

    ![Captura de pantalla de las opciones de configuración del canal para el bus de Project Acoustics](media/acoustics-bus-channel-config.png)

* Ahora, vaya a los detalles del bus de Project Acoustics y asegúrese de que puede ver la pestaña del complemento mezclador.

    ![Captura de pantalla de Wwise que muestra cómo habilitar la pestaña del complemento mezclador para el bus de Project Acoustics](media/mixer-tab-enable.png)

* A continuación, vaya a la pestaña del complemento mezclador y agregue el de Project Acoustics al bus.

    ![Captura de pantalla del bus de Wwise que muestra cómo agregar el complemento mezclador de Project Acoustics](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Configuración de la jerarquía actor-mezclador
* Por motivos de rendimiento, Project Acoustics aplica DSP de audio a todos los orígenes al mismo tiempo. Para ello, es necesario que el complemento funcione como un complemento mezclador. Wwise requiere que los complementos mezcladores estén en el bus de salida, aunque este suele transportar normalmente la señal de salida sin efectos. Project Acoustics requiere que la señal sin efectos se enrute a través de buses auxiliares, mientras que la señal con efectos se transporta en `Project Acoustics Bus`. El siguiente proceso admite la migración gradual a este flujo de señal.

* Supongamos que tiene un proyecto con una jerarquía actor-mezclador que contiene pisadas, armas y demás en el nivel superior. Cada uno tiene el bus de salida correspondientes para su mezcla sin efectos. Supongamos que quiere migrar las pisadas para usar la acústica. En primer lugar, creará un bus auxiliar correspondiente para que transporte su submezcla sin efectos que es un elemento secundario del bus de salida de pisadas. En la siguiente imagen de ejemplo hemos usado el prefijo "dry" para organizarlos, aunque el nombre exacto no es importante. Los medidores o efectos que tenía en el bus de pisadas seguirán funcionando como antes.

    ![Captura de pantalla de la configuración de mezcla sin efectos recomendada de Wwise](media/wwise-dry-mix-setup.png)

* A continuación, modifique la estructura de salida del bus para el actor-mezclador de pisadas de la manera siguiente: el bus de Project Acoustics debe estar establecido como el bus de salida y Dry_Footsteps como el bus auxiliar definido por el usuario.

    ![Captura de pantalla de configuración recomendada del bus actor-mezclador de Wwise](media/actor-mixer-bus-settings.png)

* Ahora todas las pisadas reciben tratamiento acústico y generan su reverberación en el bus de Project Acoustics. La señal sin efectos se enruta a través de Dry_Footsteps y se espacializa como de costumbre.

* Project Acoustics solo se aplica a los sonidos que tengan una ubicación 3D en el mundo. Según la [documentación de Wwise](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), las propiedades de posición deben establecerse como se muestra. El valor "3D Spatialization" (Espacialización 3D) puede ser "Position" (Posición) o "Position + Orientation" (Posición+Orientación), según sea necesario.

    ![Captura de pantalla de la configuración recomendada de posicionamiento de actor de Wwise](media/wwise-positioning.png)

* Establecer el bus de salida en algún otro bus que mezcla de forma ascendente para formar el **bus de Project Acoustics** no funcionará. Wwise impone este requisito sobre los complementos mezcladores.

* Si quiere que un elemento secundario de la jerarquía actor-mezclador de pisadas no use acústica, puede usar siempre la opción para invalidar principal para excluirlo.

* Si va a usar envíos definidos por el usuario o el juego para la reverberación en cualquier actor-mezclador del juego, desactívelos en este actor-mezclador para evitar aplicar la reverberación dos veces.

### <a name="spatialization"></a>Espacialización
De forma predeterminada, el complemento mezclador Wwise de Project Acoustics aplica reverberación por convolución, lo que deja que Wwise realice la espacialización de panoramización. Cuando se usa Project Acoustics en esta configuración predeterminada de solo reverberación, se puede usar libremente cualquier configuración de canal y método de espacialización en la mezcla sin efectos, lo que le permite mezclar y combinar casi cualquier espacializador con la reverberación de Project Acoustics. Las opciones incluyen [espacializadores binaurales basados en Ambisonics](https://www.audiokinetic.com/products/ambisonics-in-wwise/) y [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound).
 
Project Acoustics incluye un espacializador opcional que admite tanto la panoramización como la representación HRTF de alta resolución basada en objetos. Active la casilla "Perform Spatialization" (Realizar espacialización) en la configuración del complemento mezclador, elija entre HRTF o panoramización y deshabilite los envíos auxiliares definidos por el usuario anteriores para todos los buses sin efectos con el fin de evitar la espacialización dos veces: con el complemento mezclador de Project Acoustics y con Wwise. El modo de espacialización no se puede cambiar en tiempo real, ya que requiere la regeneración del banco de sonido. Debe reiniciar Unreal y luego volver a generar bancos de sonido antes de pulsar el botón de reproducción para seleccionar los cambios de configuración del complemento mezclador, como la casilla "Perform Spatialization" (Realizar espacialización).

![Captura de pantalla de configuración de la espacialización del complemento mezclador de Wwise](media/mixer-spatial-settings.png)

Lamentablemente, en este momento no se admiten otros complementos espacializadores basados en objetos dado que se implementan como complementos mezcladores, y Wwise no permite actualmente la asignación de varios complementos mezcladores a un único actor-mezclador.  

## <a name="7-audio-setup-in-unreal"></a>7. Configuración del audio en Unreal
* En primer lugar, deberá simular mediante "bake" el nivel de juego para producir un recurso acústico, que se colocará en `Content\Acoustics`. Consulte el [Tutorial de simulación mediante "bake" de Project Acoustics con Unreal](unreal-baking.md) y prosiga desde aquí. Algunos niveles previamente simulados mediante "bake" se incluyen en el paquete de ejemplos.
* Cree un actor de espacio acústico en la escena. Cree solo uno de estos actores en un nivel que represente la acústica del nivel entero. 

    ![Captura de pantalla de Unreal Editor que muestra la creación del actor de espacio acústico](media/create-acoustics-space.png)

* Ahora, asigne el recurso de datos acústicos simulados mediante "bake" a la ranura de datos acústicos del actor de espacio acústico. ¡La escena tiene ahora acústica!

    ![Captura de pantalla de Unreal Editor que muestra la asignación de recursos acústicos](media/acoustics-asset-assign.png)

* Ahora, agregue un actor vacío y haga lo siguiente:

    ![Captura de pantalla de Unreal Editor que muestra el uso del componente acústico en un actor vacío](media/acoustics-component-usage.png)

1. Agregue un componente de audio acústico al actor. Este componente amplía el componente de audio de Wwise con funcionalidad de Project Acoustics.
2. El cuadro Play on Start (Reproducir al inicio) está activado de forma predeterminada, lo que desencadenará el evento de Wwise asociado al inicio del nivel.
3. Use la casilla Show Acoustics Parameters (Mostrar parámetros de acústica) para imprimir la información de depuración en pantalla sobre la fuente.  
    ![Captura de pantalla del panel de acústica de Unreal Editor en la fuente de sonido con los valores de depuración habilitados](media/debug-values.png)
4. Asignación de un evento de Wwise por el flujo de trabajo habitual de Wwise
5. Asegúrese de que está activada la opción "Use Spatial Audio" (Usar audio espacial). En este momento, si usa Project Acoustics para un componente de audio en particular, no puede usar simultáneamente el motor de audio espacial de Wwise para la acústica.

Está listo. ¡Muévase por la escena y explore los efectos acústicos!

## <a name="next-steps"></a>Pasos siguientes
* Tutorial de [diseño](unreal-workflow.md) para Project Acoustics en Unreal y Wwise
* [Aprender a simular mediante "bake"](unreal-baking.md) las escenas de un juego 
