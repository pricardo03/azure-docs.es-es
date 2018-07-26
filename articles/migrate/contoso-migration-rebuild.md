---
title: Recompilar una aplicación local de Contoso en Azure | Microsoft Docs
description: Obtenga información acerca de cómo Contoso recompila una aplicación en Azure con Azure App Services, el servicio de Kubernetes, CosmosDB, Azure Functions y Cognitive Services.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: raynew
ms.openlocfilehash: 0d195d5fbede3100c0474ae9614a880cfb3acb19
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005006"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Migración de Contoso: Recompilar una aplicación local en Azure

En este artículo se muestra cómo Contoso migra y recompila su aplicación de SmartHotel en Azure. Migra la máquina virtual de front-end de la aplicación a las aplicaciones web de Azure App Services. El back-end de la aplicación se compila con microservicios implementados en contenedores administrados por Azure Kubernetes Service (AKS). El sitio interactúa con Azure Functions y proporciona la funcionalidad de fotos de mascotas. 

Este documento es el primero de una serie de artículos que muestran cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. En la serie se incluye información de fondo y escenarios en los que se muestra la configuración de una infraestructura de migración, la valoración de los recursos locales para la migración y la ejecución de distintos tipos de migraciones. La complejidad de los escenarios aumenta e iremos agregando otros artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Describe cómo Contoso prepara la infraestructura local y de Azure para la migración. Se utiliza la misma infraestructura para todos los artículos de migración. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Muestra cómo Contoso evalúa una aplicación local de SmartHotel de dos niveles que se ejecuta en VMware. Contoso evalúa las VM de la aplicación con el servicio [Azure Migrate](migrate-overview.md), y la base de datos de SQL Server de la aplicación con [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehospedaje de una aplicación a las VM de Azure o a una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Muestra cómo Contoso ejecuta una migración mediante lift-and-shift a Azure para la aplicación SmartHotel. Contoso migra la VM de front-end de la aplicación con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), y la base de datos de la aplicación a una instancia administrada de SQL con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible
[Artículo 5: Rehospedaje de una aplicación en VM de Azure](contoso-migration-rehost-vm.md) | Muestra cómo Contoso migra las VM de la aplicación SmartHotel solo con Site Recovery. | Disponible
[Artículo 6: Rehospedaje de una aplicación en VM de Azure y el grupo de disponibilidad Always On de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Muestra cómo Contoso migra la aplicación SmartHotel. Contoso usa Site Recovery para migrar las VM de la aplicación, y Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server protegido por un grupo de disponibilidad Always On. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso hace una migración mediante lift-and-shift de la aplicación de osTicket de Linux para VM de Azure, con Site Recovery. | Disponible
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Se muestra cómo Contoso migra la aplicación osTicket de Linux a VM de Azure mediante Site Recovery y cómo migra la base de datos de aplicaciones a una instancia de Azure MySQL Server mediante MySQL Workbench. | Disponible
[Artículo 9: Refactorizar una aplicación en Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a una aplicación web de Azure y migra la base de datos de la aplicación a la instancia de Azure SQL Server | Disponible
[Artículo 10: Refactorizar una aplicación Linux en Azure Web Apps y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Muestra cómo Contoso migra la aplicación Linux osTicket a Azure Web Apps en varios sitios integrados con GitHub para ofrecer una entrega continua. Se migra la base de datos de la aplicación a una instancia de Azure MySQL. | Disponible
[Artículo 11: Refactorizar TFS en VSTS](contoso-migration-tfs-vsts.md) | Muestra cómo Contoso migra la implementación de Team Foundation Server (TFS) local mediante la migración a Visual Studio Team Services (VSTS) en Azure. | Disponible
[Artículo 12: Rediseñar la arquitectura de una aplicación en contenedores de Azure y SQL Database](contoso-migration-rearchitect-container-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a Azure y rediseña su arquitectura. Se rediseña la arquitectura del nivel web de la aplicación como contenedor de Windows, y la base de datos de la aplicación en una instancia de Azure SQL Database. | Disponible
Artículo 13: Volver a compilar una aplicación en Azure | Muestra cómo Contoso vuelve a compilar su aplicación de SmartHotel con una gama de funcionalidades y servicios de Azure, como App Services, Azure Kubernetes, Azure Functions, Cognitive Services y Cosmos DB. | Este artículo.

En este artículo, Contoso migra Windows de dos niveles. de .NET de Windows de dos niveles que se ejecuta en las VM de VMware. Esta aplicación está disponible en código abierto y, si quiere usarla, puede descargarla en [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Impulsores del negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr con esta migración:

- **Abordar el crecimiento del negocio**: Contoso está creciendo. Quiere ofrecer experiencias diferenciadas a sus clientes en sus sitios web.
- **Agilidad**: Contoso debe poder reaccionar con más rapidez que los cambios del mercado para lograr el éxito en una economía global. 
- **Escalar**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.
- **Costos**: Contoso quiere minimizar los costos de licencia.

## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha establecido los requisitos de la aplicación para esta migración. Estos requisitos se usaron para determinar el mejor método de migración:
 - La aplicación de Azure seguirá siendo tan importante como lo es hoy en día. Debe funcionar correctamente y escalarse fácilmente.
 - La aplicación no debe usar componentes de IaaS. Debe compilarse todo para usar servicios PaaS o sin servidor.
 - Las compilaciones de la aplicación deben ejecutarse en servicios en la nube y los contenedores deben residir en un registro de contenedor de nivel empresarial y privado en la nube.
 - El servicio de API utilizado para las fotos de mascotas debe ser preciso y fiable en el mundo real, ya que las decisiones que toma la aplicación deben cumplirse en los hoteles. Cualquier mascota con acceso autorizado podrá permanecer en los hoteles.

## <a name="solution-design"></a>Diseño de la solución

Después de fijar sus objetivos y requisitos, Contoso diseña y revisa una solución de implementación e identifica el proceso de migración, incluidos los servicios de Azure que utilizará para la migración.

### <a name="current-app"></a>Aplicación actual

- La aplicación local de SmartHotel se divide en niveles entre dos VM (WEBVM y SQLVM).
- Las VM están ubicadas en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.


### <a name="proposed-architecture"></a>Arquitectura propuesta

- El front-end de la aplicación se implementará como una aplicación web de Azure App Services, en su región primaria.
- Una función de Azure proporcionará las cargas de las fotos de las mascotas y el sitio interactuará con esta funcionalidad.
- La función de fotos de mascotas aprovecha Cognitive Services Vision API y CosmosDB.
- El back-end del sitio se ha compilado con microservicios. Estos se implementarán en contenedores administrados en Azure Kubernetes Service (AKS).
- Los contenedores se compilarán con VSTS y se insertarán en Azure Container Registry (ACR).
- Por ahora, Contoso implementará manualmente el código de función y la aplicación web con Visual Studio.
- Los microservicios se implementarán mediante un script de PowerShell que llama a las herramientas de línea de comandos de Kubernetes.

    ![Arquitectura del escenario](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Revisión de la solución
Contoso evalúa su diseño propuesto creando una lista de ventajas y desventajas.

**Consideración** | **Detalles**
--- | ---
**Ventajas** | El uso de PaaS y soluciones sin servidor para la implementación de un extremo a otro reduce significativamente el tiempo de administración que Contoso debe proporcionar.<br/><br/> Cambiar a una arquitectura de microservicios permite que Contoso amplíe fácilmente su solución con el tiempo.<br/><br/> La nueva funcionalidad se puede poner en línea sin interrumpir ninguna de las bases de código de las soluciones existentes.<br/><br/> La aplicación web se configurará con varias instancias y sin ningún único punto de error.<br/><br/> El escalado automático se habilitará para que la aplicación pueda controlar volúmenes de tráfico diferentes.<br/><br/> Con el cambio a los servicios de PaaS, Contoso puede retirar soluciones obsoletas que se ejecutan en el sistema operativo Windows Server 2008 R2.<br/><br/> CosmosDB tiene tolerancia a errores integrada, que no requiere ninguna configuración por parte de Contoso. Esto significa que la capa de datos ya no es un único punto de conmutación por error.
**Desventajas** | Los contenedores son más complejos que otras opciones de migración. La curva de aprendizaje podría ser un problema para Contoso.  Contoso introduce un nuevo nivel de complejidad que proporciona un gran valor a pesar de la curva.<br/><br/> El equipo de operaciones de Contoso deberá esforzarse para comprender y ofrecer soporte técnico de Azure, los contenedores y los microservicios de la aplicación.<br/><br/> Contoso no ha implementado completamente DevOps para toda la solución. Debe pensar en eso para la implementación de servicios en AKS, funciones y App Services.



### <a name="migration-process"></a>Proceso de migración

1. Contoso aprovisiona ACR, AKS y CosmosDB.
2. Aprovisiona la infraestructura para la implementación, incluida la API, la función, la cuenta de almacenamiento y la aplicación web de Azure. 
3. Una vez instalada la infraestructura, compilará las imágenes del contenedor de microservicios con VSTS, que las inserta en ACR.
4. Contoso implementará estos microservicios en ASK mediante un script de PowerShell.
5. Por último, implementará la función de Azure y la aplicación web.

    ![Proceso de migración](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Simplifica las operaciones, la implementación y la administración de Kubernetes. Proporciona un servicio de orquestación de contenedores de Kubernetes totalmente administrado.  | AKS es un servicio gratuito.  Solo debe pagar por las máquinas virtuales y el almacenamiento y los recursos de red asociados que utilice. [Más información](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Azure Functions](https://azure.microsoft.com/services/functions/) | Acelera el proceso de desarrollo con una experiencia de proceso sin servidor controlada por eventos. Escalado a petición.  | Solo debe pagar solo por los recursos consumidos. El plan se factura en función del consumo de recursos y las ejecuciones por segundo. [Más información](https://azure.microsoft.com/pricing/details/functions/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Almacena imágenes para todos los tipos de implementaciones de contenedor. | Costo basado en características, almacenamiento y duración de la utilización. [Más información](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Compile, implemente y escale con rapidez aplicaciones web, móviles y de API de naturaleza empresarial que se puedan ejecutar en cualquier plataforma. | Los planes de App Service se facturan por segundo. [Más información](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Requisitos previos

Esto es lo que usted (y Contoso) necesita para ejecutar este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | Ya debería haber creado una suscripción cuando realizó la evaluación en el primer artículo de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.
**Infraestructura de Azure** | [Vea](contoso-migration-infrastructure.md) cómo Contoso configuró una infraestructura de Azure.
**Requisitos previos para desarrolladores** | Contoso necesita las siguientes herramientas en una estación de trabajo de desarrollador:<br/><br/> - [Visual Studio 2017 Community Edition: versión 15.5](https://www.visualstudio.com/)<br/><br/> Carga de trabajo. NET habilitada.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) o Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) configurado para usar contenedores de Windows.



## <a name="scenario-steps"></a>Pasos del escenario

Contoso ejecutará la migración de la forma siguiente:

> [!div class="checklist"]
> * **Paso 1: Aprovisionar AKS y ACR**: Contoso aprovisiona el clúster de AKS administrado y el registro de contenedores de Azure mediante PowerShell
> * **Paso 2: Compilar contenedores de Docker**: configura la integración continua para contenedores de Docker mediante VSTS y los inserta a ACR.
> * **Paso 3: Implementar microservicios de back-end**: implementa el resto de la infraestructura que aprovecharán los microservicios de back-end.
> * **Paso 4: Implementar la infraestructura de front-end**: implementa la infraestructura de front-end, incluidos el almacenamiento de blobs para los teléfonos de mascota, Cosmos DB y Vision API.
> * **Paso 5: Migrar el back-end**: implementa los microservicios y se ejecuta en AKS, para migrar el back-end.
> * **Paso 6: Publicar el front-end**: publica la aplicación de SmartHotel en el servicio App de Azure y la instancia de Function App a la que llamará el servicio de mascotas.



## <a name="step-1-provision-an-aks-cluster-and-acr"></a>Paso 1: Aprovisionar un clúster de AKS y ACR

Contoso ejecuta un script de implementación para crear el clúster de Kubernetes administrado con AKS y Azure Container Registry.

- Las instrucciones de esta sección usan el repositorio **SmartHotel360-Azure-backend**.
- El repositorio **SmartHotel360-Azure-backend** de GitHub contiene todo el software para esta parte de la implementación.

El aprovisionamiento será de la manera siguiente:

1. Antes de empezar, Contoso garantiza que todo el software necesario está instalado en la máquina de desarrollo que se está usando. 
2. Clona el repositorio localmente en la máquina de desarrollo mediante GIT.

    **clonación de GIThttps://github.com/Microsoft/SmartHotel360-Azure-backend.git**

3.  Contoso abre la carpeta con Visual Studio Code y se desplaza al directorio **/deploy/k8s**, que contiene el script **gen-aks-env.ps1**.
4. Ejecuta el script para crear el clúster de Kubernetes administrado con AKS y Container Registry.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
5.  Con el archivo abierto, actualiza el parámetro $location a **eastus2** y guarda el archivo.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

6. Hace clic en **Ver** > **Terminal integrado** para abrir el terminal integrado en el código.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

7. En el terminal integrado de PowerShell, inicia sesión en Azure mediante el comando Connect-AzureRmAccount. [Obtenga más información](https://docs.microsoft.com/powershell/azure/get-started-azureps) sobre cómo empezar a usar PowerShell.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

8. Autentica la CLI de Azure mediante la ejecución del comando **az login** y sigue las instrucciones para autenticarse con su explorador web. [Obtenga más información](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) acerca del inicio de sesión con la CLI de Azure.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

9. Ejecuta el siguiente comando, pasando el nombre del grupo de recursos de ContosoRG, el nombre del clúster de AKS smarthotel-aks-eus2 y el nuevo nombre del registro.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```

    ![AKS](./media/contoso-migration-rebuild/aks6.png)

10. Azure crea otro grupo de recursos, que contiene los recursos del clúster de AKS.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

11. Una vez finalizada la implementación, Contoso instala la herramienta de línea de comandos **kubectl**. La herramienta ya está instalada en Azure CloudShell.

    **az aks install-cli**

12. Verifica la conexión al clúster ejecutando el comando **kubectl get nodes**. El nodo tiene el mismo nombre que la máquina virtual en el grupo de recursos creado automáticamente.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

13. Ejecuta el comando siguiente para iniciar el panel de Kubernetes: 

    **az aks browse --resource-group ContosoRG --name smarthotelakseus2**

14. Se abre una pestaña del explorador en el panel. Se trata de una conexión de túnel mediante la CLI de Azure. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-build-a-docker-container"></a>Paso 2: Compilar un contenedor de Docker

### <a name="create-a-vsts-and-build"></a>Crear una instancia de VSTS y compilar

Contoso crea un proyecto de VSTS y configura una compilación de CI para crear el contenedor y, después, lo inserta en ACR. Las instrucciones de esta sección usan el repositorio [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend).

1. Desde visualstudio.com, se crea una nueva cuenta (**contosodevops360.visualstudio.com**) y se configura para usar GIT.

2. Crea un nuevo proyecto (**smarthotelrefactor**) con GIT para el control de versiones y Agile para el flujo de trabajo.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Importa el repositorio de GitHub.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. En **Compilación y versión**, crea una nueva definición mediante GIT de VSTS como origen, desde el repositorio **smarthotel** importado. 

    ![VSTS](./media/contoso-migration-rebuild/vsts3.png)

6. Selecciona la opción para empezar con una canalización vacía.

    ![VSTS](./media/contoso-migration-rebuild/vsts4.png)  

7. Selecciona **Hosted Linux Preview** (Versión preliminar hospedada de Linux) para la definición de compilación.

    ![VSTS](./media/contoso-migration-rebuild/vsts5.png) 
 
8. En **Fase 1**, agrega una tarea de **Docker Compose**. Esta tarea compila Docker Compose.

    ![VSTS](./media/contoso-migration-rebuild/vsts6.png) 

9. Repite la acción y agrega otra tarea de **Docker Compose**. Esta inserta los contenedores en ACR.

     ![VSTS](./media/contoso-migration-rebuild/vsts7.png) 

8. Selecciona la primera tarea (para compilar) y configura la compilación con la suscripción a Azure, la autorización y ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts8.png)

9. Especifica la ruta de acceso del archivo **docket-compose.yaml**, en la carpeta **src** del repositorio. Selecciona compilar imágenes de servicio e incluye la última etiqueta. Cuando la acción cambia a **Build service images** (Compilar imágenes del servicio), el nombre de la tarea de VSTS cambia a **Build services automatically** (Compilar servicios automáticamente).

    ![VSTS](./media/contoso-migration-rebuild/vsts9.png)

10. Ahora, Contoso configura la segunda tarea de Docker (para insertar). Selecciona la suscripción y la instancia de ACR **smarthotelacreus2**. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. De nuevo, introduce el archivo en docker-compose.yaml y selecciona **Push service images**  (Insertar imágenes del servicio) e incluye la última etiqueta. Cuando la acción cambia a **Push service images** (Insertar imágenes de servicio), el nombre de la tarea de VSTS cambia a **Push services automatically** (Insertar servicios automáticamente).

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Con las tareas de VSTS configuradas, Contoso guarda la definición de compilación e inicia el proceso de compilación.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Hace clic en el trabajo de compilación para comprobar el progreso.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Una vez finalizada la compilación, ACR muestra los nuevos repositorios, que se rellenan con los contenedores que usan los microservicios.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


## <a name="step-3-deploy-back-end-microservices"></a>Paso 3: Implementación de microservicios de back-end

Con el clúster de AKS creado y las imágenes de Docker compiladas, Contoso implementa ahora el resto de la infraestructura que aprovecharán los microservicios de back-end.

- En las instrucciones de esta sección se usa el repositorio [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend).
- En la carpeta **/deploy/k8s/arm**, hay un único script que permite crear todos los elementos. 

La implementación se realiza de la manera siguiente:

1. Contoso usa el archivo deploy.cmd para implementar los recursos de Azure en el grupo de recursos ContosoRG y la región EUS2, escribiendo el comando siguiente:

    **.\deploy azuredeploy ContosoRG -c eastus2**

    ![Implementar el back-end](./media/contoso-migration-rebuild/backend1.png)

2. Captura la cadena de conexión de cada base de datos, para usarla más adelante.

    ![Implementar el back-end](./media/contoso-migration-rebuild/backend2.png)

## <a name="step-4-deploy-front-end-infrastructure"></a>Paso 4: Implementar la infraestructura de front-end

Contoso necesita implementar la infraestructura que se usará en las aplicaciones de front-end. Crea un contenedor de almacenamiento de blobs para almacenar las imágenes de mascotas; la base de datos de Cosmos, para almacenar documentos con la información de las mascotas; y la instancia de Vision API para el sitio web. 

Las instrucciones de esta sección usan el repositorio [SmartHotel-public-web](https://github.com/Microsoft/SmartHotel360-public-web).

### <a name="create-storage-containers"></a>Crear contenedores de almacenamiento

1.  En Azure Portal, Contoso abre la cuenta de almacenamiento que se creó y hace clic en **Blobs**.
2.  Creará un nuevo contenedor (**Mascotas**) con el nivel de acceso público establecido en el contenedor. Los usuarios cargarán las fotos de sus mascotas en este contenedor.

    ![Blob de almacenamiento](./media/contoso-migration-rebuild/blob1.png)

3. Crea un segundo contenedor nuevo denominado **configuración**. Un archivo con la configuración de aplicación de front-end se colocará en este contenedor.

    ![Blob de almacenamiento](./media/contoso-migration-rebuild/blob2.png)

4. Captura los detalles de acceso de la cuenta de almacenamiento en un archivo de texto, para futuras referencias.

    ![Blob de almacenamiento](./media/contoso-migration-rebuild/blob2.png)

## <a name="provision-a-cosmos-database"></a>Aprovisionar una base de datos de Cosmos

Contoso aprovisiona una base de datos de Cosmos que se usará para la información de las mascotas.

1. Crea una instancia de **Azure Cosmos DB** en Azure Marketplace.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Especifica un nombre (**contosomarthotel**), selecciona la API de SQL y la coloca en el grupo de recursos de producción ContosoRG, en la región Este de EE. UU. 2 principal.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Agrega una nueva colección a la base de datos, con los valores de capacidad y rendimiento predeterminados.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Anota la información de conexión de la base de datos, para futuras referencias. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


## <a name="provision-computer-vision"></a>Aprovisionar Computer Vision

Contoso aprovisiona Computer Vision API. La función llamará a la API para evaluar las imágenes cargadas por los usuarios.

1. Crea una instancia de **Computer Vision** en Azure Marketplace.

     ![Computer Vision](./media/contoso-migration-rebuild/vision1.png)

2. Aprovisiona la API (**smarthotelpets**) en el grupo de recursos de producción ContosoRG, en la región Este de EE. UU. 2 principal.

    ![Computer Vision](./media/contoso-migration-rebuild/vision2.png)

3. Guarda la configuración de conexión para la API en un archivo de texto para consultarla más adelante.

     ![Computer Vision](./media/contoso-migration-rebuild/vision3.png)

## <a name="step-5-deploy-the-back-end-services-in-azure"></a>Paso 5: Implementar los servicios de back-end de Azure

Ahora, Contoso debe implementar el controlador de entrada NGINX para permitir el tráfico entrante en los servicios y, después, implementar los microservicios en el clúster de AKS.

Las instrucciones de esta sección usan el repositorio [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend).



1. Usa Visual Studio Code para actualizar el archivo **/deploy/k8s/config_loal.yml**. Actualiza varias conexiones de la base de datos con la información que se anotó anteriormente.

     ![Implementar microservicios](./media/contoso-migration-rebuild/deploy-micro.png)

    > [!NOTE]
    > Algunas de las opciones de configuración (por ejemplo, Active Directory B2C) no se tratan en este artículo. Hay más información acerca de estas opciones en el repositorio.

2. El archivo deploy.ps1 elimina todo el contenido del clúster (excepto la entrada y el controlador de entrada) e implementa microservicios. Lo ejecuta de la siguiente forma:

  ```
  .\deploy.ps1 -configFile .\conf_local.yml -dockerUser smarthotelacreus2  -dockerPassword [passwordhere] -registry smarthotelacreus2.azurecr.io -imageTag latest -deployInfrastructure $false -buildImages $false -pushImages $false
  ```

3. Ejecuta el siguiente comando para comprobar el estado de los servicios:

    ```
    kubectl get services
    ```
4. Abre el panel de Kubernetes para revisar la implementación:

    ```
    az aks browse --resource-group ContosoRG --name smarthotelakseus2
    ```


## <a name="step-6-publish-the-frontend"></a>Paso 6: Publicar el front-end

Como último paso, Contoso publica la aplicación de SmartHotel, que se denomina Pet Service, en Azure App Service y en Function App.

Las instrucciones de esta sección usan el repositorio [SmartHotel-public-web](https://github.com/Microsoft/SmartHotel360-public-web). .

### <a name="set-up-web-app-to-use-contoso-resources"></a>Configuración de Web App para el uso de los recursos de Contoso

1. Contoso clona el repositorio localmente en la máquina de desarrollo mediante GIT:

    **Clonación de GIT https://github.com/Microsoft/SmartHotel360-public-web.git**

2. En Visual Studio, abre la carpeta para mostrar todos los archivos del repositorio.

    ![Publicar el front-end](./media/contoso-migration-rebuild/front-publish1.png)

3. Realiza los cambios de configuración necesarios para la configuración predeterminada.

    - Cuando la aplicación web se inicia, busca la configuración de la aplicación **SettingsUrl**.
    - Esta variable debe contener una dirección URL a un archivo de configuración.
    - De forma predeterminada, esta opción de configuración se usa como un punto de conexión público.

4. Se actualiza el archivo **/config-sample.json/sample.json**. Este es el archivo de configuración para la web cuando se usa el punto de conexión público.

    - Edita las secciones **ursl** y **pets_config** con los valores para los puntos de conexión de la API de AKS, las cuentas de almacenamiento y la base de datos de Cosmos. 
    - Las direcciones URL deben coincidir con el nombre DNS de la nueva aplicación web que Contoso va a crear.
    - Para Contoso, se trata de **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![Publicar el front-end](./media/contoso-migration-rebuild/front-publish2.png)

5. Una vez actualizado el archivo, cambia su nombre a **smarthotelsettingsurl** y lo carga en el blob de almacenamiento que creó anteriormente.

     ![Publicar el front-end](./media/contoso-migration-rebuild/front-publish3.png)

6. Hace clic en el archivo para obtener la dirección URL. La aplicación usa esta dirección URL cuando comienza a extraer el archivo de configuración.

    ![Publicar el front-end](./media/contoso-migration-rebuild/front-publish4.png)

7. Actualiza **SettingsUrl** en los archivos **appsettings.Production.JSON** a la dirección URL del nuevo archivo.

    ![Publicar el front-end](./media/contoso-migration-rebuild/front-publish5.png)


### <a name="deploy-the-website-to-the-azure-app-service"></a>Implementar el sitio web en Azure App Service

Ahora, Contoso puede publicar su sitio web.


1. Habilita la supervisión de Application Insights en Visual Studio 2017. Para ello, selecciona el proyecto **PublicWeb** en el Explorador de soluciones y busca **Agregar Application Insights**. Registra la aplicación con la instancia de Application Insights que se creó cuando se implementó la infraestructura.

    ![Publicación del sitio web](./media/contoso-migration-rebuild/deploy-website1.png)

2. En Visual Studio, conecta el proyecto PublicWeb a App Insights y lo actualiza para mostrar que está configurado.
 
    ![Publicación del sitio web](./media/contoso-migration-rebuild/deploy-website2.png)

3. En Visual Studio, crea y publica su aplicación web.

    ![Publicación del sitio web](./media/contoso-migration-rebuild/deploy-website3.png)

5. Contoso especifica un nombre de aplicación y la coloca en el grupo de recursos de producción **ContosoRG**, en la región Este de EE. UU. 2 principal.

    ![Publicación del sitio web](./media/contoso-migration-rebuild/deploy-website4.png)

### <a name="deploy-the-function-to-azure"></a>Implementación de la función en Azure

1. Usa Visual Studio para crear y publicar la función. Para ello, hace clic en el botón derecho en **PetCheckerFunction** > **Publicar**. A continuación, crea una nueva función de App Service.

     ![Implementación de la función](./media/contoso-migration-rebuild/function1.png)

2. Especifica el nombre **smarthotelpetchecker** y lo coloca en el grupo de recursos ContosoRG, y un nuevo plan de App Service.

     ![Implementación de la función](./media/contoso-migration-rebuild/function2.png)

3. A continuación, selecciona la cuenta de almacenamiento y crea la función.

    ![Implementación de la función](./media/contoso-migration-rebuild/function3.png)

4. La implementación se inicia con el aprovisionamiento de la aplicación de función en Azure. En **Publicar**, Contoso agrega la configuración de la aplicación para el almacenamiento, la base de datos de Cosmos y Computer Vision API.

    ![Implementación de la función](./media/contoso-migration-rebuild/function4.png)

5. Para ejecutar primero la función localmente, se actualiza la configuración de **PetCheckerFunction/local.settings.json**.

    ![Implementación de la función](./media/contoso-migration-rebuild/function5.png)

6. Una vez implementada la función, aparece en Azure Portal, con el estado **En ejecución**.

    ![Implementación de la función](./media/contoso-migration-rebuild/function6.png)


7. Contoso navega hasta la aplicación para probar si Pet Checker AI funciona según lo previsto, en [http://smarthotel360public.azurewebsites.net/Pets](http://smarthotel360public.azurewebsites.net/Pets).
8. Hace clic en el avatar para cargar una imagen.

    ![Implementación de la función](./media/contoso-migration-rebuild/function7.png)

9. La primera foto que quiere comprobar es la de un perro pequeño.

    ![Implementación de la función](./media/contoso-migration-rebuild/function8.png)

10. La aplicación devuelve un mensaje de aceptación.

    ![Implementación de la función](./media/contoso-migration-rebuild/function9.png)




## <a name="review-the-deployment"></a>Revisión de la implementación

Con los recursos migrados de Azure, Contoso debe proteger la infraestructura nueva y ponerla totalmente en marcha.

### <a name="security"></a>Seguridad

- Contoso necesita asegurarse de que sus nuevas bases de datos son seguras. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- La aplicación necesita actualizarse para usar SSL con certificados. La instancia del contenedor debe reimplementarse para responder en 443.
- Debería considerarse el uso de KeyVault para proteger los secretos de las aplicaciones de Service Fabric. [Más información](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>Copia de seguridad y recuperación ante desastres

- Contoso necesita revisar los requisitos de copia de seguridad para la instancia de Azure SQL Database. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Debe considerar la implementación de grupos de conmutación por error de SQL para proporcionar conmutación por error regional para la base de datos. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Puede aprovechar la replicación geográfica para el SKU premium de ACR. [Más información](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

- Una vez implementados todos los recursos, Contoso debe asignar etiquetas de Azure según la [planificación de su infraestructura](contoso-migration-infrastructure.md#set-up-tagging).
- Todas las licencias se integran en el costo de los servicios de PaaS que está consumiendo Contoso. Esto se deducirá del contrato Enterprise.
- Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management.

## <a name="conclusion"></a>Conclusión

En este artículo, Contoso recompila la aplicación de SmartHotel de Azure. Recompila la VM de front-end de la aplicación local a las aplicaciones web de Azure App Services. El back-end de la aplicación se compila con microservicios implementados en contenedores administrados por Azure Kubernetes Service (AKS). Así mismo, mejora la funcionalidad de la aplicación con una aplicación de fotos de mascotas.




