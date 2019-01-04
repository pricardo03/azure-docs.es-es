---
title: Habilitación de la autenticación de Azure Active Directory para el entorno de ejecución para la integración de SSIS en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de ejecución para la integración de SSIS en Azure para habilitar conexiones que usan la autenticación de Azure Active Directory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: d2000e626166304e92556e3c965df175a27046ad
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321074"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Habilitación de la autenticación de Azure Active Directory para el entorno de ejecución para la integración de SSIS en Azure

En este artículo se muestra cómo crear el entorno de ejecución para la integración de SSIS en Azure con la identidad de servicio Azure Data Factory. Puede usar la autenticación de Azure Active Directory (Azure AD) con la identidad administrada para Azure Data Factory en lugar de la autenticación SQL para crear un entorno de ejecución de integración de SSIS en Azure.

Para más información sobre la identidad administrada de la factoría de datos, consulte [Identidad del servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Si ya ha creado un entorno de ejecución para la integración de SSIS en Azure con autenticación SQL, no puede volver a configurar el entorno de ejecución de integración para utilizar la autenticación Azure AD con PowerShell en este momento.

## <a name="enable-azure-ad-on-azure-sql-database"></a>Habilitación de Azure AD en Azure SQL Database

Azure SQL Database admite la creación de una base de datos con un usuario de Azure AD. Como resultado, puede configurar un usuario de Azure AD como administrador de Active Directory y, a continuación, iniciar sesión en SQL Server Management Studio con el usuario de Azure AD. Después puede crear un usuario contenido para el grupo de Azure AD para permitir que el entorno de ejecución para la integración cree el catálogo de SQL Server Integration Services (SSIS) en el servidor.

### <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-data-factory-a-member-of-the-group"></a>Cree un grupo en Azure AD y convierta la identidad administrada de la factoría de datos en miembro de este

Puede utilizar un grupo de Azure AD existente o crear uno nuevo con PowerShell de Azure AD.

1.  Instalación del módulo de [PowerShell de Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

2.  Inicie sesión con  `Connect-AzureAD` y ejecute el siguiente comando para crear el grupo y guardarlo en una variable:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    La salida es similar a la del siguiente ejemplo, que también examina el valor de la variable:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Agregue la identidad administrada de la factoría de datos al grupo. Puede seguir la [identidad de servicio de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obtener la ID. DE LA IDENTIDAD DE SERVICIO principal (por ejemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, pero no utilizar la ID. DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO para este propósito).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    También puede examinar la pertenencia al grupo más adelante.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Habilitación de la autenticación de Azure AD para Azure SQL Database

Puede  [configurar la autenticación de Azure AD para la base de datos SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) siguiendo estos pasos:

1.  En Azure Portal, seleccione  **Todos los servicios** -> **Servidores SQL**  en el panel de navegación izquierdo.

2.  Seleccione servidor SQL Database para habilitarlo para la autenticación de Azure AD.

3.  En la sección  **Configuración**  de la hoja, seleccione  **Administrador de Active Directory**.

4.  En la barra de comandos, seleccione  **Establecer administrador**.

5.  Seleccione una cuenta de usuario de Azure AD para que se convierta en administrador del servidor y, después, seleccione  **Seleccionar**.

6.  En la barra de comandos, seleccione **Guardar**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Cree un usuario contenido en la base de datos que representa el grupo de Azure AD

En el paso siguiente, necesita  [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)  (SSMS).

1.  Inicie SQL Server Management Studio.

2.  En el cuadro de diálogo  **Conectar al servidor** , escriba el nombre del servidor SQL en el campo  **Nombre del servidor** .

3.  En el campo  **Autenticación** , seleccione  **Active Directory: universal compatible con MFA**. (También puede usar otros dos tipos de autenticación de Active Directory. Consulte [Configuración y administración de la autenticación de Azure Active Directory con SQL Database, Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4.  En el campo  **Nombre de usuario** , escriba el nombre de la cuenta de Azure AD que estableció como administrador del servidor, por ejemplo, testuser@xxxonline.com.

5.  Seleccione  **Conectar**. Complete el proceso de inicio de sesión.

6.  En el  **Explorador de objetos**, expanda la carpeta  **Bases de datos** -> Bases de datos del sistema.

7.  Seleccione con el botón derecho la base de datos **maestra** y seleccione  **Nueva consulta**.

8.  En la ventana de consulta, escriba la línea siguiente y seleccione  **Ejecutar**  en la barra de herramientas:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    El comando debería completarse correctamente y se creará el usuario contenido para el grupo.

9.  Limpie la ventana de consulta, escriba la línea siguiente y haga clic en **Ejecutar** en la barra de herramientas:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    El comando debería completarse correctamente, concediendo al usuario contenido la capacidad de crear la base de datos.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Habilitación de Azure AD en Instancia administrada de Azure SQL Database

Instancia administrada de Azure SQL Database admite la creación de una base de datos directamente con MSI. No es necesario unir la característica MSI de la factoría de datos a un grupo de AD ni crear el usuario contenido en MI.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database-managed-instance"></a>Habilitación de la autenticación de Azure AD para Instancia administrada de Azure SQL Database

1.   En Azure Portal, seleccione **Todos los servicios** -> **Servidores SQL** en el panel de navegación izquierdo.

1.   Haga clic en el servidor SQL para habilitarlo para la autenticación de Azure AD.

1.   En la sección **Configuración** de la hoja, haga clic en **Administrador de Active Directory**.

1.   En la barra de comandos, seleccione **Establecer administrador**.

1.   Seleccione una cuenta de usuario de Azure AD para que se convierta en administrador del servidor y, después, haga clic en **Seleccionar**.

1.   En la barra de comandos, seleccione **Guardar**.

### <a name="add-data-factory-msi-as-a-user-to-the-azure-sql-database-managed-instance"></a>Adición de la característica MSI de la factoría de datos como usuario a la Instancia administrada de Azure SQL Database

1.  Inicie SQL Server Management Studio.

2.  Inicie sesión con una cuenta de administrador SQL o una cuenta de administrador de Active Directory.

3.  En el Explorador de objetos, expanda la carpeta Bases de datos -> Bases de datos del sistema.

4.  Haga clic con el botón derecho en la base de datos maestra y seleccione **Nueva consulta**.

5.  Puede consultar el artículo [Identidad de servicio de Azure Data Factory](data-factory-service-identity.md) para obtener el IDENTIFICADOR DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO DE LA ENTIDAD. (No use el IDENTIFICADOR DE LA IDENTIDAD DE SERVICIO para este propósito).

6.  En la ventana de consulta, ejecute el siguiente script para convertir el IDENTIFICADOR DE LA APLICACIÓN DE IDENTIDAD DE SERVICIO a un tipo binario:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your service identity application id}
    select CAST(@applicationId AS varbinary)
    ```

7.  Puede obtener el valor en la ventana de resultados.

8.  Borre la ventana de consulta, copie y ejecute el script siguiente:

    ```sql
    CREATE LOGIN [{MSI name}] FROM EXTERNAL PROVIDER with SID ={your service identity application id in binary type}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{MSI name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{MSI name}]
    ```

9.  El comando se completa correctamente.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Aprovisionamiento del entorno de ejecución de integración de SSIS en Azure en el Portal

Al aprovisionar Azure-SSIS IR con Azure Portal, en la página **Configuración de SQL**, active la opción "Usar autenticación de AAD con la identidad administrada de ADF". (La captura de pantalla siguiente muestra la configuración para el entorno de ejecución de integración con Azure SQL Database. Para el entorno de ejecución de integración con Instancia administrada, la propiedad "Catalog Database Service Tier" [Nivel de servicio de base de datos de catálogo] no está disponible; los demás valores de configuración son iguales).

Para más información sobre cómo crear una instancia de Integration Runtime para la integración de SSIS de Azure, consulte [Creación de una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Configuración de la instancia de Integration Runtime para la integración de SSIS en Azure](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Aprovisionamiento del entorno de ejecución para la integración de SSIS en Azure con PowerShell

Para aprovisionar el entorno de ejecución para la integración de SSIS en Azure con PowerShell, haga lo siguiente:

1.  Instale el módulo [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) .

2.  En el script, no establezca el parámetro *CatalogAdminCredential*. Por ejemplo: 

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
