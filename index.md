---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# Ejercicios de Fundamentos de Azure AI

Estos ejercicios prácticos están diseñados para admitir contenido de formación de [Microsoft Learn](https://docs.microsoft.com/training/).

Para completar estos ejercicios, necesitará una suscripción a Microsoft Azure. Puede registrarse para obtener una prueba gratuita en [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Ejercicios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
