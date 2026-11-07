# SAST-Check: стенд для сравнения Semgrep и CodeQL

Репозиторий содержит 30 намеренно уязвимых приложений, независимый эталонный перечень недостатков и результаты сканирования двумя SAST-инструментами: **Semgrep** и **CodeQL**. Стенд предназначен для воспроизводимого сравнения покрытия, анализа срабатываний и подготовки единого SAST-пайплайна.

- **Инструменты:** [Semgrep](https://semgrep.dev/) и [CodeQL](https://codeql.github.com/)
- **Эталон:** 249 записей «проект — тип недостатка» (T-01—T-30, CWE)
- **Языки:** PHP, Java, JavaScript/TypeScript, Python, Ruby, C# и др.
- **Статусы в отчётах:** `Найден`, `Найден частично`, `Не найден`, `Не ищет` (нет профильного правила/query)

---

## Структура репозитория

```text
sast-check/
├── .github/workflows/          # CI-запуск сканирования
├── <ProjectName>/              # 30 каталогов тестовых приложений
│   ├── default/                # эталон проекта (ground truth)
│   ├── semgrep/                # отчёты Semgrep (findings.json)
│   └── codeql/                 # отчёты CodeQL (SARIF)
├── downloaded-results/         # загруженные/нормализованные артефакты
├── repos/                      # upstream-репозитории / сабмодули
├── sbom-tables/                # таблицы SBOM
├── .gitmodules                 # ссылки на upstream-репозитории
├── sast-table.docx # сравнение Semgrep и QodeQL на основе эталонных данных
└── README.md
```

- **`default/`** — независимый эталон: перечень типов недостатков для конкретного проекта. Не является «эталоном для сканера»: сканеры проверяются на соответствие этому перечню.
- **`semgrep/`** — `findings.json` и связанные артефакты Semgrep.
- **`codeql/`** — SARIF-отчёты CodeQL.
- **`.github/workflows/`** — автоматизация запуска.
- **`repos/`** — исходные upstream-проекты или их зеркала, зафиксированные сабмодулями.
- **`downloaded-results/`** — агрегированные/нормализованные выгрузки.
- **`sbom-tables/`** — таблицы состава зависимостей (SBOM).

---

## Тестовые приложения

В корне репозитория размещены 30 каталогов. Ниже — перечень; полные описания, язык, версия/commit и привязка к классам T/CWE ведутся в `README.md` внутри каждого проекта.

| № | Каталог | Назначение / стек |
|---|---------|-------------------|
| 1 | `DVGA` | Damn Vulnerable GraphQL Application |
| 2 | `DVNA` | Damn Vulnerable NodeJS Application |
| 3 | `DVPWA` | Damn Vulnerable Python Web Application |
| 4 | `DVWA` | Damn Vulnerable Web Application (PHP) |
| 5 | `DVWS` | Damn Vulnerable Web Services |
| 6 | `Damn-Vulnerable-Flask-App` | Flask-приложение с учебными уязвимостями |
| 7 | `DjangoGoat` | Django-приложение с учебными уязвимостями |
| 8 | `Goof` | Node.js / Express учебное приложение |
| 9 | `IWA` | уязвимое web-приложение |
| 10 | `OWASP-Benchmark-Java` | OWASP Benchmark (Java) |
| 11 | `OWASP-Juice-Shop` | OWASP Juice Shop (Node.js/TypeScript) |
| 12 | `OWASP-Mutillidae` | OWASP Mutillidae II (PHP) |
| 13 | `OWASP-NodeGoat` | OWASP NodeGoat |
| 14 | `OWASP-WebGoat` | OWASP WebGoat (Java) |
| 15 | `PyGoat` | Python-приложение с учебными уязвимостями |
| 16 | `RailsGoat` | Rails-приложение с учебными уязвимостями |
| 17 | `TicketMagpie` | учебное приложение с бизнес-логикой и доступами |
| 18 | `VAmPI` | Vulnerable API (Python/Flask) |
| 19 | `Vuln-Bookstore` | уязвимый книжный магазин |
| 20 | `Vuln-GraphQL-Ruby` | GraphQL-Ruby с уязвимостями |
| 21 | `VulnBookStore` | Java-приложение с уязвимостями |
| 22 | `VulnShop-Django` | Django-магазин с уязвимостями |
| 23 | `Vulnado` | Java-приложение с уязвимостями |
| 24 | `Vulnerable-API` | API с учебными уязвимостями |
| 25 | `Vulnerable-OTP-App` | приложение с OTP-механикой |
| 26 | `Vulnerable-React-App` | React-приложение |
| 27 | `VulnerableApp` | многоязычный учебный стенд |
| 28 | `WebGoat.NET` | WebGoat для .NET |
| 29 | `XVWA` | Xtreme Vulnerable Web Application (PHP) |
| 30 | `bWAPP` | buggy Web Application (PHP) |

---

## Как воспроизвести сканирование

### Semgrep

```bash
semgrep --config auto --json --output findings.json <project_path>
```

Рекомендуется фиксировать:
- версию Semgrep;
- используемый набор правил (`--config`);
- исключения (`--exclude`);
- дату сканирования.

### CodeQL

```bash
codeql database create <db> --language=<lang> --source-root=<project_path>
codeql database analyze <db> <query-pack> --format=sarif-latest --output=results.sarif
```

Рекомендуется фиксировать:
- версию CodeQL CLI;
- язык и способ сборки;
- дату сканирования.

---

## Как читать результаты

1. **`default/`** — эталон: что *должно* быть найдено.
2. **`semgrep/findings.json`** и **`codeql/*.sarif`** — что *фактически* найдено.
3. Сопоставление выполняется на уровне пары «проект — тип недостатка» с учётом CWE, места в проекте и пути данных.
4. Статус **«Не ищет»** означает отсутствие профильного правила/query.
5. Срабатывания вне эталона **не считаются FP автоматически**

Сводные таблицы и подробное сопоставление ведутся в отчёте; в репозитории сохраняются raw JSON/SARIF и нормализованные CSV для пересчёта метрик.

---

## Ссылки

- **Репозиторий:** https://github.com/konurovnikita-svg/sast-check
- **Semgrep:** https://semgrep.dev/docs/
- **CodeQL:** https://codeql.github.com/docs/

Используемые проекты:

OWASP-Juice-Shop — https://github.com/juice-shop/juice-shop.git — CodeQL: javascript-typescript

OWASP-WebGoat — https://github.com/WebGoat/WebGoat.git — CodeQL: java-kotlin

OWASP-NodeGoat — https://github.com/OWASP/NodeGoat.git — CodeQL: javascript-typescript

DVWA — https://github.com/digininja/DVWA.git — CodeQL: skip

OWASP-Benchmark-Java — https://github.com/OWASP/BenchmarkJava.git — CodeQL: java-kotlin

VulnBookStore — https://github.com/Defcon2019/vulnerable-repo.git — CodeQL: java-kotlin 

VulnShop-Django — https://github.com/laugiov/code-safety.git — CodeQL: python 

PyGoat — https://github.com/adeyosemanputra/pygoat.git — CodeQL: python 

VAmPI — https://github.com/erev0s/VAmPI.git — CodeQL: python

DVGA — https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application.git — CodeQL: python

VulnerableApp — https://github.com/SasanLabs/VulnerableApp.git — CodeQL: java-kotlin

OWASP-Mutillidae — https://github.com/webpwnized/mutillidae.git — CodeQL: skip

DVWS — https://github.com/interference-security/DVWS.git — CodeQL: skip

XVWA — https://github.com/s4n7h0/xvwa.git — CodeQL: skip

bWAPP — https://github.com/jehy-security/bwapp.git — CodeQL: skip

Vulnado — https://github.com/bbhunter/vulnado.git — CodeQL: java-kotlin

WebGoat.NET — https://github.com/jerryhoff/WebGoat.NET.git — CodeQL: C#

RailsGoat — https://github.com/OWASP/railsgoat.git — CodeQL: ruby

TicketMagpie — https://github.com/dhatanian/ticketmagpie.git — CodeQL: java-kotlin

Vulnerable-OTP-App — https://github.com/mddanish/Vulnerable-OTP-Application.git — CodeQL: skip

Damn-Vulnerable-Flask-App — https://github.com/maximilianmarx/DVFA.git — CodeQL: python

DVPWA — https://github.com/anxolerd/dvpwa.git — CodeQL: python

Vuln-GraphQL-Ruby — https://github.com/rmosolgo/graphql-ruby.git — CodeQL: ruby

IWA — https://github.com/fortify/IWA-Java.git — CodeQL: javascript-typescript — проверить, Java-репо с js-языком

DVNA — https://github.com/appsecco/dvna.git — CodeQL: javascript-typescript

Goof — https://github.com/snyk/goof.git — CodeQL: javascript-typescript

Vulnerable-React-App — https://github.com/vulnerable-apps/vulnerable-react-app — CodeQL: javascript-typescript — без .git на конце

Vulnerable-API — https://github.com/jorritfolmer/vulnerable-api.git — CodeQL: javascript-typescript

DjangoGoat — https://github.com/adeyosemanputra/pygoat.git — CodeQL: python 

Vuln-Bookstore — https://github.com/Defcon2019/vulnerable-repo.git — CodeQL: java-kotlin 

