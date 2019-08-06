---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/17/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: f97ab0a0bb91af90daaf60c5c2c85f0ede56a5e7
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403968"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

En este artículo, se crea un servicio back-end que agrega propiedades deseadas a un dispositivo gemelo y luego consulta el registro de identidades para buscar todos los dispositivos con propiedades notificadas que se han actualizado en consecuencia. El servicio necesita el permiso **Conectar al servicio** para modificar las propiedades deseadas de un dispositivo gemelo y el permiso **Lectura del Registro** para consultar el registro de identidades. No hay ninguna directiva de acceso compartido predeterminada que contenga solo estos dos permisos, por lo que tendrá que crearla.
