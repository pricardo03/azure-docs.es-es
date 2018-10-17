---
title: Implementación de la aplicación de ASP.NET Core en Azure Service Fabric con Azure DevOps Projects | Tutorial de Azure DevOps Services
description: Azure DevOps Projects facilita empezar a usar Azure. Azure DevOps Projects simplifica la implementación de una aplicación de ASP.NET Core con Azure Service Fabric en unos pasos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 6a0539b2213b99e09021a4f90d914172eac62560
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300406"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-service-fabric-with-the-azure-devops-project"></a>Tutorial: Implementación de la aplicación de ASP.NET Core en Azure Service Fabric con Azure DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o elegir una de las aplicaciones de ejemplo para crear una canalización de integración continua (CI) y entrega continua (CD) en Azure.  El proyecto de DevOps crea automáticamente recursos de Azure, como Azure Service Fabric, crea y configura una canalización de versión en Azure DevOps que incluye la configuración de una canalización de CI/CD, y crea un recurso de Azure Application Insights para la supervisión.

Podrá:

> [!div class="checklist"]
> * Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Service Fabric
> * Configuración de Azure DevOps Services y una suscripción de Azure 
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * Confirmación de los cambios en Git e implementación automática en Azure
> * Limpieza de recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-service-fabric"></a>Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Service Fabric

El proyecto de Azure DevOps crea una canalización de CI/CD.  Puede crear una organización de **Azure DevOps Services nueva** o usar una **existente**.  Azure DevOps Projects también crea **recursos de Azure** como un clúster de Service Fabric en la **suscripción de Azure** de su elección.

1. Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com).

1. Elija el icono **Crear un recurso** en la barra de navegación izquierda y busque **Proyecto de DevOps**.  Seleccione **Create**.

    ![Inicio de la entrega continua](_img/azure-devops-project-service-fabric/fullbrowser.png)

1. Seleccione **.NET** y, a continuación, elija **Siguiente**.

1. En **Elegir un marco de trabajo de la aplicación**, seleccione **ASP.NET** y, después, seleccione **Siguiente**.

1. Seleccione **Clúster de Service Fabric** y, después, elija **Siguiente**.  

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configuración de Azure DevOps Services y una suscripción de Azure

1. Cree una **nueva** organización de Azure DevOps Services o elija una **existente**.  Elija un **nombre** para el proyecto de Azure DevOps.  

1. Seleccione la **suscripción de Azure**.

1. Seleccione el vínculo **Cambiar** para ver los valores de configuración adicionales de Azure e identifique el **tamaño de la máquina virtual de del nodo** y el **sistema operativo** para el **clúster de Service Fabric**.  Hay varias opciones aquí para configurar el tipo y la ubicación de los servicios de Azure.
 
1. Salga del área de configuración de Azure y elija **Listo**.

1. El proceso puede tardar varios minutos en completarse.  Se configura una aplicación ASP.NET Core de ejemplo en un repositorio de la organización de Azure DevOps Services, se crea un clúster de Service Fabric, se ejecuta una canalización de CI/CD y la aplicación se implementa en Azure.  

    Cuando haya terminado, el **panel del proyecto** de Azure DevOps se cargará en Azure Portal.  También puede ir al **panel de Azure DevOps Projects** directamente desde **Todos los recursos** en **Azure Portal**.  

    Este panel proporciona visibilidad sobre el **repositorio de código** de Azure DevOps Services, la **canalización de CI/CD en Azure DevOps Services**, y el **clúster de Service Fabric**.  Puede seguir configurando opciones adicionales en Azure DevOps Services.  En el lado derecho del panel, seleccione **Examinar** para ver la aplicación en ejecución.

## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Examen de la canalización de CI de Azure DevOps Services

Azure DevOps Projects configura automáticamente una canalización de CI/CD de Azure DevOps Services en la organización de Azure DevOps Services.  Puede explorar y personalizar la canalización.  Siga estos pasos para familiarizarse con la canalización de compilación de Azure DevOps Services.

1. Vaya al **panel de Azure DevOps Projects**.

1. Seleccione **Compilar canalizaciones** en la **parte superior** del **panel del proyecto de Azure DevOps**.  Este vínculo abre una pestaña del explorador y la canalización de compilación de Azure DevOps Services para el nuevo proyecto.

1. Mueva el cursor a la derecha de la canalización de compilación junto al campo **Estado**. Seleccione los **puntos suspensivos** que aparecen.  Esta acción abre un menú en el que se pueden iniciar varias actividades, como **poner en cola una nueva compilación**, **pausar una compilación** y **editar la canalización de compilación**.

1. Seleccione **Editar**.

1. Desde esta vista, **examine las diversas tareas** de la canalización de compilación.  La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio Git de Azure DevOps Services, restaurar dependencias y publicar salidas usadas para las implementaciones.

1. En la parte superior de la canalización de compilación, seleccione el **nombre de esta**. 

1. En el nombre de la canalización de compilación, seleccione **Historial**.  Verá un registro de auditoría de los cambios recientes de la compilación.  Azure DevOps Services realiza un seguimiento de los cambios realizados en la canalización de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**.  El proyecto Azure DevOps Projects ha creado automáticamente un desencadenador de CI y cada confirmación al repositorio da inicio a una nueva compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**.  En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Examen de la canalización de CD de Azure DevOps Services

Azure DevOps Projects crea y configura automáticamente los pasos necesarios para implementar desde la organización de Azure DevOps Services en la suscripción de Azure.  Estos pasos incluyen la configuración de una conexión de servicio de Azure para realizar la autenticación de Azure DevOps Services en la suscripción de Azure.  La automatización también crea una canalización de versión de Azure DevOps Services, la cual proporciona la implementación continua en Azure.  Siga estos pasos para más información acerca de la canalización de versión de Azure DevOps Services.

1. Seleccione **Compilación y versión** y, después, elija **Versiones**.  EL proyecto de Azure DevOps creó una canalización de versión de Azure DevOps Services para administrar las implementaciones en Azure.

1. En el lado izquierdo del explorador, seleccione los **puntos suspensivos** junto a la canalización de versión y elija **Editar**.

1. La canalización de versión contiene una **canalización**, que define el proceso de lanzamiento.  En **Artefactos**, seleccione **Colocar**.  La canalización de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. En el lado derecho del icono **Colocar**, seleccione el **icono** de **desencadenador de implementación continua** (que aparece como un rayo).  Esta canalización de versión tiene habilitado un desencadenador de CD.  El desencadenador inicia una implementación cada vez que hay disponible un nuevo artefacto de compilación.  Si lo desea, puede deshabilitar el desencadenador, por lo que las implementaciones, entonces, requerirán la ejecución manual. 

1. En el lado derecho del explorador, seleccione **Ver versiones**.  Esta vista muestra un historial de versiones.

1. Seleccione los **puntos suspensivos** junto a una de las versiones y elija **Abrir**.  Hay varios menús para explorar desde esta vista, como un **resumen de versión**, **elementos de trabajo asociados** y **Pruebas**.

1. Seleccione **Confirmaciones**.  Esta vista muestra las confirmaciones de código asociadas a la implementación específica. Puede comparar las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Registros**.  Los registros contienen información útil sobre el proceso de implementación.  Pueden verse durante y después de las implementaciones.

## <a name="commit-changes-to-git-and-automatically-deploy-to-azure"></a>Confirmación de los cambios en Git e implementación automática en Azure 

 > [!NOTE]
 > Los siguientes pasos prueban la canalización de CI/CD con un simple cambio de texto en su aplicación web.

Ahora ya puede colaborar con un equipo en una aplicación de Java con un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web.  Cada cambio realizado en el repositorio de Git inicia una compilación y un lanzamiento implementa los cambios en Azure.  Siga estos pasos o use otras técnicas para confirmar los cambios en el repositorio.  Por ejemplo, puede **clonar** el repositorio Git en su herramienta favorita o IDE, y luego insertar los cambios en este repositorio.

1. Seleccione **Código** y después **Archivos** en el menú de Azure DevOps Services y vaya al repositorio.
1. Vaya al directorio **Views\Home**, seleccione los **puntos suspensivos** junto al archivo **Index.cshtml** y, después, elija **Editar**.

1. Realice un cambio en el archivo como parte del texto dentro de una de las **etiquetas div**.  En la esquina superior derecha, seleccione **Confirmar**.  Seleccione **Confirmar** de nuevo para insertar el cambio. 

1. En unos instantes, una **compilación se inicia** y, a continuación, se ejecuta el lanzamiento para implementar los cambios.  Puede supervisar el **estado de compilación** con el panel del proyecto de DevOps o en el explorador con el registro en tiempo real de Azure DevOps Services.

1. Una vez completada la versión, **actualice la aplicación** en el explorador para comprobar que ve los cambios.

## <a name="clean-up-resources"></a>Limpieza de recursos

 > [!NOTE]
 > Con los pasos siguientes se eliminarán permanentemente los recursos.  Use esta funcionalidad solo después de leer con detenimiento las indicaciones.

Si está haciendo pruebas, puede limpiar los recursos para evitar acumular cargos de facturación.  Cuando ya no sea necesario, puede eliminar el clúster de Azure Service Fabric y los recursos relacionados creados en este tutorial utilizando la funcionalidad **Eliminar** en el panel de Azure DevOps Projects.  **Tenga cuidado**, ya que la funcionalidad de eliminación destruye los datos creados por el proyecto de Azure DevOps en Azure y Azure DevOps Services, y no podrá recuperarlos una vez que hayan desaparecido.

1. En **Azure Portal**, vaya al proyecto **Azure DevOps Projects**.
2. En la **parte superior derecha** del panel, seleccione **Eliminar**.  Después de leer el mensaje, seleccione **Sí** para **eliminar permanentemente** los recursos.

## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede modificar las canalizaciones de CI/CD de Azure para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para los demás proyectos.  Ha aprendido a:

> [!div class="checklist"]
> * Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Service Fabric
> * Configuración de Azure DevOps Services y una suscripción de Azure 
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * Confirmación de los cambios en Git e implementación automática en Azure
> * Limpieza de recursos

Para más información acerca de Service Fabric y los microservicios, consulte a continuación:

> [!div class="nextstepaction"]
> [Uso de un enfoque de microservicios para crear aplicaciones](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
