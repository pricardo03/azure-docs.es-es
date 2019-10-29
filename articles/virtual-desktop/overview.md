---
title: ¿Qué es Windows Virtual Desktop?  - Azure
description: Una información general de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 08/07/2019
ms.author: helohr
ms.openlocfilehash: e1ae0501e2a558967b7d53229dc629e035c5e067
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597900"
---
# <a name="what-is-windows-virtual-desktop"></a>¿Qué es Windows Virtual Desktop? 

Windows Virtual Desktop es un servicio de virtualización de escritorio y de aplicaciones que se ejecuta en la nube.

A continuación se indica lo que puede hacer al ejecutar Windows Virtual Desktop en Azure:

* Configurar una implementación de Windows 10 de sesión múltiple que ofrece toda la funcionalidad de Windows 10 con escalabilidad
* Virtualizar Office 365 ProPlus y optimizarlo para ejecutarse en escenarios virtuales multiusuario
* Proporcionar escritorios virtuales de Windows 7 con actualizaciones gratuitas de seguridad ampliada
* Traer sus aplicaciones y escritorios existentes de Servicios de Escritorio remoto (RDS) y Windows Server a cualquier equipo
* Virtualizar escritorios y aplicaciones
* Administrar escritorios y aplicaciones de Windows 10, Windows Server y Windows 7 con una experiencia de administración unificada

## <a name="introductory-video"></a>Vídeo de introducción

En este vídeo, obtendrá información sobre Windows Virtual Desktop, y conocerá por qué es único y cuáles son las novedades:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Vea más vídeos sobre Windows Virtual Desktop en [nuestra lista de reproducción](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>Principales capacidades

Con Windows Virtual Desktop, puede configurar un entorno flexible y escalable:

* Cree un entorno de virtualización de escritorio completo en su suscripción de Azure sin tener que ejecutar ningún servidor de puerta de enlace adicional.
* Publique todos los grupos host que necesite para dar cabida a diversas cargas de trabajo.
* Aporte su propia imagen para las cargas de trabajo de producción o de prueba desde la Galería de Azure.
* Reduzca los costos con recursos de sesión múltiple agrupados. Con la nueva funcionalidad de sesión múltiple de Windows 10 Enterprise exclusiva de Windows Virtual Desktop y del rol de host de sesión de Escritorio remoto (RDSH) de Windows Server, puede reducir considerablemente el número de máquinas virtuales y la sobrecarga del sistema operativo (SO), mientras se proporcionan los mismos recursos a los usuarios.
* Asigne propiedad individual mediante escritorios personales (permanentes).

Puede implementar y administrar escritorios virtuales:

* Use las interfaces de PowerShell y REST de Windows Virtual Desktop para configurar los grupos host, crear grupos de aplicaciones, asignar usuarios y publicar recursos.
* Publique aplicaciones de escritorio completo o aplicaciones remotas individuales desde un único grupo host, cree grupos de aplicaciones individuales para distintos conjuntos de usuarios o incluso asigne usuarios a varios grupos de aplicaciones para reducir el número de imágenes.
* A medida que administre su entorno, usará el acceso delegado integrado para asignar roles y recopilar diagnósticos con el fin de comprender diversos errores de configuración o de los usuarios.
* Use el nuevo servicio de diagnóstico para solucionar los errores.
* Administre únicamente la imagen y las máquinas virtuales y olvídese de la infraestructura. No es necesario que administre personalmente los roles de Escritorio remoto, como hace con los Servicios de Escritorio remoto, sino solo las máquinas virtuales de su suscripción de Azure.

También puede asignar y conectar a los usuarios a los escritorios virtuales:

* Una vez asignados, los usuarios pueden iniciar cualquier cliente de Windows Virtual Desktop para conectar los usuarios a sus aplicaciones y escritorios de Windows publicados. Conéctese desde cualquier dispositivo mediante una aplicación nativa de su dispositivo o por medio del cliente web HTML5 de Windows Virtual Desktop.
* Establezca de forma segura los usuarios mediante conexiones inversas al servicio, así nunca tendrá que dejar los puertos de entrada abiertos.

## <a name="requirements"></a>Requisitos

Es necesario cumplir una serie de requisitos para configurar Windows Virtual Desktop y conectar correctamente sus usuarios a los escritorios y aplicaciones de Windows.

Está previsto agregar compatibilidad con los siguientes sistemas operativos, así que asegúrese de que dispone de las [licencias adecuadas](https://azure.microsoft.com/pricing/details/virtual-desktop/) para los usuarios según el escritorio y las aplicaciones que tenga pensado implementar:

|OS|Licencia necesaria|
|---|---|
|Sesión múltiple de Windows 10 Enterprise o Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Licencia de acceso de cliente (CAL) de RDS con Software Assurance|

Su infraestructura necesita cumplir los siguientes requisitos para ser compatible con Windows Virtual Desktop:

* Una instancia de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).
* Una instancia de Windows Server Active Directory sincronizada con Azure Active Directory. Para habilitar dicha instancia, use:
  * Azure AD Connect
  * Azure AD Domain Services
     >[!NOTE]
     >Si usa Azure AD Domain Services, el origen de los usuarios debe ser Azure Active Directory. En este momento no se admite el uso de Azure AD Domain Services con usuarios cuyo origen sea Windows Server AD.
* Una suscripción a Azure, que contenga una red virtual que conste de la instancia de Windows Server Active Directory o esté conectada a ella.
  
Las máquinas virtuales de Azure que cree para Windows Virtual Desktop deben cumplir estos requisitos:

* Estar [unidas a un dominio estándar](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) o a un [dominio híbrido](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). Las máquinas virtuales no pueden estar unidas a Azure AD.
* Deben ejecutar una de las siguientes [imágenes de sistema operativo admitidas](#supported-virtual-machine-os-images).

>[!NOTE]
>Si necesita una suscripción a Azure, puede [registrarse para obtener una evaluación gratuita por un mes](https://azure.microsoft.com/free/). Si usa la versión de evaluación gratuita de Azure, debe utilizar Azure AD Domain Services para mantener sincronizada su instancia de Windows Server Active Directory con Azure Active Directory.

Las máquinas virtuales de Azure que cree para Windows Virtual Desktop deben tener acceso a TCP 443 de salida a las siguientes direcciones URL:

* *.wvd.microsoft.com
* \* .blob.core.windows.net
* *.core.windows.net
* *.servicebus.windows.net
* prod.warmpath.msftcloudes.com
* catalogartifact.azureedge.net

>[!NOTE]
>Es esencial abrir estas direcciones URL para una implementación confiable de Windows Virtual Desktop. No se admite el bloqueo del acceso a estas direcciones URL y afectará a la funcionalidad del servicio. Estas direcciones URL solo se corresponden con sitios y recursos de Windows Virtual Desktop y no incluyen direcciones URL para otros servicios como Azure AD.

Windows Virtual Desktop consta de los escritorios y las aplicaciones de Windows que entrega a los usuarios y de la solución de administración, que Microsoft hospeda en Azure como un servicio. Se pueden implementar escritorios y aplicaciones en máquinas virtuales de cualquier región de Azure, y la solución de administración y los datos de estas máquinas virtuales residirán en Estados Unidos (región Este de EE. UU. 2). Esto puede dar lugar a la transferencia de datos a Estados Unidos.

Para obtener un rendimiento óptimo, asegúrese de que la red cumple los requisitos siguientes:

* La latencia de ida y vuelta (RTT) desde la red del cliente hasta la región de Azure donde se han implementado grupos host debe ser inferior a 150 ms.
* El tráfico de red puede fluir fuera de las fronteras del país o la región si las máquinas virtuales que hospedan los escritorios y aplicaciones se conectan al servicio de administración.
* Para optimizar el rendimiento de la red, se recomienda que las máquinas virtuales del host de sesión se coloquen en la misma región de Azure que el servicio de administración.

## <a name="supported-remote-desktop-clients"></a>Clientes compatibles de Escritorio remoto

Los clientes de Escritorio remoto siguientes admiten Windows Virtual Desktop:

* [Windows](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)
* [HTML5](https://docs.microsoft.com/azure/virtual-desktop/connect-web)

## <a name="supported-virtual-machine-os-images"></a>Imágenes de SO de máquinas virtuales admitidas

Windows Virtual Desktop admite las imágenes de SO siguiente:

* Sesión múltiple de Windows 10 Enterprise
* Windows 10 Enterprise
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Las opciones de automatización y de implementación disponibles dependen del sistema operativo y la versión que elija, tal como se muestra en la tabla siguiente: 

|Sistema operativo|Galería de imágenes de Azure|Implementación manual de la máquina virtual|Integración de la plantilla de Azure Resource Manager|Aprovisionamiento de grupos host en Azure Marketplace|Actualizaciones del agente de Windows Virtual Desktop|
|--------------------------------------|:------:|:------:|:------:|:------:|:------:|
|Sesión múltiple de Windows 10, versión 1903|Sí|Sí|Sí|Sí|Automático|
|Sesión múltiple de Windows 10, versión 1809|Sí|Sí|No|Sin|Automático|
|Windows 10 Enterprise, versión 1903|Sí|Sí|Sí|Sí|Automático|
|Windows 10 Enterprise, versión 1809|Sí|Sí|No|Sin|Automático|
|Windows 7 Enterprise|Sí|Sí|No|Sin|Manual|
|Windows Server 2019|Sí|Sí|No|Sin|Automático|
|Windows Server 2016|Sí|Sí|Sí|Sí|Automático|
|Windows Server 2012 R2|Sí|Sí|No|Sin|Automático|

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, deberá crear un inquilino. Para más información sobre cómo crear un inquilino, siga con el tutorial de creación de inquilinos.

> [!div class="nextstepaction"]
> [Creación de un inquilino en Windows Virtual Desktop](tenant-setup-azure-active-directory.md)
