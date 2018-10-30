---
title: Verificación de los paquetes del fabricante de equipos originales (OEM) en la validación como servicio de Azure Stack | Microsoft Docs
description: Aprenda a verificar los paquetes del fabricante de equipos originales (OEM) con la validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: bcfc4cb65c94e34e9f6056ada53726f88489fefb
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646658"
---
# <a name="validate-oem-packages"></a>Validación de paquetes de OEM

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Puede probar un nuevo paquete de OEM cuando se ha producido un cambio en el firmware o en los controladores para una validación de solución completa. Cuando el paquete ha superado la prueba, Microsoft lo firma. La prueba debe contener el paquete de extensión de OEM actualizado con los controladores y el firmware que han superado las pruebas del logotipo de Windows Server y las pruebas PCS.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Antes de cargar o enviar paquetes, consulte [Create an OEM package](azure-stack-vaas-create-oem-package.md) (Creación de un paquete de OEM) para el formato y el contenido de paquete esperados.

## <a name="managing-packages-for-validation"></a>Administración de paquetes para la validación

Al usar el flujo de trabajo **Validación del paquete** para validar un paquete, deberá proporcionar una dirección URL a **Azure Storage Blob**. Este blob es el paquete de OEM que se instaló en la solución en el momento de la implementación. Cree el blob mediante la cuenta de almacenamiento de Azure que creó durante la instalación (consulte [Configuración de los recursos de Validación como servicio](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Requisito previo: aprovisionar un contenedor de almacenamiento

Cree un contenedor en la cuenta de almacenamiento para los blobs de paquetes. Este contenedor puede utilizarse para todas las ejecuciones de Package Validation (Validación del paquete).

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento que creó en [Configuración de los recursos de Validación como servicio](azure-stack-vaas-set-up-resources.md).
2. En la hoja izquierda de **Blob Service**, seleccione **Containers**.
3. Seleccione **+ Contenedor** en la barra de menús y proporcione un nombre al contenedor, por ejemplo, `vaaspackages`.

### <a name="upload-package-to-storage-account"></a>Carga de un paquete en una cuenta de almacenamiento

1. Prepare el paquete que desea validar. Si tiene varios archivos, comprímalos en un archivo `.zip`.
2. En [Azure Portal](https://portal.azure.com), seleccione el contenedor del paquete y cargue el paquete; para ello, seleccione **Cargar** en la barra de menús.
3. Seleccione el archivo `.zip` del paquete que se va a cargar. Mantenga los valores predeterminados para **Tipo de blob** (es decir, **Blob en bloques**) y **Tamaño de bloque**.

> [!NOTE]
> Asegúrese de que el contenido del archivo `.zip` se coloca en la raíz del archivo `.zip`. El paquete no debe tener subcarpetas.

### <a name="generate-package-blob-url-for-vaas"></a>Generación de la dirección URL del blob del paquete para VaaS

Al crear un flujo de trabajo de **Package Validation** (Validación del paquete) en el portal de VaaS, deberá proporcionar una dirección URL a la instancia de Azure Storage Blob que contiene el paquete.

#### <a name="option-1-generating-an-account-sas-url"></a>Opción 1: generación de una dirección URL de SAS para la cuenta

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Seleccione **Blob** en las opciones de **Servicios permitidos**. Anule la selección de las opciones restantes.

1. Seleccione **Contenedor** y **Objeto** en **Tipos de recursos permitidos**. Anule la selección de las opciones restantes.

1. Seleccione **Lectura** y **Lista** en **Permisos permitidos**. Anule la selección de las opciones restantes.

1. En **Hora de inicio** especifique la hora actual y en **Hora de finalización**, una hora después de la hora actual.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    El formato debe ser similar al siguiente: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Modifique la dirección URL de SAS generada para incluir el contenedor del paquete, `{containername}`, y el nombre del blob del paquete, `{mypackage.zip}`, como sigue: `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Use este valor al iniciar un nuevo flujo de trabajo **Package Validation** (Validación del paquete) en el portal de VaaS.

#### <a name="option-2-using-public-read-container"></a>Opción 2: uso de un contenedor público de lectura

> [!CAUTION]
> Esta opción abre el contenedor al acceso anónimo de solo lectura.

1. Conceda **acceso de lectura público solo para blobs** al contenedor del paquete; para ello, siga las instrucciones de la sección [Concesión de permisos a usuarios anónimos a contenedores y blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

2. En el contenedor del paquete, seleccione el blob del paquete del contenedor para abrir el panel de propiedades.

3. Copie la **dirección URL**. Use este valor al iniciar un nuevo flujo de trabajo **Package Validation** (Validación del paquete) en el portal de VaaS.

## <a name="apply-monthly-update"></a>Aplicación de actualización mensual

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Creación de un flujo de trabajo Package Validation (Validación del paquete)

1. Inicie sesión en el [portal de VaaS](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Seleccione **Iniciar** en el icono **Package Validation** (Validación del paquete).

    ![Icono de flujo de trabajo Package Validation (Validación del paquete)](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Escriba la dirección URL de Azure Storage Blob del paquete de OEM que se instaló en la solución en el momento de la implementación. Para instrucciones, consulte [Generación de la dirección URL del blob del paquete para VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > No se pueden modificar los parámetros de entorno después de crear un flujo de trabajo.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Se le redirigirá a la página de resumen de las pruebas.

## <a name="run-package-validation-tests"></a>Ejecución de pruebas de Package Validation

En la página **Package validation tests summary** (Resumen de las pruebas de Package validation) verá una lista de las pruebas necesarias para realizar la validación. Las pruebas de este flujo de trabajo se ejecutan durante aproximadamente 24 horas.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

Cuando todas las pruebas finalicen correctamente, envíe el nombre de la solución de VaaS y la validación del paquete a [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) para solicitar la firma del paquete.

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y administración de pruebas en el portal de VaaS](azure-stack-vaas-monitor-test.md)