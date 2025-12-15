# Lab5
## Практическая часть (SAST-анализ)
### 1) Анализ уязвимого файла с помощью утилиты Bandit.
<img width="886" height="962" alt="image" src="https://github.com/user-attachments/assets/ce7908d6-e203-4f33-b257-1c0c76bd7bd6" />
<img width="569" height="356" alt="image" src="https://github.com/user-attachments/assets/fa3d0a63-8499-4a75-9694-ff22d99f9406" />

### 2) Описание найденных уязвимостей

#### Критические уязвимости 

**1.Flask Debug Mode Enabled**

Описание: Приложение запускается с debug=True, что включает интерактивный отладчик Werkzeug.

Риски: Позволяет выполнять произвольный код через консоль отладчика при возникновении ошибок.

Расположение: Строка 256: app.run(debug=True, host='0.0.0.0', port=5000)

**2.Subprocess with shell=True**

Описание: Использование subprocess.check_output() с shell=True.

Риски: Возможность инъекции команд через переменную host.

Расположение: Строка 185: subprocess.check_output("ping -c 1 {host}", shell=True, text=True)

#### Средние уязвимости 

**3.Hardcoded Bind to All Interfaces**
   
Описание: Приложение привязано к 0.0.0.0, что делает его доступным из любой сети.

Риски: Повышает риск несанкционированного доступа, если не защищено брандмауэром.

Расположение: Строка 256: host='0.0.0.0'

**4.Possible SQL Injection**

Описание: Конкатенация строк в SQL-запросе без использования параметризованных запросов.

Риски: Возможность внедрения SQL-кода через username или password.

Расположение: Строка 43: query = "SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"

**5.Unsafe Pickle Deserialization**

Описание: Использование pickle.loads() для десериализации данных от пользователя.

Риски: Удаленное выполнение кода при десериализации вредоносных данных.

Расположение: Строка 166: data = pickle.loads(bytes.fromhex(user_data))

**5.Request Without Timeout**

Описание: Вызов requests.get() без указания таймаута.

Риски: Возможность атаки типа "отказ в обслуживании" через долгие ответы.

Расположение: Строка 286: response = requests.get(url)

#### Низкие уязвимости 

**7.Import of Unsafe Modules**

Описание: Импорт модулей pickle и subprocess, которые могут быть использованы небезопасно.

Риски: Повышает вероятность ошибок при их использовании.

Расположение: Строки 7–8: import pickle, import subprocess

**8.Hardcoded Secret Key**

Описание: Жестко заданный секретный ключ Flask (supersecretkey).

Риски: Позволяет подделывать сессии и CSRF-токены, если ключ известен.

Расположение: Строка 12: app.secret_key = 'supersecretkey'

### 3) Проанализируйте уязвимый файл с помощью других утилит.
В нашем случае не получилось просканировать уязвимый файл с помощью альтернативных средств, таких как semgrep (при использовании различных аргументов команды включения команд и одних и тех же ошибок «отказано в разрешении» или «тайм-аут чтения») или SonarQube (при генерации токена оперативная память была предварительно перегружена, что повлекло за собой последовательность результатов работы машины).

<img width="1472" height="488" alt="image" src="https://github.com/user-attachments/assets/9cc79654-3602-42ef-9cdb-08f4fec4e972" />

### 4) Сравнительный анализ.
Поскольку на установку и использование утилит Bandit ушло ЗНАЧИТЕЛЬНО меньше времени и ресурсов, чем на semgrep или SonarQube (которые в конечном итоге даже не привели к результатам своей работы), очевидный вывод - Bandit является самой лучшей программой для SAST-анализа.


## Практическая часть (DAST-анализ)

### 1) Программа

<img width="437" height="310" alt="image" src="https://github.com/user-attachments/assets/0b95e683-d303-423f-b694-d54ebb65d565" /> <br>
<img width="420" height="124" alt="image" src="https://github.com/user-attachments/assets/f7818308-42c3-46c1-9b43-c241a94e164c" />  <br>
<img width="266" height="108" alt="image" src="https://github.com/user-attachments/assets/47713f80-d610-44ad-a01c-7715bc2b43e8" />  <br>
<img width="242" height="169" alt="image" src="https://github.com/user-attachments/assets/dafad57c-4ec6-4f46-be0b-10af9b0184e9" />  <br>
<img width="379" height="103" alt="image" src="https://github.com/user-attachments/assets/15c20010-6dd8-4f8c-89e2-c37c8d6fa875" />  <br>
<img width="426" height="248" alt="image" src="https://github.com/user-attachments/assets/72bb8e61-bccd-4383-b322-61476d7a7352" />  <br>
<img width="165" height="107" alt="image" src="https://github.com/user-attachments/assets/ed9c53c6-d35a-40ef-ad84-ca0e34976fe8" />  <br>
<img width="194" height="66" alt="image" src="https://github.com/user-attachments/assets/fb45b080-5843-499d-a060-23ce557846ae" /> <br>
<img width="232" height="91" alt="image" src="https://github.com/user-attachments/assets/4f95b149-57f5-4b39-8122-936d86988606" /> <br>
<img width="620" height="329" alt="image" src="https://github.com/user-attachments/assets/7dcb0668-cfe6-412f-9980-458a419eac24" /> <br>

### 2) Анализ уязвимого файла с помощью OWASP ZAP

<img width="1138" height="366" alt="image" src="https://github.com/user-attachments/assets/cf13a972-3ab5-40a8-805d-0bcafba73fd4" />
<img width="299" height="140" alt="image" src="https://github.com/user-attachments/assets/c8b76e61-ff20-4422-86a6-3e18a6c9cb9f" />
<img width="507" height="266" alt="image" src="https://github.com/user-attachments/assets/f7ff5cb1-7a0f-42fa-8628-46c2f6d737f8" />
<img width="684" height="302" alt="image" src="https://github.com/user-attachments/assets/2c5e435d-59aa-49ff-8e78-8ca0c6082f8c" />
<img width="806" height="334" alt="image" src="https://github.com/user-attachments/assets/51850b28-e2ad-4139-b468-e4293676a466" />

## Практическая часть (триаж уязвимостей)

<img width="1355" height="869" alt="image" src="https://github.com/user-attachments/assets/1616aa7d-2086-4e37-a43c-46dfd6f3cffc" />
<img width="1920" height="910" alt="image" src="https://github.com/user-attachments/assets/14d6a2bb-e2fa-4a5d-a05a-27996203332d" />

### Анализ найденных endpoints:

#### 1.Основные уязвимые директории:

**FTP директория (критическая уязвимость):**

http://localhost:3000/ftp/

**Найдены файлы:**

acquisitions.md — информация о поглощениях

announcement_encrypted.md — зашифрованное объявление

coupons_2013.md.bak — резервная копия купонов

eastere.gg — пасхальное яйцо

encrypt.pyc — скомпилированный Python-скрипт

incident-support.kdbx — база данных KeePass

legal.md — юридические документы

package-lock.json.bak — резервная копия зависимостей

suspicious_errors.yml — подозрительные ошибки

**Уязвимость:** Directory Listing Enabled — можно перечислить все файлы в директории.

**Quarantine (карантин):**

http://localhost:3000/ftp/quarantine/

juicy_malware_linux_amd_64.url

juicy_malware_linux_arm_64.url

juicy_malware_macos_64.url

juicy_malware_windows_64.exe.url

**Уязвимость:** Path Traversal — доступ к карантинным файлам.

#### 2.Статические файлы (менее критично):

http://localhost:3000/robots.txt

http://localhost:3000/sitemap.xml

http://localhost:3000/styles.css

http://localhost:3000/main.js

#### 3.Внешние зависимости (CDN):

http://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.4/jquery.min.js

**Проблема:** Устаревшая версия jQuery (2.2.4) содержит уязвимости.

#### 4.Информационное раскрытие:

http://localhost:3000/juice-shop/build/routes/fileServer.js:59:18

**Уязвимость:** Stack Trace Disclosure — показываются пути к исходному коду.

### Уязвимости

#### Критические (High):

1.**Directory Traversal + Information Disclosure**

URL: http://localhost:3000/ftp/

Описание: Возможность перечисления файлов в директории

CVSS: 7.5 (High)

Эксплуатация: http://localhost:3000/ftp/../../../etc/passwd

**2.Exposed Backup Files**

Файлы: *.bak, *.md.bak, package-lock.json.bak

Риск: Раскрытие конфиденциальной информации, исходного кода

CVSS: 6.5 (Medium)

**3.Outdated jQuery Library**

Библиотека: jQuery 2.2.4

Известные уязвимости: XSS, Prototype Pollution

CVSS: 6.1 (Medium)

#### Средние (Medium):

**1.Stack Trace Disclosure**

URL: Показывает пути к файлам

Риск: Раскрытие структуры приложения

CVSS: 4.0 (Medium)

**2.Exposed Legal/Internal Documents**

Файлы: legal.md, acquisitions.md

Риск: Информационное раскрытие

CVSS: 3.5 (Low)

## Сводная таблица

![Uploading image.png…]()

## Ответы на контрольные вопросы

**1.В чем принципиальное отличие SAST от DAST?**

SAST (Static Application Security Testing) — анализ исходного кода без его выполнения ("белый ящик").

DAST (Dynamic Application Security Testing) — тестирование работающего приложения через взаимодействие с ним ("черный ящик").

**3.Какие типы уязвимостей лучше обнаруживает SAST?**

SAST лучше обнаруживает уязвимости, связанные с логикой кода и структурой приложения:

Уязвимости в бизнес-логике

Hardcoded credentials (секреты в коде)

SQL Injection (в исходном коде)

Buffer Overflows (переполнение буфера)

Insecure Deserialization (небезопасная десериализация)

Code Quality Issues (качество кода)

Weak Cryptography (слабая криптография)

Race Conditions (состояния гонки)

Missing Input Validation (отсутствие валидации)

Backdoors и Easter Eggs

**3.Какие типы уязвимостей лучше обнаруживает DAST?**

DAST лучше обнаруживает runtime уязвимости и проблемы конфигурации:

Runtime Injection Attacks (SQLi, XSS, Command Injection в работающем приложении)

Authentication/Authorization Issues (проблемы аутентификации/авторизации)

Session Management Flaws (управление сессиями)

Configuration Issues (конфигурационные ошибки сервера)

Server Security Misconfigurations (ошибки настройки сервера)

Cross-Site Request Forgery (CSRF)

Server-Side Request Forgery (SSRF)

XXE (XML External Entity)

Path Traversal (обход пути)

Information Disclosure (раскрытие информации через ошибки)

**4.Почему важно использовать оба метода в процессе разработки?**

Оба метода дополняют друг друга: SAST находит кодные ошибки на ранних стадиях, DAST проверяет реальное поведение в среде выполнения.

**5.Какие ограничения имеют каждый из методов?**

*Ограничения SAST:*

Ложные срабатывания (false positives) — до 50%

Не видит runtime проблемы — только статический анализ

Требует исходный код — не работает с бинарниками без исходников

Не понимает контекст — не знает как используется код

Сложность с библиотеками — может не анализировать зависимости

Производительность — медленный анализ больших проектов

Не находит конфигурационные ошибки

*Ограничения DAST:*

Позднее обнаружение — после развертывания

Неполное покрытие кода — только доступные endpoints

Не видит логику кода — только внешнее поведение

Сложность с аутентификацией — если нужна сложная auth

Может сломать приложение — агрессивное тестирование

Не находит backdoors в коде — только эксплуатируемые уязвимости

Зависит от среды тестирования

**6.Что такое AppSec?**

AppSec (Application Security) — это практика защиты приложений на протяжении всего их жизненного цикла от угроз безопасности.

**7.Что такое триаж уязвимостей и каковы его основные цели?**

Триаж уязвимостей (от франц. triage — "сортировка") — процесс анализа, оценки и приоритизации найденных уязвимостей.

Цели: оптимизация ресурсов, фокус на критических угрозах.

**8.Чем простое сканирование на уязвимости отличается от полноценного процесса триажа?**

Сканирование просто находит уязвимости, триаж добавляет анализ риска, бизнес-контекста и приоритезацию исправлений.

**9.Почему автоматический сканер не может полностью заменить человека в процессе триажа? Какие его ограничения компенсирует аналитик?**

Автосканер не понимает контекст, бизнес-логику, дает ложные срабатывания. Аналитик оценивает реальный риск и эксплуатацию.

**10.Почему важно документировать решение об отложении исправления уязвимости с низким приоритетом?**

Документирование уязвимостей с низким приоритом необходимо для аудита, а также для обоснования решений перед руководством.
