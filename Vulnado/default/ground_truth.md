OWASP Vulnado

SQL Injection

• Где ошибка: В src/main/java/com/scalesec/vulnado/User.java поисковые SQL-запросы к PostgreSQL формируются через прямую конкатенацию строк с пользовательским вводом вместо PreparedStatement.

Cross-Site Scripting (XSS)

• Где ошибка: В src/main/java/com/scalesec/vulnado/CowController.java входящие текстовые данные выводятся обратно в HTTP-ответ без фильтрации и HTML-экранирования.

Server-Side Request Forgery (SSRF)

• Где ошибка: В эндпоинтах, обрабатывающих внешние ссылки, отсутствует валидация переданных URL-адресов и проверка по белому списку разрешенных хостов/IP.
