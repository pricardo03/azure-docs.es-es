---
title: Capacidades de búsqueda mediante cuadernos en Azure Sentinel, versión preliminar | Microsoft Docs
description: En este artículo se describe cómo usar cuadernos con las capacidades de búsqueda de Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
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
ms.openlocfilehash: ffe3ae5b6aa26d154928a74e51864a0574b82c68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65228639"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Uso de cuadernos de Jupyter Notebook para buscar amenazas de seguridad

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La base de Azure Sentinel es el almacén de datos; combina consultas de alto rendimiento y esquemas dinámicos, y tiene capacidad para escalar a volúmenes de datos masivos. El portal de Azure Sentinel y todas las herramientas de Azure Sentinel emplean una API común para acceder a este almacén de datos. Esta misma API está disponible también para herramientas externas, como los cuadernos de [Jupyter](https://jupyter.org/) y Python. Mientras que muchas tareas comunes pueden llevarse a cabo en el portal, Jupyter amplía las posibilidades de lo que se puede hacer con estos datos. Combina una programación completa con una vasta colección de bibliotecas pensadas para el análisis de datos, la visualización y Machine Learning. Estas características hacen que Jupyter sea una herramienta fascinante para la búsqueda e investigación relacionadas con cuestiones de seguridad.

![Cuaderno de ejemplo](./media/notebooks/sentinel-nb-mapandtimeline.png)

Hemos integrado la experiencia de Jupyter en el portal de Azure Sentinel, lo que facilita el proceso para crear y ejecutar cuadernos para analizar los datos. La biblioteca *Kqlmagic* actúa como el elemento aglutinador que permite realizar consultas desde Azure Sentinel y ejecutarlas directamente en un cuaderno. En las consultas se usa el [lenguaje de consulta Kusto](https://kusto.azurewebsites.net/docs/query/index.html). Azure Sentinel viene con varios cuadernos, desarrollados por algunos de los analistas de seguridad de Microsoft. Algunos de estos cuadernos se crean para escenarios concretos y se pueden usar tal cual; otros se incluyen como ejemplos para ilustrar las técnicas y características que se pueden copiar o adaptar para usarlas en sus propios cuadernos, y otros se pueden importar desde la comunidad GitHub de Azure Sentinel.

La experiencia de Jupyter integrada usa [Azure Notebooks](https://notebooks.azure.com/) para almacenar, compartir y ejecutar cuadernos. Estos cuadernos también se pueden ejecutar localmente (si tiene Jupyter y un entorno de Python en el equipo), o en otros entornos de JupyterHub como Azure Databricks.

Los cuadernos constan de dos componentes:

- La interfaz basada en explorador donde se escriben y ejecutan las consultas y el código, y donde se muestran los resultados de la ejecución.
- Un *kernel*, responsable de analizar y ejecutar el código en sí. 

En Azure Notebooks, este kernel se ejecuta de forma predeterminada en la *informática en la nube y almacenamiento gratuitos* de Azure. Si los cuadernos incluyen visualizaciones o modelos de Machine Learning complejos, conviene considerar la posibilidad de usar recursos de proceso dedicados más eficaces, como [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM). Los cuadernos de su cuenta son privados, a menos que prefiera compartirlos.

Los cuadernos de Azure Sentinel usan muchas bibliotecas de Python conocidas, como pandas, matplotlib y bokeh, entre otras. Existen otros muchos paquetes de Python que se pueden elegir, que cubren áreas como las siguientes:

- Visualizaciones y gráficos
- Procesamiento de datos y análisis
- Estadísticas y computación numérica
- Machine Learning y aprendizaje profundo

También hemos lanzado una serie de herramientas de seguridad de Jupyter de código abierto en un paquete denominado [msticpy](https://github.com/Microsoft/msticpy/). Este paquete se usa en muchos de los cuadernos incluidos en el producto. Las herramientas de Msticpy están diseñadas expresamente para ayudar a crear cuadernos que se puedan usar en búsquedas e investigaciones, y estamos trabajando activamente en nuevas características y mejoras.

Estos son los cuadernos que se incluyen inicialmente:

- **Investigación guiada - Procesar alertas**: permite evaluar alertas rápidamente analizando la actividad en los hosts afectados.
- **Búsqueda guiada - Explorador de host de Windows**: permite explorar la actividad de la cuenta, las ejecuciones de procesos, la actividad de la red y otros eventos de un host.  
- **Búsqueda guiada - Exploración de Office 365**: busca las actividades sospechosas de Office 365 en varios conjuntos de datos de Office 365.

El [repositorio de la comunidad GitHub de Azure Sentinel](https://github.com/Azure/Azure-Sentinel) es la ubicación de los futuros cuadernos de Azure Sentinel creados por Microsoft o procedentes de contribuciones de la comunidad.

## <a name="run-a-notebook"></a>Ejecutar un cuaderno

En el siguiente ejemplo, crearemos un proyecto de Azure Notebooks desde el portal de Azure Sentinel e incluiremos en él algunos cuadernos. Antes de usar estos cuadernos, es aconsejable hacer una copia de estos y trabajar en ellas. Trabajar en la copia permite actualizar sin riesgos a versiones futuras de los cuadernos, sin sobrescribir ningún dato.

1. En el portal de Azure Sentinel, haga clic en **Notebooks** (Cuadernos) en el menú de navegación. Para crear un proyecto de Azure Notebooks, haga clic en **Clone Azure Sentinel Notebooks** (Clonar cuadernos de Azure Sentinel) o, para abrir un proyecto de cuaderno existente, haga clic en **Go to your Notebooks** (Ir a sus cuadernos).
  
   ![Seleccionar cuadernos](./media/notebooks/sentinel-az-notebooks-home.png)

2. Si eligió **Clone Azure Sentinel Notebooks** (Clonar cuadernos de Azure Sentinel) en el paso anterior, se abrirá el siguiente cuadro de diálogo. Haga clic en **Import** (Importar) para clonar el repositorio de GitHub en el proyecto de Azure Notebooks. Si no tiene una cuenta de Azure Notebooks existente, se le pedirá que cree una e inicie sesión en ella.

   ![Importar cuaderno](./media/notebooks/sentinel-nb-signin-and-clone.png)

3. Al crear un proyecto, deberá darle un nombre; use el nombre predeterminado o indique uno nuevo. No seleccione la opción **Clone Recursively** (Clonar de forma recursiva), ya que está relacionada con repositorios de GitHub vinculados. Al hacer clic en **Import** (Importar), el contenido de GitHub empieza a clonarse, lo que puede tardar unos minutos en finalizar.

   ![Importar cuaderno](./media/notebooks/sentinel-create-nb-project.png)

4. Abra la carpeta **Notebooks** (Cuadernos) para ver los cuadernos. En cada cuaderno se le guiará por el proceso para llevar a cabo una búsqueda o una investigación. Las bibliotecas y otras dependencias necesarias para el cuaderno se pueden instalar desde el propio cuaderno o a través de un sencillo procedimiento de configuración. La configuración que vincula el proyecto del cuaderno con su suscripción de Azure Sentinel se aprovisiona automáticamente en los pasos anteriores. Los cuadernos están listos para ejecutarse en el área de trabajo de Log Analytics de Azure Sentinel.

   ![Importar repositorio](./media/notebooks/sentinel-open-notebook1.png)

5. Abra un cuaderno. La opción Free Compute (Proceso gratis), aquí resaltada, está seleccionada de forma predeterminada para ejecutar los cuadernos. Si ha configurado una instancia de DSVM para usarla (vea arriba), selecciónela y autentíquese antes de abrir el primer cuaderno. Haga clic en un cuaderno para abrirlo.

   ![Seleccionar cuaderno](./media/notebooks/sentinel-open-notebook2.png)

6. Seleccione la versión de Python. Cuando abra un cuaderno por primera vez, puede que se le pida que seleccione una versión de kernel. Si no es así, seleccione el kernel como se indica aquí. Python 3.6 o posterior debe ser el kernel seleccionado (en la parte superior derecha de la ventana del cuaderno).

   ![Seleccionar cuaderno](./media/notebooks/sentinel-select-kernel.png)

Para obtener una introducción rápida sobre cómo consultar datos en Azure Sentinel, eche un vistazo al cuaderno [GetStarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) en la carpeta principal Notebooks (Cuadernos). En la subcarpeta **Sample-Notebooks** (Cuadernos de ejemplo) hay más cuadernos de ejemplo. Los cuadernos de ejemplo se han guardado con datos, así será más fácil ver los resultados previstos (se recomienda verlos en la aplicación [nbviewer](https://nbviewer.jupyter.org/)). La carpeta **HowTos** (Procedimientos) contiene cuadernos que describen tareas como, entre otras muchas, configurar la versión de Python predeterminada, configurar una instancia de DSVM o crear marcadores de Azure Sentinel desde un cuaderno.

Estos cuadernos están diseñados como herramientas útiles, pero también como ilustraciones y códigos de ejemplo que los usuarios pueden utilizar para desarrollar sus propios cuadernos.

Agradecemos cualquier comentario, sean sugerencias, peticiones de características, contribuciones de cuadernos, informes de errores o mejoras y adiciones a cuadernos ya existentes. Vaya a la [comunidad GitHub de Azure Sentinel](https://github.com/Azure/Azure-Sentinel) para abrir una incidencia, crear una bifurcación y cargar una contribución.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a usar cuadernos de Jupyter Notebooks en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- [Búsqueda de amenazas con Azure Sentinel, versión preliminar](hunting.md)
- [Realizar un seguimiento de los datos durante una búsqueda](bookmarks.md)