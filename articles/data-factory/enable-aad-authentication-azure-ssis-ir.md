---
title: Habilitación de la autenticación de Azure Active Directory para Azure-SSIS Integration Runtime | Microsoft Docs
description: En este artículo se describe cómo habilitar la autenticación de Azure Active Directory con la identidad administrada para Azure Data Factory para crear una instancia de Azure-SSIS Integration Runtime.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 1/9/2019
ms.author: douglasl
ms.openlocfilehash: 5cc625e07f1c92c53491e83f4049bad12cd9d1a1
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158268"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Habilitar la autenticación de Azure Active Directory para Azure-SSIS Integration Runtime

En este artículo se muestra cómo habilitar la autenticación de Azure Active Directory (Azure AD) con la identidad administrada para Azure Data Factory (ADF) y usarla en lugar de la autenticación de SQL para crear una instancia de Azure-SSIS Integration Runtime (IR) que a su vez creará una base de datos del catálogo de SSIS (SSISDB) en Azure SQL Server o instancia administrada en su nombre.

Para más información sobre la identidad administrada de ADF, consulte [Identidad del servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Si ya ha creado una instancia de Azure-SSIS IR con la autenticación de SQL, no puede volver a configurar IR para utilizar la autenticación de Azure AD con PowerShell en este momento, pero puede hacerlo en Azure Portal o la aplicación ADF. 

## <a name="enable-azure-ad-on-azure-sql-database"></a>Habilitación de Azure AD en Azure SQL Database

El servidor de Azure SQL Database admite la creación de una base de datos con un usuario de Azure AD. En primer lugar, debe crear un grupo de Azure AD con la identidad administrada para ADF como miembro. A continuación, debe establecer un usuario de Azure AD como administrador de Active Directory para el servidor de Azure SQL Database y luego conectarse desde SQL Server Management Studio (SSMS) con ese usuario. Por último, deberá crear un usuario contenido que represente al grupo de Azure AD, de modo que Azure-SSIS IR pueda usar la identidad administrada para ADF para crear SSISDB en su nombre.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>Crear un grupo de Azure AD con la identidad administrada para ADF como miembro

Puede utilizar un grupo de Azure AD existente o crear uno nuevo con Azure AD PowerShell.

1.  Instalación del módulo de [PowerShell de Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

2.  Inicie sesión con  `Connect-AzureAD`, ejecute el siguiente cmdlet para crear el grupo y guárdelo en una variable:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    El resultado es similar al del siguiente ejemplo, que también muestra el valor de la variable:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Agregue la identidad administrada de la instancia de ADF al grupo. Puede seguir el artículo [Identidad de servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obtener la ID. DE LA IDENTIDAD DE SERVICIO principal (por ejemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, pero no utilizar la ID. DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO para este propósito).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    También puede comprobar la pertenencia al grupo más adelante.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Configurar la autenticación de Azure AD para el servidor de Azure SQL Database

Puede  [configurar y administrar la autenticación de Azure AD con SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) siguiendo estos pasos:

1.  En Azure Portal, seleccione  **Todos los servicios** -> **Servidores SQL**  en el panel de navegación izquierdo.

2.  Seleccione el servidor de Azure SQL Database que quiera configurar con la autenticación de Azure AD.

3.  En la sección  **Configuración**  de la hoja, seleccione  **Administrador de Active Directory**.

4.  En la barra de comandos, seleccione  **Establecer administrador**.

5.  Seleccione una cuenta de usuario de Azure AD para que sea el administrador del servidor y, después, seleccione  **Seleccionar**.

6.  En la barra de comandos, seleccione **Guardar**.

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Cree un usuario contenido en el servidor de Azure SQL Database que represente al grupo de Azure AD

En el paso siguiente, necesita  [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)  (SSMS).

1.  Inicie SSMS.

2.  En el cuadro de diálogo  **Conectar al servidor** , escriba el nombre del servidor de Azure SQL Database en el campo  **Nombre del servidor** .

3.  En el campo  **Autenticación** , seleccione  **Active Directory - Universal with MFA support** (Active Directory: universal compatible con MFA) (también puede usar los otros dos tipos de autenticación de Active Directory, consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4.  En el campo  **Nombre de usuario** , escriba el nombre de la cuenta de Azure AD que estableció como administrador del servidor, por ejemplo, testuser@xxxonline.com.

5.  Seleccione  **Conectar** y complete el proceso de inicio de sesión.

6.  En el  **Explorador de objetos**, expanda la carpeta  **Bases de datos** -> **Bases de datos del sistema**.

7.  Haga clic con el botón derecho en la base de datos **maestra** y seleccione  **Nueva consulta**.

8.  En la ventana de consulta, escriba el siguiente comando T-SQL y seleccione  **Ejecutar**  en la barra de herramientas.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    El comando debería completarse correctamente y crear el usuario contenido para representar al grupo.

9.  Borre la ventana de consulta, escriba el siguiente comando T-SQL y seleccione **Ejecutar** en la barra de herramientas.

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    El comando debería completarse correctamente y conceder al usuario contenido la capacidad de crear una base de datos (SSISDB).

10.  Si la SSISDB se creó mediante la autenticación de SQL y desea cambiarla para que use la autenticación de Azure AD en la instancia de Azure-SSIS IR para acceder a ella, haga clic con el botón derecho en la base de datos **SSISDB** y seleccione **Nueva consulta**.

11.  En la ventana de consulta, escriba el siguiente comando T-SQL y seleccione  **Ejecutar**  en la barra de herramientas.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    El comando debería completarse correctamente y crear el usuario contenido para representar al grupo.

12.  Borre la ventana de consulta, escriba el siguiente comando T-SQL y seleccione **Ejecutar** en la barra de herramientas.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    El comando debería completarse correctamente y conceder al usuario contenido la capacidad de acceder a SSISDB.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Habilitación de Azure AD en Instancia administrada de Azure SQL Database

La instancia administrada de Azure SQL Database admite la creación de una base de datos con la instancia administrada para ADF directamente. No necesita unir la identidad administrada para ADF a un grupo de Azure AD ni crear un usuario contenido que represente dicho grupo en la instancia administrada.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Configurar la autenticación de Azure AD para la instancia administrada de Azure SQL Database

1.   En Azure Portal, seleccione **Todos los servicios** -> **Servidores SQL** en el panel de navegación izquierdo.

2.   Seleccione la instancia administrada que quiera configurar con la autenticación de Azure AD.

3.   En la sección **Configuración** de la hoja, haga clic en **Administrador de Active Directory**.

4.   En la barra de comandos, seleccione **Establecer administrador**.

5.   Seleccione una cuenta de usuario de Azure AD para que sea el administrador del servidor y, después, seleccione **Seleccionar**.

6.   En la barra de comandos, seleccione **Guardar**.

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>Agregar la identidad administrada para ADF como usuario en la instancia administrada de Azure SQL Database

En el paso siguiente, necesita  [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)  (SSMS).

1.  Inicie SSMS.

2.  Conéctese a la instancia administrada con su cuenta de administrador de SQL o Active Directory.

3.  En el **Explorador de objetos**, expanda la carpeta **Bases de datos** -> **Bases de datos del sistema**.

4.  Haga clic con el botón derecho en la base de datos **maestra** y seleccione **Nueva consulta**.

5.  Obtenga la identidad administrada de la instancia de ADF. Puede seguir el artículo [Identidad de servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obtener la ID. DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO principal (pero no utilizar la ID. DE LA IDENTIDAD DE SERVICIO para este propósito).

6.  En la ventana de consulta, ejecute el siguiente script de T-SQL para convertir la identidad administrada para ADF a tipo binario:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your SERVICE IDENTITY APPLICATION ID}
    select CAST(@applicationId AS varbinary)
    ```
    
    El comando debería completarse correctamente y mostrar la identidad administrada para ADF como binaria.

7.  Borre la ventana de consulta y ejecute el siguiente script de T-SQL para agregar la identidad administrada para ADF como usuario.

    ```sql
    CREATE LOGIN [{a name for the managed identity}] FROM EXTERNAL PROVIDER with SID ={your SERVICE IDENTITY APPLICATION ID as binary}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{the managed identity name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{the managed identity name}]
    ```
    
    El comando debería completarse correctamente y conceder a la identidad administrada para ADF la capacidad de crear una base de datos (SSISDB).

8.  Si la SSISDB se creó mediante la autenticación de SQL y desea cambiarla para que use la autenticación de Azure AD en la instancia de Azure-SSIS IR para acceder a ella, haga clic con el botón derecho en la base de datos **SSISDB** y seleccione **Nueva consulta**.

9.  En la ventana de consulta, escriba el siguiente comando T-SQL y seleccione  **Ejecutar**  en la barra de herramientas.

    ```sql
    CREATE USER [{the managed identity name}] FOR LOGIN [{the managed identity name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{the managed identity name}]
    ```

    El comando debería completarse correctamente y conceder a la identidad administrada para ADF la capacidad de acceder a SSISDB.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Aprovisionar Azure-SSIS IR en Azure Portal o ADF

Al aprovisionar Azure-SSIS IR en Azure Portal o la aplicación AFD, en la página **Configuración de SQL**, seleccione la opción **Use AAD authentication with the managed identity for your ADF** (Usar autenticación de AAD con la identidad administrada de ADF). En la captura de pantalla siguiente se muestra la configuración para IR con el servidor de Azure SQL Database que hospeda SSISDB. Para IR con la instancia administrada que hospeda SSISDB, los valores **Catalog Database Service Tier** (Nivel de servicio de base de datos del catálogo) y **Allow Azure services to access** no son aplicables, mientras que otros valores son los mismos.

Para más información sobre cómo crear una instancia de Azure-SSIS IR, consulte [Creación de una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Configuración de la instancia de Integration Runtime para la integración de SSIS en Azure](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>Aprovisionamiento de Azure-SSIS IR con PowerShell

Para aprovisionar el entorno de ejecución para la integración de SSIS en Azure con PowerShell, haga lo siguiente:

1.  Instale el módulo [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) .

2.  En el script, no establezca el parámetro `CatalogAdminCredential`. Por ejemplo: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
