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

<img width="437" height="310" alt="image" src="https://github.com/user-attachments/assets/0b95e683-d303-423f-b694-d54ebb65d565" />


<img width="420" height="124" alt="image" src="https://github.com/user-attachments/assets/f7818308-42c3-46c1-9b43-c241a94e164c" />


<img width="266" height="108" alt="image" src="https://github.com/user-attachments/assets/47713f80-d610-44ad-a01c-7715bc2b43e8" />


<img width="242" height="169" alt="image" src="https://github.com/user-attachments/assets/dafad57c-4ec6-4f46-be0b-10af9b0184e9" />

<img width="379" height="103" alt="image" src="https://github.com/user-attachments/assets/15c20010-6dd8-4f8c-89e2-c37c8d6fa875" />


<img width="426" height="248" alt="image" src="https://github.com/user-attachments/assets/72bb8e61-bccd-4383-b322-61476d7a7352" />


<img width="165" height="107" alt="image" src="https://github.com/user-attachments/assets/ed9c53c6-d35a-40ef-ad84-ca0e34976fe8" />



