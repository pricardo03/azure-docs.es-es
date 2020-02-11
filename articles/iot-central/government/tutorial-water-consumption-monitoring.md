---
title: 'Tutorial: Creación de una aplicación de supervisión del consumo de agua con Azure IoT Central'
description: 'Tutorial: Aprenda a crear una aplicación de supervisión del consumo de agua mediante plantillas de aplicación de Azure IoT Central.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 1bad4ab6320e757ac766776a95b8dbe6ebaa3259
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77016432"
---
# <a name="tutorial-create-a-water-consumption-monitoring-application-in-iot-central"></a>Tutorial: Creación de una aplicación de supervisión del consumo de agua en IoT Central



Este tutorial le guía por los pasos para crear una aplicación de supervisión del consumo de agua de Azure IoT Central a partir de la plantilla de aplicación Supervisión del consumo de agua de IoT Central. 

En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Usar la plantilla **Supervisión del consumo de agua** de Azure IoT Central para crear la aplicación de supervisión del consumo de agua.
> * Explorar y personalizar el panel del operador 
> * Exploración de las plantillas de dispositivo
> * Explorar los dispositivos simulados
> * Exploración y configuración de reglas
> * Trabajos de configuración
> * Realizar la personalización de marca de la aplicación mediante etiquetado.

## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial, necesita:
-  Se recomienda una suscripción de Azure. Si no tiene una suscripción de Azure, puede crear una en la [página de suscripción a Azure](https://aka.ms/createazuresubscription).

## <a name="create-water-consumption-monitoring-app-in-iot-central"></a>Creación de una aplicación de supervisión del consumo de agua en IoT Central

En esta sección, usará la plantilla **Water consumption monitoring template** de Azure IoT Central para crear la aplicación de supervisión del consumo de agua en IoT Central.

Para crear una aplicación de supervisión del consumo de agua de Azure IoT Central, siga estos pasos:  

1. Vaya a la [página principal de Azure IoT Central](https://aka.ms/iotcentral).

    Si tiene una suscripción a Azure, inicie sesión con las credenciales que usa para acceder a ella; si no la tiene, inicie sesión con una cuenta de Microsoft:

    ![Incorporación de la cuenta de la organización](media/tutorial-waterconsumptionmonitoring/sign-in.png)

2. Haga clic en **Build** (Crear) en el panel izquierdo y seleccione la pestaña **Government** (Administración Pública). En la página Government (Administración Pública) se muestran varias plantillas de aplicación.

   ![Plantillas de creación de aplicaciones de la Administración Pública](./media/tutorial-waterconsumptionmonitoring/iotcentral-government-tab-overview1.png)

1. Seleccione la plantilla de aplicación **Water consumption monitoring**. Esta plantilla incluye una plantilla de dispositivo de consumo de agua de ejemplo, un dispositivo simulado, un panel de operador y reglas de supervisión preconfiguradas.    

2. Haga clic en **Create app** (Crear aplicación); se abrirá el formulario de creación **New application** (Nueva aplicación) con los siguientes campos:
    * **Application name** (Nombre de la aplicación): de forma predeterminada, la aplicación usa *Water consumption monitoring* seguido de una cadena de identificación única que genera IoT Central. Tiene la opción de elegir un nombre descriptivo para la aplicación. También puede cambiar el nombre de la aplicación más adelante.
    * **URL**: IoT Central genera automáticamente una dirección URL basada en el nombre de la aplicación. Puede optar por actualizar la dirección URL a su gusto. También puede cambiarla más adelante. 
    * Si tiene una suscripción de Azure, escriba el *directorio, la suscripción de Azure y la región*. Si no tiene una suscripción, puede activar **7-day free trial** (Evaluación gratuita de 7 días) y completar la información de contacto necesaria.  

    Para más información acerca de los directorios y las suscripciones, consulte la [guía de inicio rápido para crear una aplicación](../core/quick-deploy-iot-central.md).

5. En la parte inferior de la página, haga clic en **Create** (Crear). 

    ![Página de creación de una aplicación de Azure IoT Central](./media/tutorial-waterconsumptionmonitoring/new-application-waterconsumptionmonitoring.png)

    ![Página de información de facturación de la creación de una aplicación de Azure IoT Central](./media/tutorial-waterconsumptionmonitoring/new-application-waterconsumptionmonitoring-billinginfo.png)

6. Ahora ha creado una aplicación de supervisión del consumo de agua mediante la plantilla **Water consumption monitoring** de Azure IoT Central.

Felicidades. Acaba de terminar de crear la aplicación de supervisión del consumo de agua, que viene preconfigurada con los siguientes elementos:
* Paneles de operadores de ejemplo
* Plantillas de dispositivo predefinidas de ejemplo de caudal de agua y válvula
* Dispositivos simulados de caudal de agua y válvula inteligente
* Reglas y trabajos preconfigurados
* Ejemplo de personalización de marca mediante etiquetado 

Como es su aplicación, puede modificarla en cualquier momento. Ahora, explore la aplicación y realice algunas personalizaciones.  

## <a name="explore-and-customize-operator-dashboard"></a>Explorar y personalizar el panel del operador 
Después de crear la aplicación, llegará al panel del operador de ejemplo denominado **Wide World water consumption monitoring dashboard**.

   ![Panel de supervisión del consumo de agua](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboardfull.png)

Como desarrollador, puede crear y personalizar las vistas del panel para los operadores. Pero antes de probar la personalización, es interesante explorar el panel. 

> [!NOTE]
> Todos los datos que se muestran en el panel se basan en los datos del dispositivo simulado, que se explorarán en la sección siguiente. 
  
El panel consta de diferentes tipos de iconos:

* **Icono de imagen de Wide World Water Utility**: el primer icono del panel es un icono de imagen de un servicio de agua ficticio, "Wide World Water". Puede personalizar el icono y colocar su propia imagen o quitarlo. 

* **Icono de KPI de Average water flow**: el icono de KPI está configurado para mostrar como ejemplo *el caudal medio en los últimos 30 minutos*. Puede personalizar los iconos de KPI y establecerlos en un tipo y un intervalo de tiempo diferentes.

* Después, a la derecha del panel están los iconos de *comando de dispositivo***Close valve**, **Open valve** o **Set valve position**. Al hacer clic en los comandos, irá a la página de comando del dispositivo simulado. En IoT Central un *comando* es un tipo de *funcionalidad de dispositivo* que puede examinar más adelante en la **sección de plantillas de dispositivo** de este tutorial.

*  **Water distribution area map**: el mapa usa Azure Maps, que puede configurar directamente en Azure IoT Central. El icono de mapa muestra la ubicación del dispositivo. Intente mantener el puntero sobre el mapa y pruebe los controles: *acercar*, *alejar* o *ampliar*. 

    ![Mapa del panel de supervisión del consumo de agua](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboard-map.png)

* En el **gráfico de líneas Average water flow line** y el **gráfico de líneas Environment conditions**, puede visualizar una o varias telemetrías representadas como un gráfico de líneas durante un intervalo de tiempo deseado.  

* En el **gráfico de mapa térmico Average valve pressure**, puede elegir el tipo de visualización de mapa término de los datos de telemetría del dispositivo cuya distribución le interese examinar durante un intervalo de tiempo con un índice de color.

* **Icono de contenido Reset alert threshold**: puede incluir iconos de contenido de llamada a la acción que insertan el vínculo a una página de acción. En este caso, el icono le llevará a la opción **Jobs** (Trabajos) de la aplicación, donde puede ejecutar actualizaciones de las propiedades del dispositivo, que se examinarán más adelante en la sección **Configuración de trabajos** de este tutorial.

* **Iconos de propiedades**: el panel muestra **Valve operational info**, **Flow alert thresholds** y **Maintenance info**. 


### <a name="customize-dashboard"></a>Personalización del panel 

Como desarrollador, puede personalizar las vistas del panel para los operadores. Pruebe lo siguiente:
1. Haga clic en **Edit** (Editar) para personalizar **Wide World water consumption dashboard**. Para personalizar el panel, haga clic en el menú **Edit** (Editar). Cuando el panel esté en modo de **edición**, puede agregar nuevos iconos o configurarlo. 

     ![Edición del panel](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-edit-dashboard.png)

2. Haga clic en **+ New** (+ Nuevo) para crear un panel y configurarlo desde el principio. Puede tener varios paneles y navegar entre ellos desde el menú del panel. 

## <a name="explore-device-template"></a>Exploración de las plantillas de dispositivo
Una plantilla de dispositivo de Azure IoT Central define la funcionalidad de un dispositivo, que puede ser telemetría, propiedad o comando. Como desarrollador, puede definir una o varias plantillas de dispositivo en IoT Central que representen la funcionalidad de los dispositivos que se van a conectar. 
 

La aplicación **Supervisión del consumo de agua** incluye dos plantillas de dispositivo de referencia que representan un *medidor de caudal* y una *válvula inteligente*. 

Para ver la plantilla de dispositivo, siga estos pasos:

1. Haga clic en **Device templates** (Plantillas de dispositivo) en el panel de navegación izquierdo de la aplicación de IoT Central. 
    En la lista, verá dos plantillas de dispositivo **Flow meter** y **Smart Valve**.

   ![Plantilla de dispositivo](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate.png)

2. Haga clic en **Flow meter** y familiarícese con las funcionalidades del dispositivo. 

     ![Plantilla de dispositivo Flow meter](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-flowmeter.png)

### <a name="customizing-the-device-template"></a>Personalización de la plantilla de dispositivo

Pruebe a personalizar lo siguiente:
1. Vaya a **Customize** (Personalizar) en el menú de la plantilla de dispositivo.
2. Busque el tipo de telemetría `Temperature`.
3. Actualice el valor de **Display name** (Nombre para mostrar) de `Temperature` a `Reported temperature`.
4. Actualice la unidad de medida o establezca *Min value* (Valor mínimo) y *Max value* (Valor máximo).
5. Haga clic en **Save** (Guardar) para guardar los cambios. 

### <a name="add-a-cloud-property"></a>Adición de una propiedad de la nube 
1. Vaya a **Cloud property** (Propiedad de la nube) en el menú de la plantilla de dispositivo.
2. Haga clic en **+ Add Cloud Property** (+ Agregar propiedad de la nube). 
    En IoT Central, puede agregar una propiedad que sea pertinente para el dispositivo. Por ejemplo, una propiedad de la nube podría ser un umbral de alerta específico del área de instalación, información de recursos u otra información de mantenimiento. 
3. Haga clic en **Save** (Guardar) para guardar los cambios. 
 
### <a name="views"></a>Vistas 
La plantilla de dispositivo de supervisión del consumo de agua incluye vistas predefinidas. Explore las vistas y realice actualizaciones. Las vistas definen cómo verán los operadores los datos del dispositivo, pero también establecen las propiedades de la nube. 

  ![Vistas de la plantilla de dispositivo](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-views.png)

### <a name="publish"></a>Publicar 
Si realizó algún cambio, seleccione **Publicar** para publicar la plantilla de dispositivo. 

### <a name="create-a-new-device-template"></a>Creación de una nueva plantilla de dispositivo 
- Seleccione **+ New** (+ Nueva) para crear una plantilla de dispositivo y siga el proceso de creación. Podrá crear una plantilla de dispositivo personalizada desde el principio o elegir una del catálogo de dispositivos de Azure. 

## <a name="explore-simulated-devices"></a>Explorar los dispositivos simulados
En IoT Central, puede crear dispositivos simulados para probar la aplicación y la plantilla de dispositivo. La aplicación **Water consumption monitoring** tiene dos dispositivos simulados asignados a las plantillas de dispositivo *Flow meter* (Medidor de caudal) y *Smart Valve* (Válvula inteligente). 

### <a name="to-view-the-devices"></a>Para ver los dispositivos, siga estos pasos:
1. Vaya a **Devices** (Dispositivos) en el panel izquierdo de IoT Central. 

   ![Dispositivos](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devices.png)

2. Haga clic en uno de los elementos **Smart Valve 1** (Válvula inteligente 1). 

    ![Dispositivo 1](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitor-device1.png)

3.  En los **comandos del dispositivo**, puede ver los tres comandos *Open Valve*, *Close Valve* y *Set Valve position*, que son funcionalidades definidas en la plantilla de dispositivo *Smart Valve* (Válvula inteligente). 
4. Explore las pestañas **Device Properties** (Propiedades del dispositivo) **Device Dashboard** (Panel del dispositivo). 

> [!NOTE]
> Tenga en cuenta que todas las pestañas se han configurado a partir de las vistas de la plantilla de dispositivo.

### <a name="add-new-devices"></a>Adición de nuevos dispositivos
* Para agregar nuevos dispositivos, haga clic en **+ New** (+ Nuevo) en la pestaña **Devices** (Dispositivos). 

## <a name="explore-and-configure-rules"></a>Exploración y configuración de reglas

En Azure IoT Central puede crear reglas para supervisar automáticamente la telemetría de los dispositivos y desencadenar acciones cuando se cumplan una o varias condiciones. Las acciones pueden incluir el envío de notificaciones por correo electrónico, el desencadenamiento de una acción de Microsoft Flow o una acción de webhook para enviar datos a otros servicios.

La aplicación **Supervisión del consumo de agua** que ha creado tiene tres reglas preconfiguradas.

### <a name="to-view-rules"></a>Para ver las reglas, siga estos pasos:
1. Vaya a **Rules** (Reglas) en el panel de navegación izquierdo de IoT Central. 

   ![Reglas](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-rules.png)

2. Seleccione y haga clic en **High pH alert** que es una de las reglas preconfiguradas en la aplicación.

     ![High pH Alert](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-highflowalert.png)

    La regla `High flow alert` está configurada para compararse con la condición `Acidity (pH)` es `greater than``Max flow threshold`. El umbral de caudal máximo es una propiedad de la nube definida en la plantilla de dispositivo *Smart Valve* (Válvula inteligente). El valor de `Max flow threshold` se establece por instancia de dispositivo. 

Ahora, se creará una acción de correo electrónico.

Para agregar una acción a la regla:

1. Seleccione **+ Email** (+ Correo electrónico). 
1. Escriba *High pH alert* como valor descriptivo de **Display name** (Nombre para mostrar) de la acción.
    * Escriba la dirección de correo electrónico asociada a la cuenta de IoT Central en **To** (Para). 
1. Opcionalmente, escriba una nota para incluirla en el texto del correo electrónico.
1. Seleccione **Done** (Listo) para completar la acción.
1. Seleccione **Save** (Guardar) para guardar y activar la nueva regla. 

A los pocos minutos de que se cumpla la **condición** configurada, recibirá un correo electrónico.

> [!NOTE]
> La aplicación enviará un correo electrónico cada vez que se cumpla una condición. **Deshabilite** la regla para dejar de recibir correo electrónico de la regla automatizada. 
  
Para crear una regla, siga estos pasos: 
- En el panel de navegación izquierdo, seleccione **+New** (+ Nueva) en **Rules** (Reglas).

## <a name="configure-jobs"></a>Configuración de trabajos

En IoT Central, los trabajos permiten desencadenar actualizaciones de propiedades de la nube o del dispositivo en varios dispositivos. Además de las propiedades, los trabajos se pueden usar también para desencadenar comandos de dispositivo en varios dispositivos. IoT Central automatizará el flujo de trabajo. 

1. En el panel de navegación izquierdo, vaya a **Jobs** (Trabajos). 
2. Haga clic en **+New** (+ Nuevo) y configure uno o varios trabajos. 


## <a name="customize-your-application"></a>Personalización de la aplicación 
Como creador, puede cambiar varias opciones de configuración para personalizar la experiencia del usuario en la aplicación.

1.  Vaya a **Administration > Customize your application** (Administración > Personalizar la aplicación).
3. Use el botón **Change** (Cambiar) para elegir una imagen para cargar en el campo **Application logo** (Logotipo de la aplicación).
4. Use el botón **Change** (Cambiar) para elegir una imagen para **Browser icon** (Icono del explorador) que aparecerá en las pestañas del explorador.
5. También puede reemplazar los valores predeterminados de **Browser colors** (Colores del explorador) mediante la adición de códigos de color HTML hexadecimales.

   ![Personalización de la aplicación de Azure IoT Central](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-customize-your-application.png)

1.  También puede cambiar las imágenes de la aplicación. Para ello, vaya a **Administration > Application settings** (Administración > Configuración de la aplicación) y haga clic en el botón **Select image** (Seleccionar imagen) para elegir la imagen que se va a cargar como imagen de la aplicación. 
2. Por último, también puede cambiar el valor de **Theme** (Tema) haciendo clic en **Settings** (Configuración) en la cabecera de la aplicación. 

  
## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, puede eliminarla mediante estos pasos:

1. Abra la pestaña Administration (Administración) en el panel izquierdo de la aplicación de IoT Central. 
2. Seleccione la configuración de la aplicación y haga clic en el botón Eliminar situado en la parte inferior de la página. 


## <a name="next-steps"></a>Pasos siguientes

* Más información sobre los [conceptos de la supervisión del consumo de agua](./concepts-waterconsumptionmonitoring-architecture.md).
