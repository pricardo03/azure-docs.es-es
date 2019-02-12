---
title: Personalización de las vistas del operador en Azure IoT Central | Microsoft Docs
description: Como generador, personalice las vistas del operador en la aplicación de Azure IoT Central.
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/29/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 267a619fe32a8d4af0ee9cc8a5001d7a321c3098
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765161"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view-new-ui-design"></a>Tutorial: Personalización de la vista del operador en Azure IoT Central (nuevo diseño de la interfaz de usuario)

Este tutorial le enseña, como generador, a personalizar la vista del operador en la aplicación. Al realizar un cambio en la aplicación como generador, puede obtener una vista previa de la vista del operador en la aplicación Microsoft Azure IoT Central.

En este tutorial se personaliza la aplicación para mostrar la información pertinente sobre el dispositivo de aire acondicionado conectado a un operador. Las personalizaciones permiten al operador administrar los dispositivos de aire acondicionado conectados a la aplicación.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración del panel del dispositivo
> * Configuración del diseño de los valores de configuración del dispositivo
> * Configuración del diseño de las propiedades del dispositivo
> * Vista previa del dispositivo como operador
> * Configuración de la página principal predeterminada
> * Vista previa de la página principal predeterminada como operador

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar los dos tutoriales anteriores:

* [Define a new device type in your Azure IoT Central application](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central).
* [Configure rules and actions for your device](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) (Configuración de reglas y acciones para el dispositivo).

## <a name="configure-your-device-dashboard"></a>Configuración del panel del dispositivo

Como generador, puede definir qué información se muestra en el panel del dispositivo. En el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json), agregó un gráfico de líneas y otra información al panel **Connected Air Conditioner**.

1. Para editar la plantilla del dispositivo **Connected Air Conditioner**, elija **Device Templates** (Plantillas de dispositivo) en el menú de navegación izquierdo:

    ![Página Plantillas de dispositivo](media/tutorial-customize-operator-experimental/devicetemplates.png)

2. Para personalizar el panel del dispositivo, haga clic en el dispositivo de la plantilla del **Connected Air Conditioner-1.0.0** que ha creado en el tutorial [Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

3. Para editar el panel, seleccione la pestaña **Panel**.

4. Para agregar un icono de indicador clave de rendimiento (KPI) al panel, elija **KPI**:

    Para definir el indicador clave de rendimiento, use la información de la tabla siguiente:

    | Configuración     | Valor |
    | ----------- | ----- |
    | NOMBRE        | Temperatura máxima |
    | Intervalo de tiempo  | La semana pasada |
    | Tipo de medida | Telemetría |
    | Medición | temperatura |
    | Agregación | Máxima |
    | Visibilidad  | habilitado |

    ![Incorporación de indicador clave de rendimiento (KPI)](media/tutorial-customize-operator-experimental/addkpi.png)

5. Haga clic en **Save**(Guardar). Ahora verá el icono de indicador clave de rendimiento en el panel:

    ![Icono de indicador clave de rendimiento](media/tutorial-customize-operator-experimental/temperaturekpi.png)

6. Para mover o cambiar el tamaño de un icono en el panel, mueva el puntero del mouse sobre este. Puede arrastrar el icono a una nueva ubicación o cambiarle el tamaño.

## <a name="configure-your-settings-layout"></a>Configuración del diseño de los valores de configuración

Como generador, también puede configurar la vista de los valores de configuración del dispositivo para el operador. El operador utiliza la pestaña de configuración del dispositivo para configurarlo. Por ejemplo, los operadores usan la pestaña de configuración para establecer la temperatura objetivo del aire acondicionado conectado.

1. Para editar el diseño de la configuración del aire acondicionado conectado, elija la pestaña **Configuración**.

2. Puede mover y cambiar el tamaño de los iconos de configuración:

    ![Edición del diseño de configuración](media/tutorial-customize-operator-experimental/settingslayout.png)

## <a name="configure-your-properties-layout"></a>Configuración del diseño de las propiedades

Además del panel y la configuración, también puede configurar la vista de las propiedades del dispositivo para el operador. El operador utiliza la pestaña de propiedades de dispositivo para administrar los metadatos de este. Por ejemplo, los operadores usan esta pestaña para ver el número de serie de un dispositivo o actualizar los datos de contacto del fabricante.

1. Para editar el diseño de las propiedades del aire acondicionado conectado, elija la pestaña **Propiedades**.

2. Puede mover y cambiar el tamaño de los campos de propiedades:

    ![Edición del diseño de las propiedades](media/tutorial-customize-operator-experimental/propertieslayout.png)

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Vista previa del dispositivo de aire acondicionado conectado como operador

La página **Device Templates** (Plantillas de dispositivo) se usa para personalizar las pestañas Panel, Configuración y Propiedades de un operador. La página **Device Explorer** (Explorador de dispositivos) se usa para ver y usar la plantilla del dispositivo.

1. Para ver y usar la plantilla del aire acondicionado conectado como operador, vaya a la página **Device Explorer** (Explorador de dispositivos) y elija el dispositivo simulado que IoT Central haya generado a partir de la plantilla:

    ![Ver y usar la plantilla del dispositivo](media/tutorial-customize-operator-experimental/usetemplate.png)

2. Para actualizar la ubicación de este dispositivo, elija **Propiedades** y edite el valor en el icono Ubicación. A continuación, haga clic en **Guardar**:

    ![Edición de un valor de propiedad](media/tutorial-customize-operator-experimental/editproperty.png)

3. Para enviar un valor de configuración al aire acondicionado conectado, elija **Settings** (Configuración), cambie el valor de configuración en un icono y elija **Update** (Actualizar):

    ![Envío de un valor de configuración al dispositivo](media/tutorial-customize-operator-experimental/sendsetting.png)

    Cuando el dispositivo confirma el nuevo valor de configuración, este se muestra como **sincronizado** en el icono.

4. Como operador, puede ver el panel del dispositivo según la configuración del generador:

    ![Vista del panel del dispositivo para el operador](media/tutorial-customize-operator-experimental/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Configuración de la página principal predeterminada

Al iniciar sesión en una aplicación de Azure IoT Central, el generador o el operador ve una página principal. Como generador, puede configurar el contenido de esta página principal para que incluya el contenido más útil y pertinente para el operador.

1. Para personalizar la página principal predeterminada, vaya a la página **principal** y seleccione **Editar** en la parte superior derecha de la misma. Se desliza un panel desde la izquierda con una lista de objetos que puede agregar a la página **principal**:

    ![Página Generador de aplicaciones](media/tutorial-customize-operator-experimental/builderhome.png)

2. Para personalizar la página **principal**, agregue iconos desde la **biblioteca**. Elija **Link** (Vincular) y agregue los detalles del sitio web de la organización. A continuación, haga clic en **Guardar**:

    ![Incorporación de un vínculo a la página principal](media/tutorial-customize-operator-experimental/addlink.png)

    > [!NOTE]
    > También puede agregar vínculos a páginas dentro de la aplicación de Azure IoT Central. Por ejemplo, a un panel de dispositivo o a una página de configuración.

3. Si lo desea, elija **Image** (Imagen) y cargue una imagen para mostrarla en la página principal. La imagen puede tener una dirección URL a la que va al hacer clic en ella:

    ![Incorporación de una imagen a la página principal](media/tutorial-customize-operator-experimental/addimage.png)

    Para más información, consulte el artículo de [Preparación y carga de imágenes para Azure IoT Central](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="preview-the-default-home-page-as-an-operator"></a>Vista previa de la página principal predeterminada como operador

Para obtener una vista previa de la página principal como operador, haga clic en **Listo** en la parte superior derecha de la página:

![Activación y desactivación del modo de diseño](media/tutorial-customize-operator-experimental/operatorviewhome.png)

Puede hacer clic en los iconos de vínculo e imagen para ir a las direcciones URL que estableció como generador.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a personalizar la vista de la aplicación para el operador.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Configuración del panel del dispositivo
> * Configuración del diseño de los valores de configuración del dispositivo
> * Configuración del diseño de las propiedades del dispositivo
> * Vista previa del dispositivo como operador
> * Configuración de la página principal predeterminada
> * Vista previa de la página principal predeterminada como operador

Una vez que ha aprendido cómo personalizar la vista del operador de la aplicación, los pasos siguientes sugeridos son:

* [Supervisión de los dispositivos (como operador)](tutorial-monitor-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Incorporación de un dispositivo nuevo a la aplicación (como operador y como desarrollador de aplicaciones)](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
