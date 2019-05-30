---
title: Introducción a Azure Portal | Microsoft Docs
description: Obtenga información sobre cómo navegar por el portal de Azure y usarlo para administrar servicios
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 05/24/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: de04b461d8e2d2dce9a88052a86fbe5d163fafec
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244196"
---
# <a name="azure-portal-overview"></a>Introducción a Azure Portal

En este artículo presenta Azure portal, identifica los elementos de la página del portal y le ayuda a familiarizarse con la experiencia de administración de portal de Azure.

## <a name="what-is-the-azure-portal"></a>¿Qué es el portal de Azure?

El portal de Azure es una consola unificada basada en web que proporciona una alternativa a las herramientas de línea de comandos. Con Azure portal, puede administrar su suscripción de Azure mediante una interfaz gráfica de usuario. Puede crear, administrar y supervisar todo, desde aplicaciones web sencillas hasta implementaciones complejas en la nube, crear paneles personalizados para una vista organizada de los recursos y configurar las opciones de accesibilidad para una mejor experiencia.

El portal de Azure está diseñado para la resistencia y disponibilidad continua. Tiene una presencia en cada centro de datos de Azure por lo que resulta resistente a errores del centro de datos individual y también evita ralentizaciones de la red al que se está cerca de los usuarios. El portal de Azure se actualiza continuamente y no requiere ningún tiempo de inactividad para las actividades de mantenimiento.

## <a name="azure-home"></a>Azure Home

Como un suscriptor nuevo a servicios de Azure, lo primero que verá después [inicie sesión en el portal](https://portal.azure.com) es **principal de Azure**. Este compila página recursos que le ayudarán a sacar el máximo partido de su suscripción de Azure. Hemos incluido vínculos para liberar los cursos en línea, documentación, servicios centrales y sitios útiles para mantenerse actuales y la administración cambiar para su organización. Para que un acceso rápido y fácil al trabajo en curso, también se muestra una lista de los recursos visitados más recientemente. No se puede personalizar esta página, pero puede elegir si ve **principal de Azure** o **panel Azure** como vista predeterminada. La primera vez que inicie sesión, hay un símbolo del sistema en la parte superior de la página donde puede guardar sus preferencias.

![Selector de vistas de captura de pantalla que muestra de forma predeterminada](./media/azure-portal-overview/azure-portal-default-view.png)

## <a name="azure-dashboard"></a>Panel de Azure

Los paneles proporcionan una vista detallada de los recursos de la suscripción que importa más a usted. Le hemos dado un panel predeterminado para que pueda comenzar. Puede personalizar este panel para poner los recursos que usa más en una sola vista. Los cambios que realice a la vista predeterminada afectan a su experiencia solo. Sin embargo, puede crear paneles adicionales para su propio uso o publicar sus paneles personalizados y compartirlos con otros usuarios de su organización. Para obtener más información, consulte [creación y uso compartido de paneles en Azure portal](../azure-portal/azure-portal-dashboards.md).

## <a name="getting-around-the-portal"></a>Moverse por el portal

Resulta útil para comprender el diseño básico de portal y cómo interactuar con él. En este caso, presentaremos los componentes de la interfaz de usuario y la terminología que se usa para dar instrucciones. Para realizar un paseo más detallado del portal, consulte la lección curso [vaya al portal](https://docs.microsoft.com/learn/modules/tour-azure-portal/3-navigate-the-portal).

La barra lateral de portal de Azure y el encabezado de página son los elementos globales que siempre están presentes. Estas características persistentes son el "shell" para la interfaz de usuario asociado con cada servicio individual o característica y el encabezado proporciona acceso a controles globales. La página de configuración (a veces se denomina una "hoja") para un recurso también puede tener un panel izquierdo para ayudarle a moverse entre las características.

La siguiente ilustración las etiquetas de los elementos básicos de Azure portal, cada uno de los cuales se describe en la tabla siguiente.

![Captura de pantalla muestra la vista del portal de pantalla completa y una clave para los elementos de interfaz de usuario](./media/azure-portal-overview/azure-portal-fullscreen-map.png)

|Clave|DESCRIPCIÓN
|:---:|---|
|1|Encabezado de página. Aparece en la parte superior de cada página del portal y contiene los elementos globales.|
|2| Búsqueda global. Utilice la barra de búsqueda para buscar rápidamente un recurso específico, un servicio o la documentación.|
|3|Controles globales. Al igual que todos los elementos globales, estas características se conservan entre el portal e incluyen: Cloud Shell, filtro de suscripción, notificaciones, configuración del portal, ayuda y soporte técnico y envíenos sus comentarios.|
|4|Su cuenta. Ver información sobre su cuenta, cambie los directorios, cierre la sesión o iniciar sesión con una cuenta diferente.|
|5|Barra lateral. La barra lateral es un elemento global que le ayuda a navegar entre los servicios. La barra lateral se puede contraer para dar el foco más al panel de trabajo.|
|6|Control principal para crear un nuevo recurso de la suscripción actual. Buscar o examinar Azure Marketplace para el tipo de recurso que desea crear.|
|7|La lista de favoritos. Agregar o quitar de favoritos desde el **todos los servicios** página.|
|8|Panel izquierdo. Muchos servicios incluyen un menú del panel izquierdo para ayudarle a administrar el servicio.|
|9|Barra de comandos. Los controles de la barra de comandos sean contextuales a su enfoque actual.|
|10|Ruta de navegación. Puede usar los vínculos de ruta de navegación para devolver un nivel en el flujo de trabajo.|
|11|Panel de trabajo.  Muestra los detalles sobre el recurso que tiene el foco.|

## <a name="get-started-with-services"></a>Introducción a servicios

Si es un suscriptor nuevo, tendrá que crear un recurso antes de que hay algo para administrar. Seleccione **+ crear un recurso** para ver los servicios disponibles en Azure Marketplace. Encontrará aplicaciones y servicios en cientos de proveedores en este caso, todos los certificados para ejecutarse en Azure.

Se ha rellenado previamente sus favoritos en la barra lateral con vínculos a los servicios usados con frecuencia.  Para ver todos los servicios disponibles, seleccione **todos los servicios** en la barra lateral.

> [!TIP]
> La forma más rápida para encontrar un recurso, el servicio o la documentación es usar *búsqueda* en el encabezado global. Use los vínculos de la ruta de navegación para volver a las páginas anteriores.
>

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre dónde se ejecuta el portal de Azure [dispositivos y exploradores compatibles](../azure-portal/azure-portal-supported-browsers-devices.md)

* Permanezca conectado sobre la marcha con [aplicación móvil de Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
