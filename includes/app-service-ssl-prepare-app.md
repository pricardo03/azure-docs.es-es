---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 858d098f18604d1360af8509eb8a0cec6fa7d0c7
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49354588"
---
## <a name="prepare-your-web-app"></a>Preparar la aplicación web

Para enlazar un certificado SSL personalizado (un certificado de terceros o un certificado de App Service) a la aplicación web, su [plan de App Service](https://azure.microsoft.com/pricing/details/app-service/) debe estar en el nivel **Básico**, **Estándar**, **Premium** o **Aislado**. En este paso, asegúrese de que la aplicación web se encuentra en el plan de tarifa compatible.

### <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Abra [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navegar a la aplicación web

En el menú izquierdo, haga clic en **App Services** y luego en el nombre de la aplicación web.

![Seleccionar la aplicación web](./media/app-service-ssl-prepare-app/select-app.png)

Ha llegado a la página de administración de la aplicación web.  

### <a name="check-the-pricing-tier"></a>Comprobar el plan de tarifa

En el panel de navegación izquierdo de la página de la aplicación web, desplácese a la sección **Configuración** y seleccione **Escalar verticalmente (plan de App Service)**.

![Menú Escalar verticalmente](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Asegúrese de que la aplicación web no está en el nivel **F1** ni **D1**. El nivel actual de la aplicación web aparece resaltado con un cuadro azul oscuro.

![Comprobar plan de tarifa](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

El SSL personalizado no es compatible con los niveles **F1** y **D1**. Si tiene que escalar verticalmente, siga los pasos de la sección siguiente. Si no, cierre la página **Escalar verticalmente** y vaya directamente a las secciones sobre cómo [cargar y enlazar el certificado SSL](#upload).

### <a name="scale-up-your-app-service-plan"></a>Escalar verticalmente el plan de App Service

Seleccione cualquiera de los niveles no gratuitos (**B1**, **B2**, **B3**, o cualquier nivel de la categoría **Producción**). Para ver opciones adicionales, haga clic en **Ver opciones adicionales**.

Haga clic en **Aplicar**.

![Elegir plan de tarifa](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Cuando vea la siguiente notificación, significará que la operación de escalado se habrá completado.

![Notificación de escalado vertical](./media/app-service-ssl-prepare-app/scale-notification.png)

