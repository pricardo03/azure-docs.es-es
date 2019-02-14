---
title: Completar una revisión de acceso para los roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a completar una revisión de acceso para los roles de directorio de Azure AD en Azure AD Privileged Identity Management y a ver los resultados.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bea4f649b40298de15b8352fc8eb522da87dead5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190354"
---
# <a name="complete-an-access-review-for-azure-ad-directory-roles-in-pim"></a>Completar una revisión de acceso para los roles de directorio de Azure AD en PIM
Los administradores de roles con privilegios pueden revisar el acceso con privilegios una vez que se [ha iniciado una revisión del acceso](pim-how-to-start-security-review.md). Privileged Identity Management (PIM) de Azure AD enviará automáticamente un correo electrónico para pedir a los usuarios que revisen su acceso. Si algún usuario no ha recibido un correo electrónico, puede enviarle las instrucciones necesarias para [realizar una revisión del acceso](pim-how-to-perform-security-review.md).

Cuando acabe el período de la revisión del acceso o cuando todos los usuarios hayan finalizado su autorrevisión, siga los pasos de este artículo para administrar la revisión y ver los resultados.

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso
1. Vaya al [Portal de Azure](https://portal.azure.com/) y seleccione la aplicación **Azure AD Privileged Identity Management** en el panel.
2. Seleccione la sección **Revisiones de acceso** del panel.
3. Seleccione la revisión de acceso que quiere administrar.

En la hoja de detalles de la revisión del acceso hay varias opciones para administrar dicha revisión.

![Botones de revisión de acceso de PIM: captura de pantalla](./media/pim-how-to-complete-review/PIM_review_buttons.png)

### <a name="remind"></a>Recuerde
Si una revisión de acceso está configurada para que los usuarios revisen por sí mismos, el botón **Recordar** envía una notificación. 

### <a name="stop"></a>Stop
Todas las revisiones de acceso tienen una fecha de finalización, pero puede usar el botón **Detener** para terminarlas antes. Si para entonces algunos de los usuarios no se han revisado, no podrán hacerlo una vez que se haya detenido la revisión. No se puede reiniciar una revisión después de que se ha detenido.

### <a name="apply"></a>Aplicar
Después de que finaliza una revisión de acceso, bien porque se ha llegado a la fecha de finalización o porque se ha detenido manualmente, el botón **Aplicar** implementa el resultado de la revisión. Si se denegó el acceso de un usuario en la revisión, este es el paso que se quitará su asignación de rol.  

### <a name="export"></a>Exportación
Si desea aplicar los resultados de la revisión del acceso manualmente, puede exportar la revisión. El botón **Exportar** iniciará la descarga de un archivo CSV. Puede administrar los resultados en Excel u otros programas que abran archivos CSV.

### <a name="delete"></a>Eliminar
Si ya no le interesa más la revisión, elimínela. El botón **Eliminar** quita la revisión de la aplicación PIM.

> [!IMPORTANT]
> No recibirá ninguna advertencia antes de la eliminación, así que asegúrese de que realmente quiere eliminar esa revisión. 

## <a name="next-steps"></a>Pasos siguientes

- [Iniciar una revisión de acceso para los roles de directorio de Azure AD en PIM](pim-how-to-start-security-review.md)
- [Realización de una revisión de acceso para los roles de directorio de Azure AD en PIM](pim-how-to-perform-security-review.md)
