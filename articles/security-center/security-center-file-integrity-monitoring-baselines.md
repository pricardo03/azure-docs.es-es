---
title: Comparar instantáneas con la supervisión de integridad de los archivos en Azure Security Center | Microsoft Docs
description: Obtenga información sobre cómo comparar las instantáneas con la supervisión de integridad de los archivos en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c8a2a589-b737-46c1-b508-7ea52e301e8f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: e403a9bd4d3f8668544dab1d81e9052b37839bef
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358444"
---
# <a name="compare-baselines-using-file-integrity-monitoring-fim"></a>Comparar instantáneas mediante la supervisión de integridad de archivos (FIM)

Supervisión de integridad de archivos (FIM) le informa cuando se producen cambios a zonas sensibles en los recursos, para que pueda investigar y resolver actividades no autorizadas. FIM supervisa los archivos de Windows, registros de Windows y los archivos de Linux.

Este tema explica cómo habilitar FIM en los archivos y los registros. Para obtener más información acerca de FIM, vea [supervisión de integridad de los archivos en Azure Security Center](security-center-file-integrity-monitoring.md).

## <a name="why-use-fim"></a>¿Por qué usar FIM?

Sistema operativo, aplicaciones y configuraciones asociadas controlan el estado de seguridad y el comportamiento de los recursos. Por lo tanto, son el objetivo de los archivos que controlan los recursos, para poder conquistar el sistema operativo de un recurso o ejecutar actividades sin ser detectado.

De hecho, muchos estándares de cumplimiento de normas como PCI-DSS e ISO 17799 requieren la implementación de controles FIM.  

## <a name="enable-built-in-recursive-registry-checks"></a>Habilitar comprobaciones de registro integrados recursiva

Los valores predeterminados FIM registro hive proporcionan una manera cómoda para supervisar los cambios de recursiva dentro de las áreas de seguridad comunes.  Por ejemplo, un adversario puede configurar una secuencia de comandos para ejecutar en el contexto LOCAL_SYSTEM mediante la configuración de una ejecución en el inicio o apagado.  Para supervisar los cambios de este tipo, habilite la comprobación integrada.  

![Registro](./media/security-center-file-integrity-monitoring-baselines/baselines-registry.png)

>[!NOTE]
> Comprobaciones recursivas se aplican solo a las secciones de seguridad recomendada y no a las rutas de acceso del registro personalizada.  

## <a name="adding-a-custom-registry-check"></a>Agregar una comprobación del registro personalizado

Líneas de base de FIM comience por identificar las características de un estado válido conocido para el sistema operativo y que admiten la aplicación.  En este ejemplo, nos centraremos en las configuraciones de directiva de contraseña para Windows Server 2008 y versiones posteriores.


|Nombre de la directiva                 | Configuración del registro|
|---------------------------------------|-------------|
|Controlador de dominio: Rechazar cambios de contraseña de cuenta de equipo| MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RefusePasswordChange|
|Miembro de dominio: Cifrar digitalmente o firmar los datos de canal seguro (siempre)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireSignOrSeal|
|Miembro de dominio: Cifrar digitalmente datos de canal seguro (cuando sea posible)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\SealSecureChannel|
|Miembro de dominio: Firmar digitalmente datos (cuando sea posible) de un canal seguro|MACHINE\System\CurrentControlSet\Services   \Netlogon\Parameters\SignSecureChannel|
|Miembro de dominio: Deshabilitar cambios de contraseña de cuenta de equipo|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DisablePasswordChange|
|Miembro de dominio: Duración de contraseña de cuenta de equipo máximo|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\MaximumPasswordAge|
|Miembro de dominio: Requerir clave de sesión segura (Windows 2000 o posterior)|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RequireStrongKey|
|Seguridad de red: Restringir NTLM:  Autenticación NTLM en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\RestrictNTLMInDomain|
|Seguridad de red: Restringir NTLM: Agregar excepciones de servidor en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\DCAllowedNTLMServers|
|Seguridad de red: Restringir NTLM: Auditar la autenticación NTLM en este dominio|MACHINE\System\CurrentControlSet\Services  \Netlogon\Parameters\AuditNTLMInDomain|

> [!NOTE]
> Para obtener más información acerca de la configuración del registro compatible con distintas versiones del sistema operativo, consulte el [hoja de cálculo de referencia de configuración de directiva de grupo](https://www.microsoft.com/en-us/download/confirmation.aspx?id=25250).

*Para configurar FIM para supervisar las líneas de base del registro:*

1. En el **agregar registro de Windows para Change Tracking** ventana, en el **clave del registro de Windows** texto, escriba la clave del registro.

    <code>

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters
    </code>

      ![Habilitar FIM en un registro](./media/security-center-file-integrity-monitoring-baselines/baselines-add-registry.png)

## <a name="tracking-changes-to-windows-files"></a>Seguimiento de cambios en los archivos de Windows

1. En el **Add Windows File para Change Tracking** ventana, en el **ENTRAR ruta** texto, escriba la carpeta que contiene los archivos que desea realizar un seguimiento. En el ejemplo en la ilustración siguiente, **aplicación Web de Contoso** reside en el D:\ unidad dentro de la **ContosWebApp** estructura de carpetas.  
1. Crear una entrada de archivo de Windows personalizada que proporciona un nombre de la clase setting, habilitando la recursividad y especificar la carpeta principales con un sufijo de carácter comodín (*).

    ![Habilitar FIM en un archivo](./media/security-center-file-integrity-monitoring-baselines/baselines-add-file.png)

## <a name="retrieving-change-data"></a>Recuperación de datos modificados

Supervisión de integridad de los datos se encuentran en Azure Log Analytics los archivos / tabla ConfigurationChange establecida.  

 1. Establezca un intervalo de tiempo para recuperar un resumen de cambios por recurso.
En el ejemplo siguiente, vamos a recuperar todos los cambios en los últimos catorce días en las categorías de registro y archivos:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry', 'Files')

    > | summarize count() by Computer, ConfigChangeType

    </code>

1. Para ver los detalles de los cambios del registro:

    1. Quitar **archivos** desde el **donde** cláusula, 
    1. Quite la línea de resumen y reemplazarlo con una cláusula de ordenación:

    <code>

    > ConfigurationChange

    > | where TimeGenerated > ago(14d)

    > | where ConfigChangeType in ('Registry')

    > | order by Computer, RegistryKey

    </code>

Los informes se pueden exportar a CSV para archivado o canalizada a un informe de Power BI.  

![Datos FIM](./media/security-center-file-integrity-monitoring-baselines/baselines-data.png)