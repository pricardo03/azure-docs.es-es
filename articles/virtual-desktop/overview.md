---
title: ¿Qué es la versión preliminar de Windows Virtual Desktop?  - Azure
description: Información general de la versión preliminar de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 6385838064c408ccfa23dacbd5785f8e82f3cc8b
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59049445"
---
# <a name="what-is-windows-virtual-desktop-preview"></a>¿Qué es la versión preliminar de Windows Virtual Desktop? 

La versión preliminar de Windows Virtual Desktop, ahora disponible públicamente, es un servicio de escritorio y de virtualización de aplicaciones que se ejecuta en la nube.

A continuación se indica lo que puede hacer al ejecutar Windows Virtual Desktop en Azure:

* Configurar una implementación de Windows 10 de sesión múltiple que ofrece toda la funcionalidad de Windows 10 con escalabilidad
* Virtualizar Office 365 ProPlus y optimizarlo para ejecutarse en escenarios virtuales multiusuario
* Proporcionar escritorios virtuales de Windows 7 con actualizaciones gratuitas de seguridad ampliada
* Traer sus aplicaciones y escritorios existentes de Servicios de Escritorio remoto (RDS) y Windows Server a cualquier equipo
* Virtualizar escritorios y aplicaciones
* Administrar escritorios y aplicaciones de Windows 10, Windows Server y Windows 7 con una experiencia de administración unificada

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

En primer lugar, asegúrese de que dispone de las [licencias adecuadas](https://azure.microsoft.com/pricing/details/virtual-desktop/) para los usuarios según el escritorio y las aplicaciones que tenga pensado implementar:

|SO|Licencia necesaria|
|---|---|
|Sesión múltiple de Windows 10 Enterprise o sesión única de Windows 10|Microsoft 365 E3, E5, A3, A5, Business, F1<br>Windows E3, E5, A3, A5|
|Windows 7|Microsoft 365 E3, E5, A3, A5, Business, F1<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Licencia de acceso de cliente (CAL) de RDS con Software Assurance|

Su infraestructura necesita cumplir los siguientes requisitos para ser compatible con Windows Virtual Desktop:

* Una instancia de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).
* Una instancia de Windows Server Active Directory sincronizada con Azure Active Directory. Para habilitar dicha instancia, use:
  * Azure AD Connect
  * Azure AD Domain Services
* Una suscripción a Azure, que contenga una red virtual que conste de la instancia de Windows Server Active Directory o esté conectada a ella.
  
Las máquinas virtuales de Azure que cree para Windows Virtual Desktop deben cumplir estos requisitos:

* Estar [unidas a un dominio estándar](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) o a un [dominio híbrido](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). Las máquinas virtuales no pueden estar unidas a Azure AD.
* Deben ejecutar una de las siguientes imágenes de sistema operativo admitidas:
  * Sesión múltiple de Windows 10 Enterprise
  * Windows Server 2016

>[!NOTE]
>Si necesita una suscripción a Azure, puede [registrarse para obtener una evaluación gratuita por un mes](https://azure.microsoft.com/free/). Si usa la versión de evaluación gratuita de Azure, debe utilizar Azure AD Domain Services para mantener sincronizada su instancia de Windows Server Active Directory con Azure Active Directory.

Windows Virtual Desktop consta de los escritorios y las aplicaciones de Windows que entrega a los usuarios y de la solución de administración, que Microsoft hospeda en Azure como un servicio. Durante la versión preliminar pública, se pueden implementar escritorios y aplicaciones en máquinas virtuales (VM) de cualquier región de Azure, y la solución de administración y los datos de estas máquinas virtuales residirán en los Estados Unidos (región Este de EE. UU. 2). Como resultado, se puede producir una transferencia de datos a los Estados Unidos mientras prueba el servicio durante la versión preliminar pública. Una vez disponible con carácter general, se comenzará a escalar horizontalmente la solución de administración y la localización de los datos a todas las regiones de Azure.

Para obtener un rendimiento óptimo, asegúrese de que la red cumple los requisitos siguientes:

* La latencia de ida y vuelta (RTT) desde la red del cliente hasta la región de Azure donde se han implementado grupos host debe ser inferior a 150 ms.
* El tráfico de red puede fluir fuera de las fronteras del país si las máquinas virtuales que hospedan los escritorios y aplicaciones se conectan al servicio de administración.
* Para optimizar el rendimiento de la red, se recomienda que las máquinas virtuales del host de sesión se coloquen en la misma región de Azure que el servicio de administración.

## <a name="provide-feedback"></a>Envío de comentarios

Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad. En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, deberá crear un inquilino. Para más información sobre cómo crear un inquilino, siga con el tutorial de creación de inquilinos.

> [!div class="nextstepaction"]
> [Creación de un inquilino en Windows Virtual Desktop (versión preliminar)](tenant-setup-azure-active-directory.md)
