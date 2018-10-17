---
title: Implementación de la aplicación de ASP.NET en Azure SQL Database con Azure DevOps Projects | Tutorial de Azure DevOps Service
description: Azure DevOps Projects facilita empezar a usar Azure. El proyecto de Azure DevOps simplifica la implementación de una aplicación de ASP.NET con Azure SQL Database en unos pasos rápidos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: d9c7c94e344daee5af87ce40ddf4dcb686696ded
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297340"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Tutorial: Implementación de la aplicación de ASP.NET en Azure SQL Database con Azure DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o elegir una de las aplicaciones de ejemplo para crear una canalización de integración continua (CI) y entrega continua (CD) en Azure.  El proyecto de DevOps crea automáticamente recursos de Azure, como una instancia de Azure SQL Database, crea y configura una canalización de versión en Azure DevOps que incluye una canalización de compilación para la integración continua y configura otra para la implementación continua y, a continuación, crea un recurso de Azure Application Insights para la supervisión.

Podrá:

> [!div class="checklist"]
> * Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Azure SQL Database
> * Configuración de Azure DevOps Services y una suscripción de Azure 
> * Examen de la canalización de CI de Azure DevOps Services
> * Examen de la canalización de CD de Azure DevOps Services
> * Confirmación de los cambios en Azure DevOps Services e implementación automática en Azure
> * Conexión con la base de datos Azure SQL Server 
> * Limpieza de recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Azure SQL Database

Azure DevOps Projects crea una canalización de CI/CD en Azure.  Puede crear una organización de **Azure DevOps Services nueva** o usar una **existente**.  Azure DevOps Projects también crea **recursos de Azure** como una base de datos de Azure SQL Database en la **suscripción de Azure** de su elección.

1. Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com).

1. Elija el icono **Crear un recurso** en la barra de navegación izquierda y busque **Proyecto de DevOps**.  Seleccione **Create**.

    ![Inicio de la entrega continua](_img/azure-devops-project-github/fullbrowser.png)

1. Seleccione **.NET** y, a continuación, elija **Siguiente**.

1. En **Elegir un marco de trabajo de la aplicación**, seleccione **ASP.NET**.

1. Seleccione **Agregar una base de datos** y, después, elija **Siguiente**.  

1. El marco de trabajo de la aplicación que eligió en los pasos anteriores, determina el tipo de destino de implementación del servicio de Azure disponible aquí.  Seleccione **Next** (Siguiente).

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configuración de Azure DevOps Services y una suscripción de Azure

1. Cree una **nueva** organización de Azure DevOps Services o elija una **existente**.  Elija un **nombre** para el proyecto de Azure DevOps.  

1. Seleccione la **suscripción de Azure**.

1. Opcionalmente, seleccione el vínculo **Cambiar** para ver los valores de configuración adicionales de Azure e identifique el **nombre de usuario** en la sección **Detalles de inicio de sesión del servidor de bases de datos**.  **Almacene** el **nombre de usuario** para los pasos posteriores de este tutorial.
 
1. Si ha realizado el paso anterior, salga del área de configuración de Azure y elija **Listo**.  En caso contrario, simplemente seleccione **Listo**.

1. El proceso puede tardar varios minutos en completarse.  Cuando haya terminado, el **panel del proyecto** de Azure DevOps se cargará en Azure Portal.  También puede ir al **panel de Azure DevOps Projects** directamente desde **Todos los recursos** en **Azure Portal**.  En el lado derecho del panel, seleccione **Examinar** para ver la aplicación en ejecución.
    
## <a name="examine-the-azure-devops-services-ci-pipeline"></a>Examen de la canalización de CI de Azure DevOps Services

Azure DevOps Projects configura automáticamente una canalización de CI/CD de Azure completa en la organización de Azure DevOps Services.  Puede explorar y personalizar la canalización.  Siga estos pasos para familiarizarse con la canalización de compilación de Azure DevOps Services.

1. Vaya al **panel de Azure DevOps Projects**.

1. Seleccione **Compilar canalizaciones** en la **parte superior** del **panel del proyecto de Azure DevOps**.  Este vínculo abre una pestaña del explorador y la canalización de compilación de Azure DevOps Services para el nuevo proyecto.

1. Mueva el cursor a la derecha de la canalización de compilación junto al campo **Estado**. Seleccione los **puntos suspensivos** que aparecen.  De este modo, se abre un menú donde puede realizar varias actividades, como **poner en cola una compilación nueva**, **pausar una compilación** y **editar la canalización de la compilación**.

1. Seleccione **Editar**.

1. Desde esta vista, **examine las diversas tareas** de la canalización de compilación.  La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio Git de Azure DevOps Services, restaurar dependencias y publicar salidas usadas para las implementaciones.

1. En la parte superior de la canalización de compilación, seleccione el **nombre de esta**.

1. Cambie el **nombre** de la canalización de compilación por algo más descriptivo.  Seleccione **Guardar y poner en cola** y, después, seleccione **Guardar**.

1. En el nombre de la canalización de compilación, seleccione **Historial**.  Verá un registro de auditoría de los cambios recientes de la compilación.  Azure DevOps Services realiza un seguimiento de los cambios realizados en la canalización de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**.  El proyecto Azure DevOps Projects ha creado automáticamente un desencadenador de CI y cada confirmación al repositorio da inicio a una nueva compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**.  En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

## <a name="examine-the-azure-devops-services-cd-pipeline"></a>Examen de la canalización de CD de Azure DevOps Services

Azure DevOps Projects crea y configura automáticamente los pasos necesarios para implementar desde la organización de Azure DevOps Services en la suscripción de Azure.  Estos pasos incluyen la configuración de una conexión de servicio de Azure para realizar la autenticación de Azure DevOps Services en la suscripción de Azure.  La automatización también crea una definición de versión de Azure DevOps Services, la cual proporciona la implementación continua en Azure.  Siga estos pasos para más información acerca de la definición de versión de Azure DevOps Services.

1. Seleccione **Compilación y versión** y, después, elija **Versiones**.  EL proyecto de Azure DevOps creó una canalización de versión de Azure DevOps Services para administrar las implementaciones en Azure.

1. En el lado izquierdo del explorador, seleccione los **puntos suspensivos** junto a la canalización de versión y elija **Editar**.

1. La canalización de versión contiene una **canalización**, que define el proceso de lanzamiento.  En **Artefactos**, seleccione **Colocar**.  La canalización de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. En el lado derecho del icono **Colocar**, seleccione el **icono** de **desencadenador de implementación continua** (que aparece como un rayo).  Esta canalización de versión tiene habilitado un desencadenador de CD.  El desencadenador inicia una implementación cada vez que hay disponible un nuevo artefacto de compilación.  Si lo desea, puede deshabilitar el desencadenador, por lo que las implementaciones, entonces, requerirán la ejecución manual. 

1. Azure DevOps Projects configura una contraseña de SQL aleatoria que se usa en la canalización de versión.  En el lado izquierdo del explorador, seleccione **Variables**. 

1. **Solo puede realizar este paso si ha cambiado la contraseña de SQL Server.**  Hay una única **contraseña** variable.  A la derecha del cuadro de texto **Valor**, seleccione el icono del **candado**.  En **Contraseña**, escriba la nueva contraseña y seleccione **Guardar**.

1. En el lado izquierdo del explorador, seleccione **Tareas** y, a continuación, elija su **entorno**.  

1. Las tareas son las actividades se ejecuta el proceso de implementación y se agrupan en **Fases**.  Esta canalización de versión comprende una sola fase.  La fase contiene una tarea **Implementación de Azure App Service** e **implementación de Azure SQL Database**.

1. Seleccione la tarea **Ejecutar SQL de Azure** y examine las distintas propiedades usadas para la implementación de SQL.  En **Paquete de implementación**, observe que la tarea usa un **archivo SQL DACPAC**.

1. En el lado derecho del explorador, seleccione **Ver versiones**.  Esta vista muestra un historial de versiones.

1. Seleccione los **puntos suspensivos** junto a una de las versiones y elija **Abrir**.  Hay varios menús para explorar desde esta vista, como un **resumen de versión**, **elementos de trabajo asociados** y **Pruebas**.

1. Seleccione **Confirmaciones**.  Esta vista muestra las confirmaciones de código asociadas a la implementación específica. Puede comparar las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Registros**.  Los registros contienen información útil sobre el proceso de implementación.  Pueden verse durante y después de las implementaciones.

## <a name="commit-changes-to-azure-devops-services-and-automatically-deploy-to-azure"></a>Confirmación de los cambios en Azure DevOps Services e implementación automática en Azure 

 > [!NOTE]
 > Los siguientes pasos prueban la canalización de CI/CD con un simple cambio de texto.  Opcionalmente, puede realizar un cambio de esquema de SQL Server en la tabla para probar el proceso de implementación de SQL.

Ahora ya puede colaborar con un equipo en una aplicación de Java con un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web.  Cada cambio realizado en el repositorio de Git de Azure DevOps Services inicia una compilación en Azure DevOps Services y una canalización de CD de Azure DevOps Services ejecuta una implementación en Azure.  Siga estos pasos o use otras técnicas para confirmar los cambios en el repositorio.  Los cambios de código inician el proceso de CI/CD y se implementan automáticamente los nuevos cambios en Azure.

1. Seleccione **Código** en el menú de Azure DevOps Services y vaya al repositorio.

1. Vaya al directorio **SampleWebApplication\Views\Home**, seleccione los **puntos suspensivos** junto al archivo **Index.cshtml** y, después, elija **Editar**.

1. Realice un cambio en el archivo como parte del texto dentro de una de las **etiquetas div**.  En la esquina superior derecha, seleccione **Confirmar**.  Seleccione **Confirmar** de nuevo para insertar el cambio. 

1. En unos instantes, una **compilación se inicia en Azure DevOps Services** y, a continuación, se ejecuta el lanzamiento para implementar los cambios.  Puede supervisar el **estado de compilación** con el panel del proyecto de DevOps o en el explorador con la organización de Azure DevOps Services.

1. Una vez completada la versión, **actualice la aplicación** en el explorador para comprobar que ve los cambios.

## <a name="connect-to-the-azure-sql-server-database"></a>Conexión con la base de datos Azure SQL Server

Necesita los permisos adecuados para conectarse a Azure SQL Database.

1. En el panel de Azure DevOps Projects, seleccione **SQL Database** para ir a la página de administración de la base de datos SQL.
   
1. Seleccione **Establecer el firewall del servidor** y, a continuación, seleccione **+ Agregar IP de cliente**.  

1. Seleccione **Guardar**.  La dirección IP de cliente ahora puede tener acceso al **recurso de Azure de SQL Server**.

1. Vuelva a la hoja **SQL Database**. 

1. En el lado derecho de la pantalla, seleccione el **nombre del servidor** para ir a la página de configuración para **SQL Server**.

1. Seleccione **Restablecer contraseña**, escriba una contraseña para el **inicio de sesión de administrador de SQL Server** y, después, seleccione **Guardar**.  **Conserve** esta contraseña para los pasos siguientes de este tutorial.

1. Ahora puede, opcionalmente, usar las herramientas de cliente, como **SQL Server Management Studio** o **Visual Studio** para conectarse a Azure SQL Server y SQL Database.  Use la propiedad de **nombre del servidor** para conectarse.

   Si no ha cambiado el nombre de usuario de la base de datos mientras configuraba inicialmente el proyecto DevOps, el nombre de usuario es la parte local de la dirección de correo electrónico.  Por ejemplo, si la dirección de correo electrónico es johndoe@microsoft.com, el nombre de usuario es johndoe.

 > [!NOTE]
 > Si cambia la contraseña para el inicio de sesión SQL, necesita cambiarla también en la variable de la canalización de versión de Azure DevOps Services, tal y como se describe en la sección **Examen de la canalización de CD de Azure DevOps Services**.

## <a name="clean-up-resources"></a>Limpieza de recursos

 > [!NOTE]
 > Con los pasos siguientes se eliminarán permanentemente los recursos.  Use esta funcionalidad solo después de leer con detenimiento las indicaciones.

Si está haciendo pruebas, puede limpiar los recursos para evitar acumular cargos de facturación.  Cuando ya no sea necesario, puede eliminar la instancia de Azure SQL Database y los recursos relacionados creados en este tutorial utilizando la funcionalidad **Eliminar** en el panel de Azure DevOps Projects.  **Tenga cuidado**, ya que la funcionalidad de eliminación destruye los datos creados por el proyecto de Azure DevOps en Azure y Azure DevOps Services, y no podrá recuperarlos una vez que hayan desaparecido.

1. En **Azure Portal**, vaya al proyecto **Azure DevOps Projects**.
2. En la **parte superior derecha** del panel, seleccione **Eliminar**.  Después de leer el mensaje, seleccione **Sí** para **eliminar permanentemente** los recursos.

## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para los demás proyectos.  Ha aprendido a:

> [!div class="checklist"]
> * Creación de un proyecto de Azure DevOps para una aplicación ASP.NET Core y Azure SQL Database
> * Configuración de Azure DevOps Services y una suscripción de Azure 
> * Examen de la canalización de CI de Azure DevOps Services
> * Examen de la canalización de CD de Azure DevOps Services
> * Confirmación de los cambios en Azure DevOps Services e implementación automática en Azure
> * Conexión con la base de datos Azure SQL Server 
> * Limpieza de recursos

Para más información acerca de la canalización de Azure, consulte este tutorial:

> [!div class="nextstepaction"]
> [Personalización del proceso de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Vídeos

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]