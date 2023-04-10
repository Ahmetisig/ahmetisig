
Veri Bilimi 101
Patika Dev

First Name	Last Name
Esad	Boran
Genel olarak yapacağımız adımlar şunlar olacak:

1.Kullanacağınız veriyi indirip, okumak

2.Verinizin içindeki eksik ve kategorik değişkenler ile ilgilenip modele besleyeceğimiz hale getirmek (derste gördüklerimizin üzerine de bir şeyler yapmanız hoşumuza gider demiştik, outlier'ları olan bir veride outlier'lar ile ilgili yaptıklarınızı görmek gibi şeyler olabilir mesela)

3.İlgilendiğiniz probleme göre error metriğine karar vermek (derste gördüğümüz RMSE-RMSLE gibi)

4.Verinizi train-validation-test diye bölmek (burada validation ve test'in gerçek hayatı yansıtması çok önemli) Olabildiğince fazla model denemek ve metriğimizde en iyi yapanı seçmek

Required Libraries
import pandas as pd
import numpy as np


# Download the Data
import os
import tarfile
import urllib.request
1.Download the Data
DOWNLOAD_ROOT = "https://raw.githubusercontent.com/ageron/handson-ml2/master/"
HOUSING_PATH = os.path.join("datasets", "housing")
HOUSING_URL = DOWNLOAD_ROOT + "datasets/housing/housing.tgz"

def fetch_housing_data(housing_url=HOUSING_URL, housing_path=HOUSING_PATH):
    if not os.path.isdir(housing_path):
        os.makedirs(housing_path)
    tgz_path = os.path.join(housing_path, "housing.tgz")
    urllib.request.urlretrieve(housing_url, tgz_path)
    housing_tgz = tarfile.open(tgz_path)
    housing_tgz.extractall(path=housing_path)
    housing_tgz.close()
fetch_housing_data()
import pandas as pd
def load_housing_data(housing_path=HOUSING_PATH): 
    csv_path = os.path.join(housing_path, "housing.csv") 
    return pd.read_csv(csv_path)
data = load_housing_data()
# shuffle the DataFrame rows
data = data.sample(frac = 1)
data.head()
longitude	latitude	housing_median_age	total_rooms	total_bedrooms	population	households	median_income	median_house_value	ocean_proximity
18799	-121.89	40.97	26.0	1183.0	276.0	513.0	206.0	2.2250	52000.0	INLAND
18235	-122.10	37.40	23.0	1755.0	508.0	1374.0	506.0	4.3077	293500.0	NEAR BAY
4158	-118.19	34.11	40.0	1266.0	348.0	1032.0	315.0	2.1667	150000.0	<1H OCEAN
1006	-121.75	37.69	26.0	2647.0	536.0	1422.0	522.0	3.7212	183800.0	INLAND
14926	-117.01	32.67	17.0	2319.0	348.0	1125.0	337.0	5.5510	266900.0	NEAR OCEAN
2.Information About Data
data.info()
<class 'pandas.core.frame.DataFrame'>
Int64Index: 20640 entries, 18799 to 17836
Data columns (total 10 columns):
 #   Column              Non-Null Count  Dtype  
---  ------              --------------  -----  
 0   longitude           20640 non-null  float64
 1   latitude            20640 non-null  float64
 2   housing_median_age  20640 non-null  float64
 3   total_rooms         20640 non-null  float64
 4   total_bedrooms      20433 non-null  float64
 5   population          20640 non-null  float64
 6   households          20640 non-null  float64
 7   median_income       20640 non-null  float64
 8   median_house_value  20640 non-null  float64
 9   ocean_proximity     20640 non-null  object 
dtypes: float64(9), object(1)
memory usage: 1.7+ MB
data.describe()
longitude	latitude	housing_median_age	total_rooms	total_bedrooms	population	households	median_income	median_house_value
count	20640.000000	20640.000000	20640.000000	20640.000000	20433.000000	20640.000000	20640.000000	20640.000000	20640.000000
mean	-119.569704	35.631861	28.639486	2635.763081	537.870553	1425.476744	499.539680	3.870671	206855.816909
std	2.003532	2.135952	12.585558	2181.615252	421.385070	1132.462122	382.329753	1.899822	115395.615874
min	-124.350000	32.540000	1.000000	2.000000	1.000000	3.000000	1.000000	0.499900	14999.000000
25%	-121.800000	33.930000	18.000000	1447.750000	296.000000	787.000000	280.000000	2.563400	119600.000000
50%	-118.490000	34.260000	29.000000	2127.000000	435.000000	1166.000000	409.000000	3.534800	179700.000000
75%	-118.010000	37.710000	37.000000	3148.000000	647.000000	1725.000000	605.000000	4.743250	264725.000000
max	-114.310000	41.950000	52.000000	39320.000000	6445.000000	35682.000000	6082.000000	15.000100	500001.000000
%matplotlib inline
import matplotlib.pyplot as plt
data.hist(bins=50, figsize=(20,15))
plt.show()

3. Missing Data
Pandas yardımıyla yatak odasındaki eksik verilerin olduğunu bulduk

missing_values_count = data.isnull().sum() 
missing_values_count
longitude               0
latitude                0
housing_median_age      0
total_rooms             0
total_bedrooms        207
population              0
households              0
median_income           0
median_house_value      0
ocean_proximity         0
dtype: int64
# data.total_bedrooms.fillna(method="ffill")  # Üstündeki değer ile doldurma şekli
# data.total_bedrooms.fillna(method="bfill")  # Altında Değer ile Doldurma Şekli
# data.total_bedrooms.fillna(value=0)         # Sabit değer atama ile doldurma şekli
data['total_bedrooms'] = data.total_bedrooms.fillna(data.total_bedrooms.median())
Pandas veri kümesinde bulunan 270 tane eksik (missing) değeri medyan değerleriyle doldurduk ve doldurduğumuza emin olmak için tekrardan isnull() komutu ile kontrol ettik

data.isnull().sum()
longitude             0
latitude              0
housing_median_age    0
total_rooms           0
total_bedrooms        0
population            0
households            0
median_income         0
median_house_value    0
ocean_proximity       0
dtype: int64
4. Categorical data
data_ocean_proximity = data[["ocean_proximity"]]
data_ocean_proximity.head(10)
ocean_proximity
18799	INLAND
18235	NEAR BAY
4158	<1H OCEAN
1006	INLAND
14926	NEAR OCEAN
15390	<1H OCEAN
17541	<1H OCEAN
8102	NEAR OCEAN
5038	<1H OCEAN
14476	NEAR OCEAN
4.1 Ordinal Encoder
from sklearn.preprocessing import OrdinalEncoder

ordinal_encoder = OrdinalEncoder()
data_ocean_proximity_encoded = ordinal_encoder.fit_transform(data_ocean_proximity)
data_ocean_proximity_encoded[:10]
array([[1.],
       [3.],
       [0.],
       [1.],
       [4.],
       [0.],
       [0.],
       [4.],
       [0.],
       [4.]])
ordinal_encoder.categories_
[array(['<1H OCEAN', 'INLAND', 'ISLAND', 'NEAR BAY', 'NEAR OCEAN'],
       dtype=object)]
4.2 One Hot Encoder
from sklearn.preprocessing import OneHotEncoder

onehot_encoder = OneHotEncoder()
data_ocean_proximity_onehotencoder = onehot_encoder.fit_transform(data_ocean_proximity)
data_ocean_proximity_onehotencoder.toarray()
array([[0., 1., 0., 0., 0.],
       [0., 0., 0., 1., 0.],
       [1., 0., 0., 0., 0.],
       ...,
       [0., 1., 0., 0., 0.],
       [0., 1., 0., 0., 0.],
       [1., 0., 0., 0., 0.]])
5. Train-Validation-Test
data.ocean_proximity  = data_ocean_proximity_encoded
longitude	latitude	housing_median_age	total_rooms	total_bedrooms	population	households	median_income	median_house_value	ocean_proximity
18799	-121.89	40.97	26.0	1183.0	276.0	513.0	206.0	2.2250	52000.0	1.0
18235	-122.10	37.40	23.0	1755.0	508.0	1374.0	506.0	4.3077	293500.0	3.0
4158	-118.19	34.11	40.0	1266.0	348.0	1032.0	315.0	2.1667	150000.0	0.0
1006	-121.75	37.69	26.0	2647.0	536.0	1422.0	522.0	3.7212	183800.0	1.0
14926	-117.01	32.67	17.0	2319.0	348.0	1125.0	337.0	5.5510	266900.0	4.0
...	...	...	...	...	...	...	...	...	...	...
11504	-118.10	33.74	37.0	997.0	262.0	531.0	282.0	4.7773	400000.0	4.0
3744	-118.40	34.16	35.0	1354.0	284.0	501.0	262.0	3.8056	384700.0	0.0
13263	-117.66	34.10	37.0	1971.0	345.0	939.0	358.0	3.4634	145300.0	1.0
1031	-120.79	38.54	34.0	1133.0	254.0	495.0	187.0	2.0500	68900.0	1.0
17836	-121.86	37.41	16.0	2411.0	420.0	1671.0	442.0	6.5004	263600.0	0.0
20640 rows × 10 columns

X = data.drop("median_house_value", axis=1)
y = data.median_house_value
from sklearn.model_selection import train_test_split
# Veri seti öncelikle train ve test olarak ikiye ayrılıyor
x_train, x_test, y_train, y_test = train_test_split(x, # bağımsız değişkenler
                                                    y, # target değişken
                                                    test_size = 0.2, # test için ayrılan küme yüzdesi
                                                    shuffle = False, # Satırları dikey yönlü karıştırır
                                                    random_state = None)
# Train veri seti kendi içinden (%25) validation bölümünü ayırıyor
x_train, x_val, y_train, y_val = train_test_split(x_train, y_train,
                                                  test_size = 0.25,
                                                  shuffle = False)
# tüm veri setleri
all = {"x train" : x_train,
       "x validation" : x_val,
       "x test" : x_test,
       "y train" : y_train,
       "y validation": y_val,
       "y test": y_test}

# Oluşan veri setlerinin özellikleri
for i in all:
    print(f"{i} satır sayısı: {len(all.get(i))}")
x train satır sayısı: 12384
x validation satır sayısı: 4128
x test satır sayısı: 4128
y train satır sayısı: 12384
y validation satır sayısı: 4128
y test satır sayısı: 4128
6.Machine Learning Algorithm
from sklearn.ensemble import RandomForestRegressor
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from math import sqrt

def evaluate_models(X, y):
    # Split the data into training and testing sets
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

    # Train and evaluate a random forest regressor
    rf = RandomForestRegressor(n_estimators=100, random_state=42)
    rf.fit(X_train, y_train)
    y_pred = rf.predict(X_test)
    rmse = sqrt(mean_squared_error(y_test, y_pred))
    print(f"Random Forest RMSE: {rmse:.2f}")

    # Train and evaluate a linear regression model
    lr = LinearRegression()
    lr.fit(X_train, y_train)
    y_pred = lr.predict(X_test)
    rmse = sqrt(mean_squared_error(y_test, y_pred))
    print(f"Linear Regression RMSE: {rmse:.2f}")
    
    from sklearn.svm import SVR

    # Train and evaluate a support vector machine
    svm = SVR(kernel="linear")
    svm.fit(X_train, y_train)
    y_pred = svm.predict(X_test)
    rmse = sqrt(mean_squared_error(y_test, y_pred))
    print(f"SVM RMSE: {rmse:.2f}")
evaluate_models(X, y)
Random Forest RMSE: 49608.59
Linear Regression RMSE: 69528.15
SVM RMSE: 93123.10
 
