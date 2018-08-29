---
title: Clasificación y detección de datos de Azure SQL Database| Microsoft Docs
description: Clasificación y detección de datos de Azure SQL Database
services: sql-database
author: giladmit
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: giladm
ms.openlocfilehash: 6ef9a701f3a228e4c40da94f83310ef2884a3f59
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "42142945"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Clasificación y detección de datos de Azure SQL Database
La opción Clasificación y detección de datos (actualmente en su versión preliminar) proporciona funcionalidades avanzadas integradas en Azure SQL Database para **detectar**, **clasificar**, **etiquetar** & **proteger** la información confidencial de las bases de datos.
Las funciones de detección y clasificación de la información confidencial más importante (empresarial, financiera, médica, personal, etc.) desempeñan un rol fundamental en el modo en que se protege la información de su organización. Puede servir como infraestructura para:
* Ayudar a cumplir los requisitos de cumplimiento de normas y los estándares relacionados con la privacidad de datos.
* Varios escenarios de seguridad, como la supervisión (auditoría) y las alertas relacionadas con accesos anómalos a información confidencial.
* Controlar el acceso y mejorar la seguridad de las bases de datos que contienen información altamente confidencial.

Data Discovery & Classification forma parte de la oferta de [protección contra amenazas avanzada (ATP) de SQL](sql-advanced-threat-protection.md). Dicha oferta es un paquete unificado para funcionalidades avanzadas de seguridad de SQL. Se puede acceder y administrar Data Discovery & Classification a través del portal de ATP de SQL.

> [!NOTE]
> Este documento tiene relación solo con Azure SQL Database. Para SQL Server (local), consulte [Clasificación y detección de datos de SQL](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>¿Qué es Data Discovery and Classification?
La función de clasificación y detección de datos incluye un conjunto de servicios avanzados y nuevas funcionalidades de SQL y forma un nuevo paradigma de Information Protection de SQL destinado a proteger los datos, no solo la base de datos:
* **Detección y recomendaciones**: el motor de clasificación examina la base de datos e identifica las columnas que contienen datos potencialmente confidenciales. A continuación, le proporciona una manera sencilla de revisar y aplicar las recomendaciones de clasificación adecuadas a través de Azure Portal.
* **Etiquetado**: las etiquetas de clasificación de la confidencialidad se pueden etiquetar de forma persistente en columnas con nuevos atributos de metadatos de clasificación introducidos en el motor de SQL. A continuación, estos metadatos se pueden utilizar para escenarios avanzados de auditoría y protección basados en la confidencialidad.
* **Confidencialidad del conjunto de resultados de consulta**: la confidencialidad del conjunto de resultados de consulta se calcula en tiempo real con fines de auditoría.
* **Visibilidad**: el estado de clasificación de la base de datos puede verse en un panel detallado en el portal. Además, puede descargar un informe (en formato de Excel) para usarlo con fines de auditoría y cumplimiento de normas, así como para otras necesidades.

## <a id="subheading-2"></a>Descubrir, clasificar y etiquetar columnas confidenciales
En la siguiente sección se describen los pasos para detectar, clasificar y etiquetar las columnas que contienen datos confidenciales de su base de datos, así como para ver el estado actual de clasificación de su base de datos y exportar informes.

La clasificación incluye dos atributos de metadatos:
* Etiquetas: atributos de clasificación principales, que se utilizan para definir el nivel de confidencialidad de los datos almacenados en la columna.  
* Tipos de información: proporcionan una granularidad adicional en el tipo de datos almacenados en la columna.

## <a name="classify-your-sql-database"></a>Clasifique su instancia de SQL Database

1. Vaya a [Azure Portal](https://portal.azure.com).

2. Navegue a **Protección contra amenazas avanzada** en el encabezado de Seguridad en el panel dela base de datos de Azure SQL. Haga clic para habilitar Advanced Threat Protection, y haga clic en la tarjeta **Data discovery & classification (preview)** (Data discovery & classification [versión preliminar])

   ![Examen de una base de datos](./media/sql-data-discovery-and-classification/data_classification.png)

3. La pestaña **Introducción** incluye un resumen del estado actual de clasificación de la base de datos, incluida una lista detallada de todas las columnas clasificadas, que también puede filtrar para ver solo tipos de información, etiquetas y partes del esquema específicos. Si aún no ha clasificado ninguna columna, [vaya al paso 5](#step-5).

   ![Resumen del estado de clasificación actual](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Para descargar un informe en formato de Excel, haga clic en la opción **Exportar** del menú superior de la ventana.

   ![Exportación a Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5.  <a id="step-5"></a>Para empezar a clasificar los datos, haga clic en la **pestaña Clasificación** en la parte superior de la ventana.

    ![Clasifique sus datos](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. El motor de clasificación examina las columnas de su base de datos que contienen datos potencialmente confidenciales y proporciona una lista de **clasificaciones de columna recomendadas**. Para ver y aplicar las recomendaciones de clasificación:

    * Para ver la lista de clasificaciones de columnas recomendadas, haga clic en el panel de recomendaciones en la parte inferior de la ventana:

      ![Clasifique sus datos](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

    * Revise la lista de recomendaciones: para aceptar una recomendación para una columna específica, active la casilla de la columna izquierda de la fila correspondiente. También puede aceptar *todas las recomendaciones*. Para ello, active la casilla del encabezado de la tabla de recomendaciones.

       ![Revise la lista de recomendaciones](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

    * Para aplicar las recomendaciones seleccionadas, haga clic en el botón azul **Accept selected recommendations** (Aceptar recomendaciones seleccionadas).

      ![Aplicación de recomendaciones](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Si quiere, también puede **clasificar manualmente** las columnas, o puede optar por hacerlo así además de realizar la clasificación basada en recomendaciones:

    * Haga clic en **Agregar clasificación** en el menú superior de la ventana.

      ![Agregue clasificación de forma manual](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

    * En la ventana de contexto que se abre, seleccione el esquema > la tabla > la columna que quiera clasificar, y el tipo de información y la etiqueta de confidencialidad. A continuación, haga clic en el botón azul **Agregar clasificación** situado en la parte inferior de la ventana de contexto.

      ![Seleccione la columna que quiera clasificar](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Para completar la clasificación y etiquetar de forma persistente las columnas de la base de datos con los nuevos metadatos de clasificación, haga clic en **Guardar** en el menú superior de la ventana.

   ![Save](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Auditoría del acceso a datos confidenciales

Un aspecto importante del paradigma de protección de la información es la capacidad de supervisar el acceso a información confidencial. La [auditoría de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) se ha mejorado para incluir un nuevo campo en el registro de auditoría denominado *data_sensitivity_information*, que registra la clasificación de confidencialidad (etiquetas) de los datos reales que devuelve la consulta.

![Registro de auditoría](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Clasificación automática o mediante programación

Puede utilizar T-SQL para agregar o quitar las clasificaciones de columna, así como recuperar todas las clasificaciones para toda la base de datos.

> [!NOTE]
> Al usar T-SQL para administrar las etiquetas, no se valida que las etiquetas que se agregan a una columna existan en la directiva de protección de información de la organización (el conjunto de etiquetas que aparecen en las recomendaciones del portal). Por lo tanto, validarlas es su responsabilidad.

* Agregue o actualice la clasificación de una o varias columnas: [ADD SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/en-us/sql/t-sql/statements/add-sensitivity-classification-transact-sql) (Agregar la clasificación de la confidencialidad)
* Quite la clasificación de una o varias columnas: [DROP SENSITIVITY CLASSIFICATION](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-sensitivity-classification-transact-sql) (Eliminar la clasificación de la confidencialidad)
* Vea todas las clasificaciones de la base de datos: [sys.sensitivity_classifications](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

También puede usar las API de REST para administrar las clasificaciones mediante programación. Las API de REST publicadas se admiten las siguientes operaciones:
* [Crear o Actualizar](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate): crea o actualiza la etiqueta de confidencialidad de una columna determinada
* [Eliminar](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete): elimina la etiqueta de confidencialidad de una columna determinada
* [Obtener](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get): obtiene la etiqueta de confidencialidad de una columna determinada
* [List By Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listbydatabase) (Enumerar por base de datos): enumera las etiquetas de confidencialidad de una base de datos determinada


## <a id="subheading-5"></a>Pasos siguientes

- Obtenga más información sobre [Protección contra amenazas avanzada de SQL](sql-advanced-threat-protection.md).
- Considere la posibilidad de configurar la [auditoría de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) para supervisar y auditar el acceso a los datos confidenciales clasificados.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
