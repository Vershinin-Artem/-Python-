# Практические задания 
**Задание №1**

Загрузите этот файл в датафрейм Pandas (с именем df). Однако, будьте осторожны - перед отправкой кода на проверку посмотрите на свой результат и убедитесь, что вы все загрузили корректно.

Создайте столбец weekday, в котором будет храниться день недели в русском варианте (Понедельник, Вторник, ...)
Создайте столбец date, в котором будет храниться дата регистрации пользователя (без времени)
Создайте датафрейм res, который получится в результате группировки исходного датафрейма по дням недели и расчета:

Количества регистраций в этот день недели
Количества уникальных дат, в которые были регистрации для этого дня недели

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
df['weekday'] = pd.to_datetime(df['date_joined']).dt.weekday
d = {0 : 'Понедельник',
     1 : 'Вторник',
     2 : 'Среда',
     3 : 'Четверг',
     4 : 'Пятница',
     5 : 'Суббота',
     6 : 'Воскресенье'}
df['weekday'] = df['weekday'].map(d)
df['date'] = pd.to_datetime(df['date_joined']).dt.date
res = df.groupby(['weekday'])[['date']].agg({'date': ['count', 'nunique']}).reset_index()
```
---

**Задание №2**

Загрузите этот файл в датафрейм Pandas (с именем df). Отфильтруйте только тех пользователей, у которых id >= 94. Пользователи с меньшим id - фейковые, они сбивают статистику.
Также уберите пользователей с одним из этих ников: 'fnoxonsnbkbxsnb', 'Jonns21241412', 'fbxbnsn'. Это также фейковые юзеры.

Создайте столбец cohort, который будет содержать год и месяц регистрации в формате YYYY-MM. 
Создайте переменную res, которая будет содержать долю каждой когорты в общем количестве зарегистрированных пользователей. В сумме все доли должны давать 100.

``` Python
import pandas as pd
df = pd.read_csv('itresume-users-pandas.csv', sep=';', skiprows=1)
df = df[(df['id'] >= 94) & (~df['username'].isin(['fnoxonsnbkbxsnb', 'Jonns21241412', 'fbxbnsn']))]
df['cohort'] = pd.to_datetime(df['date_joined']).dt.strftime('%Y-%m')
res = df.cohort.value_counts(normalize=True)*100
```
---

**Задание №3**

Загрузите этот файл в датафрейм Pandas и назовите его coderun. 


Создайте столбец cohort, который будет содержать год и месяц регистрации в формате YYYY-MM. 

Создайте столбец language, который проводит соответствие между языком и language_id. 

А теперь давайте посчитаем - в скольких задачах в среднем один пользователь отправлял на выполнение код в каждой когорте по каждому языку.

Результатом должна стать «широкая» таблица, где:

- строки - когорты, но только после января 2022 года (включительно)
- столбцы - языки
- значения - число задач, по которым пользователи в данной когорте отправляют код на выполнение по данному языку в среднем

Результат сохраните в датафрейм df.

``` Python
import pandas as pd
import numpy as np
df_map = {3 : 'Python', 2: 'SQL'}

coderun = pd.read_csv('itresume-coderun.csv')
coderun['cohort'] = pd.to_datetime(coderun['created_at']).dt.strftime('%Y-%m')
coderun['language'] = coderun['language_id'].map(df_map)

df = (coderun[coderun['cohort'] >= '2022-01']
      .groupby(['cohort', 'language', 'user_id'])['problem_id']
      .agg('nunique')
      .groupby(['cohort', 'language'])
      .agg(np.mean)
      .reset_index()
      .pivot_table(values='problem_id', columns=['language'], index=['cohort']))
```
---


**Задание №4**

Загрузите файл в датафрейм Pandas и назовите его coderun. Далее создайте 2 переменные:

problem_id - список задач, которые нужно исключить. Значения: 1, 2, 3, 13, 15, 26.

language_id - номер языка, который нужно оставить. Значение: 2.

С помощью query отфильтруйте coderun по этим двум переменным. Результат сохраните в датафрейм df.

``` Python
import pandas as pd
coderun = pd.read_csv('itresume-coderun.csv')

problem_id = [1, 2, 3, 13, 15, 26]
language_id = 2

df = coderun.query('problem_id != @problem_id and language_id == @language_id')
```
