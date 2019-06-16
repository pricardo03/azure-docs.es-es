---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66132770"
---
En este ejemplo se usa el servicio [Twilio](https://www.twilio.com/) para enviar mensajes SMS a un teléfono móvil. Azure Functions ya compatible con Twilio a través del [enlace de Twilio](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), el ejemplo utiliza esa característica.

Lo primero que necesita es una cuenta de Twilio. Puede crear una gratis en https://www.twilio.com/try-twilio. Cuando tenga una cuenta, agregue los tres **valores de configuración de la aplicación** siguientes a la aplicación de función.

| Nombre del valor de configuración de la aplicación | Descripción del valor |
| - | - |
| **TwilioAccountSid**  | Identificador de seguridad de la cuenta de Twilio |
| **TwilioAuthToken**   | Token de autenticación de la cuenta de Twilio |
| **TwilioPhoneNumber** | Número de teléfono asociado a la cuenta de Twilio, que se utiliza para enviar mensajes SMS. |