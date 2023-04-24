---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Ejercicios de Fundamentos de Azure AI

Estos ejercicios prácticos están diseñados para admitir contenido de formación de [Microsoft Learn](https://docs.microsoft.com/training/).

Para completar estos ejercicios, necesitará una suscripción a Microsoft Azure. Puede registrarse para obtener una prueba gratuita en [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="labs"></a>Laboratorios

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| Módulo | Laboratorio |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
