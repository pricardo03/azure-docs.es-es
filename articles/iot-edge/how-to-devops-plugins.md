---
title: 'Habilitación de CI/CD con el complemento de Jenkins: Azure IoT Edge | Microsoft Docs'
description: La extensión de Azure IoT Edge para Jenkins le permite integrar las tareas de desarrollo e implementación de IoT Edge en la solución de DevOps existente.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 173e6ff91acd2ad28d7203b2b5db65e0ee0ecc43
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126348"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Integración de Azure IoT Edge con canalizaciones de Jenkins

Azure IoT Edge cuenta con compatibilidad integrada con Azure DevOps y Azure DevOps Projects, pero también proporciona un complemento para expandir la funcionalidad de DevOps a Jenkins. [Jenkins](https://jenkins.io/) es un servidor de automatización de código abierto que usa complementos para admitir muchos tipos de proyectos de desarrollo e implementación, incluido IoT Edge. 

El complemento de Azure IoT Edge para Jenkins se centra en la integración continua y la implementación continua. Puede crear una canalización de compilación e inserción que compile módulos e inserte sus imágenes de contenedor en el registro de contenedor. Luego, cree una canalización de versión que implemente los módulos en los dispositivos IoT Edge. 

Antes de empezar a usar el complemento de IoT Edge para Jenkins, necesita un centro de IoT en Azure y un registro de contenedor para almacenar las imágenes de contenedor. Use una entidad de servicio de Azure para dar al colaborador de Jenkins los permisos necesarios a su centro de IoT Edge para que el complemento pueda crear implementaciones para los dispositivos IoT Edge. 

Si está listo para empezar, busque los detalles de instalación y uso del [complemento de Azure IoT Edge para Jenkins](https://plugins.jenkins.io/azure-iot-edge).