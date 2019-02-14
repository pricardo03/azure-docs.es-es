---
title: Azure Log Integration con registros de auditoría de Azure Active Directory | Microsoft Docs
description: Aquí aprenderá cómo instalar el servicio Azure Log Integration y cómo integrar los registros desde registros de auditoría de Azure
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 473f9db7eb507fa910657696590bb656a8aff52f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109884"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integración de los registros de auditoría de Azure Active Directory

Los eventos de auditoría de Azure Active Directory (Azure AD) le ayudan a identificar las acciones con privilegios que se produjeron en Azure Active Directory. Puede ver los tipos de eventos de los que puede realizar un seguimiento mediante la revisión de [Eventos del informe de auditoría de Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md).


>[!IMPORTANT]
> La característica Azure Log Integration dejará de utilizarse el 01/06/2019. Las descargas de AzLog se deshabilitaron el 27 de junio de 2018. Para obtener orientación sobre cómo avanzar, consulte el artículo [Use Azure monitor to integrate with SIEM tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) (Uso de Azure Monitor para realizar la integración con herramientas SIEM) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Pasos para la integración de los registros de auditoría de Azure Active Directory

> [!NOTE]
> Debe revisar el artículo [Introducción](security-azure-log-integration-get-started.md) y completar los pasos allí antes de intentar seguir los pasos pertinentes descritos en este artículo.

1. Abra el símbolo del sistema y ejecute este comando:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Ejecute este comando: 
 
   ``azlog createazureid``

   Este comando solicita el inicio de sesión de Azure. A continuación, el comando crea una entidad de servicio de Azure Active Directory en los inquilinos de Azure AD que hospedan las suscripciones de Azure en las que el usuario inició sesión como administrador, coadministrador o propietario. El comando producirá un error si el usuario ha iniciado sesión solo como usuario invitado en el inquilino de Azure AD. La autenticación en Azure se hace con Azure AD. La creación de una entidad de servicio para Azure Log Integration creará la identidad de Azure AD a la que se otorgará acceso de lectura de las suscripciones de Azure.

3. Ejecute el siguiente comando para proporcionar el identificador del inquilino. Debe ser miembro del rol de administrador de inquilinos para ejecutar el comando.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Ejemplo:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Compruebe las siguientes carpetas para confirmar que se han creado los archivos JSON de registro de auditoría de Azure Active Directory en las siguientes rutas:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

En el siguiente vídeo se muestran los pasos que se describen en este artículo:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Para obtener instrucciones específicas sobre cómo llevar la información de los archivos JSON al sistema de información de seguridad y administración de eventos (SIEM), póngase en contacto con el proveedor SIEM.

Puede encontrar ayuda de la comunidad en [Foro MSDN de Azure Log Integration](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Este foro permite a los usuarios de la comunidad de Azure Log Integration ayudarse unos a otros con preguntas, respuestas, consejos y sugerencias. Además, el equipo de Azure Log Integration supervisa este foro y le ayudará siempre que sea posible.

También puede abrir una [solicitud de soporte técnico](../azure-supportability/how-to-create-azure-support-request.md). Seleccione **Azure Log Integration** como el servicio para el que está solicitando el soporte técnico.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre Azure Log Integration, consulte:

* [Azure Log Integration para registros de Azure](https://www.microsoft.com/download/details.aspx?id=53324): Esta página del Centro de descargas proporciona información, requisitos del sistema e instrucciones de instalación de Azure Log Integration.
* [Introducción a Azure Log Integration](security-azure-log-integration-overview.md): Este artículo es una introducción a Azure Log Integration, sus principales funcionalidades y cómo funciona.
* [Preguntas más frecuentes sobre Azure Log Integration](security-azure-log-integration-faq.md): Este artículo responde a las preguntas sobre Azure Log Integration.
* [New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Nuevas características de Azure Diagnostics y registros de auditoría de Azure): Esta entrada de blog es una introducción a los registros de auditoría de Azure y a otras características que le ayudarán a obtener información sobre las operaciones de los recursos de Azure.
