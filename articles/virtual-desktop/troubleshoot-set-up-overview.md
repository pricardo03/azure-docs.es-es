---
title: Información general, comentarios y soporte técnico para la solución de problemas de Windows Virtual Desktop (Azure)
description: Información general para solucionar problemas al configurar un entorno de inquilinos de Windows Virtual Desktop.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 43f940c3c5b0fcf06fb5aa9be268f85db19fc8a6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077662"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Información general sobre solución de problemas, comentarios y soporte técnico

En este artículo se proporciona información general de los problemas que pueden surgir al configurar un entorno de inquilinos de Windows Virtual Desktop y proporciona distintas maneras de resolver los problemas.

## <a name="provide-feedback"></a>Envío de comentarios

En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="escalation-tracks"></a>Seguimiento del escalado

Use la siguiente tabla para identificar y resolver los problemas que puedan surgir al configurar un entorno de inquilinos mediante el cliente de Escritorio remoto. Una vez que el inquilino esté configurado, puede usar el nuevo [servicio Diagnostics](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-role-service) para identificar los problemas en escenarios comunes.

>[!NOTE]
>En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Cada vez que hagamos referencia al soporte técnico de Windows Virtual Desktop, vaya al foro de Tech Community. Visite [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre los problemas con el equipo del producto y con miembros activos de la comunidad. Si tiene que resolver un problema de soporte técnico, incluya el identificador de actividad y el período de tiempo aproximado durante el que se produjo el problema.

| **Problema**                                                            | **Solución propuesta**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Creación de un inquilino                                                    | Si se produce una interrupción en Azure, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/); en el resto de los casos, póngase en contacto con el **soporte técnico de Servicios de Escritorio remoto/Windows Virtual Desktop**.|
| Acceso a las plantillas de Marketplace desde Azure Portal       | Si se produce una interrupción de Azure, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/). <br> <br> Las plantillas de Windows Virtual Desktop de Azure Marketplace están disponibles de forma gratuita.|
| Acceso a las plantillas de Azure Resource Manager desde GitHub                                  | Consulte la sección "Creación de máquinas virtuales de host de sesión de Windows Virtual Desktop" de [Creación del grupo de inquilinos y del host](troubleshoot-set-up-issues.md). Si aun así el problema no se resuelve, póngase en contacto con el [equipo de soporte técnico de GitHub](https://github.com/contact). <br> <br> Si el error se produce después de acceder a la plantilla en GitHub, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/).|
| Configuración de Express Route y de Azure Virtual Network (VNET) en el grupo del host de sesión               | Póngase en contacto con el **soporte técnico de Azure (redes)** . |
| Creación de una máquina virtual en el grupo del host de sesión cuando las plantilla de Azure Resource Manager que se proporcionan con Windows Virtual Desktop no se usan | Póngase en contacto con el **soporte técnico de Azure (Compute)** . <br> <br> En caso de problemas con las plantillas de Azure Resource Manager que se proporcionan con Windows Virtual Desktop, consulte la sección Crear un inquilino de Windows Virtual Desktop de [Creación de los grupos de inquilinos y de host](troubleshoot-set-up-issues.md). |
| Administración del entorno del host de sesión de Windows Virtual Desktop desde Azure Portal    | Póngase en contacto con el **soporte técnico de Azure**. <br> <br> En caso de problemas de administración cuando se usa PowerShell para Windows Virtual Desktop/Servicios de Escritorio remoto, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md) o póngase en contacto con el **equipo de soporte técnico de Servicios de Escritorio remoto/Windows Virtual Desktop**. |
| Administración de la configuración de Windows Virtual Desktop asociada a los grupos de hosts y a los grupos de aplicaciones      | Consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md), o póngase en contacto con el **equipo de soporte técnico de Servicios de Escritorio remoto/Windows Virtual Desktop**. <br> <br> Si los problemas están asociados a la interfaz gráfica de usuario (GUI) de ejemplo, diríjase a la comunidad de Yammer.|
| Error de funcionamiento de los clientes de Escritorio remoto en el inicio                                                 | Consulte [Conexiones de cliente de Escritorio remoto](troubleshoot-client-connection.md), pero si esto no resuelve el problema, póngase en contacto con el **equipo de soporte técnico de Servicios de Escritorio remoto/Windows Virtual Desktop**.  <br> <br> Si es un problema de red, los usuarios deben ponerse en contacto con su administrador de red. |
| Está conectado, pero no hay ninguna fuente                                                                 | Para solucionar el problema, consulte la sección "El usuario se conecta, pero no se muestra nada (ninguna fuente)" de [Conexiones de cliente de Escritorio remoto](troubleshoot-client-connection.md). <br> <br> Si los usuarios se han asignado a un grupo de aplicaciones, escale el problema al **equipo de soporte técnico de Servicios de Escritorio remoto/Windows Virtual Desktop**. |
| Problemas de detección de fuentes debido a la red                                            | Los usuarios deben ponerse en contacto con su administrador de red. |
| Conexión de clientes                                                                    | Consulte [Conexiones de cliente de Escritorio remoto](troubleshoot-client-connection.md) y si no se soluciona el problema, consulte [Configuración de máquina virtual de host de sesión](troubleshoot-vm-configuration.md). |
| Capacidad de respuesta del escritorio o de las aplicaciones remotas                                      | Si los problemas están asociados a una aplicación o producto específicos, póngase en contacto con el equipo responsable de dicho producto. |
| Errores o mensajes de las licencias                                                          | Si los problemas están asociados a una aplicación o producto específicos, póngase en contacto con el equipo responsable de dicho producto. |

## <a name="next-steps"></a>Pasos siguientes

- Para solucionar problemas durante la creación de un grupo de inquilinos y de hosts en un entorno de Windows Virtual Desktop, consulte [Creación de los grupos de inquilinos y de host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas con conexiones de cliente de Windows Virtual Desktop, consulte [Conexiones de cliente de Escritorio remoto](troubleshoot-client-connection.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para más información acerca del servicio en versión preliminar, consulte [Entorno de versión preliminar de Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).