---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688697"
---
# <a name="azure-ai-fundamentals-exercises"></a>Ejercicios de Fundamentos de Azure AI

Use los vínculos siguientes para completar los ejercicios del laboratorio práctico del curso [AI-900 *Aspectos básicos de Microsoft Azure AI*](https://docs.microsoft.com/learn/certifications/courses/ai-900t00).

Para completar estos ejercicios, necesitará una suscripción a Microsoft Azure. Si su instructor no se la ha proporcionado, puede registrarse para obtener una evaluación gratuita en[https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Ejercicios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
