---
title: Creación y uso compartido de paneles en Azure Portal
description: En este artículo se describe cómo crear, personalizar, publicar y compartir paneles en Azure Portal.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/29/2020
ms.author: mblythe
ms.openlocfilehash: fee3d9f0ef4f2c622d42373f4ed8c895f2c76adf
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901071"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Creación y uso compartido de paneles en Azure Portal

Los paneles son una vista centrada y organizada de los recursos de nube de Azure Portal. Use los paneles como un área de trabajo donde puede iniciar tareas para las operaciones diarias y supervisar los recursos rápidamente. Por ejemplo, puede crear paneles personalizados basados en proyectos, tareas o roles de usuario.

Azure Portal proporciona un panel predeterminado como punto de partida. Puede editar el panel predeterminado. Cree y personalice paneles adicionales, y publique y comparta paneles para que estén disponibles para otros usuarios. En este artículo se describe cómo crear un nuevo panel, personalizar la interfaz, y publicar y compartir paneles.

## <a name="create-a-new-dashboard"></a>Creación de un nuevo panel

En este ejemplo, se crea un nuevo panel privado y se le asigna un nombre. Para comenzar, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el menú de Azure Portal, seleccione **Panel**. Es posible que la vista predeterminada ya esté definida en el panel.

    ![Apertura del panel](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. Seleccione **Nuevo panel**.

    ![Captura de pantalla del panel predeterminado](./media/azure-portal-dashboards/create-new-dashboard.png)

    Esta acción abre la **Galería de iconos**, desde la que podrá seleccionar los iconos, y una cuadrícula vacía, desde donde podrá organizarlos.

    ![Captura de pantalla de la galería de iconos y la cuadrícula vacía](./media/azure-portal-dashboards/dashboard-name.png)

1. Seleccione el texto **Mi panel** en la etiqueta del panel y escriba un nombre que le ayude a identificar fácilmente el panel personalizado.

1. Seleccione **Personalización finalizada** en el encabezado de página para salir del modo de edición.

Ahora, la vista de panel muestra el panel vacío. Seleccione la flecha situada junto al nombre del panel para ver los paneles disponibles. La lista puede incluir paneles que otros usuarios hayan creado y compartido.

## <a name="edit-a-dashboard"></a>Edición de paneles

Ahora vamos a editar el panel para agregar y organizar los iconos que representan los recursos de Azure y cambiar su tamaño.

### <a name="add-tiles"></a>Agregar iconos

Para agregar iconos a un panel, siga estos pasos:

1. Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** en el encabezado de la página.

    ![Captura de pantalla con el icono de edición resaltado](./media/azure-portal-dashboards/dashboard-edit.png)

1. Examine la **Galería de iconos** o use el campo de búsqueda para buscar el icono que quiera.

1. Seleccione **Agregar** para agregar el icono al panel con una ubicación y un tamaño predeterminados. O bien, arrastre el icono a la cuadrícula y colóquelo donde quiera.

Muchas páginas de recursos incluyen un icono de marcador en la barra de comandos. Si selecciona el icono, se ancla un icono que representa la página de origen al panel activo. Este método es una forma alternativa de agregar iconos al panel.

![Captura de pantalla de la barra de comandos de la página con el icono de anclaje](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Si trabaja con más de una organización, agregue el icono **Identidad de la organización** al panel para mostrar con claridad a qué organización pertenecen los recursos.
>
>

### <a name="resize-or-rearrange-tiles"></a>Reorganizar los iconos o cambiar su tamaño

Para cambiar el tamaño de un icono o para reorganizar los iconos de un panel, siga estos pasos:

1. Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** en el encabezado de la página.

1. Seleccione el menú contextual en la esquina superior derecha de un icono. A continuación, elija un tamaño de icono. Los iconos que admiten cualquier tamaño también incluyen un "identificador" en la esquina inferior derecha que le permite arrastrar el icono al tamaño que quiera.

    ![Captura de pantalla del panel con el menú de tamaño de icono abierto](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Seleccione un icono y arrástrelo a una nueva ubicación en la cuadrícula para organizar el panel.

### <a name="additional-tile-configuration"></a>Configuración de iconos adicional

Puede que sea necesario configurar adicionalmente algunos iconos para que muestren la información necesaria. Por ejemplo, el icono **Gráfico de métricas** debe configurarse para mostrar una métrica de **Azure Monitor**. También puede personalizar los datos del icono para invalidar la configuración de tiempo predeterminada del panel.

Cualquier icono que necesite configuración muestra el banner **Configurar icono** hasta que se personaliza. Seleccione ese banner y, a continuación, realice la configuración necesaria.

![Captura de pantalla del icono que requiere configuración](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Un icono de Markdown le permite mostrar contenido estático personalizado en el panel. Podrían ser instrucciones básicas, una imagen, un conjunto de hipervínculos o incluso la información de contacto. Para más información sobre el uso de un icono de Markdown, vea [Uso de un icono de Markdown en los paneles de Azure para mostrar contenido personalizado](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Personalizar los datos del icono

Los datos del panel muestran automáticamente la actividad de las últimas 24 horas. Para mostrar un intervalo de tiempo diferente solo para este icono, siga estos pasos:

1. Seleccione **Personalizar los datos del icono** en el menú contextual o el filtro ![icono de filtro](./media/azure-portal-dashboards/dashboard-filter.png) de la esquina superior izquierda del icono.

    ![Captura de pantalla del menú contextual del icono](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. Active la casilla para **Anular la configuración de tiempo del panel en el nivel de icono**.

    ![Captura de pantalla del cuadro de diálogo para configurar las opciones de tiempo del icono](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Elija el intervalo de tiempo que se mostrará para este icono. Puede elegir entre los últimos 30 minutos y los últimos 30 días o definir un intervalo personalizado.

1. Elija la granularidad de tiempo que se mostrará. Puede mostrar incrementos de entre un minuto y un mes.

1. Seleccione **Aplicar**.

## <a name="delete-a-tile"></a>Eliminar un icono

Para quitar un icono de un panel, siga estos pasos:

* Seleccione el menú contextual en la esquina superior derecha del icono y, a continuación, seleccione **Quitar del panel**. O bien,

* Seleccione ![icono de edición](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Editar** para acceder al modo de personalización. Mantenga el puntero en la esquina superior derecha del icono y, a continuación, seleccione el icono de eliminación ![icono de eliminación](./media/azure-portal-dashboards/dashboard-delete-icon.png) para quitar el icono del panel.

   ![Captura de pantalla que muestra cómo quitar el icono del panel](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Clonar un panel

Para usar un panel existente como plantilla para uno nuevo, siga estos pasos:

1. Asegúrese de que la vista de panel muestra el panel que quiere copiar.

1. En el encabezado de página, seleccione ![icono para clonar](./media/azure-portal-dashboards/dashboard-clone.png) **Clonar**.

1. Se abre una copia del panel denominada **Clon de** *nombre del panel* en modo de edición. Utilice los pasos anteriores de este artículo para cambiar el nombre del panel y personalizarlo.

## <a name="publish-and-share-a-dashboard"></a>Publicar y compartir un panel

Al crear un panel, será privado de forma predeterminada, lo que significa que será la única persona que puede verlo. Para poner paneles a disposición de otros usuarios, puede compartirlos con otros usuarios. En primer lugar, debe publicar el panel como recurso de Azure. Para publicar y compartir un panel personalizado, siga estos pasos:

1. Seleccione ![icono para compartir](./media/azure-portal-dashboards/dashboard-share-icon.png) **Compartir** en el encabezado de página. Se abre **Uso compartido y control de acceso**.

1. Compruebe que se muestra el nombre del panel correcto.

1. Seleccione un **Nombre de suscripción**. Los usuarios con acceso a la suscripción pueden usar el panel compartido. El control de acceso basado en rol de Azure determina el acceso a los recursos que representan los iconos individuales.

1. Active la casilla para publicar este panel en el grupo de recursos "paneles" para la suscripción seleccionada. O bien desactive la casilla y elija Publicar en un grupo de recursos existente.

1. Elija una ubicación para el recurso del panel. Es recomendable buscar el panel con otros recursos. Si elige entre los grupos de recursos existentes, el panel se ubica automáticamente con ese grupo de recursos.

1. Seleccione **Publicar**.

    ![Captura de pantalla del cuadro de diálogo de publicación del panel](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Definir el control de acceso en un panel compartido

Una vez publicado el panel, puede seguir estos pasos para administrar quién tiene acceso al panel:

1. En el panel **Uso compartido y control de acceso**, seleccione **Administrar usuarios**.

    ![Captura de pantalla del cuadro de diálogo Uso compartido y control de acceso del panel](./media/azure-portal-dashboards/dashboard-share-access-control.png)

1. Se abrirá la página **Control de acceso**. Puede revisar el nivel de acceso de un usuario o agregar una nueva asignación de roles. Al agregar una asignación de roles aquí, otorga permisos para el panel.

> [!NOTE]
> Los iconos son vistas representativas de los recursos de la organización. El acceso a los recursos se administra mediante la asignación del control de acceso basado en rol y los permisos se heredan de la suscripción hasta el recurso. Asignar acceso a un panel no asigna permisos automáticamente a los recursos que se muestran en él. Para más información sobre los permisos para los paneles compartidos y el control de acceso basado en rol para los recursos, vea [Uso compartido de paneles de Azure mediante el control de acceso basado en rol](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Abrir un panel compartido

Para buscar y abrir un panel compartido, siga estos pasos:

1. Seleccione la flecha junto al nombre del panel.

1. Seleccione una opción de la lista de paneles que se muestra o elija **Examinar todos los paneles** si no aparece el panel que quiere abrir.

    ![Captura de pantalla del menú de selección del panel](./media/azure-portal-dashboards/dashboard-browse.png)

1. En el campo **Tipo**, seleccione **Paneles compartidos**.

1. Seleccione una o varias suscripciones. También puede escribir texto para filtrar los paneles por nombre.

1. Seleccione un panel en la lista de paneles compartidos.

## <a name="delete-a-dashboard"></a>Eliminar un panel

Para eliminar permanentemente un panel privado o compartido, siga estos pasos:

1. Seleccione el panel que quiera eliminar en la lista situada junto al nombre del panel.

1. Seleccione ![icono de eliminación](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Eliminar** en el encabezado de página.

1. Para un panel privado, seleccione **Aceptar** en el cuadro de diálogo de confirmación para quitar el panel. Para un panel compartido, en el cuadro de diálogo de confirmación, active la casilla para confirmar que otros usuarios ya no podrán ver el panel publicado. Después, seleccione **Aceptar**.

    ![Captura de pantalla de confirmación de eliminación](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso compartido de paneles de Azure mediante el control de acceso basado en rol](azure-portal-dashboard-share-access.md)
* [Creación mediante programación de paneles de Azure](azure-portal-dashboards-create-programmatically.md)
