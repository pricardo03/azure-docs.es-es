---
title: Uso obligatorio de dispositivos administrados para el acceso a aplicaciones en la nube mediante el acceso condicional de Azure Active Directory | Microsoft Docs
description: Aprenda a configurar directivas de acceso condicional basadas en dispositivos de Azure Active Directory (Azure AD) que necesiten dispositivos administrados para el acceso a aplicaciones en la nube.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2df2d716516cb8e451c054fce3e184e1b4bf3f9b
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447605"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Instrucciones: Uso obligatorio de dispositivos administrados para el acceso a aplicaciones en la nube mediante el acceso condicional

En un mundo donde la nube y la movilidad son prioritarias, Azure Active Directory (Azure AD) permite el inicio de sesión único en aplicaciones y servicios desde cualquier parte. Los usuarios autorizados pueden acceder a las aplicaciones en la nube desde una amplia variedad de dispositivos, como dispositivos móviles y personales. No obstante, algunos entornos tienen algunas aplicaciones a las que solo se puede acceder mediante dispositivos que reúnan los estándares de seguridad y cumplimiento. A estos dispositivos también se les conoce como dispositivos administrados. 

En este artículo se explica cómo puede configurar directivas de acceso condicional que requieran dispositivos administrados para acceder a determinadas aplicaciones en la nube del entorno. 


## <a name="prerequisites"></a>Requisitos previos

Requerir dispositivos administrados para el acceso a aplicaciones en la nube une el **acceso condicional de Azure AD** y la **administración de dispositivos de Azure AD**. Si no está familiarizado con ninguna de estas áreas, debería leer los siguientes temas en primer lugar:

- **[Acceso condicional en Azure Active Directory](../active-directory-conditional-access-azure-portal.md)**: en este artículo se proporciona tanto información general conceptual de acceso condicional como la terminología relacionada.

- **[Introducción a la administración de dispositivos en Azure Active Directory](../devices/overview.md)**: en este artículo se proporciona información general de las diversas opciones que tiene para mantener los dispositivos bajo el control de la organización. 


## <a name="scenario-description"></a>Descripción del escenario

Dominar el equilibrio entre seguridad y productividad es un desafío. La proliferación de dispositivos compatibles para acceder a recursos en la nube ayuda a mejorar la productividad de los usuarios. Por otro lado, es posible que no desee que se pueda acceder a determinados recursos del entorno con un nivel de protección desconocido. Para estos recursos en cuestión debe hacer que los usuarios solo puedan acceder a ellos mediante un dispositivo administrado. 

Con el acceso condicional de Azure AD, puede abordar este requisito con una única directiva que concede acceso:

- A determinadas aplicaciones en la nube

- A los usuarios y grupos seleccionados

- Uso obligatorio de un dispositivo administrado


## <a name="managed-devices"></a>Dispositivos administrados  

En otras palabras, los dispositivos administrados son dispositivos que están bajo *algún tipo* de control de la organización. En Azure AD, el requisito previo para un dispositivo administrado es que se haya registrado en Azure AD. Al registrar un dispositivo, se crea una identidad para el dispositivo en forma de un objeto de dispositivo. Azure usa este objeto para realizar el seguimiento de información de estado sobre el dispositivo. Como administrador de Azure AD, ya puede utilizar este objeto para alternar (habilitar/deshabilitar) el estado de un dispositivo.
  
![Condiciones basadas en el dispositivo](./media/require-managed-devices/32.png)

Para registrar un dispositivo en Azure AD, tiene tres opciones:

- **[Dispositivos registrados en Azure AD](../devices/overview.md#azure-ad-registered-devices)**: registrar un dispositivo personal en Azure AD

- **[Dispositivos unidos a Azure AD](../devices/overview.md#azure-ad-joined-devices)**: registrar en Azure AD un dispositivo con Windows 10 de la organización que no se haya unido a una versión local de AD. 

- **[Dispositivos unidos a Azure AD híbrido](../devices/overview.md#hybrid-azure-ad-joined-devices)**: registrar en Azure AD un dispositivo Windows 10 o de nivel inferior compatible que esté unido a una instancia de AD local registrada.

Para convertirse en dispositivo administrado, un dispositivo registrado debe ser un **dispositivo unido a Azure AD híbrido** o un **dispositivo que se ha marcado como compatible**.  

![Condiciones basadas en el dispositivo](./media/require-managed-devices/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Requerir dispositivos unidos a Azure AD híbridos

En la directiva de acceso condicional, puede seleccionar **Requerir dispositivo unido a Azure AD híbrido** para indicar que solo se puede acceder a las aplicaciones de nube seleccionadas mediante un dispositivo administrado. 

![Condiciones basadas en el dispositivo](./media/require-managed-devices/10.png)

Esta configuración solo se aplica a dispositivos Windows 10 o de nivel inferior, como Windows 7 o Windows 8 que están unidos a una instancia de AD local. Estos dispositivos solo pueden registrarse en Azure AD con una unión a Azure AD híbrida, que es un [proceso automatizado](../devices/hybrid-azuread-join-plan.md) para registrar un dispositivo Windows 10. 

![Condiciones basadas en el dispositivo](./media/require-managed-devices/45.png)

¿Qué hace que un dispositivo unido a Azure AD híbrido sea un dispositivo administrado?  Para los dispositivos que están unidos a un AD local, se supone que el control de estos dispositivos se ejerce mediante soluciones de administración, como **System Center Configuration Manager (SCCM)** o **la directiva de grupo (GP)** para administrarlos. Dado que no hay ningún método para que Azure AD determine si alguno de estos métodos se ha aplicado a un dispositivo, requerir un dispositivo unido a Azure AD híbrido es un mecanismo relativamente débil para requerir un dispositivo administrado. Depende de usted como administrador juzgar si los métodos que se aplican a sus dispositivos locales unidos a un dominio son lo suficientemente estrictos para constituir un dispositivo administrado si tal dispositivo también es un dispositivo unido a Azure AD híbrido.


## <a name="require-device-to-be-marked-as-compliant"></a>Requerir que el dispositivo esté marcado como compatible

La opción de *requerir que un dispositivo esté marcado como compatibles* es la forma más eficaz para solicitar un dispositivo administrado.

![Condiciones basadas en el dispositivo](./media/require-managed-devices/11.png)

Esta opción exige registrar un dispositivo en Azure AD y, también, que se marque como compatible por:
         
- Intune.
- Un sistema administrado de dispositivos móviles (MDM) de terceros que administra los dispositivos de Windows 10 a través de la integración de Azure AD. No se pueden utilizar sistemas MDM de terceros con otros sistemas operativos que no sean Windows 10.
 
![Condiciones basadas en el dispositivo](./media/require-managed-devices/46.png)



Para un dispositivo que está marcado como compatible, puede suponer que: 

- Los dispositivos móviles que los trabajadores usan para tener acceso a los datos de la empresa son administrados.
- Las aplicaciones móviles que los trabajadores usan son administradas.
- La información empresarial está protegida al ayudar a controlar la manera en que los trabajadores acceden a ella y la comparten.
- El dispositivo y sus aplicaciones son compatibles con los requisitos de seguridad de la empresa.




## <a name="next-steps"></a>Pasos siguientes

Antes de configurar una directiva de acceso condicional basada en dispositivos en su entorno, debería echar un vistazo a [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).

