---
title: 'Tutorial: Creación y administración de datos exportados desde Azure Cost Management'
description: En este artículo se muestra cómo crear y administrar datos de Azure Cost Management para utilizarlos en sistemas externos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/12/2019
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 070844cbf8f6a550b92d764ddb8a31afec12f437
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72374609"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Creación y administración de datos exportados

Si lee el tutorial de análisis de costos, estará familiarizado con la descarga manual de los datos de Cost Management. Sin embargo, puede crear una tarea periódica para exportar automáticamente los datos de Cost Management en el almacenamiento de Azure con una periodicidad diaria, semanal o mensual. Los datos exportados están en formato CSV y contienen toda la información recopilada por Cost Management. A continuación, puede usar los datos exportados en el almacenamiento de Azure con sistemas externos y combinarlos con sus propios datos personalizados. También puede usar los datos exportados en un sistema externo, como un panel u otro sistema financiero.

Vea el vídeo sobre [cómo programar exportaciones a almacenamiento con Azure Cost Management](https://www.youtube.com/watch?v=rWa_xI1aRzo) para conocer más sobre la creación de una exportación programada de los datos de sus costos de Azure a Azure Storage.

Los ejemplos de este tutorial le guiarán durante la exportación de los datos de administración de costos y la comprobación de que los datos se exporten correctamente.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

## <a name="prerequisites"></a>Requisitos previos
La exportación de datos está disponible para varios tipos de cuenta de Azure, entre las que se incluyen las de los clientes con [contrato Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/). Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Para la exportación de datos por usuario y grupo, se admiten los siguientes permisos o ámbitos de Azure por suscripción. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

- Propietario: puede crear, modificar o eliminar exportaciones programadas para una suscripción.
- Colaborador: puede crear, modificar o eliminar sus propias exportaciones programadas. Puede modificar el nombre de las exportaciones programadas creadas por otros usuarios.
- Lector: puede programar las exportaciones para las que tenga permiso.

Para las cuentas de Azure Storage:
- Se necesitan permisos de escritura para cambiar la cuenta de Storage configurada, independientemente de los permisos en la exportación.
- La cuenta de Azure Storage debe configurarse para el almacenamiento de blobs o archivos.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Creación de una exportación diaria

Para crear o ver una exportación de datos o programar una exportación, abra el ámbito deseado en Azure Portal y seleccione **Análisis de costos** en el menú. Por ejemplo, vaya a **Suscripciones**, seleccione una suscripción de la lista y **Análisis de costos**, en el menú. En la parte superior de la página Análisis de costos, haga clic en **Exportar** y elija una opción de exportación. Por ejemplo, haga clic en **Programar exportación**.  

> [!NOTE]
> - Además de las suscripciones, puede crear exportaciones en grupo de recursos, cuentas, departamentos e inscripciones. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).
>- Cuando haya iniciado sesión como asociado en el ámbito de la cuenta de facturación o en el inquilino de un cliente, podrá exportar los datos a una cuenta de Azure Storage vinculada a su cuenta de almacenamiento de asociados. Sin embargo, debe tener una suscripción activa en el inquilino de CSP.
>


Haga clic en **Agregar**, escriba un nombre para la exportación y seleccione la opción **Exportación diaria de costos desde ese mes hasta la fecha actual**. Haga clic en **Next**.

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

**Exportación semanal de costos de los últimos siete días**: la exportación inicial se ejecuta inmediatamente. Las exportaciones posteriores se ejecutan el día de la semana y a la misma hora que la exportación inicial. Los costos son los de los últimos siete días.

**Personalizado**: le permite programar exportaciones semanales y mensuales con las opciones de semana hasta la fecha y de mes hasta la fecha. *La exportación inicial se ejecutará inmediatamente.*

Si tiene una suscripción de pago por uso, MSDN o Visual Studio, es posible que el período de facturación no esté alineado con el mes natural. En el caso de esos tipos de suscripciones y grupos de recursos, puede crear una exportación que se alinee con el período de su factura o con los meses naturales. Para crear una exportación que esté alineada con el mes de facturación, navegue a **Personalizado** y, luego, seleccione **Período de facturación hasta la fecha**.  Para crear una exportación que se alinee con el mes natural, seleccione **Mes hasta la fecha**.
>
>

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
