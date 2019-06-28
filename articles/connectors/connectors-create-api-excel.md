---
title: 'Conexión a Excel Online: Azure Logic Apps'
description: Administración de datos con las API REST de Excel Online y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
tags: connectors
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 28739ad65462acc9f2d2ed7db1e9ed14d19f032c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311971"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Administración de datos de Excel Online con Azure Logic Apps

Con Azure Logic Apps y el conector de Excel Online puede crear tareas automatizadas y flujos de trabajo basados en los datos de Excel Online para empresas o OneDrive. Este conector ofrece acciones que le ayudarán a trabajar con los datos y a administrar las hojas de cálculo, por ejemplo:

* Crear tablas y hojas de cálculo.
* Obtener y administrar hojas de cálculo, tablas y filas.
* Agregar filas individuales y columnas de clave.

Los resultados de estas acciones se pueden usar con acciones para otros servicios. Por ejemplo, si usa una acción que crea hojas de cálculo de cada semana, puede usar otra acción que envíe correo electrónico de confirmación mediante el conector de Office 365 Outlook.

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> Los conectores de [Excel Online para empresas](/connectors/excelonlinebusiness/) y [Excel Online para OneDrive](/connectors/excelonline/) funcionan con Azure Logic Apps y difieren del [conector de Excel para PowerApps](/connectors/excel/).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Una [cuenta de Office 365](https://www.office.com/) para la cuenta Microsoft personal o la profesional.

  Los datos de Excel pueden existir como archivo de valores separados por comas (CSV) en una carpeta de almacenamiento, por ejemplo, en OneDrive. 
  También puede utilizar este archivo CSV con el [conector de archivos planos](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiera acceder a los datos de Excel Online. Este conector ofrece únicamente las acciones, por lo que, para iniciar la aplicación lógica, debe seleccionar un desencadenador independiente, por ejemplo, el de **periodicidad**.

## <a name="add-excel-action"></a>Incorporación de una acción de Excel

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. En el desencadenador, elija **New step** (Nuevo paso).

1. En el cuadro de búsqueda, escriba "excel" como filtro. En la lista de acciones, seleccione la que desee.

1. Si se le pide que inicie sesión su cuenta de Office 365, elija **Iniciar sesión**.

   Sus credenciales autorizan a la aplicación lógica para crear una conexión a Excel Online y acceder a sus datos.

1. Continue proporcionando los detalles necesarios para la acción seleccionada y con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnicos, como los desencadenadores, las acciones y los límites, tal como lo describen los archivos OpenAPI (antes Swagger) del conector, consulte estas páginas de referencia del conector:

* [Excel Online para empresas](/connectors/excelonlinebusiness/)
* [Excel Online para OneDrive](/connectors/excelonline/)

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
