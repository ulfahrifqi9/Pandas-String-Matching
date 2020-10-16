# String Matching in Python (pandas)

By the end of this tutorial you will know:

" how to approximately match strings and determine how similar they are by going over various examples ? "

file 1 :

```python
import pandas as pd
import psycopg2


master = pd.read_csv('master_kabkota.csv')
master.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_kab</th>
      <th>kab_kota</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3372</td>
      <td>KOTA SURAKARTA</td>
      <td>Jawa Tengah</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3506</td>
      <td>KAB. KEDIRI</td>
      <td>Jawa Timur</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7313</td>
      <td>KAB. WAJO</td>
      <td>Sulawesi Selatan</td>
    </tr>
  </tbody>
</table>
</div>

File 2 :

```python
coord = pd.read_csv('polusi_kabkota.csv')
coord.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Banda Aceh</td>
      <td>Aceh</td>
      <td>Kota Banda Aceh</td>
      <td>Banda Aceh</td>
      <td>Indonesia</td>
      <td>5.54167</td>
      <td>95.33333</td>
      <td>18/03/2020 05:00</td>
      <td>Broken clouds</td>
      <td>56</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Langsa</td>
      <td>Aceh</td>
      <td>Kota Langsa</td>
      <td>Langsa</td>
      <td>Indonesia</td>
      <td>4.46830</td>
      <td>97.96830</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lhokseumawe</td>
      <td>Aceh</td>
      <td>Kota Lhokseumawe</td>
      <td>Lhokseumawe</td>
      <td>Indonesia</td>
      <td>5.18010</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
    </tr>
  </tbody>
</table>
</div>

case nya di sini kita akan mencocokan dua file, untuk mengambil semua informasi di table nya yaitu id_kab, coordinates_latitude dan coordinates_longitude

ket :

1. master : 514 row
2. coord : 3440 row

```python
coord['kab_kota']=coord['kab_kota'].str.upper()  #mengubah format kolom kab_kota menjadi UPPERCASE
df = coord.merge(master, on='kab_kota', how='left') #merge 2 dataframe menggunakan kab_kota
df.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
      <th>id_kab</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Banda Aceh</td>
      <td>Aceh</td>
      <td>Kota Banda Aceh</td>
      <td>BANDA ACEH</td>
      <td>Indonesia</td>
      <td>5.54167</td>
      <td>95.33333</td>
      <td>18/03/2020 05:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Langsa</td>
      <td>Aceh</td>
      <td>Kota Langsa</td>
      <td>LANGSA</td>
      <td>Indonesia</td>
      <td>4.46830</td>
      <td>97.96830</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lhokseumawe</td>
      <td>Aceh</td>
      <td>Kota Lhokseumawe</td>
      <td>LHOKSEUMAWE</td>
      <td>Indonesia</td>
      <td>5.18010</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

```python
df['id_kab'].isnull().sum() #jumlah kolom id_kab yang null
```

    3440

tenyata dari 3440 data, tidak ada satupun nama kab_kota yang matching, oke dri pola nya kelihatan bahwa df1 masih terdapat kata "kab" dan "kota", maka proses selanjutnya adalah cleansing data nya (df1)

```python
master['kab_kota'] = master['kab_kota'].str.replace(r'(KAB. |KOTA )','')
master.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_kab</th>
      <th>kab_kota</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3372</td>
      <td>SURAKARTA</td>
      <td>Jawa Tengah</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3506</td>
      <td>KEDIRI</td>
      <td>Jawa Timur</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7313</td>
      <td>WAJO</td>
      <td>Sulawesi Selatan</td>
    </tr>
  </tbody>
</table>
</div>

```python
coord['kab_kota']=coord['kab_kota'].str.upper()  #mengubah format kolom kab_kota menjadi UPPERCASE
df = coord.merge(master, on='kab_kota', how='left') #merge 2 dataframe menggunakan kab_kota
df = df.drop_duplicates()
df.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
      <th>id_kab</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Banda Aceh</td>
      <td>Aceh</td>
      <td>Kota Banda Aceh</td>
      <td>BANDA ACEH</td>
      <td>Indonesia</td>
      <td>5.54167</td>
      <td>95.33333</td>
      <td>18/03/2020 05:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1171.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Langsa</td>
      <td>Aceh</td>
      <td>Kota Langsa</td>
      <td>LANGSA</td>
      <td>Indonesia</td>
      <td>4.46830</td>
      <td>97.96830</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1174.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lhokseumawe</td>
      <td>Aceh</td>
      <td>Kota Lhokseumawe</td>
      <td>LHOKSEUMAWE</td>
      <td>Indonesia</td>
      <td>5.18010</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1173.0</td>
      <td>Aceh</td>
    </tr>
  </tbody>
</table>
</div>

```python
df['id_kab'].isnull().sum() #jumlah kolom id_kab yang null
```

    387

setelah dicleansing data nya kita ulangi lagi proses sebelumnya, dan voila terdapat 387 kab_kota yang blm termatching, baiklah coba kita lihat datanya seperti apa

```python
df_notnull = df[df['id_kab'].notnull()] #df yang id_kab nya tidak null
df1 = df[df['id_kab'].isnull()] #df yang id_kab nya null
df1.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
      <th>id_kab</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>76</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>18/03/2020 17:00</td>
      <td>Scattered clouds</td>
      <td>83</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>95</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

```python
#import module
from fuzzywuzzy import process

#define list
kab_similarity=[]
similarity=[]
prov_similarity=[]
id_kab_similarity=[]

for i in df1.kab_kota:
    Ratios = process.extract(i, master.kab_kota, limit=1 )
    kab_similarity.append(Ratios[0][0])
    prov_similarity.append(master.loc[master['kab_kota'] == Ratios[0][0], 'provinsi'].iloc[0])
    similarity.append(Ratios[0][1])
    id_kab_similarity.append(master.loc[master['kab_kota'] == Ratios[0][0], 'id_kab'].iloc[0])
```

    c:\python27\lib\site-packages\fuzzywuzzy\fuzz.py:11: UserWarning: Using slow pure-python SequenceMatcher. Install python-Levenshtein to remove this warning
      warnings.warn('Using slow pure-python SequenceMatcher. Install python-Levenshtein to remove this warning')

```python
df1['kab_similarity']=kab_similarity
df1['id_kab_similarity'] = id_kab_similarity
df1['prov_similarity']=prov_similarity
df1['similarity']=similarity
# df1
df2 = df1[['province','kab_kota','kab_similarity','id_kab_similarity','prov_similarity','similarity']]
df2.head(3)
```

    c:\python27\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.
    c:\python27\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy

    c:\python27\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    c:\python27\lib\site-packages\ipykernel_launcher.py:4: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>province</th>
      <th>kab_kota</th>
      <th>kab_similarity</th>
      <th>id_kab_similarity</th>
      <th>prov_similarity</th>
      <th>similarity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>49</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
</div>

terlihat hasil nya, ternyata beberpa typo yng ditemukan adalah salah penggunaan spasi. Dan telihat angka hasil similarity nya > 90%, artinya hasilnya similarity nya optimal, lanjut untuk menggabungkan dengan table sebelumnya

```python
df3 = df1[['name','province','district_city','kab_kota','country','coordinates_latitude','coordinates_longitude','current_ts','current_condition','current_humidity','id_kab_similarity','prov_similarity']]
df3.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
      <th>id_kab_similarity</th>
      <th>prov_similarity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>76</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>18/03/2020 17:00</td>
      <td>Scattered clouds</td>
      <td>83</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>95</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
  </tbody>
</table>
</div>

```python
df3 = df3.rename(columns={'id_kab_similarity':'id_kab','prov_similarity':'provinsi'})
```

```python
result = []
result = pd.concat([df_notnull,df3],sort=False)
result.head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>province</th>
      <th>district_city</th>
      <th>kab_kota</th>
      <th>country</th>
      <th>coordinates_latitude</th>
      <th>coordinates_longitude</th>
      <th>current_ts</th>
      <th>current_condition</th>
      <th>current_humidity</th>
      <th>id_kab</th>
      <th>provinsi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Banda Aceh</td>
      <td>Aceh</td>
      <td>Kota Banda Aceh</td>
      <td>BANDA ACEH</td>
      <td>Indonesia</td>
      <td>5.54167</td>
      <td>95.33333</td>
      <td>18/03/2020 05:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1171.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Langsa</td>
      <td>Aceh</td>
      <td>Kota Langsa</td>
      <td>LANGSA</td>
      <td>Indonesia</td>
      <td>4.46830</td>
      <td>97.96830</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1174.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lhokseumawe</td>
      <td>Aceh</td>
      <td>Kota Lhokseumawe</td>
      <td>LHOKSEUMAWE</td>
      <td>Indonesia</td>
      <td>5.18010</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1173.0</td>
      <td>Aceh</td>
    </tr>
  </tbody>
</table>
</div>

```python
result['id_kab'].isnull().sum() #jumlah kolom id_kab yang null
```

    0

DONE !!
