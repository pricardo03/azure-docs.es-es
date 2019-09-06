---
title: 'Protección de la comunicación de dispositivos OPC UA con OPC Vault: Azure | Microsoft Docs'
description: En este artículo se explica cómo registrar las aplicaciones OPC UA y cómo emitir certificados de aplicación firmados para los dispositivos OPC UA con OPC Vault.
author: mregen
ms.author: mregen
ms.date: 8/16/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae10bb3488c21b6637163e333231e95a18c652bf
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69996136"
---
# <a name="how-to-use-the-opc-vault-certificate-management-service"></a>Uso del servicio de administración de certificados de OPC Vault

En este artículo se explica cómo registrar las aplicaciones y cómo emitir certificados de aplicación firmados para los dispositivos OPC UA.

## <a name="prerequisites"></a>Requisitos previos

### <a name="deploy-the-certificate-management-service"></a>Implementar el servicio de administración de certificados

En primer lugar, el servicio debe implementarse en la nube de Azure.
Busque un artículo en el que se describe cómo [implementar el servicio de administración de certificados de OPC Vault](howto-opc-vault-deploy.md).

### <a name="create-the-issuer-ca-certificate"></a>Crear el certificado de entidad de certificación del emisor

Si todavía no lo ha hecho, cree el certificado de entidad de certificación del emisor.

Busque un artículo que describa cómo [crear y administrar el certificado del emisor para OPC Vault](howto-opc-vault-manage.md).

## <a name="secure-opc-ua-applications"></a>Protección de las aplicaciones OPC UA

### <a name="step-1-register-your-opc-ua-application"></a>Paso 1: registro de la aplicación OPC UA 

> [!IMPORTANT]
> El rol "Escritor" es necesario para registrar una aplicación.

1. Abra el servicio de certificados en `https://myResourceGroup-app.azurewebsites.net` e inicie sesión.
2. Vaya a la página `Register New`.
1. Para un registro de aplicación, un usuario debe tener asignado al menos el rol "Escritor".
2. El formulario de entrada sigue las convenciones de nomenclatura del mundo de OPC UA. Como ejemplo, en la imagen que aparece debajo de la configuración del ejemplo de [servidor de referencia de OPC UA](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/Reference) en OPC UA se muestra la pila .NetStandard:

   ![Registro del servidor de referencia de UA](media/howto-opc-vault-secure/reference-server-registration.png "Registro del servidor de referencia de UA")

5. Presione el botón `Register` para registrar la aplicación en la base de datos de aplicación del servicio de certificados. El flujo de trabajo guía directamente al usuario al paso siguiente para solicitar un certificado firmado para la aplicación.

### <a name="step-2-secure-your-application-with-a-ca-signed-application-certificate"></a>Paso 2: protección de la aplicación con un certificado de aplicación firmado por una entidad de certificación

Una aplicación OPC UA se puede proteger mediante la emisión de un certificado firmado basado en una solicitud de firma de certificado (CSR) o mediante la solicitud de un nuevo par de claves, que incluye también una clave privada nueva en formato PFX o PEM. Siga la documentación del dispositivo OPC UA sobre qué método es compatible con su aplicación. En general, se recomienda el método CSR, ya que no requiere la transferencia de una clave privada a través de una conexión.

- Si el dispositivo necesita emitir un nuevo par de claves, siga el [paso 3a.](##step-3a-request-a-new-certificate-with-a-new-keypair)
- Si el dispositivo admite la emisión de una solicitud de firma de certificado (CSR), siga el [paso 3b](#step-3b-request-a-new-certificate-with-a-csr).

### <a name="step-3a-request-a-new-certificate-with-a-new-keypair"></a>Paso 3a: solicitud de un nuevo certificado con un par de claves nuevo

1. Vaya a la página `Applications`.
3. Para iniciar el flujo de trabajo de la solicitud de certificado, seleccione `New Request` para una aplicación de la lista.

   ![Solicitar certificado nuevo](media/howto-opc-vault-secure/request-new-certificate.png "Solicitar certificado nuevo")

3. Presione "Request new KeyPair and Certificate" (Solicitar nuevo par de claves y certificado) para solicitar una clave privada y un nuevo certificado firmado con la clave pública para la aplicación.

   ![Generar par de claves nuevo](media/howto-opc-vault-secure/generate-new-key-pair.png "Generar par de claves nuevo")

4. Rellene el formulario con un firmante, los nombres de dominio y elija PEM o PFX con la contraseña de la clave privada. Presione el botón `Generate New Certificate` para crear la solicitud de certificado.

   ![Aprobar certificado](media/howto-opc-vault-secure/approve-reject.png "Aprobar certificado")

5. El paso de aprobación requiere un usuario con el rol "Aprobador" y con permisos de firma en Azure Key Vault. En el flujo de trabajo típico, los roles Aprobador y Solicitante deben estar asignados a distintos usuarios.
6. Apruebe o rechace la solicitud de certificado para iniciar o cancelar la creación real del par de claves y la operación de firma. El nuevo par de claves se crea y se almacena de manera segura en Azure Key Vault hasta que el solicitante del certificado lo descarga. La entidad de certificación firma el certificado resultante con la clave pública. Estas operaciones pueden tardar unos segundos en finalizar.

   ![Ver el par de claves](media/howto-opc-vault-secure/view-key-pair.png "Ver el par de claves")

7. La clave privada (PFX o PEM) y el certificado (DER) resultantes se pueden descargar desde aquí en el formato seleccionado como descarga de archivos binarios. También está disponible una versión codificada en Base64, por ejemplo, para copiar y pegar el certificado en una línea de comandos o en una entrada de texto. 
8. Una vez que la clave privada se descarga y se almacena de manera segura, puede eliminarse del servicio con el botón `Delete Private Key`. El certificado con la clave pública sigue disponible para su uso futuro.
9. Debido al uso de un certificado firmado por una entidad de certificación, el certificado de entidad de certificación y la CLR también se deben descargar aquí.
10. Ahora la aplicación del nuevo par de claves depende del dispositivo OPC UA. Por lo general, el certificado de entidad de certificación y la CRL se copian en una carpeta `trusted`, mientras que la clave pública y privada del certificado de aplicación se aplica a una carpeta `own` en el almacén de certificados. Es posible que algunos dispositivos ya admitan la "inserción de servidor" para las actualizaciones de certificados. Consulte la documentación del dispositivo OPC UA.

### <a name="step-3b-request-a-new-certificate-with-a-csr"></a>Paso 3b: solicitud de un certificado nuevo con un CSR 

1. Vaya a la página `Applications`.
3. Para iniciar el flujo de trabajo de la solicitud de certificado, seleccione `New Request` para una aplicación de la lista.

   ![Solicitar certificado nuevo](media/howto-opc-vault-secure/request-new-certificate.png "Solicitar certificado nuevo")

3. Presione "Request new Certificate with signing request" (Solicitar nuevo certificado con solicitud de firma) para solicitar un certificado firmado nuevo para la aplicación.

   ![Generar certificado nuevo](media/howto-opc-vault-secure/generate-new-certificate.png "Generar certificado nuevo")

4. Para cargar CSR, seleccione un archivo local o pegue un CSR con codificación Base64 en el formulario. Presione el botón `Generate New Certificate` para crear la solicitud de certificado.

   ![Aprobar el CSR](media/howto-opc-vault-secure/approve-reject-csr.png "Aprobar el CSR")

5. El paso de aprobación requiere un usuario con el rol "Aprobador" y con permisos de firma en Azure Key Vault. Apruebe o rechace la solicitud de certificado para iniciar o cancelar la operación de firma real. La entidad de certificación firma el certificado resultante con la clave pública. Esta operación puede tardar unos segundos en finalizar.

   ![Ver certificado](media/howto-opc-vault-secure/view-cert-csr.png "Ver certificado")

6. El certificado resultante (DER) se puede descargar desde aquí como archivo binario. También está disponible una versión codificada en Base64, por ejemplo, para copiar y pegar el certificado en una línea de comandos o en una entrada de texto. 
10. Una vez que el certificado se descarga y se almacena de manera segura, puede eliminarse del servicio con el botón `Delete Certificate`.
11. Debido al uso de un certificado firmado por una entidad de certificación, el certificado de entidad de certificación y la CLR también se deben descargar aquí.
12. Ahora la aplicación del nuevo certificado depende del dispositivo OPC UA. Por lo general, el certificado de entidad de certificación y la CRL se copian en una carpeta `trusted`, mientras que el certificado de aplicación se aplica a una carpeta `own` en el almacén de certificados. Es posible que algunos dispositivos ya admitan la "inserción de servidor" para las actualizaciones de certificados. Consulte la documentación del dispositivo OPC UA.

### <a name="step-4-device-secured"></a>Paso 4: dispositivo protegido

El dispositivo OPC UA ya está listo para comunicarse con otros dispositivos de OPC UA protegidos por certificados firmados por una entidad de certificación sin más configuraciones. ¡Disfrute!


## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a proteger los dispositivos OPC UA, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Ejecución de un servicio de administración de certificados seguro](howto-opc-vault-secure-ca.md)