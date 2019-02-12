---
title: 'Tutorial: Creación y administración de datos exportados desde Azure Cost Management | Microsoft Docs'
description: En este artículo se muestra cómo crear y administrar datos de Azure Cost Management para utilizarlos en sistemas externos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/05/2019
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 5f979f562ee871e7ae5f48de5bc53405cb3d1116
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766426"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Creación y administración de datos exportados

Si lee el tutorial de análisis de costos, estará familiarizado con la descarga manual de los datos de Cost Management. Sin embargo, puede crear una tarea periódica para exportar automáticamente los datos de Cost Management en el almacenamiento de Azure con una periodicidad diaria, semanal o mensual. Los datos exportados están en formato CSV y contienen toda la información recopilada por Cost Management. A continuación, puede usar los datos exportados en el almacenamiento de Azure con sistemas externos y combinarlos con sus propios datos personalizados. También puede usar los datos exportados en un sistema externo, como un panel u otro sistema financiero.

Los ejemplos de este tutorial le guiarán durante la exportación de los datos de administración de costos y la comprobación de que los datos se exporten correctamente.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

## <a name="prerequisites"></a>Requisitos previos
La exportación de datos está disponible para varios tipos de cuenta de Azure, entre las que se incluyen las de los clientes con [contrato Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/). Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Se admiten los siguientes permisos de Azure por suscripción para la exportación de datos por usuario y grupo:

- Propietario: puede crear, modificar o eliminar exportaciones programadas para una suscripción.
- Colaborador: puede crear, modificar o eliminar sus propias exportaciones programadas. Puede modificar el nombre de las exportaciones programadas creadas por otros usuarios.
- Lector: puede programar las exportaciones para las que tenga permiso.

Para las cuentas de Azure Storage:
- Se necesitan permisos de escritura para cambiar la cuenta de Storage configurada, independientemente de los permisos en la exportación.
- La cuenta de Azure Storage debe configurarse para el almacenamiento de blobs o archivos.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Creación de una exportación diaria

Cost Management + facturación &gt;Cost Management &gt; seleccione una suscripción o grupo de recursos de una suscripción &gt; Exportar &gt; **Agregar**.

Escriba un nombre para la exportación y seleccione la opción "Exportación diaria de costos desde ese mes hasta la fecha actual". Haga clic en **Next**.

![Nuevo ejemplo de exportación que muestra el tipo de exportación](./media/tutorial-export-acm-data/basics_exports.png)

Especifique la suscripción de su cuenta de almacenamiento de Azure y, luego, seleccione la cuenta de almacenamiento.  Especifique el contenedor de almacenamiento y la ruta del directorio al que desea que vaya el archivo de exportación.  Haga clic en **Next**.

![Nuevo ejemplo de exportación que muestra los detalles de la cuenta de almacenamiento](./media/tutorial-export-acm-data/storage_exports.png)

Revise los detalles de la exportación y haga clic en **Crear**.

La exportación nueva aparece en la lista de exportaciones. De forma predeterminada, se habilitan las nuevas exportaciones. Si quiere deshabilitar o eliminar una exportación programada, haga clic en cualquier elemento de la lista y, después, haga clic en **Deshabilitar** o **Eliminar**.

Al principio la exportación puede tardar una o dos horas en ejecutarse. Sin embargo, pueden pasar hasta cuatro horas antes de que se muestren los datos en los archivos exportados.

### <a name="export-schedule"></a>Programación de exportaciones

Las exportaciones programadas se ven afectadas por la hora y el día de la semana en que se crearon inicialmente. Cuando se crea una exportación programada, la exportación se ejecuta a la misma hora del día siempre que se vuelva a repetir la exportación. Por ejemplo, imagine que crea una exportación diaria a la 1:00 p. m. La siguiente exportación se ejecuta a la 1:00 p. m. del día siguiente. La hora actual afecta a todos los demás tipos de exportación de la misma manera: siempre se ejecutan a la misma hora del día en que la exportación se creó inicialmente. En otro ejemplo, crea una exportación semanal a las 4:00 p. m. los lunes. La siguiente exportación se ejecuta a las 4:00 p. m. del lunes siguiente. *Los datos exportados están disponibles a las cuatro horas de la ejecución.*

Cada exportación crea un nuevo archivo, por lo que las exportaciones anteriores no se sobrescriben.

Existen tres tipos de opciones de exportación:

**Exportación diaria de los costos del mes hasta la fecha de**: la exportación inicial se ejecuta inmediatamente. Las exportaciones posteriores se ejecutan el siguiente día y a la misma hora que la exportación inicial. Se agregan los datos más recientes de las exportaciones diarias anteriores.

**Costos semanales durante los últimos siete días**: la exportación inicial se ejecuta inmediatamente. Las exportaciones posteriores se ejecutan el día de la semana y a la misma hora que la exportación inicial. Los costos son los de los últimos siete días.

**Personalizado**: le permite programar exportaciones semanales y mensuales con las opciones de semana hasta la fecha y de mes hasta la fecha. *La exportación inicial se ejecutará inmediatamente.*

![La pestaña Exportación nueva - Básico que muestra una selección de semana hasta la fecha personalizada semanalmente](./media/tutorial-export-acm-data/tutorial-export-schedule-weekly-week-to-date.png)

## <a name="verify-that-data-is-collected"></a>Verificación de la recopilación de los datos

Puede verificar fácilmente que los datos de Cost Management se están recopilando y consultar el archivo CSV exportado mediante el Explorador de Azure Storage.

En la lista de exportaciones, haga clic en el nombre de la cuenta de almacenamiento. En la página de la cuenta de almacenamiento, haga clic en Abrir en el explorador. Si ve un cuadro de confirmación, haga clic en **Sí** para abrir el archivo en el Explorador de Azure Storage.

![Página de la cuenta de almacenamiento que muestra información de ejemplo y un vínculo a Abrir en el Explorador](./media/tutorial-export-acm-data/storage-account-page.png)

En el Explorador de Storage, vaya al contenedor que quiera abrir y seleccione la carpeta correspondiente al mes actual. Se muestra una lista de archivos CSV. Seleccione uno y haga clic en **Open** (Abrir).

![Información de ejemplo que se muestra en el Explorador de almacenamiento](./media/tutorial-export-acm-data/storage-explorer.png)

El archivo se abre con el programa o la aplicación que se ha establecido para abrir las extensiones de archivos CSV. Este es un ejemplo en Excel.

![Datos CSV exportados de ejemplo que se muestran en Excel](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Acceso a los datos exportados desde otros sistemas

Uno de los propósitos de exportar los datos de Cost Management es tener acceso a los datos desde sistemas externos. Puede usar un sistema de paneles u otro sistema financiero. Estos sistemas pueden variar considerablemente, por lo que no es práctico mostrar un ejemplo.  Sin embargo, puede comenzar accediendo a sus datos desde aplicaciones en [Introducción a Azure Storage](../storage/common/storage-introduction.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

Vaya al siguiente tutorial para optimizar y mejorar la eficiencia mediante la identificación de recursos inactivos e infrautilizados.

> [!div class="nextstepaction"]
> [Consulta de las recomendaciones sobre optimización y medidas](tutorial-acm-opt-recommendations.md)
