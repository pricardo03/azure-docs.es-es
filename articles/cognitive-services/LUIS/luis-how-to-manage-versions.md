---
title: Administrar versiones en aplicaciones de LUIS en Azure | Microsoft Docs
description: Obtenga información acerca de cómo administrar versiones en las aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: 672f7991be0fc236e39daf7d1ce1d6080b31815b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380226"
---
# <a name="manage-versions"></a>Administración de versiones

Cada vez que trabaje en el modelo, cree otra [versión](luis-concept-version.md) de la aplicación. 

## <a name="set-active-version"></a>Establecer la versión activa
Para trabajar con versiones, abra la aplicación, seleccione su nombre en la página **Mis aplicaciones** y, a continuación, seleccione **Configuración** en la barra superior.

![Página de versiones](./media/luis-how-to-manage-versions/settings.png)

La página **Configuración** permite configurar opciones para toda la aplicación, incluidas las versiones y los colaboradores. 

## <a name="clone-a-version"></a>Clonar una versión
1. En la página **Configuración**, después de las secciones Configuración y Colaboradores, busque la fila que contiene la versión que quiere clonar. Seleccione los tres puntos (...) en el extremo derecho. 

    ![Propiedades de la fila de versión](./media/luis-how-to-manage-versions/version-section.png)

2. Seleccione **Clonar** en la lista.

    ![Elección de las propiedades de la fila de versión](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. En el cuadro de diálogo **Clone version** (Versión de clon), escriba un nombre para la nueva versión, como "0.2".

   ![Cuadro de diálogo de versión de clon](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > La id. de versión puede constar únicamente de caracteres, dígitos o "." y no puede tener más de 10 caracteres.
 
 Se crea una nueva versión con el nombre especificado y se establece como la versión activa.
 
  ![La versión se crea y agrega a la lista](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Como se muestra en la imagen anterior, una versión publicada se asocia con una marca de color, que indica el tipo de ranura donde se ha publicado: producción (verde), almacenamiento provisional (rojo) y ambos (negro). Se muestran las fechas de aprendizaje y publicación para cada versión publicada.

## <a name="set-active-version"></a>Establecer la versión activa
1. En la página **Configuración**, en la lista **Versiones**, seleccione los tres puntos (...) en el extremo derecho.

2. En la lista emergente, seleccione **Establecer como activo**.

    ![Establecer la versión activa](./media/luis-how-to-manage-versions/set-active-version.png)

    La versión activa aparece resaltada en color azul claro, como se muestra en la captura de pantalla siguiente:

    ![Versión activa](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Importar versión
Puede importar una versión de un archivo JSON. Después de importar una versión, la nueva versión se convierte en la versión activa.

**Para importar una versión:**

1. En la página **Configuración**, seleccione el botón **Import new version** (Importar nueva versión).

    ![Botón Importar](./media/luis-how-to-manage-versions/import-version.png)

2. Seleccione **examinar** y elija el archivo JSON.

    ![Cuadro de diálogo para importar versión](./media/luis-how-to-manage-versions/import-version-dialog.png)

Basta con establecer una id. de versión si la versión en el archivo JSON ya existe en la aplicación.

## <a name="export-version"></a>Exportar versión
Puede exportar una versión a un archivo JSON.

**Para exportar una versión:**

1. En la página **Configuración**, en la lista **Versiones**, seleccione los tres puntos (...) en el extremo derecho.

2. Seleccione **Exportar** en la lista emergente de acciones y seleccione dónde desea guardar el archivo.

## <a name="delete-a-version"></a>Eliminar una versión
Puede eliminar versiones, pero se debe conservar al menos una versión de la aplicación. Puede eliminar todas las versiones, excepto la versión activa. 

1. En la página **Configuración**, en la lista **Versiones**, seleccione los tres puntos (...) en el extremo derecho.

2. Seleccione **Eliminar** en la lista emergente de acciones y seleccione dónde desea guardar el archivo.

    ![Confirmación de eliminación de versión](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Cambiar el nombre de una versión
Puede cambiar el nombre de las versiones, siempre que el nuevo nombre no esté ya en uso.  

1. En la página **Configuración**, en la lista **Versiones**, seleccione los tres puntos (...) en el extremo derecho.

2. Seleccione **Cambiar nombre** en la lista emergente de acciones.

3. Escriba el nuevo nombre de versión y seleccione **Listo**.

    ![Confirmación del cambio de nombre de versión](./media/luis-how-to-manage-versions/rename-popup.png) 
