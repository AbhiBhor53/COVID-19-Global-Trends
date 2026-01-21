<h1>WHO COVID-19 Global Data Analysis</h1>

<h2>Importing required resources</h2>

```python

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
df=pd.read_csv('WHO-COVID-19-global-daily-data.csv')
df

```

<img width="1136" height="511" alt="image" src="https://github.com/user-attachments/assets/9cd9a31b-1501-41c6-b88e-1633fad229b2" />

<h2>Data Cleaning</h2>

```python

df['New_cases']=df['New_cases'].fillna(0)
df['New_deaths']=df['New_deaths'].fillna(0)
df
```
<img width="1143" height="515" alt="image" src="https://github.com/user-attachments/assets/d625aad5-780d-41ff-a6e7-9eb9d9676490" />

<h2>Exploratory Data Analysis</h2>

Which countries had the highest case counts?

```python

result= ( 
    df.groupby('Country')['Cumulative_cases']
    .max()
    .reset_index(name='Maximum Cases')
    .sort_values(by='Maximum Cases', ascending=False)
)
result.head(20)

```

<img width="560" height="803" alt="image" src="https://github.com/user-attachments/assets/549abe31-7127-4c43-809f-9a8ce2a3e3ae" />


```python

import plotly.express as px

fig = px.choropleth(
    result,
    locations="Country",
    locationmode="country names",
    color="Maximum Cases",   
    hover_name="Country",
    color_continuous_scale="Reds",
    title="World Map of Maximum COVID-19 Cases by Country"
)

fig.show()

```

<img width="885" height="435" alt="image" src="https://github.com/user-attachments/assets/48ca6656-57fb-400d-9906-ddc9a1ea7433" />


USA,China,India,France,Germany,Brazil,Republic of Korea,Japan,Italy and UK were the countries with maximum covid cases.
It seems that these countries have large population and large number of people who does international and domestic travel that's why surge in case is observed

How did the virus spread over time (global timeline)?


```python

df_cases=(
    df[df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025'])]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_cases

```

<img width="313" height="278" alt="image" src="https://github.com/user-attachments/assets/70359032-3ade-4caf-ab85-45757c1ae658" />

```python
plt.figure(figsize=(12, 6))

plt.plot(
    df_cases['Date_reported'],
    df_cases['Total_New_Cases'],
    linewidth=2,
    marker='o',
    linestyle='-'
)

plt.title('Spread of Virus Over Time', fontsize=16, fontweight='bold')
plt.xlabel('Date Reported', fontsize=12)
plt.ylabel('Total New Cases', fontsize=12)

plt.xticks(rotation=45)
plt.grid(True, linestyle='--', alpha=0.6)

plt.tight_layout()
plt.show()
```

<img width="1511" height="741" alt="image" src="https://github.com/user-attachments/assets/7c707445-8136-49eb-867d-d5cef2ccda73" />

This analysis shows new cases of covid-19 spread almost linearly since 2020 to 2023. Sudden fall of cases in 2024 due to termination of reporting of new cases and reduction in testing frequency in the major regions like West paific.Also till this time vaccines were being availed to people and cases reduced significantly.

Which region had the fastest recovery rate?

```python
df['WHO_region'].unique()
```

Output : array(['EMR', 'AFR', 'EUR', 'AMR', 'WPR', 'SEAR', 'OTHER'], dtype=object)

```python

df_EMR=(

    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='EMR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_EMR

df_AFR=(

    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='AFR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_AFR

df_EUR=(

    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='EUR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_EUR

df_AMR=(

    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='AMR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_AMR

df_WPR=(

    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='WPR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_WPR


    df[(df['Date_reported'].isin(['04-01-2020','04-01-2021','04-01-2022','04-01-2023','04-01-2024','04-01-2025']))
       & (df['WHO_region']=='SEAR')]
    .groupby('Date_reported')['New_cases']
    .sum()
    .reset_index(name='Total_New_Cases')
)
df_SEAR
```

<img width="311" height="278" alt="image" src="https://github.com/user-attachments/assets/23a8d633-4ea9-48b3-8d0f-d92ba20893fc" />

```python

for d in [df_EMR, df_AFR, df_EUR, df_AMR, df_WPR, df_SEAR]:
    d['Date_reported'] = pd.to_datetime(d['Date_reported'], format='%d-%m-%Y')
    d.sort_values('Date_reported', inplace=True)
plt.figure(figsize=(12, 6))

plt.plot(df_EMR['Date_reported'], df_EMR['Total_New_Cases'],
         marker='o', label='EMR (Eastern Mediterranean)')

plt.plot(df_AFR['Date_reported'], df_AFR['Total_New_Cases'],
         marker='s', label='AFR (Africa)')

plt.plot(df_EUR['Date_reported'], df_EUR['Total_New_Cases'],
         marker='^', label='EUR (Europe)')

plt.plot(df_AMR['Date_reported'], df_AMR['Total_New_Cases'],
         marker='D', label='AMR (Americas)')

plt.plot(df_WPR['Date_reported'], df_WPR['Total_New_Cases'],
         marker='x', label='WPR (Western Pacific)')

plt.plot(df_SEAR['Date_reported'], df_SEAR['Total_New_Cases'],
         marker='*', label='SEAR (South-East Asia)')

plt.xlabel('Year')
plt.ylabel('Total New COVID-19 Cases')
plt.title('COVID-19 New Cases Trend by WHO Region (2020–2025)')
plt.legend(title='WHO Regions')
plt.grid(True)
plt.tight_layout()

plt.show()
```

<img width="1489" height="719" alt="image" src="https://github.com/user-attachments/assets/7db4673a-2c00-4e04-a434-37fcb60bf340" />


The Western Pacific Region shows a rapid decline in reported cases after major surges, which can be attributed largely to high vaccination coverage, effective public health measures, and prior epidemic preparedness. However, the apparent ‘high recovery rate’ is also influenced by changes in testing intensity and reporting practices after 2023. Therefore, the trend reflects a combination of real epidemiological improvement and data reporting dynamics.

<h1>Insights</h1>

The high COVID-19 case counts observed in countries like the USA, India, and major European nations are influenced by a combination of large population bases, extensive domestic and international travel, high urban density, and robust testing infrastructure. While mobility and population size contributed to rapid transmission, higher testing and reporting capacity also played a significant role in the observed surge in reported cases.

This analysis indicates a generally increasing trend in reported new COVID-19 cases from 2020 to 2023 across major WHO regions. The apparent decline in cases observed in 2024 is influenced by multiple factors, including reduced testing frequency and changes in reporting practices, particularly in regions such as the Western Pacific. Additionally, widespread vaccine availability and high vaccination coverage by this period contributed to a significant reduction in severe infections and transmission, leading to lower reported case counts.

The Western Pacific Region shows a rapid decline in reported COVID-19 cases following major surges, which can largely be attributed to high vaccination coverage, effective public health interventions, and prior epidemic preparedness. However, the apparent ‘high recovery rate’ is also influenced by changes in testing intensity and reporting practices after 2023. Consequently, the observed trend reflects a combination of genuine epidemiological improvement and evolving data reporting dynamics.
