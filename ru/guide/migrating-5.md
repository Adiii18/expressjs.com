---
### TRANSLATION INSTRUCTIONS FOR THIS SECTION:
### TRANSLATE THE VALUE OF THE title ATTRIBUTE AND UPDATE THE VALUE OF THE lang ATTRIBUTE. 
### DO NOT CHANGE ANY OTHER TEXT. 
layout: page
title: Миграция до версии Express 5
menu: guide
lang: ru
redirect_from: "/guide/migrating-5.html"
### END HEADER BLOCK - BEGIN GENERAL TRANSLATION
---

# Переход к Express 5

<h2 id="overview">Обзор</h2>

Express 5.0 пока выпущен как альфа-версия, но ниже кратко описаны изменения, которые будут внесены в этом выпуске, а также способы миграции приложения Express 4 до версии Express 5.

Express 5 не слишком отличается от Express 4: Изменения в API не столь значительны, как в версии 4.0 по сравнению с версией 3.0.  Хотя базовый API остается прежним, изменения, ломающие существующий код, все же присутствуют; другими словами, существующая программа версии Express 4, возможно, не будет работать после обновления до версии Express 5.

Для того чтобы установить новейший альфа-выпуск и предварительно ознакомиться с Express 5, введите в корневом каталоге приложения следующую команду:

<pre>
<code class="language-sh" translate="no">
$ npm install express@5.0.0-alpha.2 --save
</code>
</pre>

Затем можно провести автоматическое тестирование, выявить сбои и устранить неполадки в соответствии с перечисленными ниже изменениями. После устранения сбоев, выявленных путем тестирования, запустите приложение и обратите внимание на возникшие ошибки. Если приложением используются методы или свойства, поддержка которых приостановлена, вы заметите это немедленно.

<h2 id="changes">Изменения в Express 5</h2>

Ниже приводится список изменений (внесенных в альфа-выпуске 2), являющихся значимыми для пользователей Express.
Список всех функций, включение которых планируется, приведен в [запросе на включение](https://github.com/strongloop/express/pull/2237).

**Удаленные методы и свойства**

<ul class="doclist">
  <li><a href="#app.del">app.del()</a></li>
  <li><a href="#app.param">app.param(fn)</a></li>
  <li><a href="#plural">Названия методов во множественном числе</a></li>
  <li><a href="#leading">Двоеточие перед аргументом name в app.param(name, fn)</a></li>
  <li><a href="#req.param">req.param(name)</a></li>
  <li><a href="#res.json">res.json(obj, status)</a></li>
  <li><a href="#res.jsonp">res.jsonp(obj, status)</a></li>
  <li><a href="#res.send.body">res.send(body, status)</a></li>
  <li><a href="#res.send.status">res.send(status)</a></li>
  <li><a href="#res.sendfile">res.sendfile()</a></li>
</ul>

**Измененные:**

<ul class="doclist">
  <li><a href="#app.router">app.router</a></li>
  <li><a href="#req.host">req.host</a></li>
  <li><a href="#req.query">req.query</a></li>
</ul>

**Усовершенствованные:**

<ul class="doclist">
  <li><a href="#res.render">res.render()</a></li>
</ul>

<h3>Удаленные методы и свойства</h3>

Если указанные ниже методы и свойства используются в вашем приложении, оно даст сбой. Поэтому, после обновления до версии 5 вам потребуется внести изменения в существующее приложение.

<h4 id="app.del">app.del()</h4>

В Express 5 больше не поддерживается функция `app.del()`. В случае использования этой функции выдается сообщение об ошибке. Для регистрации маршрутов HTTP DELETE воспользуйтесь функцией `app.delete()`.

Первоначально, вместо `delete` использовался код `del`, так как `delete` является зарезервированным ключевым словом в JavaScript. Тем не менее, начиная с версии ECMAScript 6, `delete` и другие зарезервированные ключевые слова разрешается использовать в качестве имен свойств. Здесь можно ознакомиться с рассуждениями, в результате которых было принято решение о признании функции `app.del` устаревшей.

<h4 id="app.param">app.param(fn)</h4>

Сигнатура `app.param(fn)` использовалась для изменения особенностей функции `app.param(name, fn)`. Она помечена как устаревшая, начиная с версии 4.11.0, а в Express 5 ее поддержка окончательно приостановлена.

<h4 id="plural">Названия методов во множественном числе</h4>

Перечисленные ниже названия методов преобразованы в форму множественного числа. В Express 4 при использовании старых методов выдавалось предупреждение об устаревании.  В Express 5 они уже не поддерживаются:

`req.acceptsCharset()` заменен на `req.acceptsCharsets()`.

`req.acceptsEncoding()` заменен на `req.acceptsEncodings()`.

`req.acceptsLanguage()` заменен на `req.acceptsLanguages()`.

<h4 id="leading">Двоеточие (:) перед аргументом name в app.param(name, fn)</h4>

Символ двоеточия (:) перед аргументом name в функции `app.param(name, fn)` - это пережиток Express 3, и в целях совместимости с предыдущими версиями он поддерживался в Express 4, снабженный пометкой об устаревании. В Express 5 это полностью игнорируется, и параметр name будет использоваться без предшествующего двоеточия.

Это не повлияет на существующий код, при условии следования документации Express 4 в отношении параметра [app.param](/{{ page.lang }}/4x/api.html#app.param), поскольку там нет упоминания о двоеточии перед аргументом.

<h4 id="req.param">req.param(name)</h4>

Этот потенциально неоднозначный и опасный метод извлечения данных форм был удален. Теперь следует специально обращать внимание на имя конкретного переданного параметра в объекте `req.params`, `req.body` и `req.query`.

<h4 id="res.json">res.json(obj, status)</h4>

В Express 5 больше не поддерживается сигнатура `res.json(obj, status)`. Вместо этого необходимо задать аргумент status и объединить его в цепочку с методом `res.json()` следующим образом: `res.status(status).json(obj)`.

<h4 id="res.jsonp">res.jsonp(obj, status)</h4>

В Express 5 больше не поддерживается сигнатура `res.jsonp(obj, status)`. Вместо этого необходимо задать аргумент status и объединить его в цепочку с методом `res.jsonp()` следующим образом: `res.status(status).jsonp(obj)`.

<h4 id="res.send.body">res.send(body, status)</h4>

В Express 5 больше не поддерживается сигнатура `res.send(obj, status)`. Вместо этого необходимо задать аргумент status и объединить его в цепочку с методом `res.send()` следующим образом: `res.status(status).send(obj)`.

<h4 id="res.send.status">res.send(status)</h4>

В Express 5 больше не поддерживается сигнатура <code>res.send(<em>status</em>)</code>, где *`status`* является числом. Вместо этого используйте функцию `res.sendStatus(statusCode)`, которая задает код состояния заголовка ответа HTTP и отправляет текстовую версию кода: "Not Found" ("Не найдено"), "Internal Server Error" ("Внутренняя ошибка сервера") и т.д.
Если необходимо передать число с помощью функции `res.send()`, заключите его в кавычки, чтобы преобразовать в строку. Таким образом, Express не будет интерпретировать передачу числа как попытку использования неподдерживаемой устаревшей сигнатуры.

<h4 id="res.sendfile">res.sendfile()</h4>

В Express 5 функция `res.sendfile()` заменена вариантом, в котором вторая часть составной фразы написана с заглавной буквы: `res.sendFile()`.

<h3>Измененные:</h3>

<h4 id="app.router">app.router</h4>

Объект `app.router`, удаленный в Express 4, возвращен в Express 5. В новой версии данный объект представляет собой лишь ссылку на базовый маршрутизатор Express, в отличие от Express 3, где приложение должно было загружать его явным образом.

<h4 id="req.host">req.host</h4>

В Express 4 функция `req.host` некорректно отсекала номер порта, если таковой был указан. В Express 5 указание номера порта сохраняется.

<h4 id="req.query">req.query</h4>

В Express 4.7, Express 5 и в последующих версиях опция анализатора запросов может принимать значение `false`, чтобы отключить синтаксический анализ строки запроса, если в логике анализа строки запроса вам необходимо использовать собственную функцию.

<h3>Усовершенствования</h3>

<h4 id="res.render">res.render()</h4>

Этот метод принудительно задает асинхронное представление для всех средств просмотра, что позволяет избежать ошибок, возникавших из-за средств просмотра с синхронной реализацией, не соответствующих рекомендованному интерфейсу.