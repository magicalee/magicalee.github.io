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

## Contact Me
andylee03060306@gmail.com
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
