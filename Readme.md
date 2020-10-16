
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
df
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
      <td>5.541670</td>
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
      <td>4.468300</td>
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
      <td>5.180100</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1173.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sabang</td>
      <td>Aceh</td>
      <td>Kota Sabang</td>
      <td>SABANG</td>
      <td>Indonesia</td>
      <td>5.889690</td>
      <td>95.31644</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>1172.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Meulaboh</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Barat</td>
      <td>ACEH BARAT</td>
      <td>Indonesia</td>
      <td>4.144020</td>
      <td>96.12664</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>68</td>
      <td>1105.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Blangpidie</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Barat Daya</td>
      <td>ACEH BARAT DAYA</td>
      <td>Indonesia</td>
      <td>3.739560</td>
      <td>96.83608</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>74</td>
      <td>1112.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Jantho</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Besar</td>
      <td>ACEH BESAR</td>
      <td>Indonesia</td>
      <td>5.333400</td>
      <td>95.61336</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1106.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Calang</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Jaya</td>
      <td>ACEH JAYA</td>
      <td>Indonesia</td>
      <td>4.630600</td>
      <td>95.57540</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>69</td>
      <td>1114.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Tapaktuan</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Selatan</td>
      <td>ACEH SELATAN</td>
      <td>Indonesia</td>
      <td>3.286250</td>
      <td>97.16045</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>74</td>
      <td>1101.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Singkil</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Singkil</td>
      <td>ACEH SINGKIL</td>
      <td>Indonesia</td>
      <td>2.287400</td>
      <td>97.78840</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1110.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Karangbaru</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tamiang</td>
      <td>ACEH TAMIANG</td>
      <td>Indonesia</td>
      <td>4.305630</td>
      <td>98.02709</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1116.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Takengon</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tengah</td>
      <td>ACEH TENGAH</td>
      <td>Indonesia</td>
      <td>4.621200</td>
      <td>96.84670</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>49</td>
      <td>1104.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Kutacane</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tenggara</td>
      <td>ACEH TENGGARA</td>
      <td>Indonesia</td>
      <td>3.522110</td>
      <td>97.79625</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>57</td>
      <td>1102.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Idi Rayeuk</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Timur</td>
      <td>ACEH TIMUR</td>
      <td>Indonesia</td>
      <td>4.930370</td>
      <td>97.79129</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>68</td>
      <td>1103.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Simpang Tiga Redelong</td>
      <td>Aceh</td>
      <td>Kabupaten Bener Meriah</td>
      <td>BENER MERIAH</td>
      <td>Indonesia</td>
      <td>4.722000</td>
      <td>96.87430</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>49</td>
      <td>1117.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bireun</td>
      <td>Aceh</td>
      <td>Kabupaten Bireuen</td>
      <td>BIREUEN</td>
      <td>Indonesia</td>
      <td>5.203000</td>
      <td>96.70090</td>
      <td>18/03/2020 16:00</td>
      <td>Clear sky</td>
      <td>69</td>
      <td>1111.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Blangkejeren</td>
      <td>Aceh</td>
      <td>Kabupaten Gayo Lues</td>
      <td>GAYO LUES</td>
      <td>Indonesia</td>
      <td>3.997100</td>
      <td>97.33840</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1113.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Suka Makmue</td>
      <td>Aceh</td>
      <td>Kabupaten Nagan Raya</td>
      <td>NAGAN RAYA</td>
      <td>Indonesia</td>
      <td>4.003120</td>
      <td>96.61762</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>66</td>
      <td>1115.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Sigli</td>
      <td>Aceh</td>
      <td>Kabupaten Pidie</td>
      <td>PIDIE</td>
      <td>Indonesia</td>
      <td>5.384800</td>
      <td>95.96090</td>
      <td>18/03/2020 16:00</td>
      <td>Few clouds</td>
      <td>66</td>
      <td>1107.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Sinabang</td>
      <td>Aceh</td>
      <td>Kabupaten Simeulue</td>
      <td>SIMEULUE</td>
      <td>Indonesia</td>
      <td>2.480300</td>
      <td>96.38010</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>73</td>
      <td>1109.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Mangupura</td>
      <td>Bali</td>
      <td>Kabupaten Badung</td>
      <td>BADUNG</td>
      <td>Indonesia</td>
      <td>-8.543890</td>
      <td>115.17000</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5103.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Amlapura</td>
      <td>Bali</td>
      <td>Kabupaten Karangasem</td>
      <td>KARANGASEM</td>
      <td>Indonesia</td>
      <td>-8.450000</td>
      <td>115.61667</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5107.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Semarapura</td>
      <td>Bali</td>
      <td>Kabupaten Klungkung</td>
      <td>KLUNGKUNG</td>
      <td>Indonesia</td>
      <td>-8.534600</td>
      <td>115.40220</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5105.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Denpasar</td>
      <td>Bali</td>
      <td>Kota Denpasar</td>
      <td>DENPASAR</td>
      <td>Indonesia</td>
      <td>-8.663225</td>
      <td>115.19754</td>
      <td>18/03/2020 17:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5171.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Bangli</td>
      <td>Bali</td>
      <td>Kabupaten Bangli</td>
      <td>BANGLI</td>
      <td>Indonesia</td>
      <td>-8.454200</td>
      <td>115.35450</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5106.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Singaraja</td>
      <td>Bali</td>
      <td>Kabupaten Buleleng</td>
      <td>BULELENG</td>
      <td>Indonesia</td>
      <td>-8.112000</td>
      <td>115.08818</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>73</td>
      <td>5108.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Gianyar</td>
      <td>Bali</td>
      <td>Kabupaten Gianyar</td>
      <td>GIANYAR</td>
      <td>Indonesia</td>
      <td>-8.543500</td>
      <td>115.32720</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5104.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Negara</td>
      <td>Bali</td>
      <td>Kabupaten Jembrana</td>
      <td>JEMBRANA</td>
      <td>Indonesia</td>
      <td>-8.356940</td>
      <td>114.61694</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>66</td>
      <td>5101.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Tabanan</td>
      <td>Bali</td>
      <td>Kabupaten Tabanan</td>
      <td>TABANAN</td>
      <td>Indonesia</td>
      <td>-8.541300</td>
      <td>115.12522</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5102.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Serang</td>
      <td>Banten</td>
      <td>Kota Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.115280</td>
      <td>106.15417</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>83</td>
      <td>3604.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3794</th>
      <td>Gianyar</td>
      <td>Bali</td>
      <td>Kabupaten Gianyar</td>
      <td>GIANYAR</td>
      <td>Indonesia</td>
      <td>-8.543500</td>
      <td>115.32720</td>
      <td>05/04/2020 13:00</td>
      <td>Scattered clouds</td>
      <td>79</td>
      <td>5104.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>3795</th>
      <td>Negara</td>
      <td>Bali</td>
      <td>Kabupaten Jembrana</td>
      <td>JEMBRANA</td>
      <td>Indonesia</td>
      <td>-8.356940</td>
      <td>114.61694</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>5101.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>3796</th>
      <td>Tabanan</td>
      <td>Bali</td>
      <td>Kabupaten Tabanan</td>
      <td>TABANAN</td>
      <td>Indonesia</td>
      <td>-8.541300</td>
      <td>115.12522</td>
      <td>05/04/2020 13:00</td>
      <td>Scattered clouds</td>
      <td>79</td>
      <td>5102.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>3797</th>
      <td>Serang</td>
      <td>Banten</td>
      <td>Kota Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.115280</td>
      <td>106.15417</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3604.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3798</th>
      <td>Serang</td>
      <td>Banten</td>
      <td>Kota Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.115280</td>
      <td>106.15417</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3673.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3799</th>
      <td>Tangerang</td>
      <td>Banten</td>
      <td>Kota Tangerang</td>
      <td>TANGERANG</td>
      <td>Indonesia</td>
      <td>-6.178060</td>
      <td>106.63000</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3603.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3800</th>
      <td>Tangerang</td>
      <td>Banten</td>
      <td>Kota Tangerang</td>
      <td>TANGERANG</td>
      <td>Indonesia</td>
      <td>-6.178060</td>
      <td>106.63000</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3671.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3801</th>
      <td>South Tangerang</td>
      <td>Banten</td>
      <td>Kota Tangerang Selatan</td>
      <td>TANGERANG SELATAN</td>
      <td>Indonesia</td>
      <td>-6.288620</td>
      <td>106.71789</td>
      <td>05/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>3674.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3802</th>
      <td>Rangkasbitung</td>
      <td>Banten</td>
      <td>Kabupaten Lebak</td>
      <td>LEBAK</td>
      <td>Indonesia</td>
      <td>-6.359100</td>
      <td>106.24940</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3602.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3803</th>
      <td>Pandeglang</td>
      <td>Banten</td>
      <td>Kabupaten Pandeglang</td>
      <td>PANDEGLANG</td>
      <td>Indonesia</td>
      <td>-6.308400</td>
      <td>106.10670</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3601.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3804</th>
      <td>Ciruas</td>
      <td>Banten</td>
      <td>Kabupaten Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.125000</td>
      <td>106.23278</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3604.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3805</th>
      <td>Ciruas</td>
      <td>Banten</td>
      <td>Kabupaten Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.125000</td>
      <td>106.23278</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>62</td>
      <td>3673.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3806</th>
      <td>Tigaraksa</td>
      <td>Banten</td>
      <td>Kabupaten Tangerang</td>
      <td>TANGERANG</td>
      <td>Indonesia</td>
      <td>-6.228060</td>
      <td>106.51389</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>66</td>
      <td>3603.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3807</th>
      <td>Tigaraksa</td>
      <td>Banten</td>
      <td>Kabupaten Tangerang</td>
      <td>TANGERANG</td>
      <td>Indonesia</td>
      <td>-6.228060</td>
      <td>106.51389</td>
      <td>05/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>66</td>
      <td>3671.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>3808</th>
      <td>Bengkulu</td>
      <td>Bengkulu</td>
      <td>Kota Bengkulu</td>
      <td>BENGKULU</td>
      <td>Indonesia</td>
      <td>-3.800440</td>
      <td>102.26554</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>78</td>
      <td>1771.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3809</th>
      <td>Manna</td>
      <td>Bengkulu</td>
      <td>Kabupaten Bengkulu Selatan</td>
      <td>BENGKULU SELATAN</td>
      <td>Indonesia</td>
      <td>-4.465100</td>
      <td>102.90386</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>93</td>
      <td>1701.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3810</th>
      <td>Karang Tinggi</td>
      <td>Bengkulu</td>
      <td>Kabupaten Bengkulu Tengah</td>
      <td>BENGKULU TENGAH</td>
      <td>Indonesia</td>
      <td>-3.750000</td>
      <td>102.42250</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>78</td>
      <td>1709.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3811</th>
      <td>Argamakmur</td>
      <td>Bengkulu</td>
      <td>Kabupaten Bengkulu Utara</td>
      <td>BENGKULU UTARA</td>
      <td>Indonesia</td>
      <td>-3.423200</td>
      <td>102.18910</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>79</td>
      <td>1703.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3812</th>
      <td>Bintuhan</td>
      <td>Bengkulu</td>
      <td>Kabupaten Kaur</td>
      <td>KAUR</td>
      <td>Indonesia</td>
      <td>-4.793710</td>
      <td>103.34836</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>81</td>
      <td>1704.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3813</th>
      <td>Kepahiang</td>
      <td>Bengkulu</td>
      <td>Kabupaten Kepahiang</td>
      <td>KEPAHIANG</td>
      <td>Indonesia</td>
      <td>-3.645500</td>
      <td>102.57120</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>85</td>
      <td>1708.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3814</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>77</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3815</th>
      <td>Curup</td>
      <td>Bengkulu</td>
      <td>Kabupaten Rejang Lebong</td>
      <td>REJANG LEBONG</td>
      <td>Indonesia</td>
      <td>-3.470300</td>
      <td>102.52070</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>85</td>
      <td>1702.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3816</th>
      <td>Tais</td>
      <td>Bengkulu</td>
      <td>Kabupaten Seluma</td>
      <td>SELUMA</td>
      <td>Indonesia</td>
      <td>-4.075430</td>
      <td>102.57739</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>1705.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3817</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>05/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3818</th>
      <td>Tilamuta</td>
      <td>Gorontalo</td>
      <td>Kabupaten Boalemo</td>
      <td>BOALEMO</td>
      <td>Indonesia</td>
      <td>0.527300</td>
      <td>122.34630</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>54</td>
      <td>7502.0</td>
      <td>Gorontalo</td>
    </tr>
    <tr>
      <th>3819</th>
      <td>Limboto</td>
      <td>Gorontalo</td>
      <td>Kabupaten Gorontalo</td>
      <td>GORONTALO</td>
      <td>Indonesia</td>
      <td>0.627000</td>
      <td>122.97780</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>7571.0</td>
      <td>Gorontalo</td>
    </tr>
    <tr>
      <th>3820</th>
      <td>Limboto</td>
      <td>Gorontalo</td>
      <td>Kabupaten Gorontalo</td>
      <td>GORONTALO</td>
      <td>Indonesia</td>
      <td>0.627000</td>
      <td>122.97780</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>7501.0</td>
      <td>Gorontalo</td>
    </tr>
    <tr>
      <th>3821</th>
      <td>Gorontalo</td>
      <td>Gorontalo</td>
      <td>Kota Gorontalo</td>
      <td>GORONTALO</td>
      <td>Indonesia</td>
      <td>0.537500</td>
      <td>123.06250</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>7571.0</td>
      <td>Gorontalo</td>
    </tr>
    <tr>
      <th>3822</th>
      <td>Gorontalo</td>
      <td>Gorontalo</td>
      <td>Kota Gorontalo</td>
      <td>GORONTALO</td>
      <td>Indonesia</td>
      <td>0.537500</td>
      <td>123.06250</td>
      <td>05/04/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>7501.0</td>
      <td>Gorontalo</td>
    </tr>
    <tr>
      <th>3823</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>52</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>3824 rows × 12 columns</p>
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


```python
df1['kab_similarity']=kab_similarity
df1['id_kab_similarity'] = id_kab_similarity
df1['prov_similarity']=prov_similarity
df1['similarity']=similarity
# df1
df2 = df1[['province','kab_kota','kab_similarity','id_kab_similarity','prov_similarity','similarity']]
df2
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
    <tr>
      <th>57</th>
      <td>Jambi</td>
      <td>SUNGAIPENUH</td>
      <td>SUNGAI PENUH</td>
      <td>1572</td>
      <td>Jambi</td>
      <td>96</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Jambi</td>
      <td>BATANG HARI</td>
      <td>BATANGHARI</td>
      <td>1504</td>
      <td>Jambi</td>
      <td>95</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Jambi</td>
      <td>MERANGIN</td>
      <td>MERANGIN</td>
      <td>1502</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Jambi</td>
      <td>MUARO JAMBI</td>
      <td>MUARO JAMBI</td>
      <td>1505</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>225</th>
      <td>Kalimantan Tengah</td>
      <td>PALANGKA RAYA</td>
      <td>PALANGKARAYA</td>
      <td>6271</td>
      <td>Kalimantan Tengah</td>
      <td>96</td>
    </tr>
    <tr>
      <th>249</th>
      <td>Kepulauan Bangka Belitung</td>
      <td>PANGKALPINANG</td>
      <td>PANGKAL PINANG</td>
      <td>1971</td>
      <td>Kepulauan Bangka Belitung</td>
      <td>96</td>
    </tr>
    <tr>
      <th>324</th>
      <td>Nusa Tenggara Timur</td>
      <td>TIMOR TENGAH SELATAN</td>
      <td>KAB TIMOR TENGAH SELATAN</td>
      <td>5302</td>
      <td>Nusa Tenggara Timur</td>
      <td>95</td>
    </tr>
    <tr>
      <th>346</th>
      <td>Papua</td>
      <td>PEGUNUNGAN BINTANG</td>
      <td>KAB PEGUNUNGAN BINTANG</td>
      <td>9112</td>
      <td>Papua</td>
      <td>95</td>
    </tr>
    <tr>
      <th>359</th>
      <td>Papua Barat</td>
      <td>FAKFAK</td>
      <td>FAK FAK</td>
      <td>9203</td>
      <td>Papua Barat</td>
      <td>92</td>
    </tr>
    <tr>
      <th>370</th>
      <td>Riau</td>
      <td>ROKAN HILIR</td>
      <td>ROKAN HILIR</td>
      <td>1407</td>
      <td>Riau</td>
      <td>100</td>
    </tr>
    <tr>
      <th>378</th>
      <td>Riau</td>
      <td>PELALAWAN</td>
      <td>PELALAWAN</td>
      <td>1405</td>
      <td>Riau</td>
      <td>100</td>
    </tr>
    <tr>
      <th>396</th>
      <td>Sulawesi Selatan</td>
      <td>PARE-PARE</td>
      <td>PARE PARE</td>
      <td>7372</td>
      <td>Sulawesi Selatan</td>
      <td>100</td>
    </tr>
    <tr>
      <th>407</th>
      <td>Sulawesi Selatan</td>
      <td>PANGKAJENE DAN KEPULAUAN</td>
      <td>PANGKAJENE KEPULAUAN</td>
      <td>7310</td>
      <td>Sulawesi Selatan</td>
      <td>95</td>
    </tr>
    <tr>
      <th>417</th>
      <td>Sulawesi Tengah</td>
      <td>TOJO UNA-UNA</td>
      <td>TOJO UNA UNA</td>
      <td>7209</td>
      <td>Sulawesi Tengah</td>
      <td>100</td>
    </tr>
    <tr>
      <th>418</th>
      <td>Sulawesi Tengah</td>
      <td>TOLI-TOLI</td>
      <td>TOLI TOLI</td>
      <td>7204</td>
      <td>Sulawesi Tengah</td>
      <td>100</td>
    </tr>
    <tr>
      <th>434</th>
      <td>Sulawesi Utara</td>
      <td>BOLAANG MONGONDOW SELATAN</td>
      <td>BOLAANG MONGONDOW \rSELATAN</td>
      <td>7111</td>
      <td>Sulawesi Utara</td>
      <td>98</td>
    </tr>
    <tr>
      <th>435</th>
      <td>Sulawesi Utara</td>
      <td>BOLAANG MONGONDOW TIMUR</td>
      <td>BOLAANG MONGONDOW \rTIMUR</td>
      <td>7110</td>
      <td>Sulawesi Utara</td>
      <td>98</td>
    </tr>
    <tr>
      <th>436</th>
      <td>Sulawesi Utara</td>
      <td>BOLAANG MONGONDOW UTARA</td>
      <td>BOLAANG MONGONDOW \rUTARA</td>
      <td>7108</td>
      <td>Sulawesi Utara</td>
      <td>98</td>
    </tr>
    <tr>
      <th>437</th>
      <td>Sulawesi Utara</td>
      <td>KEPULAUAN SIAU TAGULANDANG BIARO</td>
      <td>KEP. SIAU TAGULANDANG \rBIARO</td>
      <td>7109</td>
      <td>Sulawesi Utara</td>
      <td>87</td>
    </tr>
    <tr>
      <th>473</th>
      <td>Sumatera Selatan</td>
      <td>OGAN KOMERING ULU SELATAN</td>
      <td>OGAN KOMERING ULU \r\nSELATAN</td>
      <td>1609</td>
      <td>Sumatera Selatan</td>
      <td>96</td>
    </tr>
    <tr>
      <th>474</th>
      <td>Sumatera Selatan</td>
      <td>PENUKAL ABAB LEMATANG ILIR</td>
      <td>PENUKAL ABAB LEMATANG \r\nILIR</td>
      <td>1612</td>
      <td>Sumatera Selatan</td>
      <td>96</td>
    </tr>
    <tr>
      <th>475</th>
      <td>Sumatera Selatan</td>
      <td>LUBUKLINGGAU</td>
      <td>LUBUK LINGGAU</td>
      <td>1673</td>
      <td>Sumatera Selatan</td>
      <td>96</td>
    </tr>
    <tr>
      <th>479</th>
      <td>Sumatera Selatan</td>
      <td>OGAN KOMERING ULU TIMUR</td>
      <td>OGAN KOMERING ULU \r\nTIMUR</td>
      <td>1608</td>
      <td>Sumatera Selatan</td>
      <td>96</td>
    </tr>
    <tr>
      <th>480</th>
      <td>Sumatera Utara</td>
      <td>ASAHAN</td>
      <td>ASAHAN</td>
      <td>1209</td>
      <td>Sumatera Utara</td>
      <td>100</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Sumatera Utara</td>
      <td>BATU BARA</td>
      <td>BATU BARA</td>
      <td>1219</td>
      <td>Sumatera Utara</td>
      <td>100</td>
    </tr>
    <tr>
      <th>482</th>
      <td>Sumatera Utara</td>
      <td>DAIRI</td>
      <td>DAIRI</td>
      <td>1211</td>
      <td>Sumatera Utara</td>
      <td>100</td>
    </tr>
    <tr>
      <th>483</th>
      <td>Sumatera Utara</td>
      <td>DELI SERDANG</td>
      <td>DELI SERDANG</td>
      <td>1207</td>
      <td>Sumatera Utara</td>
      <td>100</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3310</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3313</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3319</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3366</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3369</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3375</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3422</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3431</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3478</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3481</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3487</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3537</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3543</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3590</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3593</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3599</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3646</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3649</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3655</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3702</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3705</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3711</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3758</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3761</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3767</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3814</th>
      <td>Bengkulu</td>
      <td>MUKOMUKO</td>
      <td>MUKO MUKO</td>
      <td>1706</td>
      <td>Bengkulu</td>
      <td>94</td>
    </tr>
    <tr>
      <th>3817</th>
      <td>DKI Jakarta</td>
      <td>JAKARTA</td>
      <td>ADM. JAKARTA TIMUR</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3823</th>
      <td>Jambi</td>
      <td>KERINCI</td>
      <td>KERINCI</td>
      <td>1501</td>
      <td>Jambi</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
<p>387 rows × 6 columns</p>
</div>



terlihat hasil nya, ternyata beberpa typo yng ditemukan adalah salah penggunaan spasi. Dan telihat angka hasil similarity nya > 90%, artinya hasilnya similarity nya optimal, lanjut untuk menggabungkan dengan table sebelumnya 


```python
df3 = df1[['name','province','district_city','kab_kota','country','coordinates_latitude','coordinates_longitude','current_ts','current_condition','current_humidity','id_kab_similarity','prov_similarity']]
df3
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
      <td>101.111800</td>
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
      <td>106.793240</td>
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
      <td>101.334170</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>95</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Sungai Penuh</td>
      <td>Jambi</td>
      <td>Kota Sungaipenuh</td>
      <td>SUNGAIPENUH</td>
      <td>Indonesia</td>
      <td>-2.056100</td>
      <td>101.391300</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>95</td>
      <td>1572</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Muara Bulian</td>
      <td>Jambi</td>
      <td>Kabupaten Batanghari</td>
      <td>BATANG HARI</td>
      <td>Indonesia</td>
      <td>-1.713270</td>
      <td>103.259020</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>88</td>
      <td>1504</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Bangko</td>
      <td>Jambi</td>
      <td>Kabupaten Merangin</td>
      <td>MERANGIN</td>
      <td>Indonesia</td>
      <td>-2.079600</td>
      <td>102.257300</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>77</td>
      <td>1502</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Sengeti</td>
      <td>Jambi</td>
      <td>Kabupaten Muaro Jambi</td>
      <td>MUARO JAMBI</td>
      <td>Indonesia</td>
      <td>-1.477200</td>
      <td>103.507650</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>88</td>
      <td>1505</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>225</th>
      <td>Palangkaraya</td>
      <td>Kalimantan Tengah</td>
      <td>Kota Palangka Raya</td>
      <td>PALANGKA RAYA</td>
      <td>Indonesia</td>
      <td>-2.216105</td>
      <td>113.913977</td>
      <td>18/03/2020 07:00</td>
      <td>Broken clouds</td>
      <td>48</td>
      <td>6271</td>
      <td>Kalimantan Tengah</td>
    </tr>
    <tr>
      <th>249</th>
      <td>Pangkalpinang</td>
      <td>Kepulauan Bangka Belitung</td>
      <td>Kota Pangkalpinang</td>
      <td>PANGKALPINANG</td>
      <td>Indonesia</td>
      <td>-2.129140</td>
      <td>106.113770</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>80</td>
      <td>1971</td>
      <td>Kepulauan Bangka Belitung</td>
    </tr>
    <tr>
      <th>324</th>
      <td>Soe</td>
      <td>Nusa Tenggara Timur</td>
      <td>Kabupaten Timor Tengah Selatan</td>
      <td>TIMOR TENGAH SELATAN</td>
      <td>Indonesia</td>
      <td>-9.860710</td>
      <td>124.283950</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>73</td>
      <td>5302</td>
      <td>Nusa Tenggara Timur</td>
    </tr>
    <tr>
      <th>346</th>
      <td>Oksibil</td>
      <td>Papua</td>
      <td>Kabupaten Pegunungan Bintang</td>
      <td>PEGUNUNGAN BINTANG</td>
      <td>Indonesia</td>
      <td>-4.904170</td>
      <td>140.629720</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>99</td>
      <td>9112</td>
      <td>Papua</td>
    </tr>
    <tr>
      <th>359</th>
      <td>Fakfak</td>
      <td>Papua Barat</td>
      <td>Kabupaten Fakfak</td>
      <td>FAKFAK</td>
      <td>Indonesia</td>
      <td>-2.924810</td>
      <td>132.298130</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>79</td>
      <td>9203</td>
      <td>Papua Barat</td>
    </tr>
    <tr>
      <th>370</th>
      <td>Bagansiapiapi</td>
      <td>Riau</td>
      <td>Kabupaten Rokan Hilir</td>
      <td>ROKAN HILIR</td>
      <td>Indonesia</td>
      <td>2.156680</td>
      <td>100.807630</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>1407</td>
      <td>Riau</td>
    </tr>
    <tr>
      <th>378</th>
      <td>Pangkalan Kerinci</td>
      <td>Riau</td>
      <td>Kabupaten Pelalawan</td>
      <td>PELALAWAN</td>
      <td>Indonesia</td>
      <td>0.396110</td>
      <td>101.857220</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>83</td>
      <td>1405</td>
      <td>Riau</td>
    </tr>
    <tr>
      <th>396</th>
      <td>Parepare</td>
      <td>Sulawesi Selatan</td>
      <td>Kota Parepare</td>
      <td>PARE-PARE</td>
      <td>Indonesia</td>
      <td>-4.013500</td>
      <td>119.625500</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>72</td>
      <td>7372</td>
      <td>Sulawesi Selatan</td>
    </tr>
    <tr>
      <th>407</th>
      <td>Pangkajene</td>
      <td>Sulawesi Selatan</td>
      <td>Kabupaten Pangkajene dan Kepulauan</td>
      <td>PANGKAJENE DAN KEPULAUAN</td>
      <td>Indonesia</td>
      <td>-4.837200</td>
      <td>119.547200</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>88</td>
      <td>7310</td>
      <td>Sulawesi Selatan</td>
    </tr>
    <tr>
      <th>417</th>
      <td>Ampana</td>
      <td>Sulawesi Tengah</td>
      <td>Kabupaten Tojo Una-Una</td>
      <td>TOJO UNA-UNA</td>
      <td>Indonesia</td>
      <td>-0.869010</td>
      <td>121.583670</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>71</td>
      <td>7209</td>
      <td>Sulawesi Tengah</td>
    </tr>
    <tr>
      <th>418</th>
      <td>Tolitoli</td>
      <td>Sulawesi Tengah</td>
      <td>Kabupaten Tolitoli</td>
      <td>TOLI-TOLI</td>
      <td>Indonesia</td>
      <td>1.039200</td>
      <td>120.816900</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>78</td>
      <td>7204</td>
      <td>Sulawesi Tengah</td>
    </tr>
    <tr>
      <th>434</th>
      <td>Molibagu</td>
      <td>Sulawesi Utara</td>
      <td>Kabupaten Bolaang Mongondow Selatan</td>
      <td>BOLAANG MONGONDOW SELATAN</td>
      <td>Indonesia</td>
      <td>0.386200</td>
      <td>123.982400</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>94</td>
      <td>7111</td>
      <td>Sulawesi Utara</td>
    </tr>
    <tr>
      <th>435</th>
      <td>Tutuyan</td>
      <td>Sulawesi Utara</td>
      <td>Kabupaten Bolaang Mongondow Timur</td>
      <td>BOLAANG MONGONDOW TIMUR</td>
      <td>Indonesia</td>
      <td>0.765400</td>
      <td>124.614400</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>98</td>
      <td>7110</td>
      <td>Sulawesi Utara</td>
    </tr>
    <tr>
      <th>436</th>
      <td>Boroko</td>
      <td>Sulawesi Utara</td>
      <td>Kabupaten Bolaang Mongondow Utara</td>
      <td>BOLAANG MONGONDOW UTARA</td>
      <td>Indonesia</td>
      <td>0.908300</td>
      <td>123.267900</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>69</td>
      <td>7108</td>
      <td>Sulawesi Utara</td>
    </tr>
    <tr>
      <th>437</th>
      <td>Ondong Siau</td>
      <td>Sulawesi Utara</td>
      <td>Kabupaten Kepulauan Siau Tagulandang Biaro</td>
      <td>KEPULAUAN SIAU TAGULANDANG BIARO</td>
      <td>Indonesia</td>
      <td>2.747300</td>
      <td>125.361100</td>
      <td>18/03/2020 16:00</td>
      <td>Few clouds</td>
      <td>78</td>
      <td>7109</td>
      <td>Sulawesi Utara</td>
    </tr>
    <tr>
      <th>473</th>
      <td>Muara Dua</td>
      <td>Sumatera Selatan</td>
      <td>Kabupaten Ogan Komering Ulu Selatan</td>
      <td>OGAN KOMERING ULU SELATAN</td>
      <td>Indonesia</td>
      <td>-4.531780</td>
      <td>104.073970</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>87</td>
      <td>1609</td>
      <td>Sumatera Selatan</td>
    </tr>
    <tr>
      <th>474</th>
      <td>Talang Ubi</td>
      <td>Sumatera Selatan</td>
      <td>Kabupaten Penukal Abab Lematang Ilir</td>
      <td>PENUKAL ABAB LEMATANG ILIR</td>
      <td>Indonesia</td>
      <td>-3.265600</td>
      <td>103.825300</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>89</td>
      <td>1612</td>
      <td>Sumatera Selatan</td>
    </tr>
    <tr>
      <th>475</th>
      <td>Lubuklinggau</td>
      <td>Sumatera Selatan</td>
      <td>Kota Lubuklinggau</td>
      <td>LUBUKLINGGAU</td>
      <td>Indonesia</td>
      <td>-3.294500</td>
      <td>102.861400</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>94</td>
      <td>1673</td>
      <td>Sumatera Selatan</td>
    </tr>
    <tr>
      <th>479</th>
      <td>Martapura</td>
      <td>Sumatera Selatan</td>
      <td>Kabupaten Ogan Komering Ulu Timur</td>
      <td>OGAN KOMERING ULU TIMUR</td>
      <td>Indonesia</td>
      <td>-3.410900</td>
      <td>114.864200</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>72</td>
      <td>1608</td>
      <td>Sumatera Selatan</td>
    </tr>
    <tr>
      <th>480</th>
      <td>Kisaran</td>
      <td>Sumatera Utara</td>
      <td>Kabupaten Asahan</td>
      <td>ASAHAN</td>
      <td>Indonesia</td>
      <td>2.984500</td>
      <td>99.615800</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>51</td>
      <td>1209</td>
      <td>Sumatera Utara</td>
    </tr>
    <tr>
      <th>481</th>
      <td>Limapuluh</td>
      <td>Sumatera Utara</td>
      <td>Kabupaten Batu Bara</td>
      <td>BATU BARA</td>
      <td>Indonesia</td>
      <td>3.168900</td>
      <td>99.417500</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>51</td>
      <td>1219</td>
      <td>Sumatera Utara</td>
    </tr>
    <tr>
      <th>482</th>
      <td>Sidikalang</td>
      <td>Sumatera Utara</td>
      <td>Kabupaten Dairi</td>
      <td>DAIRI</td>
      <td>Indonesia</td>
      <td>2.748990</td>
      <td>98.312700</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>76</td>
      <td>1211</td>
      <td>Sumatera Utara</td>
    </tr>
    <tr>
      <th>483</th>
      <td>Lubuk Pakam</td>
      <td>Sumatera Utara</td>
      <td>Kabupaten Deli Serdang</td>
      <td>DELI SERDANG</td>
      <td>Indonesia</td>
      <td>3.559200</td>
      <td>98.873300</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>83</td>
      <td>1207</td>
      <td>Sumatera Utara</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3310</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>27/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>71</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3313</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>27/03/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3319</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>27/03/2020 13:00</td>
      <td>Rain</td>
      <td>82</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3366</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>28/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3369</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>28/03/2020 14:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3375</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>28/03/2020 13:00</td>
      <td>Rain</td>
      <td>70</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3422</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>29/03/2020 13:00</td>
      <td>Clear sky</td>
      <td>76</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>29/03/2020 14:00</td>
      <td>Rain</td>
      <td>63</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3431</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>29/03/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3478</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>30/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>80</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3481</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>30/03/2020 14:00</td>
      <td>Thunderstorm</td>
      <td>74</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3487</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>30/03/2020 13:00</td>
      <td>Rain</td>
      <td>75</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>31/03/2020 13:00</td>
      <td>Clear sky</td>
      <td>75</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3537</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>31/03/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>47</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3543</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>31/03/2020 13:00</td>
      <td>Rain</td>
      <td>80</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3590</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>01/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>74</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3593</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>01/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>59</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3599</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>01/04/2020 13:00</td>
      <td>Rain</td>
      <td>82</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3646</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>02/04/2020 13:00</td>
      <td>Clear sky</td>
      <td>73</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3649</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>02/04/2020 14:00</td>
      <td>Rain</td>
      <td>62</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3655</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>02/04/2020 13:00</td>
      <td>Rain</td>
      <td>79</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3702</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>03/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>69</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3705</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>03/04/2020 14:00</td>
      <td>Rain</td>
      <td>70</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3711</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>03/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>62</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3758</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>04/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>78</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3761</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>04/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>59</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3767</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>04/04/2020 13:00</td>
      <td>Rain</td>
      <td>77</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3814</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.111800</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>77</td>
      <td>1706</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3817</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.793240</td>
      <td>05/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>3175</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3823</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.334170</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>52</td>
      <td>1501</td>
      <td>Jambi</td>
    </tr>
  </tbody>
</table>
<p>387 rows × 12 columns</p>
</div>




```python
df3 = df3.rename(columns={'id_kab_similarity':'id_kab','prov_similarity':'provinsi'})
```


```python
result = []
result = pd.concat([df_notnull,df3])
result
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
      <td>5.541670</td>
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
      <td>4.468300</td>
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
      <td>5.180100</td>
      <td>97.15070</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1173.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sabang</td>
      <td>Aceh</td>
      <td>Kota Sabang</td>
      <td>SABANG</td>
      <td>Indonesia</td>
      <td>5.889690</td>
      <td>95.31644</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>1172.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Meulaboh</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Barat</td>
      <td>ACEH BARAT</td>
      <td>Indonesia</td>
      <td>4.144020</td>
      <td>96.12664</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>68</td>
      <td>1105.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Blangpidie</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Barat Daya</td>
      <td>ACEH BARAT DAYA</td>
      <td>Indonesia</td>
      <td>3.739560</td>
      <td>96.83608</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>74</td>
      <td>1112.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Jantho</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Besar</td>
      <td>ACEH BESAR</td>
      <td>Indonesia</td>
      <td>5.333400</td>
      <td>95.61336</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1106.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Calang</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Jaya</td>
      <td>ACEH JAYA</td>
      <td>Indonesia</td>
      <td>4.630600</td>
      <td>95.57540</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>69</td>
      <td>1114.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Tapaktuan</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Selatan</td>
      <td>ACEH SELATAN</td>
      <td>Indonesia</td>
      <td>3.286250</td>
      <td>97.16045</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>74</td>
      <td>1101.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Singkil</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Singkil</td>
      <td>ACEH SINGKIL</td>
      <td>Indonesia</td>
      <td>2.287400</td>
      <td>97.78840</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>1110.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Karangbaru</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tamiang</td>
      <td>ACEH TAMIANG</td>
      <td>Indonesia</td>
      <td>4.305630</td>
      <td>98.02709</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1116.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Takengon</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tengah</td>
      <td>ACEH TENGAH</td>
      <td>Indonesia</td>
      <td>4.621200</td>
      <td>96.84670</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>49</td>
      <td>1104.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Kutacane</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Tenggara</td>
      <td>ACEH TENGGARA</td>
      <td>Indonesia</td>
      <td>3.522110</td>
      <td>97.79625</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>57</td>
      <td>1102.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Idi Rayeuk</td>
      <td>Aceh</td>
      <td>Kabupaten Aceh Timur</td>
      <td>ACEH TIMUR</td>
      <td>Indonesia</td>
      <td>4.930370</td>
      <td>97.79129</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>68</td>
      <td>1103.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Simpang Tiga Redelong</td>
      <td>Aceh</td>
      <td>Kabupaten Bener Meriah</td>
      <td>BENER MERIAH</td>
      <td>Indonesia</td>
      <td>4.722000</td>
      <td>96.87430</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>49</td>
      <td>1117.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bireun</td>
      <td>Aceh</td>
      <td>Kabupaten Bireuen</td>
      <td>BIREUEN</td>
      <td>Indonesia</td>
      <td>5.203000</td>
      <td>96.70090</td>
      <td>18/03/2020 16:00</td>
      <td>Clear sky</td>
      <td>69</td>
      <td>1111.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Blangkejeren</td>
      <td>Aceh</td>
      <td>Kabupaten Gayo Lues</td>
      <td>GAYO LUES</td>
      <td>Indonesia</td>
      <td>3.997100</td>
      <td>97.33840</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>56</td>
      <td>1113.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Suka Makmue</td>
      <td>Aceh</td>
      <td>Kabupaten Nagan Raya</td>
      <td>NAGAN RAYA</td>
      <td>Indonesia</td>
      <td>4.003120</td>
      <td>96.61762</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>66</td>
      <td>1115.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Sigli</td>
      <td>Aceh</td>
      <td>Kabupaten Pidie</td>
      <td>PIDIE</td>
      <td>Indonesia</td>
      <td>5.384800</td>
      <td>95.96090</td>
      <td>18/03/2020 16:00</td>
      <td>Few clouds</td>
      <td>66</td>
      <td>1107.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Sinabang</td>
      <td>Aceh</td>
      <td>Kabupaten Simeulue</td>
      <td>SIMEULUE</td>
      <td>Indonesia</td>
      <td>2.480300</td>
      <td>96.38010</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>73</td>
      <td>1109.0</td>
      <td>Aceh</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Mangupura</td>
      <td>Bali</td>
      <td>Kabupaten Badung</td>
      <td>BADUNG</td>
      <td>Indonesia</td>
      <td>-8.543890</td>
      <td>115.17000</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5103.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Amlapura</td>
      <td>Bali</td>
      <td>Kabupaten Karangasem</td>
      <td>KARANGASEM</td>
      <td>Indonesia</td>
      <td>-8.450000</td>
      <td>115.61667</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5107.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Semarapura</td>
      <td>Bali</td>
      <td>Kabupaten Klungkung</td>
      <td>KLUNGKUNG</td>
      <td>Indonesia</td>
      <td>-8.534600</td>
      <td>115.40220</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5105.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Denpasar</td>
      <td>Bali</td>
      <td>Kota Denpasar</td>
      <td>DENPASAR</td>
      <td>Indonesia</td>
      <td>-8.663225</td>
      <td>115.19754</td>
      <td>18/03/2020 17:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5171.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Bangli</td>
      <td>Bali</td>
      <td>Kabupaten Bangli</td>
      <td>BANGLI</td>
      <td>Indonesia</td>
      <td>-8.454200</td>
      <td>115.35450</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5106.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Singaraja</td>
      <td>Bali</td>
      <td>Kabupaten Buleleng</td>
      <td>BULELENG</td>
      <td>Indonesia</td>
      <td>-8.112000</td>
      <td>115.08818</td>
      <td>18/03/2020 16:00</td>
      <td>Rain</td>
      <td>73</td>
      <td>5108.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Gianyar</td>
      <td>Bali</td>
      <td>Kabupaten Gianyar</td>
      <td>GIANYAR</td>
      <td>Indonesia</td>
      <td>-8.543500</td>
      <td>115.32720</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5104.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Negara</td>
      <td>Bali</td>
      <td>Kabupaten Jembrana</td>
      <td>JEMBRANA</td>
      <td>Indonesia</td>
      <td>-8.356940</td>
      <td>114.61694</td>
      <td>18/03/2020 16:00</td>
      <td>Broken clouds</td>
      <td>66</td>
      <td>5101.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Tabanan</td>
      <td>Bali</td>
      <td>Kabupaten Tabanan</td>
      <td>TABANAN</td>
      <td>Indonesia</td>
      <td>-8.541300</td>
      <td>115.12522</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>84</td>
      <td>5102.0</td>
      <td>Bali</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Serang</td>
      <td>Banten</td>
      <td>Kota Serang</td>
      <td>SERANG</td>
      <td>Indonesia</td>
      <td>-6.115280</td>
      <td>106.15417</td>
      <td>18/03/2020 16:00</td>
      <td>Scattered clouds</td>
      <td>83</td>
      <td>3604.0</td>
      <td>Banten</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3310</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>27/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>71</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3313</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>27/03/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3319</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>27/03/2020 13:00</td>
      <td>Rain</td>
      <td>82</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3366</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>28/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>74</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3369</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>28/03/2020 14:00</td>
      <td>Rain</td>
      <td>66</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3375</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>28/03/2020 13:00</td>
      <td>Rain</td>
      <td>70</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3422</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>29/03/2020 13:00</td>
      <td>Clear sky</td>
      <td>76</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>29/03/2020 14:00</td>
      <td>Rain</td>
      <td>63</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3431</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>29/03/2020 13:00</td>
      <td>Rain</td>
      <td>68</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3478</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>30/03/2020 13:00</td>
      <td>Broken clouds</td>
      <td>80</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3481</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>30/03/2020 14:00</td>
      <td>Thunderstorm</td>
      <td>74</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3487</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>30/03/2020 13:00</td>
      <td>Rain</td>
      <td>75</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>31/03/2020 13:00</td>
      <td>Clear sky</td>
      <td>75</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3537</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>31/03/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>47</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3543</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>31/03/2020 13:00</td>
      <td>Rain</td>
      <td>80</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3590</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>01/04/2020 13:00</td>
      <td>Few clouds</td>
      <td>74</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3593</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>01/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>59</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3599</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>01/04/2020 13:00</td>
      <td>Rain</td>
      <td>82</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3646</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>02/04/2020 13:00</td>
      <td>Clear sky</td>
      <td>73</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3649</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>02/04/2020 14:00</td>
      <td>Rain</td>
      <td>62</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3655</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>02/04/2020 13:00</td>
      <td>Rain</td>
      <td>79</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3702</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>03/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>69</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3705</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>03/04/2020 14:00</td>
      <td>Rain</td>
      <td>70</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3711</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>03/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>62</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3758</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>04/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>78</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3761</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>04/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>59</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3767</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>04/04/2020 13:00</td>
      <td>Rain</td>
      <td>77</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
    <tr>
      <th>3814</th>
      <td>Mukomuko</td>
      <td>Bengkulu</td>
      <td>Kabupaten Mukomuko</td>
      <td>MUKOMUKO</td>
      <td>Indonesia</td>
      <td>-2.568900</td>
      <td>101.11180</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>77</td>
      <td>1706.0</td>
      <td>Bengkulu</td>
    </tr>
    <tr>
      <th>3817</th>
      <td>Jakarta</td>
      <td>DKI Jakarta</td>
      <td>Jakarta</td>
      <td>JAKARTA</td>
      <td>Indonesia</td>
      <td>-6.236704</td>
      <td>106.79324</td>
      <td>05/04/2020 14:00</td>
      <td>Scattered clouds</td>
      <td>66</td>
      <td>3175.0</td>
      <td>DKI Jakarta</td>
    </tr>
    <tr>
      <th>3823</th>
      <td>Siulak</td>
      <td>Jambi</td>
      <td>Kabupaten Kerinci</td>
      <td>KERINCI</td>
      <td>Indonesia</td>
      <td>-1.946670</td>
      <td>101.33417</td>
      <td>05/04/2020 13:00</td>
      <td>Broken clouds</td>
      <td>52</td>
      <td>1501.0</td>
      <td>Jambi</td>
    </tr>
  </tbody>
</table>
<p>3824 rows × 12 columns</p>
</div>




```python
result['id_kab'].isnull().sum() #jumlah kolom id_kab yang null
```




    0



DONE !!
