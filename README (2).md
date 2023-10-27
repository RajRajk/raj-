import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sb

import warnings
warnings.filterwarnings('ignore')
df = pd.read_csv('dataset.csv')
df.head()
df.shape
df.info()
df.describe()
splitted = df['Origin Time'].str.split(' ', n=1,
									expand=True)

df['Date'] = splitted[0]
df['Time'] = splitted[1].str[:-4]

df.drop('Origin Time',
		axis=1,
		inplace=True)
df.head()
splitted = df['Date'].str.split('-', expand=True)

df['day'] = splitted[2].astype('int')
df['month'] = splitted[1].astype('int')
df['year'] = splitted[0].astype('int')

df.drop('Date', axis=1,
		inplace=True)
df.head()
plt.figure(figsize=(10, 5))
x = df.groupby('year').mean()['Depth']
x.plot.bar()
plt.show()
plt.figure(figsize=(10, 5))
sb.lineplot(data=df,
			x='month',
			y='Magnitude')
plt.show()
plt.subplots(figsize=(15, 5))

plt.subplot(1, 2, 1)
sb.distplot(df['Depth'])

plt.subplot(1, 2, 2)
sb.boxplot(df['Depth'])

plt.show()
plt.subplots(figsize=(15, 5))

plt.subplot(1, 2, 1)
sb.distplot(df['Magnitude'])

plt.subplot(1, 2, 2)
sb.boxplot(df['Magnitude'])

plt.show()
plt.figure(figsize=(10, 8))
sb.scatterplot(data=df,
			x='Latitude',
			y='Longitude',
			hue='Magnitude')
plt.show()
import plotly.express as px
import pandas as pd

fig = px.scatter_geo(df, lat='Latitude',
					lon='Longitude',
					color="Magnitude",
					fitbounds='locations',
					scope='asia')
fig.show()
