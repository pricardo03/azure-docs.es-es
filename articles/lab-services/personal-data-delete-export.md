---
title: Exportación y eliminación de datos personales de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo eliminar y exportar datos personales desde el servicio de Azure DevLast Labs para cumplir las obligaciones contraídas en virtud del Reglamento general de protección de datos (RGPD).
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: spelluru
ms.openlocfilehash: e681652c13e521bd33524e247db65088f47a794c
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51637145"
---
# <a name="export-or-delete-personal-data-from-azure-devtest-labs"></a>Exportación y eliminación de datos personales de Azure DevTest Labs
En este artículo se proporcionan los pasos para eliminar y exportar datos personales desde el servicio de Azure DevTest Labs. 

## <a name="what-personal-data-does-devtest-labs-collect"></a>¿Qué datos personales recopila DevTest Labs?
DevTest Labs recopila dos elementos principales de los datos personales del usuario: la dirección de correo electrónico del usuario y el id. de objeto de usuario. Esta información es fundamental para que el servicio proporcione características en servicio a los administradores y usuarios del laboratorio.

### <a name="user-email-address"></a>Dirección de correo electrónico del usuario
DevTest Labs usa la dirección de correo electrónico del usuario para enviar las notificaciones de correo electrónico de apagado automático a los usuarios del laboratorio. El correo electrónico notifica a los usuarios que la máquina se va a apagar. Si quieren, los usuarios pueden posponer el apagado u omitirlo. Configure la dirección de correo electrónico en el nivel de laboratorio o en el nivel de la VM.

**Configuración del correo electrónico en el laboratorio:**

![Configuración del correo electrónico en el nivel del laboratorio](./media/personal-data-delete-export/lab-user-email.png)

**Configuración del correo electrónico en la VM:**

![Configuración del correo electrónico en el nivel de la VM](./media/personal-data-delete-export/vm-user-email.png)

### <a name="user-object-id"></a>Id. de objeto de usuario
DevTest Labs usa el id. de objeto de usuario para mostrar las tendencias de costo de mes a mes y el costo por información de recursos para los administradores del laboratorio. Les permite realizar un seguimiento de los costos y administrar los umbrales de su laboratorio. 

**Tendencia de costo estimado para el mes en curso:**
![Tendencia de costo estimado para el mes en curso](./media/personal-data-delete-export/estimated-cost-trend-per-month.png)

**Costo estimado de valor mensual hasta la fecha por recurso:**
![Costo estimado de valor mensual hasta la fecha por recurso](./media/personal-data-delete-export/estimated-month-to-date-cost-by-resource.png)


## <a name="why-do-we-need-this-personal-data"></a>¿Por qué necesitamos estos datos personales?
El servicio de DevTest Labs usa los datos personales con fines operativos. Estos datos son fundamentales para que el servicio ofrezca las características clave. Si establece una directiva de retención en la dirección de correo electrónico del usuario, los usuarios del laboratorio no recibirán notificaciones oportunas de correo electrónico de apagado automático tras eliminar su dirección de correo electrónico de nuestro sistema. De forma similar, el administrador del laboratorio no podrá ver las tendencias de costos de mes a mes ni el costo por recurso para las máquinas de sus laboratorios si los id. de objeto de usuario se eliminan en función de una directiva de retención. Por lo tanto, estos datos deben conservarse mientras el recurso del usuario esté activo en el laboratorio.

## <a name="how-can-i-have-the-system-to-forget-my-personal-data"></a>¿Cómo puedo hacer que el sistema olvide mis datos personales?
Como usuario del laboratorio, si quiere que estos datos personales se eliminen, puede hacerlo mediante la eliminación del recurso correspondiente en el laboratorio. El servicio de DevTest Labs anonimiza los datos personales eliminados 30 después de que el usuario los elimine.

Por ejemplo, si elimina la VM o quita la dirección de correo electrónico, el servicio de DevTest Labs anonimiza estos datos 30 días después de eliminar el recurso. La directiva de retención de 30 días tras la eliminación sirve para garantizar que se proporciona una proyección de costos precisa de mes a mes para el administrador de laboratorio.

## <a name="how-can-i-request-an-export-on-my-personal-data"></a>¿Cómo puedo solicitar la exportación de mis datos personales?
Como usuario del laboratorio, puede solicitar una exportación de los datos personales que almacena el servicio de DevTest Labs. Para solicitar una exportación, vaya a la opción **Datos personales** en la página **Introducción** del laboratorio. Al seleccionar el botón **Solicitar exportación** se inicia la creación de un archivo de Excel descargable en la cuenta de almacenamiento del administrador del laboratorio. A continuación, puede ponerse en contacto con él para ver los datos.

1. Seleccione **Datos personales** en el menú de la izquierda. 

    ![Página de datos personales](./media/personal-data-delete-export/personal-data-page.png)
2. Seleccione el **grupo de recursos** que contiene el laboratorio.

    ![Selección de un grupo de recursos](./media/personal-data-delete-export/select-resource-group.png)
3. Seleccione la **cuenta de almacenamiento** en el grupo de recursos.
4. En la página **Cuenta de almacenamiento**, seleccione **Blobs**.

    ![Selección del icono de blobs](./media/personal-data-delete-export/select-blobs-tile.png)
5. Seleccione el contenedor denominado **labresourceusage** en la lista de contenedores.

    ![Selección del contenedor del blob](./media/personal-data-delete-export/select-blob-container.png)
6. Seleccione la **carpeta** que nombró tras el laboratorio. Encontrará archivos **CSV** para **discos** y **máquinas virtuales** del laboratorio en esta carpeta. Puede descargar estos archivos CSV, filtrar el contenido para el usuario del laboratorio que solicita un acceso y compartirla con él.

    ![Descargar archivo CSV](./media/personal-data-delete-export/download-csv-file.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- [Set policies for a lab](devtest-lab-get-started-with-lab-policies.md) (Definición de directivas para un laboratorio)
- [Preguntas más frecuentes](devtest-lab-faq.md)
