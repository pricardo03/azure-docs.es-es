---
title: 'Azure AD Connect: opciones de dispositivo | Microsoft Docs'
description: En este documento se detallan las opciones de dispositivo disponibles en Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6993895b6f2b836308b8ed7c375212c3fae12768
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46305924"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: opciones de dispositivo

La siguiente documentación proporciona información acerca de las distintas opciones de dispositivo disponibles en Azure AD Connect. Azure AD Connect se puede usar para configurar las dos operaciones siguientes: 
* **Combinación de Azure AD híbrido**: si su entorno tiene un uso local de AD y también quiere aprovechar las ventajas de Azure AD, puede implementar dispositivos híbridos unidos a Azure AD. Estos dispositivos están unidos tanto al entorno local de Active Directory como a Azure Active Directory.
* **Escritura diferida de dispositivos**: la escritura diferida de dispositivos se utiliza para habilitar el acceso condicional basado en dispositivos a los dispositivos protegidos de AD FS (2012 R2 o posterior)

## <a name="configure-device-options-in-azure-ad-connect"></a>Configurar opciones de dispositivo en Azure AD Connect

1.  Ejecute Azure AD Connect. En la página **Tareas adicionales**, seleccione **Configurar opciones de dispositivo**.  Haga clic en **Next**.
    ![Configurar opciones de dispositivo](./media/how-to-connect-device-options/deviceoptions.png) 

    En la página **Información general** se muestran los detalles.
    ![Información general](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > La nueva opción Configurar opciones de dispositivo está disponible únicamente en la versión 1.1.819.0 y posteriores.

2.  Después de proporcionar las credenciales de Azure AD, en la página Opciones de dispositivo puede elegir la operación que se va a realizar.
    ![Operaciones de dispositivo](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de dispositivos híbridos unidos a Azure Active Directory](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Configuración/deshabilitación de escritura diferida de dispositivo](how-to-connect-device-writeback.md)

