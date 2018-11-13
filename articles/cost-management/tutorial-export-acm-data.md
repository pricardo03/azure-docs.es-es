---
title: 'Tutorial: Creación y administración de datos exportados desde Azure Cost Management | Microsoft Docs'
description: En este artículo se muestra cómo crear y administrar datos de Azure Cost Management para utilizarlos en sistemas externos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 7f93a225db845840545b761d812f5a8a81f76f91
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913570"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Creación y administración de datos exportados

Si lee el tutorial de análisis de costos, estará familiarizado con la descarga manual de los datos de Cost Management. Sin embargo, puede crear una tarea periódica diaria para exportar automáticamente los datos de Cost Management en el almacenamiento de Azure. Los datos exportados están en formato CSV y contienen toda la información recopilada por Cost Management. A continuación, puede usar los datos exportados en el almacenamiento de Azure con sistemas externos y combinarlos con sus propios datos personalizados. También puede usar los datos exportados en un sistema externo, como un panel u otro sistema financiero.

Los ejemplos de este tutorial le guiarán durante la exportación de los datos de administración de costos y la comprobación de que los datos se exporten correctamente.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

## <a name="prerequisites"></a>Requisitos previos

La exportación de datos está disponible para todos los clientes con [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Se admiten los siguientes permisos de Azure por suscripción para la exportación de datos por usuario y grupo:

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

Escriba un nombre para la exportación y especifique la suscripción, la cuenta de almacenamiento de Azure, el contenedor y el directorio de almacenamiento de archivos o el contenedor de blobs. Después, haga clic en **Crear**.

![Nueva exportación](./media/tutorial-export-acm-data/new-export01.png)

La exportación nueva aparece en la lista de exportaciones. De forma predeterminada, se habilitan nuevas exportaciones y se ejecutan todos los días. Si quiere deshabilitar o eliminar una exportación programada, haga clic en cualquier elemento de la lista y, después, haga clic en **Deshabilitar** o **Eliminar**.

Al principio la exportación puede tardar una o dos horas en ejecutarse. Sin embargo, pueden pasar hasta cuatro horas antes de que se muestren los datos en los archivos exportados.

## <a name="verify-that-data-is-collected"></a>Verificación de la recopilación de los datos

Puede verificar fácilmente que los datos de Cost Management se están recopilando y consultar el archivo CSV exportado mediante el Explorador de Azure Storage.

En la lista de exportaciones, haga clic en el nombre de la cuenta de almacenamiento. En la página de la cuenta de almacenamiento, haga clic en Abrir en el explorador. Si ve un cuadro de confirmación, haga clic en **Sí** para abrir el archivo en el Explorador de Azure Storage.

![Página de la cuenta de almacenamiento](./media/tutorial-export-acm-data/storage-account-page.png)

En el Explorador de Storage, vaya al contenedor que quiera abrir y seleccione la carpeta correspondiente al mes actual. Se muestra una lista de archivos CSV. Seleccione uno y haga clic en **Open** (Abrir).

![Explorador de Storage](./media/tutorial-export-acm-data/storage-explorer.png)

El archivo se abre con el programa o la aplicación que se ha establecido para abrir las extensiones de archivos CSV. Este es un ejemplo en Excel.

![Ejemplo de exportación de datos](./media/tutorial-export-acm-data/example-export-data.png)

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
