---
title: Azure Stream Analytics en IoT Edge (versión preliminar)
description: Cree trabajos perimetrales en Azure Stream Analytics e impleméntelos en dispositivos en los que se ejecuta Azure IoT Edge.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/16/2017
ms.openlocfilehash: 73b594aaabd814108dfce813b53a4ea865336e63
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985070"
---
# <a name="azure-stream-analytics-on-iot-edge-preview"></a>Azure Stream Analytics en IoT Edge (versión preliminar)

> [!IMPORTANT]
> Esta funcionalidad se encuentra en versión preliminar y su uso no se recomienda en producción.
 
Azure Stream Analytics (ASA) en IoT Edge permite a los desarrolladores desarrollar inteligencia analítica casi en tiempo real más próxima a los dispositivos IoT para que puedan desbloquear el valor total de los datos generados por los dispositivos. Azure Stream Analytics está diseñado con los objetivos de baja latencia, resiliencia, uso eficiente de ancho de banda y cumplimiento. Las empresas pueden implementar ahora la lógica de control cerca de las operaciones industriales y complementar los análisis de macrodatos que se realizan en la nube.  

Azure Stream Analytics en IoT Edge se ejecuta en el marco de [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). Una vez que se crea el trabajo en ASA, puede implementar y administrar los trabajos de ASA mediante IoT Hub. Esta característica se encuentra en su versión preliminar. Si tiene alguna pregunta o comentario, puede usar [esta encuesta](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) para ponerse en contacto con el equipo de producto. 

## <a name="scenarios"></a>Escenarios
![Diagrama de alto nivel](media/stream-analytics-edge/ASAedge_highlevel.png)

* **Comando de baja latencia y control**: por ejemplo, los sistemas de seguridad en la fabricación deben responder a los datos operativos con una latencia muy baja. Con ASA en IoT Edge, puede analizar los datos de sensores casi en tiempo real y emitir comandos cuando detecte anomalías para detener una máquina o desencadenar alertas.
*   **Conectividad limitada a la nube**: los sistemas críticos, como un equipo de minería remoto, buques conectados o las perforaciones petrolíferas en mar abierto necesitan analizar y reaccionar ante los datos, incluso cuando la conectividad a la nube sea intermitente. Con el ASA, la lógica de streaming funciona independientemente de la conectividad de red y puede elegir lo que envía a la nube para su posterior procesamiento o almacenamiento.
* **Ancho de banda limitado**: el volumen de datos que generan los motores de jet o los automóviles conectados puede ser tan grande que los datos se deben filtrar o procesar con anterioridad antes de enviarlos a la nube. ASA puede filtrar o agregar los datos que se envían a la nube.
* **Cumplimiento**: el cumplimiento de las normas puede requerir que se agreguen algunos datos o que se oculte su identidad localmente antes de enviarlos a la nube.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Trabajos de Edge en Azure Stream Analytics
### <a name="what-is-an-edge-job"></a>¿Qué es un trabajo "perimetral"?

Los trabajos de Edge de ASA se ejecutan como módulos en el [runtime de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works). Constan de dos partes:
1.  Una parte en la nube que es responsable de la definición del trabajo: los usuarios definen las entradas, la salida, las consultas y otros valores (eventos que no funcionan, etc.) en la nube.
2.  El módulo de ASA en IoT Edge que se ejecuta localmente. Contiene el motor de procesamiento de eventos complejos de ASA y recibe la definición del trabajo de la nube. 

ASA usa IoT Hub para implementar los trabajos perimetrales en los dispositivos. Para más información acerca de [la implementación de IoT Edge, consulte aquí](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

![Trabajo perimetral](media/stream-analytics-edge/ASAedge_job.png)


### <a name="installation-instructions"></a>Instrucciones de instalación
Los valores posibles se describen en la tabla siguiente. En las secciones siguientes encontrará más información.
|      |Paso   | Lugar     | Notas   |
| ---   | ---   | ---       |  ---      |
| 1   | **Creación de un contenedor de almacenamiento**   | Azure Portal       | Los contenedores de almacenamiento se utilizan para guardar la definición de trabajo donde pueden obtenerla sus dispositivos de IoT. <br>  Todos los contenedor de almacenamiento existente se pueden reutilizar.     |
| 2   | **Creación de un trabajo perimetral de ASA**   | Azure Portal      |  Cree un nuevo trabajo y seleccione **Edge** como **entorno de hospedaje**. <br> Estos trabajos se crean o administran desde la nube y se ejecutan en sus propios dispositivos de IoT Edge.     |
| 3   | **Configuración de un entorno de IoT Edge en los dispositivos**   | Dispositivos      | Instrucciones para [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) o [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).          |
| 4   | **Implementación de ASA en dispositivos de IoT Edge**   | Azure Portal      |  La definición del trabajo ASA se exporta al contenedor de almacenamiento que creó anteriormente.       |
Puede seguir [este tutorial paso a paso](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) para implementar su primer trabajo de ASA en IoT Edge. El vídeo siguiente debería ayudarle a entender el proceso para ejecutar un trabajo de Stream Analytics en un dispositivo con IoT Edge:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Creación de un contenedor de almacenamiento
Se requiere un contenedor de almacenamiento es necesario para exportar la consulta ASA compilada y la configuración del trabajo. Se utiliza para configurar la imagen de Docker ASA con una consulta específica. 
1. Siga [estas instrucciones](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) para crear una cuenta de almacenamiento desde Azure Portal. Puede mantener todas las opciones predeterminadas para usar esta cuenta con ASA.
2. En la cuenta recién creada, cree un contenedor de almacenamiento de blobs:
    1. Haga clic en **Blobs** y, después, en **+ Contenedor**. 
    2. Escriba un nombre y mantenga el contenedor como **Privado**.

#### <a name="create-an-asa-edge-job"></a>Creación de un trabajo de Edge de ASA
> [!Note]
> Este tutorial se centra en la creación de un trabajo ASA mediante Azure Portal. También puede [usar el complemento de Visual Studio para crear un trabajo de Edge de ASA](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

1. En Azure Portal, cree un nuevo "trabajo de Stream Analytics". [Vínculo directo para crear un nuevo trabajo de ASA aquí](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. En la pantalla de creación, seleccione **Edge** como **entorno de hospedaje** (vea la imagen siguiente) ![Creación de trabajo](media/stream-analytics-edge/ASAEdge_create.png)
3. Definición de trabajo
    1. **Definir flujos de entrada**. Defina uno o varios flujos de entrada para el trabajo.
    2. Definición de datos de referencia (opcional).
    3. **Definir flujos de salida**. Defina uno o varios flujos de salida para el trabajo. 
    4. **Definir consulta**. Defina la consulta ASA en la nube mediante el editor insertado. El compilador comprueba automáticamente la sintaxis habilitada para el perímetro de ASA. La consulta también se puede probar mediante la carga de datos de ejemplo. 
4. Establezca la información del contenedor de almacenamiento en el menú **Configuración de IoT Edge**.
5. Establecimiento de valores opcionales
    1. **Ordenación de eventos**. Puede configurar la directiva de fuera de servicio en el portal. La documentación está disponible [aquí](https://msdn.microsoft.com/library/azure/mt674682.aspx?f=255&MSPPError=-2147217396).
    2. **Configuración regional**. Defina el formato de internalización.



> [!Note]
> Cuando se crea una implementación, ASA exporta la definición del trabajo a un contenedor de almacenamiento. Esta definición de trabajo siguen siendo la misma mientras dure una implementación. En consecuencia, si desea actualizar un trabajo que se ejecuta en el perímetro, es preciso que lo edite en ASA y que, a continuación, cree una nueva implementación en IoT Hub.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>Configuración de un entorno de IoT Edge en los dispositivos
Los trabajos de Edge se pueden implementar en dispositivos que ejecuten Azure IoT Edge.
Para hacerlo, es preciso seguir estos pasos:
- Cree una instancia de IoT Hub.
- Instale Docker y el entorno de ejecución de Azure IoT Edge en los dispositivos perimetrales.
- Configure los dispositivos como **dispositivos de IoT Edge** en IoT Hub.

Estos pasos se describen en la documentación de IoT Edge para [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) o [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>Implementación de ASA en dispositivos de IoT Edge
##### <a name="add-asa-to-your-deployment"></a>Adición de ASA a una implementación
- En Azure Portal, abra IoT Hub, vaya a **IoT Edge** y haga clic en el dispositivo que va a ser el destino de esta implementación.
- Seleccione **Establecer módulos**, **+ Agregar** y, finalmente, **Módulo de Azure Stream Analytics**.
- Seleccione la suscripción y el trabajo Edge de ASA que ha creado. Haga clic en Guardar.
![Adición de un módulo de ASA a una implementación](media/stream-analytics-edge/set_module.png)


> [!Note]
> Durante este paso, ASA crea una carpeta llamada "EdgeJobs" en el contenedor de almacenamiento (si aún no existe). Para cada implementación, se crea una nueva subcarpeta en la carpeta "EdgeJobs".
> Con el fin de implementar el trabajo en dispositivos perimetrales, ASA crea una firma de acceso compartido (SAS) para el archivo de definición de trabajo. La clave SAS se transmite de forma segura a los dispositivos de IoT Edge que usen un dispositivo gemelo. Esta clave expira a los tres años de haberse creado.


Para más información sobre las implementaciones de IoT Edge, consulte [esta página](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).


##### <a name="configure-routes"></a>Configuración de rutas
IoT Edge proporciona una manera de enrutar de forma declarativa mensajes tanto entre los módulos como entre los módulos e IoT Hub. La sintaxis completa se describe [aquí](https://docs.microsoft.com/azure/iot-edge/module-composition).
Los nombres de las entradas y salidas creadas en el trabajo de ASA se pueden utilizar como puntos de conexión para el enrutamiento.  

###### <a name="example"></a>Ejemplo
```
{
"routes": {                                              
    "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
    "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream", 
    "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")" 
}
}   

```
En este ejemplo se muestran las rutas del escenario descrito en la siguiente ilustración. Contiene un trabajo perimetral denominado "**ASA**", con una entrada denominada "**temperature**" y una salida denominada "**alert**".
![Ejemplo de enrutamiento](media/stream-analytics-edge/RoutingExample.png)

En este ejemplo se definen las siguientes rutas:
- Todos los mensaje de **tempSensor** se envían al módulo denominado **ASA**, a la entrada denominada **temperature**.
- Todas las salidas del módulo de **ASA** se envían a la instancia de IoT Hub vinculada a este dispositivo ($upstream).
- Todas las salidas del módulo de **ASA** se envían al punto de conexión **control** de **tempSensor**.


## <a name="technical-information"></a>Información técnica
### <a name="current-limitations-for-edge-jobs-compared-to-cloud-jobs"></a>Limitaciones actuales de los trabajos perimetrales, en comparación con los trabajos en la nube
El objetivo es tener paridad entre los trabajos perimetrales y los trabajos en la nube. La mayoría de las características de lenguaje de consulta de SQL ya se admiten.
Sin embargo, las siguientes características aún no se admiten en los trabajos perimetrales:
* Funciones definidas por el usuario (UDF) y agregados definidos por el usuario (UDA).
* Funciones de Azure Machine Learning.
* Uso de más de 14 agregados en un solo paso.
* Formato AVRO para la entrada y salida. En este momento solo se admiten CSV y JSON.
* Los siguientes operadores SQL:
    * AnomalyDetection
    * Operadores geoespaciales:
        * CreatePoint
        * CreatePolygon
        * CreateLineString
        * ST_DISTANCE
        * ST_WITHIN
        * ST_OVERLAPS
        * ST_INTERSECTS
    * PARTITION BY
    * GetMetadataPropertyValue


### <a name="runtime-and-hardware-requirements"></a>Requisitos de runtime y hardware
Para ejecutar ASA en IoT Edge, se necesitan dispositivos que puedan ejecutar [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). 

ASA y Azure IoT Edge usan contenedores de **Docker** para proporcionar una solución portátil que se ejecuta en sistemas operativos que admiten múltiples hosts (Windows, Linux).

ASA en IoT Edge está disponible en forma de imágenes de Windows y Linux que se ejecutan en arquitecturas x86-64 o de Azure Resource Manager. 


### <a name="input-and-output"></a>Entrada y salida
#### <a name="input-and-output-streams"></a>Flujos de entrada y salida
Los trabajos de Edge de ASA pueden obtener entradas y salidas de otros módulos que se ejecutan en dispositivos de IoT Edge. Para conectarse a determinados módulos, y desde ellos, puede establecer la configuración de enrutamiento en el momento de la implementación. Se puede encontrar más información en [la documentación de composición del módulo de IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-composition).

En las entradas y salidas, se admiten los formatos CSV y JSON.

Por cada flujo de entrada y salida que se crea en un trabajo de ASA, se crea un punto de conexión correspondiente en el módulo implementado. Estos puntos de conexión se pueden utilizar en las rutas de la implementación.

En este momento, los únicos tipos de entrada y salida de flujo admitidos son Centro de Microsoft Edge. La entrada de referencia admite el tipo de archivo de referencia. Se puede llegar a otras salidas mediante un trabajo de nube de nivel inferior. Por ejemplo, un trabajo de Stream Analytics hospedado en Edge envía una salida al Centro de Microsoft Edge, el cual puede enviar luego una salida a IoT Hub. Puede usar un segundo trabajo de Azure Stream Analytics hospedado en la nube con entrada desde IoT Hub y salida a Power BI u otro tipo de salida.



##### <a name="reference-data"></a>Datos de referencia
Los datos de referencia (que también se conocen como tabla de búsqueda) son un conjunto finito de datos estáticos o que, por naturaleza, cambian lentamente. Se utilizan para realizar búsquedas o para ponerlos en correlación con su flujo de datos. Para usar los datos de referencia en el trabajo de Azure Stream Analytics, por lo general usará una [combinación de datos de referencia](https://msdn.microsoft.com/library/azure/dn949258.aspx) en la consulta. Para más información, consulte la [documentación de ASA acerca de los datos de referencia](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data).

Para usar datos de referencia para ASA en IoT Edge, siga estos pasos: 
1. Cree una nueva entrada para el trabajo
2. Elija **Datos de referencia** como **tipo de origen**.
3. Establezca la ruta de acceso del archivo. Esta debe ser una ruta de acceso **absoluta** en el dispositivo ![Creación de datos de referencia](media/stream-analytics-edge/ReferenceData.png)
4. Habilite **Unidades compartidas** en la configuración de Docker y asegúrese de que la unidad está habilitada antes de iniciar la implementación.

Para más información, consulte la [documentación de Docker para Windows](https://docs.docker.com/docker-for-windows/#shared-drives).

> [!Note]
> Por el momento solo se admiten datos de referencia locales.




## <a name="license-and-third-party-notices"></a>Licencia y avisos de terceros
* [Licencia de versión preliminar de Azure Stream Analytics en IoT Edge](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Aviso de terceros para la versión preliminar de Azure Stream Analytics en IoT Edge](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="get-help"></a>Obtención de ayuda
Para más ayuda, pruebe el [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [Tutorial de ASA en IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Enviar comentarios al equipo mediante esta encuesta](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
* [Desarrollo de trabajos para dispositivos perimetrales de Stream Analytics mediante herramientas de Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
