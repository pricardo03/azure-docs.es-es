---
title: Ejecución de paquetes de SQL Server Integration Services (SSIS) con la utilidad dtexec habilitada para Azure | Microsoft Docs
description: Obtenga información sobre cómo ejecutar paquetes de SQL Server Integration Services (SSIS) con la utilidad dtexec habilitada para Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/21/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 80d4bd2f4f9ea4c451f312f05fd42fbc1ba433ab
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71181227"
---
# <a name="run-sql-server-integration-services-ssis-packages-with-azure-enabled-dtexec-utility"></a>Ejecución de paquetes de SQL Server Integration Services (SSIS) con la utilidad dtexec habilitada para Azure
En este artículo se describe la utilidad de símbolo del sistema **dtexec** (**AzureDTExec**) habilitada para Azure.  Se utiliza para ejecutar paquetes SSIS en Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF).

La utilidad tradicional **dtexec** se incluye con SQL Server; consulte la documentación de la [utilidad dtexec](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017) para más información.  A menudo, se llaman a los orquestadores o programadores de terceros, como Active Batch o Control-M, entre otros, para ejecutar paquetes SSIS de forma local.  La utilidad **AzureDTExec** moderna se incluye en la herramienta SQL Server Management Studio (SSMS).  También puede invocarla los orquestadores o programadores de terceros para ejecutar paquetes SSIS en Azure.  Facilita la migración mediante lift-and-shift de los paquetes de SSIS a la nube.  Después de la migración, si desea seguir usando orquestadores o programadores de terceros en las operaciones cotidianas, ahora pueden invocar **AzureDTExec** en lugar de **dtexec**.

**AzureDTExec** ejecutará los paquetes como actividades de ejecución de paquetes SSIS en las canalizaciones de ADF; consulte el artículo [Ejecución de paquetes SSIS como actividades de ADF](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) para más información.  Se puede configurar mediante SSMS para usar una aplicación de Azure Active Directory (AAD) que genera canalizaciones en ADF.  También puede configurarse para acceder a los sistemas de archivos, recursos compartidos de archivos o Azure Files donde se almacenan los paquetes.  En función de los valores que proporcione para sus opciones de invocación, **AzureDTExec** generará y ejecutará una canalización de ADF única con la actividad Ejecutar paquete SSIS.  Al invocar **AzureDTExec** con los mismos valores para sus opciones, se volverá a ejecutar la canalización existente.  Al invocar **AzureDTExec** con nuevos valores para sus opciones, se generará una nueva canalización.

## <a name="prerequisites"></a>Requisitos previos
Para usar **AzureDTExec**, descargue e instale la versión más reciente de SSMS (versión 18.3 o posterior) [aquí](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

## <a name="configure-azuredtexec-utility"></a>Configuración de la utilidad AzureDTExec
La instalación de SSMS en la máquina local también instalará **AzureDTExec**.  Para configurar sus opciones, inicie SSMS con la opción **Ejecutar como administrador** y seleccione el elemento de menú desplegable en cascada **Herramientas-> Migrar a Azure-> Configure Azure-enabled DTExec** (Configurar DTExec habilitado para Azure).

![Configuración del menú de dtexec habilitado para Azure](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-menu.png)

Esta acción abrirá la ventana **AzureDTExecConfig** que debe abrirse con privilegios administrativos para que pueda escribir en el archivo **AzureDTExec.settings**.  Si no ha ejecutado SSMS como administrador, aparecerá una ventana de control de cuentas de usuario (UAC) para que escriba la contraseña de administrador con el fin de elevar los privilegios.

![Configuración de las opciones de dtexec habilitadas para Azure](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-settings.png)

En **AzureDTExecConfig**, puede especificar las opciones de configuración como se indica a continuación:

- **ApplicationId**: Escriba el identificador único de la aplicación de AAD que cree con los permisos adecuados para generar canalizaciones en ADF. Consulte [Creación de una aplicación de AAD y una entidad de servicio mediante Azure Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) para más información.

- **AuthenticationKey**: Escriba la clave de autenticación de la aplicación de AAD.

- **TenantId**: Escriba el identificador único del inquilino de AAD, en el que se crea la aplicación de AAD.

- **SubscriptionId**: Escriba el identificador único de la suscripción de Azure en la que se creó la instancia de ADF.

- **ResourceGroup**: Escriba el nombre del grupo de recursos de Azure en el que se creó la instancia de ADF.

- **DataFactory**:  Escriba el nombre de la instancia de ADF, en la que las canalizaciones únicas con la actividad Ejecutar paquete SSIS en ellas se generan en función de los valores de las opciones proporcionadas al invocar **AzureDTExec**.

- **IRName**: Escriba el nombre de Azure-SSIS IR en la instancia de ADF, en la que se ejecutarán los paquetes especificados en la ruta de acceso de convención de nomenclatura universal (UNC) al invocar **AzureDTExec**.

- **PackageAccessDomain**: Escriba la credencial de dominio para acceder a los paquetes en su ruta de acceso UNC especificada al invocar **AzureDTExec**.

- **PackageAccessUserName**:  Escriba la credencial de nombre de usuario para acceder a los paquetes en su ruta de acceso UNC especificada al invocar **AzureDTExec**.

- **PackageAccessPassword**: Escriba la credencial de contraseña para acceder a los paquetes en su ruta de acceso UNC especificada al invocar **AzureDTExec**.

- **LogPath**:  Escriba la ruta de acceso UNC de la carpeta de registro, en la que se escribirán los archivos de registro de las ejecuciones del paquete en Azure-SSIS IR.

- **LogLevel**:  Escriba el ámbito seleccionado de registro de las opciones predefinidas **null**/**Basic**/**Verbose**/**Performance** para las ejecuciones de paquetes en Azure-SSIS IR.

- **LogAccessDomain**: Escriba la credencial de dominio para acceder a la carpeta de registro en la ruta de acceso UNC al escribir los archivos de registro, necesaria cuando se especifica **LogPath **y** LogLevel** no es un valor **null**.

- **LogAccessUserName**: Escriba la credencial de nombre de usuario para acceder a la carpeta de registro en la ruta de acceso UNC al escribir los archivos de registro, necesaria cuando se especifica **LogPath **y** LogLevel** no es un valor **null**.

- **LogAccessPassword**: Escriba la credencial de contraseña para acceder a la carpeta de registro en la ruta de acceso UNC al escribir los archivos de registro, necesaria cuando se especifica **LogPath **y** LogLevel** no es un valor **null**.

- **PipelineNameHashStrLen**: Escriba la longitud de las cadenas hash que se generarán a partir de los valores de las opciones que se proporcionan al invocar **AzureDTExec**.  Las cadenas se usarán para formar nombres únicos para las canalizaciones de ADF que ejecutan los paquetes en Azure-SSIS IR.  Una longitud de 32 caracteres es suficiente.

Si planea almacenar los paquetes y archivos de registro en los sistemas de archivos o recursos compartidos de archivos de forma local, debe unir la instancia de Azure-SSIS IR a una red virtual conectada a la red local, para que pueda capturar los paquetes y escribir los archivos de registro; consulte el artículo [Unión de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) para más información.

Para evitar que se muestren los valores confidenciales escritos en el archivo **AzureDTExec.settings** en texto sin formato, se codificarán en cadenas de codificación Base64.  Al invocar **AzureDTExec**, todas las cadenas codificadas en Base64 se descodificarán de nuevo en sus valores originales.  Para proteger aún más el archivo **AzureDTExec.settings**, limite las cuentas que pueden acceder a él.

## <a name="invoke-azuredtexec-utility"></a>Invocación de la utilidad AzureDTExec
Puede invocar **AzureDTExec** en el símbolo de la línea de comandos y proporcionar los valores pertinentes para las opciones específicas en el escenario de caso de uso, por ejemplo:

`AzureDTExec.exe
  /F \\MyStorageAccount.file.core.windows.net\MyFileShare\MyPackage.dtsx
  /Conf \\MyStorageAccount.file.core.windows.net\MyFileShare\MyConfig.dtsConfig
  /Conn "MyConnectionManager;Data Source=MyDatabaseServer.database.windows.net;User ID=MyAdminUsername;Password=MyAdminPassword;Initial Catalog=MyDatabase"
  /Set \package.variables[MyVariable].Value;MyValue
  /De MyEncryptionPassword`

Al invocar **AzureDTExec**, se ofrecen opciones similares al invocar **dtexec**, consulte la documentación de la [utilidad dtexec](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017) para más información.  Estas son las opciones que se admiten actualmente:

- **/F[ile]** : Carga un paquete que está almacenado en el sistema de archivos, recurso compartido de archivos o Azure Files.  Como valor de esta opción, puede especificar la ruta de acceso UNC para el archivo de paquete en el sistema de archivos, recurso compartido de archivos o Azure Files con la extensión dtsx.  Si la ruta de acceso UNC especificada contiene algún espacio, debe colocar las comillas alrededor de la ruta de acceso completa.

- **/Conf[igFile]** : Especifica un archivo de configuración del que se van a extraer los valores.  Con esta opción, puede establecer una configuración en tiempo de ejecución para el paquete que difiere de la especificada en tiempo de diseño.  Puede almacenar valores diferentes en un archivo de configuración XML y, después, cargarlos antes de la ejecución del paquete.  Consulte el artículo [Configuraciones de paquetes SSIS](https://docs.microsoft.com/sql/integration-services/packages/package-configurations?view=sql-server-2017) para obtener más información.  Como valor de esta opción, puede especificar la ruta de acceso UNC para el archivo de configuración en el sistema de archivos, recurso compartido de archivos o Azure Files con la extensión dtsConfig.  Si la ruta de acceso UNC especificada contiene algún espacio, debe colocar las comillas alrededor de la ruta de acceso completa.

- **/Conn[ection]** : Especifica las cadenas de conexión para los administradores de conexiones existentes en el paquete.  Con esta opción, puede establecer las cadenas de conexión en tiempo de ejecución para los administradores de conexiones existentes en el paquete que difieren de las especificadas en tiempo de diseño.  Como valor de esta opción, se puede especificar de la siguiente manera: `connection_manager_name_or_id;connection_string [[;connection_manager_name_or_id;connection_string]...]`.

- **/Set**: Invalida la configuración de un parámetro, variable, propiedad, contenedor, proveedor de registro, enumerador Foreach o conexión en el paquete.  Esta opción se puede especificar varias veces.  Como valor de esta opción, puede especificarlo de la siguiente manera: `property_path;value`, por ejemplo `\package.variables[counter].Value;1` invalida el valor de la variable `counter` como 1.  Puede usar el Asistente para la configuración de paquetes para buscar, copiar y pegar el valor de `property_path` para los elementos del paquete cuyo valor desea invalidar; consulte la documentación del [Asistente para la configuración de paquetes](https://docs.microsoft.com/sql/integration-services/package-configuration-wizard-ui-reference?view=sql-server-2014) para más información.

- **/De[crypt]** : Establece la contraseña de descifrado para el paquete que está configurado con el nivel de protección **EncryptAllWithPassword**/**EncryptSensitiveWithPassword**.

## <a name="next-steps"></a>Pasos siguientes
Cuando las canalizaciones únicas con la actividad Ejecutar paquete SSIS en ellas se generan y ejecutan después de invocar **AzureDTExec**, se pueden editar y volver a ejecutar en el portal de ADF; consulte el artículo [Ejecución de paquetes SSIS como actividades de ADF](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) para más información.
