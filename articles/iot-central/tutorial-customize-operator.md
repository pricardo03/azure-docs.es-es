---
title: Personalización de las vistas del operador en Azure IoT Central | Microsoft Docs
description: Como generador, personalice las vistas del operador en la aplicación de Azure IoT Central.
author: sandeeppujar
ms.author: sandeepu
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 070a6ee4bc712b2dbec598cdb9be5c324895c033
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154988"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view"></a>Tutorial: Personalización de la vista del operador en Azure IoT Central

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

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, debe completar los dos tutoriales anteriores:

* [Define a new device type in your Azure IoT Central application](tutorial-define-device-type.md) (Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central).
* [Configure rules and actions for your device](tutorial-configure-rules.md) (Configuración de reglas y acciones para el dispositivo).

## <a name="configure-your-device-dashboard"></a>Configuración del panel del dispositivo

Como generador, puede definir qué información se muestra en el panel del dispositivo. En el tutorial sobre la [definición de un tipo nuevo de dispositivo en la aplicación](tutorial-define-device-type.md) se ha agregado un gráfico de líneas y otra información al panel **Connected Air Conditioner-1**.

1. Para editar la plantilla de dispositivo **Connected Air Conditioner**, elija **Explorer** (Explorador) en el menú de navegación izquierdo:

    ![Página Explorer](media/tutorial-customize-operator/explorer.png)

2. Para empezar a personalizar el panel del dispositivo de aire acondicionado conectado, seleccione la plantilla de dispositivo **Connected Aire Conditioner (1.0.0)**. Elija el dispositivo **Connected Air Conditioner-1** dispositivo creado en el tutorial de [definición de un tipo nuevo de dispositivo en la aplicación](tutorial-define-device-type.md):

    ![Selección del dispositivo de aire acondicionado conectado](media/tutorial-customize-operator/selectdevice.png)

    Cuando se encuentra dentro de un dispositivo, como **Connected Air Conditioner-1**, puede seleccionar **Editar plantilla** para realizar un cambio en la plantilla subyacente. Para más información, consulte [Create a new device template version](howto-version-devicetemplate.md) (Creación de una versión de plantilla nueva del dispositivo).

3. Para editar un panel, elija **Panel** y seleccione **Editar plantilla**:

    ![Página del panel de la plantilla de dispositivo](media/tutorial-customize-operator/dashboard.png)

4. Para agregar un icono de indicador clave de rendimiento (KPI) al panel, elija **KPI**:

    ![Incorporación de indicador clave de rendimiento (KPI)](media/tutorial-customize-operator/addkpi.png)

    Para definir el indicador clave de rendimiento, use la información de la tabla siguiente:

    | Configuración     | Valor |
    | ----------- | ----- |
    | NOMBRE        | Temperatura máxima |
    | Medición | temperatura |
    | Agregación | Máxima |
    | Intervalo de tiempo  | La semana pasada |

5. Elija **Guardar**. Ahora verá el icono de indicador clave de rendimiento en el panel:

    ![Icono de indicador clave de rendimiento](media/tutorial-customize-operator/temperaturekpi.png)

6. Para mover o cambiar el tamaño de un icono en el panel, mueva el puntero del mouse sobre este. Puede arrastrar el icono a una nueva ubicación o cambiarle el tamaño:

    ![Edición del diseño del panel](media/tutorial-customize-operator/dashboardlayout.png)

7. Haga clic en **Listo** cuando haya terminado de realizar cambios.

## <a name="configure-your-settings-layout"></a>Configuración del diseño de los valores de configuración

Como generador, también puede configurar la vista de los valores de configuración del dispositivo para el operador. El operador utiliza la página de los valores de configuración del dispositivo para configurarlo. Por ejemplo, para establecer el objetivo de temperatura de la máquina de aire acondicionado.

1. Para editar el diseño de los valores de configuración del aire acondicionado conectado, elija **Configuración** y seleccione **Editar plantilla**:

    ![Página Configuración](media/tutorial-customize-operator/settings.png)

2. Puede mover y cambiar el tamaño de los iconos de configuración:

    ![Edición del diseño de configuración](media/tutorial-customize-operator/settingslayout.png)

3. Haga clic en **Listo** cuando haya terminado de realizar cambios.

> [!NOTE]
> En el modo **Editar plantilla**, no se pueden editar los valores de configuración.

## <a name="configure-your-properties-layout"></a>Configuración del diseño de las propiedades

Además del panel y la configuración, también puede configurar la vista de las propiedades del dispositivo para el operador. El operador utiliza la página de propiedades de dispositivo para administrar los metadatos de este. Por ejemplo, para ver el número de serie de un dispositivo o actualizar los datos de contacto del fabricante.

1. Para editar el diseño de propiedades de su aire acondicionado conectado, elija **Propiedades** y seleccione **Editar plantilla**:

    ![Página de propiedades](media/tutorial-customize-operator/properties.png)

2. Puede mover y cambiar el tamaño de los campos de propiedades:

    ![Edición del diseño de las propiedades](media/tutorial-customize-operator/propertieslayout.png)

3. Haga clic en **Listo** cuando haya terminado de realizar cambios.

> [!NOTE]
> En el modo **Editar plantilla**, no se pueden editar los valores de las propiedades.

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Vista previa del dispositivo de aire acondicionado conectado como operador

En **Modo de diseño**, puede personalizar el panel, la configuración y las páginas de propiedades para el operador. Si no está en el modo **Editar plantilla**, puede ver la aplicación como un operador.

1. Para ver la máquina de aire acondicionado conectada como operador, haga clic en **Listo** para detener la edición de la plantilla y volver a la vista del operador del dispositivo.

2. Para actualizar la ubicación de este dispositivo, edite el valor en el icono Ubicación y elija **Guardar**:

    ![Edición de un valor de propiedad](media/tutorial-customize-operator/editproperty.png)

3. Para enviar un valor de configuración al aire acondicionado conectado, elija **Settings** (Configuración), cambie el valor de configuración en un icono y elija **Update** (Actualizar):

    ![Envío de un valor de configuración al dispositivo](media/tutorial-customize-operator/sendsetting.png)

    Cuando el dispositivo confirma el nuevo valor de configuración, este se muestra como **sincronizado** en el icono.

4. Como operador, puede ver el panel del dispositivo según la configuración del generador:

    ![Vista del panel del dispositivo para el operador](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Configuración de la página principal predeterminada

Al iniciar sesión en una aplicación de Azure IoT Central, el generador o el operador ve una página principal. Como generador, puede configurar el contenido de esta página principal para que incluya el contenido más útil y pertinente para el operador.

1. Para personalizar la página principal predeterminada, vaya a la página **principal** y seleccione **Editar** en la parte superior derecha de la página. Al seleccionar **Editar**, un panel se deslizará hacia fuera desde la derecha con una lista de objetos que puede agregar a la página principal.

    ![Página Application Builder](media/tutorial-customize-operator/builderhome.png)

2. Para personalizar la página principal, agregue iconos desde la **biblioteca**. Elija **Link** (Vincular) y agregue los detalles del sitio web de la organización. Después, elija **Save** (Guardar):

    ![Incorporación de un vínculo a la página principal](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > También puede agregar vínculos a páginas dentro de la aplicación de Azure IoT Central. Por ejemplo, a un panel de dispositivo o a una página de configuración.

3. Si lo desea, elija **Image** (Imagen) y cargue una imagen para mostrarla en la página principal. La imagen puede tener una dirección URL a la que va al hacer clic en ella:

    ![Incorporación de una imagen a la página principal](media/tutorial-customize-operator/addimage.png)

    Para más información, consulte el artículo de [Preparación y carga de imágenes para Azure IoT Central](howto-prepare-images.md).

## <a name="preview-the-default-home-page-as-an-operator"></a>Vista previa de la página principal predeterminada como operador

Para obtener una vista previa de la página principal como operador y no editar más, seleccione **Listo** en la parte superior derecha de la página.

![Activación y desactivación del modo de diseño](media/tutorial-customize-operator/operatorviewhome.png)

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

* [Supervisión de los dispositivos (como operador)](tutorial-monitor-devices.md)
* [Incorporación de un dispositivo nuevo a la aplicación (como operador y como desarrollador de aplicaciones)](tutorial-add-device.md)
