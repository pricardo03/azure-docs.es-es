---
title: Creación de una canalización de CI/CD para el código existente con Azure DevOps Projects | Tutorial de VSTS
description: Azure DevOps Projects facilita empezar a usar Azure. Le ayuda a usar su propio código y repositorio GitHub para iniciar una aplicación en un servicio Azure de su elección en pocos pasos rápidos.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 8c92b45cd3949e56515286c963b035e3c449835b
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967450"
---
# <a name="create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Creación de una canalización de CI/CD para el código existente con Azure DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o elegir una de las aplicaciones de ejemplo para crear una canalización de integración continua (CI) y entrega continua (CD) en Azure.

Podrá:

> [!div class="checklist"]
> * Crear un proyecto de Azure DevOps Projects
> * Configurar el acceso al repositorio de GitHub y elegir un marco de trabajo
> * Configurar VSTS y una suscripción a Azure 
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Examen de la canalización de CI/CD en VSTS
> * Configurar la supervisión de Azure Application Insights

## <a name="prerequisites"></a>requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Acceso a un GitHub o repositorio Git externo que contenga código.NET, Java, PHP, Node, Python o código web estático.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Azure DevOps Projects crea una canalización de CI/CD en VSTS.  Puede crear una **nueva cuenta de VSTS** o usar una **existente**.  Azure DevOps Projects también crea **recursos de Azure** en la **suscripción de Azure** de su elección.

1. Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com).

1. Elija el icono **+ Nuevo** en la barra de navegación izquierda y, después, busque **DevOps Projects**.  Seleccione **Create**.

    ![Inicio de la entrega continua](_img/azure-devops-project-github/fullbrowser.png)

1. Seleccione **Traiga su propio código**.  Cuando termine, elija **Siguiente**.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>Configurar el acceso al repositorio de GitHub y elegir un marco de trabajo

1. Seleccione **GitHub**.  Si lo desea, puede elegir un **repositorio Git externo**.  Elija el **repositorio** y la **rama** que contiene la aplicación.

1. Seleccione su **marco web**.  Cuando termine, elija **Siguiente**.

    ![Marco .NET](_img/azure-devops-project-github/webframework.png)

1. El marco de trabajo de la aplicación que eligió en los pasos anteriores, determina el tipo de destino de implementación del servicio de Azure disponible aquí.  Seleccione el **servicio de destino** de su elección.  Cuando termine, elija **Siguiente**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar VSTS y una suscripción a Azure 

1. Cree una cuenta **nueva** de VSTS o elija una **existente**.  Elija un **nombre** para el proyecto de VSTS.  Seleccione la **suscripción de Azure**, la **ubicación** y elija un **nombre** para la aplicación.  Cuando termine, elija **Listo**.

    ![Introducción de la información de VSTS](_img/azure-devops-project-github/vstsazureinfo.png)

1. En unos minutos, el **panel del proyecto** se carga en Azure Portal.  Una aplicación de ejemplo se configura en un repositorio en la cuenta de VSTS, se ejecuta una compilación y la aplicación se implementa en Azure.  Este panel proporciona visibilidad sobre el **repositorio de código** de GitHub, la **canalización de CI/CD en VSTS** y la **aplicación de Azure**.  En el lado derecho del panel, seleccione **Examinar** para ver la aplicación en ejecución.

    ![Vista de panel](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps Projects configura automáticamente una compilación de CI y un desencadenador de versión.  El código permanece en el GitHub o en otro repositorio externo.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Confirmar cambios en GitHub e implementarlos automáticamente en Azure 

Ahora ya puede colaborar con un equipo en una aplicación de Java con un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web.  Cada cambio realizado en el repositorio de GitHub inicia una compilación en VSTS y una definición de VSTS Release Management ejecuta una implementación en Azure.

1.  Realice un cambio en la aplicación y **confirme** el cambio en el repositorio de GitHub.
2.  En unos instantes, se inicia una compilación en VSTS.  Puede supervisar el estado de compilación con el panel de proyecto de DevOps o en el explorador con la cuenta de VSTS.
3.  Una vez completada la compilación, **actualícela** en el explorador para comprobar que ve los cambios.

## <a name="examine-the-vsts-cicd-pipeline"></a>Examen de la canalización de CI/CD en VSTS

Azure DevOps Projects ha configurado automáticamente una canalización de VSTS CI/CD completa en la cuenta de VSTS.  Explore y personalice la canalización según sea necesario.  Siga estos pasos para familiarizarse con las definiciones de compilación y de versión de VSTS.

1. Seleccione **Compilar canalizaciones** en la **parte superior** del panel de proyecto de Azure DevOps.  Este vínculo abre una pestaña del explorador y abre la definición de compilación de VSTS para el nuevo proyecto.

1. Mueva el cursor a la derecha de la definición de compilación junto al campo **Estado**. Seleccione los **puntos suspensivos** que aparecen.  Esta acción abre un menú en el que se pueden iniciar varias actividades, como poner en cola una nueva compilación, pausar una compilación y editar la definición de compilación.

1. Seleccione **Editar**.

1. Desde esta vista, **examine las diversas tareas** para la definición de compilación.  La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio Git, restaurar dependencias y publicar salidas usadas para implementaciones.

1. En la parte superior de la definición de compilación, seleccione el **nombre de la definición de compilación**.

1. Cambie el **nombre** de la definición de compilación por algo más descriptivo.  Seleccione **Guardar y poner en cola** y, después, seleccione **Guardar**.

1. En el nombre de la definición de compilación, seleccione **Historial**.  Verá un registro de auditoría de los cambios recientes de la compilación.  VSTS realiza un seguimiento de los cambios realizados en la definición de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**.  El proyecto Azure DevOps Projects ha creado automáticamente un desencadenador de CI y cada confirmación al repositorio da inicio a una nueva compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**.  En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

1. Seleccione **Compilación y versión** y, después, elija **Versiones**.  Azure DevOps Projects creó una definición de versión de VSTS para administrar las implementaciones en Azure.

1. En el lado izquierdo del explorador, seleccione los **puntos suspensivos** junto a la definición de versión y, después elija **Editar**.

1. La definición de versión contiene una **canalización**, que define el proceso de lanzamiento.  En **Artefactos**, seleccione **Colocar**.  La definición de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. En el lado derecho del icono **Colocar**, seleccione el **desencadenador de implementación continua**.  Esta definición de versión tiene un desencadenador de CD habilitado, que ejecuta una implementación cada vez que hay un nuevo artefacto de compilación disponible.  Opcionalmente, puede deshabilitar el desencadenador, por lo que las implementaciones van a requerir una ejecución manual. 

1. En el lado izquierdo del explorador, seleccione **Tareas**.  Las tareas son las actividades que realiza el proceso de implementación.  En este ejemplo, se crea una tarea para implementarse en **Azure App Service**.

1. En el lado derecho del explorador, seleccione **Ver versiones**.  Esta vista muestra un historial de versiones.

1. Seleccione los **puntos suspensivos** junto a una de las versiones y elija **Abrir**.  Hay varios menús para explorar desde esta vista, como un resumen de versiones, elementos de trabajo asociados y pruebas.

1. Seleccione **Confirmaciones**.  Esta vista muestra las confirmaciones de código asociadas a la implementación específica. 

1. Seleccione **Registros**.  Los registros contienen información útil sobre el proceso de implementación.  Pueden verse durante y después de las implementaciones.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar la supervisión de Azure Application Insights

Con Azure Application Insights puede supervisar fácilmente el rendimiento y la utilización de la aplicación.  El proyecto de Azure DevOps Projects configuró automáticamente un recurso de Application Insights para su aplicación.  Puede seguir configurando diversas alertas y supervisando las funcionalidades según sea necesario.

1. Vaya al panel **Azure DevOps Projects** en Azure Portal.  En la esquina inferior derecha del panel, elija el vínculo **Application Insights** para la aplicación.

1. La hoja **Application Insights** en Azure Portal.  Esta vista contiene información sobre la supervisión de la disponibilidad, el rendimiento y el uso de la aplicación.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Seleccione **Intervalo de tiempo** y, a continuación, elija **Última hora**.  Seleccione **Actualización** para filtrar los resultados.  Ahora ve toda la actividad de los últimos 60 minutos.  Seleccione la **x** para salir del intervalo de tiempo.

1. Seleccione **Alertas** y, a continuación, seleccione **+ Agregar alerta de métrica**.  

1. Escriba un **nombre** para la alerta.

1. Seleccione la lista desplegable para **Source Alter on** (Modificación del origen en).  Elija su **recurso de App Service.**

1. La alerta predeterminada es para un **tiempo de respuesta de servidor mayor de 1 segundo**.  Seleccione el cuadro desplegable **Métrica** para examinar las distintas métricas de alertas.  Puede configurar fácilmente diversas alertas para mejorar las funcionalidades de supervisión de la aplicación.

1. Seleccione la casilla **Notify via Email owners, contributors, and readers** (Notificar a través de correo electrónico a lectores, colaboradores y propietarios).  Si lo desea, puede realizar acciones adicionales cuando se desencadene una alerta mediante la ejecución de una aplicación de lógica de Azure.

1. Elija **Aceptar** para crear la alerta.  En unos instantes, aparece como activa en el panel.  **Salga** del área de alertas y vuelva a la **hoja Application Insights**.

1. Seleccione **Disponibilidad** y, a continuación, **+ Agregar prueba**. 

1. Escriba un **Nombre de la prueba** y, a continuación, elija **Crear**.  Así se crea la prueba de ping simple para comprobar la disponibilidad de la aplicación.  Después de unos minutos, los resultados de la prueba están disponibles y el panel de Application Insights muestra un estado de disponibilidad.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, puede eliminar Azure App Service y los recursos relacionados creados en esta guía de inicio rápido mediante la funcionalidad **Eliminar**, que se encuentra en el panel de Azure DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Cuando configuró el proceso de CI/CD en este tutorial, se creó automáticamente una definición de compilación y de versión en el proyecto de VSTS. Puede modificar estas definiciones de compilación y de versión para satisfacer las necesidades de su equipo. Ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de Azure DevOps Projects
> * Configurar el acceso al repositorio de GitHub y elegir un marco de trabajo
> * Configurar VSTS y una suscripción a Azure 
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Examen de la canalización de CI/CD en VSTS
> * Configurar la supervisión de Azure Application Insights

Para más información sobre la canalización en VSTS, consulte este tutorial:

> [!div class="nextstepaction"]
> [Personalización del proceso de CD](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
