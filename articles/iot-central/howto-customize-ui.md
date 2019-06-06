---
title: Personalizar la interfaz de usuario de Azure IoT Central | Microsoft Docs
description: Cómo personalizar los vínculos de ayuda y el tema de la aplicación central de IoT de Azure
author: dominicbetts
ms.author: dobett
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 4d385f1e8c883453b4153ca4c9119d3be0a608bb
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495557"
---
# <a name="customize-the-azure-iot-central-ui"></a>Personalizar la interfaz de usuario de Azure IoT Central 

*En este artículo se aplica a los administradores.*

IoT Central le permite personalizar la interfaz de usuario de la aplicación aplicando temas personalizados y modificar los vínculos de ayuda para que apunte a sus propios recursos de ayuda personalizado. Captura de pantalla siguiente muestra una página con el tema estándar:

![Tema estándar de IoT Central](./media/howto-customize-ui/standard-ui.png)

Captura de pantalla siguiente muestra una página con una captura de pantalla personalizado con los elementos de interfaz de usuario personalizados resaltados:

![Tema Central personalizado de IoT](./media/howto-customize-ui/themed-ui.png)

## <a name="create-theme"></a>Crear tema

Para crear un tema personalizado, navegue hasta la **personalizar su aplicación** página en el **administración** sección:

![Temas de IoT Central](./media/howto-customize-ui/themes.png)

En esta página, puede personalizar los siguientes aspectos de la aplicación:

### <a name="application-logo"></a>Logotipo de la aplicación

Una imagen PNG, no puede superar 1 MB, con un fondo transparente. Este logotipo se muestra a la izquierda en la barra de título de la aplicación de IoT Central.

Si la imagen del logotipo incluye el nombre de la aplicación, puede ocultar el texto de nombre de la aplicación. Para obtener más información, consulte [Administrar configuración de la aplicación](./howto-administer.md#manage-application-settings).

### <a name="browser-icon-favicon"></a>Icono del explorador (favicon)

Una imagen PNG, no más de 32 x 32 píxeles, con un fondo transparente. Un explorador web puede usar esta imagen en la barra de direcciones, historial, marcadores y pestaña del explorador.

### <a name="browser-colors"></a>Colores del explorador

Puede cambiar el color del encabezado de página y el color usado para los acentos botones y otros aspectos destacados. Utilice un valor de color hexadecimal de seis caracteres con el formato `##ff6347`. Para obtener más información acerca de **valor HEXADECIMAL** notación de color, vea [colores HTML](https://www.w3schools.com/html/html_colors.asp).

> [!NOTE]
> Siempre puede revertir a las opciones predeterminadas en el **personalizar su aplicación** página.

### <a name="changes-for-operators"></a>Cambios para los operadores

Si un administrador crea un tema personalizado, operadores y otros usuarios de la aplicación ya no pueden elegir un tema en **configuración**.

## <a name="replace-help-links"></a>Reemplace los vínculos de ayuda

Para proporcionar información de ayuda personalizada para sus operadores y otros usuarios, puede modificar los vínculos de la aplicación **ayuda** menú.

Para modificar los vínculos de ayuda, navegue hasta la **personalizar Ayuda** página en el **administración** sección:

![Personalizar los vínculos de Ayuda de IoT Central](./media/howto-customize-ui/help-links.png)

También puede agregar nuevas entradas en el menú Ayuda y quitar entradas predeterminadas:

![Ayuda de IoT Central personalizada](./media/howto-customize-ui/custom-help.png)

> [!NOTE]
> Siempre puede revertir a los vínculos de Ayuda de forma predeterminada en el **personalizar Ayuda** página.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a personalizar la interfaz de usuario en la aplicación IoT Central, estos son algunos pasos sugeridos:

- [Administrar su aplicación](./howto-administer.md)
- [Configurar el panel de la aplicación](./howto-configure-homepage.md)