# 1 공공데이터 상권정보 분석 ( 의료기관)   
  
  - https://www.data.go.kr/data/15012005/fileData.do#layer_data_infomation
  - 국가중점데이터인 상권정보 살펴보기.


# 1.1 필요한 라이브러리 


```python
import pandas as pd
import numpy as np
import seaborn as sns

```

# 1.2 시각화를 위한 폰트 설정


```python
import matplotlib.pyplot as plt
# 윈도우의 한글 폰트 설정
plt.rc('font', family = 'Malgun Gothic')

# 시각화 그래프가 노트북 안에 보이게 하기
%matplotlib inline
```


```python
from IPython.display import set_matplotlib_formats
set_matplotlib_formats('retina')
```

# 1.3 데이터 로드하기


```python
df = pd.read_csv('./소상공인시장진흥공단_상가업소정보_의료기관_201909.csv',low_memory = False)

df.shape
```




    (91335, 39)




```python
# 위에서 5개 출력
df.head()
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19956873</td>
      <td>하나산부인과</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B10</td>
      <td>산부인과</td>
      <td>Q86201</td>
      <td>...</td>
      <td>4127310900110810000010857</td>
      <td>산호한양아파트</td>
      <td>경기도 안산시 단원구 달미로 10</td>
      <td>425764.0</td>
      <td>15236.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.814295</td>
      <td>37.336344</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20024149</td>
      <td>타워광명내과의원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B07</td>
      <td>내과/외과</td>
      <td>Q86201</td>
      <td>...</td>
      <td>1168011800104670014000001</td>
      <td>NaN</td>
      <td>서울특별시 강남구 언주로30길 39</td>
      <td>135270.0</td>
      <td>6292.0</td>
      <td>NaN</td>
      <td>4</td>
      <td>NaN</td>
      <td>127.053198</td>
      <td>37.488742</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20152277</td>
      <td>조정현신경외과의원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B15</td>
      <td>신경외과</td>
      <td>Q86201</td>
      <td>...</td>
      <td>4139013200117400001017064</td>
      <td>한라프라자</td>
      <td>경기도 시흥시 중심상가로 178</td>
      <td>429450.0</td>
      <td>15066.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.734841</td>
      <td>37.344955</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20350610</td>
      <td>한귀원정신과의원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B99</td>
      <td>기타병원</td>
      <td>NaN</td>
      <td>...</td>
      <td>2650010400100740001009932</td>
      <td>NaN</td>
      <td>부산광역시 수영구 수영로 688</td>
      <td>613100.0</td>
      <td>48266.0</td>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>129.115438</td>
      <td>35.166872</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20364049</td>
      <td>더블유스토어수지점</td>
      <td>수지점</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>G47811</td>
      <td>...</td>
      <td>4146510100107120002026238</td>
      <td>NaN</td>
      <td>경기도 용인시 수지구 문정로 32</td>
      <td>448170.0</td>
      <td>16837.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>127.095522</td>
      <td>37.323528</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>




```python
# tail : 뒤에 있는 5개 데이터 출력
df.tail()
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>91330</th>
      <td>16196725</td>
      <td>온누리약국</td>
      <td>베스트</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>G47811</td>
      <td>...</td>
      <td>3017011200115070000021096</td>
      <td>NaN</td>
      <td>대전광역시 서구 문예로 67</td>
      <td>302831.0</td>
      <td>35240.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.389865</td>
      <td>36.352728</td>
    </tr>
    <tr>
      <th>91331</th>
      <td>16192180</td>
      <td>리원</td>
      <td>봄산후조</td>
      <td>S</td>
      <td>의료</td>
      <td>S07</td>
      <td>의료관련서비스업</td>
      <td>S07A07</td>
      <td>산후조리원</td>
      <td>S96993</td>
      <td>...</td>
      <td>4128112300111460000011715</td>
      <td>청한프라자</td>
      <td>경기도 고양시 덕양구 성신로 14</td>
      <td>412827.0</td>
      <td>10503.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.830144</td>
      <td>37.627530</td>
    </tr>
    <tr>
      <th>91332</th>
      <td>16127538</td>
      <td>참좋은요양병원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B17</td>
      <td>노인/치매병원</td>
      <td>Q86102</td>
      <td>...</td>
      <td>2641010800105380001005572</td>
      <td>한신시티빌</td>
      <td>부산광역시 금정구 금강로 209</td>
      <td>609841.0</td>
      <td>46294.0</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>129.082790</td>
      <td>35.227138</td>
    </tr>
    <tr>
      <th>91333</th>
      <td>16108681</td>
      <td>경희중앙한의원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B06</td>
      <td>한의원</td>
      <td>Q86203</td>
      <td>...</td>
      <td>1174010500103450009002392</td>
      <td>NaN</td>
      <td>서울특별시 강동구 천중로 213</td>
      <td>134811.0</td>
      <td>5303.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.143958</td>
      <td>37.540993</td>
    </tr>
    <tr>
      <th>91334</th>
      <td>16109073</td>
      <td>천안김안과천안역본점의원</td>
      <td>NaN</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B13</td>
      <td>안과의원</td>
      <td>Q86201</td>
      <td>...</td>
      <td>4413110700102660017016314</td>
      <td>김안과</td>
      <td>충청남도 천안시 동남구 중앙로 92</td>
      <td>330952.0</td>
      <td>31127.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.152651</td>
      <td>36.806640</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>



# 1.5 데이터 요약하기
## 1.5.1 요약정보


```python
# info로 데이터 요약( 컬럼명 출력, 개수, d-type) 결측치 확인할 수 있음
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 91335 entries, 0 to 91334
    Data columns (total 39 columns):
     #   Column     Non-Null Count  Dtype  
    ---  ------     --------------  -----  
     0   상가업소번호     91335 non-null  int64  
     1   상호명        91335 non-null  object 
     2   지점명        1346 non-null   object 
     3   상권업종대분류코드  91335 non-null  object 
     4   상권업종대분류명   91335 non-null  object 
     5   상권업종중분류코드  91335 non-null  object 
     6   상권업종중분류명   91335 non-null  object 
     7   상권업종소분류코드  91335 non-null  object 
     8   상권업종소분류명   91335 non-null  object 
     9   표준산업분류코드   86413 non-null  object 
     10  표준산업분류명    86413 non-null  object 
     11  시도코드       90956 non-null  float64
     12  시도명        90956 non-null  object 
     13  시군구코드      90956 non-null  float64
     14  시군구명       90956 non-null  object 
     15  행정동코드      91335 non-null  int64  
     16  행정동명       90956 non-null  object 
     17  법정동코드      91280 non-null  float64
     18  법정동명       91280 non-null  object 
     19  지번코드       91335 non-null  int64  
     20  대지구분코드     91335 non-null  int64  
     21  대지구분명      91335 non-null  object 
     22  지번본번지      91335 non-null  int64  
     23  지번부번지      72079 non-null  float64
     24  지번주소       91335 non-null  object 
     25  도로명코드      91335 non-null  int64  
     26  도로명        91335 non-null  object 
     27  건물본번지      91335 non-null  int64  
     28  건물부번지      10604 non-null  float64
     29  건물관리번호     91335 non-null  object 
     30  건물명        46453 non-null  object 
     31  도로명주소      91335 non-null  object 
     32  구우편번호      91323 non-null  float64
     33  신우편번호      91333 non-null  float64
     34  동정보        7406 non-null   object 
     35  층정보        44044 non-null  object 
     36  호정보        15551 non-null  object 
     37  경도         91335 non-null  float64
     38  위도         91335 non-null  float64
    dtypes: float64(9), int64(7), object(23)
    memory usage: 27.2+ MB
    

# 1.5.2 컬럼명 보기



```python
# 컬럼명만 출력
df.columns

```




    Index(['상가업소번호', '상호명', '지점명', '상권업종대분류코드', '상권업종대분류명', '상권업종중분류코드',
           '상권업종중분류명', '상권업종소분류코드', '상권업종소분류명', '표준산업분류코드', '표준산업분류명', '시도코드',
           '시도명', '시군구코드', '시군구명', '행정동코드', '행정동명', '법정동코드', '법정동명', '지번코드',
           '대지구분코드', '대지구분명', '지번본번지', '지번부번지', '지번주소', '도로명코드', '도로명', '건물본번지',
           '건물부번지', '건물관리번호', '건물명', '도로명주소', '구우편번호', '신우편번호', '동정보', '층정보',
           '호정보', '경도', '위도'],
          dtype='object')



# 1.5.3 데이터 타입


```python
# 데이터 타입만 출력
df.dtypes
```




    상가업소번호         int64
    상호명           object
    지점명           object
    상권업종대분류코드     object
    상권업종대분류명      object
    상권업종중분류코드     object
    상권업종중분류명      object
    상권업종소분류코드     object
    상권업종소분류명      object
    표준산업분류코드      object
    표준산업분류명       object
    시도코드         float64
    시도명           object
    시군구코드        float64
    시군구명          object
    행정동코드          int64
    행정동명          object
    법정동코드        float64
    법정동명          object
    지번코드           int64
    대지구분코드         int64
    대지구분명         object
    지번본번지          int64
    지번부번지        float64
    지번주소          object
    도로명코드          int64
    도로명           object
    건물본번지          int64
    건물부번지        float64
    건물관리번호        object
    건물명           object
    도로명주소         object
    구우편번호        float64
    신우편번호        float64
    동정보           object
    층정보           object
    호정보           object
    경도           float64
    위도           float64
    dtype: object



# 1.6 결측치


```python
#각 컬럼의 결측치 개수 합
null_count = df.isnull().sum()
```


```python
# 위에서 구한 결측치를 .plot.bar를 통해 막대그래프로 표현.
# null_count.plot()
#null_count.plot.bar()

# 글씨 회전
#rot = null_count.plot.bar(rot = 60)

# x, y축 전환, 사이즈 조절
inv = null_count.plot.barh(figsize=(5,7))
```


![png](output_18_0.png)



```python
# 위에서 계산한 결측치 수를 reset_index를 통해 데이터 프레임으로 만들기
# df_null_count 변수에 결과를 담아서 head로 미리보기

df_null_count = null_count.reset_index()
df_null_count.head()
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
      <th>index</th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>상가업소번호</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>상호명</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지점명</td>
      <td>89989</td>
    </tr>
    <tr>
      <th>3</th>
      <td>상권업종대분류코드</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>상권업종대분류명</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



# 1.7 컬럼명 변경하기


```python
# df_null_count 변수에 담겨있는 컬럼의 이름을 '컬럼명', '결측치 수'로 변경하기.

df_null_count.columns = ['컬럼명', '결측치 수']
df_null_count.head()
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
      <th>컬럼명</th>
      <th>결측치 수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>상가업소번호</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>상호명</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지점명</td>
      <td>89989</td>
    </tr>
    <tr>
      <th>3</th>
      <td>상권업종대분류코드</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>상권업종대분류명</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



# 1.8 정렬하기


```python
# df_null_count 데이터프레임에 있는 결측치 수 컬럼을 sort_values를 통해 정렬
# 결측치 수가 적은 순으로 출력
# df_null_count.sort_values(by='결측치 수')

# 결측 수가 많은 순으로  출력
df_null_count_top = df_null_count.sort_values(by='결측치 수', ascending=False).head(10)
```

 # 1.9 특정 컬럼만 불러오기
 


```python
# 지점명 컬럼을 불러오기
# nan == Not a Number 의 약자로 결측치 의미

df['지점명'].head()
```




    0    NaN
    1    NaN
    2    NaN
    3    NaN
    4    수지점
    Name: 지점명, dtype: object




```python
# '컬럼명'이라는 컬럼의 값만 가져와서 drop_columns라는 변수에 담기
# 왜냐하면 결측치가 너무 많기 때문에
drop_columns = df_null_count_top['컬럼명'].tolist()
```


```python
# drop_columns 변수로 해당 컬럼 정보만 데이터프레임에서 가져오기
df[drop_columns].head()
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
      <th>지점명</th>
      <th>동정보</th>
      <th>건물부번지</th>
      <th>호정보</th>
      <th>층정보</th>
      <th>건물명</th>
      <th>지번부번지</th>
      <th>표준산업분류코드</th>
      <th>표준산업분류명</th>
      <th>시도코드</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>산호한양아파트</td>
      <td>NaN</td>
      <td>Q86201</td>
      <td>일반 의원</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4</td>
      <td>NaN</td>
      <td>14.0</td>
      <td>Q86201</td>
      <td>일반 의원</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>한라프라자</td>
      <td>1.0</td>
      <td>Q86201</td>
      <td>일반 의원</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수지점</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>G47811</td>
      <td>의약품 및 의료용품 소매업</td>
      <td>41.0</td>
    </tr>
  </tbody>
</table>
</div>



# 1.10 제거하기



```python
#  axis 0: 행 , 1:열 // 열 기준으로 제거 
print(df.shape)
df = df.drop(drop_columns, axis= 1)
print(df.shape)
```

    (91335, 39)
    (91335, 29)
    


```python
# 제거 결과를 info로 확인한다.

df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 91335 entries, 0 to 91334
    Data columns (total 29 columns):
     #   Column     Non-Null Count  Dtype  
    ---  ------     --------------  -----  
     0   상가업소번호     91335 non-null  int64  
     1   상호명        91335 non-null  object 
     2   상권업종대분류코드  91335 non-null  object 
     3   상권업종대분류명   91335 non-null  object 
     4   상권업종중분류코드  91335 non-null  object 
     5   상권업종중분류명   91335 non-null  object 
     6   상권업종소분류코드  91335 non-null  object 
     7   상권업종소분류명   91335 non-null  object 
     8   시도명        90956 non-null  object 
     9   시군구코드      90956 non-null  float64
     10  시군구명       90956 non-null  object 
     11  행정동코드      91335 non-null  int64  
     12  행정동명       90956 non-null  object 
     13  법정동코드      91280 non-null  float64
     14  법정동명       91280 non-null  object 
     15  지번코드       91335 non-null  int64  
     16  대지구분코드     91335 non-null  int64  
     17  대지구분명      91335 non-null  object 
     18  지번본번지      91335 non-null  int64  
     19  지번주소       91335 non-null  object 
     20  도로명코드      91335 non-null  int64  
     21  도로명        91335 non-null  object 
     22  건물본번지      91335 non-null  int64  
     23  건물관리번호     91335 non-null  object 
     24  도로명주소      91335 non-null  object 
     25  구우편번호      91323 non-null  float64
     26  신우편번호      91333 non-null  float64
     27  경도         91335 non-null  float64
     28  위도         91335 non-null  float64
    dtypes: float64(6), int64(7), object(16)
    memory usage: 20.2+ MB
    

---

# 1.11 기초 통계값 보기

### 1. 11. 1 기초 통계 수치


```python
# 자료값
df.dtypes
```




    상가업소번호         int64
    상호명           object
    상권업종대분류코드     object
    상권업종대분류명      object
    상권업종중분류코드     object
    상권업종중분류명      object
    상권업종소분류코드     object
    상권업종소분류명      object
    시도명           object
    시군구코드        float64
    시군구명          object
    행정동코드          int64
    행정동명          object
    법정동코드        float64
    법정동명          object
    지번코드           int64
    대지구분코드         int64
    대지구분명         object
    지번본번지          int64
    지번주소          object
    도로명코드          int64
    도로명           object
    건물본번지          int64
    건물관리번호        object
    도로명주소         object
    구우편번호        float64
    신우편번호        float64
    경도           float64
    위도           float64
    dtype: object




```python
# 특정 컬럼(위도)의 평균값 
df['위도'].mean()
```




    36.62471119236673




```python
# 특정 컬럼(위도)의 평균값 
df['위도'].median()
```




    37.23465231770329




```python
# 특정 컬럼(위도)의 최댓값 
df['위도'].max()
```




    38.499658570559795




```python
# 특정 컬럼(위도)의 최솟값 
df['위도'].min()
```




    33.2192896688307




```python
# 개수
df['위도'].count()
```




    91335



# 1.11.2 기초통계값 요약하기 - describe

- describe를 사용하면, 데이터를 요약해 볼 수 있다.
- 기본적으로 수치형 데이터를 요약해서 보여준다.
- 데이터 개수, 평균, 표준편차, 최솟값, 제 1사분위수 부터 제3사분위수, 최댓값을 볼 수 있다.


```python
# 위도를 describe로 요약

df['위도'].describe()
```




    count    91335.000000
    mean        36.624711
    std          1.041361
    min         33.219290
    25%         35.811830
    50%         37.234652
    75%         37.507463
    max         38.499659
    Name: 위도, dtype: float64




```python
# 2게의 컬럼을 describe로 요약하기.
# 2개의 컬럼을 불러올때는, 리스트 형태로 불러오기.

df[['위도','경도']]
df[['위도','경도']].describe()
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
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>91335.000000</td>
      <td>91335.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>36.624711</td>
      <td>127.487524</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.041361</td>
      <td>0.842877</td>
    </tr>
    <tr>
      <th>min</th>
      <td>33.219290</td>
      <td>124.717632</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>35.811830</td>
      <td>126.914297</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>37.234652</td>
      <td>127.084550</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>37.507463</td>
      <td>128.108919</td>
    </tr>
    <tr>
      <th>max</th>
      <td>38.499659</td>
      <td>130.909912</td>
    </tr>
  </tbody>
</table>
</div>




```python
# describe로 수치형(number) 데이터타입의 요약보기
df.describe(include='number')
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
      <th>상가업소번호</th>
      <th>시군구코드</th>
      <th>행정동코드</th>
      <th>법정동코드</th>
      <th>지번코드</th>
      <th>대지구분코드</th>
      <th>지번본번지</th>
      <th>도로명코드</th>
      <th>건물본번지</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>9.133500e+04</td>
      <td>90956.000000</td>
      <td>9.133500e+04</td>
      <td>9.128000e+04</td>
      <td>9.133500e+04</td>
      <td>91335.000000</td>
      <td>91335.000000</td>
      <td>9.133500e+04</td>
      <td>91335.000000</td>
      <td>91323.000000</td>
      <td>91333.00000</td>
      <td>91335.000000</td>
      <td>91335.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.121818e+07</td>
      <td>32898.381877</td>
      <td>3.293232e+09</td>
      <td>3.293385e+09</td>
      <td>3.293191e+18</td>
      <td>1.001336</td>
      <td>587.534549</td>
      <td>3.293207e+11</td>
      <td>251.200482</td>
      <td>428432.911085</td>
      <td>28085.47698</td>
      <td>127.487524</td>
      <td>36.624711</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.042828e+06</td>
      <td>12985.393171</td>
      <td>1.297387e+09</td>
      <td>1.297706e+09</td>
      <td>1.297393e+18</td>
      <td>0.036524</td>
      <td>582.519364</td>
      <td>1.297391e+11</td>
      <td>477.456487</td>
      <td>193292.339066</td>
      <td>18909.01455</td>
      <td>0.842877</td>
      <td>1.041361</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.901108e+06</td>
      <td>11110.000000</td>
      <td>1.111052e+09</td>
      <td>1.111010e+09</td>
      <td>1.111010e+18</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.111020e+11</td>
      <td>0.000000</td>
      <td>100011.000000</td>
      <td>1000.00000</td>
      <td>124.717632</td>
      <td>33.219290</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.001931e+07</td>
      <td>26350.000000</td>
      <td>2.635065e+09</td>
      <td>2.635011e+09</td>
      <td>2.635011e+18</td>
      <td>1.000000</td>
      <td>162.000000</td>
      <td>2.635042e+11</td>
      <td>29.000000</td>
      <td>302120.000000</td>
      <td>11681.00000</td>
      <td>126.914297</td>
      <td>35.811830</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.211900e+07</td>
      <td>41117.000000</td>
      <td>4.111758e+09</td>
      <td>4.111710e+09</td>
      <td>4.111711e+18</td>
      <td>1.000000</td>
      <td>462.000000</td>
      <td>4.111743e+11</td>
      <td>92.000000</td>
      <td>440300.000000</td>
      <td>24353.00000</td>
      <td>127.084550</td>
      <td>37.234652</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.480984e+07</td>
      <td>43113.000000</td>
      <td>4.311370e+09</td>
      <td>4.311311e+09</td>
      <td>4.311311e+18</td>
      <td>1.000000</td>
      <td>858.000000</td>
      <td>4.311332e+11</td>
      <td>257.000000</td>
      <td>602811.000000</td>
      <td>46044.00000</td>
      <td>128.108919</td>
      <td>37.507463</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.852470e+07</td>
      <td>50130.000000</td>
      <td>5.013061e+09</td>
      <td>5.013032e+09</td>
      <td>5.013061e+18</td>
      <td>2.000000</td>
      <td>7338.000000</td>
      <td>5.013049e+11</td>
      <td>8795.000000</td>
      <td>799801.000000</td>
      <td>63643.00000</td>
      <td>130.909912</td>
      <td>38.499659</td>
    </tr>
  </tbody>
</table>
</div>




```python
# describe로 문자열(object) 데이터타입의 요약보기 ( 결측치 제거 후 )
df.describe(include='object')
# unique : 서로 다른 값의 개수, top: 제일 많이 나온 문자열이고, freq: top의 문자열 등장 횟수이다.
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
      <th>상호명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
      <th>법정동명</th>
      <th>대지구분명</th>
      <th>지번주소</th>
      <th>도로명</th>
      <th>건물관리번호</th>
      <th>도로명주소</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>90956</td>
      <td>90956</td>
      <td>90956</td>
      <td>91280</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>56910</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>34</td>
      <td>34</td>
      <td>17</td>
      <td>228</td>
      <td>2791</td>
      <td>2822</td>
      <td>2</td>
      <td>53118</td>
      <td>16610</td>
      <td>54142</td>
      <td>54031</td>
    </tr>
    <tr>
      <th>top</th>
      <td>리원</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>경기도</td>
      <td>서구</td>
      <td>중앙동</td>
      <td>중동</td>
      <td>대지</td>
      <td>서울특별시 동대문구 제기동 965-1</td>
      <td>서울특별시 강남구 강남대로</td>
      <td>1123010300109650001031604</td>
      <td>서울특별시 동대문구 약령중앙로8길 10</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>152</td>
      <td>91335</td>
      <td>91335</td>
      <td>60774</td>
      <td>60774</td>
      <td>18964</td>
      <td>18964</td>
      <td>21374</td>
      <td>3165</td>
      <td>1856</td>
      <td>874</td>
      <td>91213</td>
      <td>198</td>
      <td>326</td>
      <td>198</td>
      <td>198</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe(include='all')
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>...</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물관리번호</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>9.133500e+04</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>91335</td>
      <td>90956</td>
      <td>90956.000000</td>
      <td>...</td>
      <td>91335</td>
      <td>9.133500e+04</td>
      <td>91335</td>
      <td>91335.000000</td>
      <td>91335</td>
      <td>91335</td>
      <td>91323.000000</td>
      <td>91333.00000</td>
      <td>91335.000000</td>
      <td>91335.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>56910</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>34</td>
      <td>34</td>
      <td>17</td>
      <td>NaN</td>
      <td>...</td>
      <td>53118</td>
      <td>NaN</td>
      <td>16610</td>
      <td>NaN</td>
      <td>54142</td>
      <td>54031</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>리원</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>경기도</td>
      <td>NaN</td>
      <td>...</td>
      <td>서울특별시 동대문구 제기동 965-1</td>
      <td>NaN</td>
      <td>서울특별시 강남구 강남대로</td>
      <td>NaN</td>
      <td>1123010300109650001031604</td>
      <td>서울특별시 동대문구 약령중앙로8길 10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>152</td>
      <td>91335</td>
      <td>91335</td>
      <td>60774</td>
      <td>60774</td>
      <td>18964</td>
      <td>18964</td>
      <td>21374</td>
      <td>NaN</td>
      <td>...</td>
      <td>198</td>
      <td>NaN</td>
      <td>326</td>
      <td>NaN</td>
      <td>198</td>
      <td>198</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.121818e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32898.381877</td>
      <td>...</td>
      <td>NaN</td>
      <td>3.293207e+11</td>
      <td>NaN</td>
      <td>251.200482</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>428432.911085</td>
      <td>28085.47698</td>
      <td>127.487524</td>
      <td>36.624711</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.042828e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12985.393171</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.297391e+11</td>
      <td>NaN</td>
      <td>477.456487</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>193292.339066</td>
      <td>18909.01455</td>
      <td>0.842877</td>
      <td>1.041361</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.901108e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11110.000000</td>
      <td>...</td>
      <td>NaN</td>
      <td>1.111020e+11</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>100011.000000</td>
      <td>1000.00000</td>
      <td>124.717632</td>
      <td>33.219290</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.001931e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>26350.000000</td>
      <td>...</td>
      <td>NaN</td>
      <td>2.635042e+11</td>
      <td>NaN</td>
      <td>29.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>302120.000000</td>
      <td>11681.00000</td>
      <td>126.914297</td>
      <td>35.811830</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.211900e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41117.000000</td>
      <td>...</td>
      <td>NaN</td>
      <td>4.111743e+11</td>
      <td>NaN</td>
      <td>92.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>440300.000000</td>
      <td>24353.00000</td>
      <td>127.084550</td>
      <td>37.234652</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.480984e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>43113.000000</td>
      <td>...</td>
      <td>NaN</td>
      <td>4.311332e+11</td>
      <td>NaN</td>
      <td>257.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>602811.000000</td>
      <td>46044.00000</td>
      <td>128.108919</td>
      <td>37.507463</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.852470e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>50130.000000</td>
      <td>...</td>
      <td>NaN</td>
      <td>5.013049e+11</td>
      <td>NaN</td>
      <td>8795.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>799801.000000</td>
      <td>63643.00000</td>
      <td>130.909912</td>
      <td>38.499659</td>
    </tr>
  </tbody>
</table>
<p>11 rows × 29 columns</p>
</div>



# 1.11.2 기초통계값 요약하기 - describe
- unique로 중복을 제거한 값을 보고 nunique로 개수를 세기



```python
# '상권업종대분류명'
df['상권업종대분류명'].unique()
```




    array(['의료'], dtype=object)




```python
# unique의 개수 
df['상권업종대분류명'].nunique()
```




    1




```python
# '상권업종중분류명'
df['상권업종중분류명'].unique()
```




    array(['병원', '약국/한약방', '수의업', '유사의료업', '의료관련서비스업'], dtype=object)




```python
df['상권업종중분류명'].nunique()
```




    5




```python
# '상권업종소분류명'
df['상권업종소분류명'].unique()
```




    array(['산부인과', '내과/외과', '신경외과', '기타병원', '약국', '동물병원', '한약방', '탕제원',
           '정형/성형외과', '소아과', '이비인후과의원', '노인/치매병원', '언어치료', '수의업-종합', '한의원',
           '치과의원', '침구원', '일반병원', '안과의원', '조산원', '한방병원', '종합병원', '유사의료업기타',
           '응급구조대', '혈액원', '치과병원', '척추교정치료', '피부과', '비뇨기과', '치과기공소', '산후조리원',
           '접골원', '수의업-기타', '제대혈'], dtype=object)




```python
# df['상권업종소분류명'].nunique()
len(df['상권업종소분류명'].unique())
```




    34



# 1.11.4 그룹화된 요약값 보기 : Value_counts
- value_counts를 사용하면 카테고리 형태의 데이터 개수를 세어볼 수 있다.



```python
# value_counts를 사용하면 카테고리 형태의 데이터 개수를 세어볼 수 있다.
# 시도명을 세어보기
city = df['시도명'].value_counts()
city
```




    경기도        21374
    서울특별시      18943
    부산광역시       6473
    경상남도        4973
    인천광역시       4722
    대구광역시       4597
    경상북도        4141
    전라북도        3894
    충청남도        3578
    전라남도        3224
    광주광역시       3214
    대전광역시       3067
    충청북도        2677
    강원도         2634
    울산광역시       1997
    제주특별자치도     1095
    세종특별자치시      353
    Name: 시도명, dtype: int64




```python
# normalize=True 옵션을 사용하면 df에서의 비율을 구할 수 있다.

city_normalize = df['시도명'].value_counts(normalize=True)
city_normalize
```




    경기도        0.234993
    서울특별시      0.208266
    부산광역시      0.071166
    경상남도       0.054675
    인천광역시      0.051915
    대구광역시      0.050541
    경상북도       0.045528
    전라북도       0.042812
    충청남도       0.039338
    전라남도       0.035446
    광주광역시      0.035336
    대전광역시      0.033720
    충청북도       0.029432
    강원도        0.028959
    울산광역시      0.021956
    제주특별자치도    0.012039
    세종특별자치시    0.003881
    Name: 시도명, dtype: float64




```python
# pandas에는 plot 기능을 내장하고 있다.
# 위에서 분석한 시도명 수를 막대그래프로 표현하기.
# city.plot.area()
# city.plot.bar()
city.plot.barh()
# city.plot.box()

```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd54daba88>




![png](output_56_1.png)



```python
# 판다스의 plot.pie()를 사용해서 파이그래이프로 그리기.
city_normalize.plot.pie(figsize=(7,7))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd54ecfb48>




![png](output_57_1.png)


--- 
- 데이터 요약하기 - seaborn 으로 빈도수 시각화 하기


```python
# seaborn의 countplot으로 그리기
sns.countplot(data=df , y='시도명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd58378d08>




![png](output_59_1.png)



```python
# '상권업종대분류명'으로 개수를 세어보기.
df['상권업종대분류명'].value_counts()
```




    의료    91335
    Name: 상권업종대분류명, dtype: int64




```python
# '상권업종중분류명'으로 개수를 세어보기.
c = df['상권업종중분류명'].value_counts()
c
```




    병원          60774
    약국/한약방      20923
    수의업          5323
    유사의료업        3774
    의료관련서비스업      541
    Name: 상권업종중분류명, dtype: int64




```python
# normalize=True를 사용해 비율 구하기.

n = df['상권업종중분류명'].value_counts(normalize= True)

n
```




    병원          0.665397
    약국/한약방      0.229080
    수의업         0.058280
    유사의료업       0.041320
    의료관련서비스업    0.005923
    Name: 상권업종중분류명, dtype: float64




```python
# 판다스의 plot.bar()를 사용해서 막대그래프 그리기. 
c.plot.bar(rot=0)
# n.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd58a201c8>




![png](output_63_1.png)



```python
# 판다스의 plot.pie()를 사용해서 파이 그래프 그리기.
n.plot.pie()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd5884a088>




![png](output_64_1.png)



```python
# '상권업종소분류명'에 대한 그룹화 된 값을 카운트하기.
k = df['상권업종소분류명'].value_counts()
k
```




    약국         18964
    치과의원       13731
    한의원        13211
    내과/외과      11374
    기타병원        4922
    일반병원        3385
    동물병원        3098
    정형/성형외과     2562
    소아과         2472
    수의업-종합      2216
    치과기공소       1724
    이비인후과의원     1486
    한약방         1442
    피부과         1273
    산부인과        1116
    노인/치매병원     1055
    안과의원        1042
    비뇨기과         809
    종합병원         762
    치과병원         756
    언어치료         664
    유사의료업기타      629
    탕제원          517
    산후조리원        511
    신경외과         421
    한방병원         397
    척추교정치료       338
    침구원          154
    혈액원          130
    응급구조대        125
    조산원           30
    수의업-기타         9
    접골원            9
    제대혈            1
    Name: 상권업종소분류명, dtype: int64




```python
# '상권업종 소분류명'으로 개수를 세어보기.
# 판다스의 plot.ba()를 사용해서 막대그래프 그리기.
k.plot.barh(figsize=(6, 8),grid=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd58898c08>




![png](output_66_1.png)


# 1.12 데이터 색인하기
- 특정 데이터만 모아서 따로 보기.


```python
# '상권업종중분류명'이 '약국/한약방'인 데이터만 가져온다.
#  df_medical 이라는 변수에 담는다.
# head()를 통해 미리보기를 한다.

df['상권업종중분류명'] == '약국/한약방'      # 불리언 인덱싱 (True/ False)
df_medical = df[df['상권업종중분류명'] == '약국/한약방'].copy()  # 약국/한약방 data만 가져오기. (원본에 영향 없도록 copy)
df_medical.head()
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>...</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물관리번호</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>20364049</td>
      <td>더블유스토어수지점</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>경기도</td>
      <td>41465.0</td>
      <td>...</td>
      <td>경기도 용인시 수지구 풍덕천동 712-2</td>
      <td>414653205024</td>
      <td>경기도 용인시 수지구 문정로</td>
      <td>32</td>
      <td>4146510100107120002026238</td>
      <td>경기도 용인시 수지구 문정로 32</td>
      <td>448170.0</td>
      <td>16837.0</td>
      <td>127.095522</td>
      <td>37.323528</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20733252</td>
      <td>춘산한약방</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A02</td>
      <td>한약방</td>
      <td>강원도</td>
      <td>42110.0</td>
      <td>...</td>
      <td>강원도 춘천시 중앙로2가 99</td>
      <td>421104454113</td>
      <td>강원도 춘천시 낙원길</td>
      <td>50</td>
      <td>4211010500101000000023668</td>
      <td>강원도 춘천시 낙원길 50</td>
      <td>200042.0</td>
      <td>24273.0</td>
      <td>127.726905</td>
      <td>37.880504</td>
    </tr>
    <tr>
      <th>7</th>
      <td>20582210</td>
      <td>부부탕제원</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A03</td>
      <td>탕제원</td>
      <td>충청북도</td>
      <td>43111.0</td>
      <td>...</td>
      <td>충청북도 청주시 상당구 금천동 187-17</td>
      <td>431114508623</td>
      <td>충청북도 청주시 상당구 중고개로337번길</td>
      <td>134</td>
      <td>4311112000101870017042942</td>
      <td>충청북도 청주시 상당구 중고개로337번길 134</td>
      <td>360802.0</td>
      <td>28726.0</td>
      <td>127.499206</td>
      <td>36.625355</td>
    </tr>
    <tr>
      <th>10</th>
      <td>21057519</td>
      <td>민생약국</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A01</td>
      <td>약국</td>
      <td>경상남도</td>
      <td>48890.0</td>
      <td>...</td>
      <td>경상남도 합천군 용주면 월평리 78-2</td>
      <td>488904844473</td>
      <td>경상남도 합천군 용주면 월평길</td>
      <td>149</td>
      <td>4889046030200780002048274</td>
      <td>경상남도 합천군 용주면 월평길 149-35</td>
      <td>678912.0</td>
      <td>50212.0</td>
      <td>128.118615</td>
      <td>35.575962</td>
    </tr>
    <tr>
      <th>13</th>
      <td>21217689</td>
      <td>제중당한약방</td>
      <td>S</td>
      <td>의료</td>
      <td>S02</td>
      <td>약국/한약방</td>
      <td>S02A02</td>
      <td>한약방</td>
      <td>전라남도</td>
      <td>46830.0</td>
      <td>...</td>
      <td>전라남도 영암군 도포면 덕화리 296</td>
      <td>468304685396</td>
      <td>전라남도 영암군 도포면 인덕길</td>
      <td>75</td>
      <td>4683035023102960000000001</td>
      <td>전라남도 영암군 도포면 인덕길 75-10</td>
      <td>526832.0</td>
      <td>58429.0</td>
      <td>126.630348</td>
      <td>34.834080</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 29 columns</p>
</div>




```python
# '상권업종대분류명'에서 '의료'데이터만 가져온다.
#  df.loc를 사용하면 행, 열을 함게 가져올 수 있다.
# 이 기능을 통해 '상권업종준분류명'만 가져온다.
# 가져온 결과를 value_counts를 통해 중분류의 개수를 세어본다.

'''
m = df['상권업종대분류명']=='의료'
df.loc[m,'상권업종중분류명'].value_counts()
'''
# 위와 똑같은 기능을 수행하는 코드를 아래와 같이 한 줄에 표현할 수도 있다.
df.loc[df['상권업종대분류명']=='의료','상권업종중분류명'].value_counts()
```




    병원          60774
    약국/한약방      20923
    수의업          5323
    유사의료업        3774
    의료관련서비스업      541
    Name: 상권업종중분류명, dtype: int64




```python
# 유사의료업만 따로 모으기
# 유사의료업만 df_medi 변수에 담기
df_medi = df[df['상권업종중분류명']=='유사의료업']
df_medi
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>...</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물관리번호</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>22</th>
      <td>21013731</td>
      <td>세종언어치료센터</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B07</td>
      <td>언어치료</td>
      <td>부산광역시</td>
      <td>26410.0</td>
      <td>...</td>
      <td>부산광역시 금정구 구서동 84-1</td>
      <td>264102000010</td>
      <td>부산광역시 금정구 중앙대로</td>
      <td>1817</td>
      <td>2641010700100840001017686</td>
      <td>부산광역시 금정구 중앙대로 1817-11</td>
      <td>609310.0</td>
      <td>46273.0</td>
      <td>129.091662</td>
      <td>35.246528</td>
    </tr>
    <tr>
      <th>40</th>
      <td>20933900</td>
      <td>고려수지침학회</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B03</td>
      <td>침구원</td>
      <td>경상남도</td>
      <td>48123.0</td>
      <td>...</td>
      <td>경상남도 창원시 성산구 상남동 5-2</td>
      <td>481234784088</td>
      <td>경상남도 창원시 성산구 마디미로4번길</td>
      <td>9</td>
      <td>4812312700100050002026799</td>
      <td>경상남도 창원시 성산구 마디미로4번길 9</td>
      <td>642832.0</td>
      <td>51495.0</td>
      <td>128.684678</td>
      <td>35.224113</td>
    </tr>
    <tr>
      <th>97</th>
      <td>21717820</td>
      <td>청명원</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B09</td>
      <td>유사의료업기타</td>
      <td>충청북도</td>
      <td>43760.0</td>
      <td>...</td>
      <td>충청북도 괴산군 청안면 금신리 241</td>
      <td>437604538132</td>
      <td>충청북도 괴산군 청안면 금신로1길</td>
      <td>93</td>
      <td>4376037022102410000007293</td>
      <td>충청북도 괴산군 청안면 금신로1길 93</td>
      <td>367831.0</td>
      <td>28050.0</td>
      <td>127.635740</td>
      <td>36.768935</td>
    </tr>
    <tr>
      <th>102</th>
      <td>21865854</td>
      <td>응급환자이송센터</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B01</td>
      <td>응급구조대</td>
      <td>대전광역시</td>
      <td>30140.0</td>
      <td>...</td>
      <td>대전광역시 중구 대사동 248-237</td>
      <td>301404295026</td>
      <td>대전광역시 중구 계룡로921번길</td>
      <td>40</td>
      <td>3014011000102480237013097</td>
      <td>대전광역시 중구 계룡로921번길 40</td>
      <td>301846.0</td>
      <td>34946.0</td>
      <td>127.417693</td>
      <td>36.321801</td>
    </tr>
    <tr>
      <th>108</th>
      <td>21914637</td>
      <td>태화아동발달지원센터</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B07</td>
      <td>언어치료</td>
      <td>대전광역시</td>
      <td>30140.0</td>
      <td>...</td>
      <td>대전광역시 중구 문화동 27</td>
      <td>301404295402</td>
      <td>대전광역시 중구 보문산로333번길</td>
      <td>29</td>
      <td>3014011600100270000008172</td>
      <td>대전광역시 중구 보문산로333번길 29</td>
      <td>301130.0</td>
      <td>35020.0</td>
      <td>127.412725</td>
      <td>36.312953</td>
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
      <th>91300</th>
      <td>16131218</td>
      <td>으뜸치과기공소</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B06</td>
      <td>치과기공소</td>
      <td>경상남도</td>
      <td>48170.0</td>
      <td>...</td>
      <td>경상남도 진주시 수정동 39-11</td>
      <td>481704797625</td>
      <td>경상남도 진주시 향교로18번길</td>
      <td>8</td>
      <td>4817011600100390011004490</td>
      <td>경상남도 진주시 향교로18번길 8</td>
      <td>660180.0</td>
      <td>52753.0</td>
      <td>128.084600</td>
      <td>35.197029</td>
    </tr>
    <tr>
      <th>91310</th>
      <td>16199325</td>
      <td>보령치과기공소</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B06</td>
      <td>치과기공소</td>
      <td>서울특별시</td>
      <td>11290.0</td>
      <td>...</td>
      <td>서울특별시 성북구 동소문동4가 103-11</td>
      <td>112903107003</td>
      <td>서울특별시 성북구 동소문로</td>
      <td>47</td>
      <td>1129010700101030014050661</td>
      <td>서울특별시 성북구 동소문로 47-15</td>
      <td>136821.0</td>
      <td>2832.0</td>
      <td>127.010602</td>
      <td>37.591455</td>
    </tr>
    <tr>
      <th>91311</th>
      <td>16199088</td>
      <td>점프셈교실</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B09</td>
      <td>유사의료업기타</td>
      <td>경상북도</td>
      <td>47130.0</td>
      <td>...</td>
      <td>경상북도 경주시 황성동 446</td>
      <td>471304715895</td>
      <td>경상북도 경주시 용담로104번길</td>
      <td>16</td>
      <td>4713012400104460000024894</td>
      <td>경상북도 경주시 용담로104번길 16</td>
      <td>780954.0</td>
      <td>38084.0</td>
      <td>129.211755</td>
      <td>35.865600</td>
    </tr>
    <tr>
      <th>91319</th>
      <td>16108560</td>
      <td>씨앤디자인치과기공소</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B06</td>
      <td>치과기공소</td>
      <td>서울특별시</td>
      <td>11545.0</td>
      <td>...</td>
      <td>서울특별시 금천구 가산동 60-25</td>
      <td>115453116013</td>
      <td>서울특별시 금천구 벚꽃로</td>
      <td>234</td>
      <td>1154510100100600025000001</td>
      <td>서울특별시 금천구 벚꽃로 234</td>
      <td>153798.0</td>
      <td>8513.0</td>
      <td>126.886122</td>
      <td>37.475986</td>
    </tr>
    <tr>
      <th>91327</th>
      <td>16190388</td>
      <td>오피스알파</td>
      <td>S</td>
      <td>의료</td>
      <td>S03</td>
      <td>유사의료업</td>
      <td>S03B06</td>
      <td>치과기공소</td>
      <td>경기도</td>
      <td>41173.0</td>
      <td>...</td>
      <td>경기도 안양시 동안구 호계동 970-24</td>
      <td>411734349013</td>
      <td>경기도 안양시 동안구 경수대로507번길</td>
      <td>28</td>
      <td>4117310400109700024005182</td>
      <td>경기도 안양시 동안구 경수대로507번길 28</td>
      <td>431849.0</td>
      <td>14120.0</td>
      <td>126.956365</td>
      <td>37.367779</td>
    </tr>
  </tbody>
</table>
<p>3774 rows × 29 columns</p>
</div>




```python
# 상호명을 그룹화해서 개수 세기.
# value_counts0를 사용해서 상위 10개 출력
df['상호명'].value_counts().head()
```




    리원       152
    온누리약국    149
    경희한의원    141
    우리약국     119
    중앙약국     111
    Name: 상호명, dtype: int64




```python

# df_medi 변수에서 상호명으로 개수 세기
# 가장 많은 상호 상위 10개 출력
df_medi['상호명'].value_counts().head(10)
```




    리원          32
    고려수지침       22
    대한적십자사      17
    헌혈의집        12
    수치과기공소      10
    고려수지침학회     10
    제일치과기공소      9
    스마일치과기공소     8
    어울림치과기공소     8
    아트치과기공소      8
    Name: 상호명, dtype: int64



# 1.12.1 여러 조건으로 색인하기


```python
# '상권업종소분류명'이 '약국'인 것과 '시도명'이 '서울특별시'인 데이터 가져오기.

df['상권업종소분류명'] == '약국'
df['시도명'] =='서울특별시'

# 한번에 표현하기 (서울에 있는 약국)
df_seoul_drug= df[(df['상권업종소분류명'] == '약국' )& (df['시도명'] =='서울특별시')]
df_seoul_drug.shape
```




    (3579, 29)



# 1.12.2 구별로 보기


```python
# 위에서 색인한 데이터로 '시군구명'으로 그룹화 개수 세어보기.
# 구별로 약국이 몇개가 있는지 확인
c = df_seoul_drug['시군구명'].value_counts()
c.head()
```




    강남구     374
    동대문구    261
    광진구     212
    서초구     191
    송파구     188
    Name: 시군구명, dtype: int64




```python
# normalize = True를 통해 비율을 구하기.
n = df_seoul_drug['시군구명'].value_counts(normalize = True)
n.head()
```




    강남구     0.104498
    동대문구    0.072925
    광진구     0.059234
    서초구     0.053367
    송파구     0.052529
    Name: 시군구명, dtype: float64




```python
# 위에서구한 결과를 판다스의 plot.bar()를 활용해 막대그래프로 그리기
c.plot.bar(rot= 60)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd590450c8>




![png](output_78_1.png)



```python
# '상권업종소분류명'이 '종합병원'인 것과
# '시도명'이 '서울특별시'인 데이터만 가져오기.
# 결과를 df_seoul_hospital에 할당해서 재사용.
df_seoul_hospital = df[(df['상권업종소분류명']=='종합병원' ) & (df['시도명']=='서울특별시')].copy()
# df_seoul_hospital
```


```python
# '시군구명'으로 그룹화해서 구별로 종합병원의 수를 세어보기.
df_seoul_hospital['시군구명'].value_counts()
```




    강남구     15
    영등포구     8
    광진구      6
    서초구      6
    송파구      5
    강동구      5
    중구       5
    서대문구     4
    양천구      4
    강북구      4
    도봉구      4
    성북구      3
    성동구      2
    중랑구      2
    동대문구     2
    노원구      2
    강서구      2
    금천구      2
    종로구      2
    구로구      2
    관악구      2
    용산구      1
    동작구      1
    은평구      1
    마포구      1
    Name: 시군구명, dtype: int64



# 1.12.3 텍스트 데이터 색인하기



```python
# 색인하기 전에 상호명 중에 종합병원이 아닌 데이터를 찾아봅니다.
df_seoul_hospital.loc[~df_seoul_hospital['상호명'].str.contains('종합병원'),'상호명'].unique()
```




    array(['대진의료재단', '홍익병원별관', 'SNUH', '평화드림여의도성모병원의료기매장', '한양', '백산의료재단친구병원',
           '서울보훈병원', '서울성모병원장례식장꽃배달', '서울대학교병원', '알콜중독및정신질환상담소',
           '강남성모병원장례식장꽃배달', '제일병원', '이랜드클리닉', '사랑나눔의료재단', '우울증센터', '성심의료재단',
           '다나의료재단', '서울아산병원신관', '원자력병원장례식장', '국민의원', '고려대학교구로병원', '학교법인일송학원',
           '삼성의료원장례식장', '희명스포츠의학센터인공신장실', '연세대학교의과대학강남세브란스', '국립정신병원',
           '코아클리닉', '수서제일의원', '사랑의의원', '한국전력공사부속한일병원', '신촌연세병원', '창동제일의원',
           '영동세브란스병원', '제일성심의원', '삼성의료재단강북삼성태', '서울시립보라매병원', '서울이의원',
           '서울대학교병원비상계획외래', '평화드림서울성모병원의료', '홍익병원', '사랑나눔의료재단서', '독일의원',
           '서울연합의원', '우신향병원', '동부제일병원', '아산재단금강병원', '명곡안연구소', '아산재단서울중앙병원',
           '메디힐특수여객', '삼성생명공익재단삼성서', '성광의료재단차병원', '한국건강관리협회서울특',
           '정해복지부설한신메디피아', '성베드로병원', '성애의료재단', '실로암의원', 'Y&T성모마취과', '광진성모의원',
           '서울현대의원', '이노신경과의원', '송정훼밀리의원', '서울중앙의원', '영남의료재단', '인제대학교서울백병원',
           '한국필의료재단', '세브란스의원', '가톨릭대학교성바오로병원장례식장', '서울연세의원', '사랑의병원',
           '성삼의료재단미즈메디병원', '씨엠충무병원', '성신의원', '원진재단부설녹색병원', '송파제일의원',
           '카톨릭성모의원', '한양성심의원', '관악성모의원', '강남센트럴병원', '우이한솔의원', '우리들병원',
           '서울성모병원어린이집', '건국대학교병원', '서울적십자병원', '북부성모의원', '한림대학교부속한강성심병원장례식장',
           '서울성모병원응급의료센터', '라마르의원', '가톨릭대학교여의도성모병원', '씨엠병원'], dtype=object)




```python
# 특정 단어가 들어가는 데이터만 가져옵니다. -의료기
df_seoul_hospital[df_seoul_hospital['상호명'].str.contains('의료기')]
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>시도명</th>
      <th>시군구코드</th>
      <th>...</th>
      <th>지번주소</th>
      <th>도로명코드</th>
      <th>도로명</th>
      <th>건물본번지</th>
      <th>건물관리번호</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1917</th>
      <td>23210677</td>
      <td>평화드림여의도성모병원의료기매장</td>
      <td>S</td>
      <td>의료</td>
      <td>S01</td>
      <td>병원</td>
      <td>S01B01</td>
      <td>종합병원</td>
      <td>서울특별시</td>
      <td>11560.0</td>
      <td>...</td>
      <td>서울특별시 영등포구 여의도동 62</td>
      <td>115603118001</td>
      <td>서울특별시 영등포구 63로</td>
      <td>10</td>
      <td>1156011000100620000031477</td>
      <td>서울특별시 영등포구 63로 10</td>
      <td>150713.0</td>
      <td>7345.0</td>
      <td>126.936693</td>
      <td>37.518296</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 29 columns</p>
</div>




```python
 # '꽃배달|의료기|장례식장|상담소|어린이집'은 종합병원과 무관하기 때문에 전처리를 위해 해당 텍스트를 한 번에 검색
# 제거할 데이터의 인덱스만 drop_row에 담아주고 list 형태로 변환한다.
drop_row = df_seoul_hospital[df_seoul_hospital['상호명'].str.contains('꽃배달|의료기|장례식장|상담소|어린이집')].index
drop_row = drop_row.tolist()
drop_row
```




    [1917, 2803, 4431, 4644, 7938, 10283, 47008, 60645, 70177]




```python
# 의원으로 끝나는 데이터도 종합병원으로 볼 수 없기 때문에 인덱스를 찾아서
# drop_row2에 담아주고 list 형태로 변환합니다.
# endswith : 마지막에 특정 스트링으로 끝나는 문자열 
drop_row2 = df_seoul_hospital[df_seoul_hospital['상호명'].str.endswith('의원')].index
drop_row2 = drop_row2.tolist()
drop_row2
```




    [8479,
     12854,
     13715,
     14966,
     16091,
     18047,
     20200,
     20415,
     30706,
     32889,
     34459,
     34720,
     35696,
     37251,
     45120,
     49626,
     51575,
     55133,
     56320,
     56404,
     56688,
     57551,
     62113,
     76508]




```python
# 삭제할 행을 drop_row에 합쳐준다.
drop_row =  drop_row + drop_row2
len(drop_row)
```




    33




```python
# 해당 셀을 삭제하고 삭제 전과 후의 행의 개수 비교하기
print(df_seoul_hospital.shape)
df_seoul_hospital = df_seoul_hospital.drop(drop_row, axis=0)
print(df_seoul_hospital.shape)
```

    (91, 29)
    (58, 29)
    


```python
# 시군구명에 따라 종합병원의 숫자를 count.plot으로 표현하기.
df_seoul_hospital['시군구명'].value_counts().plot.barh()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd590d2d08>




![png](output_88_1.png)



```python
plt.figure(figsize=(18,4))

sns.countplot(data=df_seoul_hospital, x='시군구명', 
             order=df_seoul_hospital['시군구명'].value_counts().index)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd5934fe48>




![png](output_89_1.png)



```python
df_seoul_hospital['상호명'].unique()
```




    array(['대진의료재단', '홍익병원별관', 'SNUH', '한양', '백산의료재단친구병원', '서울보훈병원',
           '서울대학교병원', '제일병원', '이랜드클리닉', '사랑나눔의료재단', '우울증센터', '성심의료재단',
           '다나의료재단', '서울아산병원신관', '고려대학교구로병원', '학교법인일송학원', '희명스포츠의학센터인공신장실',
           '연세대학교의과대학강남세브란스', '국립정신병원', '코아클리닉', '한국전력공사부속한일병원', '신촌연세병원',
           '영동세브란스병원', '삼성의료재단강북삼성태', '서울시립보라매병원', '서울대학교병원비상계획외래',
           '평화드림서울성모병원의료', '홍익병원', '사랑나눔의료재단서', '우신향병원', '동부제일병원', '아산재단금강병원',
           '명곡안연구소', '아산재단서울중앙병원', '메디힐특수여객', '삼성생명공익재단삼성서', '성광의료재단차병원',
           '한국건강관리협회서울특', '정해복지부설한신메디피아', '성베드로병원', '성애의료재단', 'Y&T성모마취과',
           '영남의료재단', '인제대학교서울백병원', '한국필의료재단', '사랑의병원', '성삼의료재단미즈메디병원',
           '씨엠충무병원', '원진재단부설녹색병원', '강남센트럴병원', '우리들병원', '건국대학교병원', '서울적십자병원',
           '서울성모병원응급의료센터', '가톨릭대학교여의도성모병원', '씨엠병원'], dtype=object)



# 1.12.4 특정 지역만 보기



```python
# 서울에 있는 데이터의 위도와 경도를 봅니다.
# 결과를 df_seoul 이라는 데이터프레임에 저장합니다.
# 새로운 변수에 데이터프레임을 저장식 copy()를 사용합니다.
df_seoul = df[df['시도명'] == '서울특별시'].copy()
df_seoul.shape
```




    (18943, 29)




```python
# matplit의 plot(bar)을 사용해서 위에서 만든 df_seoul 데이터프레임의 시군구명을 시각화 합니다.
df_seoul['시군구명'].value_counts().plot.bar(figsize=(10,4),rot=45)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd593fcd88>




![png](output_93_1.png)



```python
# seaborn의 countplot을 사용해서 위에서 만든 df_seoul 데이터프레임의 시군구명을 시각화 합니다.

plt.figure(figsize=(20,4))
sns.countplot(data=df_seoul, x ='시군구명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd59418348>




![png](output_94_1.png)



```python
# pandas의 plot.scatter를 통해 경도와 위도를 표시
df_seoul[['경도','위도','시군구명']].plot.scatter(x='경도',y='위도',figsize=(8,7),grid=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd59721148>




![png](output_95_1.png)



```python
# seaborn의 scatterplot을 통해 경도와 위도를 표시합니다.
plt.figure(figsize=(9,8))
sns.scatterplot(data=df_seoul, x='경도',y='위도',hue='시군구명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd59769e08>




![png](output_96_1.png)



```python
# seaborn의 scatterplot을 통해 '상권업종중분류명'의 경도와 위도를 표시합니다.

plt.figure(figsize=(9,8))
sns.scatterplot(data=df_seoul, x='경도',y='위도',hue='상권업종중분류명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd5a2fa148>




![png](output_97_1.png)



```python
# seaborn의 scatterplot을 통해  데이터(df)로 구별 경도와 위도를 표시합니다.

plt.figure(figsize=(9,8))
sns.scatterplot(data=df, x='경도',y='위도',hue='시도명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd5c7e2248>




![png](output_98_1.png)



```python
# seaborn의 scatterplot을 통해  데이터(df)로 구별 경도와 위도를 표시합니다.

plt.figure(figsize=(16,12))
sns.scatterplot(data=df, x='경도',y='위도',hue='상권업종중분류명')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1cd5c8b2588>




![png](output_99_1.png)


# 1.13 Folium으로 지도 활용하기


```python
import folium
folium.Map()

```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF84ZmQxYjM1OWM0OTg0ZTdlOTlmY2MyNjNiMDM4YmEzZSB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfOGZkMWIzNTljNDk4NGU3ZTk5ZmNjMjYzYjAzOGJhM2UiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwXzhmZDFiMzU5YzQ5ODRlN2U5OWZjYzI2M2IwMzhiYTNlID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwXzhmZDFiMzU5YzQ5ODRlN2U5OWZjYzI2M2IwMzhiYTNlIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFswLCAwXSwKICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3LAogICAgICAgICAgICAgICAgICAgIHpvb206IDEsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl81MWQyYTk1MmUxZWM0MTA0YmRkZmMxYzYyZjU4ODc4MCA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfOGZkMWIzNTljNDk4NGU3ZTk5ZmNjMjYzYjAzOGJhM2UpOwogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
random_map = folium.Map(location=[37.5236,127.660])
random_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF8zMGVmM2UxYzZkOTU0NzljODBhN2Y0OWI5NTZkMGI2MyB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfMzBlZjNlMWM2ZDk1NDc5YzgwYTdmNDliOTU2ZDBiNjMiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwXzMwZWYzZTFjNmQ5NTQ3OWM4MGE3ZjQ5Yjk1NmQwYjYzID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwXzMwZWYzZTFjNmQ5NTQ3OWM4MGE3ZjQ5Yjk1NmQwYjYzIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFszNy41MjM2LCAxMjcuNjZdLAogICAgICAgICAgICAgICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcsCiAgICAgICAgICAgICAgICAgICAgem9vbTogMTAsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl80ZmZhZDA2YmVjNTI0MzE0YTExZTlhZWJiOTc0MWZkNCA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMzBlZjNlMWM2ZDk1NDc5YzgwYTdmNDliOTU2ZDBiNjMpOwogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
# 지도의 중심을 지정하기 위해 위도와 경도의 평균 구하기.
df_seoul_hospital['위도'].mean()
df_seoul_hospital['경도'].mean()
```




    126.9963589356625




```python
folium.Map(location=[df_seoul_hospital['위도'].mean(),df_seoul_hospital['경도'].mean()])
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF8yY2RiMTNhNmMzNzY0YTk3OGMyMzFmNGY5NDU2OTIxMSB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfMmNkYjEzYTZjMzc2NGE5NzhjMjMxZjRmOTQ1NjkyMTEiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwXzJjZGIxM2E2YzM3NjRhOTc4YzIzMWY0Zjk0NTY5MjExID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwXzJjZGIxM2E2YzM3NjRhOTc4YzIzMWY0Zjk0NTY5MjExIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFszNy41Mjk3MjI3NjM1NTU5NCwgMTI2Ljk5NjM1ODkzNTY2MjVdLAogICAgICAgICAgICAgICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcsCiAgICAgICAgICAgICAgICAgICAgem9vbTogMTAsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl83ZDZmN2ZlNGUyZDM0MzYzYjAxNjhjOTMwY2FhOGNiMyA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMmNkYjEzYTZjMzc2NGE5NzhjMjMxZjRmOTQ1NjkyMTEpOwogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
#zoom_start : 확대
map = folium.Map(location=[df_seoul_hospital['위도'].mean(),df_seoul_hospital['경도'].mean()],zoom_start=12)
map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTAiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFszNy41Mjk3MjI3NjM1NTU5NCwgMTI2Ljk5NjM1ODkzNTY2MjVdLAogICAgICAgICAgICAgICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcsCiAgICAgICAgICAgICAgICAgICAgem9vbTogMTIsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl80YzdhZjY5ZGI0OTg0MjllODkyMjJiNWE1MjBmNjg1NiA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
# 임의의 병원 지도 위에 마커 표시하기
for n in df_seoul_hospital.index:
    name =df_seoul_hospital.loc[n,'상호명'] 
    address = df_seoul_hospital.loc[n,'도로명주소']
    popup = f"{name}-{address}"
    location = [df_seoul_hospital.loc[n,'위도'],df_seoul_hospital.loc[n,'경도']]
    folium.Marker(location = location, popup = popup).add_to(map)
map
    
    #     print(popup
    
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTAiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFszNy41Mjk3MjI3NjM1NTU5NCwgMTI2Ljk5NjM1ODkzNTY2MjVdLAogICAgICAgICAgICAgICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcsCiAgICAgICAgICAgICAgICAgICAgem9vbTogMTIsCiAgICAgICAgICAgICAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgICAgICAgICAgICAgcHJlZmVyQ2FudmFzOiBmYWxzZSwKICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgKTsKCiAgICAgICAgICAgIAoKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl80YzdhZjY5ZGI0OTg0MjllODkyMjJiNWE1MjBmNjg1NiA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgImh0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nIiwKICAgICAgICAgICAgICAgIHsiYXR0cmlidXRpb24iOiAiRGF0YSBieSBcdTAwMjZjb3B5OyBcdTAwM2NhIGhyZWY9XCJodHRwOi8vb3BlbnN0cmVldG1hcC5vcmdcIlx1MDAzZU9wZW5TdHJlZXRNYXBcdTAwM2MvYVx1MDAzZSwgdW5kZXIgXHUwMDNjYSBocmVmPVwiaHR0cDovL3d3dy5vcGVuc3RyZWV0bWFwLm9yZy9jb3B5cmlnaHRcIlx1MDAzZU9EYkxcdTAwM2MvYVx1MDAzZS4iLCAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsICJtYXhOYXRpdmVab29tIjogMTgsICJtYXhab29tIjogMTgsICJtaW5ab29tIjogMCwgIm5vV3JhcCI6IGZhbHNlLCAib3BhY2l0eSI6IDEsICJzdWJkb21haW5zIjogImFiYyIsICJ0bXMiOiBmYWxzZX0KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZDBiMTk0Y2YzNDIzNGQyOGE4Y2QyZjhiNjdiMzcwNTUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NTkwNDc4MDY2OTE5LCAxMjcuMDg4Mjc5MzU4ODMzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zMmU4MjM2ZTg0ZTg0OGYxODZiZmQ5NzBhMWM2OGRjMCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZDAzNGU5MjNiNTYxNDAwNWE1MWRmZGM5MzdmZjFiZjUgPSAkKGA8ZGl2IGlkPSJodG1sX2QwMzRlOTIzYjU2MTQwMDVhNTFkZmRjOTM3ZmYxYmY1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7rjIDsp4TsnZjro4zsnqzri6gt7ISc7Jq47Yq567OE7IucIOq0keynhOq1rCDquLTqs6DrnpHroZwgMTE5PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzMyZTgyMzZlODRlODQ4ZjE4NmJmZDk3MGExYzY4ZGMwLnNldENvbnRlbnQoaHRtbF9kMDM0ZTkyM2I1NjE0MDA1YTUxZGZkYzkzN2ZmMWJmNSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9kMGIxOTRjZjM0MjM0ZDI4YThjZDJmOGI2N2IzNzA1NS5iaW5kUG9wdXAocG9wdXBfMzJlODIzNmU4NGU4NDhmMTg2YmZkOTcwYTFjNjhkYzApCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfYmExMjRjZWYzNDM0NGYxNjkyMWEyMDEwYzhmOGI1ZjIgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MjkyMTMyODYzNjQzOTYsIDEyNi44NjI4MDUxMjg1NDQ5OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfOWU1YjdhZjhhZTM2NDRjZWIzNmZkYzI0ZTUyYjFhZmIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzJkZDVjOGE3M2NlMjQ1ZmNhZDc1ODhiOTU1MGRlYmNkID0gJChgPGRpdiBpZD0iaHRtbF8yZGQ1YzhhNzNjZTI0NWZjYWQ3NTg4Yjk1NTBkZWJjZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7ZmN7J2167OR7JuQ67OE6rSALeyEnOyauO2KueuzhOyLnCDslpHsspzqtawg6rWt7ZqM64yA66GcIDI1MDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF85ZTViN2FmOGFlMzY0NGNlYjM2ZmRjMjRlNTJiMWFmYi5zZXRDb250ZW50KGh0bWxfMmRkNWM4YTczY2UyNDVmY2FkNzU4OGI5NTUwZGViY2QpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYmExMjRjZWYzNDM0NGYxNjkyMWEyMDEwYzhmOGI1ZjIuYmluZFBvcHVwKHBvcHVwXzllNWI3YWY4YWUzNjQ0Y2ViMzZmZGMyNGU1MmIxYWZiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2UxNmQxZTQyNWU5ZDQ0OWZhZjcxMGQwMDIxNjE5OGJjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDk5NjMwMjY2MTExNiwgMTI3LjAzNTgyNDc4ODk0MV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYmQxZGIyMjllZDQ4NGM3YmIxYzc2ZDRmNGU3YWYzZjMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2U2ZGU4ZDBmZjg0MjQ2ZWRiMmM0YmUwMDA1MDYyODI3ID0gJChgPGRpdiBpZD0iaHRtbF9lNmRlOGQwZmY4NDI0NmVkYjJjNGJlMDAwNTA2MjgyNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U05VSC3shJzsmrjtirnrs4Tsi5wg6rCV64Ko6rWsIO2FjO2XpOuegOuhnDI26ri4IDEwPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2JkMWRiMjI5ZWQ0ODRjN2JiMWM3NmQ0ZjRlN2FmM2YzLnNldENvbnRlbnQoaHRtbF9lNmRlOGQwZmY4NDI0NmVkYjJjNGJlMDAwNTA2MjgyNyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9lMTZkMWU0MjVlOWQ0NDlmYWY3MTBkMDAyMTYxOThiYy5iaW5kUG9wdXAocG9wdXBfYmQxZGIyMjllZDQ4NGM3YmIxYzc2ZDRmNGU3YWYzZjMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOWMxZjIxZmUxMmNjNDIxOGEzYmI5ZjZjYjI0ZTIzODYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NTk0Njg4MDA2NTczLCAxMjcuMDQxMzI0NzExODg2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yNzE2YzliYzAxODY0YTdmOTZjYWE3ZDgzY2I3ODViMSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYzgwNWY2OTQ2Nzc1NDMxYmE2OGVmMGFlZDE4MDVkZGQgPSAkKGA8ZGl2IGlkPSJodG1sX2M4MDVmNjk0Njc3NTQzMWJhNjhlZjBhZWQxODA1ZGRkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7tlZzslpEt7ISc7Jq47Yq567OE7IucIOyEseuPmeq1rCDrp4jsobDroZwgMjItMjwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yNzE2YzliYzAxODY0YTdmOTZjYWE3ZDgzY2I3ODViMS5zZXRDb250ZW50KGh0bWxfYzgwNWY2OTQ2Nzc1NDMxYmE2OGVmMGFlZDE4MDVkZGQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOWMxZjIxZmUxMmNjNDIxOGEzYmI5ZjZjYjI0ZTIzODYuYmluZFBvcHVwKHBvcHVwXzI3MTZjOWJjMDE4NjRhN2Y5NmNhYTdkODNjYjc4NWIxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzg1ZWFmYTE4Y2FjODQ5ZDI4NmU5MTUxNzZmOGJlNzM2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTQyMjcwODA5ODMzMjA1LCAxMjcuMTI1MjgzNDExNjIxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9lYTBmYjU3ZjExMmU0YTZjYjQ3MDkwMWExY2JjNGMyYyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWQ5MWRhZTk0OTgyNDIwMWFmOGY3OGZlYTg4YjZlMDIgPSAkKGA8ZGl2IGlkPSJodG1sXzVkOTFkYWU5NDk4MjQyMDFhZjhmNzhmZWE4OGI2ZTAyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7rsLHsgrDsnZjro4zsnqzri6jsuZzqtazrs5Hsm5At7ISc7Jq47Yq567OE7IucIOqwleuPmeq1rCDsmKzrprztlL3roZwgNjg0PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2VhMGZiNTdmMTEyZTRhNmNiNDcwOTAxYTFjYmM0YzJjLnNldENvbnRlbnQoaHRtbF81ZDkxZGFlOTQ5ODI0MjAxYWY4Zjc4ZmVhODhiNmUwMik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl84NWVhZmExOGNhYzg0OWQyODZlOTE1MTc2ZjhiZTczNi5iaW5kUG9wdXAocG9wdXBfZWEwZmI1N2YxMTJlNGE2Y2I0NzA5MDFhMWNiYzRjMmMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNzQ0Mjg2YjJhMDNhNGRhM2JkZDU5NTE3NjljNzY0NjkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41Mjg0NjA4NzgyNTg3MDYsIDEyNy4xNDc5MTQyMjY1NzZdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2Y1Y2MxMmMxMzNkNDQwNjlhMzBmOTM3YzI2MWFiZDM0ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9hNmRjZTNlN2QxMTU0NWZhYjhjZjM2YWY0NTBjYWYzYSA9ICQoYDxkaXYgaWQ9Imh0bWxfYTZkY2UzZTdkMTE1NDVmYWI4Y2YzNmFmNDUwY2FmM2EiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEnOyauOuztO2biOuzkeybkC3shJzsmrjtirnrs4Tsi5wg6rCV64+Z6rWsIOynhO2ZqeuPhOuhnDYx6ri4IDUzPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2Y1Y2MxMmMxMzNkNDQwNjlhMzBmOTM3YzI2MWFiZDM0LnNldENvbnRlbnQoaHRtbF9hNmRjZTNlN2QxMTU0NWZhYjhjZjM2YWY0NTBjYWYzYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl83NDQyODZiMmEwM2E0ZGEzYmRkNTk1MTc2OWM3NjQ2OS5iaW5kUG9wdXAocG9wdXBfZjVjYzEyYzEzM2Q0NDA2OWEzMGY5MzdjMjYxYWJkMzQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMWMzYzQ3YmY2OGYzNDdmZmJhZDJjZmI3YjdhYzJhNTkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MDAwMTQxMTM1MjU1LCAxMjcuMDM2NDg3MTE4NzM2OTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzk0NDNkM2QyMjNhMzQ5ODk4YmRhYzhiNGI5YzhjMjk2ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iNDA5Y2MwOWEyM2E0ODI2Yjk2M2Q3YjNhYmRkN2E0ZSA9ICQoYDxkaXYgaWQ9Imh0bWxfYjQwOWNjMDlhMjNhNDgyNmI5NjNkN2IzYWJkZDdhNGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEnOyauOuMgO2Vmeq1kOuzkeybkC3shJzsmrjtirnrs4Tsi5wg6rCV64Ko6rWsIO2FjO2XpOuegOuhnCAxNTI8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfOTQ0M2QzZDIyM2EzNDk4OThiZGFjOGI0YjljOGMyOTYuc2V0Q29udGVudChodG1sX2I0MDljYzA5YTIzYTQ4MjZiOTYzZDdiM2FiZGQ3YTRlKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzFjM2M0N2JmNjhmMzQ3ZmZiYWQyY2ZiN2I3YWMyYTU5LmJpbmRQb3B1cChwb3B1cF85NDQzZDNkMjIzYTM0OTg5OGJkYWM4YjRiOWM4YzI5NikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9jZGNjYmEyOWQzNmU0OTliYjllYzI1ZTQ1ZGZlODdiNiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjU2MTcwODc4OTU3NzEwNCwgMTI2Ljk5OTYxOTg0ODE4OF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfOTlkMjI2OGVmNjVhNGYwZmEwMmMwNjljMzNlNjZlNjMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzVkMmM1NTBjMTkyZjQxNjA4OTQzM2ViYWI2MTcxMzBmID0gJChgPGRpdiBpZD0iaHRtbF81ZDJjNTUwYzE5MmY0MTYwODk0MzNlYmFiNjE3MTMwZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7KCc7J2867OR7JuQLeyEnOyauO2KueuzhOyLnCDspJHqtawg7ISc7JWg66GcMeq4uCAxNzwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF85OWQyMjY4ZWY2NWE0ZjBmYTAyYzA2OWMzM2U2NmU2My5zZXRDb250ZW50KGh0bWxfNWQyYzU1MGMxOTJmNDE2MDg5NDMzZWJhYjYxNzEzMGYpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfY2RjY2JhMjlkMzZlNDk5YmI5ZWMyNWU0NWRmZTg3YjYuYmluZFBvcHVwKHBvcHVwXzk5ZDIyNjhlZjY1YTRmMGZhMDJjMDY5YzMzZTY2ZTYzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzA5YzFmNTRhZDI2MjQ4NTFhMjFkMGNiYWQ2ODg0MmFjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDgxOTA3NTA0NzUxNTA0LCAxMjYuODgxNTY3OTk5OTc5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9lMjFhYTFkNTVlYmE0YzMyOGUzY2M3MjU0ZmQzMTMwYiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMjU4YzQ5YjZkNjRkNDg5MzlmMzY2MmZiODkxYTBlZTIgPSAkKGA8ZGl2IGlkPSJodG1sXzI1OGM0OWI2ZDY0ZDQ4OTM5ZjM2NjJmYjg5MWEwZWUyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7snbTrnpzrk5ztgbTrpqzri4kt7ISc7Jq47Yq567OE7IucIOq4iOyynOq1rCDqsIDsgrDrlJTsp4DthLgx66GcIDE4NjwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9lMjFhYTFkNTVlYmE0YzMyOGUzY2M3MjU0ZmQzMTMwYi5zZXRDb250ZW50KGh0bWxfMjU4YzQ5YjZkNjRkNDg5MzlmMzY2MmZiODkxYTBlZTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDljMWY1NGFkMjYyNDg1MWEyMWQwY2JhZDY4ODQyYWMuYmluZFBvcHVwKHBvcHVwX2UyMWFhMWQ1NWViYTRjMzI4ZTNjYzcyNTRmZDMxMzBiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzQwYzM3MTA1NDJhMDRkOWViNTMzYWYzZTBjMTg1MDI4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTU5ODg4NDU3OTA0NiwgMTI2Ljk2NDA2OTg4MjU1Nl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfN2U0ODhkOTk4YjA1NDlhZjhlNzJiNTJhYjFlNDhmY2MgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzg2Y2ZkYjkwYzJhZjRlMWM5ZTVhZWFkNzdkMjNiZGQyID0gJChgPGRpdiBpZD0iaHRtbF84NmNmZGI5MGMyYWY0ZTFjOWU1YWVhZDc3ZDIzYmRkMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7IKs656R64KY64iU7J2Y66OM7J6s64uoLeyEnOyauO2KueuzhOyLnCDshJzrjIDrrLjqtawg7ISc7IaM66y466GcIDIxPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzdlNDg4ZDk5OGIwNTQ5YWY4ZTcyYjUyYWIxZTQ4ZmNjLnNldENvbnRlbnQoaHRtbF84NmNmZGI5MGMyYWY0ZTFjOWU1YWVhZDc3ZDIzYmRkMik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl80MGMzNzEwNTQyYTA0ZDllYjUzM2FmM2UwYzE4NTAyOC5iaW5kUG9wdXAocG9wdXBfN2U0ODhkOTk4YjA1NDlhZjhlNzJiNTJhYjFlNDhmY2MpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZmY3NjlhY2U0MDU2NDE1NTlhODRiM2ZkYmNiNTQxN2EgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41ODg0ODQ5ODYwOTE2LCAxMjcuMDMxNzAxODA5Njk1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80ZmM5Y2U2NjBhMGQ0NDc2OTQxZTgyZTdiNmQzMWJjMiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmEwOTYyZDFiOTYyNDJiNzlhMDkyMzNmMDAyMmFjYTIgPSAkKGA8ZGl2IGlkPSJodG1sX2JhMDk2MmQxYjk2MjQyYjc5YTA5MjMzZjAwMjJhY2EyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7smrDsmrjspp3shLzthLAt7ISc7Jq47Yq567OE7IucIOyEseu2geq1rCDslYjslZTroZwgMTQ1PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzRmYzljZTY2MGEwZDQ0NzY5NDFlODJlN2I2ZDMxYmMyLnNldENvbnRlbnQoaHRtbF9iYTA5NjJkMWI5NjI0MmI3OWEwOTIzM2YwMDIyYWNhMik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mZjc2OWFjZTQwNTY0MTU1OWE4NGIzZmRiY2I1NDE3YS5iaW5kUG9wdXAocG9wdXBfNGZjOWNlNjYwYTBkNDQ3Njk0MWU4MmU3YjZkMzFiYzIpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNWEwODgwZTNhYTU3NDIxZDg3MmJjOTgyMTY5M2JlYTQgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MjM0MjU5MzM2NDI0LCAxMjYuOTEwMzA1Njc2MDc1OTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2JjNmMzM2M4YTdlZTQ4OTk5NDAzMDQxMWVjMDhlMzIzID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF82ZWMxYTZjOTg1ZjQ0NTUwYmM2ZmIxOWM3YWZmYTBkMyA9ICQoYDxkaXYgaWQ9Imh0bWxfNmVjMWE2Yzk4NWY0NDU1MGJjNmZiMTljN2FmZmEwZDMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEseyLrOydmOujjOyerOuLqC3shJzsmrjtirnrs4Tsi5wg7JiB65Ox7Y+s6rWsIOuyhOuTnOuCmOujqOuhnCA1NTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9iYzZjMzNjOGE3ZWU0ODk5OTQwMzA0MTFlYzA4ZTMyMy5zZXRDb250ZW50KGh0bWxfNmVjMWE2Yzk4NWY0NDU1MGJjNmZiMTljN2FmZmEwZDMpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNWEwODgwZTNhYTU3NDIxZDg3MmJjOTgyMTY5M2JlYTQuYmluZFBvcHVwKHBvcHVwX2JjNmMzM2M4YTdlZTQ4OTk5NDAzMDQxMWVjMDhlMzIzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzFkOWY2MWExODdlOTQwOTliNDcyOWUzZDNjMjU3MDYxID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDk2NzE5MzUzOTU5MSwgMTI2Ljg1Njk1Mzc2Njk5NDk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80YzUyNTEwZDNlNDU0ZTk3YWRjNjVmMzRhMmYxNTQzMCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNzZlMzg5OWE0NTE1NGRlYWI5ZDcxNTQ5YjIwMTIxMTMgPSAkKGA8ZGl2IGlkPSJodG1sXzc2ZTM4OTlhNDUxNTRkZWFiOWQ3MTU0OWIyMDEyMTEzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7ri6TrgpjsnZjro4zsnqzri6gt7ISc7Jq47Yq567OE7IucIOq1rOuhnOq1rCDqsJzrtInroZwgMTI2PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzRjNTI1MTBkM2U0NTRlOTdhZGM2NWYzNGEyZjE1NDMwLnNldENvbnRlbnQoaHRtbF83NmUzODk5YTQ1MTU0ZGVhYjlkNzE1NDliMjAxMjExMyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8xZDlmNjFhMTg3ZTk0MDk5YjQ3MjllM2QzYzI1NzA2MS5iaW5kUG9wdXAocG9wdXBfNGM1MjUxMGQzZTQ1NGU5N2FkYzY1ZjM0YTJmMTU0MzApCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjJkOGFhMzE0MTcxNDkzNjkyN2QwMjFlNDE1Yzg3MjggPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MjUyNDc2NDg1NzczLCAxMjcuMTA5MzE1Mjc1NzE1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yOWUyMmI5Nzg2ZmM0Zjg2YWU5NDQ3YTQ2OTM2ZWFmYiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNjYwMzdjYjdjZjExNDhiNGEyMmYxNzQ4MjBhNmIzMTQgPSAkKGA8ZGl2IGlkPSJodG1sXzY2MDM3Y2I3Y2YxMTQ4YjRhMjJmMTc0ODIwYTZiMzE0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7shJzsmrjslYTsgrDrs5Hsm5Dsi6DqtIAt7ISc7Jq47Yq567OE7IucIOyGoe2MjOq1rCDsmKzrprztlL3roZw0M+q4uCA4ODwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yOWUyMmI5Nzg2ZmM0Zjg2YWU5NDQ3YTQ2OTM2ZWFmYi5zZXRDb250ZW50KGh0bWxfNjYwMzdjYjdjZjExNDhiNGEyMmYxNzQ4MjBhNmIzMTQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZjJkOGFhMzE0MTcxNDkzNjkyN2QwMjFlNDE1Yzg3MjguYmluZFBvcHVwKHBvcHVwXzI5ZTIyYjk3ODZmYzRmODZhZTk0NDdhNDY5MzZlYWZiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzA1NWY1NzljNWM2ODQ4ZDE5OTBiOWJmYWI3NDExOWFlID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDkxMjA4NTMyNTQ3NDk2LCAxMjYuODg0NjYzNDQzNV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYzkzMDdjYTM1NDQ0NDM0Yzk4YzE0MTUwZDU2YWE3NTAgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzRhYzBmMzVlNGE5ZTRkMDdhZDA0MmIzYzlmOTM1ZjAyID0gJChgPGRpdiBpZD0iaHRtbF80YWMwZjM1ZTRhOWU0ZDA3YWQwNDJiM2M5ZjkzNWYwMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+6rOg66Ck64yA7ZWZ6rWQ6rWs66Gc67OR7JuQLeyEnOyauO2KueuzhOyLnCDqtazroZzqtawg6rWs66Gc64+Z66GcIDE0ODwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jOTMwN2NhMzU0NDQ0MzRjOThjMTQxNTBkNTZhYTc1MC5zZXRDb250ZW50KGh0bWxfNGFjMGYzNWU0YTllNGQwN2FkMDQyYjNjOWY5MzVmMDIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDU1ZjU3OWM1YzY4NDhkMTk5MGI5YmZhYjc0MTE5YWUuYmluZFBvcHVwKHBvcHVwX2M5MzA3Y2EzNTQ0NDQzNGM5OGMxNDE1MGQ1NmFhNzUwKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2FjMGIwMDVlMWFhZTQxODc5ZWM5NjQyYzlmZjdkYjMyID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTIzNDI1OTMzNjQyNCwgMTI2LjkxMDMwNTY3NjA3NTk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF85ODk0YzZkZmZmMTQ0YTMzOTAzNzVkMjY0NWQ5ZDQ2MSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmNmYTg3NDk0MjI5NDE0NWIxZTRjNzYwY2FhODYyOGMgPSAkKGA8ZGl2IGlkPSJodG1sX2JjZmE4NzQ5NDIyOTQxNDViMWU0Yzc2MGNhYTg2MjhjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7tlZnqtZDrspXsnbjsnbzshqHtlZnsm5At7ISc7Jq47Yq567OE7IucIOyYgeuTse2PrOq1rCDrsoTrk5zrgpjro6jroZwgNTU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfOTg5NGM2ZGZmZjE0NGEzMzkwMzc1ZDI2NDVkOWQ0NjEuc2V0Q29udGVudChodG1sX2JjZmE4NzQ5NDIyOTQxNDViMWU0Yzc2MGNhYTg2MjhjKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2FjMGIwMDVlMWFhZTQxODc5ZWM5NjQyYzlmZjdkYjMyLmJpbmRQb3B1cChwb3B1cF85ODk0YzZkZmZmMTQ0YTMzOTAzNzVkMjY0NWQ5ZDQ2MSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl81YjJmOTlkZGExNmE0Nzg2YmQyN2Q4YzI3ZTI5Mzg1YSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjQ1NDQyMDgxNDgxMDksIDEyNi45MDEyMzcwNDAyNzcwMV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNWRlYjdhMjM0ZGQ1NGY0YThhZTIzNDExZGJkMzk2OTcgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Q1YzllMTg3ZTRhYzRlNDk4ZmRiNmIxNjkyYzIyYmQwID0gJChgPGRpdiBpZD0iaHRtbF9kNWM5ZTE4N2U0YWM0ZTQ5OGZkYjZiMTY5MmMyMmJkMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7Z2s66qF7Iqk7Y+s7Lig7J2Y7ZWZ7IS87YSw7J246rO17Iug7J6l7IukLeyEnOyauO2KueuzhOyLnCDquIjsspzqtawg7Iuc7Z2l64yA66GcIDIzMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81ZGViN2EyMzRkZDU0ZjRhOGFlMjM0MTFkYmQzOTY5Ny5zZXRDb250ZW50KGh0bWxfZDVjOWUxODdlNGFjNGU0OThmZGI2YjE2OTJjMjJiZDApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNWIyZjk5ZGRhMTZhNDc4NmJkMjdkOGMyN2UyOTM4NWEuYmluZFBvcHVwKHBvcHVwXzVkZWI3YTIzNGRkNTRmNGE4YWUyMzQxMWRiZDM5Njk3KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2IxZjg5NTY3ZWMxOTQ0NDBiMmNhMWZjZjVkOTRjODgwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDkyNzk1MTM1MTk2MSwgMTI3LjA0NjI5Mzg2NDcyNF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfM2I4OGM5ZGQwYjFmNGY2MGJkZDQwYTdlNzkxMDkwNzAgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzk2NDVkNDQyODcyYjRkNjQ5NDkxODhhZmNkZDZiNWEyID0gJChgPGRpdiBpZD0iaHRtbF85NjQ1ZDQ0Mjg3MmI0ZDY0OTQ5MTg4YWZjZGQ2YjVhMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7Jew7IS464yA7ZWZ6rWQ7J2Y6rO864yA7ZWZ6rCV64Ko7IS467iM656A7IqkLeyEnOyauO2KueuzhOyLnCDqsJXrgqjqtawg7Ja47KO866GcIDIxMTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zYjg4YzlkZDBiMWY0ZjYwYmRkNDBhN2U3OTEwOTA3MC5zZXRDb250ZW50KGh0bWxfOTY0NWQ0NDI4NzJiNGQ2NDk0OTE4OGFmY2RkNmI1YTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYjFmODk1NjdlYzE5NDQ0MGIyY2ExZmNmNWQ5NGM4ODAuYmluZFBvcHVwKHBvcHVwXzNiODhjOWRkMGIxZjRmNjBiZGQ0MGE3ZTc5MTA5MDcwKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2E5ZTMyOGQ0NDUwNjQ0MmFiYWEzOGZlMDY1OTViNmE2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTY1MzEzODgxOTczNiwgMTI3LjA4NjQzNzE1NDYzOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYzk0MzhkYzNlZTU5NDQzMjkyZTNhYzhiN2VmMjdkODIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzk5YTU0MmMyZmIxMzRiNDg4Y2E1MjQyMDBjYzE1NDU0ID0gJChgPGRpdiBpZD0iaHRtbF85OWE1NDJjMmZiMTM0YjQ4OGNhNTI0MjAwY2MxNTQ1NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+6rWt66a97KCV7Iug67OR7JuQLeyEnOyauO2KueuzhOyLnCDqtJHsp4Tqtawg64ql64+Z66GcIDM5ODwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jOTQzOGRjM2VlNTk0NDMyOTJlM2FjOGI3ZWYyN2Q4Mi5zZXRDb250ZW50KGh0bWxfOTlhNTQyYzJmYjEzNGI0ODhjYTUyNDIwMGNjMTU0NTQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYTllMzI4ZDQ0NTA2NDQyYWJhYTM4ZmUwNjU5NWI2YTYuYmluZFBvcHVwKHBvcHVwX2M5NDM4ZGMzZWU1OTQ0MzI5MmUzYWM4YjdlZjI3ZDgyKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2I5Y2Q4NTU2ODMyNDRmZjk5YjMyM2YyMDA3ZWVjMmY3ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTA0MTU4NTgxNTE1MSwgMTI3LjA1MjAzMDY0Njg1NDk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yNmNkOGVkYmNiOTI0YzdjOWM0NTk2ODAwOGUyNzRmNCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODY0MTNjOWFkN2Q1NDYzOThmMjgwMTczNDAyZDUzNDggPSAkKGA8ZGl2IGlkPSJodG1sXzg2NDEzYzlhZDdkNTQ2Mzk4ZjI4MDE3MzQwMmQ1MzQ4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7svZTslYTtgbTrpqzri4kt7ISc7Jq47Yq567OE7IucIOqwleuCqOq1rCDshKDrponroZw4Nuq4uCAzMTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yNmNkOGVkYmNiOTI0YzdjOWM0NTk2ODAwOGUyNzRmNC5zZXRDb250ZW50KGh0bWxfODY0MTNjOWFkN2Q1NDYzOThmMjgwMTczNDAyZDUzNDgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYjljZDg1NTY4MzI0NGZmOTliMzIzZjIwMDdlZWMyZjcuYmluZFBvcHVwKHBvcHVwXzI2Y2Q4ZWRiY2I5MjRjN2M5YzQ1OTY4MDA4ZTI3NGY0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzA2MDg1NDI0NWZjOTRlYWQ4YjBmY2M4ZjFmMzFiMDY1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNjQ2Njc5MTcyODUwNSwgMTI3LjAyOTA0MzA3ODIxMzAxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9mNDRlNDZjNjcxYTY0OTZjYTZhZTA4NWY0MzlmMGQyMSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWExNmZlZDFmNDZlNDEyYTgxZTU5NDQ0NzM2YjY0OWUgPSAkKGA8ZGl2IGlkPSJodG1sXzVhMTZmZWQxZjQ2ZTQxMmE4MWU1OTQ0NDczNmI2NDllIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7tlZzqta3soITroKXqs7XsgqzrtoDsho3tlZzsnbzrs5Hsm5At7ISc7Jq47Yq567OE7IucIOuPhOu0ieq1rCDsmrDsnbTsspzroZwgMzA4PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2Y0NGU0NmM2NzFhNjQ5NmNhNmFlMDg1ZjQzOWYwZDIxLnNldENvbnRlbnQoaHRtbF81YTE2ZmVkMWY0NmU0MTJhODFlNTk0NDQ3MzZiNjQ5ZSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8wNjA4NTQyNDVmYzk0ZWFkOGIwZmNjOGYxZjMxYjA2NS5iaW5kUG9wdXAocG9wdXBfZjQ0ZTQ2YzY3MWE2NDk2Y2E2YWUwODVmNDM5ZjBkMjEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMzNiMzRmOWFmYTIyNDcyNTg4ODVjMTg3MjhiM2NhNjQgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NTIzNTEzNTAzMzcsIDEyNi45MzQyMTcwMjk3MTddLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzA4ZjQzNzNjOTU2YTQ3OWU4MDczNzVkN2IwY2ViNTI3ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9jNWY0ZmM2YTkxNTk0ZGRhYWU0NzA3NzQxMGYwOTNkMSA9ICQoYDxkaXYgaWQ9Imh0bWxfYzVmNGZjNmE5MTU5NGRkYWFlNDcwNzc0MTBmMDkzZDEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyLoOy0jOyXsOyEuOuzkeybkC3shJzsmrjtirnrs4Tsi5wg66eI7Y+s6rWsIOyEnOqwleuhnDE06ri4IDE0PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzA4ZjQzNzNjOTU2YTQ3OWU4MDczNzVkN2IwY2ViNTI3LnNldENvbnRlbnQoaHRtbF9jNWY0ZmM2YTkxNTk0ZGRhYWU0NzA3NzQxMGYwOTNkMSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8zM2IzNGY5YWZhMjI0NzI1ODg4NWMxODcyOGIzY2E2NC5iaW5kUG9wdXAocG9wdXBfMDhmNDM3M2M5NTZhNDc5ZTgwNzM3NWQ3YjBjZWI1MjcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfY2ZmZWM5MGU5NDE2NDI2YTg5ZjljMzE5MWRiYmM5ZmIgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy40OTI3OTUxMzUxOTYxLCAxMjcuMDQ2MjkzODY0NzI0XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9kNjVkNDFhNzY3MDM0ZWMyOGQ2YjA5ZjU3ZWFhNjc0MCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDQ0N2IxMzVkMzkxNGMyM2IyMDA4NzM1YzA0NWRmMDYgPSAkKGA8ZGl2IGlkPSJodG1sXzQ0NDdiMTM1ZDM5MTRjMjNiMjAwODczNWMwNDVkZjA2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7smIHrj5nshLjruIzrnoDsiqTrs5Hsm5At7ISc7Jq47Yq567OE7IucIOqwleuCqOq1rCDslrjso7zroZwgMjExPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2Q2NWQ0MWE3NjcwMzRlYzI4ZDZiMDlmNTdlYWE2NzQwLnNldENvbnRlbnQoaHRtbF80NDQ3YjEzNWQzOTE0YzIzYjIwMDg3MzVjMDQ1ZGYwNik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9jZmZlYzkwZTk0MTY0MjZhODlmOWMzMTkxZGJiYzlmYi5iaW5kUG9wdXAocG9wdXBfZDY1ZDQxYTc2NzAzNGVjMjhkNmIwOWY1N2VhYTY3NDApCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOTMzMGNmNzAyZDkyNDgzOGJmZjEwMDljNzNiMWQ5NjAgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NjIzNzYzNjQ5NzA0LCAxMjYuOTc1NTQ2ODAzODU3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9iOWVlZjQ3MzM3N2U0NDkxYmZjZmIzMmY3YTU1YzVkNCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMDhhZDk1ZWQ0YTVjNDIyMTkyNjgzYzY3YThjZTllMzYgPSAkKGA8ZGl2IGlkPSJodG1sXzA4YWQ5NWVkNGE1YzQyMjE5MjY4M2M2N2E4Y2U5ZTM2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7sgrzshLHsnZjro4zsnqzri6jqsJXrtoHsgrzshLHtg5wt7ISc7Jq47Yq567OE7IucIOykkeq1rCDshLjsooXrjIDroZwgNjc8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYjllZWY0NzMzNzdlNDQ5MWJmY2ZiMzJmN2E1NWM1ZDQuc2V0Q29udGVudChodG1sXzA4YWQ5NWVkNGE1YzQyMjE5MjY4M2M2N2E4Y2U5ZTM2KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzkzMzBjZjcwMmQ5MjQ4MzhiZmYxMDA5YzczYjFkOTYwLmJpbmRQb3B1cChwb3B1cF9iOWVlZjQ3MzM3N2U0NDkxYmZjZmIzMmY3YTU1YzVkNCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9kOGQzN2E3ZjMyZDM0OTMzYWMwNzJmYTM4ZWEzNDhlMyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjQ5Mjk5OTkxOTU3NjUwNSwgMTI2LjkyNDMxNzk5MzE4Mzk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF81OGYxNGRhMDljYmQ0YmMyYmUxMGRjMDMxYTcwMzIzNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZmFkMjBkZTUzYzYwNGNmZmI3ZmUxMjk2YWJkYjY4ZmUgPSAkKGA8ZGl2IGlkPSJodG1sX2ZhZDIwZGU1M2M2MDRjZmZiN2ZlMTI5NmFiZGI2OGZlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7shJzsmrjsi5zrpr3rs7Trnbzrp6Trs5Hsm5At7ISc7Jq47Yq567OE7IucIOuPmeyekeq1rCDrs7Trnbzrp6TroZw16ri4IDIwPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzU4ZjE0ZGEwOWNiZDRiYzJiZTEwZGMwMzFhNzAzMjM2LnNldENvbnRlbnQoaHRtbF9mYWQyMGRlNTNjNjA0Y2ZmYjdmZTEyOTZhYmRiNjhmZSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9kOGQzN2E3ZjMyZDM0OTMzYWMwNzJmYTM4ZWEzNDhlMy5iaW5kUG9wdXAocG9wdXBfNThmMTRkYTA5Y2JkNGJjMmJlMTBkYzAzMWE3MDMyMzYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMmRhZjBjZWM1YzM5NDM4Njg0NjAxZTE5MmY3MzVhOTggPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41ODA0NDc4MjI0NTQ3MSwgMTI2Ljk5NzE4NDA3Njk1OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMzEzNWYwNTZmODU2NGE3ODhhNWZiNDIwMDYzODM2NGIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2E1M2RhZTZhZWZlZTRiMDI5NDQxOTY5NmI2ODk1MDM4ID0gJChgPGRpdiBpZD0iaHRtbF9hNTNkYWU2YWVmZWU0YjAyOTQ0MTk2OTZiNjg5NTAzOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7ISc7Jq464yA7ZWZ6rWQ67OR7JuQ67mE7IOB6rOE7ZqN7Jm4656YLeyEnOyauO2KueuzhOyLnCDsooXroZzqtawg64yA7ZWZ66GcIDEwMTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zMTM1ZjA1NmY4NTY0YTc4OGE1ZmI0MjAwNjM4MzY0Yi5zZXRDb250ZW50KGh0bWxfYTUzZGFlNmFlZmVlNGIwMjk0NDE5Njk2YjY4OTUwMzgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMmRhZjBjZWM1YzM5NDM4Njg0NjAxZTE5MmY3MzVhOTguYmluZFBvcHVwKHBvcHVwXzMxMzVmMDU2Zjg1NjRhNzg4YTVmYjQyMDA2MzgzNjRiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2IyNjI1ZDliYzkwOTRhOGViMzUwMzEzMmUyNmY1ZDRiID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTAyMzgyMjM2OTMxMSwgMTI3LjAwNTg0MDcxMTU1Mzk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jNWJhOTA3NTA4MGM0MTU4YWQ5MDE3YzhlMWI5MTU5ZCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfN2QyMTEyMmJmOGVjNGU5ZDk4NGMzOWQ5ZjQzNGI5YzQgPSAkKGA8ZGl2IGlkPSJodG1sXzdkMjExMjJiZjhlYzRlOWQ5ODRjMzlkOWY0MzRiOWM0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7tj4ntmZTrk5zrprzshJzsmrjshLHrqqjrs5Hsm5DsnZjro4wt7ISc7Jq47Yq567OE7IucIOyEnOy0iOq1rCDrsJjtj6zrjIDroZwgMjIyPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2M1YmE5MDc1MDgwYzQxNThhZDkwMTdjOGUxYjkxNTlkLnNldENvbnRlbnQoaHRtbF83ZDIxMTIyYmY4ZWM0ZTlkOTg0YzM5ZDlmNDM0YjljNCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9iMjYyNWQ5YmM5MDk0YThlYjM1MDMxMzJlMjZmNWQ0Yi5iaW5kUG9wdXAocG9wdXBfYzViYTkwNzUwODBjNDE1OGFkOTAxN2M4ZTFiOTE1OWQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZWNmYTNkYmU0ODQ3NGM0NTliNzQwN2RhZDliZDJiNGIgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41Mjg3OTc4MDk0MTI3LCAxMjYuODYzNjM3MzQxNDM5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80MzZkNDdlOTU2MWE0ZjU0YWE2YWFhNTc0OTRmMTlkNyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZjNmMTU3ODc1ZGM1NGFkZDgxZWVjMmJmN2Q0NGVlOTggPSAkKGA8ZGl2IGlkPSJodG1sX2YzZjE1Nzg3NWRjNTRhZGQ4MWVlYzJiZjdkNDRlZTk4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7tmY3snbXrs5Hsm5At7ISc7Jq47Yq567OE7IucIOyWkeyynOq1rCDrqqnrj5nroZwgMjI3PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQzNmQ0N2U5NTYxYTRmNTRhYTZhYWE1NzQ5NGYxOWQ3LnNldENvbnRlbnQoaHRtbF9mM2YxNTc4NzVkYzU0YWRkODFlZWMyYmY3ZDQ0ZWU5OCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9lY2ZhM2RiZTQ4NDc0YzQ1OWI3NDA3ZGFkOWJkMmI0Yi5iaW5kUG9wdXAocG9wdXBfNDM2ZDQ3ZTk1NjFhNGY1NGFhNmFhYTU3NDk0ZjE5ZDcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNDY1OTI4NjliODRhNDBjMjllNGQwMTg3MzZmMTY3ZDkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NTk4ODg0NTc5MDQ2LCAxMjYuOTY0MDY5ODgyNTU2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80MjdiZGEzY2M5N2E0N2IxOTBkMWZiMjU2MjY4YzBmNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfOWQ4OWJjNzBlODU3NGFkOGFmNTBmZmNmODI2Mjc5N2QgPSAkKGA8ZGl2IGlkPSJodG1sXzlkODliYzcwZTg1NzRhZDhhZjUwZmZjZjgyNjI3OTdkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7sgqzrnpHrgpjriJTsnZjro4zsnqzri6jshJwt7ISc7Jq47Yq567OE7IucIOyEnOuMgOusuOq1rCDshJzshozrrLjroZwgMjE8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNDI3YmRhM2NjOTdhNDdiMTkwZDFmYjI1NjI2OGMwZjYuc2V0Q29udGVudChodG1sXzlkODliYzcwZTg1NzRhZDhhZjUwZmZjZjgyNjI3OTdkKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzQ2NTkyODY5Yjg0YTQwYzI5ZTRkMDE4NzM2ZjE2N2Q5LmJpbmRQb3B1cChwb3B1cF80MjdiZGEzY2M5N2E0N2IxOTBkMWZiMjU2MjY4YzBmNikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wNjY4OTZkNzBhZjk0YzZiYWYzMGEwMTUzYWU4YjEyMyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjU4NDg2NzgyNDcxMzcwNCwgMTI3LjAzMTIxNjUxMjM3N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNmU3YzA5MmQzMjViNGNjN2I2NWNjNTEyM2M3Mzc2MTEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Q2ZDIxMTc4NGU3NDRmNDBhNWQ0MDBiMTk4OTNkMzM1ID0gJChgPGRpdiBpZD0iaHRtbF9kNmQyMTE3ODRlNzQ0ZjQwYTVkNDAwYjE5ODkzZDMzNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7Jqw7Iug7Zal67OR7JuQLeyEnOyauO2KueuzhOyLnCDshLHrtoHqtawg7JWI7JWU66GcIDk5PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzZlN2MwOTJkMzI1YjRjYzdiNjVjYzUxMjNjNzM3NjExLnNldENvbnRlbnQoaHRtbF9kNmQyMTE3ODRlNzQ0ZjQwYTVkNDAwYjE5ODkzZDMzNSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8wNjY4OTZkNzBhZjk0YzZiYWYzMGEwMTUzYWU4YjEyMy5iaW5kUG9wdXAocG9wdXBfNmU3YzA5MmQzMjViNGNjN2I2NWNjNTEyM2M3Mzc2MTEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfODczZjRmN2FiYWQxNDg5NWFiYzY4Y2QyNjE5YWE1M2EgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy42MDA4MjIzNjkwMzUzLCAxMjcuMTA4NTk2MjczMTcxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zNmI4MzhjYTJkOWU0NTgwOTA2YWJmOTFkNzJmOGI3OSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMmUxMzVjNzkzZTc1NDk5OWJiYmVhZmUxZWJhNWU3ODggPSAkKGA8ZGl2IGlkPSJodG1sXzJlMTM1Yzc5M2U3NTQ5OTliYmJlYWZlMWViYTVlNzg4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7rj5nrtoDsoJzsnbzrs5Hsm5At7ISc7Jq47Yq567OE7IucIOykkeuekeq1rCDslpHsm5Dsl63roZwgMjwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zNmI4MzhjYTJkOWU0NTgwOTA2YWJmOTFkNzJmOGI3OS5zZXRDb250ZW50KGh0bWxfMmUxMzVjNzkzZTc1NDk5OWJiYmVhZmUxZWJhNWU3ODgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfODczZjRmN2FiYWQxNDg5NWFiYzY4Y2QyNjE5YWE1M2EuYmluZFBvcHVwKHBvcHVwXzM2YjgzOGNhMmQ5ZTQ1ODA5MDZhYmY5MWQ3MmY4Yjc5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2Y2NzNhYTliYmVhNDQyYjNiY2YwMWRjNWI3MGIyN2QzID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTE3Njc0MzM3MzAwMjEsIDEyNi45ODI3MDgwMTMzNzFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2VkZjUwN2FlYzliZTQxMjk4NDZhNDgxZWFkNDEzOTUxID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF83OTdiMTJmOTBiMTU0YjFmODhkMWUzNzZiMmI2N2Q1OSA9ICQoYDxkaXYgaWQ9Imh0bWxfNzk3YjEyZjkwYjE1NGIxZjg4ZDFlMzc2YjJiNjdkNTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyVhOyCsOyerOuLqOq4iOqwleuzkeybkC3shJzsmrjtirnrs4Tsi5wg7Jqp7IKw6rWsIOydtOy0jOuhnCAzMTg8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZWRmNTA3YWVjOWJlNDEyOTg0NmE0ODFlYWQ0MTM5NTEuc2V0Q29udGVudChodG1sXzc5N2IxMmY5MGIxNTRiMWY4OGQxZTM3NmIyYjY3ZDU5KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Y2NzNhYTliYmVhNDQyYjNiY2YwMWRjNWI3MGIyN2QzLmJpbmRQb3B1cChwb3B1cF9lZGY1MDdhZWM5YmU0MTI5ODQ2YTQ4MWVhZDQxMzk1MSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wYjliZTRjMmY3MTA0NGE0OWQ3YzBlZDE5NWU0MDc0ZCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUxOTY2ODc2MDI4OTUwNSwgMTI2LjkwMTU3MDU2MTE3M10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYTkwZmM1ZDYzNzhjNDJjYmI1YmM1YmViMmY0NmExYTkgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzFlOWYwMmI5M2IwMTQxOWJiMTNlNDVjZTRmZjViYWEyID0gJChgPGRpdiBpZD0iaHRtbF8xZTlmMDJiOTNiMDE0MTliYjEzZTQ1Y2U0ZmY1YmFhMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+66qF6rOh7JWI7Jew6rWs7IaMLeyEnOyauO2KueuzhOyLnCDsmIHrk7Htj6zqtawg7JiB7Iug66GcIDEzNjwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9hOTBmYzVkNjM3OGM0MmNiYjViYzViZWIyZjQ2YTFhOS5zZXRDb250ZW50KGh0bWxfMWU5ZjAyYjkzYjAxNDE5YmIxM2U0NWNlNGZmNWJhYTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMGI5YmU0YzJmNzEwNDRhNDlkN2MwZWQxOTVlNDA3NGQuYmluZFBvcHVwKHBvcHVwX2E5MGZjNWQ2Mzc4YzQyY2JiNWJjNWJlYjJmNDZhMWE5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzM3ZjE3OTg5YWZkNTQ1ZDQ5MGMzZWJhM2NkNGRiZGMwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTI1MjQ3NjQ4NTc3MywgMTI3LjEwOTMxNTI3NTcxNV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZTgxMzU0MGRmYjI3NGY2N2FhOWZiNDAwMDEzMjIxOTAgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2ZiODlmNGM1YTUwZDRiMTZiNjlmYmM0Mjk4MzQ1MzBmID0gJChgPGRpdiBpZD0iaHRtbF9mYjg5ZjRjNWE1MGQ0YjE2YjY5ZmJjNDI5ODM0NTMwZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7JWE7IKw7J6s64uo7ISc7Jq47KSR7JWZ67OR7JuQLeyEnOyauO2KueuzhOyLnCDshqHtjIzqtawg7Jis66a87ZS966GcNDPquLggODg8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZTgxMzU0MGRmYjI3NGY2N2FhOWZiNDAwMDEzMjIxOTAuc2V0Q29udGVudChodG1sX2ZiODlmNGM1YTUwZDRiMTZiNjlmYmM0Mjk4MzQ1MzBmKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzM3ZjE3OTg5YWZkNTQ1ZDQ5MGMzZWJhM2NkNGRiZGMwLmJpbmRQb3B1cChwb3B1cF9lODEzNTQwZGZiMjc0ZjY3YWE5ZmI0MDAwMTMyMjE5MCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wNDU3M2E1YzViYmY0MmY5OTgzNzM4ZTI0YjQwYmJjOCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUzNzI1MDg0NTA1MzYsIDEyNi44Mjc4MjE0Nzk1MjddLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzg1MGJjOTA4OTRjMzQ5ZTc4N2Y0ZjY0MTE0ODQ2OTMzID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9lYWI3ZGE1NGJiMzU0MjQzOWQyZmM3ZTUxMDcwYWVlMCA9ICQoYDxkaXYgaWQ9Imh0bWxfZWFiN2RhNTRiYjM1NDI0MzlkMmZjN2U1MTA3MGFlZTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuuplOuUlO2ekO2KueyImOyXrOqwnS3shJzsmrjtirnrs4Tsi5wg7JaR7LKc6rWsIOuCqOu2gOyInO2ZmOuhnCAzMzE8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODUwYmM5MDg5NGMzNDllNzg3ZjRmNjQxMTQ4NDY5MzMuc2V0Q29udGVudChodG1sX2VhYjdkYTU0YmIzNTQyNDM5ZDJmYzdlNTEwNzBhZWUwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzA0NTczYTVjNWJiZjQyZjk5ODM3MzhlMjRiNDBiYmM4LmJpbmRQb3B1cChwb3B1cF84NTBiYzkwODk0YzM0OWU3ODdmNGY2NDExNDg0NjkzMykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl82YjY3MjQ5YzVmOTI0YjkwOGFiYmRiZWI5NGUwYWRiMSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjQ5MDMzNDM0MDg2OTQ5NSwgMTI3LjA4OTU3ODgzMTYzMjAxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80YWMzYzU1ZjNkNDE0MWRlYjU0NDY2MWRlOTU5MDg1MCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYzczZTNiY2RmNTBlNDY0NDk3OTY1MjM3MmQyNjQ1OWYgPSAkKGA8ZGl2IGlkPSJodG1sX2M3M2UzYmNkZjUwZTQ2NDQ5Nzk2NTIzNzJkMjY0NTlmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7sgrzshLHsg53rqoXqs7XsnbXsnqzri6jsgrzshLHshJwt7ISc7Jq47Yq567OE7IucIOqwleuCqOq1rCDsnbzsm5DroZwgODE8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNGFjM2M1NWYzZDQxNDFkZWI1NDQ2NjFkZTk1OTA4NTAuc2V0Q29udGVudChodG1sX2M3M2UzYmNkZjUwZTQ2NDQ5Nzk2NTIzNzJkMjY0NTlmKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzZiNjcyNDljNWY5MjRiOTA4YWJiZGJlYjk0ZTBhZGIxLmJpbmRQb3B1cChwb3B1cF80YWMzYzU1ZjNkNDE0MWRlYjU0NDY2MWRlOTU5MDg1MCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9hOGRjODNiNGUwZDU0YTk1YTgyZGY4MjY0ZTc5ZGY3NCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUwNjg0MDQ5MzU5NTQsIDEyNy4wMzQ5NDAzMTgyMDldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2EyOWFlYThiNjU4NDRmYzY4NDZlY2QyY2ZlZmU2NDFhID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80ZTdlNGFiYzVlMDU0MzEzOTkxMDczMTBiNmY3MDgzOSA9ICQoYDxkaXYgaWQ9Imh0bWxfNGU3ZTRhYmM1ZTA1NDMxMzk5MTA3MzEwYjZmNzA4MzkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEseq0keydmOujjOyerOuLqOywqOuzkeybkC3shJzsmrjtirnrs4Tsi5wg6rCV64Ko6rWsIOuFvO2YhOuhnCA1NjY8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYTI5YWVhOGI2NTg0NGZjNjg0NmVjZDJjZmVmZTY0MWEuc2V0Q29udGVudChodG1sXzRlN2U0YWJjNWUwNTQzMTM5OTEwNzMxMGI2ZjcwODM5KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2E4ZGM4M2I0ZTBkNTRhOTVhODJkZjgyNjRlNzlkZjc0LmJpbmRQb3B1cChwb3B1cF9hMjlhZWE4YjY1ODQ0ZmM2ODQ2ZWNkMmNmZWZlNjQxYSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl80MzVhYTllNzNhODI0NDI1ODdkNjEzZTZmOTRiMjkwNiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUxNjcxMTk5OTkyMDUsIDEyNy4xMDUzODk2MDUwMDFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzVjYjA2YzA4NWUyYjQxMzc4ZjY2NGRlNDI3M2UzMTQzID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF84MDI3Zjc3Yzk5MGU0MmRkOWI1ZmUyZjljMzIyZTNmZSA9ICQoYDxkaXYgaWQ9Imh0bWxfODAyN2Y3N2M5OTBlNDJkZDliNWZlMmY5YzMyMmUzZmUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPu2VnOq1reqxtOqwleq0gOumrO2Yke2ajOyEnOyauO2KuS3shJzsmrjtirnrs4Tsi5wg7Iah7YyM6rWsIOyYpOq4iOuhnCA1ODwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81Y2IwNmMwODVlMmI0MTM3OGY2NjRkZTQyNzNlMzE0My5zZXRDb250ZW50KGh0bWxfODAyN2Y3N2M5OTBlNDJkZDliNWZlMmY5YzMyMmUzZmUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNDM1YWE5ZTczYTgyNDQyNTg3ZDYxM2U2Zjk0YjI5MDYuYmluZFBvcHVwKHBvcHVwXzVjYjA2YzA4NWUyYjQxMzc4ZjY2NGRlNDI3M2UzMTQzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzUxNWMyMGRlOTQzMjQzODBiYmRhZDQxMDhiMjFjYTZhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTEyMjQxNjQzOTE3OCwgMTI3LjAwODIwOTc2ODM4Ml0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNjIzNTQ4M2E5ZDM0NGNmMWI0ZThiMjU3NDFjOTY4NzYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzY4NDQwMTEyMmUxZTQ4Njk4NjYzMTVhZWY2Yjc5OTIwID0gJChgPGRpdiBpZD0iaHRtbF82ODQ0MDExMjJlMWU0ODY5ODY2MzE1YWVmNmI3OTkyMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7KCV7ZW067O17KeA67aA7ISk7ZWc7Iug66mU65SU7ZS87JWELeyEnOyauO2KueuzhOyLnCDshJzstIjqtawg7J6g7JuQ66GcIDg4PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzYyMzU0ODNhOWQzNDRjZjFiNGU4YjI1NzQxYzk2ODc2LnNldENvbnRlbnQoaHRtbF82ODQ0MDExMjJlMWU0ODY5ODY2MzE1YWVmNmI3OTkyMCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl81MTVjMjBkZTk0MzI0MzgwYmJkYWQ0MTA4YjIxY2E2YS5iaW5kUG9wdXAocG9wdXBfNjIzNTQ4M2E5ZDM0NGNmMWI0ZThiMjU3NDFjOTY4NzYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfN2JhNzU5MTQxOGZkNGVmNTlmMzIwZDI0MWE0OThjMjcgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy40ODUzNTUyOTM1MTYxLCAxMjcuMDM3NzYwOTE4NzI1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF83MzFlMzY1YTA2YmU0ZWM4ODAyMTYxYTJkZWQyYjIyZSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZDMxOGNhM2U2OTcxNGY2NGFmZjJkYTZiYzU4NmVjZGIgPSAkKGA8ZGl2IGlkPSJodG1sX2QzMThjYTNlNjk3MTRmNjRhZmYyZGE2YmM1ODZlY2RiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7shLHrsqDrk5zroZzrs5Hsm5At7ISc7Jq47Yq567OE7IucIOqwleuCqOq1rCDrgqjrtoDsiJztmZjroZwgMjYzMzwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83MzFlMzY1YTA2YmU0ZWM4ODAyMTYxYTJkZWQyYjIyZS5zZXRDb250ZW50KGh0bWxfZDMxOGNhM2U2OTcxNGY2NGFmZjJkYTZiYzU4NmVjZGIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfN2JhNzU5MTQxOGZkNGVmNTlmMzIwZDI0MWE0OThjMjcuYmluZFBvcHVwKHBvcHVwXzczMWUzNjVhMDZiZTRlYzg4MDIxNjFhMmRlZDJiMjJlKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzIxNWRjYzMxNzI4YTQ4ZGQ5NmY0NzdkY2I4NTZiZDljID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTEyMDM5MTA1MzEzLCAxMjYuOTIyMzQ4MTA1NDE1OTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2RhOGIyMWNhOWM3YzQ2YjM5NGRiZTYyZDEzYTNhMzNmID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF85YzViOTZkMTYzNzg0MTU2YWNiMjkzYzZkNTllNDE1MiA9ICQoYDxkaXYgaWQ9Imh0bWxfOWM1Yjk2ZDE2Mzc4NDE1NmFjYjI5M2M2ZDU5ZTQxNTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEseyVoOydmOujjOyerOuLqC3shJzsmrjtirnrs4Tsi5wg7JiB65Ox7Y+s6rWsIOyXrOydmOuMgOuwqeuhnDUz6ri4IDIyPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2RhOGIyMWNhOWM3YzQ2YjM5NGRiZTYyZDEzYTNhMzNmLnNldENvbnRlbnQoaHRtbF85YzViOTZkMTYzNzg0MTU2YWNiMjkzYzZkNTllNDE1Mik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8yMTVkY2MzMTcyOGE0OGRkOTZmNDc3ZGNiODU2YmQ5Yy5iaW5kUG9wdXAocG9wdXBfZGE4YjIxY2E5YzdjNDZiMzk0ZGJlNjJkMTNhM2EzM2YpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMWJiZmEwYmVhMDQ4NDNmZmJmMmE4ZGE0OTkzNDQzZWYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MDg2MjYxMzE4Mzc4LCAxMjcuMDU4NTc5NzE3NjQ3OTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2ViZWQ4MDEzNzAwZDRkODZhZmJjYjlkYjY4NDI1ZDNjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF84ODRiYWU4ODcxNjE0NTA0OTNhZGFhMjdhMTc3OTIyMSA9ICQoYDxkaXYgaWQ9Imh0bWxfODg0YmFlODg3MTYxNDUwNDkzYWRhYTI3YTE3NzkyMjEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlkmVOyEseuqqOuniOy3qOqzvC3shJzsmrjtirnrs4Tsi5wg6rCV64Ko6rWsIO2FjO2XpOuegOuhnDg36ri4IDEzPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2ViZWQ4MDEzNzAwZDRkODZhZmJjYjlkYjY4NDI1ZDNjLnNldENvbnRlbnQoaHRtbF84ODRiYWU4ODcxNjE0NTA0OTNhZGFhMjdhMTc3OTIyMSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8xYmJmYTBiZWEwNDg0M2ZmYmYyYThkYTQ5OTM0NDNlZi5iaW5kUG9wdXAocG9wdXBfZWJlZDgwMTM3MDBkNGQ4NmFmYmNiOWRiNjg0MjVkM2MpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjQ5MmJlODI4YmEwNDY5NGIxY2ZiYWQwODhiZTMxZDIgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NTk0NDYwNDgzMDk5LCAxMjYuOTY4MDgyNzU3MzExXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9mMzFlNmRjODYzZjE0ZWJiOWYzNDg3MTViYmRlNThhNSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZDk1NWE5ZGZlNTAyNGY4MTlhNGQ5Y2Y3M2ZlOWZkODUgPSAkKGA8ZGl2IGlkPSJodG1sX2Q5NTVhOWRmZTUwMjRmODE5YTRkOWNmNzNmZTlmZDg1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7smIHrgqjsnZjro4zsnqzri6gt7ISc7Jq47Yq567OE7IucIOykkeq1rCDssq3tjIzroZwgNDQ5LTE8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZjMxZTZkYzg2M2YxNGViYjlmMzQ4NzE1YmJkZTU4YTUuc2V0Q29udGVudChodG1sX2Q5NTVhOWRmZTUwMjRmODE5YTRkOWNmNzNmZTlmZDg1KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Y0OTJiZTgyOGJhMDQ2OTRiMWNmYmFkMDg4YmUzMWQyLmJpbmRQb3B1cChwb3B1cF9mMzFlNmRjODYzZjE0ZWJiOWYzNDg3MTViYmRlNThhNSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9kODEzMjNkMDE1MzU0NzJiOWQ3MzA5Y2MxOTgwYjVjNyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjU2NDgyMzA4MzA1NzUsIDEyNi45ODg2NTUwMzk0MTI5OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfM2Y2MGEzNDU2ZjE0NGYyZThhNDc2NDUyM2ZkZjI2YWUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzExZjU4NDY4NWJlNTQ4YjE4Nzg2ZmFkYmUxOTM0ZDBiID0gJChgPGRpdiBpZD0iaHRtbF8xMWY1ODQ2ODViZTU0OGIxODc4NmZhZGJlMTkzNGQwYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7J247KCc64yA7ZWZ6rWQ7ISc7Jq467Cx67OR7JuQLeyEnOyauO2KueuzhOyLnCDspJHqtawg66eI66W464K066GcIDk8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfM2Y2MGEzNDU2ZjE0NGYyZThhNDc2NDUyM2ZkZjI2YWUuc2V0Q29udGVudChodG1sXzExZjU4NDY4NWJlNTQ4YjE4Nzg2ZmFkYmUxOTM0ZDBiKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Q4MTMyM2QwMTUzNTQ3MmI5ZDczMDljYzE5ODBiNWM3LmJpbmRQb3B1cChwb3B1cF8zZjYwYTM0NTZmMTQ0ZjJlOGE0NzY0NTIzZmRmMjZhZSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9mM2E2ZmVlNjdmZjk0MDJlYWIyNTRiYjIzODljYWQ5NSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUyNzgxMDkxMjAzNDEsIDEyNy4xMjc5OTc5MDUxNjRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzg3OTdlMjVhNjE3MjRmYzg5NmJhYTRhOGQyMDUyYjRlID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF84OWY0MDUzY2U4ZGE0MjFiOGMyZmJjODk4Zjk1NWQxZCA9ICQoYDxkaXYgaWQ9Imh0bWxfODlmNDA1M2NlOGRhNDIxYjhjMmZiYzg5OGY5NTVkMWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPu2VnOq1re2VhOydmOujjOyerOuLqC3shJzsmrjtirnrs4Tsi5wg6rCV64+Z6rWsIOyEseuCtOuhnCA3MTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF84Nzk3ZTI1YTYxNzI0ZmM4OTZiYWE0YThkMjA1MmI0ZS5zZXRDb250ZW50KGh0bWxfODlmNDA1M2NlOGRhNDIxYjhjMmZiYzg5OGY5NTVkMWQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZjNhNmZlZTY3ZmY5NDAyZWFiMjU0YmIyMzg5Y2FkOTUuYmluZFBvcHVwKHBvcHVwXzg3OTdlMjVhNjE3MjRmYzg5NmJhYTRhOGQyMDUyYjRlKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzg2MGJhYzA1NGZjNjQyOTBhZDE1ZjNhMGRkZjNiNjY2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNDc5NjE3NTgxOTU4NzA2LCAxMjYuOTU2MjY5NTk4ODA2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zNjMyNTViNjE4MDQ0ZDhkODRjMDdjZjJmMjlkZmExNCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODU4ZGFhN2M5ZWEyNDhkN2I0MWFkZTk5ZDMwY2E3YzAgPSAkKGA8ZGl2IGlkPSJodG1sXzg1OGRhYTdjOWVhMjQ4ZDdiNDFhZGU5OWQzMGNhN2MwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7sgqzrnpHsnZjrs5Hsm5At7ISc7Jq47Yq567OE7IucIOq0gOyVheq1rCDrgqjrtoDsiJztmZjroZwgMTg2MDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zNjMyNTViNjE4MDQ0ZDhkODRjMDdjZjJmMjlkZmExNC5zZXRDb250ZW50KGh0bWxfODU4ZGFhN2M5ZWEyNDhkN2I0MWFkZTk5ZDMwY2E3YzApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfODYwYmFjMDU0ZmM2NDI5MGFkMTVmM2EwZGRmM2I2NjYuYmluZFBvcHVwKHBvcHVwXzM2MzI1NWI2MTgwNDRkOGQ4NGMwN2NmMmYyOWRmYTE0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzNiOWMxODIxZWM2ZTRjMDI5MTJkZGE1M2RiOTE4MjQwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTUyMjE1NTU0NTIwNiwgMTI2LjgzNTg3MTM3NTkwNV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfM2VhMDFkMzdlNmIwNGQ2ZGE1ZDZiNzI4ODZjMjI0ODYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzc4ZDdjMTZjY2M5NDQ0YWFhYmVmYzcxYzliN2FiZGU0ID0gJChgPGRpdiBpZD0iaHRtbF83OGQ3YzE2Y2NjOTQ0NGFhYWJlZmM3MWM5YjdhYmRlNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7ISx7IK87J2Y66OM7J6s64uo66+47KaI66mU65SU67OR7JuQLeyEnOyauO2KueuzhOyLnCDqsJXshJzqtawg6rCV7ISc66GcIDI5NTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zZWEwMWQzN2U2YjA0ZDZkYTVkNmI3Mjg4NmMyMjQ4Ni5zZXRDb250ZW50KGh0bWxfNzhkN2MxNmNjYzk0NDRhYWFiZWZjNzFjOWI3YWJkZTQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfM2I5YzE4MjFlYzZlNGMwMjkxMmRkYTUzZGI5MTgyNDAuYmluZFBvcHVwKHBvcHVwXzNlYTAxZDM3ZTZiMDRkNmRhNWQ2YjcyODg2YzIyNDg2KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzQ0MWM2MmNkMmM1NDRmOWZhNjY0MmFkYjZiNGQyOTgzID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTE4ODA2OTgwODc4NSwgMTI2LjkwMzg1Njk2NjEwOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMTNlYmY0Y2RhODk2NDU5NzliYTNhYjIwYzk5NTJjODIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2YyYzAwYzgyN2ZhZjRjNzFiMjQyZjBiYzFmNGYzNGQ0ID0gJChgPGRpdiBpZD0iaHRtbF9mMmMwMGM4MjdmYWY0YzcxYjI0MmYwYmMxZjRmMzRkNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7JSo7Jeg7Lap66y067OR7JuQLeyEnOyauO2KueuzhOyLnCDsmIHrk7Htj6zqtawg7JiB65Ox7Y+s66GcMzbquLggMTM8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMTNlYmY0Y2RhODk2NDU5NzliYTNhYjIwYzk5NTJjODIuc2V0Q29udGVudChodG1sX2YyYzAwYzgyN2ZhZjRjNzFiMjQyZjBiYzFmNGYzNGQ0KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzQ0MWM2MmNkMmM1NDRmOWZhNjY0MmFkYjZiNGQyOTgzLmJpbmRQb3B1cChwb3B1cF8xM2ViZjRjZGE4OTY0NTk3OWJhM2FiMjBjOTk1MmM4MikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85N2IxZGY4ZjFkMTE0ZDM5YmQ4M2MxYWQ1MTQ5MDY0NSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjU4MzU1NzA1ODA1MDUsIDEyNy4wODYwNDQ0Mjk4NTUwMV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZWRhMjAwMGJkZmJmNDYxNWE2YzAyN2NiNDkwNDEyNzIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzU4YjEyOTY1NWFjMzQyOTZiZmMyMTM0MWZjNjM5ODg1ID0gJChgPGRpdiBpZD0iaHRtbF81OGIxMjk2NTVhYzM0Mjk2YmZjMjEzNDFmYzYzOTg4NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7JuQ7KeE7J6s64uo67aA7ISk64W57IOJ67OR7JuQLeyEnOyauO2KueuzhOyLnCDspJHrnpHqtawg7IKs6rCA7KCV66GcNDnquLggNTM8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZWRhMjAwMGJkZmJmNDYxNWE2YzAyN2NiNDkwNDEyNzIuc2V0Q29udGVudChodG1sXzU4YjEyOTY1NWFjMzQyOTZiZmMyMTM0MWZjNjM5ODg1KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzk3YjFkZjhmMWQxMTRkMzliZDgzYzFhZDUxNDkwNjQ1LmJpbmRQb3B1cChwb3B1cF9lZGEyMDAwYmRmYmY0NjE1YTZjMDI3Y2I0OTA0MTI3MikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9iMTI3NTBjYzQxMzU0YWI4YTcyYTI1NjRiM2Y4MzU4ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjQ4ODQzNjQwMzk1MzYsIDEyNy4xMDI5Njc3OTA1NDUwMV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfODVkYmIzNTMwYjc5NDhkZmFjY2JjY2YxM2FjZjNmMGMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2MxNjc5ZDIxYjNiYTRkYmI5ZmMyNDYwZmFlYWM2NGViID0gJChgPGRpdiBpZD0iaHRtbF9jMTY3OWQyMWIzYmE0ZGJiOWZjMjQ2MGZhZWFjNjRlYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+6rCV64Ko7IS87Yq465+067OR7JuQLeyEnOyauO2KueuzhOyLnCDqsJXrgqjqtawg6rSR7Y+J66GcNTHquLggNi05PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzg1ZGJiMzUzMGI3OTQ4ZGZhY2NiY2NmMTNhY2YzZjBjLnNldENvbnRlbnQoaHRtbF9jMTY3OWQyMWIzYmE0ZGJiOWZjMjQ2MGZhZWFjNjRlYik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9iMTI3NTBjYzQxMzU0YWI4YTcyYTI1NjRiM2Y4MzU4ZS5iaW5kUG9wdXAocG9wdXBfODVkYmIzNTMwYjc5NDhkZmFjY2JjY2YxM2FjZjNmMGMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNzY1N2RlMjY5ZDQzNGI0NTg3Y2NiZDUwZmI5M2MxYTkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41MTkwODc0NzE3NzIzLCAxMjcuMDQ5NTkwNjMwMzY0XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9kMzA1ZjFiZWIxNjE0YmViOGFhNzUxNmMxYWY0OWI3ZiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmU2YWY2MmM1YjhkNDU3YWEyZWNhODA5NDg4YjJmNWQgPSAkKGA8ZGl2IGlkPSJodG1sX2JlNmFmNjJjNWI4ZDQ1N2FhMmVjYTgwOTQ4OGIyZjVkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7smrDrpqzrk6Trs5Hsm5At7ISc7Jq47Yq567OE7IucIOqwleuCqOq1rCDtlZnrj5nroZwgNDQ1PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2QzMDVmMWJlYjE2MTRiZWI4YWE3NTE2YzFhZjQ5YjdmLnNldENvbnRlbnQoaHRtbF9iZTZhZjYyYzViOGQ0NTdhYTJlY2E4MDk0ODhiMmY1ZCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl83NjU3ZGUyNjlkNDM0YjQ1ODdjY2JkNTBmYjkzYzFhOS5iaW5kUG9wdXAocG9wdXBfZDMwNWYxYmViMTYxNGJlYjhhYTc1MTZjMWFmNDliN2YpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfODEyZmFiODVlMmU1NDk5MjkyZThlNTBjZTY1NWEyNjUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NDEyMjQ2ODIzMDk3MDQsIDEyNy4wNzIwNDMxOTYxNDk5OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNzk2MDhhZjg5NDM2NGNkYWIzOTZjMDg3MTUwYjAxYjEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2FiMzQxMjg1NjBjNDQ3Nzg5ZjVjNTMyNTZkMjM5NGVkID0gJChgPGRpdiBpZD0iaHRtbF9hYjM0MTI4NTYwYzQ0Nzc4OWY1YzUzMjU2ZDIzOTRlZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+6rG06rWt64yA7ZWZ6rWQ67OR7JuQLeyEnOyauO2KueuzhOyLnCDqtJHsp4Tqtawg64ql64+Z66GcIDEyMC0xPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzc5NjA4YWY4OTQzNjRjZGFiMzk2YzA4NzE1MGIwMWIxLnNldENvbnRlbnQoaHRtbF9hYjM0MTI4NTYwYzQ0Nzc4OWY1YzUzMjU2ZDIzOTRlZCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl84MTJmYWI4NWUyZTU0OTkyOTJlOGU1MGNlNjU1YTI2NS5iaW5kUG9wdXAocG9wdXBfNzk2MDhhZjg5NDM2NGNkYWIzOTZjMDg3MTUwYjAxYjEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNTg1ZGZiYmMyZTI4NDhmN2E3MjI0MTI3Mjg3YTBjNTggPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NjY3NTQ2OTIyNDA1LCAxMjYuOTY3MTg5NTY3MzVdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2Y0ODMxODljMWRhNjQxYmE4M2Q4OWM3OGE4NjdiM2Q4ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80YzhmNTdhZjc2N2Q0NmQ3YWY5ZGIwNWUwOWFhMGZmNiA9ICQoYDxkaXYgaWQ9Imh0bWxfNGM4ZjU3YWY3NjdkNDZkN2FmOWRiMDVlMDlhYTBmZjYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEnOyauOyggeyLreyekOuzkeybkC3shJzsmrjtirnrs4Tsi5wg7KKF66Gc6rWsIOyDiOusuOyViOuhnCA5PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2Y0ODMxODljMWRhNjQxYmE4M2Q4OWM3OGE4NjdiM2Q4LnNldENvbnRlbnQoaHRtbF80YzhmNTdhZjc2N2Q0NmQ3YWY5ZGIwNWUwOWFhMGZmNik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl81ODVkZmJiYzJlMjg0OGY3YTcyMjQxMjcyODdhMGM1OC5iaW5kUG9wdXAocG9wdXBfZjQ4MzE4OWMxZGE2NDFiYTgzZDg5Yzc4YTg2N2IzZDgpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZGFiZDNiMWViNWRmNDBlYmFkODlkODM4NTc0ZjFhYTYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41Mjg0NzMyNjUyODY3MDYsIDEyNi44NjM2MTk2Mzc2ODE5OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9mMWUxMzgzMjI1Y2I0NDJhYTFjNjZhOWM2YjIwMzRlMCk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZDZiOTc4N2RkZWIyNDBjYWI4MGY2YjdmN2FjMDBmNGYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzRiNjc2MDQ5MGFjNTRiYTU4NThkNGFiZmQyOTk3NzZkID0gJChgPGRpdiBpZD0iaHRtbF80YjY3NjA0OTBhYzU0YmE1ODU4ZDRhYmZkMjk5Nzc2ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+7ZmN7J2167OR7JuQLeyEnOyauO2KueuzhOyLnCDslpHsspzqtawg66qp64+Z66GcIDIyNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9kNmI5Nzg3ZGRlYjI0MGNhYjgwZjZiN2Y3YWMwMGY0Zi5zZXRDb250ZW50KGh0bWxfNGI2NzYwNDkwYWM1NGJhNTg1OGQ0YWJmZDI5OTc3NmQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZGFiZDNiMWViNWRmNDBlYmFkODlkODM4NTc0ZjFhYTYuYmluZFBvcHVwKHBvcHVwX2Q2Yjk3ODdkZGViMjQwY2FiODBmNmI3ZjdhYzAwZjRmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzhiMjBjMGM1Y2M4MjRlNzJiNzIyMWIzNzdlMDg4NjU4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbMzcuNTAyMzgyMjM2OTMxMSwgMTI3LjAwNTg0MDcxMTU1Mzk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF81YzUyZmViMjVhYTE0YTA0YWM0NGQzNjZiYTBkNDVhYSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDc1NjRiNzE4ZjgzNDhiZmEwZWY3NzQ4ZWUxZGE0ZWEgPSAkKGA8ZGl2IGlkPSJodG1sXzQ3NTY0YjcxOGY4MzQ4YmZhMGVmNzc0OGVlMWRhNGVhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7shJzsmrjshLHrqqjrs5Hsm5DsnZHquInsnZjro4zshLzthLAt7ISc7Jq47Yq567OE7IucIOyEnOy0iOq1rCDrsJjtj6zrjIDroZwgMjIyPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzVjNTJmZWIyNWFhMTRhMDRhYzQ0ZDM2NmJhMGQ0NWFhLnNldENvbnRlbnQoaHRtbF80NzU2NGI3MThmODM0OGJmYTBlZjc3NDhlZTFkYTRlYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl84YjIwYzBjNWNjODI0ZTcyYjcyMjFiMzc3ZTA4ODY1OC5iaW5kUG9wdXAocG9wdXBfNWM1MmZlYjI1YWExNGEwNGFjNDRkMzY2YmEwZDQ1YWEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZDhjNjEyYWIyOGM5NDk2ODhhYjA5ZGVlYjg1MWZhMDUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy41NjM2NjI0OTEwOTY1LCAxMjYuOTg2NzU3NzkxNTg2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2YxZTEzODMyMjVjYjQ0MmFhMWM2NmE5YzZiMjAzNGUwKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yYzg2NzUwZmNmNzU0OWI3OTc5OWRhMDc3YWU4YWJlNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWY3N2VkY2VlMDFkNGZiYmEyODQzYTExY2MwNTEzOGYgPSAkKGA8ZGl2IGlkPSJodG1sXzVmNzdlZGNlZTAxZDRmYmJhMjg0M2ExMWNjMDUxMzhmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij7qsIDthqjrpq3rjIDtlZnqtZDsl6zsnZjrj4TshLHrqqjrs5Hsm5At7ISc7Jq47Yq567OE7IucIOykkeq1rCDrqoXrj5nquLggNzQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMmM4Njc1MGZjZjc1NDliNzk3OTlkYTA3N2FlOGFiZTYuc2V0Q29udGVudChodG1sXzVmNzdlZGNlZTAxZDRmYmJhMjg0M2ExMWNjMDUxMzhmKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Q4YzYxMmFiMjhjOTQ5Njg4YWIwOWRlZWI4NTFmYTA1LmJpbmRQb3B1cChwb3B1cF8yYzg2NzUwZmNmNzU0OWI3OTc5OWRhMDc3YWU4YWJlNikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9mOGEyY2U0MDIzOWU0ZGU2YjBmNTIxMTU2MzVjODIwZiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzM3LjUxODgwNjk4MDg3ODUsIDEyNi45MDM4NTY5NjYxMDldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzE5Y2RmNTQ2NGNlMzQ1NDY4YzgyMjczYjVlOTA4YjQxID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8yNmVmZTQ4MGQ2MmI0YzBmOWQxNmIyMTlkYjQ0YjIzYSA9ICQoYDxkaXYgaWQ9Imh0bWxfMjZlZmU0ODBkNjJiNGMwZjlkMTZiMjE5ZGI0NGIyM2EiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyUqOyXoOuzkeybkC3shJzsmrjtirnrs4Tsi5wg7JiB65Ox7Y+s6rWsIOyYgeuTse2PrOuhnDM26ri4IDEzPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzE5Y2RmNTQ2NGNlMzQ1NDY4YzgyMjczYjVlOTA4YjQxLnNldENvbnRlbnQoaHRtbF8yNmVmZTQ4MGQ2MmI0YzBmOWQxNmIyMTlkYjQ0YjIzYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mOGEyY2U0MDIzOWU0ZGU2YjBmNTIxMTU2MzVjODIwZi5iaW5kUG9wdXAocG9wdXBfMTljZGY1NDY0Y2UzNDU0NjhjODIyNzNiNWU5MDhiNDEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOTY0ODAzODkxZjM1NDdmOGIyMGNmNDgxOTgyZDliZTAgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFszNy40ODU2MDQyNjM3NTk2MDUsIDEyNy4wMzk1NjY5MTE1NThdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZjFlMTM4MzIyNWNiNDQyYWExYzY2YTljNmIyMDM0ZTApOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzY2YzFjNDhkYTMxNjRhY2FhNzFhOGEwZmZkNTYzNWQ2ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF85M2NjNmVhYzIwYTY0NmQxYjJmMGFjZWYyMDk4ZGZhNiA9ICQoYDxkaXYgaWQ9Imh0bWxfOTNjYzZlYWMyMGE2NDZkMWIyZjBhY2VmMjA5OGRmYTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPuyEseuyoOuTnOuhnOuzkeybkC3shJzsmrjtirnrs4Tsi5wg6rCV64Ko6rWsIOuCqOu2gOyInO2ZmOuhnCAyNjQ5PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzY2YzFjNDhkYTMxNjRhY2FhNzFhOGEwZmZkNTYzNWQ2LnNldENvbnRlbnQoaHRtbF85M2NjNmVhYzIwYTY0NmQxYjJmMGFjZWYyMDk4ZGZhNik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl85NjQ4MDM4OTFmMzU0N2Y4YjIwY2Y0ODE5ODJkOWJlMC5iaW5kUG9wdXAocG9wdXBfNjZjMWM0OGRhMzE2NGFjYWE3MWE4YTBmZmQ1NjM1ZDYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAo8L3NjcmlwdD4= onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
map.save('index.html')
```
