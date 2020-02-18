---
title: Creación de una canalización de CI/CD para Node.js con Azure DevOps Projects
description: Con DevOps Projects es fácil empezar a usar Azure. En pocos y rápidos pasos le ayuda a iniciar una aplicación en el servicio de Azure de su elección.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
monikerRange: vsts
ms.openlocfilehash: 35eebeaa393ff75ada11752aaf9f195efddfa12b
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049798"
---
#  <a name="quickstart-create-a-cicd-pipeline-in-azure-pipelines-for-nodejs-with-azure-devops-projects"></a>Inicio rápido: Creación de una canalización de CI/CD en Azure Pipelines para Node.js con Azure DevOps Projects

En este inicio rápido se usa la experiencia simplificada de Azure DevOps Projects para configurar una canalización de integración continua (CI) y entrega continua (CD) para una aplicación de Node.js en Azure Pipelines. Azure DevOps Projects se puede usar para configurar todo lo necesario para desarrollar, implementar y supervisar una aplicación. 

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). 
- Una cuenta de [Azure DevOps](https://azure.microsoft.com/services/devops/) y una organización.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

DevOps Projects crea una canalización de CI/CD en Azure Pipelines. Puede crear una organización de Azure DevOps nueva o usar una existente. DevOps Projects también crea recursos de Azure en la suscripción de Azure que prefiera.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y, en el panel izquierdo, seleccione **Crear un recurso**. 

   ![Creación de un recurso de Azure en Azure Portal](_img/azure-devops-project-nodejs/create-azure-resource.png)

1. Busque **DevOps Project**, selecciónelo y, después, seleccione **Crear**.

## <a name="select-a-sample-application-and-azure-service"></a>Seleccione una aplicación de ejemplo y el servicio de Azure

1. Seleccione la aplicación de ejemplo de Node.js.  
    Los ejemplos de Node.js incluyen una opción de varios marcos de trabajo de la aplicación.

1. El marco de trabajo de ejemplo predeterminado es Express.js. Deje la configuración predeterminada y seleccione **Siguiente**.  
    **Web App on Windows** es el destino de implementación predeterminado.  El marco de trabajo de la aplicación que ha elegido antes determina el tipo de destino de implementación del servicio de Azure disponible aquí.  

2. Deje el servicio predeterminado y seleccione **Siguiente**.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configuración de Azure DevOps y una suscripción de Azure 

1. Cree una organización de Azure DevOps nueva o elija una existente. 
   
   1. Escriba un nombre para el proyecto.
      
   1. Seleccione la suscripción de Azure y la ubicación, escriba el nombre de la aplicación y seleccione **Listo**.  
      En unos minutos, el panel de DevOps Projects se muestra en Azure Portal. Una aplicación de ejemplo se configura en un repositorio en la organización de Azure DevOps, se ejecuta una compilación y la aplicación se implementa en Azure. Este panel proporciona visibilidad sobre el repositorio de código, la canalización de CI/CD y la aplicación de Azure.
   
1. Seleccione **Examinar** para ver la aplicación en ejecución.
   
   ![Vista de panel de la canalización de CI/CD](_img/azure-devops-project-nodejs/devops-projects-dashboard.png) 

DevOps Projects ha configurado automáticamente una compilación de integración continua y un desencadenador de versión.  Ahora ya puede colaborar con un equipo en una aplicación de Node.js con un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web.

## <a name="commit-code-changes-and-execute-cicd"></a>Confirmación de los cambios de código y ejecución de CI/CD

DevOps Projects crea un repositorio de Git en Azure Repos o GitHub. Para ver el repositorio y realizar cambios en el código de la aplicación, siga los pasos descritos a continuación.

1. En el lado izquierdo del panel de DevOps Projects, seleccione el vínculo de la rama maestra.  
Este vínculo abre una vista al repositorio de Git recién creado.

1. Para ver la dirección URL de clonación del repositorio, seleccione **Clonar** en la parte superior derecha del explorador.   
    Puede clonar el repositorio de Git en su IDE favorito. En los pasos siguientes, puede usar el explorador web para realizar los cambios en el código y confirmarlos directamente en la rama maestra.

1. En el lado izquierdo del explorador, vaya al archivo **views/index.pug**.

1. Seleccione **Editar** y, a continuación, realice un cambio en el encabezado h2.  
    Por ejemplo, escriba **Empezar a trabajar de inmediato con Azure DevOps Projects** o haga cualquier otro cambio.

1. Seleccione **Confirmar** y guarde los cambios.

1. En el explorador, vaya al panel de DevOps Projects.   
Debería ver que hay una compilación en curso. Los cambios que acaba de realizar se compilan e implementan automáticamente a través de una canalización de CI/CD.

## <a name="examine-the-azure-cicd-pipeline"></a>Examen de la canalización de CI/CD de Azure

En el paso anterior, DevOps Projects configuró automáticamente una canalización de CI/CD completa. Explore y personalice la canalización según sea necesario. Siga estos pasos para familiarizarse con las canalizaciones de compilación y de versión.

1. En la parte superior del panel de DevOps Projects, seleccione **Compilar canalizaciones**.  
Este vínculo abre una pestaña del explorador y la canalización de compilación del nuevo proyecto.

1. Elija el campo **Estado** y seleccione los puntos suspensivos (...).  
    Esta acción abre un menú en el que se pueden iniciar varias actividades, como poner en cola una nueva compilación, pausar una compilación y editar la canalización de compilación.

1. Seleccione **Editar**.

1. En este panel puede examinar las distintas tareas de la canalización de compilación.  
La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio Git, restaurar dependencias y publicar salidas que se usan para las implementaciones.

1. En la parte superior de la canalización de compilación, seleccione el nombre de esta.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo, seleccione **Guardar y poner en cola** y, luego, **Guardar**.

1. En el nombre de la canalización de compilación, seleccione **Historial**.   
En el panel **Historial** verá un registro de auditoría de los cambios recientes en la compilación.  Azure Pipelines realiza un seguimiento de los cambios que se realizan en la canalización de compilación y permite comparar versiones.

1. Seleccione **Desencadenadores**.   
 DevOps Projects creó automáticamente un desencadenador de integración continua y cada confirmación en el repositorio inicia una compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**.   
En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

1. Seleccione **Compilación y versión** y, después, **Versiones**.  
 DevOps Projects crea una canalización de versión para administrar implementaciones en Azure.

1. En la parte izquierda, seleccione los puntos suspensivos (...) que se encuentran junto a la canalización de versión, y, después, **Editar**.  
La canalización de versión define el proceso de versión.

12. En **Artefactos**, seleccione **Colocar**.  
    La canalización de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. Al lado del icono **Colocar**, seleccione **Desencadenador de implementación continua**.  
Esta canalización de versión tiene un desencadenador de implementación continua habilitado que ejecuta una implementación cada vez que hay un nuevo artefacto de compilación disponible. Opcionalmente, puede deshabilitar el desencadenador, con lo que las implementaciones van a requerir una ejecución manual. 


1. En el lado izquierdo, seleccione **Tareas**.   
Las tareas son las actividades que ejecuta el proceso de implementación. En este ejemplo, se ha creado una tarea que se implementa en Azure App Service.


1. A la derecha, seleccione **Ver versiones**.  
Esta vista muestra un historial de versiones.

1. Seleccione los puntos suspensivos (...) junto a una de las versiones y, luego, seleccione **Abrir**.  
Hay varios menús para explorar, como un resumen de las versiones, elementos de trabajo asociados y las pruebas.

1. Seleccione **Confirmaciones**.   
Esta vista muestra las confirmaciones de código asociadas a la implementación concreta.

1. Seleccione **Registros**.  
Los registros contienen información útil sobre el proceso de implementación. Pueden verse durante y después de las implementaciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Azure App Service y otros recursos relacionados que haya creado se pueden eliminar cuando no se necesiten. Use la funcionalidad de **eliminación** del panel de DevOps Projects.


## <a name="next-steps"></a>Pasos siguientes

Cuando configuró el proceso de CI/CD, se crearon automáticamente canalizaciones de compilación y de versión. Puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. Para más información acerca de la canalización de CI/CD, consulte:

> [!div class="nextstepaction"]
> [Personalización del proceso de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Vídeos

> [!VIDEO https://www.youtube.com/embed/3etwjubReJs]
