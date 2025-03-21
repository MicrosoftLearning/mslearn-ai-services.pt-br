---
title: Exercícios dos Serviços de IA do Azure
permalink: index.html
layout: home
---

# Exercícios dos Serviços de IA do Azure

Os exercícios a seguir foram projetados para dar suporte aos módulos no Microsoft Learn.


{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Exercises'" %} {% for activity in labs  %} {% unless activity.url contains 'ai-foundry' %}
- [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) {% endunless %} {% endfor %}
