---
layout: page
title: 麻吉卡
description: About Me
keywords: Me
comments: true
menu: Me
permalink: /about/
---

Magica

平日禮拜四或禮拜五通常會約桌遊團，假日偶爾也會開

想要參加桌遊團或是交流其他項目都可以在文章下回覆或是加我LINE

## Contact Me
* Email : andylee03060306@gmail.com
* Line  : amilegend

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords
### Domain Know-how
* Financial Technology (Especially payment technology)
* Blockchain in Finance

### Blockchain
* Hyperledger Fabric
* Ethereum & Dapp
* Bitcoin
* Cryptocurrency

### Backend
* Language: Groovy, Java, Chaincode(Golang), Solidity
* Framework: Grails, Spring Boot
* Web Service: RESTful, SOAP
* Database: MSSQL
* Build: Gradle, Maven
* Server: RedHat JBOSS EAP
* Platform : Unity

### Frontend
* Language: HTML5, CSS, JavaScript
* Framework: jQuery, Bootstrap

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
