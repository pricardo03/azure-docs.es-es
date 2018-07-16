---
title: Implementación de la aplicación de ASP.NET en Azure Virtual Machines con Azure DevOps Projects | Tutorial de VSTS
description: Azure DevOps Projects facilita empezar a usar Azure. El proyecto de Azure DevOps Projects simplifica la implementación de una aplicación de ASP.NET en Azure Virtual Machines en unos pocos pasos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b5a9ce204f68d812da4dfcfebb18b79b9c70c6c9
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967443"
---
# <a name="tutorial--deploy-your-aspnet-app-to-azure-virtual-machines-with-the-azure-devops-project"></a>Tutorial: Implementación de la aplicación de ASP.NET en Azure Virtual Machines con Azure DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o elegir una de las aplicaciones de ejemplo para crear una canalización de integración continua (CI) y entrega continua (CD) en Azure.  El proyecto de DevOps crea automáticamente recursos de Azure, como una nueva máquina virtual de Azure, crea y configura una canalización de versión en VSTS que incluye una definición de versión para implementación continua y, a continuación, crea un recurso de Azure Application Insights para la supervisión.

Podrá:

> [!div class="checklist"]
> * Crear un proyecto de Azure DevOps para una aplicación de ASP.NET
> * Configurar VSTS y una suscripción a Azure 
> * Examinar la definición de compilación de CI de VSTS
> * Examinar la definición de Release Management de CD de VSTS
> * Confirmar cambios en VSTS e implementarlos automáticamente en Azure
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

## <a name="prerequisites"></a>requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app"></a>Crear un proyecto de Azure DevOps para una aplicación de ASP.NET

Azure DevOps Projects crea una canalización de CI/CD en VSTS.  Puede crear una **nueva cuenta de VSTS** o usar una **existente**.  Azure DevOps Projects también crea **recursos de Azure** como máquinas virtuales en la **suscripción de Azure** que prefiera.

1. Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com).

1. Elija el icono **+ Nuevo** en la barra de navegación izquierda y, después, busque **DevOps Projects**.  Seleccione **Create**.

    ![Inicio de la entrega continua](_img/azure-devops-project-github/fullbrowser.png)

1. Seleccione **.NET** y, a continuación, elija **Siguiente**.

1. En **Choose an application Framework** (Elegir una plataforma de aplicación), seleccione **ASP.NET** y, a continuación, elija **Siguiente**. 

1. El marco de trabajo de la aplicación que eligió en los pasos anteriores, determina el tipo de destino de implementación del servicio de Azure disponible aquí.  Seleccione la **Máquina Virtual** y, a continuación, elija **Siguiente**.

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurar VSTS y una suscripción a Azure

1. Cree una cuenta **nueva** de VSTS o elija una **existente**.  Elija un **nombre** para el proyecto de VSTS.  

1. Seleccione la **suscripción de Azure**.  Si lo desea, puede seleccionar el vínculo **Cambiar** y, a continuación, escribir más detalles de configuración, por ejemplo, para cambiar la ubicación de los recursos de Azure.
 
1. Escriba un **Nombre de máquina virtual**, un **Nombre de usuario** y una **Contraseña** para el nuevo recurso de máquina virtual de Azure y, a continuación, elija **Listo**.

1. Hay que esperar varios minutos para que la máquina virtual de Azure esté preparada.  Una aplicación de ASP.NET de ejemplo se configura en un repositorio en su cuenta de VSTS, se ejecuta la compilación y el lanzamiento, y la aplicación se implementa en la máquina virtual de Azure recién creada.  

    Cuando haya terminado, el **panel del proyecto** de Azure DevOps se carga en Azure Portal.  También puede ir al **panel de Azure DevOps Projects** directamente desde **Todos los recursos** en **Azure Portal**.  

    Este panel proporciona visibilidad sobre el **repositorio de código** de VSTS, la **canalización CI/CD de VSTS** y su **aplicación en Azure** en ejecución.    

    ![Vista de panel](_img/azure-devops-project-vms/dashboardnopreview.png)

1. El proyecto de Azure DevOps configura automáticamente una compilación de CI y lanza el desencadenador que implementa automáticamente los cambios del código en el repositorio.  Puede seguir configurando opciones adicionales en VSTS.  En el lado derecho del panel, seleccione **Examinar** para ver la aplicación en ejecución.
    
## <a name="examine-the-vsts-ci-build-definition"></a>Examinar la definición de compilación de CI de VSTS

Azure DevOps Projects configura automáticamente una canalización CI/CD de VSTS completa en su cuenta de VSTS.  Puede explorar y personalizar la canalización.  Siga los pasos siguientes para familiarizarse con la definición de compilación de VSTS.

1. Seleccione **Compilar canalizaciones** en la **parte superior** del **panel de proyecto de Azure DevOps**.  Este vínculo abre una pestaña del explorador y abre la definición de compilación de VSTS para el nuevo proyecto.

1. Mueva el cursor a la derecha de la definición de compilación junto al campo **Estado**. Seleccione los **puntos suspensivos** que aparecen.  De este modo, se abre un menú donde puede realizar varias actividades, como **poner en cola una compilación nueva**, **pausar una compilación** y **editar la definición de la compilación**.

1. Seleccione **Editar**.

1. Desde esta vista, **examine las diversas tareas** para la definición de compilación.  La compilación ejecuta varias tareas, como recuperar los orígenes desde el repositorio Git de VSTS, restaurar las dependencias y publicar las salidas usadas para las implementaciones.

1. En la parte superior de la definición de compilación, seleccione el **nombre de la definición de compilación**.

1. Cambie el **nombre** de la definición de compilación por algo más descriptivo.  Seleccione **Guardar y poner en cola** y, después, seleccione **Guardar**.

1. En el nombre de la definición de compilación, seleccione **Historial**.  Verá un registro de auditoría de los cambios recientes de la compilación.  VSTS realiza un seguimiento de los cambios realizados en la definición de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**.  El proyecto Azure DevOps Projects ha creado automáticamente un desencadenador de CI y cada confirmación al repositorio da inicio a una nueva compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**.  En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Examinar la definición de Release Management de CD de VSTS

Azure DevOps Projects crea y configura automáticamente los pasos necesarios para implementar desde su cuenta de VSTS en su suscripción de Azure.  Estos pasos incluyen la configuración de una conexión de servicio de Azure para realizar la autenticación de VSTS en su suscripción de Azure.  La automatización también crea una definición de versión de VSTS y esto proporciona el CD para la máquina virtual de Azure.  Siga estos pasos para tener más información acerca de la definición de versión de VSTS.

1. Seleccione **Compilación y versión** y, después, elija **Versiones**.  Azure DevOps Projects creó una definición de versión de VSTS para administrar las implementaciones en Azure.

1. En el lado izquierdo del explorador, seleccione los **puntos suspensivos** junto a la definición de versión y, después elija **Editar**.

1. La definición de versión contiene una **canalización**, que define el proceso de lanzamiento.  En **Artefactos**, seleccione **Colocar**.  La definición de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. En el lado derecho del icono **Colocar**, seleccione el **icono** de **desencadenador de implementación continua** (que aparece como un rayo).  Esta definición de versión tiene habilitado un desencadenador de CD.  El desencadenador inicia una implementación cada vez que hay disponible un nuevo artefacto de compilación.  Si lo desea, puede deshabilitar el desencadenador, por lo que las implementaciones, entonces, requerirán la ejecución manual. 

1. En el lado izquierdo del explorador, seleccione **Tareas** y, a continuación, elija su **entorno**.  

1. Las tareas son las actividades que ejecuta el proceso de implementación y se agrupan en **Fases**.  Hay dos **Fases** para esta definición de versión.  La primera contiene una tarea de **implementación del grupo de recursos de Azure** que configura la máquina virtual para la implementación y agrega la nueva máquina virtual a un **grupo de implementación de VSTS**.  El grupo de implementación de máquina virtual en VSTS administra los grupos lógicos de las máquinas de **destino de la implementación**.

1. En esta segunda fase, se creó una tarea de **administración de aplicación Web de IIS** para crear un sitio Web de IIS en la máquina virtual.  Una segunda tarea de **implementación de aplicación Web de IIS** se creó para implementar el sitio.

1. En el lado derecho del explorador, seleccione **Ver versiones**.  Esta vista muestra un historial de versiones.

1. Seleccione los **puntos suspensivos** junto a una de las versiones y elija **Abrir**.  Hay varios menús para explorar desde esta vista, como un **resumen de versión**, **elementos de trabajo asociados** y **Pruebas**.

1. Seleccione **Confirmaciones**.  Esta vista muestra las confirmaciones de código asociadas a la implementación específica. Compare las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Registros**.  Los registros contienen información útil sobre el proceso de implementación.  Pueden verse durante y después de las implementaciones.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Confirmar cambios en VSTS e implementarlos automáticamente en Azure 

Ahora ya puede colaborar con un equipo en una aplicación de Java con un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web.  Cada cambio realizado en el repositorio de git de VSTS inicia una compilación en VSTS y una definición de VSTS Release Management ejecuta una implementación en la máquina virtual de Azure.  Siga estos pasos o use otras técnicas para confirmar los cambios en el repositorio.  Los cambios de código inician el proceso de CI/CD y se implementan automáticamente los nuevos cambios en el sitio Web de IIS en la máquina virtual de Azure.

1. Seleccione **Código** en el menú de VSTS y vaya al repositorio.

1. Vaya al directorio **Views\Home**, seleccione los **puntos suspensivos** junto al archivo **Index.cshtml** y, después, elija **Editar**.

1. Realice un cambio en el archivo como parte del texto dentro de una de las **etiquetas div**.  En la esquina superior derecha, seleccione **Confirmar**.  Seleccione **Confirmar** de nuevo para insertar el cambio. 

1. En unos instantes, una **compilación se inicia en VSTS** y, a continuación, se ejecuta el lanzamiento para implementar los cambios.  Supervise el **estado de compilación** con el panel de proyecto de DevOps o en el explorador con la cuenta de VSTS.

1. Una vez completada la versión, **actualice la aplicación** en el explorador para comprobar que ve los cambios.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar la supervisión de Azure Application Insights

Con Azure Application Insights puede supervisar fácilmente el rendimiento y la utilización de la aplicación.  El proyecto de Azure DevOps Projects configuró automáticamente un recurso de Application Insights para su aplicación.  Puede seguir configurando diversas alertas y supervisando las funcionalidades según sea necesario.

1. Vaya al panel de **Azure DevOps Projects** en **Azure Portal**.  En la esquina inferior derecha del panel, elija el vínculo **Application Insights** para la aplicación.

1. La hoja **Application Insights** en Azure Portal.  Esta vista contiene información sobre la supervisión de la disponibilidad, el rendimiento y el uso de la aplicación.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Seleccione **Intervalo de tiempo** y, a continuación, elija **Última hora**.  Seleccione **Actualización** para filtrar los resultados.  Ahora ve toda la actividad de los últimos 60 minutos.  Seleccione la **x** para salir del intervalo de tiempo.

1. Puede encontrar **Alertas** y otros vínculos útiles en la parte superior del panel.  Seleccione **Alertas** y, a continuación, seleccione **+ Agregar alerta de métrica**.

1. Escriba un **Nombre** para la alerta.

1. La alerta predeterminada es para un **tiempo de respuesta de servidor mayor de 1 segundo**.  Seleccione el cuadro desplegable **Métrica** para examinar las distintas métricas de alertas.  Por ejemplo, puede configurar el valor de **Tiempo de ejecución de solicitud ASP.NET** para una aplicación ASP.NET.  Puede configurar fácilmente diversas alertas para mejorar las funcionalidades de supervisión de la aplicación.

1. Seleccione la casilla **Notify via Email owners, contributors, and readers** (Notificar a través de correo electrónico a lectores, colaboradores y propietarios).  Si lo desea, puede realizar acciones adicionales cuando se desencadene una alerta mediante la ejecución de una aplicación de lógica de Azure.

1. Elija **Aceptar** para crear la alerta.  En unos instantes, aparece como activa en el panel.  **Salga** del área de alertas y vuelva a la **hoja Application Insights**.

1. Seleccione **Disponibilidad** y, a continuación, **+ Agregar prueba**. 

1. Escriba un **Nombre de la prueba** y, a continuación, elija **Crear**.  Así se crea la prueba de ping simple para comprobar la disponibilidad de la aplicación.  Después de unos minutos, los resultados de la prueba están disponibles y el panel de Application Insights muestra un estado de disponibilidad.

## <a name="clean-up-resources"></a>Limpieza de recursos

 > [!NOTE]
 > Con los pasos siguientes se eliminarán permanentemente los recursos.  Use esta funcionalidad solo después de leer con detenimiento las indicaciones.

Si está haciendo pruebas, puede limpiar los recursos para evitar acumular cargos de facturación.  Cuando ya no sea necesario, puede eliminar la máquina virtual de Azure y los recursos relacionados creados en este tutorial utilizando la funcionalidad **Eliminar** en el panel de Azure DevOps Projects.  **Tenga cuidado**, ya que la funcionalidad de eliminación destruye los datos creados por el proyecto Azure DevOps Projects en Azure y VSTS, y no podrá recuperarlos una vez que hayan desaparecido.

1. En **Azure Portal**, vaya al proyecto **Azure DevOps Projects**.
2. En la **parte superior derecha** del panel, seleccione **Eliminar**.  Después de leer el mensaje, seleccione **Sí** para **eliminar permanentemente** los recursos.

## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede modificar estas definiciones de compilación y de versión para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para los demás proyectos.  Ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de Azure DevOps para una aplicación de ASP.NET
> * Configurar VSTS y una suscripción a Azure 
> * Examinar la definición de compilación de CI de VSTS
> * Examinar la definición de Release Management de CD de VSTS
> * Confirmar cambios en VSTS e implementarlos automáticamente en Azure
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

Para más información sobre la canalización en VSTS, consulte este tutorial:

> [!div class="nextstepaction"]
> [Personalización del proceso de CD](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
