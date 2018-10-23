---
title: Creación de una solución de aprendizaje automático de Edge con Azure y Azure Stack | Microsoft Docs
description: Aprenda a crear una solución de aprendizaje automático de Edge mediante Python con Azure y Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 28ff8dbf073596e5f9565c56ae903af6af68f3e2
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353723"
---
# <a name="tutorial-create-an-edge-machine-learning-solution-with-azure-and-azure-stack"></a>Tutorial: Creación de una solución de aprendizaje automático de Edge con Azure y Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Aprenda a crear una solución de aprendizaje automático de Edge con Azure y Azure Stack.

En este tutorial, creará un entorno de ejemplo para:

> [!div class="checklist"]
> - Cree una cuenta de almacenamiento y un contenedor que contenga los datos limpios.
> - Crear una instancia de Data Science Virtual Machine (DSVM) Ubuntu en Azure Portal
> - Implementar Azure Machine Learning Services en Azure para compilar y entrenar modelos
> - Crear cuentas de Azure Machine Learning Services
> - Implementar y usar Azure Container Registry
> - Implementar un clúster de Kubernetes en Azure Stack
> - Crear una cuenta de almacenamiento de Azure Stack y la cola de almacenamiento para los datos
> - Crear una función de Azure Stack para mover los datos limpios de Azure Stack a Azure

**Cuándo se debe utilizar esta solución**

 -  Su organización usa un enfoque DevOps, o tiene uno planeado en un futuro cercano.
 -  Quiere implementar prácticas de CI/CD en su implementación de Azure Stack y la nube pública.
 -  Quiere consolidar la canalización de CI/CD en entornos locales y de nube.
 -  Quiere la posibilidad de desarrollar aplicaciones mediante servicios locales y en la nube.
 -  Quiere aprovechar habilidades de desarrollo coherentes en aplicaciones locales y en la nube.

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack es una extensión de Azure. Azure Stack aporta la agilidad y la innovación de la informática en la nube a su entorno local y hace posible la única nube híbrida que le permite crear e implementar aplicaciones híbridas en cualquier parte.  
> 
> En las notas del producto [Consideraciones de diseño para aplicaciones híbridas](https://aka.ms/hybrid-cloud-applications-pillars) se revisan los pilares de la calidad de software (selección de ubicación, escalabilidad, disponibilidad, resistencia, manejabilidad y seguridad) para diseñar, implementar y usar aplicaciones híbridas. Las consideraciones de diseño ayudan a optimizar el diseño de aplicaciones híbridas y reducen los desafíos en los entornos de producción.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este caso de uso se requieren unos cuantos componentes y cierto tiempo de preparación:

 -  Un asociado de hardware u OEM de Azure puede implementar una instancia de Azure Stack de producción y todos los usuarios pueden implementar un ASDK.

 -  Además, un operador de Azure Stack debe implementar también la instancia de App Service, crear planes y ofertas, crear una suscripción de inquilino y agregar la imagen de Windows Server 2016.

 -  Se necesita una configuración de red híbrida y App Service (aprenda más sobre la [integración de aplicaciones con las redes virtuales.](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet))

 -  Antes de la implementación, se debe configurar el [agente de compilación y versión](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) privado para la integración de VSTS (antes de comenzar, asegúrese de que los componentes existentes usados cumplan los requisitos).

Se requieren conocimientos previos de Azure y Azure Stack. Para más información antes de continuar, comience con estos temas:

 -  [Introducción a Azure](https://azure.microsoft.com/overview/what-is-azure/)

 -  [Conceptos clave de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

 -  [Guía de soluciones híbridas de CI/CD de Azure Stack](/azure/azure-stack/user/azure-stack-solution-pipeline)

**Las tablas de Azure**

 -  Una suscripción de Azure (cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

 -  Una dirección URL de aplicación web creada mediante [Web App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) en Azure

 -  Implementación de [Kubernetes de Azure Container Services (ACS) en Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

 -  Implementación del servicio Azure Machine Learning (versión preliminar), [tutorial de 4 partes](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/tutorial-classifying-iris-part-1)

 -  Una [cuenta](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/experimentation-service-template) de Experimentación de Azure Machine Learning

**Azure Stack**

 -  Un sistema integrado de Azure Stack o la implementación del kit de desarrollo de Azure Stack.

    - Encontrará instrucciones para la instalación de Azure Stack en [Instalación del Kit de desarrollo de Azure Stack](/articles/azure-stack/asdk/asdk-install).
     - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Esta instalación puede tardar algunas horas en completarse.

 -  Implementación de servicios PaaS de [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) en Azure Stack

 -  Un [plan u oferta](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) dentro del entorno de Azure Stack

 -  Una [suscripción de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) dentro del entorno de Azure Stack

 -  Una imagen de máquina virtual Ubuntu Server (disponible en [Marketplace de Azure Stack](https://buildazure.com/2016/05/04/azure-marketplace-ubuntu-server-16-04-lts/)). Esta máquina virtual se creará en la suscripción del inquilino en Azure Stack como el agente de compilación privado y como las máquinas virtuales de Kubernetes. Si esta imagen no está disponible, el operador de Azure Stack puede ayudarle para garantizar que se agrega al entorno. (No use la compilación 18.04 de Ubuntu, ya que no se admite actualmente).

 -  Una aplicación web dentro de la suscripción del inquilino (anote la nueva dirección URL de la aplicación web para usarla más tarde).

 -  Implementación de una máquina virtual de agente de compilación privado basado en Linux para VSTS, dentro de la suscripción de inquilino

 -  Implementación de [Kubernetes de Azure Container Services (ACS) en Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

**Herramientas para desarrolladores**

 -  [Área de trabajo de VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) (el proceso de inicio de sesión crea un proyecto llamado "MyFirstProject")

 -  Instalación de [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) e inicio de sesión de [VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services)

 -  [Clon local](https://www.visualstudio.com/docs/git/gitquickstart) del proyecto

 -  Instalación del [subsistema Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) (para BASH y SSH)

 -  Instalación de [Docker para Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)

 -  Instalación de [Azure Machine Learning Workbench (versión preliminar)](https://aka.ms/azureml-wb-msi)

 -  Instalación del entorno de [Python](https://www.python.org/ftp/python/3.7.0/python-3.7.0rc1-amd64.exe)

**VSTS**

 -  **Cuenta de VSTS.** Configure rápidamente la integración continua para compilación, prueba e implementación. Para más información sobre las cuentas de VSTS, consulte [Creación de la cuenta de VSTS](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student?view=vsts).

 -  **Repositorio de código.** Con el uso de los repositorios de código existentes en GitHub, BitBucket, DropBox, One Drive y TFS, la plataforma VSTS puede aprovechar varios repositorios de código para agilizar la canalización de desarrollo. Para más información sobre los repositorios de código, consulte el tutorial [Get Started with Git and VSTS](https://docs.microsoft.com/vsts/git/gitquickstart?view=vsts&tabs=visual-studio) (Introducción a Git y VSTS).

 -  **Conexión a servicios.** Conéctese a servicios externos y remotos para ejecutar tareas de pruebas, compilación o implementación. Para más información sobre las conexiones a servicios de VSTS, consulte [Service Endpoints for Build and Release](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints?view=vsts) (Puntos de conexión de servicio para compilación y versión).

 -  **Definiciones de compilación.** Defina los procesos de compilación automatizada y elabore un conjunto de tareas mediante el catálogo de tareas. Para más información sobre las definiciones de compilación, consulte la documentación sobre la [creación de una definición de compilación](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts).

 -  **Definiciones de versión.** Defina el proceso de implementación para una colección de entornos donde se implementan los artefactos de la aplicación. Para más información sobre las definiciones de versión, consulte la documentación sobre la [creación de una definición de versión](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts).

 -  **Grupo de agentes de compilación de Linux para VSTS hospedados.** Compile, pruebe e implemente rápidamente aplicaciones mediante un agente hospedado administrado y mantenido por Microsoft. Para más información sobre los agentes de compilación de VSTS hospedados, consulte la documentación sobre los [agentes hospedados](https://docs.microsoft.com/vsts/build-release/concepts/agents/hosted?view=vsts).

## <a name="step-1-create-a-storage-account"></a>Paso 1: Creación de una cuenta de almacenamiento

Cree una cuenta de almacenamiento y un contenedor que contenga los datos limpios.

1.  Inicie sesión en [*Azure Portal*](https://portal.azure.com/).

2.  En Azure Portal, expanda el menú de la izquierda para abrir el menú de servicios y elija **Todos los servicios**. Desplácese hacia abajo hasta **Almacenamiento** y elija **Cuentas de almacenamiento**. En la ventana **Cuentas de almacenamiento**, elija **Agregar**.

3.  Escriba un nombre para la cuenta de almacenamiento.

    > [!Note]  
    > Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. El nombre de la cuenta de almacenamiento debe ser único dentro de Azure. Azure Portal indicará si el nombre de la cuenta de almacenamiento que seleccione ya está en uso.

4.  Especifique el modelo de implementación que se va a usar: **Resource Manager**.

5.  Seleccione el tipo de cuenta de almacenamiento: **uso general V1** y, luego, especifique el nivel de rendimiento: **Estándar**.

6.  Seleccione la opción de replicación de la cuenta de almacenamiento: **GRS**.

7.  Seleccione la nueva suscripción de la cuenta de almacenamiento.

8.  Especifique un nuevo grupo de recursos o seleccione un grupo de recursos existente.

9.  Seleccione la ubicación geográfica de la cuenta de almacenamiento.

10. Seleccione **Crear** para crear la cuenta de almacenamiento.

    ![Texto alternativo](\media\azure-stack-solution-machine-learning\image1.png)

11.  Elija la cuenta de almacenamiento creada recientemente.

12.  Seleccione **Blobs**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image2.png)

13.  Seleccione **+ Contenedor** y seleccione **Contenedor**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image3.png)

14.  Asigne al contenedor el nombre **uploadeddata** y elija el tipo de acceso **Contenedor**.

15.  Seleccione **Crear**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image4.png)

## <a name="step-2-create-a-data-science-virtual-machine"></a>Paso 2: Creación de una instancia de Data Science Virtual Machine

Cree una instancia de Data Science Virtual Machine (DSVM) Ubuntu en Azure Portal.

1.  Inicie sesión en Azure Portal desde [https://portal.azure.com](https://portal.azure.com/).

2.  Seleccione el vínculo **+NUEVO** y busque "Data Science Virtual Machine for Linux (Ubuntu)".

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image5.png)

1.  Elija **Data Science Virtual Machine for Linux (Ubuntu)** en la lista y siga las instrucciones en pantalla para crear la DSVM.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image6.png)

> Importante  
> **Elija** contraseña** como*Tipo de autenticación*.

Coloque la nueva DSVM en el mismo grupo de recursos que la cuenta de almacenamiento recién creada. Todos los objetos de aprendizaje automático de Edge se implementan en Azure dentro de este grupo de recursos.

1.  En Configuración, configure las características opcionales.

    a.  Seleccione la **cuenta de almacenamiento** que creó anteriormente.

    b.  Cree una **red virtual**, una **subred** y una **dirección IP pública**; se creará de forma predeterminada un nombre basado en el nombre del grupo de recursos.

    c.  Cree un **grupo de seguridad de red**; se creará automáticamente con las reglas correctas ya aplicadas.

    d.  En **Cuenta de almacenamiento de diagnóstico**, seleccione la cuenta de almacenamiento que creó anteriormente.

    e.  Nota: Con AAD habilitado y configurado para la suscripción de Azure, también se pueden habilitar las identidades administradas para los recursos de Azure.

2.  Seleccione **Aceptar**.

### <a name="connect-to-the-dsvm"></a>Conexión a la DSVM

Una vez creada la DSVM, conéctese a la máquina virtual desde el subsistema Windows para Linux.

```Bash  
    ssh <user>@<DSVM Public IP>
```

1.  Cuando se le pida que confirme la conexión de la máquina virtual, escriba Sí.

2.  Escriba la contraseña que creó para la DSVM.

### <a name="update-the-dsvm"></a>Actualización de la DSVM 

```Bash  
sudo su 
apt-get update 
apt-get -y upgrade 
apt-get -y dist-upgrade 
apt-get -y autoremove
```

## <a name="step-3-deploy-azure-machine-learning-services"></a>Paso 3: Implementación de Azure Machine Learning Services

Implementación de Azure Machine Learning Services en Azure.

Los servicios de Azure Machine Learning (versión preliminar) son una solución integrada y completa de análisis avanzado y ciencia de datos. Ayuda a los científicos de datos profesionales a preparar datos, desarrollar experimentos e implementar modelos a escala de nube.

En esta sección se explica cómo:

> [!div class="checklist"]
> - Crear cuentas de servicio para los servicios de Azure Machine Learning
> - Instalar Azure Machine Learning Workbench e iniciar sesión
> - Crear un proyecto en Workbench
> - Ejecutar un script en el proyecto
> - Acceder a la interfaz de la línea de comandos (CLI)

Como parte de la cartera de Microsoft Azure, los servicios de Azure Machine Learning requieren una suscripción a Azure. Para obtener una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Se necesitan los permisos adecuados para crear recursos, como grupos de recursos, máquinas virtuales. etc.

La aplicación Azure Machine Learning Workbench se puede instalar en los siguientes sistemas operativos:

 -  Windows 10 o Windows Server 2016
 -  macOS Sierra o High Sierra

## <a name="step-4-create-azure-machine-learning-services"></a>Paso 4: Creación de cuentas de Azure Machine Learning Services

Cree cuentas de Azure Machine Learning Services.

Use Azure Portal para aprovisionar las cuentas de Azure Machine Learning:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure que se va a usar. Para obtener una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image7.png)

1.  Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal.

    ![Creación de un recurso en Azure Portal](media\azure-stack-solution-machine-learning\image8.png)

1.  Escriba **Machine Learning** en la barra de búsqueda. Seleccione el resultado de búsqueda denominado **Experimentación de Machine Learning (versión preliminar)** .

    ![Búsqueda de Azure Machine Learning](media\azure-stack-solution-machine-learning\image9.png)

1.  En el panel **Experimentación de Machine Learning**, desplácese hasta la parte inferior y seleccione **Crear** para empezar a definir la cuenta de experimentación.

    ![Azure Machine Learning: creación de una cuenta de experimentación](media\azure-stack-solution-machine-learning\image10.png)

1.  En el panel **Experimentación de ML**, configure la cuenta de Experimentación de Machine Learning.

    | Configuración | Valor recomendado para el tutorial | DESCRIPCIÓN |
    |---------------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Nombre de cuenta de Experimentación | Nombre único | Escriba un nombre único que identifique la cuenta. Use el nombre de departamento o proyecto que mejor identifique el experimento. El nombre debe tener entre 2 y 32 caracteres. Debe incluir solo caracteres alfanuméricos y el carácter de guión (-). |
    | Subscription | La suscripción | Elija la suscripción de Azure que se usará en el experimento. Si tiene varias suscripciones, elija aquella en la que se factura el recurso. |
    | Grupos de recursos | El grupo de recursos | Use un grupo de recursos existente en la suscripción o escriba un nombre para crear un grupo de recursos para esta cuenta de experimentación. |
    | Ubicación | La región más cercana a los usuarios | Elija la ubicación más cercana a los usuarios y los recursos de datos. |
    | Número de puestos | 2 | Escriba el número de puestos. Obtenga información acerca de cómo los [puestos afectan a los precios](https://azure.microsoft.com/pricing/details/machine-learning/).<br><br>En esta guía de inicio rápido, se necesitan solo dos puestos. Los puestos se pueden agregar o quitar según sea necesario en Azure Portal. |
    | Cuenta de almacenamiento | Nombre único | Seleccione **Crear nuevo** y especifique un nombre para crear una [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal). El nombre debe tener entre 3 y 24 caracteres e incluir únicamente caracteres alfanuméricos. Además, seleccione **Usar existente** y elija la cuenta de almacenamiento existente de la lista. La cuenta de almacenamiento es necesaria y se utiliza para almacenar los artefactos de proyecto y ejecutar datos de historial. |
    | Área de trabajo para Experimentación | IrisGarden<br>(el nombre usado en los tutoriales) | Proporcione el nombre de un área de trabajo para esta cuenta. El nombre debe tener entre 2 y 32 caracteres. Debe incluir solo caracteres alfanuméricos y el carácter de guión (-). Esta área de trabajo contiene herramientas necesarias para crear, administrar y publicar experimentos. |
    | Asignar propietario para el área de trabajo | La cuenta | Seleccione su propia cuenta como propietario del área de trabajo. |
    | Creación de cuenta de Administración de modelos | **activar** | Cree una cuenta de Administración de modelos. Se usará para implementar y administrar los modelos como servicios web en tiempo real. <br><br>Si bien es opcional, se recomienda crear la cuenta de Administración de modelos al mismo tiempo que la cuenta de Experimentación. |
    | Nombre de cuenta | Nombre único | Elija un nombre único que identifique la cuenta de Administración de modelos. Use el nombre de departamento o proyecto que mejor identifique el experimento. El nombre debe tener entre 2 y 32 caracteres. Debe incluir solo caracteres alfanuméricos y el carácter de guión (-). |
    | Plan de tarifa de Administración de modelos | **DEVTEST** | Seleccione **No se ha seleccionado ningún plan de tarifa** para especificar el plan de tarifa para la nueva cuenta de Administración de modelos. Para ahorrar costos, seleccione el plan de tarifa DEVTEST, en caso de que esté disponible para su suscripción (disponibilidad limitada). De lo contrario, seleccione el plan de tarifa S1. Elija Seleccionar para guardar la selección del plan de tarifa. |
    | Anclar al panel | check | Seleccione la opción **Anclar al panel** para permitir realizar un seguimiento fácil de la cuenta de Experimentación de Machine Learning en la página del panel frontal de Azure Portal. |

    ![Configuración de la cuenta de Experimentación de Machine Learning](media\azure-stack-solution-machine-learning\image11.png)

1.  Seleccione **Crear** para comenzar el proceso de creación de la cuenta de Experimentación junto con la cuenta de Administración de modelos.

    ![Configuración de la cuenta de Experimentación de Machine Learning](media\azure-stack-solution-machine-learning\image12.png)

    La cuenta puede tardar unos momentos en crearse. Para comprobar el estado del proceso de implementación, haga clic en el icono Notificaciones (campana) en la barra de herramientas de Azure Portal.

    ![Notificaciones de Azure Portal](media\azure-stack-solution-machine-learning\image13.png)

### <a name="install-and-log-in-to-workbench"></a>Instalación e inicio de sesión en Workbench 

Azure Machine Learning Workbench está disponible para Windows o Mac OS. Consulte la lista de [plataformas admitidas](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation).

**Advertencia:** la instalación puede tardar alrededor de una hora en completarse.

1.  Descargue e inicie el instalador de Workbench más reciente.

    > [!Important]  
    > Descargue el instalador por completo en el disco y ejecútelo desde allí. No lo ejecute directamente desde el widget de descarga del explorador.<br>**En Windows:<br>** a. Descargue [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi).<br>b. Haga doble clic en el instalador descargado en el Explorador de archivos.

1.  Siga las instrucciones en pantalla en el instalador hasta su finalización.

    **La instalación puede tardar hasta 30 minutos en completarse. **
    
    `Windows: C:\\Users\\<user>\\AppData\\Local\\AmlWorkbench`
    
    El instalador descarga e instala todas las dependencias necesarias, como Python, Miniconda y otras bibliotecas relacionadas. Esta instalación incluye también la herramienta de línea de comandos multiplataforma de Azure, o CLI de Azure.

1.  Para iniciar Workbench, seleccione el botón **Launch Workbench** (Iniciar Workbench) en la última pantalla del instalador.

    Si está cerrado el instalador, inícielo con el acceso directo del escritorio **Machine Learning Workbench**.

1.  Seleccione **Iniciar sesión con Microsoft** para autenticarse en Azure Machine Learning Workbench. Use las mismas credenciales que utilizó en Azure Portal para crear las cuentas de Administración de modelos y Experimentación.

    Cuando haya iniciado sesión, Workbench usa la primera cuenta de Experimentación que encuentra en las suscripciones de Azure y muestra todas las áreas de trabajo y los proyectos asociados a esa cuenta.

    > [!Tip]  
    > Para cambiar a otra cuenta de Experimentación, use el icono situado en la esquina inferior izquierda de la ventana de la aplicación Workbench.

### <a name="create-a-new-project-in-workbench"></a>Creación de un proyecto en Workbench

1.  Abra la aplicación Azure Machine Learning Workbench e inicie sesión si es necesario.

    - En Windows, puede iniciarla con el acceso directo del escritorio **Machine Learning Workbench**.

    - En Mac OS, seleccione **Azure ML Workbench** en el Launchpad.

1.  En el panel **PROJECTS** (PROYECTOS), seleccione el signo más (+) y elija **New Project** (Nuevo proyecto).

    ![Nueva área de trabajo](media\azure-stack-solution-machine-learning\image14.png)

1.  Rellene los campos del formulario y seleccione el botón **Create** (Crear) para crear un nuevo proyecto en Workbench.

    | Campo | Valor recomendado para el tutorial | DESCRIPCIÓN |
    |-------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Nombre de proyecto | myIris | Escriba un nombre único que identifique la cuenta. Use el nombre de departamento o proyecto que mejor identifique el experimento. El nombre debe tener entre 2 y 32 caracteres. Debe incluir solo caracteres alfanuméricos y el carácter de guión (-). |
    | Directorio del proyecto | c:\Temp\ | Especifique el directorio en el que se creará el proyecto. |
    | Descripción del proyecto | déjelo en blanco | Campo opcional útil para describir los proyectos. |
    | Dirección URL de repositorio de GIT de VisualStudio.com | déjelo en blanco | Campo opcional. Asocie un proyecto con un repositorio de Git en Visual Studio Team Services con vistas al control de código fuente y la colaboración. [Más información sobre cómo configurar un repositorio](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project). |
    | Área de trabajo seleccionada | IrisGarden (si existe) | Elija el área de trabajo que creó para la cuenta de Experimentación en Azure Portal. <br>Mediante la opción de inicio rápido, se muestra el área de trabajo con el nombre IrisGarden. De lo contrario, use el área de trabajo con el nombre de la cuenta de Experimentación, o un nombre de cuenta de su elección. |
    | Plantilla de proyecto | Clasificación de iris | Las plantillas contienen scripts y datos usados para explorar el producto. Esta plantilla contiene los scripts y los datos necesarios para esta guía de inicio rápido y otros tutoriales de este sitio de documentación. |

    ![Nuevo proyecto](media\azure-stack-solution-machine-learning\image15.png)

1.  Se crea un nuevo proyecto y se abre el panel de proyecto con ese proyecto. Explore la página principal, los orígenes de datos, los cuadernos y los archivos de código fuente del proyecto.

    ![Abrir proyecto](media\azure-stack-solution-machine-learning\image16.png)

### <a name="attach-a-dsvm-compute-target"></a>Adjuntar un destino de procesos de DSVM

Una vez creada la DSVM, asóciela al proyecto de Azure ML.

1.  En la aplicación Azure ML Workbench, seleccione **File**->**Open PowerShell** (Archivo > Abrir PowerShell) para iniciar la CLI de Azure ML Workbench.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image17.png)

1.  Una vez que se ha abierto el símbolo del sistema de PowerShell, use el siguiente comando:

    ```PowerShell  
        az login
    ```

1.  Recibirá el siguiente aviso:

     ![Texto alternativo](media\azure-stack-solution-machine-learning\image18.png)

1.  Vaya al sitio como se detalla en el símbolo del sistema y escriba el código que se proporciona.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image19.png)

1.  Seleccione Continue (Continuar) cuando se le solicite y, luego, seleccione la cuenta de Azure a la que está asociada la cuenta de Experimentación de Azure ML.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image20.png)

1.  La CLI de Azure ML Workbench envía entonces el siguiente símbolo del sistema:

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image21.png)

1.  Cuando el inicio de sesión de cuenta de ML y del área de trabajo se muestre como correcto, asocie la DSVM.

    ```PowerShell  
        az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password>
    ```

    Aparece la siguiente notificación:

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image22.png)

    ```PowerShell  
        # prepare the Docker image on the DSVM 
        az ml experiment prepare -c <compute target name>
    ```

Este proceso tarda algún tiempo y genera una cantidad considerable de texto a medida que se extraen varias imágenes de Docker, se registran y, luego, se les aplica el código y las aplicaciones necesarios.

Ahora se pueden ejecutar los experimentos en esta DSVM.

### <a name="create-a-data-preparation-package"></a>Generar un paquete de preparación de datos.

A continuación, comience a preparar los datos en Azure Machine Learning Workbench. Todas las transformaciones que se realizan en Workbench se almacenan en formato JSON en un paquete de preparación de datos local (archivo \*.dprep). Este paquete de preparación de datos es el contenedor principal del trabajo de preparación de datos en Workbench.

Este paquete de preparación de datos se puede entregar posteriormente a un entorno de ejecución, por ejemplo, local-C\#/CoreCLR, Scala/Spark o Scala/HDI.

1.  Seleccione el icono de carpeta para abrir la vista Files (Archivos) y seleccione **iris.csv** para abrirlo.

    Este archivo contiene una tabla con 5 columnas y 50 filas. Las cuatro columnas son columnas de características numéricas. La quinta columna es una columna de destino de cadena. Ninguna de las columnas tienen nombres de encabezado.

    ![iris.csv](media\azure-stack-solution-machine-learning\image23.png)

1.  En la **Data View** (Vista de datos), seleccione el signo más (**+**) para agregar un nuevo origen de datos. Se abre la página **Add Data Source** (Agregar origen de datos).

    ![Vista de datos en Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image24.png)

1.  Seleccione **Text Files(\*.csv, \*.json, \*.txt., ...)** (Archivos de texto [.csv, .json, .txt,...]).

    ![Origen de datos en Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image25.png)

1.  Seleccione **Next** (Siguiente).

2.  Vaya al archivo **iris.csv** y haga clic en **Finish** (Finalizar). Se usarán los valores predeterminados para los parámetros como el separador y los tipos de datos.

    > [!Important]  
    > Seleccione el archivo **iris.csv** desde el directorio del proyecto actual de este ejercicio. De lo contrario, los pasos posteriores podrían dar error.

    ![Selección de Iris](media\azure-stack-solution-machine-learning\image26.png)

1.  Se crea un archivo llamado `*iris-1.dsource`. Se asigna al archivo un nombre único con `-1` porque el proyecto de ejemplo ya contiene un archivo **iris.dsource** sin numerar.

    Se abre el archivo y se muestran los datos. De forma automática, se agrega una serie de encabezados de columna, de **Column1** a **Column5**, a este conjunto de datos. Desplácese hasta el final y observe que la última fila del conjunto de datos está vacía. La fila está vacía porque hay un salto de línea adicional en el archivo CSV.

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image27.png)

1.  Seleccione el botón **Metrics** (Métricas). Se generan y se muestran histogramas.

    Vuelva a la vista de datos con el botón **Data** (Datos).

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image28.png)

1.  Observe los histogramas. Se ha calculado un conjunto completo de las estadísticas para cada columna.

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image29.png)

1.  Empiece a crear un paquete de preparación de datos, para lo que debe seleccionar el botón **Prepare** (Preparar). Se abre el cuadro de diálogo **Prepare** (Preparar).

    El proyecto de ejemplo contiene un archivo de preparación de datos, **iris.dprep**, que está seleccionado de forma predeterminada.

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image30.png)

1.  Cree un archivo de preparación de datos; para ello, seleccione **+ New Data Preparation Package** (+Nuevo paquete de preparación de datos) en el menú.

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image31.png)

1.  Escriba un valor nuevo como nombre del paquete (use **iris-1**) y, luego, pulse **Aceptar**.

    Se crea un paquete de preparación de datos denominado **iris-1.dprep** y se abre en el editor de preparación de datos.

    ![Vista de datos de Iris](media\azure-stack-solution-machine-learning\image32.png)

    A continuación, es necesario preparar los datos.

1.  Seleccione cada encabezado de columna para poder modificar su texto. Después, cambie el nombre de cada columna como se indica a continuación:

    Por orden, escriba **Sepal Length** (Longitud del sépalo), **Sepal Width** (Anchura del sépalo), **Petal Length** (Longitud del pétalo), **Petal Width** (Anchura del pétalo) y **Species** (Especies) para las cinco columnas respectivamente.

    ![Cambio del nombre de las columnas](media\azure-stack-solution-machine-learning\image33.png)

1.  Recuente los distintos valores:

    1.  Seleccione la columna **Species** (Especie).

    2.  Haga clic con el botón derecho para seleccionarla.

    3.  Seleccione **Value Counts** (Recuentos de valores) en el menú.

        Se abre el panel **Inspectores** debajo de los datos. Aparece un histograma con cuatro barras. La columna de destino tiene cuatro valores distintos: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** y un valor **(null)**.

    ![Selección de Value Counts (Recuentos de valores)](media\azure-stack-solution-machine-learning\image34.png)

    ![Histograma de recuento de valores](media\azure-stack-solution-machine-learning\image35.png)

1.  Para filtrar los valores null, seleccione la barra "(null)" y después seleccione el signo menos (**-**).

    Luego, la fila (null) pasa a ser de color gris para indicar que se filtró.

    ![Filtrar valores null](media\azure-stack-solution-machine-learning\image36.png)

1.  Fíjese en los pasos de la preparación de datos individuales que se detallan en el panel **STEPS** (PASOS). Cuando se cambia el nombre de las columnas y se filtran las filas con valores null, cada acción se registra como un paso de preparación de datos. Edite los pasos individuales para ajustar su configuración: puede reordenarlos o eliminarlos.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image37.png)

1.  Cierre el editor de preparación de datos. Seleccione el icono de la **x** en la pestaña **iris-1** con el icono del gráfico para cerrar la pestaña. El trabajo se guarda automáticamente en el archivo **iris-1.dprep** que aparece bajo el encabezado **Data Preparations** (Preparaciones de datos).

    ![cierre](media\azure-stack-solution-machine-learning\image38.png)

### <a name="generate-python-code-to-invoke-a-data-preparation-package"></a>Generación de código de Python para invocar un paquete de preparación de datos

La salida de un paquete de preparación de datos se puede explorar directamente en Python o en Jupyter Notebook. Los paquetes se puede ejecutar en varios runtimes, entre los que se incluyen Python, Spark (incluido en Docker) y HDInsight locales.

1.  Busque el archivo **1.dprep iris** en la pestaña Data Preparations (Preparaciones de datos).

2.  Haga clic con el botón derecho en el archivo **iris-1.dprep** y seleccione **Generate Data Access Code File** (Generar archivo de código de acceso a datos) en el menú contextual.

    ![Generación de código](media\azure-stack-solution-machine-learning\image39.png)

    Se abre un nuevo archivo llamado **iris-1.py** con las siguientes líneas de código para invocar la lógica creada como un paquete de preparación de datos:

    ```Python
    # Use the Azure Machine Learning data preparation package
    from azureml.dataprep import package

    # Use the Azure Machine Learning data collector to log various metrics
    from azureml.logging import get_azureml_logger
    logger = get_azureml_logger()

    # This call will load the referenced package and return a DataFrame.
    # If run in a PySpark environment, this call returns a
    # Spark DataFrame. If not, it will return a Pandas DataFrame.
    df = package.run('iris-1.dprep', dataflow_idx=0)
    # Remove this line and add code that uses the DataFrame
    df.head(10)
    ```

    En función del contexto en el que se ejecuta este código, representa un tipo de DataFrame diferente:

    -  Cuando se ejecuta en un runtime de Python, se usa un [DataFrame de pandas](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).

    -  Cuando se ejecuta en un runtime de Spark, se usa un [DataFrame de Spark](https://spark.apache.org/docs/latest/sql-programming-guide.html).

### <a name="review-irissklearnpy-and-the-configuration-files"></a>Revisión de iris_sklearn.py y los archivos de configuración

1.  En el proyecto abierto, haga clic en el botón **Files** (Archivos) (el icono de la carpeta) del panel izquierdo para abrir la lista de archivos de la carpeta del proyecto.

    ![Abrir el proyecto de Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image40.png)

1.  Seleccione el archivo de script de Python **iris_sklearn.py**.

    ![Elección de un script](media\azure-stack-solution-machine-learning\image41.png)

    El código se abre en una nueva pestaña del editor de texto de Workbench.

    > [!Note]  
    > Es posible que el código mostrado no sea exactamente el mismo que el anterior, ya que este proyecto de ejemplo se actualiza con frecuencia.

    ![Abrir un archivo](media\azure-stack-solution-machine-learning\image42.png)

1.  Inspeccione el código de script de Python para familiarizarse con el estilo de codificación.

    El script **iris_sklearn.py** realiza las siguientes tareas:  

    -  Carga el paquete de preparación de datos predeterminado **iris.dprep** para crear un objeto [pandas.DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).

    -   Agrega características aleatorias para que el problema sea más difícil de resolver. Dicha aleatoriedad es necesaria porque Iris es un conjunto de datos pequeño que se clasifica fácilmente con casi un 100 % de precisión.

    -  Usa la biblioteca thescikit-learnmachine learning para compilar un modelo de regresión logística. Esta biblioteca incluye Azure Machine Learning Workbench de forma predeterminada.

    -  Serializa el modelo mediante el uso de la biblioteca [pickle](https://docs.python.org/3/library/pickle.html) en un archivo de la carpeta `outputs`.

    -  Carga el modelo serializado y, después, lo deserializa en la memoria.

    -  Usa el modelo deserializado para realizar una predicción en un nuevo registro.

    -  Traza dos gráficos, una matriz de confusión y una curva ROC (característica operativa del receptor) de varias clases, con la biblioteca [matplotlib](https://matplotlib.org/), y, después, los guarda en theoutputsfolder. Si aún no está instalada, instale esta biblioteca en el entorno.

    -  Traza automáticamente la tasa de regularización y la precisión del modelo en el historial de ejecución. El objeto `run_logger` se utiliza en todo el proceso para registrar la tasa de regularización y la precisión del modelo en los registros.

### <a name="run-irissklearnpy-in-the-local-environment"></a>Ejecución de iris_sklearn.py en el entorno local

1.  Inicie la interfaz de la línea de comandos (CLI) de Azure Machine Learning:

    1.  Inicie Azure Machine Learning Workbench.

    2.  En el menú de Workbench, seleccione **Archivo**> **Open Command Prompt** (Abrir símbolo del sistema).

    La ventana de la interfaz de la línea de comandos (CLI) de Azure Machine Learning se inicia en la carpeta del proyecto `C:\Temp\\myIris\` en Windows. Este proyecto es el mismo que el que creó en la parte 1 del tutorial.

    > [!Important]  
    > Use esta ventana de la CLI para realizar los pasos siguientes.

1.  En la ventana de la CLI, instale la biblioteca de trazado de Python, **matplotlib**, si aún no está instalada.

    El script **iris_sklearn.py** tiene dependencias en dos paquetes de Python: **scikit-learn** y **matplotlib**. Para su comodidad, Azure Machine Learning Workbench instala el paquete **scikit-learn**. Sin embargo, puede que no sea necesario instalar **matplotlib**.

    Aunque continúe sin haber instalado **matplotlib**, puede que el código de este tutorial se ejecute correctamente. Sin embargo, el código no puede generar la salida de la matriz de confusión ni los trazados de curva ROC de varias clases, tal como se muestra en las visualizaciones del historial.

    ```CLI
    pip install matplotlib
    python -m pip install --upgrade pip
    ```

    Esta instalación tarda aproximadamente un minuto.

1.  Vuelva a la aplicación Workbench.

2.  Busque la pestaña denominada **iris_sklearn.py**.

    ![Búsqueda de la pestaña con el script](media\azure-stack-solution-machine-learning\image43.png)

1.  En la barra de herramientas de esa pestaña, seleccione **local**, como entorno de ejecución, y iris_sklearn.pyas como el script que se va a ejecutar. Es posible que estos ya estén seleccionados.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image44.png)

1.  Vaya a la derecha de la barra de herramientas y escriba 0.01 en el campo **Arguments** (Argumentos).

    Este valor corresponde a la tasa de regularización del modelo de regresión logística.

    ![Elección de local y del script](media\azure-stack-solution-machine-learning\image45.png)

1.  Haga clic en el botón **Ejecutar**. Se programará un trabajo de forma inmediata. El trabajo se muestra en el panel **Trabajos** de la derecha de la ventana de Workbench.

    ![Elección de local y del script](media\azure-stack-solution-machine-learning\image46.png)

    Poco después, el estado del trabajo pasa de **Submitting** (Enviando) a **Running** (En ejecución) y luego a **Completed** (Completado).

1.  Seleccione **Completado** en el texto de estado del trabajo en el panel **Trabajos**.

    ![Ejecutar sklearn](media\azure-stack-solution-machine-learning\image47.png)

    Se abre una ventana emergente que muestra el texto de salida estándar (stdout) de la ejecución. Para cerrar el texto stdout, haga clic en el botón **Cerrar** (**x**) de la esquina superior derecha de la ventana emergente.

    ![Salida estándar](media\azure-stack-solution-machine-learning\image48.png)

1.  En el mismo estado del trabajo en el panel **Jobs** (Trabajos), seleccione el texto azul **iris_sklearn.py \[n\] **(* n* es el número de ejecución) que está inmediatamente encima del estado **Completed** (Completado) y la hora de inicio. Se abre la ventana **Run Properties** (Propiedades de ejecución), que muestra la siguiente información de esa ejecución concreta:

    -  Información de **Run Properties** (Propiedades de ejecución)

    -  **Outputs**

    -  **Métricas**

    -  **Visualizaciones**, en caso de haber

    -  **Registros**

    Cuando la ejecución finaliza, la ventana emergente muestra los resultados siguientes:

    > [!Note]  
    > Al haber introducido el tutorial cierta aleatoriedad en el conjunto de aprendizaje anterior, los resultados exactos pueden variar con respecto a los que se muestran aquí.

    ```Python  
        Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]

        Iris dataset shape: (150, 5)
        Regularization rate is 0.01
        LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
        Accuracy is 0.6792452830188679

        ==========================================
        Serialize and deserialize using the outputs folder.

        Export the model to model.pkl
        Import the model from model.pkl
        New sample: [[3.0, 3.6, 1.3, 0.25]]
        Predicted class is ['Iris-setosa']
        Plotting confusion matrix...
        Confusion matrix in text:
        [[50  0  0]
        [ 1 37 12]
        [ 0  4 46]]
        Confusion matrix plotted.
        Plotting ROC curve....

        ROC curve plotted.
        Confusion matrix and ROC curve plotted. See them in Run History details pane.

    ```

1.  Cierre la pestaña **Run Properties** (Propiedades de ejecución) y vuelve a la pestaña **iris_sklearn.py**.

1.  Repita las ejecuciones adicionales.

Escriba una serie de valores en el campo **Arguments** (Argumentos) que oscila entre `0.001` y `10`. Seleccione **Ejecutar** para ejecutar el código varias veces. El valor del argumento que cambia cada vez se introduce en el modelo de regresión logística del código, lo que produce resultados diferentes cada vez.

### <a name="review-the-run-history-in-detail"></a>Detalles de la revisión del historial

En Azure Machine Learning Workbench, cada ejecución de script se captura como un registro del historial de ejecución. Para ver el historial de ejecución de un script determinado, abra la vista **Runs** (Ejecuciones).

1.  Para ello, haga clic en el botón **Ejecuciones** (icono del reloj) de la barra de herramientas izquierda. Luego, seleccione **iris_sklearn.py** para mostrar el **panel de ejecuciones** de iris_sklearn.py.

    ![Vista de ejecución](media\azure-stack-solution-machine-learning\image49.png)

1.  Se abrirá la pestaña **Run Dashboard** (Panel de ejecución).

    Consulte las estadísticas que se han capturado durante varias ejecuciones. Los gráficos se representan en la parte superior de la pestaña. Cada ejecución tiene un número consecutivo y sus detalles se muestran en la tabla de la parte inferior de la pantalla.

    ![Panel de ejecución](media\azure-stack-solution-machine-learning\image50.png)

1.  Filtre la tabla y seleccione cualquiera de los gráficos para ver el estado, la duración, la precisión y la tasa de regularización de cada ejecución.

2.  Seleccione las casillas junto a dos o más ejecuciones en la tabla **Ejecuciones**. Seleccione el botón **Comparar** para abrir un panel de comparación detallado. Revise la comparación en paralelo.

3.  Para volver a **Run Dashboard** (Panel de ejecución), seleccione el botón para volver **Lista de ejecuciones** en la parte superior izquierda del panel **Comparison** (Comparación).

    ![Volver a Lista de ejecuciones](media\azure-stack-solution-machine-learning\image51.png)

1.  Seleccione una ejecución individual para ver la vista de detalles de la misma. Observe que las estadísticas de la ejecución seleccionada se enumeran en la sección **Run Properties** (Propiedades de ejecución). Los archivos escritos en la carpeta de salida se enumeran en sección **Outputs** (Salidas), de donde se pueden descargar.

    ![Detalles de ejecución](media\azure-stack-solution-machine-learning\image52.png)

Los dos trazados, la matriz de confusión y la curva ROC de varias clases, se representan en la sección **Visualizaciones**. Todos los archivos de registro también pueden encontrarse en la sección **Registros**.

### <a name="run-scripts-in-the-azure-machine-learning-ml-workbench-cli-window"></a>Ejecución de scripts en la ventana de la CLI de Azure Machine Learning (ML) Workbench

1.  Inicie la interfaz de la línea de comandos (CLI) de Azure Machine Learning:

    1.  Inicie Azure Machine Learning Workbench.

    2.  En el menú de Workbench, seleccione **Archivo** > **Open Command Prompt** (Abrir símbolo del sistema).

    Se inicia el símbolo del sistema de la CLI en la carpeta del proyecto `C:\\Temp\\myIris` en Windows. Este es el proyecto que creó en la parte 1 del tutorial.

    > [!Important]  
    > Use esta ventana de la CLI para realizar los pasos siguientes.

1.  En la ventana de la CLI, inicie sesión en Azure. [Más información acerca de az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    Si ya ha iniciado sesión, omita este paso.

1.  En el símbolo del sistema, escriba:

    ```CLI
        az login
    ```

    Este comando devuelve un código que se utiliza en el explorador en [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

1.  Vaya a [https://aka.ms/devicelogin](https://aka.ms/devicelogin) en el explorador. Escriba el código que recibió de la CLI.

    La aplicación Workbench y la CLI usan cachés de credenciales independientes al autenticarse con recursos de Azure. No es necesario autenticarse de nuevo hasta que expire el token en caché.

1.  Si la organización tiene varias suscripciones de Azure (entorno empresarial), establezca la que se va a usar. Busque la suscripción, establézcala mediante el identificador de suscripción y pruébela.

1.  Enumere cada suscripción de Azure a la que se ha accedido con este comando:

    ```CLI
        az account list -o table 
    ```

    El comando **az account list** devuelve la lista de suscripciones disponibles para el inicio de sesión. Si hay más de una, identifique el valor del identificador de suscripción de la suscripción usada.

1.  Establezca la suscripción de Azure usada como la cuenta predeterminada:

    ```CLI
        az account set -s <the-subscription-id
    ```

    Donde <the-subscription-id> es el valor de identificador de la suscripción usada. No incluya los corchetes.

1.  Confirme la nueva configuración de suscripción solicitando los detalles de la suscripción actual.

    ```CLI
        az account show
    ```

1.  En la ventana de la CLI, envíe el script **iris_sklearn.py** como experimento.

    La ejecución de iris_sklearn.py se realiza en el contexto de proceso local.

1.  En Windows:

    ```CLI
        az ml experiment submit -c local .\\iris_sklearn.py
    ```

1.  En macOS:

    ```CLI
        az ml experiment submit -c local iris_sklearn.py
    ```

    La salida debe ser similar a: 

    ```
        RunId: myIris_1521077190506

    Executing user inputs .....
    ===========================

    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]

    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679

    ==========================================
    Serialize and deserialize using the outputs folder.

    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.

    Execution Details
    =================
    RunId: myIris_1521077190506

    ```

6.  Revise el resultado. La salida y el resultado deben ser los mismos que cuando Workbench ejecuta el script.

7.  Vuelva a Workbench y:

    Seleccione el icono de carpeta en el panel izquierdo para enumerar los archivos del proyecto.  Abra el script de Python llamado **run.py**. Este script es útil para ejecutar varias tasas de regularización en bucle. 

    ![Volver a Lista de ejecuciones](media\azure-stack-solution-machine-learning\image53.png)

1.  Ejecute el experimento varias veces con estas tasas.

    Este script inicia ` aniris_sklearn.pyjob` con una tasa de regularización de `10.0` (un número demasiado grande). Luego, el script recorta la tasa por la mitad en la siguiente ejecución, y así sucesivamente hasta que la tasa no es menor que `0.005`. El script contiene el código siguiente:

    ![Volver a Lista de ejecuciones](media\azure-stack-solution-machine-learning\image54.png)

1.  Ejecute el script **run.py** desde la línea de comandos del modo siguiente:

    ```CLI
        python run.py
    ```
Este comando envía `iris_sklearn.py` varias veces con distintas tasas de regularización.

Cuando `run.py` finaliza, se pueden ver gráficos de distintas métricas en la vista de lista del historial de ejecución del área de trabajo.

### <a name="run-scripts-in-an-ubuntu-based-data-science-virtual-machine-dsvm-on-azure"></a>Ejecución de scripts en una instancia de Data Science Virtual Machine (DSVM) Ubuntu en Azure

Para ejecutar el script en un contenedor de Docker de una máquina remota de Linux, se necesita acceso de SSH (nombre de usuario y contraseña) para ejecutar esa máquina remota. Además, dicha máquina debe tener instalado un motor de Docker y este debe de estar en ejecución.

1.  Edite el archivo runconfig generado<your DSVM Name> en aml_config y cambie el marco de trabajo predeterminado valuePySparktoPython:

    ```yaml  
    Framework: Python
    ```
1.  Emita el mismo comando que antes en la ventana de la CLI, con el destino *<DSVM>*, pero esta vez ejecute iris_sklearn.py en un contenedor de Docker remoto: (sustituya <DSVM> por el nombre de la instancia de Data Science VM, sin los corchetes).

    ```CLI
        az ml experiment submit -c <DSVM> iris_sklearn.py
    ```

El comando se ejecuta como si estuviera en adocker-pythonenvironment, salvo que la ejecución se produce en la máquina virtual Linux remota. La ventana de CLI muestra la misma información de salida.

### <a name="download-the-model-pickle-file"></a>Descarga del archivo pickle de modelo

En la parte anterior del tutorial, el script **iris_sklearn.py** se ejecutó localmente en Machine Learning Workbench. Esta acción serializó el modelo de regresión logística con el popular paquete de serialización de objetos de Python [pickle](https://docs.python.org/3/library/pickle.html).

1.  Abra la aplicación Machine Learning Workbench A continuación, abra el proyecto **myIris** creado en las partes anteriores de la serie de tutoriales.

2.  Una vez abierto el proyecto, haga clic en el botón **Files** (Archivos) (icono de carpeta), en el panel izquierdo, para abrir la lista de archivos de la carpeta del proyecto.

3.  Seleccione el archivo **iris_sklearn.py**. El código Python se abre en una nueva pestaña del editor de texto de Workbench.

4.  Revise el archivo **iris_sklearn.py** para ver dónde se generó el archivo pickle. Seleccione Ctrl+F para abrir el cuadro de diálogo **Buscar** y, a continuación, busque la palabra **pickle** en el código Python.

Este fragmento de código muestra cómo se generó el archivo de salida de pickle. El archivo pickle de salida se denomina **model.pkl** en el disco.

    ```Python
        print("Export the model to model.pkl")
        f = open('./outputs/model.pkl', 'wb')
        pickle.dump(clf1, f)
        f.close()

    ```

1.  Busque el archivo pickle del modelo en los archivos de salida de una ejecución anterior.

    Cuando ejecutó el script **iris_sklearn.py**, el archivo de modelo se escribió en la carpeta **outputs** llamada **model.pkl**. Esta carpeta reside en el entorno de ejecución elegido para ejecutar el script, y no en la carpeta del proyecto local. 

    1. Para buscar el archivo, seleccione el botón **Runs** (Ejecuciones) (icono de reloj), en el panel izquierdo, para abrir la lista **All Runs** (Todas las ejecuciones).  

    2. Se abre la pestaña **All Runs** (Todas las ejecuciones). En la tabla de ejecuciones, seleccione una de las ejecuciones recientes cuyo destino era **local** y cuyo nombre de script era **iris_sklearn.py**.  

    3. Se abre la página **Run Properties** (Propiedades de ejecución). En la sección superior derecha del panel, observe la sección **Outputs** (Salidas). d\. Para descargar el archivo pickle, active la casilla junto al archivo **model.pkl** y, luego, seleccione **Download** (Descargar). Guarde el archivo en la raíz de la carpeta del proyecto. Lo necesitará en los próximos pasos.  

    ![Descarga del archivo pickle](media\azure-stack-solution-machine-learning\image55.png)

### <a name="get-scoring-script-and-schema-files"></a>Obtención de los archivos de script de puntuación y esquema

Para implementar el servicio web junto con el archivo de modelo, se necesita un script de puntuación. Opcionalmente, se necesita un esquema para los datos de entrada del servicio web. El script de puntuación carga el archivo **model.pkl** de la carpeta actual y lo usa para generar nuevas predicciones.

1.  Abra la aplicación Machine Learning Workbench y abra el proyecto **myIris** que ha creado en la parte anterior de la serie de tutoriales.

2.  Una vez abierto el proyecto, haga clic en el botón **Files** (Archivos) (icono de carpeta), en el panel izquierdo, para abrir la lista de archivos de la carpeta del proyecto.

3.  Seleccione el archivo **score_iris.py**. Se abre el script de Python. Este archivo se usa como el archivo de puntuación.

    ![Archivo de puntuación](media\azure-stack-solution-machine-learning\image56.png)

1.  Para obtener el archivo de esquema, ejecute el script. Seleccione el entorno **local** y el script **score_iris.py** en la barra de comandos y, luego, seleccione **Run** (Ejecutar).

    Este script crea un archivo JSON en la carpeta **Outputs** (Salidas), que captura el esquema de datos de entrada requerido por el modelo.

1.  Observe el panel **Jobs** (Trabajos) en el lado derecho de **Project Dashboard** (Panel del proyecto). Espere a que el último trabajo denominado **score_iris.py** aparezca con el estado **Completed** (Completado) en verde. A continuación, seleccione el hipervínculo **score_iris.py** correspondiente a la última ejecución del trabajo para ver los detalles de dicha ejecución.

2.  En la página **Run Properties** (Propiedades de ejecución), en la sección **Salidas**, seleccione el archivo recién creado **service_schema.json**. Active la casilla situada junto al nombre de archivo y, a continuación, seleccione **Download** (Descargar). Guarde el archivo en la carpeta raíz del proyecto.

3.  Vuelva a la pestaña anterior y al script **score_iris.py**. Mediante la recopilación de datos, se pueden capturar entradas y predicciones de modelos del servicio web. Los pasos siguientes son de interés para la recopilación de datos.

4.  Revise el código en la parte superior del archivo, que importa la clase **ModelDataCollector**. Contiene la funcionalidad de recopilación de datos de modelo:

    ```Python  
        from azureml.datacollector import ModelDataCollector
    ```

1.  Revise las siguientes líneas de código de la función **init()**, que crea instancias de **ModelDataCollector**:

    ```Python  
        global inputs_dc, prediction_dc
        inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
        prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1.  Revise las siguientes líneas de código de la función **run(input_df)**, que recopila los datos de entrada y predicción:

    ```Python  
        inputs_dc.collect(input_df)
        prediction_dc.collect(pred)
    ```

Ahora, prepare el entorno para operacionalizar el modelo.

## <a name="step-5-deploy-and-use-azure-container-registry"></a>Paso 5: Implementación y uso de Azure Container Registry

Implemente y use Azure Container Registry.

Cree una instancia de Azure Container Registry con el comando **az acr create**. El nombre del registro debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. El grupo de recursos es el mismo.

    ```CLI
        az acr create --resource-group <ResourceGroup> --name  <acrName> --sku Basic
    ```

### <a name="container-registry-login"></a>Inicio de sesión en Container Registry

Después, use el comando **az acr login** para iniciar sesión en la instancia de Azure Container Registry. Proporcione el nombre único que se especificó para el registro de contenedor cuando se creó.

    ```CLI
        az acr login --name <acrName>
    ```

Al finalizar, el comando devuelve un mensaje que indica que el inicio de sesión se ha realizado correctamente.

### <a name="prepare-to-operationalize-locally-for-development-and-testing-the-service"></a>Preparación para la operacionalización local: desarrollo y prueba del servicio

Use la implementación en *modo local* para la ejecución en contenedores de Docker en el equipo local y para desarrollo y pruebas.

El motor de Docker debe ejecutarse localmente para completar los pasos siguientes y poner el modelo en funcionamiento. Use la marca `-h` al final de cada comando para mostrar el mensaje de ayuda correspondiente.

    > [!Note]  
    > If Docker engine is not locally available, proceed by creating a cluster in Azure for deployment and keep the cluster for re-use, or delete it after the tutorial to avoid ongoing charges.

    > [!Note]  
    > Web services deployed locally do not appear in Azure Portal's list of services. They will be running in Docker on the local machine.

1.  Abra la interfaz de la línea de comandos (CLI). En la aplicación Machine Learning Workbench, en el menú **File** (Archivo), seleccione **Open Command Prompt** (Abrir símbolo del sistema).

    Se abre el símbolo del sistema en la ubicación actual de la carpeta del proyecto **c:\\temp\\myIris**.

1.  Asegúrese de que el proveedor de recursos de Azure **Microsoft.ContainerRegistry** esté registrado en la suscripción. Registre este proveedor de recursos antes de crear un entorno en el paso 3. Compruebe si ya está registrado mediante el comando siguiente:

    ```CLI
        az provider list --query "\[\].{Provider:namespace, Status:registrationState}" --out table
    ```

    Vea esta salida:

    ```CLI
        Provider                                    Status 
        --------                                    --------
        Microsoft.Authorization                     Registered 
        Microsoft.ContainerRegistry                 Registered 
        microsoft.insights                          Registered 
        Microsoft.MachineLearningExperimentation    Registered 
    ```

    Si **Microsoft.ContainerRegistry** no está registrado, use el siguiente comando:

    ```CLI
    az provider register --namespace Microsoft.ContainerRegistry
    ```

    El registro puede tardar unos minutos. Compruebe el estado de registro con el comando **az provider list** anterior o el siguiente comando:

    ```CLI
    az provider show -n Microsoft.ContainerRegistry
    ```

    La tercera línea de la salida muestra **"registrationState": "Registering"** ("registrationState": "Registrando"). Espere unos minutos y repita el comando **show**, hasta que la salida muestre **"registrationState": "Registered"** ("registrationState": "Registrado").

1.  Cree el entorno. Ejecute este paso una vez por cada entorno.

    El siguiente comando de configuración requiere acceso de colaborador a la suscripción. Se requiere acceso de colaborador a la implementación del grupo de recursos. Especifique el nombre del grupo de recursos como parte del comando de configuración mediante la marca- g.

    ```CLI
    az ml env setup -n <new deployment environment name> --location <e.g. eastus2> -g <existing resource group name>
    ```

    Siga las instrucciones en pantalla para aprovisionar una cuenta de almacenamiento para guardar las imágenes de Docker, una instancia de Azure Container Registry para enumerar imágenes de Docker y una cuenta de Azure Application Insights para recopilar la telemetría. **Si usa el modificador -c, el comando creará además un clúster de servicio de contenedor**.

    El nombre del clúster es una forma de identificar el entorno. La ubicación debe ser la misma que la de la cuenta de Administración de modelos que creó en Azure Portal.

    Para asegurarse de que dicho entorno se configura correctamente, use el siguiente comando para comprobar el estado:

    ```CLI
    az ml env show -n <deployment environment name> -g <existing resource group name>
    ```

    Asegúrese de que el estado de aprovisionamiento sea correcto, tal y como se muestra, antes de configurar el entorno en el paso 5:

    ![Estado de aprovisionamiento](media\azure-stack-solution-machine-learning\image57.png)

1.  Configure el entorno.

    Una vez finalizada la instalación, use el siguiente comando para establecer las variables de entorno necesarias para hacer funcionar el entorno. Use el mismo nombre de entorno que usó anteriormente en el paso 3. Use el mismo nombre de grupo de recursos que obtuvo en la ventana de comandos cuando se completó el proceso de instalación.

    ```CLI
        az ml env set -n <deployment environment name> -g <existing resource group name>
    ```

1.  Para comprobar la correcta configuración de su entorno operacionalizado para la implementación del servicio web local, escriba el siguiente comando:

    ```CLI
    az ml env show
    ```

    Ahora, cree el servicio web en tiempo real.

    > [!Note]  
    > Reutilice la cuenta y el entorno de Administración de modelos en las implementaciones sucesivas de servicios web. No es necesario crearlos para cada servicio web. Una cuenta o un entorno pueden tener asociados varios servicios web.

### <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Creación de un servicio web en tiempo real mediante comandos independientes

Como alternativa al comando **az ml service create realtime** mostrado anteriormente, estos pasos también se pueden realizar por separado.

En primer lugar, registre el modelo. A continuación, genere el manifiesto, compile la imagen de Docker y cree el servicio web. Este enfoque paso a paso le proporciona mayor flexibilidad en cada paso. Además, se pueden reutilizar las entidades generadas en pasos anteriores.

1. Proporcione el nombre del archivo pickle para registrar el modelo.

    ```CLI
    az ml model register --model model.pkl --name model.pkl
    ```

    Este comando genera un identificador de modelo.

2.  Cree un manifiesto.

    Para crear un manifiesto, use el siguiente comando y proporcione el identificador de modelo del paso anterior: 
    
    ```CLI
    az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
    ```

    Este comando genera un identificador de manifiesto.

3.  Cree una imagen de Docker.

    Para crear una imagen de Docker, use el siguiente comando y especifique la salida del valor del identificador de manifiesto del paso anterior. También se pueden usar las dependencias de Conda mediante el modificador `-c`. 
    
    ```CLI
    az ml image create -n irisimage --manifest-id <manifest ID> -c aml_config\conda_dependencies.yml
    ```
    
    Este comando genera un identificador de imagen de Docker.

2.  Cree el servicio.

    Para crear un servicio, use el siguiente comando y proporcione el valor del identificador de imagen del paso anterior: 
    
    ```CLI
    az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
    ```
    
    Este comando genera un identificador de servicio web.
    
    A continuación, ejecute el servicio web.

## <a name="step-6-deploy-a-kubernetes-cluster-to-azure-stack"></a>Paso 6: Implementación de un clúster de Kubernetes en Azure Stack

Implemente un clúster de Kubernetes en Azure Stack.

Se puede instalar Kubernetes mediante plantillas de Azure Resource Manager generadas por el motor de Azure Container Service (ACS) en Azure Stack. [*Kubernetes*](https://kubernetes.io/) es un sistema de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores. Un [*contenedor*](https://www.docker.com/what-container) se encuentra en una imagen, similar a una máquina virtual. A diferencia de una máquina virtual, la imagen de contenedor incluye los recursos necesarios para ejecutar una aplicación, como el código, el tiempo de ejecución para ejecutar el código, bibliotecas específicas y la configuración.

Use Kubernetes para:

 -  Desarrollar aplicaciones escalables de forma masiva y actualizables que se pueden implementar en segundos.

 -  Simplificar el diseño de la aplicación y mejorar la confiabilidad mediante distintas aplicaciones de Helm. [*Helm*](https://github.com/kubernetes/helm) es una herramienta de empaquetado de código abierto que le ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes.

 -  Supervisar y diagnosticar fácilmente el estado de las aplicaciones con la funcionalidad de escalado y actualización.

> [!Note]  
> La instalación del clúster tardará aproximadamente una hora, así que haga sus planes teniendo esto en cuenta.

### <a name="prerequisites-for-kubernetes-cluster-deployment"></a>Requisitos previos para la implementación del clúster de Kubernetes

Para comenzar, confirme los permisos y la preparación de Azure Stack:

1.  Valide que el elemento **Create Kubernetes Cluster (Preview)** (Crear clúster de Kubernetes [versión preliminar]) está disponible en Marketplace de Azure Stack. Si falta este elemento, hable con un operador de Azure Stack para que le agregue este elemento al entorno de Azure Stack.

2.  Compruebe que puede crear aplicaciones en el inquilino de Azure Active Directory (Azure AD). Se necesitan permisos para la implementación de Kubernetes.

    Para instrucciones sobre cómo comprobar sus permisos, consulte [*Comprobación de los permisos de Azure Active Directory*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

3.  Genere un par de claves SSH pública y privada para iniciar sesión en la máquina virtual de Linux en Azure Stack. Se necesita la clave pública al crear el clúster. Para instrucciones, consulte [Generación de una clave de autenticación para SSH](#generate-an-authenticatio-key-for-ssh).

4.  Compruebe que la suscripción en el portal de inquilinos de Azure Stack es válida y que hay suficientes direcciones IP públicas disponibles para agregar nuevas aplicaciones.

    No se puede implementar el clúster en una suscripción de **Administrador** de Azure Stack. Use una suscripción de **Usuario**.

###  <a name="generate-an-authentication-key-for-ssh"></a>Generación de una clave de autenticación para SSH

Desde la sesión del subsistema Windows para Linux, use los siguientes comandos para generar una clave de autenticación: 

1. Escriba: 

    ```Bash  
    ssh-keygen -t rsa
    ```
    
2. Cuando se le solicite, seleccione `id_rsa`. 
3. Cuando se le solicite, cree una frase de contraseña. Es importante anotar esta contraseña porque la usará más adelante. 
    La salida será similar a esta: 
    
    ```Bash  
    Your identification has been saved in `id_rsa`.
    Your public key has been saved in `id_rsa.pub`. 
    The key fingerprint is: SHA256:lUtUUjzaqWqGeolEPKeBmsnrhcNGM9Dn2OxYatt05SE  <user>@<machine-name>
    The key's randomart image is:  
    ```
    ![Texto alternativo](media\azure-stack-solution-machine-learning\image58.png)

4. Después de generar la clave, pegue la información de clave con los siguientes comandos: 
    ```Bash
    cat id_rsa.pub
    ```
    La salida será similar a esta: 
    ```Bash
    ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQDHaWrAktR3BlQ356T8Qvv8O2Q/U/hsXQwS9xJbuduuc9lkJwddzgmNCyM9PooZWYtGVXyHU6SC3YH1XkwqGtKhtPb03d24hmhX1euAaqIqHHSp4WgUS5s1gNsp37L8QCSGNCnF31FWavI8bnjOjccUkMowKl8iyGN++5UyQgNuynEVUbFJjrntoDL66HUu88xDxTcVB7rqDr2QKFwYJkg4HSoHYpezJd7W8kcmv33eh0xs8nlDA7Dzu7zXpyVc54bH9XtPR6EUXaQa+UqKaNlQNiJqEs+1X/zNuK9V6NLVNiO0h3jCHI3K8o4VnZyRKHCQProasiiPLUUJ6SF/RTLN  <user>@<machine-name>
    ```
    
5. Copie la clave generada en el campo de clave pública de SSH.

### <a name="create-a-service-principal-in-azure-ad"></a>Creación de una entidad de servicio en Azure AD

1.  Inicie sesión en [*Azure Portal*](http://www.poartal.azure.com/) global.

2.  Inicie sesión con el inquilino de Azure AD asociado a la instancia de Azure Stack.

3.  Crear una aplicación de Azure.

    1. Seleccione **Azure Active Directory** > **+ Registros de aplicaciones**> **Nuevo registro de aplicaciones**.
    2. Escriba un **Nombre** para la aplicación. 
    3. Seleccione **Aplicación web/API**. 
    4. Escriba `http://localhost` para **URL de inicio de sesión**. 
    5. Seleccione **Crear**.

1.  Anote el **Id. de la aplicación**. Este identificador es necesario al crear el clúster y se hace referencia a él como **identificador de cliente de entidad de servicio**.

2.  Seleccione **Configuración** > **Claves**.

    1. Escriba la **Descripción**. 
    2. Seleccione **Never expires** (Nunca expira) para **Expira**. 
    3. Seleccione **Guardar**. Tome nota de la cadena de clave. La cadena de clave es necesaria al crear el clúster y se hace referencia a ella como **secreto de cliente de entidad de servicio**.

### <a name="give-the-service-principal-access"></a>Dar acceso a la entidad de servicio

Asigne a la entidad de servicio acceso a la suscripción para que pueda crear recursos.

1.  Inicie sesión en el [portal de administración](https://adminportal.local.azurestack.external/).

2.  Seleccione **Más servicios** > **Suscripciones de usuario** > **+ Agregar**.

3.  Seleccione la suscripción creada.

4.  Seleccione **Control de acceso (IAM)** > seleccione **+ Agregar**.

5.  Seleccione el rol **Propietario**.

6.  Seleccione el nombre de aplicación creado para la entidad de servicio. Si es necesario, escriba el nombre en el cuadro de búsqueda.

7.  Seleccione **Guardar**.

8.  Abra el [portal de Azure Stack](https://portal.local.azurestack.external).

9.  Seleccione **+ Nuevo** > **Proceso** > **Clúster de Kubernetes**. Seleccione **Crear**.

    ![Implementar plantilla de solución](media\azure-stack-solution-machine-learning\image59.png)

10\. Seleccione **Conceptos básicos** en Creación de un clúster de Kubernetes.

    ![Deploy Solution Template](media\azure-stack-solution-machine-learning\image60.png)

11. Escriba el **nombre de usuario del administrador de la máquina virtual Linux**. Nombre de usuario de las máquinas virtuales de Linux que forman parte del clúster de Kubernetes y DVM.

12. Escriba la **clave pública SSH** utilizada para la autorización en todas las máquinas Linux creadas como parte del clúster de Kubernetes y DVM.

13. Escriba el **prefijo del DNS del perfil maestro** que es único para la región. Debe ser un nombre único para la región, como `ask8s-12345`. Como procedimiento recomendado, intente elegirlo de modo que sea el mismo que el nombre del grupo de recursos.

    > [!Note]  
    > Para cada clúster, use un prefijo de DNS del perfil principal nuevo y único.

14. Escriba el **recuento de perfiles del grupo de agentes**. El recuento contiene el número de agentes en el clúster. Puede haber de 1 a 4.

15. Escriba el **id. de cliente de la entidad de servicio**. El proveedor de nube de Azure Kubernetes usa este identificador.

16. Escriba el **secreto de cliente de la entidad de servicio** que creó al crear la aplicación de la entidad de servicio.

17. Escriba la **versión del proveedor de nube de Azure Kubernetes**. Esta es la versión para el proveedor de Kubernetes Azure. Azure Stack publica una compilación de Kubernetes personalizada para cada versión de Azure Stack.

18. Seleccione el **identificador de la suscripción**.

19. Escriba el nombre del nuevo grupo de recursos o seleccione uno existente. El nombre del recurso debe ser alfanumérico y estar en minúsculas.

20. Seleccione la **ubicación** del grupo de recursos. Esta es la región que eligió para la instalación de Azure Stack.

### <a name="specify-the-azure-stack-settings"></a>Especifique la configuración de Azure Stack

1.  Seleccione la **configuración de marca de Azure Stack**.

    ![Implementar plantilla de solución](media\azure-stack-solution-machine-learning\image61.png)

2.  Escriba el **punto de conexión de inquilino de Azure Resource Manager**. Este es el punto de conexión de Azure Resource Manager para conectarse para crear el grupo de recursos para el clúster de Kubernetes. En sistemas integrados se requiere el punto de conexión del operador de Azure Stack. Para obtener el Kit de desarrollo de Azure Stack (ASDK), visite `https://management.local.azurestack.external`.

3.  Escriba el **identificador de inquilino** de este. Consulte [*Obtención del identificador de inquilino*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) para encontrar el valor del identificador de inquilino.

### <a name="install-kubectl-on-windows-powershell-environment"></a>Instalación de kubectl en el entorno de Windows PowerShell

Desde el entorno de WSL, ejecute los comandos siguientes para instalar kubectl en el entorno de WSL.

```PowerShell  
Install-script -name install-kubectl -scope CurrentUser -force
Install-kubectl.ps1 -downloadlocation “C:\Users\<Current User>\Documents\Kube
```

### <a name="install-kubectl-on-the-windows-subsystem-for-linux-environment"></a>Instalación de kubectl en el entorno del subsistema Windows para Linux

Desde el entorno de WSL, ejecute los comandos siguientes para instalar kubectl en el entorno de WSL.

```Bash  
    apt-get update && apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb http://apt.kubernetes.io/ kubernetes-xenial main
    EOF
    apt-get update
    apt-get install -y kubectl
```

### <a name="configure-kubectl"></a>Configuración de kubectl

Para que kubectl encuentre el clúster de Kubernetes y pueda acceder a él, se necesita un *archivo kubeconfig*. Este archivo se crea automáticamente cuando se crea un clúster mediante kube-up.sh o al implementar un clúster de Minikube. Consulte las [*guías de introducción*](https://kubernetes.io/docs/setup/) para más información sobre la creación de clústeres. Para acceder a un clúster creado por otro usuario, consulte el [*documento de uso compartido de acceso al clúster*](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). De forma predeterminada, la configuración de kubectl se encuentra en **~.kube/config**.

### <a name="check-the-kubectl-configuration"></a>Comprobación de la configuración de kubectl

Obtenga el estado del clúster para comprobar que kubectl está configurado correctamente:

```Bash  
kubectl cluster-info
```

kubectl está configurado correctamente para acceder al clúster cuando se muestra la respuesta de la dirección URL.

kubectl no está configurado correctamente o no puede conectarse a un clúster de Kubernetes si se muestra el siguiente mensaje:

```Bash  
The connection to the server <server-name:port> was refused -  did you specify the right host or port?
```

Por ejemplo, cuando se ejecuta un clúster de Kubernetes en un equipo portátil local, puede ser necesaria una herramienta (minikube o similar) para volver a ejecutar los comandos indicados anteriormente.

Si kubectl cluster-info devuelve la respuesta de la dirección URL pero el clúster sigue estando inaccesible, compruebe que la configuración es correcta mediante:

```Bash  
> kubectl cluster-info dump
```

### <a name="enable-shell-autocompletion"></a>Habilitación de la finalización automática del shell

kubectl incluye compatibilidad con la finalización automática, lo que permite que la habilitación del shell sea rápida y fácil.

kubectl genera el propio script de finalización y este es accesible desde el perfil.

### <a name="connect-to-the-kubernetes-cluster"></a>Conexión al clúster de Kubernetes

Para conectarse al clúster, se necesita el cliente de línea de comandos de Kubernetes (**kubectl**). En la [documentación de Azure Container Service](https://docs.microsoft.com/azure/container-service/dcos-swarm/), encontrará instrucciones sobre cómo conectarse al clúster y administrarlo.

Se puede usar cualquier cliente SSH para conectar el clúster de Kubernetes. Se recomienda el subsistema Windows para Linux (WSL). La máquina que se conecta al clúster de Kubernetes estará en el grupo de recursos creado para el clúster y se iniciará con el prefijo de la pila vmd edge-ml.

```Bash  
ssh <user>@vmd-edge-ml-stack.<FQDN of Kubernetes Machine in  Azure Stack>
```

Una vez conectado a la máquina de Kubernetes, ejecute la siguiente máquina para obtener el archivo de configuración de Kubernetes.

```Bash  
sudo find / -name \*kubeconfig\* -type f
```

La salida tendrá un aspecto similar a la siguiente:

```Bash  
/var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Copie la información de esta ruta de acceso de archivos y, luego, ejecute el siguiente comando y pegue la ruta de acceso de archivo de kubeconfig copiada anteriormente:

```Bash  
sudo cat  /var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

La salida será un gran bloque de texto, que es el contenido JSON sin procesar. Copie este texto de salida, pegue este código en un archivo de Visual Studio y, luego, guárdelo como un archivo JSON. El resultado es un archivo kubeconfig.json almacenado localmente. (guardar en el directorio /mnt/c/users/<current user>/documents/Kube como kubeconfig.json)

### <a name="configure-the-kubernetes-cluster"></a>Configuración del clúster de Kubernetes

Una vez obtenido el archivo JSON local, en una nueva sesión WSL, use los siguientes comandos para configurar el clúster.

```Bash  
    cd /mnt/c/users/<current user>/documents/Kube
    kubectl proxy
    kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
    kubectl proxy
    set KUBECONFIG=”/mnt/c/users/<current user>/documents/Kube/kubeconfig.json”
    kubectl.exe config view
```

Se definirá la configuración de Kubernetes (consulte la salida siguiente).

![Texto alternativo](media\azure-stack-solution-machine-learning\image62.png)

Inicie el servicio de proxy local:

```Bash  
kubectl proxy
```

Vaya a la interfaz de usuario del clúster de Kubernetes en la siguiente dirección: `https://localhost:8001`.

![Texto alternativo](media\azure-stack-solution-machine-learning\image63.png)

Ahora dispone de un lugar para implementar el contenedor y un contenedor que reside en la nube que puede ver en el entorno local.

![Texto alternativo](media\azure-stack-solution-machine-learning\image64.png)

Usando cualquier editor de código de su elección, personalice el archivo **iris_deployment.yaml** (ubicado en el directorio /*mnt/c/users/<current user>/documents/Kube*), así **webservicename** y los contenedores **Image** y **Name** coincidirán con la implementación.

![Texto alternativo](media\azure-stack-solution-machine-learning\image65.png)

Establezca el puerto del contenedor en **5001.**

![Texto alternativo](media\azure-stack-solution-machine-learning\image66.png)

Y, a continuación, cree el elemento **imagePullSecret**:

### <a name="create-a-secret-in-the-cluster-that-holds-the-authorization-token"></a>Creación de un secreto en el clúster que contiene el token de autorización

Un clúster de Kubernetes usa el secreto de tipo **docker-registry** para autenticarse con un registro de contenedor y extraer una imagen privada.

Cree este secreto y llámelo **azuremlcr**:

```Bash  
kubectl create secret docker-registry azuremlcr --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password= "<your-pword>" --docker-email=<your-email>
```

Busque:

- **<your-registry-server>** es el **servidor de inicio de sesión** de Azure Container Registry.
- **<your-name>** es el **nombre de usuario** de Azure Container Registry.
- **<your-pword>** es la **contraseña** de Azure Container Registry. Asegúrese de escribir la contraseña entre comillas.
- **<your-email>** es el usuario con acceso de lectura y escritura al contenedor.
- Busque esta información en **Azure Container** **Registry** en **Access Keys** (Claves de acceso).
- Las credenciales de Docker están ahora establecidas en el clúster como un secreto llamado **azuremlcr**.

Guarde el archivo **iris_deployment.yaml** (ubicado en el directorio /*/mnt/c/users/<current user>/documents/Kube*).

### <a name="create-the-kubernetes-container"></a>Creación del contenedor de Kubernetes

```Bash  
kubectl.exe create -f /mnt/c/users/<current  user>/documents/Kube/iris_deployment.yaml
```

    ![Alt text](media\azure-stack-solution-machine-learning\image67.png)

Compruebe el estado de la implementación:

```Bash  
Kubectl get deployments
```

    ![Alt text](media\azure-stack-solution-machine-learning\image68.png)

La implementación puede tardar algún tiempo.

### <a name="configure-visual-studio-team-services-to-deploy-automatically"></a>Configuración de Visual Studio Team Services para la implementación automática

#### <a name="create-a-team-project"></a>Creación de un proyecto de equipo

1.  Asegúrese de la [pertenencia al grupo de administradores de la colección de proyectos.](https://docs.microsoft.com/vsts/organizations/security/set-project-collection-level-permissions?view=vsts) Para crear proyectos de equipo, el permiso **Crear nuevos proyectos** se debe establecer en **Permitir**.

2.  En la página Proyectos, seleccione **Nuevo proyecto**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image69.png)

1.  Nombre al proyecto **HybridMLIris**.

2.  Seleccione su tipo de control de origen inicial como **Git**

3.  Seleccione un proceso y seleccione **Crear**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image70.png)

### <a name="import-some-code--create-repository"></a>Importación de algo de código: creación del repositorio

Se necesita un repositorio de Git para el código YAML.

#### <a name="add-user-to-the-git-repo"></a>Incorporación del usuario al repositorio de Git

1.  En el panel de proyecto predeterminado, seleccione Generar credenciales de GIT.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image71.png)

1.  Escriba la contraseña donde sea necesario y guarde las credenciales de Git.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image72.png)

1.  Inicialice el repositorio; para ello, seleccione el botón **Inicializar** y cree un archivo **LÉAME**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image73.png)

#### <a name="clone-the-git-repository-locally-and-upload-the-code"></a>Clone el repositorio de Git localmente y cargue el código. 

1.  Cree un directorio en la máquina en `c:\\users\\<User>\\source\\repos\\hybridMLIris` y clone el repositorio.

    ```Bash  
    sudo mkdir /mnt/c/users/<User>/source sudo mkdir /mnt/c/users/<User>/source/repos sudo mkdir /mnt/c/users/<User>/source/repos/hybridMLIris cd /mnt/c/users/<User>/source/repos/hybridMLIris sudo git clone  https://<yourvstssite>.visualstudio.com/HybridMLIris/_git/HybridMLIris
    ```

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image74.png)

1.  Vaya al repositorio recién clonado:

    ```Bash  
    ls
    cd ./HybridMLIris
    ```
    
    ![Texto alternativo](media\azure-stack-solution-machine-learning\image75.png)

1.  Copie el archivo **iris_deployment.yaml** en el repositorio.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_deployment.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_deployment.yaml
    ``` 

1.  Confirme el cambio en Git.

    ```Bash  
    git add . git commit -m Added Deployment YAML git push
    ```

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image76.png)

### <a name="prepare-the-private-build-and-release-agent-for-vsts-integration"></a>Preparación del agente de compilación y versión privado para la integración de VSTS

#### <a name="prerequisites"></a>Requisitos previos

VSTS se autentica en Azure Resource Manager mediante una entidad de servicio. Para que VSTS pueda aprovisionar recursos en una suscripción de Azure Stack, necesita el estado de colaborador. \ **Estos son los pasos generales que es necesario configurar para permitir dicha autenticación:**

1.  Se debe crear una entidad de servicio o usarse una ya existente.

2.  Se deben crear las claves de autenticación para la entidad de servicio.

3.  La suscripción de Azure Stack se debe validar mediante el control de acceso basado en rol para permitir que el SPN forme parte del rol de colaborador.

4.  Se debe crear una nueva definición de servicio en VSTS mediante los puntos de conexión de Azure Stack y la información de SPN.

#### <a name="service-principal-creation"></a>Creación de la entidad de servicio

Para crear una entidad de servicio y elegir Web App o API como tipo de aplicación, consulte las [instrucciones de creación de una entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

**Creación de las claves de acceso**

Una entidad de servicio requiere una clave para la autenticación; siga los pasos descritos en esta sección para generar una clave.

1.  En **Registros de aplicaciones**, en Azure Active Directory, seleccione la aplicación.

    ![seleccionar aplicación](media\azure-stack-solution-machine-learning\image77.png)

1.  Anote el valor del **identificador de aplicación. El valor se usa al configurar el punto de conexión de servicio en VSTS.**

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image78.png)

1.  Para generar una clave de autenticación, seleccione **Configuración**.

    ![Seleccionar Configuración](media\azure-stack-solution-machine-learning\image79.png)

1.  Seleccione **Claves**.

    ![seleccionar claves](media\azure-stack-solution-machine-learning\image80.png)

1.  Proporcione una descripción de la clave y una duración. Cuando haya terminado, seleccione **Guardar**.

    ![guardar clave](media\azure-stack-solution-machine-learning\image81.png)

Después de guardar la clave, se muestra el valor de la clave. Copie este valor porque lo necesitará más adelante. Se necesita el **valor de clave** con el identificador de aplicación para iniciar sesión con la aplicación. Guarde el valor de clave donde la aplicación pueda recuperarlo.

![Texto alternativo](media\azure-stack-solution-machine-learning\image82.png)

#### <a name="get-tenant-id"></a>Obtención del identificador de inquilino

Como parte de la configuración del punto de conexión de servicio, VSTS requiere el **identificador de inquilino** que corresponde al directorio de AAD en el que se implementó la marca de Azure Stack. Siga los pasos de esta sección para obtener el identificador de inquilino.

1.  Seleccione **Azure Active Directory**.

    ![seleccionar azure active directory](media\azure-stack-solution-machine-learning\image83.png)

1.  Para obtener el identificador de inquilino, seleccione **Propiedades** en el inquilino de Azure AD.

    ![Selección de las propiedades de Azure AD](media\azure-stack-solution-machine-learning\image84.png)

1.  Copie el **id. de directorio**. Este valor es el identificador de inquilino.

    ![tenant ID](media\azure-stack-solution-machine-learning\image85.png)

Concesión de derechos de la entidad de servicio para implementar recursos en la suscripción de Azure Stack

Para acceder a los recursos de la suscripción, asigne un rol a la aplicación. Decida qué rol representa los permisos adecuados para la aplicación. Para obtener más información sobre los roles disponibles, vea [RBAC: Roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Establezca el ámbito en el nivel de suscripción, grupo de recursos o recurso. Los permisos se heredan en los niveles inferiores del ámbito. Por ejemplo, el hecho de agregar el rol de lector a una aplicación en un grupo de recursos significa que dicha aplicación puede leer el grupo de recursos y los recursos que contiene.

1.  Desplácese hasta el nivel de ámbito al que desea asignar la aplicación. Por ejemplo, para asignar un rol en el ámbito de suscripción, seleccione **Suscripciones**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image86.jpeg)

1.  Seleccione la **suscripción** (grupo de recursos o recurso) a la que asignar la aplicación.

    ![seleccionar suscripción para la asignación](media\azure-stack-solution-machine-learning\image87.png)

1.  Seleccione **Access Control (IAM)**.

    ![seleccionar acceso](media\azure-stack-solution-machine-learning\image88.png)

1.  Seleccione **Agregar**.

    ![seleccionar agregar](media\azure-stack-solution-machine-learning\image89.png)

1.  Seleccione el rol que asignará a la aplicación. En la imagen siguiente se muestra el rol **Propietario**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image90.png)

1.  De manera predeterminada, las aplicaciones de Azure Active Directory no se muestran en las opciones disponibles. Para encontrar la aplicación, **proporcione el nombre** en el campo de búsqueda y selecciónelo.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image91.png)

1.  Haga clic en **Guardar** para finalizar la asignación del rol. La aplicación se muestra en la lista de usuarios asignados a un rol para ese ámbito.

### <a name="role-based-access-control"></a>Control de acceso basado en roles

El control de acceso basado en rol (RBAC) de Azure hace posible la administración del acceso de forma pormenorizada en Azure y Azure Stack. Con RBAC, puede conceder únicamente la cantidad de acceso que los usuarios necesiten para realizar sus trabajos. Para más información sobre el control de acceso basado en rol, consulte [Administración del acceso a los recursos de suscripción de Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

**Grupos de agentes de VSTS**

En lugar de administrar cada agente de forma individual, los agentes se organizan en **grupos**. Un grupo de agentes define el límite de uso compartido de todos los agentes de ese grupo. En VSTS, los grupos de agentes se limitan a la cuenta de VSTS, por lo que se pueden compartir entre proyectos de equipo. Para más información sobre cómo crear grupos de agentes de VSTS y ver un tutorial al respecto, consulte [Create Agent Pools and Queues](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts) (Creación de grupos de agentes y colas).

**e un token de acceso personal (PAT) a Azure Stack**

 -  Inicie sesión en la cuenta de VSTS y seleccione el nombre del **perfil de la cuenta**.

 -  Seleccione **Administrar seguridad** para acceder a la página de creación de tokens de acceso.

![Texto alternativo](media\azure-stack-solution-machine-learning\image92.png)

![Texto alternativo](media\azure-stack-solution-machine-learning\image93.jpeg)

![Texto alternativo](media\azure-stack-solution-machine-learning\image94.jpeg)

> [!Note]  
> Obtenga la información del token. Después de salir de esta pantalla, no se volverá a mostrar.

1.  Copie el **token**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image95.png)

#### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Instalación del agente de compilación de VSTS en el servidor de compilación hospedado de Azure Stack

1.  Conéctese al **servidor de compilación** implementado en el host de Azure Stack.

    > [!Note]  
    > Asegúrese de que servidor de compilación hospedado por Azure Stack sea accesible desde la red Internet pública. Si no es así, hable con un operador de Azure Stack para obtener acceso.

    ```Bash  
    ssh <user>@<buildserver.publicip>
    ```

2.  Actualice el servidor de compilación de Ubuntu a la versión más reciente (18.04):

    ```Bash  
    sudo su
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    apt-get autoremove
    do-release-upgrade -d
    ```

    > [!Note]  
    > Esta operación tarda un tiempo.

2.  Instale las aplicaciones que son requisito previo para el servidor de compilación. Desde el shell de Bash del servidor de compilación basado en Ubuntu, ejecute los siguientes comandos:

    ```Bash  
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list 
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install liblttng-ust0 libcurl3 libssl1.0.0 curl lsb-release libkrb5-3 zlib1g libicu60 aspnetcore-runtime-2.1 dotnet-sdk-2.1
    wget https://github.com/PowerShell/PowerShell/releases/download/v6.1.0-preview.3/powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo dpkg -i powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo apt-get install -f
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –
    sudo apt-get update && sudo apt-get install azure-cli
    ```

2.  Descargue e implemente el agente de compilación como un servicio mediante un **token de acceso personal (PAT)** y ejecútelo como la cuenta de administrador de la máquina virtual.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image96.png)

    ```Bash  
        cd \home\<user>
        sudo mkdir myagent && cd myagent
        wget https://vstsagentpackage.azureedge.net/agent/2.134.2/vsts-agent-linux-x64-2.134.2.tar.gz
        sudo tar zxvf vsts-agent-linux-x64-2.134.2.tar.gz
    ```

2.  Vaya a la carpeta del agente de compilación extraído. Ejecute el código siguiente:

    ```Bash  
        cd ..
        sudo chmod -R 777 myagent
        cd myagent
        ./config.sh
    ```

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image97.png)

2.  Una vez que ha finalizado **./config.sh**, ejecute el siguiente código para habilitar el servicio en el arranque del servidor e iniciar el servicio:

    ```Bash  
    sudo ./svc.sh install
    sudo ./svc.sh start
    ```

El agente ahora está visible en la carpeta de VSTS.

#### <a name="endpoint-creation-permissions"></a>Permisos de creación de puntos de conexión

Los usuarios pueden crear puntos de conexión para que las compilaciones de VSTO puedan implementar aplicaciones de servicios de Azure en la pila. VSTS se conecta con el agente de compilación, que, a su vez, se conecta con Azure Stack.

![Texto alternativo](media\azure-stack-solution-machine-learning\image98.png)

1.  En el menú **Configuración**, seleccione **Seguridad**.

2.  En la lista **Grupos de VSTS** de la izquierda, seleccione **Creadores de extremos**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image99.png)

3.  En la pestaña **Miembros**, seleccione **+Agregar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image100.png)

1.  Escriba **username** y seleccione el nombre de usuario de la lista.

2.  Seleccione **Save changes** (Guardar los cambios).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image101.png)

3.  En la lista **Grupos de VSTS** de la izquierda, seleccione **Administradores de extremos**.

4.  En la pestaña **Miembros**, seleccione **+Agregar**.

5.  Escriba un **nombre de usuario** y seleccione ese usuario de la lista.

6.  Seleccione **Guardar cambios**.

    ![buchatech](media\azure-stack-solution-machine-learning\image102.jpeg)

    El agente de compilación de Azure Stack obtiene instrucciones de VSTS, el cual transmite la información de punto de conexión para la comunicación con Azure Stack.

    La conexión de VSTS a Azure Stack ya está lista.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image103.png)

### <a name="configure-build-and-release-definitions"></a>Configuración de definiciones de compilación y versión

Ahora que están establecidas las conexiones, asignará manualmente el punto de conexión de Azure creado, AKS y Azure Container Registry a las definiciones de compilación y versión.

#### <a name="create-the-build-definition-for-the-yaml-code"></a>Creación de la definición de compilación para el código YAML

1.  Seleccione la sección Builds (Compilaciones) que aparece en la central Build and Release (Compilación y versión) y cree una definición.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image104.png)

1.  Seleccione GIT de VSTS y seleccione el repositorio que creó anteriormente.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image105.png)

1.  Seleccione una canalización vacía como plantilla.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image106.png)

1.  Llame a la compilación **Copy Artifact** (Copiar artefacto) y seleccione el servidor de compilación de Azure Stack para la cola de agentes.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image107.png)

1.  Seleccione la fase 1 en los procesos y cambie su nombre por **Copy Artifact** (Copiar artefacto); a continuación, **agregue una tarea** a la fase:

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image108.png)

1.  Seleccione **Publicar los artefactos de la compilación** en la lista **Utilidad** y seleccione **Agregar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image109.png)

1.  Seleccione la **ruta de acceso para publicar** y seleccione el archivo **iris_deployment.yaml**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image110.png)

1.  Llame al artefacto **iris_deployment** y seleccione que la ubicación de publicación sea **Visual Studio Team Services/TFS**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image111.png)

1.  Seleccione **Guardar y poner en cola**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image112.png)

1.  Para comprobar el estado de la compilación, seleccione el identificador de compilación.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image113.png)

Si es correcto, el resultado será similar al siguiente:

![Texto alternativo](media\azure-stack-solution-machine-learning\image114.png)

#### <a name="create-the-release-definition-for-the-yaml-code"></a>Creación de la definición de versión para el código YAML

1.  Seleccione la sección Releases (Versiones) que aparece en la central Build and Release (Compilación y versión) y cree una definición.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image115.png)

1.  Seleccione la canalización vacía como plantilla.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image106.png)

1.  Llame al entorno Azure Stack.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image116.png)

1.  Agregue un nuevo artefacto; para ello, seleccione **Artefactos** y **+Agregar**.

2.  Seleccione compilación como el tipo de origen y **HybridMLIris** como el proyecto.

3.  A continuación, seleccione la definición de compilación que creó anteriormente para el origen.

4.  Seguidamente, seleccione **Agregar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image117.png)

1.  Seleccione Azure Stack de los entornos y, a continuación, agregue una nueva tarea a Azure Stack.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image118.png)


1.  En la fase de agente, establezca la cola de agentes en el servidor de compilación hospedado por Azure Stack.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image119.png)

1.  Agregue una nueva tarea a esta fase, en Implementar, seleccione la tarea de implementación en Kubernetes y seleccione Agregar.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image120.png)


1.  Llámela **Kubectl Apply** (nombre predeterminado) y seleccione el comando apply.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image121.png)

    Ahora cree una nueva conexión a Kubernetes Service.

#### <a name="create-kubernetes-service-endpoint"></a>Creación de un punto de conexión de servicio de Kubernetes

1.  En Conexión de servicio de Kubernetes, haga clic en el botón **+ Nuevo** y elija **Kubernetes** en la lista. Puede usar este punto de conexión para conectar **VSTS** y **Azure Container Service (AKS)**.

2.  **Nombre de la conexión**: proporcione el nombre de la conexión.

3.  **Dirección URL del servidor**: proporcione la dirección del servicio de contenedor en el formato http://{dirección de servidor de API}

4.  **Kubeconfig**: para obtener el valor de Kubeconfig, ejecute los siguientes comandos de Azure en un símbolo iniciado con privilegios de administrador.

    > [!Important]  
    > Use esta ventana de la CLI para realizar los pasos siguientes.

6.  En la ventana de la CLI, inicie sesión en Azure. [Más información acerca de az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

7.  En el símbolo del sistema, escriba:

    ```CLI
        az login
    ```

10. Este comando devuelve un código que se utiliza en el explorador en <https://aka.ms/devicelogin>.

11. Vaya a <https://aka.ms/devicelogin> en el explorador. Cuando se le solicite, escriba el código, recibido en la CLI, en el explorador.

    ![Punto de conexión de servicio de Kubernetes](media\azure-stack-solution-machine-learning\image122.png)

1.  Escriba el siguiente comando en el símbolo del sistema para obtener las credenciales de acceso del clúster de Kubernetes.

### <a name="azure-ml-workbench-cli"></a>CLI de Azure ML Workbench

az aks get-credentials nombre del grupo de recursos <yourResourceGroup><yourazurecontainerservice>

![Punto de conexión de servicio de Kubernetes](media\azure-stack-solution-machine-learning\image123.png)

1.  Vaya a la carpeta **.kube** en el directorio principal (por ejemplo, C:\\Users\\<user>\\Documents\\Kube)

2.  Copie el contenido del archivo **config** y péguelo en la ventana de conexión de Kubernetes. Seleccione el botón **Aceptar**.

    ![Punto de conexión de servicio de Kubernetes](media\azure-stack-solution-machine-learning\image124.png)
    

3.  Una vez que se crea y se selecciona el punto de conexión de Kubernetes, active la casilla Use configuration files (Usar archivos de configuración) para agregar un archivo de configuración. A continuación, busque el archivo iris_deployment.yaml en los artefactos vinculados.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image125.png)

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image126.png)

4.  Guarde la definición de versión.

#### <a name="check-the-status-of-the-release-definition"></a>Compruebe el estado de la definición de versión. 

Una vez guardada la definición de versión, VSTS debe iniciar automáticamente una versión.

Compruebe el estado de la implementación; para ello, ejecute un comando rápido en el símbolo del sistema de WSL y, a continuación, compruebe la interfaz de usuario de Kubernetes.

```Bash  
kubectl get deployments
```

La salida debe ser similar a la siguiente mientras tiene lugar el proceso de implementación.

![Texto alternativo](media\azure-stack-solution-machine-learning\image127.png)

```Bash  
kubectl proxy
```

Una vez que la interfaz de usuario de Kubernetes está en ejecución, vaya a la implementación en [**https://localhost:8001/**](https://localhost:8001/) y navegue a **Workloads-> Replica Sets** (Cargas de trabajo > Conjuntos de réplicas).

![Texto alternativo](media\azure-stack-solution-machine-learning\image128.png)

### <a name="deploy-the-yaml-service"></a>Implementación del servicio YAML

#### <a name="upload-the-irisserviceyaml-to-the-repository-and-sync-changes"></a>Carga del archivo iris_service.yaml en el repositorio y sincronización de los cambios

1.  Vaya al repositorio recién clonado:

    ```Bash  
    cd /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/
    ```

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image75.png)

1.  Copie el archivo **iris_service.yaml** en el repositorio.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_service.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_service.yaml
    ```

1.  Confirme el cambio en Git.

    ```Bash  
    git add .
    git commit -m “Added Service YAML” 
    git push
    ```

![Texto alternativo](media\azure-stack-solution-machine-learning\image129.png)

#### <a name="update-the-build-definition-for-the-yaml-code"></a>Actualización de la definición de compilación para el código YAML

1.  Seleccione la sección Builds (Compilaciones) que aparece en la central Build and Release (Compilación y versión) y elija la definición que creó anteriormente.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image130.png)

2.  Seleccione el botón Editar para editar la definición.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image131.png)

3.  **Agregue una tarea** al panel. Seleccione **Publicar los artefactos de la compilación** en la lista **Utilidad** y seleccione **Agregar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image108.png)

4.  Llámela **Kubectl Apply** (nombre predeterminado) y seleccione el comando apply.



#### <a name="update-the-release-definition-for-the-yaml-code"></a>Actualización de la definición de versión para el código YAML

1.  Seleccione la sección Releases (Versiones) situada en la central Build and Release (Compilación y versión) y elija la definición de versión que creó anteriormente. A continuación, seleccione el vínculo Editar.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image132.png)

1.  Seleccione el entorno **Azure Stack** y agregue una nueva tarea a Azure Stack.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image133.png)

1.  Agregue una **nueva tarea** a esta fase, en **Implementar**, seleccione la tarea de **implementación en Kubernetes** y seleccione **Agregar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image134.png)

1.  Llámela **Kubectl Apply** (nombre predeterminado) y seleccione el comando apply.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image109.png)

1.  Establezca la conexión de Kubernetes Service en la conexión de Azure Stack creada anteriormente y, a continuación, active la casilla **Use configuration files** (Usar archivos de configuración) para agregar un archivo de configuración. Busque el archivo iris_service.yaml en los artefactos vinculados.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image135.png)


    ![Texto alternativo](media\azure-stack-solution-machine-learning\image136.png)

1.  Guarde la definición de versión.

#### <a name="check-the-status-of-the-release-definition"></a>Compruebe el estado de la definición de versión.

Una vez guardada la definición de versión, VSTS debe iniciar automáticamente una versión.

Compruebe el estado de la implementación; para ello, ejecute un comando rápido en el símbolo del sistema de WSL y, a continuación, compruebe la interfaz de usuario de Kubernetes.

```Bash  
kubectl get deployments
```

La salida debe ser similar a la siguiente mientras tiene lugar el proceso de implementación.

![Texto alternativo](media\azure-stack-solution-machine-learning\image127.png)


```Bash  
kubectl proxy
```

Una vez que la interfaz de usuario de Kubernetes está en ejecución, vaya a la implementación en [**https://localhost:8001/**](https://localhost:8001/) y navegue a **Workloads-> Replica Sets** (Cargas de trabajo > Conjuntos de réplicas).

![Texto alternativo](media\azure-stack-solution-machine-learning\image137.png)


### <a name="kubernetes-scoring-and-validation"></a>Puntuación y validación de Kubernetes

Inicio de la interfaz de usuario de Kubernetes

```Bash  
kubectl proxy
```

Vaya a la interfaz de usuario de Kubernetes y, luego, diríjase a **Deployments** -> **Iris-Deployment** -> **New Replica Set** -> **Iris-Deployment-xxxxxxxxx** (Implementaciones > Implementación de Iris > Nuevo conjunto de réplicas > Implementación de Iris-xxxxxxxx) (donde las x son el identificador de implementación).

![Texto alternativo](media\azure-stack-solution-machine-learning\image138.png)

A continuación, vaya a **Services** (Servicios) y seleccione el **punto de conexión externo** del servicio para validar que funciona.

![Texto alternativo](media\azure-stack-solution-machine-learning\image139.png)

Se mostrará un mensaje de validación similar al siguiente:

![Texto alternativo](media\azure-stack-solution-machine-learning\image140.png)

#### <a name="create-azure-stack-scoring-function-app-in-the-azure-stack-portal"></a>Creación de una aplicación de función de puntuación de Azure Stack en el portal de Azure Stack

Se necesita una aplicación de función para hospedar la ejecución de cada función. Una aplicación de función permite la agrupación de funciones como una unidad lógica para la administración, la implementación y el uso compartido de recursos de forma fácil.

1.  En el portal de usuarios de Azure Stack, seleccione el botón **+ New** (+Nuevo) que se encuentra en la esquina superior izquierda y seleccione **Web + Mobile** >**Function App** (Web y móvil > Aplicación de función).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image141.png)

1.  Llame a la función **data-functions** y colóquela en el mismo grupo de recursos con el resto de contenido de Machine Learning. Permita que la herramienta cree automáticamente un plan de App Service para consumo y use la cuenta de almacenamiento creada anteriormente para el almacenamiento de la aplicación.

    ![Definición de la configuración de la nueva aplicación de función](media\azure-stack-solution-machine-learning\image142.png)

1.  Seleccione **Create** (Crear) para aprovisionar e implementar la aplicación de función.

2.  Seleccione el icono de notificación de la esquina superior derecha del portal y observe el mensaje **Deployment succeeded** (Implementación correcta).

    ![Definir nueva configuración de Function App](media\azure-stack-solution-machine-learning\image143.png)

1.  Seleccione **Go to resource** (Ir al recurso) para ver la nueva aplicación de función.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image144.png)

1.  Para crear una función, seleccione **Functions** (Funciones) y luego el botón **+New Function** (+ Nueva función).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image145.png)

1.  Selección del desencadenador HTTP

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image146.png)

1.  Seleccione **C\#** como lenguaje y llame a la función: **clean-score-data**. Luego, establezca el nivel de autorización en **Anonymous** (Anónimo).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image147.png)

1.  Copie el contenido del código de ejemplo de clean-score-data y péguelo en la función.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image148.png)

#### <a name="use-postman-to-validate-functions"></a>Uso de Postman para validar funciones

Para asegurarse de que ha configurado Kubernetes y Functions correctamente, puede usar la aplicación gratuita Postman para probar y validar esquemas y funciones. Para iniciar este proceso, primero recopilará alguna información de la instancia de Kubernetes.

1.  Vaya a la interfaz de usuario de Kubernetes y, luego, diríjase a **Deployments** -> **Iris-Deployment** -> **New Replica Set** -> **Iris-Deployment-xxxxxxxxx** (Implementaciones > Implementación de Iris > Nuevo conjunto de réplicas > Implementación de Iris-xxxxxxxx) (donde las x son el identificador de implementación).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image138.png)

1.  A continuación, vaya a **Services** (Servicios) y copie el **punto de conexión externo**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image149.png)

1.  Si es necesario, descargue e instale la aplicación Postman [aquí](https://www.getpostman.com/apps).

2.  Inicie sesión en la aplicación Postman y cierre el cuadro de diálogo New File (Nuevo archivo).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image150.png)

1.  En la aplicación Postman, seleccione POST.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image151.png)

1.  Pegue la dirección URL del **punto de conexión externo** en la aplicación Postman, en la **dirección URL de solicitud**, y agregue **\\score** al final de esta dirección, como se muestra a continuación.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image152.png)

1.  Seleccione la pestaña **Body** (Cuerpo) y, luego, el tipo de datos como **raw** y seleccione **JSON**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image153.png)

1.  En un explorador web, vaya a **External endpoint** (Punto de conexión externo). Agregue lo siguiente a la dirección URL **/swagger.json**. Esta dirección lleva al archivo de Swagger de servicios usado para la configuración de la prueba.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image154.png)

1.  Copie el ejemplo que se muestra en el archivo **Swagger.JSON**.

2.  En la aplicación Postman, pegue el ejemplo en el cuerpo de la publicación y seleccione **Send** (Enviar). Se devolverá un valor similar al que se muestra a continuación.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image155.png)

## <a name="step-7-create-an-azure-stack-storage-account-and-storage-queue"></a>Paso 7: Creación de una cuenta de almacenamiento de Azure Stack y la cola de almacenamiento

Cree una cuenta de almacenamiento de Azure Stack y la cola de almacenamiento para los datos.

1.  Inicie sesión en el portal de usuarios de Azure Stack. (Cada instancia de Azure Stack tiene una dirección URL única de portal)

2.  En el portal de usuarios de Azure Stack, expanda el menú de la izquierda para abrir el menú de servicios y elija **All Services** (Todos los servicios). Desplácese hacia abajo hasta **Almacenamiento** y elija **Cuentas de almacenamiento**. En la ventana **Storage Accounts** (Cuentas de almacenamiento), elija **Add** (Agregar).

3.  Escriba un nombre para la cuenta de almacenamiento.

4.  Seleccione la opción de replicación de la cuenta de almacenamiento: **LRS**.

5.  Especifique un nuevo grupo de recursos o seleccione un grupo de recursos existente.

6.  Seleccione **Local** para la ubicación de la cuenta de almacenamiento.

7.  Seleccione **Create** (Create) para crear la cuenta de almacenamiento.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image156.png)

1.  Elija la cuenta de almacenamiento creada recientemente.

2.  Seleccione **Queues** (Colas).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image157.png)

1.  Seleccione **+ Queue** (+ Cola), asigne un nombre a la cola y haga clic en **OK** (Aceptar).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image158.png)

1.  Obtenga la **cadena de conexión** de la cola de almacenamiento y cópiela.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image159.png)

1.  Vaya a la aplicación de función de Azure y, luego, seleccione **Configuración de la aplicación**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image160.png)

1.  En Configuración de la aplicación de la aplicación de función, desplácese hacia abajo hasta Configuración de la aplicación y seleccione **+ Agregar nuevo valor**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image161.png)

1.  Escriba el nombre de la cuenta de almacenamiento en el campo **Nombre** y agregue al final _STORAGE.

De esta forma, la aplicación comprende que esto es un punto de conexión de cuenta de almacenamiento.

1.  A continuación, pegue la cadena de conexión en el campo **Valor**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image162.png)

1.  Desplácese hasta la parte superior de Configuración de la aplicación y seleccione **Guardar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image163.png)

### <a name="update-the-scoring-function-to-use-storage-queue"></a>Actualización de la función de puntuación para usar la cola de almacenamiento

1.  Seleccione **Integrar** en la función y deseleccione la opción **GET**.

2.  Seleccione **Guardar**.

3.  A continuación, seleccione **+ Nueva salida** en Salidas.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image164.png)

1.  Luego, seleccione **Azure Queue Storage** y elija **Seleccionar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image165.png)

1.  Actualice el **nombre de la cola** con la cola de almacenamiento creada anteriormente y, luego, establezca **Conexión de cuenta de Storage** en la conexión de la cuenta de almacenamiento creada anteriormente. A continuación, seleccione **Guardar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image166.png)

## <a name="step-8-create-a-function-to-handle-clean-data"></a>Paso 8: Creación de una función para administrar los datos limpios

Cree una función de Azure Stack para mover los datos limpios de Azure Stack a Azure.

1.  Para crear una función, seleccione **Functions** (Funciones) y luego el botón **+New Function** (+ Nueva función).

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image167.png)

1.  Seleccione **Desencadenador de temporizador**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image168.png)

1.  Seleccione **C\#** como lenguaje y llame a la función **upload-to-azure**, luego establezca la programación en **0 0 \*/1 \* \* \*** que, en notación de CRON, es una vez cada hora.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image169.png)

### <a name="get-the-connection-string-to-the-azure-hosted-storage-account"></a>Obtención de la cadena de conexión a la cuenta de almacenamiento hospedada por Azure

1.  Vaya a <https://portal.azure.com> y, a continuación, seleccione la **cuenta de Azure Storage** creada anteriormente.

2.  Seleccione **Claves de acceso** y copie la **cadena de conexión** de la cuenta de almacenamiento.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image170.png)

### <a name="update-the-upload-to-azure-function-to-use-the-azure-hosted-storage"></a>Actualización de la función upload-to-azure para usar el almacenamiento hospedado por Azure

1.  Vaya a la aplicación de función de Azure y, luego, seleccione **Configuración de la aplicación**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image171.png)

1.  En Configuración de la aplicación de la aplicación de función, desplácese hacia abajo hasta Configuración de la aplicación y seleccione **+ Agregar nuevo valor**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image172.png)

1.  Escriba el nombre de la cuenta de almacenamiento en el campo **Nombre** y agregue al final _STORAGE.

De esta forma, la aplicación comprende que esto es un punto de conexión de cuenta de almacenamiento.

1.  A continuación, pegue la cadena de conexión de la cuenta de almacenamiento hospedada por Azure en el campo **Valor**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image173.png)

1.  Desplácese hasta la parte superior de Configuración de la aplicación y seleccione **Guardar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image174.png)

1.  Vuelva a la función **upload-to-azure**.

2.  Seleccione **Integrar** en la función.

3.  A continuación, seleccione **+ Nueva salida** en Salidas.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image175.png)

1.  A continuación, seleccione **Azure Blob Storage** y elija **Seleccionar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image176.png)

1.  Actualice la **ruta de acceso** con el contenedor de almacenamiento creado anteriormente con el siguiente formato: **uploadeddata/{rand-guid}.txt** y, luego, establezca **Conexión de cuenta de Storage** en la conexión de la cuenta de almacenamiento a Azure creada anteriormente y haga clic en **Guardar**.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image177.png)

1.  Copie el contenido del código de ejemplo de **upload-to-azure** en la función.

2.  Modifique lo que sea necesario para que apunte a la cadena de conexión de cuentas de almacenamiento.

3.  Guarde y ejecute el código.

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image178.png)

1.  Compruebe la cuenta de almacenamiento hospedada por Azure para ver que los datos se han analizado de Azure a la nube. Si la operación se ha realizado correctamente, se mostrará algo parecido a esto:

    ![Texto alternativo](media\azure-stack-solution-machine-learning\image179.png)

Machine Learning de Kubernetes hospedado por Azure Stack ha saneado los datos de información confidencial y los ha cargado en la nube pública de Azure desde la instancia local de Azure Stack, mediante aplicaciones de función hospedadas por Azure Stack, y puede realizar una copia intermedia de los datos para cargarlos en escenarios perimetrales o desconectados.

## <a name="next-steps"></a>Pasos siguientes

 - Para más información sobre los patrones de nube de Azure, consulte [Patrones de diseño en la nube](https://docs.microsoft.com/azure/architecture/patterns).