---
title: Capacidades de búsqueda con blocs de notas de versión preliminar de Azure Sentinel | Microsoft Docs
description: Este artículo describe cómo usar cuadernos con las capacidades de búsqueda Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: ae9d52e4a26825e4318a6afb8aadc86ac29fa2b3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677843"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Uso de cuadernos de Jupyter buscando amenazas de seguridad

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La base de Centinela de Azure es el almacén de datos; combina consultas, el esquema dinámico y se adapta para volúmenes masivos de datos de alto rendimiento. El portal de Azure Sentinel y todas las herramientas de Azure Sentinel usar una API común para tener acceso a este almacén de datos. La misma API también está disponible para herramientas externas como [Jupyter](https://jupyter.org/) blocs de notas y Python. Mientras que muchas tareas comunes pueden llevarse a cabo en el portal, Jupyter amplía el alcance de lo que puede hacer con estos datos. Combina la programación completa con una enorme colección de bibliotecas para el análisis de datos, visualización y aprendizaje de máquina. Estos atributos hacen que sea Jupyter una herramienta atractiva para la investigación de seguridad y búsqueda.

![Cuaderno de ejemplo](./media/notebooks/sentinel-nb-mapandtimeline.png)

Hemos integrado la experiencia de Jupyter en el portal de Azure Sentinel, facilitando el proceso para crear y ejecutar blocs de notas para analizar los datos. El *Kqlmagic* biblioteca proporciona el elemento aglutinador que permite realizar consultas desde Azure Sentinel y ejecutarlos directamente dentro de un bloc de notas. Las consultas usan el [lenguaje de consulta Kusto](https://kusto.azurewebsites.net/docs/query/index.html). Varios blocs de notas, desarrollados por algunos de los analistas de seguridad de Microsoft, se empaquetan con Centinela de Azure. Algunos de estos cuadernos se generan para un escenario concreto y se puede usar como-es. Como ejemplos de otros usuarios están diseñados para ilustrar técnicas y características que puede adaptarse para su uso en sus propios cuadernos o copiar. También pueden importarse otros blocs de notas de la Comunidad Azure Sentinel GitHub.

La experiencia de Jupyter integrada usa [Azure Notebooks](https://notebooks.azure.com/) para almacenar, compartir y ejecutar blocs de notas. También puede ejecutar estos cuadernos localmente (si tiene un entorno de Python y Jupyter en el equipo) o en otros entornos JupterHub como Azure Databricks.

Los equipos portátiles tienen dos componentes:

- la interfaz basada en explorador donde escribir y ejecutar consultas y el código, y donde se muestran los resultados de la ejecución.
- un *kernel* que es responsable de analizar y ejecutar el propio código. 

En Azure Notebooks, este kernel se ejecuta en Azure *libre de Cloud Computing y almacenamiento* de forma predeterminada. Si los blocs de notas incluyen modelos de aprendizaje automático complejos o visualizaciones debe considerar el uso más eficaz y dedicado recursos de proceso como [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM). Blocs de notas en su cuenta se mantienen privadas, a menos que elija compartirlos.

Los cuadernos de Azure Sentinel usan muchas bibliotecas populares de Python, como pandas, matplotlib, bokeh y otros usuarios. Hay un gran número de otros paquetes de Python para que pueda elegir, que cubre áreas como:

- gráficos y visualizaciones
- procesamiento de datos y análisis
- las estadísticas y la informática numérica
- aprendizaje automático y aprendizaje profundo

También hemos lanzado algunas herramientas de seguridad de Jupyter de código abierto en un paquete denominado [msticpy](https://github.com/Microsoft/msticpy/). Este paquete se utiliza en muchos de los portátiles incluyen. Msticpy herramientas están diseñadas específicamente para ayudar a crear para buscar blocs de notas e investigación y estamos trabajando activamente en las nuevas características y mejoras.

Se incluyen los cuadernos iniciales:

- **Guiado investigación - proceso alertas**: Le permite evaluar rápidamente las alertas mediante el análisis de actividad en los hosts afectados.
- **Guiado caza - Explorador de Windows host**: Permite explorar la actividad de la cuenta, las ejecuciones de proceso, actividad de red y otros eventos en un host.  
- **Guiado caza - exploración de Office 365**: Búsqueda de actividad sospechosa de Office 365 en varios conjuntos de datos de Office 365.

El [repositorio de GitHub de Azure Sentinel Comunidad](https://github.com/Azure/Azure-Sentinel) es la ubicación de los blocs de notas futuras Sentinel Azure creada por Microsoft o proporcionada por la Comunidad.

## <a name="run-a-notebook"></a>Ejecutar un bloc de notas

En el ejemplo siguiente, creamos un proyecto de Azure Notebooks desde el portal de Azure Sentinel, rellenar el proyecto con blocs de notas. Antes de usar estos blocs de notas, es una buena idea hacer una copia del Bloc de notas y trabajar en la copia. Trabajar con copias le permite actualizar de manera segura a versiones futuras de blocs de notas sin sobrescribir cualquiera de los datos.

1. En el portal de Azure Sentinel, haga clic en **blocs de notas** en el menú de navegación. Para crear un nuevo proyecto de Azure Notebooks, haga clic en **clon Azure Sentinel Notebooks** o haga clic en los proyectos abrir los blocs de notas existentes **vaya a los blocs de notas**.
  
   ![Seleccione los blocs de notas](./media/notebooks/sentinel-az-notebooks-home.png)

2. Si eligió **clon Azure Sentinel Notebooks** en el paso anterior, aparecerá el cuadro de diálogo siguiente. Haga clic en **importación** para clonar el repositorio de GitHub en el proyecto de Azure Notebooks. Si no tienes una cuenta de Azure Notebooks existente, se le pedirá crear uno e iniciar sesión en.

   ![Cuaderno de importación](./media/notebooks/sentinel-nb-signin-and-clone.png)

3. Al crear un nuevo proyecto, deberá al proyecto el nombre: use el nombre predeterminado o escribir en una nueva. No active el **clon recursivamente** opción - esta opción hace referencia a los repositorios de GitHub vinculados. Al hacer clic en **importación** empieza a clonar el contenido de GitHub, que puede tardar unos minutos en completarse.

   ![Cuaderno de importación](./media/notebooks/sentinel-create-nb-project.png)

4. Abra el **blocs de notas** carpeta para ver los blocs de notas. Cada cuaderno le guiará a través de los pasos para llevar a cabo una búsqueda o la investigación. Bibliotecas y otras dependencias necesarias para el Bloc de notas pueden instalarse desde el Bloc de notas o a través de un procedimiento de configuración simple. Configuración que vincula el proyecto de Bloc de notas a su suscripción de Azure Sentinel se aprovisiona automáticamente en los pasos anteriores. Los blocs de notas están listos para ejecutarse en el área de trabajo de Log Analytics de Centinela de Azure.

   ![Importación de repositorio](./media/notebooks/sentinel-open-notebook1.png)

5. Abra un bloc de notas. Proceso gratis se selecciona de forma predeterminada para ejecutar los blocs de notas (resaltados). Si ha configurado una instancia de DSVM para utilizar (véase más arriba), seleccione la instancia de DSVM y autenticarse antes de abrir el primer cuaderno. Haga clic en un bloc de notas para abrirlo.

   ![Seleccione el Bloc de notas](./media/notebooks/sentinel-open-notebook2.png)

6. Si selecciona la versión de Python. Cuando abra por primera vez un bloc de notas, que le solicite que seleccione una versión de kernel. Si no es así, seleccione el kernel para utilizar como sigue. Python 3.6 o posterior debe ser el kernel seleccionado (en la parte superior derecha de la ventana del Bloc de notas).

   ![Seleccione el Bloc de notas](./media/notebooks/sentinel-select-kernel.png)

Para obtener una introducción rápida a la consulta de datos en Azure Sentinel, examine el [GetStarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) Bloc de notas en la carpeta principal de blocs de notas. Cuadernos de ejemplo adicional pueden encontrarse en el **cuadernos de ejemplo** subcarpeta. Se guardaron los cuadernos de ejemplo con datos, para que resulte más fácil ver la salida prevista (se recomienda verlos en [nbviewer](https://nbviewer.jupyter.org/)). El **explicativa** carpeta contiene blocs de notas que describe, por ejemplo: configuración predeterminada de la versión de Python, configurar una instancia de DSVM, creación de Azure Sentinel marcadores desde un bloc de notas y otros temas.

Estos cuadernos están diseñados como ambas herramientas útiles, como las ilustraciones y los ejemplos de código que puede usar en el desarrollo de sus propios cuadernos.

Agradecemos los comentarios, si tiene sugerencias, las solicitudes de características, contribuyó con blocs de notas, los informes de errores o mejoras y adiciones a los blocs de notas existentes. Vaya a la [GitHub de Azure Sentinel Comunidad](https://github.com/Azure/Azure-Sentinel) para crear un problema o una bifurcación y cargar una contribución.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo empezar a usar Jupyter notebooks en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- [Captura de forma proactiva en busca de amenazas](hunting.md)
- [Uso de marcadores para guardar información interesante durante la búsqueda](bookmarks.md)