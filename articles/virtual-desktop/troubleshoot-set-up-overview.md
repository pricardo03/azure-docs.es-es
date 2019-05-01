---
title: 'Información general, comentarios y soporte técnico: Azure de la solución de problemas de Escritorio Virtual de Windows'
description: Información general para solucionar problemas al configurar un entorno de inquilinos de Escritorio Virtual de Windows.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 8e344d6908ba19f8e2294c7777b9c1016eafaf52
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927648"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Información general de solución de problemas, comentarios y soporte técnico

En este artículo se proporciona información general de los problemas que puede surgir al configurar un entorno de inquilinos de Escritorio Virtual de Windows y proporciona maneras de resolver los problemas.

## <a name="provide-feedback"></a>Envío de comentarios

En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="escalation-tracks"></a>Realiza un seguimiento de escalado

Use la tabla siguiente para identificar y resolver los problemas que pueden producirse al configurar un entorno de inquilinos mediante el cliente de escritorio remoto.

>[!NOTE]
>En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Cada vez que nos referimos al soporte técnico de Escritorio Virtual de Windows, vaya a nuestro foro de comunidad tecnológica por ahora. Visite el [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) debatir problemas con el equipo de producto y los miembros de la Comunidad activo. Si tiene que resolver un problema de soporte técnico, incluya el identificador de actividad y el período de tiempo aproximado para cuando se produjo el problema.

| **Problema**                                                            | **Solución sugerida**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Creación de un inquilino                                                    | Si se produce una interrupción de Azure, póngase en contacto con [soporte técnico de Azure](https://azure.microsoft.com/support/options/); en caso contrario, póngase en contacto con **soporte técnico de Escritorio Virtual de Windows/servicios de escritorio remoto**.|
| Acceso a las plantillas de Marketplace en Azure portal       | Si se produce una interrupción de Azure, póngase en contacto con [soporte técnico de Azure](https://azure.microsoft.com/support/options/). <br> <br> Plantillas de Escritorio Virtual de Windows Marketplace de Azure están disponibles gratuitamente.|
| Acceso a las plantillas de Azure Resource Manager desde GitHub                                  | Consulte la sección "Host de sesión de Escritorio Virtual de Windows de crear máquinas virtuales" de [creación del grupo de inquilinos y host](troubleshoot-set-up-issues.md). Si el problema sigue sin resolverse, póngase en contacto con el [equipo de soporte técnico de GitHub](https://github.com/contact). <br> <br> Si el error se produce después de obtener acceso a la plantilla en GitHub, póngase en contacto con [soporte técnico de Azure](https://azure.microsoft.com/support/options/).|
| Configuración de ruta de Express y Azure Virtual Network (VNET) de la agrupación del host sesión               | Póngase en contacto con **soporte técnico de Azure (redes)**. |
| Grupo de host de sesión creación de máquina Virtual (VM) cuando no se usan plantillas de Azure Resource Manager proporcionadas con el escritorio Virtual de Windows | Póngase en contacto con **soporte técnico de Azure (Compute)**. <br> <br> Para problemas con las plantillas de Azure Resource Manager que se proporcionan con el escritorio Virtual de Windows, consulte la sección del inquilino de creación de Escritorio Virtual de Windows de [creación del grupo de inquilinos y host](troubleshoot-set-up-issues.md). |
| Administrar el entorno de host de sesión de Escritorio Virtual de Windows desde el portal de Azure    | Póngase en contacto con **soporte técnico de Azure**. <br> <br> Para problemas de administración cuando se usa PowerShell para el escritorio de remoto Desktop Services/Windows Virtual, consulte [Windows PowerShell de Escritorio Virtual](troubleshoot-powershell.md) o póngase en contacto con el **equipo de soporte técnico de Escritorio Virtual de Windows/servicios de escritorio remoto** . |
| Administrar la configuración del escritorio Virtual de Windows asociada a los grupos host y los grupos de aplicaciones (grupos de aplicaciones)      | Consulte [Windows PowerShell de Escritorio Virtual](troubleshoot-powershell.md), o póngase en contacto con el **equipo de soporte técnico de Escritorio Virtual de Windows/servicios de escritorio remoto**. <br> <br> Si los problemas están vinculados a la interfaz gráfica de usuario de ejemplo (GUI), llegar a la Comunidad de Yammer.|
| Error de funcionamiento de los clientes de escritorio remoto en el inicio                                                 | Consulte [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md) y si esto no resuelve el problema, póngase en contacto con **equipo de soporte técnico de Escritorio Virtual de Windows/servicios de escritorio remoto**.  <br> <br> Si es un problema de red, los usuarios necesitan ponerse en contacto con su administrador de red. |
| Está conectado pero ninguna fuente de distribución                                                                 | Solución de problemas mediante el "usuario se conecta, pero no aparece nada (ninguna fuente de distribución)" sección de [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md). <br> <br> Si los usuarios se han asignado a un grupo de aplicaciones, remitir a la **equipo de soporte técnico de Escritorio Virtual de Windows/servicios de escritorio remoto**. |
| Problemas de detección de fuentes debido a la red                                            | Los usuarios necesitan ponerse en contacto con su administrador de red. |
| Conexión de clientes                                                                    | Consulte [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md) y si no se soluciona el problema, consulte [configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md). |
| Capacidad de respuesta de las aplicaciones remotas o el escritorio                                      | Si los problemas están vinculados a una aplicación específica o un producto, póngase en contacto con el equipo responsable de ese producto. |
| Errores o mensajes de licencias                                                          | Si los problemas están vinculados a una aplicación específica o un producto, póngase en contacto con el equipo responsable de ese producto. |

## <a name="next-steps"></a>Pasos siguientes

- Para solucionar problemas durante la creación de un grupo de inquilinos y host en un entorno de Escritorio Virtual de Windows, consulte [creación del grupo de inquilinos y host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en el escritorio Virtual de Windows, consulte [configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas con conexiones de cliente de Escritorio Virtual de Windows, consulte [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md).
- Para solucionar problemas al usar PowerShell con el escritorio Virtual de Windows, consulte [Windows PowerShell de Escritorio Virtual](troubleshoot-powershell.md).
- Para obtener más información sobre el servicio en versión preliminar, consulte [entorno de versión preliminar de Windows Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de implementaciones de plantilla de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).