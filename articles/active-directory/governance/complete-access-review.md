---
title: 'Revisión de acceso de grupos o aplicaciones: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo realizar una revisión de acceso de los miembros del grupo o el acceso a la aplicación de revisiones de acceso de Azure Active Directory.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4265a7e08eab079e55ce91b27142ec3e55b3f3e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579604"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Revisión de acceso de los grupos o las revisiones de acceso de las aplicaciones de Azure AD

Los administradores pueden usar Azure Active Directory (Azure AD) para [crear una revisión de acceso](create-access-review.md) para miembros de grupo o usuarios asignados en una aplicación. Azure AD envía automáticamente a los revisores un correo electrónico en el que se les solicita que revisen el acceso. Si un usuario no recibió un correo electrónico, puede enviarle las instrucciones en [revisen el acceso a grupos o aplicaciones](perform-access-review.md). (Tenga en cuenta que los invitados que están asignados como revisores pero no han aceptado la invitación no recibirán un correo electrónico de las revisiones de acceso, ya que primero deben aceptar una invitación antes de la revisión). Una vez finalizado el período de revisión de acceso o si el administrador detiene la revisión de acceso, siga los pasos de este artículo para ver los resultados y aplicarlos.

## <a name="view-an-access-review-in-the-azure-portal"></a>Visualización de una revisión de acceso en Azure Portal

1. Vaya a la [página de revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), seleccione **Programas** y seleccione el programa que contenga el control de revisión de acceso.

2. Seleccione **Administrar** y seleccione el control de revisión de acceso. Si hay muchos controles en el programa, puede filtrar para los controles de un tipo específico y ordenar por estado. También puede buscar por el nombre del control de revisión de acceso o el nombre para mostrar del propietario que lo creó. 

## <a name="stop-a-review-that-hasnt-finished"></a>Detención de una revisión que aún no ha finalizado

Si la revisión no ha alcanzado la fecha final programada, un administrador puede seleccionar **Detener** para finalizar antes la revisión. Después de detener la revisión, no se podrán revisar más los usuarios. No puede reiniciar una revisión una vez detenida.

## <a name="apply-the-changes"></a>Aplicación de cambios 

Una vez finalizada una revisión de acceso, ya sea porque ha alcanzado la fecha de finalización o un administrador la detuvo manualmente y la aplicación automática no se configuró para la revisión, puede seleccionar **Aplicar** para aplicar los cambios manualmente. El resultado de la revisión se implementa actualizando el grupo o la aplicación. Si el acceso de un usuario se denegó en la revisión, cuando un administrador seleccione esta opción, Azure AD quitará la asignación de pertenencia o de la aplicación. 

Después de que haya finalizado una revisión de acceso y se haya configurado la aplicación automática, el estado de la revisión cambiará de Completado a los diferentes estados intermedios y, por último, pasará al estado Aplicada. Debería esperar ver a los usuarios denegados, si es que los hay, eliminados de la pertenencia al grupo de recursos o la asignación de aplicaciones en unos minutos.

Una revisión de aplicación automática configurada o la selección de **Aplicar** no tiene ningún efecto en un grupo que se origina en un directorio local o en un grupo dinámico. Si desea cambiar un grupo que se origina en un directorio local, descargue los resultados y aplique esos cambios a la representación del grupo en ese directorio.

## <a name="download-the-results-of-the-review"></a>Descarga de los resultados de la revisión

Para recuperar los resultados de la revisión, seleccione **Aprobaciones** y, luego, seleccione **Descargar**. El archivo CSV resultante puede verse en Excel o en otros programas que abren archivos CSV codificados en UTF-8.

## <a name="optional-delete-a-review"></a>Opcional: eliminación de una revisión
Si ya no está interesado en la revisión, puede eliminarla. Seleccione **Eliminar** para quitar la revisión de Azure AD.

> [!IMPORTANT]
> No se produce ninguna advertencia antes de la eliminación, así que asegúrese de que realmente quiere eliminar esa revisión.
> 
> 

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso de los usuarios con las revisiones de acceso de Azure AD](manage-user-access-with-access-reviews.md)
- [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md)
- [Administración de los programas y los controles para las revisiones de acceso de Azure AD](manage-programs-controls.md)
- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Creación de una revisión de acceso de los usuarios en un rol administrativo de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)
