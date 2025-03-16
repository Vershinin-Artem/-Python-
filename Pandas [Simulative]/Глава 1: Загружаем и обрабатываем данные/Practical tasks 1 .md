# Практические задания 
**Задание №1**

Загрузите этот файл в датафрейм Pandas (с именем df). Затем убедитесь, что:

- каждая переменная считалась в отдельный столбец
- все столбцы названы верно
- нет лишних строк
- затем сохраните в переменную df2 только последние 3 строки считанного датафрейма.

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
df2 = df.tail(3)
```
---

**Задание №2**

Загрузите этот файл в датафрейм Pandas (с именем df). Сформируйте словарь params, который будет содержать информацию о датафрейме:

- список имен столбцов

- размер датафрейма

- форма датафрейма

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
params = {
    'Имена столбцов': ['id', 'username', 'date_joined'],
    'Размер': 300,
    'Форма': (100, 3)
}
```
---

**Задание №3**

Создайте переменную are_types_equal, в которую запишите результат сравнения типа данных у столбцов username и date_joined.

``` Python
import pandas as pd

df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)

are_types_equal = (df['username'].dtype == df['date_joined'].dtype)
```
---

**Задание №4**

Создайте (или переопределите) переменную df таким образом, чтобы она содержала столбцы с названиями:


- user_id (ранее - id)

- created_at (ранее - date_joined)

- login (ранее - username)

Столбцы должны идти в заданном порядке и иметь именно такие названия.

``` Python
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
df.columns= ['user_id','login','created_at']
col = ['user_id','created_at','login']
df = df[col]
```
---

**Задание №5**

При выгрузке данных с нашего сервера произошли небольшие сбои, которые мы не успели пофиксить:


у некоторых имен пользователей в разные места логина добавились значки долларов

некоторые имена пользователей не прогрузились вообще

Вам необходимо создать (или переопределить) переменную df таким образом, чтобы она содержала исходный датафрейм, в котором нет строк с пропусками, а все знаки долларов просто удалены.

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
df['username'] = df['username'].str.replace('$', '')
df = df.dropna()
```
---

**Задание №6**

Загрузите этот файл в датафрейм Pandas (с именем df). Однако, будьте осторожны - перед отправкой кода на проверку посмотрите на свой результат и убедитесь, что вы все загрузили корректно.

Сформируйте список dates, который будет содержать уникальные даты регистрации пользователей в формате YYYY-MM-DD.

``` Python
import pandas as pd

df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)

dates = list(pd.to_datetime(df['date_joined']).dt.date.unique())
dates = [dt.strftime("%Y-%m-%d") for dt in dates]
```
---

**Задание №7**

Создайте словарь registrations_by_weekday, который будет содержать информацию о дне недели и количестве зарегистрированных в этот день пользователей. Элементы словаря должны быть отсортированы по убыванию количества дней.

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
from collections import Counter
import pandas as pd

df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)

days = pd.to_datetime(df['date_joined']).dt.day_name()
registrations_by_wd = dict(Counter(days))

registrations_by_weekday = dict(sorted(registrations_by_wd.items(), key=lambda x: x[1], reverse=True))
```
---
