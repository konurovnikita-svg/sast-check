• A1 — Injection: Включает в себя Server Side JS Injection (выполнение кода через eval) и NoSQL Injection (обход логина в MongoDB).
• A2 — Broken Authentication: Ошибки в механизме сессий и уязвимость к брутфорсу (подбору паролей).
• A3 — XSS: Межсайтовый скриптинг (хранимый и отраженный).
• A4 — Insecure Direct Object References (IDOR): Прямой доступ к чужим профилям по userId без проверки прав.
• A5 — Security Misconfiguration: Включенный дебаггер Werkzeug и утечки системных логов.
• A6 — Sensitive Data Exposure: Хранение паролей в базе данных без безопасного шифрования/хеширования.
• A9 — Using Components with Known Vulnerabilities: Использование устаревших npm-пакетов с публичными уязвимостями.
