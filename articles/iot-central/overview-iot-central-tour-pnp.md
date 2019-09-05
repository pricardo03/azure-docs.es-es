---
title: Un paseo por la interfaz de usuario de Azure IoT Central | Microsoft Docs
description: Como compilador, familiarícese con las áreas principales de la interfaz de usuario de Azure IoT Central, que se utiliza para crear una solución de IoT.
author: dominicbetts
ms.author: dobett
ms.date: 07/05/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 4a0c9d16474ddf032ff88382bc240713bc734ff8
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211887"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-preview-features"></a>Paseo por la interfaz de usuario de Azure IoT Central (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este artículo presenta la interfaz de usuario de Microsoft Azure IoT Central. Puede usar la interfaz de usuario para crear, administrar y usar una solución de Azure IoT Central y sus dispositivos conectados.

Como _compilador_, usará la interfaz de usuario de Azure IoT Central para definir la solución de Azure IoT Central. Puede usar la interfaz de usuario para:

* Definir los tipos de dispositivos que se conectan a la solución.
* Configurar las reglas y las acciones para sus dispositivos.
* Personalizar la interfaz de usuario para un _operador_ que utiliza la solución.

Como _operador_, usará la interfaz de usuario de Azure IoT Central para administrar la solución de Azure IoT Central. Puede usar la interfaz de usuario para:

* Supervisar los dispositivos.
* Configurar los dispositivos.
* Solucionar los problemas y errores de los dispositivos.
* Aprovisionar dispositivos nuevos.

## <a name="use-the-left-navigation-menu"></a>Uso del menú de navegación izquierdo

Use el menú de navegación izquierdo para acceder a las diferentes áreas de la aplicación. Puede expandir o contraer la barra de navegación mediante la selección de **<** o **>** :

:::row:::
  :::column span="":::
      ![Left navigation menu](media/overview-iot-central-tour-pnp/navigationbar.png)
  :::column-end:::
  :::column span="2":::

      **Dashboard** displays your application dashboard. As a builder, you can customize the dashboard for your operators. Users can also create their own  dashboards.
    
      **Devices** lists the simulated and real devices associated with each device template in the application. As an operator, you use the **Device Explorer** to manage your connected devices.
    
      **Device groups** lets you view and create device groups. As an operator, you can create device groups as a logical collections of devices specified by a query.

      **Rules** lets you edit rules that fire based on device telemetry and trigger customizable actions.
    
      **Analytics** shows analytics derived from device telemetry for devices and device groups. As an operator, you can create custom views on top of device data to derive insights from your application.
    
      **Jobs** enables bulk device management by having you create and run jobs to update your devices at scale.
    
      **Device templates** shows the tools a builder uses to create and manage device templates.
    
      **Data export** enables an administrator to configure a continuous export to other Azure services such as storage and queues.
    
      **Administration** shows the application administration pages where an administrator can manage application settings, users, and roles.
   :::column-end:::
:::row-end:::

## <a name="search-help-and-support"></a>Búsqueda, ayuda y soporte técnico

El menú superior aparece en todas las páginas:

![Barra de herramientas](media/overview-iot-central-tour-pnp/toolbar.png)

* Para buscar plantillas de dispositivos y dispositivos, escriba un valor **Buscar**.
* Para cambiar el idioma o el tema de la interfaz de usuario, elija el icono **Configuración**.
* Para cerrar la sesión de la aplicación, elija el icono **Cuenta**.
* Para obtener ayuda y soporte técnico, elija el menú desplegable **Ayuda** para obtener una lista de recursos. En una aplicación de prueba, los recursos de soporte técnico incluyen acceso al [chat en directo](howto-show-hide-chat.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

Puede elegir entre un tema claro o un tema oscuro para la interfaz de usuario:

![Elija un tema para la interfaz de usuario](media/overview-iot-central-tour-pnp/themes.png)

> [!NOTE]
> La opción de elegir entre temas claros y oscuros no está disponible si su administrador ha configurado un tema personalizado para la aplicación.

## <a name="dashboard"></a>panel

![panel](media/overview-iot-central-tour-pnp/homepage.png)

* El panel es la primera página que verá cuando inicie sesión en la aplicación de Azure IoT Central. Como generador, puede personalizar el panel de la aplicación para otros usuarios mediante la adición de iconos. Para más información, consulte el tutorial [Configuración de una plantilla de dispositivo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

* Como operador, puede crear paneles personalizados y cambiar entre ellos y el panel predeterminado. Para más información, consulte el artículo de procedimientos [Crear y administrar paneles personales](howto-personalize-dashboard.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="devices"></a>Dispositivos

![Página de dispositivos](media/overview-iot-central-tour-pnp/explorer.png)

La página del explorador muestra los _dispositivos_ que hay en la aplicación de Azure IoT Central agrupados por _plantilla del dispositivo_.

* Una plantilla de dispositivo define un tipo de dispositivo que se puede conectar a la aplicación. Para más información, consulte [Define a new device type in your Azure IoT Central application](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central).
* Un dispositivo representa un dispositivo real o simulado en la aplicación. Para más información, consulte [Add a real device to your Azure IoT Central application](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Adición de un nuevo dispositivo a la aplicación de Azure IoT Central).

## <a name="device-groups"></a>Grupos de dispositivos

![Página Device groups (Grupos de dispositivos)](media/overview-iot-central-tour-pnp/devicesets.png)

La página _device groups_ (Grupos de dispositivos) muestra los conjuntos de dispositivos que ha creado el generador. Un grupo de dispositivos es una colección de dispositivos relacionados. Un generador define una consulta para identificar los dispositivos que se incluyen en un grupo de dispositivos. Los grupos de dispositivos se utilizan para personalizar el análisis en la aplicación. Para más información, consulte el artículo [Uso de grupos de dispositivos en la aplicación de Azure IoT Central](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="rules"></a>Reglas

![Página de Reglas](media/overview-iot-central-tour-pnp/rules.png)

La página de reglas permite definir reglas en función de la telemetría, el estado del dispositivo o los eventos del dispositivo. Cuando se activa una regla, puede desencadenar cualquier acción, como el envío de un correo a un proveedor. El generador usa esta página para crear y administrar reglas. Para más información, consulte el tutorial [Configuración de reglas y acciones para sus dispositivos en Azure IoT Central](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="analytics"></a>Análisis

![Página Analytics](media/overview-iot-central-tour-pnp/analytics.png)

La página de análisis muestra gráficos que le ayudarán a entender cómo se comportan los dispositivos conectados a la aplicación. Un operador usará esta página para supervisar e investigar los problemas de los dispositivos conectados. El compilador puede definir los gráficos que se muestran en esta página. Para más información, consulte el artículo [Create custom analytics for your Azure IoT Central application](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Creación de análisis personalizados para la aplicación de Azure IoT Central).

## <a name="jobs"></a>Trabajos

![Página de trabajos](media/overview-iot-central-tour-pnp/jobs.png)

La página de trabajos le permite realizar operaciones de administración de dispositivos en masa. El compilador usa esta página para actualizar los comandos, la configuración y las propiedades del dispositivo. Para obtener más información, consulte el artículo [Run a job](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Ejecución de un trabajo).

## <a name="device-templates"></a>Plantillas de dispositivo

![Página Device templates (Plantillas de dispositivos)](media/overview-iot-central-tour-pnp/templates.png)

La página de plantillas de dispositivos es donde un generador crea y administra las plantillas de dispositivo en la aplicación. Una plantilla de dispositivo especifica las características del dispositivo, por ejemplo:

* Medidas de telemetría, estado y eventos
* Propiedades.
* Comandos

El generador también puede crear formularios y paneles para que los operadores los usen para administrar dispositivos.

Para más información, consulte el tutorial [Define a new device type in your Azure IoT Central application](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Definición de un nuevo tipo de dispositivo en la aplicación de Azure IoT Central).

## <a name="data-export"></a>Exportación de datos

![Página Data export (Exportación de datos)](media/overview-iot-central-tour-pnp/export.png)

La página de exportación de datos es el lugar en que los administradores definen cómo transmitir datos en secuencias, como la telemetría, desde la aplicación. Otros servicios pueden almacenar los datos exportados o utilizarlos para el análisis. Para más información, consulte el artículo [Exportación de datos a Azure IoT Central](howto-export-data.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="administration"></a>Administración

![Página Administration](media/overview-iot-central-tour-pnp/administration.png)

La página de administración contiene vínculos a las herramientas que usa un administrador, como definir los usuarios y roles de la aplicación y personalizar la interfaz de usuario. Para más información, consulte el artículo [Administer your Azure IoT Central application](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Administración de la aplicación de Azure IoT Central).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya tiene información general sobre Azure IoT Central y está familiarizado con el diseño de la interfaz de usuario, el siguiente paso sugerido es completar la guía de inicio rápido [Create an Azure IoT Central application](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Creación de una aplicación de Azure IoT Central).
