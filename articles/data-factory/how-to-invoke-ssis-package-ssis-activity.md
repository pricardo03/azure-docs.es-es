---
title: 'Ejecución de paquetes SSIS con la actividad Ejecutar paquete de SSIS: Azure | Microsoft Docs'
description: En este artículo se describe cómo ejecutar un paquete de SQL Server Integration Services (SSIS) desde una canalización de Azure Data Factory mediante la actividad Ejecutar paquete de SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 09/13/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 9057cefa5108924c57dbc85bbb895b31e804a51c
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71000652"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Ejecución de un paquete de SSIS mediante la actividad Ejecutar paquete SSIS de Azure Data Factory
En este artículo se describe cómo ejecutar un paquete de SQL Server Integration Services (SSIS) desde una canalización de Azure Data Factory (ADF) mediante la actividad Ejecutar paquete de SSIS. 

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Cree una instancia de Azure-SSIS Integration Runtime (IR) si no tiene ya una. Para ello, siga las instrucciones paso a paso del [Tutorial: Aprovisionamiento de Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Ejecución de un paquete en Azure Portal
En esta sección, usará la interfaz de usuario (UI) o aplicación de ADF para crear una canalización de ADF con la actividad Ejecutar paquete de SSIS que ejecuta el paquete de SSIS.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Creación de una canalización con una actividad Ejecutar paquete de SSIS
En este paso, usará la interfaz de usuario o aplicación de ADF para crear una canalización. Agregará una actividad Ejecutar paquete de SSIS a la canalización y la configurará para ejecutar el paquete de SSIS. 

1. En la página de inicio o información general de ADF de Azure Portal, haga clic en el icono **Author & Monitor** (Creación y supervisión) para iniciar la interfaz de usuario o aplicación de ADF en una pestaña aparte. 

   ![Página principal Factoría de datos](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   En la página **Let's get started** (Empecemos), haga clic en **Create pipeline** (Crear canalización): 

   ![Página de introducción](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. En el cuadro de herramientas **Activities** (Actividades), expanda **General**, arrastre la actividad **Execute SSIS Package** (Ejecutar paquete de SSIS) y colóquela en la superficie del diseñador de canalizaciones. 

   ![Arrastrar la actividad Execute SSIS Package (Ejecutar paquete de SSIS) a la superficie del diseñador](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. En la pestaña **General** de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), proporcione un nombre y una descripción para la actividad. Establezca el tiempo de espera opcional y los valores de reintento.

   ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. En la pestaña **Configuración** de la actividad Ejecutar paquete de SSIS, seleccione una instancia de Azure-SSIS IR donde quiera ejecutar el paquete. Si el paquete usa autenticación de Windows para acceder a almacenes de datos, p. ej. recursos compartidos de archivos o servidores SQL Server en local, Azure Files, etc., active la casilla **Autenticación de Windows** y escriba los valores de las credenciales de ejecución del paquete (**Dominio**/**Nombre de usuario**/**Contraseña**). También puede usar secretos almacenados en Azure Key Vault (AKV) como sus valores. Para ello, haga clic en la casilla **AZURE KEY VAULT** junto a la credencial correspondiente, seleccione o edite el servicio vinculado de AKV existente o cree uno nuevo y luego seleccione el nombre o la versión del secreto para el valor de la credencial.  Cuando se crea o se edita el servicio vinculado de AKV, se puede seleccionar o editar la instancia de AKV existente o crear una nueva, pero hay que conceder acceso a la identidad administrada de ADF a la instancia de AKV si aún no se ha hecho. Los secretos también se pueden escribir directamente en el siguiente formato: `<AKV linked service name>/<secret name>/<secret version>`. Si el paquete necesita el entorno de ejecución de 32 bits para funcionar, active la casilla **32-Bit runtime** (Entorno de ejecución de 32 bits). 

   Para **Ubicación del paquete**, seleccione **SSISDB**, **Sistema de archivos (paquete)** o **Sistema de archivos (proyecto)** . Si selecciona **SSISDB** como la ubicación del paquete, opción que se selecciona automáticamente si se ha aprovisionado la instancia de Azure-SSIS IR con el catálogo de SSIS (SSISDB) hospedada por el servidor de Azure SQL Database o Instancia administrada, deberá especificar que el paquete que se va a ejecutar se ha implementado en SSISDB. Si se ejecuta la instancia de Azure-SSIS IR y la casilla **Manual entries** (Entradas manuales) está desactivada, puede examinar y seleccionar sus carpetas, proyectos, paquetes y entornos existentes de SSISDB. Haga clic en el botón **Refresh** (Actualizar) para capturar las carpetas, proyectos, paquetes y entornos recién agregados de SSISDB, de forma que estén disponibles para su examen y selección. Para examinar y seleccionar los entornos para la ejecución de paquetes, debe configurar los proyectos antes de agregar esos entornos como referencias a partir de las mismas carpetas de SSISDB. Para más información, consulte [Creación/asignación de entornos de SSIS](https://docs.microsoft.com/sql/integration-services/create-and-map-a-server-environment?view=sql-server-2014).

   En **Logging level** (Nivel de registro), seleccione un ámbito predefinido de registro para la ejecución de su paquete. Active la casilla **Customized** (Personalizado), si quiere escribir en su lugar un nombre de registro personalizado. 

   ![Establecer propiedades de la pestaña Settings (Configuración): Automatic (Automática)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Si la instancia de Azure-SSIS IR no se está ejecutando o la casilla **Manual entries** (Entradas manuales) está activada, puede escribir las rutas de acceso del paquete y el entorno de SSISDB directamente en los siguientes formatos: `<folder name>/<project name>/<package name>.dtsx` y `<folder name>/<environment name>`.

   ![Establecer las propiedades en la pestaña Settings (Configuración): manual](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   Si selecciona **Sistema de archivos (paquete)** como la ubicación del paquete, opción que se selecciona automáticamente si se ha aprovisionado la instancia de Azure-SSIS IR sin SSISDB, deberá especificar que el paquete se ejecute mediante el suministro de una ruta de acceso de convención de nomenclatura universal (UNC) para el archivo del paquete (`.dtsx`) en la **ruta de acceso al paquete**. Por ejemplo, si almacena el paquete en Azure Files, su ruta de acceso al paquete será `\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`. 
   
   Si configura el paquete en un archivo independiente, también deberá proporcionar una ruta de acceso UNC para el archivo de configuración (`.dtsConfig`) en la **ruta de acceso de configuración**. Por ejemplo, si almacena la configuración de Azure Files, su ruta de acceso de configuración será `\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`.

   ![Establecer las propiedades en la pestaña Settings (Configuración): manual](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   Si selecciona **Sistema de archivos (proyecto)** como la ubicación del paquete, deberá especificar que el paquete se ejecute mediante el suministro de una ruta de acceso UNC para el archivo del proyecto (`.ispac`) en la **ruta de acceso del proyecto** y un archivo de paquete (`.dtsx`) del proyecto en el **nombre del paquete**. Por ejemplo, si almacena el proyecto en Azure Files, su ruta de acceso del proyecto será `\\<storage account name>.file.core.windows.net\<file share name>\<project name>.ispac`.

   ![Establecer las propiedades en la pestaña Settings (Configuración): manual](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   A continuación, deberá especificar las credenciales para tener acceso a los archivos de configuración, del proyecto o del paquete. Si previamente ha introducido los valores para las credenciales de ejecución del paquete (véase más arriba), puede volver a usarlas mediante la selección de la casilla **Same as package execution credentials** (Igual que las credenciales de ejecución del paquete). En caso contrario, deberá especificar los valores de las credenciales de acceso al paquete (**Dominio**/**Nombre de usuario**/**Contraseña**). Por ejemplo, si almacena su proyecto, paquete o configuración en Azure Files, el **Dominio** es `Azure`; el **Nombre de usuario** es `<storage account name>`; y la **Contraseña** es `<storage account key>`. También puede usar secretos almacenados en AKV como sus valores (véase más arriba). Estas credenciales se usarán para tener acceso al paquete y a los paquetes secundarios en la tarea Ejecutar paquete, todo ello desde su propia ruta de acceso o desde el mismo proyecto, así como a las configuraciones, incluidas las que se especifiquen en los paquetes. 
   
   Si ha usado el nivel de protección **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** al crear el paquete a través de SQL Server Data Tools (SSDT), deberá introducir el valor de la contraseña en **Contraseña de cifrado**. También puede usar un secreto almacenado en AKV como su valor (véase más arriba). Si ha usado el nivel de protección **EncryptSensitiveWithUserKey**, deberá volver a introducir los valores confidenciales en los archivos de configuración en las pestañas **Parámetros de SSIS**/**Administradores de conexión**/**Reemplazos de propiedad** (véase a continuación). Si ha usado el nivel de protección **EncryptAllWithUserKey**, no se admite, por lo que deberá volver a configurar el paquete para usar otro nivel de protección a través de SSDT o la utilidad de la línea de comandos `dtutil`. 
   
   En **Logging level** (Nivel de registro), seleccione un ámbito predefinido de registro para la ejecución de su paquete. Active la casilla **Customized** (Personalizado), si quiere escribir en su lugar un nombre de registro personalizado. Si quiere registrar las ejecuciones del paquete mediante el uso de los proveedores de registro estándar que se pueden especificar en el paquete, deberá especificar la carpeta de registro mediante el suministro de su ruta de acceso UNC en la **ruta de acceso a registros**. Por ejemplo, si almacena los registros en Azure Files, su ruta de acceso al registro será `\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`. Se creará una subcarpeta en esta ruta de acceso para cada ejecución de paquete individual y se le asignará un nombre tras el id. de ejecución de actividad Ejecutar paquete de SSIS, en el que se generarán archivos de registro cada cinco minutos. 
   
   Por último, también deberá especificar las credenciales para tener acceso a la carpeta de registro. Si previamente ha introducido los valores para las credenciales de acceso al paquete (véase más arriba), puede volver a usarlas mediante la selección de la casilla **Same as package access credentials** (Igual que las credenciales de acceso al paquete). En caso contrario, deberá especificar los valores de las credenciales de acceso al registro (**Dominio**/**Nombre de usuario**/**Contraseña**). Por ejemplo, si almacena los registros en Azure Files, el **Dominio** es `Azure`; el **Nombre de usuario** es `<storage account name>`; y la **Contraseña** es `<storage account key>`. También puede usar secretos almacenados en AKV como sus valores (véase más arriba). Estas credenciales se usarán para almacenar los registros. 
   
   Para todas las rutas de acceso UNC mencionadas anteriormente, el nombre de archivo completo debe ser inferior a 260 caracteres y el nombre del directorio debe tener menos de 248 caracteres.

5. En la pestaña **Parámetros de SSIS** de la actividad Ejecutar paquete de SSIS, si la instancia de Azure-SSIS IR está en ejecución, se ha seleccionado **SSISDB** como la ubicación del paquete y la casilla **Manual entries** (Entradas manuales) de la pestaña **Configuración** está desactivada, se muestran los parámetros de SSIS existentes en el proyecto o paquete seleccionados de SSISDB para que les asigne valores. En caso contrario, puede escribirlos uno a uno para asignar valores manualmente. Asegúrese de que existen y se han escrito correctamente para que la ejecución del paquete se realice de forma adecuada. 
   
   Si ha usado el nivel de protección **EncryptSensitiveWithUserKey** al crear el paquete a través de SSDT y se ha seleccionado **Sistema de archivos (paquete)** /**Sistema de archivos (proyecto)** como la ubicación del paquete, también deberá volver a introducir los parámetros confidenciales para asignarles valores en los archivos de configuración o en esta pestaña. 
   
   Al asignar valores a los parámetros, puede agregar contenido dinámico mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF. También puede usar secretos almacenados en AKV como sus valores (véase más arriba).

   ![Establecer propiedades en la pestaña SSIS Parameters (Parámetros de SSIS)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. En la pestaña **Administradores de conexión** de la actividad Ejecutar paquete de SSIS, si la instancia de Azure-SSIS IR está en ejecución, se ha seleccionado **SSISDB** como la ubicación del paquete y la casilla **Manual entries** (Entradas manuales) de la pestaña **Configuración** está desactivada, se muestran los administradores de conexión existentes en el proyecto o paquete seleccionado de SSISDB para que se asignen valores a sus propiedades. De lo contrario, puede escribirlos uno a uno para asignar valores a sus propiedades manualmente. Asegúrese de que existen y se han escrito correctamente para que la ejecución del paquete sea correcta. 
   
   Si ha usado el nivel de protección **EncryptSensitiveWithUserKey** al crear el paquete a través de SSDT y se ha seleccionado **Sistema de archivos (paquete)** /**Sistema de archivos (proyecto)** como la ubicación del paquete, también deberá volver a introducir las propiedades confidenciales del administrador de conexiones para asignarles valores en los archivos de configuración o en esta pestaña. 
   
   Al asignar valores a las propiedades del administrador de conexiones, puede agregar contenido dinámico mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF. También puede usar secretos almacenados en AKV como sus valores (véase más arriba).

   ![Establecer propiedades en la pestaña Connection Managers (Administradores de conexiones)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. En la pestaña **Reemplazos de propiedad** de la actividad Ejecutar paquete de SSIS, puede escribir las rutas de acceso de las propiedades existentes en el paquete seleccionado una a una para asignarles valores manualmente. Asegúrese de que existen y de que se escriben correctamente para que la ejecución del paquete se realice de forma adecuada; por ejemplo, para invalidar el valor de la variable de usuario, escriba su ruta de acceso en el siguiente formato: `\Package.Variables[User::<variable name>].Value`. 
   
   Si ha usado el nivel de protección **EncryptSensitiveWithUserKey** al crear el paquete a través de SSDT y se ha seleccionado **Sistema de archivos (paquete)** /**Sistema de archivos (proyecto)** como la ubicación del paquete, también deberá volver a introducir las propiedades confidenciales para asignarles valores en los archivos de configuración o en esta pestaña. 
   
   Al asignar valores a las propiedades, puede agregar contenido dinámico mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF.

   ![Establecer propiedades en la pestaña Property Overrides (Reemplazos de propiedad)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   Los valores asignados en los archivos de configuración y en la pestaña *Parámetros de SSIS* puede invalidarse mediante las pestañas **Administradores de conexión**/**Reemplazos de propiedad**, mientras que los asignados en la pestaña **Administradores de conexión** también se pueden invalidar mediante la pestaña **Reemplazos de propiedad**.

8. Para validar la configuración de la canalización, haga clic en **Validate** (Validar) en la barra de herramientas. Para cerrar **Pipeline Validation Report** (Informe de comprobación de la canalización), haga clic en **>>** .

9. Para publicar la canalización en ADF, haga clic en **Publish All** (Publicar todo). 

### <a name="run-the-pipeline"></a>Ejecución de la canalización
En este paso, desencadenará una ejecución de canalización. 

1. Para desencadenar una ejecución de canalización, haga clic en **Trigger** (Desencadenar) en la barra de herramientas y en **Trigger now** (Desencadenar ahora). 

   ![Trigger now (Desencadenar ahora)](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. En la ventana **Pipeline Run** (Ejecución de canalización), seleccione **Finish** (Finalizar). 

### <a name="monitor-the-pipeline"></a>Supervisar la canalización

1. Cambie a la pestaña **Monitor** (Supervisar) de la izquierda. Verá la ejecución de canalización y su estado junto con otro tipo de información (como la hora de inicio de la ejecución). Para actualizar la vista, haga clic en **Refresh** (Actualizar).

   ![Ejecuciones de la canalización](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Haga clic en el vínculo **View Activity Runs** (Ver ejecuciones de actividad) de la columna **Actions** (Acciones). Solo verá una ejecución de actividad porque la canalización solo tiene una actividad (actividad Ejecutar paquete de SSIS).

   ![Ejecuciones de actividad](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Puede ejecutar la **consulta** siguiente en la base de datos SSISDB en el servidor de Azure SQL para comprobar la ejecución del paquete. 

   ```sql
   select * from catalog.executions
   ```

   ![Comprobación de las ejecuciones del paquete](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. También puede obtener el identificador de ejecución de SSISDB desde la salida de la ejecución de la actividad de canalización, y usar el identificador para comprobar registros de ejecución y mensajes de error más completos en SQL Server Management Studio (SSMS).

   ![Obtenga el identificador de ejecución.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programación de la canalización con un desencadenador

También puede crear un desencadenador programado para la canalización de manera que esta se ejecute según una programación (por hora, cada día, etc.). Para ver un ejemplo, consulte [Create a data factory - Data Factory UI](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule) (Creación de una factoría de datos: interfaz de usuario de Data Factory).

## <a name="run-a-package-with-powershell"></a>Ejecución de un paquete con PowerShell
En esta sección, usará Azure PowerShell para crear una canalización de ADF con una actividad Ejecutar paquete de SSIS que ejecuta el paquete de SSIS. 

Instale los módulos de Azure PowerShell más recientes siguiendo las instrucciones paso a paso que se indican en [Cómo instalar y configurar Azure PowerShell](/powershell/azure/install-az-ps).

### <a name="create-an-adf-with-azure-ssis-ir"></a>Creación de un ADF con Azure-SSIS IR
Puede usar un ADF existente que ya tenga aprovisionado Azure-SSIS IR o crear un ADF con Azure-SSIS IR siguiendo las instrucciones paso a paso del [Tutorial: Implementación de paquetes SSIS en Azure mediante PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Creación de una canalización con una actividad Ejecutar paquete de SSIS 
En este paso se crea una canalización con una actividad Ejecutar paquete de SSIS. La actividad ejecuta el paquete de SSIS. 

1. Cree un archivo JSON con el nombre **RunSSISPackagePipeline.json** en la carpeta **C:\ADF\RunSSISPackage** con un contenido similar al del siguiente ejemplo:

   > [!IMPORTANT]
   > Reemplace los nombres de objeto, las descripciones, las rutas de acceso, los valores de propiedad y parámetro, las contraseñas y otros valores de variable antes de guardar el archivo. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "MySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "MyAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyExecutionDomain",
                       "username": "MyExecutionUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "MyExecutionPassword"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "MyFolder/MyProject/MyPackage.dtsx",
                       "type": "SSISDB"
                   },
                   "environmentPath": "MyFolder/MyEnvironment",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyProjectParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MyPackageParameter"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "username": {
                               "value": "MyConnectionUsername"
                           },
                           "password": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "MyConnectionPassword"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "username": {
                               "value": {
                                   "value": "@pipeline().parameters.MyConnectionUsername",
                                   "type": "Expression"
                               }
                           },
                           "password": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyConnectionPassword",
                                   "secretVersion": "MyConnectionPasswordVersion"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

   Para ejecutar paquetes almacenados en sistemas de archivos, recursos compartidos de archivos o Azure Files, puede especificar los valores de las propiedades de la ubicación del paquete o del registro como se indica a continuación.

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   },
                   "logLocation": {
                       "logPath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyLogFolder",
                       "type": "File",
                       "typeProperties": {
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

   Para ejecutar paquetes de proyectos almacenados en sistemas de archivos, recursos compartidos de archivos o Azure Files, puede especificar los valores de la propiedad de la ubicación del paquete como se indica a continuación.

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyProject.ispac:MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "userName": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

2. En Azure PowerShell, cambie a la carpeta `C:\ADF\RunSSISPackage`.

3. Para crear la canalización **RunSSISPackagePipeline**, ejecute el cmdlet **Set-AzDataFactoryV2Pipeline**.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Este es la salida de ejemplo:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>Ejecución de la canalización
Use el cmdlet **Invoke-AzDataFactoryV2Pipeline** para ejecutar la canalización. El cmdlet devuelve el identificador de ejecución de la canalización para realizar una supervisión en un futuro.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>Supervisar la canalización

Ejecute el script de PowerShell siguiente para comprobar continuamente el estado de ejecución de la canalización hasta que termine de copiar los datos. Copie y pegue el siguiente script en la ventana de PowerShell y presione ENTRAR. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

También puede supervisar la canalización mediante Azure Portal. Para ver instrucciones paso a paso, consulte [Supervisar la canalización](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programación de la canalización con un desencadenador
En el paso anterior, ejecutó la canalización a petición. También puede crear un desencadenador de programación para ejecutar la canalización en una programación (cada hora, día, etc.).

1. Cree un archivo JSON con el nombre **MyTrigger.json** en la carpeta **C:\ADF\RunSSISPackage** con el siguiente contenido: 

   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
2. En **Azure PowerShell**, cambie a la carpeta **C:\ADF\RunSSISPackage**.
3. Ejecute el cmdlet **Set-AzDataFactoryV2Trigger**, que crea el desencadenador. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. De manera predeterminada, el desencadenador está en estado detenido. Inicie el desencadenador al ejecutar el cmdlet **Start-AzDataFactoryV2Trigger**. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Confirme que el desencadenador se ha iniciado al ejecutar el cmdlet **Get-AzDataFactoryV2Trigger**. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Ejecute el comando siguiente al comenzar la hora siguiente. Por ejemplo, si la hora actual es 15:25 UTC, ejecute el comando a las 16:00 UTC. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Puede ejecutar la consulta siguiente en la base de datos SSISDB en el servidor de Azure SQL para comprobar la ejecución del paquete. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Pasos siguientes
Vea la siguiente entrada de blog:
-   [Modernize and extend your ETL/ELT workflows with SSIS activities in ADF pipelines](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Modernize-and-Extend-Your-ETL-ELT-Workflows-with-SSIS-Activities/ba-p/388370) (Modernización y ampliación de los flujos de trabajo ETL/ETL con actividades de SSIS en las canalizaciones de ADF)
