---
title: Crear una instancia de Data Science Virtual Machine de Windows
titleSuffix: Azure
description: Configure y cree una instancia de Data Science Virtual Machine en Azure para realizar análisis y aprendizaje automático.
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: quickstart
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 2b328ccf63259c7faf4debdc07313d24381313a5
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70047689"
---
# <a name="provision-a-windows-data-science-virtual-machine-on-azure"></a>Aprovisionamiento de una instancia de Data Science Virtual Machine de Windows en Azure

Microsoft Data Science Virtual Machine (DSVM) es una imagen de máquina virtual (VM) Windows Server 2016 en Azure. Se preinstala y configura con herramientas para el análisis de datos y el aprendizaje automático.

## <a name="included-data-science-tools"></a>Herramientas de ciencia de datos incluidas

Se incluyen las siguientes herramientas en DSVM:

* SDK de Python para [Azure Machine Learning Service](../service/index.yml).
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer edition.
* Anaconda Python Distribution.
* Jupyter Notebook con kernels R, Python y PySpark.
* Microsoft Visual Studio Community.
* Microsoft Power BI Desktop.
* Microsoft SQL Server 2017 Developer edition.
* Una instancia independiente de Apache Spark para desarrollo y pruebas locales.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Herramientas de aprendizaje automático y análisis de datos:
  * Plataformas de aprendizaje profundo: En la máquina virtual se incluyen las plataformas [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), MXNet y Keras AI.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Un sistema de aprendizaje automático rápido que admite varias técnicas como el uso de hash en línea, la clase AllReduce, las reducciones, learning2search y los aprendizajes activo e interactivo.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Una herramienta que proporciona una implementación de árbol ampliada rápida y precisa.
  * [Rattle](https://togaware.com/rattle/). La herramienta analítica de R que simplifica la introducción al análisis de datos y al aprendizaje automático en R con una exploración de datos basada en GUI y un modelado con generación automática de códigos en R.
  * [Weka](https://www.cs.waikato.ac.nz/ml/weka/): Un software de minería de datos visual y aprendizaje automático de Java.
  * [Apache Drill](https://drill.apache.org/): Un motor de consultas SQL sin esquemas para Apache Hadoop, NoSQL y almacenamiento en la nube. Es compatible con las interfaces ODBC y JDBC para consultas NoSQL y archivos de herramientas de BI estándar como Power BI, Excel y Tableau.
* Bibliotecas en R y Python para usarlas en Azure Machine Learning y en otros servicios de Azure.
* Git, incluido Git Bash, para trabajar con repositorios de código fuente, como GitHub y Azure DevOps. Git proporciona varias herramientas de Linux conocidas de línea de comandos a las que se puede acceder en Git Bash y en un símbolo del sistema. Algunos ejemplos son awk, sed, perl, grep, find, wget y curl.
* Editores y herramientas de desarrollo (RStudio, PyCharm).

### <a name="about-data-science"></a>Información acerca de la ciencia de datos

La ciencia de datos implica la iteración de una secuencia de tareas:

1. Búsqueda, carga y preprocesamiento de datos.
1. Compile y pruebe los modelos.
1. Implemente los modelos para el consumo en aplicaciones inteligentes.

Los científicos de datos usan varias herramientas para estas tareas. Puede ser bastante lento encontrar las versiones del software adecuadas, descargarlas e instalarlas. La instancia de DSVM permite ahorrar tiempo al proporcionar una imagen lista para usar que se puede aprovisionar en Azure con las herramientas más populares preinstaladas y configuradas.

DSVM comienza de inmediato su proyecto de análisis. Puede trabajar en tareas en varios lenguajes, incluidos R, Python, SQL y C#. Visual Studio proporciona un entorno de desarrollo integrado (IDE) fácil de usar para desarrollar y probar el código. El SDK de Azure se incluye en la máquina virtual, por lo que puede crear las aplicaciones con servicios en la plataforma en la nube de Microsoft.

No hay cargos de software por esta imagen de DSVM. Pagará solo las cuotas de uso de Azure. Estas dependen del tamaño de la máquina virtual que aprovisione. Para más información, consulte la página [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows).

### <a name="other-dsvm-versions"></a>Otras versiones de DSVM

* Una imagen de [Ubuntu](dsvm-ubuntu-intro.md). Tiene herramientas similares a DSVM, más varias plataformas adicionales de aprendizaje profundo.
* Una imagen de [Linux CentOS](linux-dsvm-intro.md).
* La edición [Windows Server 2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) de Data Science Virtual Machine. Algunas herramientas solo están disponibles en la edición de Windows Server 2016. Respecto al resto, este artículo también se aplica a la versión Windows Server 2012.

## <a name="prerequisite"></a>Requisito previo

Para crear una instancia de Microsoft Data Science Virtual Machine, debe tener una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.com/free).

## <a name="create-your-dsvm"></a>Creación de su instancia de DSVM

Para crear una instancia de DSVM:

1. Vaya al listado de máquinas virtuales en [Azure Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016). Es posible que se le pida que inicie sesión en su cuenta de Azure, si todavía no lo ha hecho.
1. Seleccione el botón **Crear** que se encuentra en la parte inferior.

   ![Máquina virtual que se enumera en Azure Portal, con el botón Crear](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Escriba la siguiente información para configurar cada uno de los pasos que aparecen en el panel derecho de la captura de pantalla:

   1. **Aspectos básicos**:
      * **Nombre**: escriba el nombre de la instancia de DSVM.
      * **Tipo de disco de máquina virtual**: Seleccione **SSD** o bien **HDD**. Para una instancia de GPU NC_v1, como la basada en NVidia Tesla K80, elija **HDD** como el tipo de disco.
      * **Nombre de usuario**: escriba el identificador de la cuenta de administrador.
      * **Contraseña**: escriba la contraseña de la cuenta de administrador.
      * **Suscripción**: si tiene más de una suscripción, seleccione aquella en la que se creará y facturará la máquina.
      * **Grupo de recursos**: cree un grupo o use uno existente.
      * **Ubicación**: seleccione el centro de datos más adecuado. Para disfrutar de un acceso más rápido a la red, elija el centro de datos que tenga la mayoría de los datos o el que esté más cerca de su ubicación física.
   1. **Tamaño**: seleccione el tipo de servidor que cumpla sus requisitos funcionales y las restricciones de costo. Seleccione **Ver todo** para consultar más opciones de tamaños de máquina virtual.
   1. **Configuración**:  
      * **Uso de discos administrados**. Elija **Administrado** si desea que Azure administre los discos de la máquina virtual. En caso contrario, debe especificar una cuenta de almacenamiento nueva o existente.  
      * **Otros parámetros**. Puede usar los valores predeterminados. Si desea utilizar valores no predeterminados, mueva el puntero sobre el vínculo informativo para obtener ayuda sobre los campos específicos.
   1. **Resumen**: Compruebe que toda la información que ha especificado es correcta. Seleccione **Crear**.

> [!NOTE]
> * No se aplica ningún cargo adicional a la máquina virtual que no sea el del proceso por el tamaño de servidor elegido en el paso de selección de **tamaño**.
> * El aprovisionamiento tarda entre 10 y 20 minutos. Puede ver el estado de la máquina virtual en Azure Portal.

## <a name="access-the-dsvm"></a>Acceso a la instancia de DSVM

Una vez creada y aprovisionada la máquina virtual, puede acceder a ella mediante una conexión al escritorio remoto. Use las credenciales de la cuenta de administrador que configuró en el paso **Aspectos básicos** de la creación de una máquina virtual. 

Ya está listo para empezar a usar las herramientas que están instaladas y configuradas en la máquina virtual. Se puede acceder a muchas de las herramientas mediante los iconos del menú **Inicio** y de los iconos del escritorio.

También puede conectar una instancia de DSVM a Azure Notebooks para ejecutar cuadernos de Jupyter en la máquina virtual y omitir las limitaciones del nivel de servicio gratuito. Para más información, consulte [Administración y configuración de proyectos de Azure Notebooks](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

<a name="tools"></a>

## <a name="tools-installed-on-the-dvsm"></a>Herramientas instaladas en la instancia de DVSM

En las secciones siguientes, obtendrá más información sobre las herramientas que se instalan en la instancia de Data Science Virtual Machine.

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer edition

Puede usar Microsoft Enterprise Library para el análisis, ya que en la máquina virtual está instalado Machine Learning Server Developer Edition. Anteriormente conocido como Microsoft R Server, Machine Learning Server es una plataforma de análisis que se puede implementar en gran medida. Es escalable y se admite comercialmente.

Machine Learning Server admite diversas estadísticas de macrodatos, modelado predictivo y tareas de aprendizaje automático. Admite toda la gama de análisis: exploración, análisis, visualización y modelado. Mediante el uso y la extensión de R y Python de código abierto, Machine Learning Server es compatible con los scripts y funciones de R y Python. También es compatible con los paquetes de CRAN, pip y Conda para analizar datos a escala empresarial.

Machine Learning Server resuelve las limitaciones en memoria de R de código abierto agregando el procesamiento paralelo y fragmentado de datos. Esto significa que puede ejecutar análisis en datos mucho más grandes de lo que cabe en la memoria principal. 

Visual Studio Community está incluido en la máquina virtual. Contiene las extensiones Herramientas de R para Visual Studio y Herramientas de Python para Visual Studio (PTVS) que proporcionan un IDE completo para trabajar con R o Python. También se proporcionan otros IDE, como [RStudio](https://www.rstudio.com) y [PyCharm Community Edition](https://www.jetbrains.com/pycharm/), en la máquina virtual.

### <a name="python"></a>Python

Para el desarrollo con Python, se han instalado las distribuciones 2.7 y 3.6 de Anaconda Python. Esta distribución contiene Python base, junto con aproximadamente 300 de los paquetes de matemáticas, ingeniería y análisis de datos más populares. Puede usar PTVS, que se instala con Visual Studio Community 2017. O bien, puede usar uno de los IDE incluidos con Anaconda, como IDLE o Spyder. Busque y abra uno de estos paquetes (tecla del logotipo de Windows + S).

> [!NOTE]
> Para que Herramientas de Python para Visual Studio apunte a Anaconda Python 2.7, tiene que crear entornos personalizados para cada versión. Para establecer estas rutas de entorno en Visual Studio 2017 Community, vaya a **Herramientas** > **Herramientas de Python** > **Entornos de Python**. Seleccione **+ Personalizar**.

Se instala Anaconda Python 3.6 en C:\Anaconda. Se instala Anaconda Python 2.7 en c:\Anaconda\envs\python2. Consulte la [documentación de PTVS](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters) para ver los pasos detallados.

### <a name="the-jupyter-notebook"></a>Jupyter Notebook

La distribución de Anaconda también incluye una instancia de Jupyter Notebook, un entorno para compartir código y análisis. Previamente se ha configurado un servidor de Jupyter Notebook con kernels de Python 2.7, Python 3.x, PySpark, Julia y R. Para iniciar el servidor de Jupyter y el explorador para acceder al servidor de notebook, use el icono del escritorio llamado **Jupyter Notebook**.

Hemos empaquetado varios notebooks de ejemplo en Python y en R. Después de acceder a Jupyter, los notebooks muestran cómo trabajar con las siguientes tecnologías:

* Machine Learning Server
* SQL Server Machine Learning Services, análisis de base de datos
* Python
* Microsoft Cognitive Toolkit
* TensorFlow
* Otras tecnologías de Azure

Puede encontrar el vínculo a los ejemplos en la página principal del notebook después de que se autentique en Jupyter Notebook con la contraseña creada antes.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

DSVM incluye Visual Studio Community. Se trata de una versión gratuita del popular IDE de Microsoft que puede usar para fines de evaluación y para equipos pequeños. Consulte los [Términos de licencia del software de Microsoft](https://www.visualstudio.com/support/legal/mt171547).

Utilice el icono de escritorio o el menú **Inicio** para abrir Visual Studio. Busque programas (logotipo de Windows + S), seguido de **Visual Studio**. Desde allí, puede crear proyectos en lenguajes como C#, Python, R o Node.js. Los complementos instalados hacen que sea conveniente trabajar con los siguientes servicios de Azure:

* Azure Data Catalog
* Azure HDInsight para Hadoop y Spark
* Azure Data Lake

También hay un complemento llamado Azure Machine Learning para Visual Studio Code que se integra perfectamente en Azure Machine Learning y le ayuda a crear aplicaciones de inteligencia artificial rápidamente.

> [!NOTE]
> Quizás vea un mensaje que indica que el período de evaluación ha expirado. Escriba las credenciales de su cuenta Microsoft. O cree una nueva cuenta gratuita para acceder a Visual Studio Community.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

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

### <a name="azure"></a>Azure

En la VM se instalan varias herramientas de Azure:

* Un acceso directo del escritorio lleva a la documentación del SDK de Azure.
* Use AzCopy para copiar datos hacia la cuenta de Azure Storage y desde esta. Para ver el uso escriba **Azcopy** en un símbolo del sistema.
* Use el Explorador de Azure Storage para examinar los objetos que almacena en la cuenta de Azure Storage. También copia datos hacia Azure Storage y desde este servicio. Para acceder a esta herramienta, escriba **Explorador de Storage** en el campo **Búsqueda**. O buscarlo en el menú **Inicio** de Windows.
* AdlCopy copia datos en Azure Data Lake. Para ver el uso escriba **adlcopy** en un símbolo del sistema.
* La herramienta dtui permite copiar datos hacia Azure Cosmos DB y desde este servicio, una base de datos NoSQL en la nube. Escriba **dtui** en un símbolo del sistema.
* Integration Runtime permite copiar datos entre orígenes de datos en el entorno local y la nube. Se usa en herramientas como Azure Data Factory.
* Use Azure PowerShell para administrar los recursos de Azure en el lenguaje de scripting de PowerShell. También se instala en la máquina virtual.

### <a name="power-bi"></a>Power BI

La instancia de DSVM viene con Power BI Desktop instalado para ayudarle a crear paneles y visualizaciones. Use esta herramienta para extraer datos de orígenes diferentes, para crear los paneles e informes y publicarlos en la nube. Para más información, consulte el [sitio de Power BI](https://powerbi.microsoft.com). Puede encontrar Power BI Desktop en el menú **Inicio**.

> [!NOTE]
> Necesitará una cuenta de Microsoft Office 365 para acceder a Power BI.

### <a name="azure-machine-learning-sdk-for-python"></a>SDK de Azure Machine Learning para Python

Los científicos de datos y los desarrolladores de inteligencia artificial usan el SDK de Azure Machine Learning para Python para crear y ejecutar flujos de trabajo de aprendizaje automático con [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md). Puede interactuar con el servicio en Jupyter Notebooks u otro IDE de Python mediante marcos de código abierto, como TensorFlow y scikit-learn.

El SDK de Python viene preinstalado en Microsoft Data Science Virtual Machine. Para comenzar a usar el SDK de Python, consulte [Uso del SDK de Python para empezar a usar Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Herramientas de desarrollo de Microsoft adicionales

Puede usar el [Instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) para detectar y descargar otras herramientas de desarrollo de Microsoft. También hay un acceso directo a la herramienta en el escritorio de Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Directorios importantes en la máquina virtual

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

* Para explorar las herramientas en DSVM; abra el menú **Inicio**.
* Obtenga información acerca de Azure Machine Learning Service en [¿Qué es el servicio Azure Machine Learning?](../service/overview-what-is-azure-ml.md) y pruebe los [inicios rápidos y tutoriales](../service/index.yml).
* En el Explorador de archivos, vaya a C:\Archivos de programa\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts para ver ejemplos de uso de la biblioteca RevoScaleR de R que admiten análisis de datos a escala empresarial. 
* Lea el artículo [Diez cosas que puede hacer en Data Science Virtual Machine](https://aka.ms/dsvmtenthings).
* Aprenda a crear soluciones analíticas completas mediante el uso sistemático del [proceso de ciencia de datos en equipo](../team-data-science-process/index.yml).
* Visite la [Galería de Azure AI](https://gallery.cortanaintelligence.com) para ver ejemplos de aprendizaje automático y análisis de datos donde se usa Azure Machine Learning y servicios de datos relacionados en Azure. También hemos proporcionado un icono para esta galería en el menú **Inicio** y en el escritorio de la máquina virtual.
