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

