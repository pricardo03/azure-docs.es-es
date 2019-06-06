---
title: Conectar con el Explorador de datos de Azure con ODBC
description: En este artículo, aprenderá a configurar una conexión de Open Database Connectivity (ODBC) al explorador de datos de Azure.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 02ae9673f1dc402ee1500b466d7e259263ef3262
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494848"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Conectar con el Explorador de datos de Azure con ODBC

Abre la conectividad de base de datos ([ODBC](/sql/odbc/reference/odbc-overview)) es una interfaz de programación de aplicaciones ampliamente aceptado (API) para el acceso a la base de datos. Usar ODBC para conectarse al explorador de datos de Azure desde las aplicaciones que no tienen un conector específico.

En segundo plano, las aplicaciones llaman a funciones en la interfaz ODBC, que se implementan en los módulos específicos de la base de datos denominados *controladores*. El Explorador de datos de Azure admite un subconjunto del protocolo de comunicación de SQL Server ([MS TDS](/azure/kusto/api/tds/)), así que puede usar el controlador ODBC para SQL Server.

En este artículo, aprenderá a usar el controlador ODBC de SQL Server, para que pueda conectarse al explorador de datos de Azure desde cualquier aplicación que admite ODBC.

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente:

* [Microsoft ODBC Driver para SQL Server versión 17.2.0.1 o posterior](/sql/connect/odbc/download-odbc-driver-for-sql-server) para su sistema operativo.

## <a name="configure-the-odbc-data-source"></a>Configurar el origen de datos ODBC

Siga estos pasos para configurar un origen de datos ODBC mediante el controlador ODBC para SQL Server.

1. En Windows, busque *orígenes de datos ODBC*y abra la aplicación de escritorio de orígenes de datos ODBC.

1. Seleccione **Agregar**.

    ![Agregar origen de datos](media/connect-odbc/add-data-source.png)

1. Seleccione **ODBC Driver 17 for SQL Server** , a continuación, **finalizar**.

    ![Seleccionar controlador](media/connect-odbc/select-driver.png)

1. Escriba un nombre y una descripción para la conexión y el clúster que desea conectarse, a continuación, seleccione **siguiente**. El clúster de dirección URL debe tener el formato  *\<ClusterName\>.\< Región\>. kusto.windows.net*.

    ![Seleccionar servidor](media/connect-odbc/select-server.png)

1. Seleccione **integrado en Active Directory** , a continuación, **siguiente**.

    ![Integrada en Active Directory](media/connect-odbc/active-directory-integrated.png)

1. Seleccione la base de datos con los datos de ejemplo, a continuación, **siguiente**.

    ![Cambiar base de datos predeterminada](media/connect-odbc/change-default-database.png)

1. En la siguiente pantalla, deje todas las opciones como a continuación, seleccione el valor predeterminado es **finalizar**.

1. Seleccione **Probar origen de datos**.

    ![Origen de datos de prueba](media/connect-odbc/test-data-source.png)

1. Compruebe que la prueba se realizó correctamente, a continuación, seleccione **Aceptar**. Si la prueba no se realizó correctamente, compruebe los valores que especificó en los pasos anteriores y asegúrese de que tiene permisos suficientes para conectarse al clúster.

    ![Prueba se realizó correctamente](media/connect-odbc/test-succeeded.png)

## <a name="next-steps"></a>Pasos siguientes

* [Conectarse al explorador de datos de Azure desde una plantilla](tableau.md)