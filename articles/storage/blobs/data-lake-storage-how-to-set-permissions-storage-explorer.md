---
title: Establecimiento de permisos para Data Lake Storage Gen2 con Explorador de Azure Storage
description: En este tema, aprenderá a establecer permisos con el Explorador de Azure Storage en archivos y directorios dentro de su cuenta de almacenamiento compatible con Azure Data Lake Storage Gen2.
services: storage
author: normesta
ms.custom: mvc
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: normesta
ms.openlocfilehash: 5f0211765c96cad668abaad7d42da87ec88298c3
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939307"
---
# <a name="set-file-and-directory-level-permissions-using-azure-storage-explorer-with-azure-data-lake-storage-gen2"></a>Establecimiento de permisos en el nivel de archivo y directorio mediante el Explorador de Azure Storage con Azure Data Lake Storage Gen2

Los archivos almacenados en Azure Data Lake Storage Gen2 admiten la administración de listas de control de acceso (ACL) y permisos detallados. En conjunto, la administración de ACL y los permisos detallados le permiten administrar el acceso a los datos de manera muy detallada.

En este artículo, aprenderá a usar Explorador de Azure Storage para:

> [!div class="checklist"]
> * Establecer permisos en el nivel de archivo
> * Establecer permisos en el nivel de directorio
> * Agregar usuarios o grupos a una lista de control de acceso

## <a name="prerequisites"></a>Requisitos previos

Para describir mejor el proceso, es necesario que complete la guía de [inicio rápido de Explorador de Azure Storage](data-lake-storage-Explorer.md). Esto garantiza que la cuenta de almacenamiento tendrá el estado más adecuado (con el sistema de archivos creado y los datos cargados en él).

## <a name="managing-access"></a>Administración del acceso

Puede establecer permisos en la raíz del sistema de archivos. Para ello, debe iniciar sesión en el Explorador de Azure Storage con su cuenta individual con derechos para hacerlo (en contraposición al uso de una cadena de conexión). Haga clic con el botón derecho en el sistema de archivos y seleccione **Administrar permisos** y aparecerá el cuadro de diálogo **Administrar permiso**.

![Explorador de Microsoft Azure Storage: administración del acceso al directorio](media/storage-quickstart-blobs-storage-Explorer/manageperms.png)

El cuadro de diálogo **Administrar permiso** permite administrar los permisos de propietario y del grupo de propietarios. También permite agregar usuarios y grupos nuevos a la lista de control de acceso para la que quiere administrar los permisos.

Seleccione el campo **Agregar usuario o grupo** para agregar un usuario o grupo nuevo a la lista de control de acceso.

Escriba la entrada de Azure Active Directory (AAD) correspondiente que quiere agregar a la lista y, luego, seleccione **Agregar**.

El usuario o grupo ahora aparecerá en el campo **Usuarios y grupos:**, lo que le permitirá empezar a administrar sus permisos.

> [!NOTE]
> Se trata de un procedimiento recomendado para crear un grupo de seguridad en AAD y mantener los permisos en el grupo en lugar de usuarios individuales. Para más detalles sobre esta recomendación, así como otros procedimientos recomendados, consulte los [procedimientos recomendados para Data Lake Storage Gen2](data-lake-storage-best-practices.md).

Hay dos categorías de permisos que puede asignar: ACL de acceso y ACL predeterminadas.

* **Acceso**: Las ACL de acceso controlan el acceso a un objeto. Tanto archivos como directorios tienen ACL de acceso.

* **Predeterminadas**: Una plantilla de ACL asociada con un directorio que determina las ACL de acceso de todos los elementos secundarios que se crean en ese directorio. Los archivos no tienen ACL predeterminadas.

Dentro de ambas categorías existen tres permisos que puede asignar en los archivos o directorios: **Leer**, **Escribir** y **Ejecutar**.

>[!NOTE]
> Las selecciones que haga aquí no establecerán permisos en ningún elemento existente actual dentro del directorio. Debe ir a cada elemento individual y establecer manualmente los permisos, si el archivo ya existe.

Puede administrar los permisos en directorios individuales, así como en archivos individuales, que es lo que le permite tener un control de acceso detallado. El proceso de administrar permisos tanto para directorios como archivos es el mismo que ya se describió. Haga clic con el botón derecho en el archivo o el directorio en el que quiere administrar los permisos y siga el mismo proceso.

## <a name="next-steps"></a>Pasos siguientes

En este tema, aprendió a establecer permisos en archivos y directorios mediante **Explorador de Azure Storage**. Para más información sobre las ACL, incluidas las ACL predeterminadas, las ACL de acceso, su comportamiento y permisos correspondientes, vaya a nuestro artículo conceptual sobre el tema.

> [!div class="nextstepaction"]
> [Control de acceso en Azure Data Lake Storage Gen2](data-lake-storage-access-control.md)
