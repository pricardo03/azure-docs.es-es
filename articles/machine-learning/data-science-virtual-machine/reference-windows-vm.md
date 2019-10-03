---
title: 'Referencia: Windows DSVM'
description: Detalles de las herramientas incluidas en una máquina Data Science VM de Windows
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: be4397de477891f2a698c9f7dcb131da79479529
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200016"
---
# <a name="reference-windows-data-science-virtual-machine"></a>Referencia: Data Science Virtual Machine de Windows

A continuación encontrará una lista de las herramientas disponibles en la máquina Data Science Virtual Machine de Windows. 

## <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer edition

Puede usar Microsoft Enterprise Library para el análisis, ya que en la máquina virtual está instalado Machine Learning Server Developer Edition. Anteriormente conocido como Microsoft R Server, Machine Learning Server es una plataforma de análisis que se puede implementar en gran medida. Es escalable y se admite comercialmente.

Machine Learning Server admite diversas estadísticas de macrodatos, modelado predictivo y tareas de aprendizaje automático. Admite toda la gama de análisis: exploración, análisis, visualización y modelado. Mediante el uso y la extensión de R y Python de código abierto, Machine Learning Server es compatible con los scripts y funciones de R y Python. También es compatible con los paquetes de CRAN, pip y Conda para analizar datos a escala empresarial.

Machine Learning Server resuelve las limitaciones en memoria de R de código abierto agregando el procesamiento paralelo y fragmentado de datos. Esto significa que puede ejecutar análisis en datos mucho más grandes de lo que cabe en la memoria principal. 

Visual Studio Community está incluido en la máquina virtual. Contiene las extensiones Herramientas de R para Visual Studio y Herramientas de Python para Visual Studio (PTVS) que proporcionan un IDE completo para trabajar con R o Python. También se proporcionan otros IDE, como [RStudio](https://www.rstudio.com) y [PyCharm Community Edition](https://www.jetbrains.com/pycharm/), en la máquina virtual.

## <a name="python"></a>Python

Para el desarrollo con Python, se han instalado las distribuciones 2.7 y 3.6 de Anaconda Python. Esta distribución contiene Python base, junto con aproximadamente 300 de los paquetes de matemáticas, ingeniería y análisis de datos más populares. Puede usar PTVS, que se instala con Visual Studio Community 2017. O bien, puede usar uno de los IDE incluidos con Anaconda, como IDLE o Spyder. Busque y abra uno de estos paquetes (tecla del logotipo de Windows + S).

> [!NOTE]
> Para que Herramientas de Python para Visual Studio apunte a Anaconda Python 2.7, tiene que crear entornos personalizados para cada versión. Para establecer estas rutas de entorno en Visual Studio 2017 Community, vaya a **Herramientas** > **Herramientas de Python** > **Entornos de Python**. Seleccione **+ Personalizar**.

Se instala Anaconda Python 3.6 en C:\Anaconda. Se instala Anaconda Python 2.7 en c:\Anaconda\envs\python2. Consulte la [documentación de PTVS](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters) para ver los pasos detallados.

## <a name="the-jupyter-notebook"></a>Jupyter Notebook

La distribución de Anaconda también incluye una instancia de Jupyter Notebook, un entorno para compartir código y análisis. Previamente se ha configurado un servidor de Jupyter Notebook con kernels de Python 2.7, Python 3.x, PySpark, Julia y R. Para iniciar el servidor de Jupyter y el explorador para acceder al servidor de notebook, use el icono del escritorio llamado **Jupyter Notebook**.

Hemos empaquetado varios notebooks de ejemplo en Python y en R. Después de acceder a Jupyter, los notebooks muestran cómo trabajar con las siguientes tecnologías:

* Machine Learning Server
* SQL Server Machine Learning Services, análisis de base de datos
* Python
* Microsoft Cognitive Toolkit
* TensorFlow
* Otras tecnologías de Azure

Puede encontrar el vínculo a los ejemplos en la página principal del notebook después de que se autentique en Jupyter Notebook con la contraseña creada antes.

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

DSVM incluye Visual Studio Community. Se trata de una versión gratuita del popular IDE de Microsoft que puede usar para fines de evaluación y para equipos pequeños. Consulte los [Términos de licencia del software de Microsoft](https://www.visualstudio.com/support/legal/mt171547).

Utilice el icono de escritorio o el menú **Inicio** para abrir Visual Studio. Busque programas (logotipo de Windows + S), seguido de **Visual Studio**. Desde allí, puede crear proyectos en lenguajes como C#, Python, R o Node.js. Los complementos instalados hacen que sea conveniente trabajar con los siguientes servicios de Azure:

* Azure Data Catalog
* Azure HDInsight para Hadoop y Spark
* Azure Data Lake

También hay un complemento llamado Azure Machine Learning para Visual Studio Code que se integra perfectamente en Azure Machine Learning y le ayuda a crear aplicaciones de inteligencia artificial rápidamente.

> [!NOTE]
> Quizás vea un mensaje que indica que el período de evaluación ha expirado. Escriba las credenciales de su cuenta Microsoft. O cree una nueva cuenta gratuita para acceder a Visual Studio Community.

## <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

DSVM se proporciona con una versión para programadores de SQL Server 2017 con Machine Learning Services. Esta edición de SQL Server está disponible en R o Python y puede ejecutar análisis en bases de datos. 

Machine Learning Services proporciona una plataforma para desarrollar e implementar aplicaciones inteligentes. Puede usar estos lenguajes y los numerosos paquetes de la comunidad para crear modelos y generar predicciones con sus datos de SQL Server. Puede mantener el análisis cerca de los datos, porque Machine Learning Services (en la base de datos) integra los lenguajes R y Python dentro de SQL Server. Esta integración elimina los costos y riesgos de seguridad asociados con el movimiento de datos.

> [!NOTE]
> La edición SQL Server Developer solo puede utilizarse para fines de desarrollo y prueba. Para ejecutarlo en producción necesita una licencia.

Para acceder a SQL Server, abra Microsoft SQL Server Management Studio. El nombre de la máquina virtual se rellenará como **Nombre del servidor**. Use la autenticación de Windows cuando inicie sesión como administrador en Windows. Cuando esté en SQL Server Management Studio, puede crear otros usuarios, crear bases de datos, importar datos y ejecutar consultas SQL.

Para habilitar el análisis en la base de datos con Machine Learning Services de SQL Server, ejecute el siguiente comando como una acción puntual en SQL Server Management Studio después de iniciar sesión como administrador del servidor:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Reemplace `%COMPUTERNAME%` por el nombre de la máquina virtual.

## <a name="azure"></a>Azure

En la VM se instalan varias herramientas de Azure:

* Un acceso directo del escritorio lleva a la documentación del SDK de Azure.
* Use AzCopy para copiar datos hacia la cuenta de Azure Storage y desde esta. Para ver el uso escriba **Azcopy** en un símbolo del sistema.
* Use el Explorador de Azure Storage para examinar los objetos que almacena en la cuenta de Azure Storage. También copia datos hacia Azure Storage y desde este servicio. Para acceder a esta herramienta, escriba **Explorador de Storage** en el campo **Búsqueda**. O buscarlo en el menú **Inicio** de Windows.
* AdlCopy copia datos en Azure Data Lake. Para ver el uso escriba **adlcopy** en un símbolo del sistema.
* La herramienta dtui permite copiar datos hacia Azure Cosmos DB y desde este servicio, una base de datos NoSQL en la nube. Escriba **dtui** en un símbolo del sistema.
* Integration Runtime permite copiar datos entre orígenes de datos en el entorno local y la nube. Se usa en herramientas como Azure Data Factory.
* Use Azure PowerShell para administrar los recursos de Azure en el lenguaje de scripting de PowerShell. También se instala en la máquina virtual.

## <a name="power-bi"></a>Power BI

La instancia de DSVM viene con Power BI Desktop instalado para ayudarle a crear paneles y visualizaciones. Use esta herramienta para extraer datos de orígenes diferentes, para crear los paneles e informes y publicarlos en la nube. Para más información, consulte el [sitio de Power BI](https://powerbi.microsoft.com). Puede encontrar Power BI Desktop en el menú **Inicio**.

> [!NOTE]
> Necesitará una cuenta de Microsoft Office 365 para acceder a Power BI.

## <a name="azure-machine-learning-sdk-for-python"></a>SDK de Azure Machine Learning para Python

Los científicos de datos y los desarrolladores de inteligencia artificial usan el SDK de Azure Machine Learning para Python para crear y ejecutar flujos de trabajo de aprendizaje automático con [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md). Puede interactuar con el servicio en Jupyter Notebooks u otro IDE de Python mediante marcos de código abierto, como TensorFlow y scikit-learn.

El SDK de Python viene preinstalado en Microsoft Data Science Virtual Machine. Para comenzar a usar el SDK de Python, consulte [Uso del SDK de Python para empezar a usar Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Herramientas de desarrollo de Microsoft adicionales

Puede usar el [Instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) para detectar y descargar otras herramientas de desarrollo de Microsoft. También hay un acceso directo a la herramienta en el escritorio de Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-virtual-machine"></a>Directorios importantes en la máquina virtual

| item | Directorio |
| --- | --- |
| Configuraciones del servidor de Jupyter Notebook | C:\ProgramData\jupyter |
| Directorio principal de ejemplos de notebooks de Jupyter | C:\dsvm\notebooks y c:\users\\<nombre_de_usuario\>\notebooks |
| Otros ejemplos | C:\dsvm\samples |
| Anaconda, predeterminado: Python 3.6 | C:\Anaconda |
| Entorno Anaconda Python 2.7 | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (independiente) para Python | C:\Archivos de programa\Microsoft\ML Server\PYTHON_SERVER |
| Instancia de R predeterminada, Machine Learning Server (independiente) | C:\Archivos de programa\Microsoft\ML Server\R_SERVER |
| Directorio de la instancia en la base de datos de Machine Learning Services de SQL Server | C:\Archivos de programa\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Herramientas varias | C:\dsvm\tools |

> [!NOTE]
> En la edición Windows Server 2012 de DSVM y la edición de Windows Server 2016 anterior a marzo de 2018, el entorno de Anaconda predeterminado es Python 2.7. El entorno secundario es Python 3.5, ubicado en C:\Anaconda\envs\py35.

## <a name="next-steps"></a>Pasos siguientes

¿Tiene más preguntas? Considere la creación de [una incidencia de soporte técnico](https://azure.microsoft.com/support/create-ticket/).
