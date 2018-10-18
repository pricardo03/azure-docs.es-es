---
title: Importación de un archivo de Power BI Desktop en Azure Analysis Services | Microsoft Docs
description: Describe cómo importar un archivo de Power BI Desktop (pbix) mediante Azure Portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/27/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e164488a1bf176d5b6c0e28a84cd1ec22cae4cce
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423585"
---
# <a name="import-a-power-bi-desktop-file"></a>Importación de un archivo de Power BI Desktop

Puede importar un modelo de datos en un archivo de Power BI Desktop (pbix) en Azure Analysis Services. Se importan metadatos del modelo, datos en caché y conexiones a origen de datos. Los informes y las visualizaciones no se importan. Los modelos de datos importados de Power BI Desktop se encuentran en el nivel de compatibilidad 1400.

**Restricciones**   

- La importación desde un archivo pbix utiliza la característica de diseñador web en el portal, que es la **versión preliminar**. La funcionalidad es limitada. Para llevar a cabo el desarrollo y las pruebas de modelos más avanzados, es mejor usar Visual Studio (SSDT) y SQL Server Management Studio (SSMS).
- Si su modelo de datos se creó Power BI Desktop, actualización de julio de 2018 (2.60.5169.3201) o posterior, asegúrese de que ninguna característica en vista previa esté habilitada. Las características en vista previa aún no se admiten en Azure Analysis Services.  
Si recibe este error al importar, significa que el archivo pbix tiene características en vista previa habilitadas que aún no se admiten en Azure Analysis Services.

    ![Advertencia de nivel de compatibilidad](./media/analysis-services-import-pbix/aas-import-pbix-cl-warning.png)   
- Debe tener permisos de administrador del servidor para importar desde un archivo pbix.
- El modelo pbix puede conectarse solo a orígenes de datos de **Azure SQL Database** y **Azure SQL Data Warehouse**.
- El modelo pbix no puede tener conexiones en vivo o de DirectQuery. 


## <a name="to-import-from-pbix"></a>Para importar desde pbix

1. En la opción **Información general** > **Web designer** (Diseñador web) del servidor, haga clic en **Abrir**.

    ![Creación de un modelo en Azure Portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. En **Web designer** > **Modelos**, haga clic en **+ Agregar**.

    ![Creación de un modelo en Azure Portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. En **Nuevo modelo**, escriba un nombre de modelo y, a continuación, seleccione el archivo de Power BI Desktop.

    ![Cuadro de diálogo Nuevo modelo en Azure Portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. En **Importar**, encuentre y seleccione su archivo.

     ![Cuadro de diálogo Connect (Conexión) en Azure Portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>Cambio de credenciales

De forma predeterminada, las credenciales usadas para conectarse a un origen de datos se establecen como ServiceAccount cuando se importa un modelo de datos desde un archivo pbix. Después de que se ha importado un modelo desde un archivo pbix, puede cambiar las credenciales mediante los métodos siguientes:

- Use la versión de SSMS de julio de 2018 (versión 17.8.1) o posteriores para editar credenciales. Esta es la forma más sencilla.
- Use el [comando Alter](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl) de TMSL en el [objeto DataSources](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl) para modificar la propiedad de cadena de conexión. 
- Abra el modelo en Visual Studio, edite las credenciales para la conexión de origen de datos y, a continuación, vuelva a implementar el modelo.

Para cambiar las credenciales mediante SSMS. 

1. En SSMS, expanda la base de datos > **Conexiones**. 
2. Haga clic en la conexión de base de datos y, a continuación, haga clic en **Actualizar credenciales**. 

    ![Actualizar credenciales](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. En el cuadro de diálogo de credenciales, seleccione un tipo de credencial y escriba las credenciales. Para la autenticación de SQL, seleccione Base de datos. Para la cuenta de organización (OAuth), seleccione Cuenta de Microsoft.
    ![Editar credenciales](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

La versión de julio de 2018 de Power BI Desktop incluye una nueva característica para cambiar los permisos del origen de datos. En **Inicio** , haga clic en **Editar consultas**  > **Configuración del origen de datos**. Seleccione la conexión de origen de datos y, a continuación, haga clic en **Editar permisos**.


## <a name="see-also"></a>Otras referencias

[Creación de un modelo en Azure Portal](analysis-services-create-model-portal.md)   
[Conexión a Azure Analysis Services](analysis-services-connect.md)  
