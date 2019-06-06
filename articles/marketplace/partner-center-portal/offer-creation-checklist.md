---
title: Lista de comprobación de creación - la oferta comercial de Marketplace para Azure
description: Los detalles que pueden proporcionar en el proceso de creación de la oferta. -Comercial Marketplace para Azure
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 904058c2c98c8ded2ea9c91e8aa7ec595aa49b05
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481450"
---
# <a name="offer-creation-checklist"></a>Lista de comprobación de creación de ofertas

El proceso de creación de la oferta le llevará a través de varias páginas. Estos son los detalles que pueden proporcionar en cada página, con vínculos para obtener más información acerca de cada elemento.

Los elementos que se le solicitará que proporcione o que especifique se detallan a continuación. Algunas áreas son opcionales o los valores predeterminados proporcionados, que se puede cambiar según sea necesario. No debe trabajar en estas secciones en el orden mostrado aquí.

| **Elemento**    | **Propósito**  |
| :---------- | :-------------------|
| [**Nuevo estado Modal de oferta**](#new-offer-modal) | Recopila información de identidad de la oferta.  |
| [Página de configuración de la oferta](#offer-setup-page) | Puede optar por usar las características clave y elegir cómo vender su oferta a través de Microsoft.  |
| [Página de propiedades](#properties-page) | Definir las categorías y los sectores que se usa para agrupar la oferta en los catálogos de soluciones, los contratos legales que admite su oferta y la versión de la aplicación. |
| [Página de listado de oferta](#offer-listing-page) | Defina los detalles de la oferta que se mostrará en el mercado, incluidas las descripciones de la oferta y activos de marketing. |
| [Página de vista previa](#preview-page) | Definir a una audiencia de versión preliminar limitada para liberar su oferta antes de publicar su oferta en directo a los destinatarios de marketplace más amplia. |
| [Página de configuración técnica de la oferta](#technical-configuration-page)  | Solo está disponible si selecciona para vender la oferta a través de Microsoft. Defina los detalles técnicos (ruta de acceso URL, webhook, Id. de inquilino y el identificador de aplicación) usados para conectarse a su oferta. |
| [**Nuevo estado Modal de Plan**](#plan-identity-modal) | Recopila información de identidad del plan.  |
| [Página de listado de plan](#plan-listing-page)  | Solo está disponible si selecciona para vender la oferta a través de Microsoft. Defina los detalles usados para mostrar el Plan en marketplace.  |
| [Página de disponibilidad & plan de tarifa](#plan-pricing--availability-page)  | Solo está disponible si selecciona para vender la oferta a través de Microsoft.  Recopila las características empresariales (modelo de precios), la disponibilidad de audiencia y mercado para cada plan (versión) de la oferta.  |
| [Página de listado de unidad de prueba](#test-drive-listing-page)  | Solo está disponible si selecciona para ofrecer una versión de prueba de la oferta. Defina los detalles que se utiliza para la prueba de unidad en el marketplace de lista.  |
| Página de configuración técnica de unidad de prueba  | Solo está disponible si selecciona para ofrecer una versión de prueba de la oferta. Defina los detalles técnicos para la demostración (o "prueba") que permitirán a los clientes probar su oferta antes de comprometerse a comprarlos.  |
| [Revise y página de publicación](#review-and-publish-page)  | Seleccione los cambios que desee publicar, ver el estado de cada página y proporcione notas al equipo de certificación.  |


## <a name="new-offer-modal"></a>Nueva oferta modal 

Los primeros fragmentos de información que se pedirá que proporcione son un nombre y un identificador para la oferta. 

| **Nombre del campo**    | **Notas**   |  
| :---------------- | :-----------| 
| Id. de oferta  | Requerido, no se puede cambiar después de su creación. Máximo de 50 caracteres y debe incluir solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. |
| Nombre de la oferta  | Necesario. |

## <a name="offer-setup-page"></a>Página de configuración de la oferta

La página de configuración de la oferta es donde se puede optar por distintos canales y los movimientos de ventas, así como declarar que conduce el uso de las características clave, como la versión de prueba y el cliente. 

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------|  
| ¿Desea vender a través de Microsoft?  | Necesario. Valor predeterminado: Sí |
| ¿Cómo desea clientes potenciales para interactuar con la oferta recoge? (Llamada a la acción)  | Obligatorio si no se venden a través de Microsoft. Valor predeterminado: Evaluación gratuita, las opciones: "Obtenerla ahora", "Free Trial", "Ponerse en contacto conmigo." |
| Dirección URL de la versión de evaluación  | Necesario si se selecciona "Free Trial", como los clientes de manera deben interactuar con la lista de la oferta. |
| URL de la oferta  | Requerido si "Empezar ahora" está seleccionado, como los clientes de manera deben interactuar con la lista de la oferta |
| Canales  | Opcional. Valor predeterminado: No empezó a participar en el canal CSP (distribuidor).  |
| Versión de prueba | Opcional. Valor predeterminado: No hay ninguna unidad de prueba habilitada.  |
| Tipo de versión de prueba | Obligatorio si se habilita una versión de prueba. Valor predeterminado: Ninguno seleccionado. Opciones: Administrador de recursos de Azure, Dynamics 365 para el centro de negocios, Dynamics 365 for Customer Engagement, Dynamics 365 para las operaciones de aplicación lógica, Power BI.  |
| Administración de clientes potenciales: conectarse a un sistema CRM | Requerida vender a través de Microsoft o a que lista ofrece como "Ponerse en contacto conmigo." Valor predeterminado: ningún sistema CRM conectado. Opciones de CRM: Tabla de Azure, blob de Azure, Dynamics CRM online, de punto de conexión HTTPs, Marketo, Salesforce  |

## <a name="properties-page"></a>Página de propiedades

La página de propiedades es donde se definen las categorías y los sectores que se usa para agrupar la oferta en los catálogos de soluciones, los contratos legales que admite su oferta y la versión de la aplicación. No olvide proporcionar detalles completos y precisos sobre su oferta en esta página, para que se muestren correctamente y que ofrece para el conjunto correcto de los clientes. 

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------|  
| Categoría y subcategoría | Requiere 3 1 y el valor máximo. Valor predeterminado: Ninguno seleccionado. |
| Sectores y subindustries | Opcional. máximo de 2 industrias L1 y máximo de 2 subindustries dentro de cada sector L1, de forma predeterminada: Ninguno seleccionado |
| Versión de la aplicación  | Opcional. Valor predeterminado: Ninguno. |
| Contrato de uso estándar  | Opcional. Valor predeterminado: no se ha seleccionado.  | |
| Términos de uso  | Necesario si el contrato estándar no está seleccionada.  |

## <a name="offer-listing-page"></a>Página de listado de oferta

La página de enumeración es donde debe proporcionar el texto y las imágenes que los clientes ven cuando consultan el listado de su oferta en marketplace. 

| **Nombre del campo**    | **Notas**   |
| :---------------- | :-----------| 
| NOMBRE  | Requerido, máx. los 50 caracteres. |
| Resumen  | Requerido, máx. 100 caracteres. | 
| DESCRIPCIÓN  | Requerido, máx. 3000 caracteres. |
| Instrucciones de introducción  | Requerido, máx. 3000 caracteres. |
| Instrucciones de introducción  | Requerido, máx. 3000 caracteres. |
| Palabras clave de búsqueda  | Opcional, pero recomendado, máximo 3 palabras clave. |
| URL de la política de privacidad  | Necesario. |
| URL de materiales de Marketing de programa CSP  | Opcional. |
| Vínculos útiles título + dirección URL  | Opcional. |
| Compatibilidad con documentos título + File  | Es necesario, 1 min y max 3. Debe ser el formato de archivo PDF. |
| Capturas de pantalla  | Requerido, captura de pantalla de 1 min y max 5; cuatro o más recomendado. Debe ser 1280 X 720 en el formato PNG. |
| Store los logotipos (pequeño, mediano, grande, toda la imagen prominente)  | Pequeño (48 X 48) y grande (216 X 216) necesarios; otros tamaños opcionales pero recomendado: Mediana (90 x 90), todo (255 x 115), el héroe (815 x 290). Debe estar en formato PNG. |
| Nombre de vídeos, dirección URL + miniatura  | Opcional, pero recomendado, el número máximo de 4 vídeos. La miniatura debe ser 1280 x 720 en formato PNG. Vídeo debe hospedarse en YouTube o Vimeo. |
| Contactos (programa CSP, ingeniería, soporte técnico)  | Contacto de soporte técnico y de ingeniería necesario (nombre, correo electrónico y número de teléfono); Contacto del programa CSP opcional pero recomendado. |
| Dirección URL de soporte técnico  | Necesario. |

## <a name="preview-page"></a>Página de vista previa

La página de vista previa es donde debe especificar la audiencia para tener acceso a la versión preliminar de oferta, para comprobar que la oferta cumple todos los requisitos antes de su lanzamiento. 

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------| 
| Correo electrónico AAD/MSA + descripción | Requiere 1 min y max 10 Si especifica manualmente o hasta 20 si carga un archivo CSV. |

## <a name="technical-configuration-page"></a>Página de configuración técnica 

La página de configuración técnica es donde especifica los detalles técnicos de Microsoft usados para conectarse a su oferta. Esta página no es visible para usted si ha decidido no venda a través de Microsoft.

| **Nombre del campo**    | **Notas**   |  
| :---------------- | :-----------| 
| URL de página de aterrizaje | Obligatorio si la venta a través de Microsoft. |
| Webhook de conexión | Obligatorio si la venta a través de Microsoft. |
| Identificador de inquilino de Azure AD | Obligatorio si la venta a través de Microsoft. |
| Identificador de aplicación de Azure AD | Obligatorio si la venta a través de Microsoft. |

## <a name="plan-identity-modal"></a>Plan de identidad modal

El primer fragmentos de información que se le pide que proporcione son un nombre y un identificador para el Plan. Esta página no es visible para usted si ha decidido no venda a través de Microsoft.

| **Nombre del campo**    | **Notas**   |  
| :---------------- | :-----------| 
| Id. de plan  | Obligatorio si la venta a través de Microsoft. No se puede cambiar después de su creación. Máximo de 50 caracteres y debe incluir solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. |
| Nombre del plan  | Obligatorio si la venta a través de Microsoft. Debe ser único en todos los planes en la oferta. Máximo de 50 caracteres. |

## <a name="plan-listing-page"></a>Página de listado de plan

El plan en la página de publicación es donde proporciona el texto para que los clientes ven cuando consultan el plan en marketplace. Esta página no es visible para usted si ha decidido no venda a través de Microsoft.

| **Nombre del campo**    | **Notas**   |  
| :---------------- | :-----------| 
| Descripción del plan   | Obligatorio si la venta a través de Microsoft. Máximo 500 caracteres. | |

## <a name="plan-pricing--availability-page"></a>Plan de la página de precios y disponibilidad

La página de precios y disponibilidad del plan es donde define el características empresariales, la audiencia y disponibilidad en el mercado para cada plan (versión) de la oferta. Esta página no es visible para usted si ha decidido no venda a través de Microsoft.

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------| 
| Disponibilidad en el mercado  | Es necesario, 1 min y max 141. |
| Modelo de precios  | Necesario. Valor predeterminado: Tarifa plana. Opciones: Tarifa plana, por usuario. |
| Como mínimo y máximos puestos  | Opcional, solo está disponible si seleccionarlo el modelo de precios basadas en puestos. |
| Término de facturación  | Necesario. Valor predeterminado: Mensuales. Opciones: Mensual, anual. |
| Precio  | Requiere USD al mes, si la condición seleccionada; facturación mensual o USD al año si anual de facturación de la condición seleccionada. |
| Audiencia de plan  | Opcional. Valor predeterminado: Plan de público. Opciones: Pública, privada por identificador de inquilino |
| Restringido planear audiencia (identificador de inquilino + descripción)  | Necesario si se selecciona un plan privado. 1 min y max 10 identificadores de inquilino si introduce manualmente. Max 20000 si la importación de archivos CSV. |

## <a name="test-drive-listing-page"></a>Página de listado de unidad de prueba

Solo está disponible si selecciona para ofrecer una versión de prueba de la oferta. Defina los detalles que se utiliza para la prueba de unidad en el marketplace de lista.

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------| 
| DESCRIPCIÓN  | Necesario. |
| Nombre de usuario Manual + file  | Requerido, máx. 1 doc. Debe estar en formato PDF. |
| Nombre del vídeo, URL + miniatura  | Opcional, pero recomendado. La miniatura debe ser 533 × 324 en formato JPGP o PNG. Vídeo debe hospedarse en YouTube o Vimeo. |

## <a name="review-and-publish-page"></a>Revise y página de publicación

| **Nombre del campo**    | **Notas**   | 
| :---------------- | :-----------| 
| Notas de la certificación  | Opcional. |

## <a name="next-steps"></a>Pasos siguientes

- [Cree una nueva oferta de SaaS](./create-new-saas-offer.md)
