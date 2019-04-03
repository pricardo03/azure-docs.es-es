---
title: Solución de errores de inicio de sesión mediante informes de Azure Active Directory | Microsoft Docs
description: Aprenda a solucionar errores de inicio de sesión utilizando los informes de Azure Active Directory en Azure Portal.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: db68ad2a29dcaa53d219b679b9e0f24a50a6f576
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877064"
---
# <a name="how-to-troubleshoot-sign-in-errors-using-azure-active-directory-reports"></a>Procedimientos para: Solución de errores de inicio de sesión mediante informes de Azure Active Directory

En el [informe de inicios de sesión](concept-sign-ins.md) de Azure Active Directory (Azure AD), puede encontrar respuesta a algunas preguntas sobre la administración del acceso a las aplicaciones de la organización, como las siguientes:

- ¿Cuál es el patrón de inicio de sesión de un usuario?
- ¿Cuántos usuarios tienen usuarios que han iniciado sesión durante una semana?
- ¿Cuál es el estado de estos inicios de sesión?


Además, el informe de inicios de sesión también puede ayudarle a solucionar errores de inicio de sesión con usuarios de la organización. En esta guía, aprenderá a aislar un error del informe de inicios de sesión y a utilizarlo para entender la causa principal del problema.

## <a name="prerequisites"></a>Requisitos previos

Necesita:

* Un inquilino de Azure AD con una licencia premium (P1 y P2). Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory.
* Un usuario, que se encuentra en la **administrador global**, **Administrador de seguridad**, **lector de seguridad**, o **lector de informes** rol para el inquilino. Además, cualquier usuario puede acceder a sus propios inicios de sesión. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Solución de errores de inicio de sesión mediante el informe de inicios de sesión

1. Vaya a [Azure Portal](https://portal.azure.com) y seleccione su directorio.
2. Seleccione **Azure Active Directory** y elija **Inicios de sesión** en la sección **Supervisión**. 
3. Utilice los filtros proporcionados para restringir el error por el identificador de nombre de usuario, el identificador de un objeto, el nombre de la aplicación o la fecha. Además, seleccione **error** desde el **estado** lista desplegable para mostrar solo los inicios de sesión erróneos. 

    ![Filtrar resultados](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Identifique en el inicio de sesión con errores que desea investigar. Selecciónela para abrir la ventana de detalles adicionales con más información sobre el inicio de sesión con error. Anote el **código de error de inicio de sesión** y el **motivo del error**. 

    ![Seleccionar registro](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. También puede encontrar esta información en la pestaña **Solución de problemas y soporte técnico** de la ventana de detalles.

    ![Solución de problemas y soporte técnico](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. El motivo del error es una descripción del error. Por ejemplo, en el escenario anterior, el motivo del error es **no válido nombre de usuario o contraseña o nombre de usuario no válido en el entorno local o**. La solución consiste sencillamente en iniciar sesión de nuevo con la contraseña y el usuario correctos.

7. Puede obtener información adicional, incluidas algunas ideas sobre posibles soluciones, buscando el código de error (**50126** en este ejemplo) en la [referencia de códigos de error de inicios de sesión](reference-sign-ins-error-codes.md). 

8. Si nada de lo demás funciona o el problema persiste a pesar de tomar las medidas recomendadas, [abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md) siguiendo los pasos de la pestaña **Solución de problemas y soporte técnico**. 

## <a name="next-steps"></a>Pasos siguientes

* [Hacer referencia a los códigos de error de inicios de sesión](reference-sign-ins-error-codes.md)
* [Información general sobre el informe de inicios de sesión](concept-sign-ins.md)
