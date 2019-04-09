---
title: ¿Qué es la administración de certificados OPC UA de Azure IoT | Microsoft Docs
description: Introducción a OPC Vault
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ba3313d927ec4317d6c051f6058d75a415b92288
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759817"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-ua-certificate-management"></a>¿Qué es la administración de certificados de comunicaciones de plataforma abierta (OPC) UA de Azure IoT?

La administración de certificados OPC UA de Azure IoT, que también se conoce como OPC Vault, es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. En este artículo se describen los casos de uso sencillos de OPC Vault.

## <a name="certificate-management"></a>Administración de certificados

Por ejemplo, una empresa de fabricación debe conectar su máquina servidor de OPC UA a su aplicación cliente recién creada. Cuando el fabricante crea el acceso inicial de la máquina servidor, se muestra inmediatamente un mensaje de error en la aplicación servidor de OPC UA para indicar que la aplicación cliente no es segura. Este mecanismo se incorpora en la máquina servidor de OPC UA para evitar el acceso no autorizado a las aplicaciones, y así impedir la piratería feroz en la planta.

## <a name="application-security-management"></a>Administración de la seguridad de aplicaciones
Un profesional de seguridad usa el microservicio OPC Vault para permitir que el servidor OPC UA se comunique fácilmente con cualquier aplicación cliente, porque OPC Vault tiene todas las funciones para el registro, el almacenamiento y la administración del ciclo de vida de los certificados. Ahora que el servidor de OPC UA está conectado de forma segura, se puede comunicar con la aplicación cliente recién creada.

## <a name="the-complete-opc-vault-architecture"></a>La arquitectura completa de OPC Vault
El siguiente diagrama ilustra la arquitectura completa de OPC Vault.

![Arquitectura de OPC Vault](media/overview-opc-vault-architecture/opc-vault.png)