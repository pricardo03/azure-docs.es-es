---
title: Uso de un icono de Markdown en los paneles de Azure
description: Aprenda a agregar un icono de Markdown a un panel de Azure para mostrar contenido estático
services: azure-portal
keywords: ''
author: kfollis
ms.author: v-biyu
origin.date: 01/25/2019
ms.date: 03/04/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: ec8cbddda4137656a53fd4968c451cd413959274
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551607"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Uso de un icono de Markdown en los paneles de Azure para mostrar contenido personalizado

Puede agregar un icono de Markdown a los paneles de Azure para mostrar contenido estático y personalizado. Por ejemplo, con un icono Markdown puede mostrar instrucciones básicas, una imagen o un conjunto de hipervínculos.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Incorporación de un icono de Markdown al panel

1. Seleccione **Panel** en la barra lateral de Azure Portal. Si ha creado algún panel personalizado, en la vista del panel, seleccione el panel donde debe aparecer el icono de Markdown personalizado de la lista desplegable. Seleccione el icono de edición para abrir la **Galería de iconos**.

   ![Captura de pantalla de la vista de edición del panel](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

2. En la **Galería de iconos**, busque el denominado **Markdown** y haga clic en **Agregar**. Este icono se agregará al panel y el panel **Editar Markdown** se abrirá.

3. Edite los campos **Título**, **Subtítulo** y **Contenido** para personalizar el icono. En el ejemplo que se muestra a continuación, el icono de Markdown se ha editado para mostrar información personalizada del departamento de soporte técnico.

   ![Captura de pantalla de la vista de edición del icono de Markdown](./media/azure-portal-markdown-tile/azure-portal-edit-markdown-tile.png)

4. Seleccione **Listo** para descartar el panel **Editar Markdown**. El contenido aparecerá en el icono de Markdown, cuyo tamaño puede cambiarse al arrastrar el manipulador de la esquina inferior derecha.

   ![Captura de pantalla del icono de Markdown personalizado](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Funcionalidades y limitaciones del contenido de Markdown

En el icono de Markdown puede usar cualquier combinación de texto sin formato, la sintaxis de Markdown y contenido HTML. Azure Portal usa una biblioteca de código abierto denominada _marked_ para transformar el contenido en HTML que se muestra en el icono. El código HTML generado por _marked_ se procesa previamente en el portal antes de representarse. Este paso ayuda a garantizar que la personalización no afecta a la seguridad o al diseño del portal. Durante ese procesamiento previo, se quita cualquier parte del código HTML que suponga una posible amenaza. En el portal no se permiten los siguientes tipos de contenido:

* JavaScript: las etiquetas `<script>` se quitarán, así como las evaluaciones de JavaScript insertadas.
* iframes: las etiquetas `<iframe>` se quitarán.
* Style: las etiquetas `<style>` se quitarán. Oficialmente no se admiten los atributos de estilo insertados en los elementos HTML. Quizá algunos elementos insertados le pudieran ser de ayuda, pero si afectan al diseño del portal, dejarían de funcionar en cualquier momento. El icono de Markdown está pensado para contenido estático básico que usa los estilos predeterminados del portal.

## <a name="next-steps"></a>Pasos siguientes

* Para crear un panel personalizado, consulte [Creación y uso compartido de paneles en Azure Portal](../azure-portal/azure-portal-dashboards.md)
