# Fitness club data research

## Цель проекта: 
Оценить динамику пользовательской базы, активность клиентов и финансовые результаты сети фитнес-клубов, определить влияние маркетинговой акции и особенности ключевых клиентских сегментов.

## Что сделал:
* Провёл анализ данных о клиентах и платежах.
* Рассчитал средний чек, выручку и жизненную ценность клиента (LTR).
* Оценил эффект от маркетинговой акции и удержание клиентов по когортам.
* Сегментировал пользователей по возрасту и городам, выявив различия в поведении.
* Подготовил визуализации и выводы для бизнеса.

## Навыки и инструменты: 
* Работа с Python (pandas, numpy, matplotlib, seaborn) для анализа и визуализации данных.
* Подготовка и очистка данных, агрегирование, построение пользовательских функций и аналитических метрик.
* Проведение когортного анализа, расчёт LTR/LT и интерпретация бизнес-показателей.
* Оформление анализа и визуализаций в Jupyter Notebook с логичными выводами и рекомендациями.
* Развитие аналитического мышления и умения формулировать выводы на основе данных.

## Результат: 
Сделал аналитический отчёт, отражающий динамику клиентской базы и выручки, выявил факторы удержания и сегменты с наибольшим потенциалом роста. Проект демонстрирует владение инструментами Python и способность применять аналитические методы для решения реальных бизнес-задач.

# Jupiter Notebook:
## Техническое задание 1: анализ данных о посетителях фитнес-клуба
Работаем со списком возрастов посетителей фитнес-клубов age_lst

**Задача 1.** Рассчитайте средний возраст по всему списку age_lst

**Задача 2.** Рассчитайте средний возраст для посетителей, которые моложе 50 лет

**Задача 3.** Оберните предыдущий код в функцию, которая на вход принимает список и пороговое значение

**Задача 4.** Рассчитайте среднее значение для всех элементов списка, за исключением тех, которые лежат вне интервала [Q1-1,5*IQR; Q3+1,5*IQR]

**Задача 5.** Оберните предыдущий код в функцию, которая принимает на вход список и рассчитывает средний возраст, исключая из расчётов выбросы


```python
age_lst = [23, 45, 18, 80, 22, 25, 34, 37, 41, 88, 19, 20, 18, 45, 41, 39, 31, 32, 22, 28, 37]
```

### Задача 1. Рассчитайте средний возраст по всему списку age_lst


```python
import numpy as np
```


```python
round(np.mean(age_lst).item(), 1)
```


```python
round(np.median(age_lst).item(), 1)
```


```python
avg_age = sum(age_lst) / len(age_lst)
print(f"Средний возраст посетителей {round(avg_age, 1)} лет")
print(f"Медианный возраст посетителей {round(np.median(age_lst).item(), 1)} лет")
```

### Задача 2. Рассчитайте средний возраст для посетителей, которые моложе 50 лет


```python
sum_user = 0
cnt_user = 0
for i in age_lst:
    if i < 50:
        sum_user += i
        cnt_user += 1
avg_age = sum_user / cnt_user
print(f"Средний возраст посетителей младше 50 лет = {round(avg_age, 1)} года")
```

### Задача 3. Оберните предыдущий код в функцию, которая на вход принимает список и пороговое значение


```python
def avg_age_calc(lst, thr):
    sum_user = 0
    cnt_user = 0
    for i in lst:
        try:
            if i < thr:
                sum_user += i
                cnt_user += 1
        except TypeError:
            print(f"В списке есть нечисловое значение {i}")
    try:
        avg_age = sum_user / cnt_user
        return round(avg_age, 1)
    except:
        print(f"В списке нет возраста меньше {thr}")
```


```python
avg_age_calc(age_lst, 40)
```

### Задача 4. Рассчитайте среднее значение для всех элементов списка, за исключением тех, которые лежат вне интервала [Q1-1,5*IQR; Q3+1,5*IQR]


```python
q1 = np.percentile(age_lst, 25)
q2 = np.percentile(age_lst, 50)
q3 = np.percentile(age_lst, 75)
print(q1, q2, q3)
```


```python
iqr = q3 - q1
print(q1 - 1.5 * iqr)
print(q3 + 1.5 * iqr)
```


```python
sum_user = 0
cnt_user = 0
for i in age_lst:
    if (i < q3 + 1.5 * iqr) and (i > q1 - 1.5 * iqr):
        sum_user += i
        cnt_user += 1
avg_age = sum_user / cnt_user
print(f"Средний возраст посетителей внутри интервала до порога выбросов значений = {round(avg_age, 1)} года")
```



### Задача 5. Оберните предыдущий код в функцию, которая принимает на вход список и рассчитывает средний возраст, исключая из расчётов выбросы


```python
def calc_avg_IQR(lst):
    try:
        q1 = np.percentile(age_lst, 25)
        q3 = np.percentile(age_lst, 75)
        iqr = q3 - q1
        sum_user = 0
        cnt_user = 0
        for i in age_lst:
            if (i < q3 + 1.5 * iqr) and (i > q1 - 1.5 * iqr):
                sum_user += i
                cnt_user += 1
        avg_age = sum_user / cnt_user
        return round(avg_age, 1)
    except:
        print('Нечисловые значения в списке или пустой список')
```


```python
calc_avg_IQR(age_lst)
```


## Техническое задание 2: анализ фин.показателей и маркетинговой акции
**Задача 1.** Изучите таблицу users_info, содержащую информацию о пользователях. Провести разведочный анализ: проверить типы данных, очистить от пропусков, найти и убрать выбросы по возрасту (с помощью IQR). Построить визуализацию распределения возрастов.

**Задача 2.** Проанализируйте распределение пользователей по полу и городам:
+ Рассчитайте количество пользователей по каждому полу и городу.
+ Используйте pivot_table, чтобы представить данные:
1) города в строках,
2) пол в столбцах.
+ Для каждого города рассчитайте долю женщин среди всех пользователей.
+ Постройте столбчатую диаграмму (bar chart), чтобы сравнить долю женщин в разных городах.Какой город отличается от остальных?

**Задача 3.** Сегментация.
Загрузите таблицу payments_monthly.csv с платежами, очищенными от выбросов и сгруппированными по месяцам.
Рассчитайте среднее количество тренировок в месяц для каждого пользователя.
+ Разделите пользователей на три группы по уровню активности:
1) мало (редко тренируются),
2) средне,
3) много (тренируются часто).
+ Посчитайте, сколько пользователей входит в каждую из этих групп.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

```python
users_info = pd.read_csv('users_info.csv')
users_info.head()
```
<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1003</td>
      <td>Москва</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
  </tbody>
</table>
</div>

### Задача 1. EDA 
Изучите таблицу users_info, содержащую информацию о пользователях. Провести разведочный анализ: проверить типы данных, очистить от пропусков, найти и убрать выбросы по возрасту (с помощью IQR). Построить визуализацию распределения возрастов.

```python
users_info.shape
```




    (1000, 4)




```python
users_info.dtypes
```




    id_user      int64
    city        object
    age        float64
    gender      object
    dtype: object




```python
users_info.isnull().sum()
```




    id_user     0
    city        0
    age        88
    gender     89
    dtype: int64




```python
# 89 нулов на 1000
# Удаляю все нулы
users_info_without_nulls = users_info.dropna()
users_info_without_nulls.head(30)
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1005</td>
      <td>СПб</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1006</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1007</td>
      <td>Москва</td>
      <td>58.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1008</td>
      <td>СПб</td>
      <td>53.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1009</td>
      <td>Москва</td>
      <td>22.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1010</td>
      <td>Москва</td>
      <td>47.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1011</td>
      <td>СПб</td>
      <td>22.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1012</td>
      <td>Москва</td>
      <td>53.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1013</td>
      <td>Москва</td>
      <td>43.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1014</td>
      <td>СПб</td>
      <td>59.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1015</td>
      <td>СПб</td>
      <td>41.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1016</td>
      <td>Москва</td>
      <td>25.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1017</td>
      <td>СПб</td>
      <td>20.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1018</td>
      <td>Екатеринбург</td>
      <td>19.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1019</td>
      <td>Москва</td>
      <td>40.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1020</td>
      <td>Екатеринбург</td>
      <td>64.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1021</td>
      <td>СПб</td>
      <td>63.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1022</td>
      <td>Москва</td>
      <td>26.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1023</td>
      <td>Москва</td>
      <td>29.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1024</td>
      <td>Казань</td>
      <td>59.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1025</td>
      <td>Москва</td>
      <td>37.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1026</td>
      <td>Москва</td>
      <td>37.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1027</td>
      <td>Москва</td>
      <td>25.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1029</td>
      <td>СПб</td>
      <td>42.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>30</th>
      <td>1030</td>
      <td>Екатеринбург</td>
      <td>18.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>31</th>
      <td>1031</td>
      <td>Екатеринбург</td>
      <td>54.0</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
</div>




```python
# IQR по age
Q1 = np.percentile(users_info_without_nulls['age'], 25)
Q3 = np.percentile(users_info_without_nulls['age'], 75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5*IQR
upper_bound = Q3 + 1.5*IQR
print(IQR)
print(lower_bound)
print(upper_bound)
print(users_info_without_nulls['age'].mean())
```

    23.0
    -6.5
    85.5
    39.41965317919075
   
```python
users_info_without_nulls['age'].describe()
```

    count    865.000000
    mean      39.419653
    std       13.774501
    min       18.000000
    25%       28.000000
    50%       37.000000
    75%       51.000000
    max       65.000000
    Name: age, dtype: float64

```python
# удаляю выбросы из таблицы
users_info_normalized = users_info_without_nulls.loc[(users_info_without_nulls['age'] < upper_bound) & (users_info_without_nulls['age'] > lower_bound)].copy()
users_info_normalized
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1005</td>
      <td>СПб</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>993</th>
      <td>1993</td>
      <td>СПб</td>
      <td>36.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>994</th>
      <td>1994</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>995</th>
      <td>1995</td>
      <td>Москва</td>
      <td>43.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>997</th>
      <td>1997</td>
      <td>СПб</td>
      <td>58.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>999</th>
      <td>1999</td>
      <td>Екатеринбург</td>
      <td>52.0</td>
      <td>male</td>
    </tr>
  </tbody>
</table>
<p>865 rows × 4 columns</p>
</div>

```python
# распределение возрастов пользователей (гистограмма)
sns.histplot(users_info_normalized['age'], kde = True)
```




    <Axes: xlabel='age', ylabel='Count'>



<img width="571" height="432" alt="output_11_1" src="https://github.com/user-attachments/assets/f8b35697-94a0-439e-990e-182ce29d3c33" />
    

### Задача 2. Проанализируйте распределение пользователей по полу и городам.

```python
users_city = users_info_normalized.groupby('city').agg(cnt_users = ('id_user', 'count')).reset_index()
users_city
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>cnt_users</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Екатеринбург</td>
      <td>133</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Казань</td>
      <td>130</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Москва</td>
      <td>325</td>
    </tr>
    <tr>
      <th>3</th>
      <td>СПб</td>
      <td>277</td>
    </tr>
  </tbody>
</table>
</div>

```python
users_gender = users_info_normalized.groupby('gender').agg(cnt_users = ('id_user', 'count')).reset_index()
users_gender
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>cnt_users</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>451</td>
    </tr>
    <tr>
      <th>1</th>
      <td>male</td>
      <td>414</td>
    </tr>
  </tbody>
</table>
</div>

```python
users_city_gender = users_info_normalized.groupby(['city', 'gender']).agg(cnt_users = ('id_user', 'count')).reset_index()
users_city_gender
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>gender</th>
      <th>cnt_users</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Екатеринбург</td>
      <td>female</td>
      <td>55</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>78</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Казань</td>
      <td>female</td>
      <td>53</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Казань</td>
      <td>male</td>
      <td>77</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Москва</td>
      <td>female</td>
      <td>220</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Москва</td>
      <td>male</td>
      <td>105</td>
    </tr>
    <tr>
      <th>6</th>
      <td>СПб</td>
      <td>female</td>
      <td>123</td>
    </tr>
    <tr>
      <th>7</th>
      <td>СПб</td>
      <td>male</td>
      <td>154</td>
    </tr>
  </tbody>
</table>
</div>

```python
pivot_users = pd.pivot_table(users_city_gender, \
                             values = 'cnt_users', \
                             index = 'city', \
                             columns = 'gender', \
                             aggfunc = 'sum', \
                             fill_value = 0).reset_index()
pivot_users
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>gender</th>
      <th>city</th>
      <th>female</th>
      <th>male</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Екатеринбург</td>
      <td>55</td>
      <td>78</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Казань</td>
      <td>53</td>
      <td>77</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Москва</td>
      <td>220</td>
      <td>105</td>
    </tr>
    <tr>
      <th>3</th>
      <td>СПб</td>
      <td>123</td>
      <td>154</td>
    </tr>
  </tbody>
</table>
</div>




```python
pivot_users.columns = ['city', 'cnt_female', 'cnt_male']
pivot_users['total'] = pivot_users['cnt_female'] + pivot_users['cnt_male']
pivot_users['female_share'] = pivot_users ['cnt_female'] / pivot_users['total']
pivot_users
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>cnt_female</th>
      <th>cnt_male</th>
      <th>total</th>
      <th>female_share</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Екатеринбург</td>
      <td>55</td>
      <td>78</td>
      <td>133</td>
      <td>0.413534</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Казань</td>
      <td>53</td>
      <td>77</td>
      <td>130</td>
      <td>0.407692</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Москва</td>
      <td>220</td>
      <td>105</td>
      <td>325</td>
      <td>0.676923</td>
    </tr>
    <tr>
      <th>3</th>
      <td>СПб</td>
      <td>123</td>
      <td>154</td>
      <td>277</td>
      <td>0.444043</td>
    </tr>
  </tbody>
</table>
</div>


```python
plt.figure(figsize=(8, 6))
plt.bar(pivot_users['city'], pivot_users['female_share'], color='skyblue', edgecolor='navy')
plt.title('Доля женского пола')
plt.xlabel('Город')
plt.ylabel('Доля женщин')
plt.grid(axis='y', alpha=0.3)
plt.show()
```

<img width="691" height="545" alt="output_18_0" src="https://github.com/user-attachments/assets/a5452910-1982-49a0-8de9-44ac7894ebdb" />



```python
# Только в Москве женщин больше чем мужчин, значительно
```

### Задача 3. Сегментация - делю пользователей на бины по возрасту

```python
pay_mon = pd.read_csv('payments_monthly.csv')
pay_mon.head()
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
      <th>cnt_group</th>
      <th>cnt_indiv</th>
      <th>sum_group</th>
      <th>sum_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>2023-03</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3600</td>
      <td>2</td>
      <td>3600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000</td>
      <td>2023-04</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000</td>
      <td>2023-05</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>6400</td>
      <td>4</td>
      <td>6400</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1000</td>
      <td>2023-06</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3600</td>
      <td>2</td>
      <td>3600</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1000</td>
      <td>2023-07</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
    </tr>
  </tbody>
</table>
</div>

```python
avg_workouts = round(pay_mon.groupby('id_user')['cnt_total'].mean().reset_index(), 1)
avg_workouts.columns = ['id_user', 'mean_workouts_month']
avg_workouts
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mean_workouts_month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>3.7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>11.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>11.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1003</td>
      <td>5.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>964</th>
      <td>1994</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>965</th>
      <td>1995</td>
      <td>11.5</td>
    </tr>
    <tr>
      <th>966</th>
      <td>1996</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>967</th>
      <td>1997</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>968</th>
      <td>1998</td>
      <td>4.4</td>
    </tr>
  </tbody>
</table>
<p>969 rows × 2 columns</p>
</div>

```python
avg_workouts.describe()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mean_workouts_month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>969.000000</td>
      <td>969.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1500.323013</td>
      <td>6.343240</td>
    </tr>
    <tr>
      <th>std</th>
      <td>289.124265</td>
      <td>3.540042</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1250.000000</td>
      <td>3.500000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1501.000000</td>
      <td>5.800000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1752.000000</td>
      <td>8.800000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1998.000000</td>
      <td>18.000000</td>
    </tr>
  </tbody>
</table>
</div>

```python
bins = [0, 5, 10, float('inf')]
labels = ['мало', 'средне', 'много']
avg_workouts['intensity'] = pd.cut(avg_workouts['mean_workouts_month'], bins=bins, labels=labels, right=True)
avg_workouts.head(10)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mean_workouts_month</th>
      <th>intensity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>3.7</td>
      <td>мало</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>11.7</td>
      <td>много</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>11.6</td>
      <td>много</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1003</td>
      <td>5.2</td>
      <td>средне</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>2.0</td>
      <td>мало</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1005</td>
      <td>3.5</td>
      <td>мало</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1006</td>
      <td>9.8</td>
      <td>средне</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1007</td>
      <td>1.2</td>
      <td>мало</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1008</td>
      <td>5.5</td>
      <td>средне</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1009</td>
      <td>2.2</td>
      <td>мало</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_workouts_gr = avg_workouts.groupby('intensity', observed=False).agg(users_by_avg_workouts_cnt = ('id_user', 'count')).reset_index()
avg_workouts_gr

#observed = False дописал из-за появившегося предупреждения об обновлении pandas, он по дефолту станет True в новой версии)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>intensity</th>
      <th>users_by_avg_workouts_cnt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>мало</td>
      <td>407</td>
    </tr>
    <tr>
      <th>1</th>
      <td>средне</td>
      <td>408</td>
    </tr>
    <tr>
      <th>2</th>
      <td>много</td>
      <td>154</td>
    </tr>
  </tbody>
</table>
</div>

## Техническое задание 3: анализ сегментов и клиентской базы
Продолжение работы с объединенным датасетом merged_data

**Задача 1.** Выделить пользователей, которые делают упор на индивидуальные тренировки.
Выберите пользователей, у которых >70% тренировок — индивидуальные.
Для этих пользователей:
+ Посчитайте средний чек;
+ Посчитайте среднее число тренировок в месяц;
+ Разбейте их по городам и полу.

Вопросы для анализа:
+ Кто чаще выбирает индивидуальные тренировки?
+ Есть ли зависимость от пола или города?
+ Насколько высок средний чек у таких пользователей?

**Задача 2.** Найдите 10 пользователей с наибольшим общим количеством посещений (групповые + индивидуальные тренировки) за весь период наблюдения.

Что нужно сделать:
+ Рассчитайте общее количество тренировок для каждого пользователя.
+ Определите 10 самых активных пользователей и сохраните их идентификаторы в виде списка.

Постройте распределение этих пользователей по:
+ городу
+ полу

Вопросы для анализа:
+ В каких городах больше всего супер-активных клиентов?
+ Какого они пола?

**Задача 3.** Проанализировать, как менялась клиентская база по месяцам: сколько клиентов приходило, сколько уходило и сколько оставалось активными.

Шаги:
+ Новые клиенты — это те, у кого первый месяц появления (минимальный mon в данных).
+ Ушедшие клиенты — это те, у кого последний месяц активности (максимальный mon в данных).
+ Активные клиенты — это те, кто был активен в конкретном месяце.

Рассчитайте три метрики для каждого месяца:
+ new_clients: количество клиентов, для которых этот месяц — первый.
+ gone_clients: количество клиентов, для которых этот месяц — последний.
+ active_clients: общее количество уникальных клиентов в этом месяце.

Создайте три отдельных датафрейма:
+ с новыми клиентами по месяцам,
+ с ушедшими клиентами по месяцам,
+ с активными клиентами по месяцам.

Объедините их по полю mon.
Постройте линейный график с тремя линиями.



```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
# загружаем очищенный датасет
df = pd.read_csv('merged_data.csv')
df.head(3)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
      <th>cnt_group</th>
      <th>cnt_indiv</th>
      <th>sum_group</th>
      <th>sum_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
      <th>min_mon</th>
      <th>max_mon</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>2023-03</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3600</td>
      <td>2</td>
      <td>3600</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000</td>
      <td>2023-04</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000</td>
      <td>2023-05</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>6400</td>
      <td>4</td>
      <td>6400</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
</div>



### Задача 1. Пользователи с 70%+ инд.тренировок


```python
users_gr = df.copy()
```


```python
users_gr = users_gr.groupby(['id_user', 'city', 'gender']).agg({'mon':'count',
                                                                'cnt_indiv':'sum',
                                                                'cnt_total':'sum',
                                                                'sum_total':'sum'}). \
                                                           reset_index().rename(columns = {'mon':'nmonths'})
users_gr.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>gender</th>
      <th>nmonths</th>
      <th>cnt_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>female</td>
      <td>9</td>
      <td>33</td>
      <td>33</td>
      <td>58600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>Москва</td>
      <td>female</td>
      <td>11</td>
      <td>23</td>
      <td>129</td>
      <td>124800</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>64</td>
      <td>128</td>
      <td>166000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>9</td>
      <td>18</td>
      <td>18</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>СПб</td>
      <td>female</td>
      <td>2</td>
      <td>7</td>
      <td>7</td>
      <td>12600</td>
    </tr>
  </tbody>
</table>
</div>




```python
users_gr['share_indiv'] = users_gr['cnt_indiv'] / users_gr['cnt_total']
```


```python
indiv_prefs_users = users_gr.loc[users_gr['share_indiv'] > 0.7].copy()
```


```python
indiv_prefs_users.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>gender</th>
      <th>nmonths</th>
      <th>cnt_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
      <th>share_indiv</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>female</td>
      <td>9</td>
      <td>33</td>
      <td>33</td>
      <td>58600</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>9</td>
      <td>18</td>
      <td>18</td>
      <td>32000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>СПб</td>
      <td>female</td>
      <td>2</td>
      <td>7</td>
      <td>7</td>
      <td>12600</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1007</td>
      <td>Москва</td>
      <td>female</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>9000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1009</td>
      <td>Москва</td>
      <td>female</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>16200</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
indiv_prefs_users['avg_check_per_mon'] = indiv_prefs_users['sum_total'] / indiv_prefs_users['nmonths']
indiv_prefs_users['avg_train_per_mon'] = indiv_prefs_users['cnt_total'] / indiv_prefs_users['nmonths']
indiv_prefs_users.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>gender</th>
      <th>nmonths</th>
      <th>cnt_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
      <th>share_indiv</th>
      <th>avg_check_per_mon</th>
      <th>avg_train_per_mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>СПб</td>
      <td>female</td>
      <td>9</td>
      <td>33</td>
      <td>33</td>
      <td>58600</td>
      <td>1.0</td>
      <td>6511.111111</td>
      <td>3.666667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>9</td>
      <td>18</td>
      <td>18</td>
      <td>32000</td>
      <td>1.0</td>
      <td>3555.555556</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>СПб</td>
      <td>female</td>
      <td>2</td>
      <td>7</td>
      <td>7</td>
      <td>12600</td>
      <td>1.0</td>
      <td>6300.000000</td>
      <td>3.500000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1007</td>
      <td>Москва</td>
      <td>female</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>9000</td>
      <td>1.0</td>
      <td>2250.000000</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1009</td>
      <td>Москва</td>
      <td>female</td>
      <td>4</td>
      <td>9</td>
      <td>9</td>
      <td>16200</td>
      <td>1.0</td>
      <td>4050.000000</td>
      <td>2.250000</td>
    </tr>
  </tbody>
</table>
</div>




```python
indiv_stats_city_gender = indiv_prefs_users.groupby(['city', 'gender']).agg({'id_user':'nunique',
                                                                             'avg_check_per_mon':'mean',
                                                                             'share_indiv':'mean',
                                                                             'avg_train_per_mon':'mean'}). \
                                                                        sort_values(by = 'avg_check_per_mon', ascending = False). \
                                                                        reset_index().rename(columns = {'id_user':'nusers'})
indiv_stats_city_gender
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>gender</th>
      <th>nusers</th>
      <th>avg_check_per_mon</th>
      <th>share_indiv</th>
      <th>avg_train_per_mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Москва</td>
      <td>male</td>
      <td>59</td>
      <td>6915.316115</td>
      <td>1.000000</td>
      <td>3.872733</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Казань</td>
      <td>female</td>
      <td>8</td>
      <td>6297.500000</td>
      <td>0.964744</td>
      <td>3.756250</td>
    </tr>
    <tr>
      <th>2</th>
      <td>СПб</td>
      <td>female</td>
      <td>32</td>
      <td>6093.278770</td>
      <td>1.000000</td>
      <td>3.399777</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>12</td>
      <td>5832.394180</td>
      <td>1.000000</td>
      <td>3.256184</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Москва</td>
      <td>female</td>
      <td>118</td>
      <td>5565.425624</td>
      <td>0.997881</td>
      <td>3.141311</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Екатеринбург</td>
      <td>female</td>
      <td>12</td>
      <td>5391.746032</td>
      <td>1.000000</td>
      <td>3.009524</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Казань</td>
      <td>male</td>
      <td>12</td>
      <td>5229.378307</td>
      <td>1.000000</td>
      <td>2.931548</td>
    </tr>
    <tr>
      <th>7</th>
      <td>СПб</td>
      <td>male</td>
      <td>28</td>
      <td>5024.889456</td>
      <td>1.000000</td>
      <td>2.821443</td>
    </tr>
  </tbody>
</table>
</div>




```python
indiv_stats_city = indiv_prefs_users.groupby('city').agg({'id_user':'nunique',
                                                                             'avg_check_per_mon':'mean',
                                                                             'share_indiv':'mean',
                                                                             'avg_train_per_mon':'mean'}). \
                                                                        sort_values(by = 'avg_check_per_mon', ascending = False). \
                                                                        reset_index().rename(columns = {'id_user':'nusers'})
indiv_stats_city
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>nusers</th>
      <th>avg_check_per_mon</th>
      <th>share_indiv</th>
      <th>avg_train_per_mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Москва</td>
      <td>177</td>
      <td>6015.389121</td>
      <td>0.998588</td>
      <td>3.385118</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Казань</td>
      <td>20</td>
      <td>5656.626984</td>
      <td>0.985897</td>
      <td>3.261429</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Екатеринбург</td>
      <td>24</td>
      <td>5612.070106</td>
      <td>1.000000</td>
      <td>3.132854</td>
    </tr>
    <tr>
      <th>3</th>
      <td>СПб</td>
      <td>60</td>
      <td>5594.697090</td>
      <td>1.000000</td>
      <td>3.129888</td>
    </tr>
  </tbody>
</table>
</div>




```python
indiv_stats_gender = indiv_prefs_users.groupby('gender').agg({'id_user':'nunique',
                                                                             'avg_check_per_mon':'mean',
                                                                             'share_indiv':'mean',
                                                                             'avg_train_per_mon':'mean'}). \
                                                                        sort_values(by = 'avg_check_per_mon', ascending = False). \
                                                                        reset_index().rename(columns = {'id_user':'nusers'})
indiv_stats_gender
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>nusers</th>
      <th>avg_check_per_mon</th>
      <th>share_indiv</th>
      <th>avg_train_per_mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>male</td>
      <td>111</td>
      <td>6139.115544</td>
      <td>1.00000</td>
      <td>3.439139</td>
    </tr>
    <tr>
      <th>1</th>
      <td>female</td>
      <td>170</td>
      <td>5686.977039</td>
      <td>0.99687</td>
      <td>3.209599</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(8, 5))
#plt.xlim(0, 1)
plt.ylim(0, 7500)
sns.scatterplot(data = indiv_stats_city,
               x = 'avg_train_per_mon',
               y = 'avg_check_per_mon',
               hue = 'city',
                size='nusers',
                sizes=(50, 300))
plt.xlabel('Среднее число тренировок в месяц')
plt.ylabel('Средний чек в месяц')
plt.title('Метрики по городам')
plt.legend()
plt.grid(True)
plt.show()
```


<img width="704" height="468" alt="output_13_0" src="https://github.com/user-attachments/assets/9543a168-068a-4a83-874f-50fd818f6aec" />
   


### Задача 2. Топ-10 клиентов


```python
users_gr_2 = df.groupby('id_user').agg({'cnt_total':'sum'}).reset_index().sort_values('cnt_total', ascending = False).head(10)
users_gr_2
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>cnt_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>319</th>
      <td>1381</td>
      <td>159</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1013</td>
      <td>151</td>
    </tr>
    <tr>
      <th>551</th>
      <td>1657</td>
      <td>144</td>
    </tr>
    <tr>
      <th>420</th>
      <td>1506</td>
      <td>138</td>
    </tr>
    <tr>
      <th>780</th>
      <td>1929</td>
      <td>130</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>129</td>
    </tr>
    <tr>
      <th>474</th>
      <td>1570</td>
      <td>129</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>128</td>
    </tr>
    <tr>
      <th>485</th>
      <td>1582</td>
      <td>127</td>
    </tr>
    <tr>
      <th>168</th>
      <td>1194</td>
      <td>127</td>
    </tr>
  </tbody>
</table>
</div>




```python
users_gr_2_lst = users_gr_2['id_user'].tolist()
users_gr_2_lst
```




    [1381, 1013, 1657, 1506, 1929, 1001, 1570, 1002, 1582, 1194]




```python
top_10_by_train_cnt = df[df['id_user'].isin(users_gr_2_lst)]
top_10_by_train_cnt.head()
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
      <th>cnt_group</th>
      <th>cnt_indiv</th>
      <th>sum_group</th>
      <th>sum_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
      <th>min_mon</th>
      <th>max_mon</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>9</th>
      <td>1001</td>
      <td>2023-01</td>
      <td>12</td>
      <td>2</td>
      <td>9600</td>
      <td>3600</td>
      <td>14</td>
      <td>13200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1001</td>
      <td>2023-02</td>
      <td>11</td>
      <td>3</td>
      <td>8800</td>
      <td>5400</td>
      <td>14</td>
      <td>14200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1001</td>
      <td>2023-03</td>
      <td>8</td>
      <td>6</td>
      <td>6400</td>
      <td>10800</td>
      <td>14</td>
      <td>17200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1001</td>
      <td>2023-04</td>
      <td>9</td>
      <td>5</td>
      <td>7200</td>
      <td>9000</td>
      <td>14</td>
      <td>16200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1001</td>
      <td>2023-05</td>
      <td>3</td>
      <td>7</td>
      <td>2400</td>
      <td>11200</td>
      <td>10</td>
      <td>13600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_10_by_train_cnt_gr = top_10_by_train_cnt.groupby(['id_user', 'city', 'gender']). \
                                             agg({'mon':'count',
                                                  'cnt_total':'sum'}).reset_index(). \
                                             rename(columns = {'mon': 'nmonths'})
top_10_by_train_cnt_gr
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>city</th>
      <th>gender</th>
      <th>nmonths</th>
      <th>cnt_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>Москва</td>
      <td>female</td>
      <td>11</td>
      <td>129</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>128</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1013</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>151</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1194</td>
      <td>СПб</td>
      <td>female</td>
      <td>12</td>
      <td>127</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1381</td>
      <td>Москва</td>
      <td>female</td>
      <td>12</td>
      <td>159</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1506</td>
      <td>Москва</td>
      <td>female</td>
      <td>10</td>
      <td>138</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1570</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>11</td>
      <td>129</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1582</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>8</td>
      <td>127</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1657</td>
      <td>СПб</td>
      <td>male</td>
      <td>10</td>
      <td>144</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1929</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>130</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_10_by_train_cnt_gr['avg_train_per_month'] = top_10_by_train_cnt_gr['cnt_total'] / top_10_by_train_cnt_gr['nmonths']
top_10_by_train_cnt_gr.sort_values('avg_train_per_month', ascending = False).reset_index()
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>id_user</th>
      <th>city</th>
      <th>gender</th>
      <th>nmonths</th>
      <th>cnt_total</th>
      <th>avg_train_per_month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>1582</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>8</td>
      <td>127</td>
      <td>15.875000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>1657</td>
      <td>СПб</td>
      <td>male</td>
      <td>10</td>
      <td>144</td>
      <td>14.400000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>1506</td>
      <td>Москва</td>
      <td>female</td>
      <td>10</td>
      <td>138</td>
      <td>13.800000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>1013</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>151</td>
      <td>13.727273</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1381</td>
      <td>Москва</td>
      <td>female</td>
      <td>12</td>
      <td>159</td>
      <td>13.250000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9</td>
      <td>1929</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>130</td>
      <td>11.818182</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0</td>
      <td>1001</td>
      <td>Москва</td>
      <td>female</td>
      <td>11</td>
      <td>129</td>
      <td>11.727273</td>
    </tr>
    <tr>
      <th>7</th>
      <td>6</td>
      <td>1570</td>
      <td>Екатеринбург</td>
      <td>male</td>
      <td>11</td>
      <td>129</td>
      <td>11.727273</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1</td>
      <td>1002</td>
      <td>Москва</td>
      <td>male</td>
      <td>11</td>
      <td>128</td>
      <td>11.636364</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3</td>
      <td>1194</td>
      <td>СПб</td>
      <td>female</td>
      <td>12</td>
      <td>127</td>
      <td>10.583333</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_10_graphics = top_10_by_train_cnt_gr.groupby(['city', 'gender']).size().unstack(fill_value=0)
top_10_graphics.head()
```

<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>gender</th>
      <th>female</th>
      <th>male</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Екатеринбург</th>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Москва</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>СПб</th>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(10, 6))
top_10_graphics.plot(kind='bar', ax=ax)
ax.set_title('Топ 10 по количеству тренировок')
ax.set_ylabel('Количество клиентов')
ax.set_xlabel('Город')
ax.legend(title='Пол')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

<img width="989" height="590" alt="output_21_0" src="https://github.com/user-attachments/assets/f841b5be-7ee4-4be9-9923-04cc9f5cd3cb" />
  

```python
pivot_top_10 = top_10_by_train_cnt_gr.pivot_table(index='city', 
                                                  columns='gender', 
                                                  values='avg_train_per_month', 
                                                  aggfunc='mean', 
                                                  fill_value=0
                                                  )
print(pivot_top_10)
```

    gender           female       male
    city                              
    Екатеринбург   0.000000  13.801136
    Москва        12.925758  12.393939
    СПб           10.583333  14.400000
    


```python
plt.figure(figsize=(12, 8))
sns.heatmap(pivot_top_10, annot = True, fmt = ".1f", cmap = 'YlGnBu')
plt.xlabel('Пол')
plt.ylabel('Город')
plt.title('Тепловая карта топ 10 клиентов')
plt.show()
```

<img width="912" height="699" alt="output_23_0" src="https://github.com/user-attachments/assets/728043ea-1676-4a6e-9d5b-31e2f0dd5fba" />

  

```python
# Самые активные - мужчины в СПб, в Москве немного активнее женщины, в ЕКб только мужчины 
# (анализ основан только на топ-10 посетителях и не включает общую активность по городам и полам)
```

### Задача 3. Динамика клиентской базы


```python
df.head(50)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
      <th>cnt_group</th>
      <th>cnt_indiv</th>
      <th>sum_group</th>
      <th>sum_indiv</th>
      <th>cnt_total</th>
      <th>sum_total</th>
      <th>min_mon</th>
      <th>max_mon</th>
      <th>city</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>2023-03</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3600</td>
      <td>2</td>
      <td>3600</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000</td>
      <td>2023-04</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000</td>
      <td>2023-05</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>6400</td>
      <td>4</td>
      <td>6400</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1000</td>
      <td>2023-06</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3600</td>
      <td>2</td>
      <td>3600</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1000</td>
      <td>2023-07</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1000</td>
      <td>2023-08</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1000</td>
      <td>2023-09</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1000</td>
      <td>2023-11</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>9000</td>
      <td>5</td>
      <td>9000</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1000</td>
      <td>2023-12</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>9000</td>
      <td>5</td>
      <td>9000</td>
      <td>2023-03</td>
      <td>2023-12</td>
      <td>СПб</td>
      <td>44.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1001</td>
      <td>2023-01</td>
      <td>12</td>
      <td>2</td>
      <td>9600</td>
      <td>3600</td>
      <td>14</td>
      <td>13200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1001</td>
      <td>2023-02</td>
      <td>11</td>
      <td>3</td>
      <td>8800</td>
      <td>5400</td>
      <td>14</td>
      <td>14200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1001</td>
      <td>2023-03</td>
      <td>8</td>
      <td>6</td>
      <td>6400</td>
      <td>10800</td>
      <td>14</td>
      <td>17200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1001</td>
      <td>2023-04</td>
      <td>9</td>
      <td>5</td>
      <td>7200</td>
      <td>9000</td>
      <td>14</td>
      <td>16200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1001</td>
      <td>2023-05</td>
      <td>3</td>
      <td>7</td>
      <td>2400</td>
      <td>11200</td>
      <td>10</td>
      <td>13600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1001</td>
      <td>2023-06</td>
      <td>12</td>
      <td>0</td>
      <td>9600</td>
      <td>0</td>
      <td>12</td>
      <td>9600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1001</td>
      <td>2023-07</td>
      <td>9</td>
      <td>0</td>
      <td>7200</td>
      <td>0</td>
      <td>9</td>
      <td>7200</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1001</td>
      <td>2023-08</td>
      <td>12</td>
      <td>0</td>
      <td>9600</td>
      <td>0</td>
      <td>12</td>
      <td>9600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1001</td>
      <td>2023-09</td>
      <td>7</td>
      <td>0</td>
      <td>5600</td>
      <td>0</td>
      <td>7</td>
      <td>5600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1001</td>
      <td>2023-10</td>
      <td>11</td>
      <td>0</td>
      <td>8800</td>
      <td>0</td>
      <td>11</td>
      <td>8800</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1001</td>
      <td>2023-12</td>
      <td>12</td>
      <td>0</td>
      <td>9600</td>
      <td>0</td>
      <td>12</td>
      <td>9600</td>
      <td>2023-01</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1002</td>
      <td>2023-02</td>
      <td>4</td>
      <td>8</td>
      <td>3200</td>
      <td>14400</td>
      <td>12</td>
      <td>17600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1002</td>
      <td>2023-03</td>
      <td>4</td>
      <td>8</td>
      <td>3200</td>
      <td>14400</td>
      <td>12</td>
      <td>17600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1002</td>
      <td>2023-04</td>
      <td>7</td>
      <td>4</td>
      <td>5600</td>
      <td>7200</td>
      <td>11</td>
      <td>12800</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1002</td>
      <td>2023-05</td>
      <td>2</td>
      <td>10</td>
      <td>1600</td>
      <td>16000</td>
      <td>12</td>
      <td>17600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1002</td>
      <td>2023-06</td>
      <td>7</td>
      <td>4</td>
      <td>5600</td>
      <td>7200</td>
      <td>11</td>
      <td>12800</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1002</td>
      <td>2023-07</td>
      <td>8</td>
      <td>4</td>
      <td>6400</td>
      <td>7200</td>
      <td>12</td>
      <td>13600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1002</td>
      <td>2023-08</td>
      <td>5</td>
      <td>7</td>
      <td>4000</td>
      <td>12600</td>
      <td>12</td>
      <td>16600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1002</td>
      <td>2023-09</td>
      <td>8</td>
      <td>3</td>
      <td>6400</td>
      <td>5400</td>
      <td>11</td>
      <td>11800</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1002</td>
      <td>2023-10</td>
      <td>7</td>
      <td>4</td>
      <td>5600</td>
      <td>7200</td>
      <td>11</td>
      <td>12800</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1002</td>
      <td>2023-11</td>
      <td>8</td>
      <td>4</td>
      <td>8000</td>
      <td>7200</td>
      <td>12</td>
      <td>15200</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>30</th>
      <td>1002</td>
      <td>2023-12</td>
      <td>4</td>
      <td>8</td>
      <td>3200</td>
      <td>14400</td>
      <td>12</td>
      <td>17600</td>
      <td>2023-02</td>
      <td>2023-12</td>
      <td>Москва</td>
      <td>34.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>31</th>
      <td>1004</td>
      <td>2023-02</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>32</th>
      <td>1004</td>
      <td>2023-03</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1800</td>
      <td>1</td>
      <td>1800</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>33</th>
      <td>1004</td>
      <td>2023-04</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>34</th>
      <td>1004</td>
      <td>2023-05</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3200</td>
      <td>2</td>
      <td>3200</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1004</td>
      <td>2023-06</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1800</td>
      <td>1</td>
      <td>1800</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>36</th>
      <td>1004</td>
      <td>2023-07</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>37</th>
      <td>1004</td>
      <td>2023-08</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>38</th>
      <td>1004</td>
      <td>2023-09</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1800</td>
      <td>1</td>
      <td>1800</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>39</th>
      <td>1004</td>
      <td>2023-10</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1800</td>
      <td>1</td>
      <td>1800</td>
      <td>2023-02</td>
      <td>2023-10</td>
      <td>Екатеринбург</td>
      <td>60.0</td>
      <td>male</td>
    </tr>
    <tr>
      <th>40</th>
      <td>1005</td>
      <td>2023-07</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>5400</td>
      <td>3</td>
      <td>5400</td>
      <td>2023-07</td>
      <td>2023-10</td>
      <td>СПб</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>41</th>
      <td>1005</td>
      <td>2023-10</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>7200</td>
      <td>4</td>
      <td>7200</td>
      <td>2023-07</td>
      <td>2023-10</td>
      <td>СПб</td>
      <td>35.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>42</th>
      <td>1006</td>
      <td>2023-01</td>
      <td>9</td>
      <td>0</td>
      <td>7200</td>
      <td>0</td>
      <td>9</td>
      <td>7200</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>43</th>
      <td>1006</td>
      <td>2023-02</td>
      <td>12</td>
      <td>0</td>
      <td>9600</td>
      <td>0</td>
      <td>12</td>
      <td>9600</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>44</th>
      <td>1006</td>
      <td>2023-03</td>
      <td>8</td>
      <td>0</td>
      <td>6400</td>
      <td>0</td>
      <td>8</td>
      <td>6400</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>45</th>
      <td>1006</td>
      <td>2023-04</td>
      <td>9</td>
      <td>0</td>
      <td>7200</td>
      <td>0</td>
      <td>9</td>
      <td>7200</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>46</th>
      <td>1006</td>
      <td>2023-08</td>
      <td>9</td>
      <td>0</td>
      <td>7200</td>
      <td>0</td>
      <td>9</td>
      <td>7200</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>47</th>
      <td>1006</td>
      <td>2023-09</td>
      <td>12</td>
      <td>0</td>
      <td>9600</td>
      <td>0</td>
      <td>12</td>
      <td>9600</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>48</th>
      <td>1006</td>
      <td>2023-10</td>
      <td>9</td>
      <td>0</td>
      <td>8000</td>
      <td>0</td>
      <td>9</td>
      <td>8000</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
    <tr>
      <th>49</th>
      <td>1006</td>
      <td>2023-11</td>
      <td>10</td>
      <td>0</td>
      <td>8000</td>
      <td>0</td>
      <td>10</td>
      <td>8000</td>
      <td>2023-01</td>
      <td>2023-11</td>
      <td>Казань</td>
      <td>46.0</td>
      <td>female</td>
    </tr>
  </tbody>
</table>
</div>




```python
users_first_month = df.groupby('id_user')['mon'].min().reset_index()
users_first_month
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>2023-03</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>2023-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>2023-02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2023-02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2023-07</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>833</th>
      <td>1992</td>
      <td>2023-01</td>
    </tr>
    <tr>
      <th>834</th>
      <td>1993</td>
      <td>2023-08</td>
    </tr>
    <tr>
      <th>835</th>
      <td>1994</td>
      <td>2023-10</td>
    </tr>
    <tr>
      <th>836</th>
      <td>1995</td>
      <td>2023-06</td>
    </tr>
    <tr>
      <th>837</th>
      <td>1997</td>
      <td>2023-01</td>
    </tr>
  </tbody>
</table>
<p>838 rows × 2 columns</p>
</div>




```python
new_clients_df = users_first_month.groupby('mon').size().reset_index(name='new_clients')
new_clients_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mon</th>
      <th>new_clients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023-01</td>
      <td>337</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023-02</td>
      <td>88</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023-03</td>
      <td>83</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023-04</td>
      <td>62</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023-05</td>
      <td>57</td>
    </tr>
  </tbody>
</table>
</div>




```python
users_last_months = df.groupby('id_user')['mon'].max().reset_index()
users_last_months.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_user</th>
      <th>mon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>2023-12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>2023-12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>2023-12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2023-10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2023-10</td>
    </tr>
  </tbody>
</table>
</div>




```python
gone_clients_df = users_last_months.groupby('mon').size().reset_index(name='gone_clients')
gone_clients_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mon</th>
      <th>gone_clients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023-01</td>
      <td>15</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023-02</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023-03</td>
      <td>37</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023-04</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023-05</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
active_clients_df = df.groupby('mon')['id_user'].nunique().reset_index(name='active_clients')
active_clients_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mon</th>
      <th>active_clients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023-01</td>
      <td>337</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023-02</td>
      <td>350</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023-03</td>
      <td>355</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023-04</td>
      <td>345</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023-05</td>
      <td>360</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_plus_gone_users_df = pd.merge(new_clients_df, gone_clients_df, on = 'mon', how = 'outer')
new_plus_gone_users_df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mon</th>
      <th>new_clients</th>
      <th>gone_clients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023-01</td>
      <td>337</td>
      <td>15</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023-02</td>
      <td>88</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023-03</td>
      <td>83</td>
      <td>37</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023-04</td>
      <td>62</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023-05</td>
      <td>57</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
monthly_clients = pd.merge(new_plus_gone_users_df, active_clients_df, on = 'mon', how = 'outer')
monthly_clients.head(20)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mon</th>
      <th>new_clients</th>
      <th>gone_clients</th>
      <th>active_clients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2023-01</td>
      <td>337</td>
      <td>15</td>
      <td>337</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2023-02</td>
      <td>88</td>
      <td>22</td>
      <td>350</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2023-03</td>
      <td>83</td>
      <td>37</td>
      <td>355</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2023-04</td>
      <td>62</td>
      <td>21</td>
      <td>345</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2023-05</td>
      <td>57</td>
      <td>21</td>
      <td>360</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2023-06</td>
      <td>51</td>
      <td>13</td>
      <td>383</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2023-07</td>
      <td>48</td>
      <td>33</td>
      <td>412</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2023-08</td>
      <td>35</td>
      <td>33</td>
      <td>423</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2023-09</td>
      <td>28</td>
      <td>55</td>
      <td>429</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2023-10</td>
      <td>27</td>
      <td>101</td>
      <td>441</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2023-11</td>
      <td>10</td>
      <td>130</td>
      <td>399</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2023-12</td>
      <td>12</td>
      <td>357</td>
      <td>357</td>
    </tr>
  </tbody>
</table>
</div>




```python
months = ['Январь','Февраль','Март','Апрель','Май','Июнь','Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь']
monthly_clients.plot(kind='line', figsize=(12, 6), linewidth=2, marker='o')
plt.title('Динамика клиентской базы по месяцам')
plt.xlabel('Месяц')
plt.ylabel('Количество клиентов')
plt.xticks(range(len(months)), months, rotation=45)
plt.legend(title='Метрика')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

<img width="1189" height="590" alt="output_34_0" src="https://github.com/user-attachments/assets/f5cf5604-6049-4674-ac35-1dbb4c5bff1b" />
