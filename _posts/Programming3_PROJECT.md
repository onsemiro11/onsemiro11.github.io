<center>
    <font size=5><b>Programming3 Project</b></font>
    <br><br>
    <b>: 노트북 데이터를 활용하여 사용자가 원하는 조건의 노트북들을 비교 분석하기</b>
</center>    
    <br><br>
    <div style="text-align: right"> 산업데이터사이언스학부 Team05</div>
    <div style="text-align: right"> 201804200 나승채</div>
    <div style="text-align: right"> 201804234 이현도</div>
    <div style="text-align: right"> 201804255 함도윤</div>
    
---
### >프로젝트 주제 선정 이유

: 노트북과 같이 다양한 스펙을 갖는 데이터가 있는 제품을 구매하고자 할때,  
 내가 원하는 스펙의 조건중 가장 합리적인 제품인 무엇인지 결정하는데에는 많은 어려움이 있다.  
 
 현재 네이버나 다나와 쇼핑몰에서는 cpu와 제조사, 화면 크기, 메모리, 저장 용량, 무게까지는 원하는 스펙을 선택하여  
 그에 맞는 제품을 확인할 수 있도록 구성되어 있지만, GPU나 배터리, HDMI 포트 유무, 두께, 웹캠 유무처럼  
 섬세한 스펙은 선택하여 볼 수 없다. 
 
 따라서 이런 부족한 부분을 보완하여 소비자가 자신에게 적합한 노트북을 선택할 수 있도록  
 도와주는 시스템을 구축하는 것이 본 프로젝트 주제 선정의 목표다.
 
 
 
---

### >데이터 소개

- 다나와 쇼핑몰에서 노트북를 검색하여 나오는 페이지와 각 노트북 상세 페이지에서 크롤링 진행


<div>
<img src="attachment:7ca1f9ca-32ee-4bbf-b8be-d9403aed46e5.png" width="650"/> <img src="attachment:7ed23631-adf6-4d66-9717-7518d03d80d8.png" width="650"/>
</div>

# Data Crawling  
: 다나와 쇼핑몰에서 '노트북'을 검색하여 인기상품목록을 크롤링하였다.


```python
import pandas as pd
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
matplotlib.rc('font', family='Malgun Gothic')
import seaborn as sns
import re
import time
plt.rcParams['axes.unicode_minus'] = False
from selenium import webdriver
from selenium.webdriver import Chrome
from selenium.webdriver import ChromeOptions
from bs4 import BeautifulSoup

options = ChromeOptions()
options.add_argument('headless')
```

    <frozen importlib._bootstrap>:228: RuntimeWarning: scipy._lib.messagestream.MessageStream size changed, may indicate binary incompatibility. Expected 56 from C header, got 64 from PyObject



```python
import numpy as np

item_list = []
want_title = ["화면 크기","CPU 종류","운영체제(OS)",'화면 비율','광시야각','메모리 타입','메모리','저장 용량','저장장치 종류','GPU 칩셋','HDMI','웹캠(HD)','배터리','어댑터','두께','무게','USB','ㅗ형 방향키','ㅡ형 방향키']

for page in range(1,10): #몇 페이지 까지 할 것인지 range 결정

    print('\n',page)
    url = "http://search.danawa.com/dsearch.php?query=%EB%85%B8%ED%8A%B8%EB%B6%81&originalQuery=%EB%85%B8%ED%8A%B8%EB%B6%81&previousKeyword=%EB%85%B8%ED%8A%B8%EB%B6%81&volumeType=allvs&page="+str(page)+"&limit=40&sort=saveDESC&list=list&boost=true&addDelivery=N&recommendedSort=Y&defaultUICategoryCode=112758&defaultPhysicsCategoryCode=860%7C869%7C10586%7C0&defaultVmTab=85012&defaultVaTab=7076658&tab=goods"
    driver = Chrome(executable_path='/Users/ihyeondo/Downloads/chromedriver') 
    driver.get(url) #셀레니움의 chrom 웹드라이버로 창 열기
    
    soup = BeautifulSoup(driver.page_source) #셀레니움으로 들어간 웹 창에서 beautifulSoup으로 크롤링 진행
    import numpy as np

item_list = []
want_title = ["화면 크기","CPU 종류","운영체제(OS)",'화면 비율','광시야각','메모리 타입','메모리','저장 용량','저장장치 종류','GPU 칩셋','HDMI','웹캠(HD)','배터리','어댑터','두께','무게','USB','ㅗ형 방향키','ㅡ형 방향키']

for page in range(1,10):

    print('\n',page)
    url = "http://search.danawa.com/dsearch.php?query=%EB%85%B8%ED%8A%B8%EB%B6%81&originalQuery=%EB%85%B8%ED%8A%B8%EB%B6%81&previousKeyword=%EB%85%B8%ED%8A%B8%EB%B6%81&volumeType=allvs&page="+str(page)+"&limit=40&sort=saveDESC&list=list&boost=true&addDelivery=N&recommendedSort=Y&defaultUICategoryCode=112758&defaultPhysicsCategoryCode=860%7C869%7C10586%7C0&defaultVmTab=85012&defaultVaTab=7076658&tab=goods"
    driver = Chrome(executable_path='/Users/ihyeondo/Downloads/chromedriver')
    driver.get(url)
    
    soup = BeautifulSoup(driver.page_source)
    product_li_tags = soup.select('p.prod_name a.click_log_product_standard_title_')
    num = 0
    for tg in product_li_tags:
        item_one_list = []
        
        name = tg.get_text()
        item_one_list.append(name)
        
        link = tg.attrs['href']
        url_item = link
        driver_item = Chrome(executable_path='/Users/ihyeondo/Downloads/chromedriver')
        driver_item.get(url_item)
        soup_item = BeautifulSoup(driver_item.page_source)

        item_inpo_title = soup_item.select('div.detail_cont div.prod_spec table th.tit')
        item_inpo_value = soup_item.select('div.detail_cont div.prod_spec table td.dsc')

        product_price   = soup_item.select('div.row span.lwst_prc em.prc_c')
        item_one_list.append(product_price[0].get_text())
        
        del item_inpo_title[0]
        del item_inpo_value[0]

        item_inpo_title_list = []
        item_inpo_value_list = []
        for tit,val in zip(item_inpo_title,item_inpo_value):
            title = tit.get_text()
            value = val.get_text()
            item_inpo_title_list.append(title)
            item_inpo_value_list.append(value)
            

        for i in want_title:
            if i in item_inpo_title_list:
                item_one_list.append(item_inpo_value_list[item_inpo_title_list.index(i)])
            else:
                item_one_list.append(np.nan)

        item_list.append(tuple(item_one_list))
        print(num,end=' ')
        num += 1
        

    #각 제품의 이름이 있는 부분들 모두 크롤링
    product_li_tags = soup.select('p.prod_name a.click_log_product_standard_title_')
    
    num = 0
    #for 문으로 하나하나 제품들 정보 크롤
        
    #한 제품의 정보들을 임시로 담아둘 list 생성
    item_one_list = []

    #제품 이름 크롤링
    name = tg.get_text()
    item_one_list.append(name)

    #제품 링크 크롤링 후 그 링크로 다시 창 띄우기 -> 상세 페이지 크롤링 진행
    link = tg.attrs['href']
    url_item = link
    driver_item = Chrome(executable_path='/Users/ihyeondo/Downloads/chromedriver')
    driver_item.get(url_item)
    soup_item = BeautifulSoup(driver_item.page_source)

    #제품 상세정보 제목들 크롤링
    item_inpo_title = soup_item.select('div.detail_cont div.prod_spec table th.tit')

    #제품 상세정보 정보들 크롤링
    item_inpo_value = soup_item.select('div.detail_cont div.prod_spec table td.dsc')

    #제품 최저가 크롤링
    product_price   = soup_item.select('div.row span.lwst_prc em.prc_c')
    item_one_list.append(product_price[0].get_text()) #제품 가격이 사이트 별로 많아서 첫번째 가격(최저가)으로 크롤링

    #상세정보에서 제조사에 두 문자열이 포함되어 있어, 크롤링 하기 힘들어 지워주었다.
    del item_inpo_title[0]
    del item_inpo_value[0]

    #위에서 제품 상세정보 제목들 & 정보들 크롤링한 것을 list에 모두 넣어두고 아래에서 indexing 진행
    item_inpo_title_list = []
    item_inpo_value_list = []
    for tit,val in zip(item_inpo_title,item_inpo_value):
        title = tit.get_text()
        value = val.get_text()
        item_inpo_title_list.append(title)
        item_inpo_value_list.append(value)

    #상세정보에서 원하다고 list 만들어놓은 want_title 요소들을 추출
    for i in want_title:
        if i in item_inpo_title_list:
            item_one_list.append(item_inpo_value_list[item_inpo_title_list.index(i)])
        else:
            item_one_list.append(np.nan)

    #한개의 제품에서 필요한 정보들을 넣어둔 item_onelist를 tuple형태로 item_list에 넣기
    item_list.append(tuple(item_one_list))
    print(num,end=' ')
    num += 1 #어디까지 크롤링 진행됐는지 확인
```

    
     1



    ---------------------------------------------------------------------------

    WebDriverException                        Traceback (most recent call last)

    /var/folders/vw/b3c7dks943vccln0gj5lpks40000gn/T/ipykernel_3699/3576060878.py in <module>
          9     url = "http://search.danawa.com/dsearch.php?query=%EB%85%B8%ED%8A%B8%EB%B6%81&originalQuery=%EB%85%B8%ED%8A%B8%EB%B6%81&previousKeyword=%EB%85%B8%ED%8A%B8%EB%B6%81&volumeType=allvs&page="+str(page)+"&limit=40&sort=saveDESC&list=list&boost=true&addDelivery=N&recommendedSort=Y&defaultUICategoryCode=112758&defaultPhysicsCategoryCode=860%7C869%7C10586%7C0&defaultVmTab=85012&defaultVaTab=7076658&tab=goods"
         10     driver = Chrome(executable_path='/Users/ihyeondo/Downloads/chromedriver')
    ---> 11     driver.get(url) #셀레니움의 chrom 웹드라이버로 창 열기
         12 
         13     soup = BeautifulSoup(driver.page_source) #셀레니움으로 들어간 웹 창에서 beautifulSoup으로 크롤링 진행


    ~/.local/lib/python3.9/site-packages/selenium/webdriver/remote/webdriver.py in get(self, url)
        307         Loads a web page in the current browser session.
        308         """
    --> 309         self.execute(Command.GET, {'url': url})
        310 
        311     @property


    ~/.local/lib/python3.9/site-packages/selenium/webdriver/remote/webdriver.py in execute(self, driver_command, params)
        295         response = self.command_executor.execute(driver_command, params)
        296         if response:
    --> 297             self.error_handler.check_response(response)
        298             response['value'] = self._unwrap_value(
        299                 response.get('value', None))


    ~/.local/lib/python3.9/site-packages/selenium/webdriver/remote/errorhandler.py in check_response(self, response)
        192         elif exception_class == UnexpectedAlertPresentException and 'alert' in value:
        193             raise exception_class(message, screen, stacktrace, value['alert'].get('text'))
    --> 194         raise exception_class(message, screen, stacktrace)
        195 
        196     def _value_or_default(self, obj, key, default):


    WebDriverException: Message: unknown error: cannot determine loading status
    from unknown error: cannot determine loading status
    from disconnected: received Inspector.detached event
      (Session info: chrome=105.0.5195.125)




```python

```


```python
#모두 담겨진 item_list를 해당 columns을 setting하여 dataframe으로 변환.
df_item = pd.DataFrame(item_list, columns = ['제품명','가격',"화면 크기","CPU 종류","운영체제(OS)",'화면 비율','광시야각','메모리 타입','메모리','저장 용량','저장장치 종류','GPU 칩셋','HDMI','웹캠(HD)','배터리','어댑터','두께','무게','USB','ㅗ형 방향키','ㅡ형 방향키'])

df_item
```


```python
#notebooks_final로 저장.
df_item.to_csv("data_story/Programming3/notebooks_final.csv", mode='w')
```


```python
df_item.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 360 entries, 0 to 359
    Data columns (total 21 columns):
     #   Column    Non-Null Count  Dtype 
    ---  ------    --------------  ----- 
     0   제품명       360 non-null    object
     1   가격        360 non-null    object
     2   화면 크기     360 non-null    object
     3   CPU 종류    360 non-null    object
     4   운영체제(OS)  360 non-null    object
     5   화면 비율     354 non-null    object
     6   광시야각      281 non-null    object
     7   메모리 타입    350 non-null    object
     8   메모리       360 non-null    object
     9   저장 용량     360 non-null    object
     10  저장장치 종류   360 non-null    object
     11  GPU 칩셋    353 non-null    object
     12  HDMI      225 non-null    object
     13  웹캠(HD)    266 non-null    object
     14  배터리       351 non-null    object
     15  어댑터       306 non-null    object
     16  두께        360 non-null    object
     17  무게        360 non-null    object
     18  USB       358 non-null    object
     19  ㅗ형 방향키    304 non-null    object
     20  ㅡ형 방향키    56 non-null     object
    dtypes: object(21)
    memory usage: 59.2+ KB


# 2. Data Preprocessing

## 데이터 값 수정  & 형 변환
  
  
  
***크롤링한 데이터를 보기 좋게 변환하기***


```python
notebooks = pd.read_csv("data_story/Programming3/notebooks_final.csv",index_col = '제품명')
notebooks = notebooks.drop(columns=['Unnamed: 0'])
```


```python
notebooks.head()
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
      <th>가격</th>
      <th>화면 크기</th>
      <th>CPU 종류</th>
      <th>운영체제(OS)</th>
      <th>화면 비율</th>
      <th>광시야각</th>
      <th>메모리 타입</th>
      <th>메모리</th>
      <th>저장 용량</th>
      <th>저장장치 종류</th>
      <th>GPU 칩셋</th>
      <th>HDMI</th>
      <th>웹캠(HD)</th>
      <th>배터리</th>
      <th>어댑터</th>
      <th>두께</th>
      <th>무게</th>
      <th>USB</th>
      <th>ㅗ형 방향키</th>
      <th>ㅡ형 방향키</th>
    </tr>
    <tr>
      <th>제품명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>레노버 아이디어패드 Slim3-15ITL 5D WIN10 16GB램</th>
      <td>637,190</td>
      <td>39.62cm(15.6인치)</td>
      <td>코어i5-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>○</td>
      <td>DDR4</td>
      <td>16GB</td>
      <td>256GB</td>
      <td>M.2(NVMe)</td>
      <td>Iris Xe</td>
      <td>○</td>
      <td>○</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.9mm</td>
      <td>1.65kg</td>
      <td>총3개</td>
      <td>NaN</td>
      <td>○</td>
    </tr>
    <tr>
      <th>삼성전자 갤럭시북 NT750XDZ-AM58S</th>
      <td>711,000</td>
      <td>39.6cm(15.6인치)</td>
      <td>코어i5-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>○</td>
      <td>LPDDR4x(온보드)</td>
      <td>8GB</td>
      <td>256GB</td>
      <td>M.2(NVMe)</td>
      <td>Iris Xe</td>
      <td>○</td>
      <td>○</td>
      <td>54Wh</td>
      <td>65W</td>
      <td>15.4mm</td>
      <td>1.55kg</td>
      <td>총4개</td>
      <td>○</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>LG전자 2022 그램16(12세대) 16ZD90Q-EX76K</th>
      <td>2,249,000</td>
      <td>40.6cm(16인치)</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>○</td>
      <td>LPDDR5(온보드)</td>
      <td>16GB</td>
      <td>256GB</td>
      <td>M.2(NVMe)</td>
      <td>RTX2050</td>
      <td>○</td>
      <td>NaN</td>
      <td>90Wh</td>
      <td>65W</td>
      <td>16.8mm</td>
      <td>1.285kg</td>
      <td>총4개</td>
      <td>○</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>레노버 요가 Slim7 Carbon 14ACN6 82L0004YKR</th>
      <td>1,568,000</td>
      <td>35.56cm(14인치)</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>NaN</td>
      <td>LPDDR4x(온보드)</td>
      <td>16GB</td>
      <td>1TB</td>
      <td>M.2(NVMe)</td>
      <td>MX450</td>
      <td>NaN</td>
      <td>○</td>
      <td>61Wh</td>
      <td>NaN</td>
      <td>14.9mm</td>
      <td>1.09kg</td>
      <td>총3개</td>
      <td>NaN</td>
      <td>○</td>
    </tr>
    <tr>
      <th>레노버 V15 G2 82KD000UKR 8GB램</th>
      <td>444,990</td>
      <td>39.62cm(15.6인치)</td>
      <td>라이젠5-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>NaN</td>
      <td>DDR4</td>
      <td>8GB</td>
      <td>256GB</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
      <td>○</td>
      <td>NaN</td>
      <td>38Wh</td>
      <td>NaN</td>
      <td>19.9mm</td>
      <td>1.7kg</td>
      <td>총3개</td>
      <td>NaN</td>
      <td>○</td>
    </tr>
  </tbody>
</table>
</div>




```python
#데이터를 분석하기 좋게 값들과 형을 변환해주었다.
def preprocessing_data(df):
    
    # ㅗ형 방향키 & ㅡ형 방향키 수정
    # na가 아닌 값이 있는 ㅗ형 방향키와 ㅡ형 방향키 값을 더하면 360개라서(즉, 두 열에 존재하는 값을 더하면 결측치가 없는 열을 만들 수 있다.)
    # ㅗ형 방향키는 값이 있고 ㅡ형 방향키에는 값이 없으면, '1',
    # ㅡ형 방향키는 값이 없고 ㅡ형 방향키에는 값이 있으면, '0'으로 되어있는 열을 생성
    make_wk_list = []
    for wk1,wk2 in zip(df['ㅗ형 방향키'].fillna('0'),df['ㅡ형 방향키'].fillna('0')):
        if (wk1 == '0') & (wk2 != '0'):
            make_wk_list.append('0')
        elif (wk1 != '0') & (wk2 == '0'):
            make_wk_list.append('1')
        else:
            make_wk_list.append(np.nan)
    make_wk = pd.DataFrame(make_wk_list,columns = ['방향키(ㅡ형/ㅗ형)'],dtype = 'category')
    
    #USB 열 수정
    #'n개'형태로 되어있는 값에서 n만 추출하여 값으로 지정함. dtype은 int8로 변환
    def pre_usb(val):
        if val == '0':
            return val
        else:
            return val[1:-1]
    make_usb = df['USB'].fillna('0').apply(pre_usb).rename('USB(개)').astype(np.int8)
    
    #무게 열 수정
    #'nkg'형태로 되어있는 값에서 n만 추출하여 값으로 지정함. dtype은 float64로 변환하고 소숫점 2째자리까지만
    def pre_kg(mm):
        if mm[-2:] == 'kg':
            return mm[:-2]
        else:
            return str(float(mm[:-1])*0.001)
    make_kg = df['무게'].fillna('0').apply(pre_kg).rename('무게(kg)').astype(np.float64).round(2)
    
    #어댑터 열 수정
    #'nW'형태로 되어있는 값에서 n만 추출하여 값으로 지정함. dtype은 float64로 변환하고 소숫점 2째자리까지만
    def pre_ad(ad):
        if ad =='0':
            return np.nan
        else:
            return ad[:-1]
    make_ad = df['어댑터'].fillna('0').apply(pre_ad).rename('어댑터(W)').astype(np.float64).round(2)
    
    #HDMI 열 수정
    #'○'값은 1로 결측치는 '0'으로 지정.
    make_hdmi = df['HDMI'].fillna('0').replace('○','1').astype('category')
    
    #저장 용량 열 수정
    #저장 용량에는 1tb와 2tb 4tb도 있어서 기본 단위는 GB로 하고 TB는 1tb = 1024 ,2tb = 2048로 변환해주었다.
    def pre_sm(sm):
        msm = sm[:-2]
        if msm == '1':
            return msm+'024'
        elif msm == '2':
            return msm+'048'
        elif msm == '4':
            return msm+'096'
        else:
            return msm
    make_sm = df['저장 용량'].apply(pre_sm).rename('저장 용량(GB)').astype(np.int16)
    
    #메모리 열 수정
    #'nGB'형태로 되어있는 값에서 n만 추출하여 값으로 지정함. dtype은 int16로 변환
    def pre_mem(mem):
        return mem[:-2]
    make_mem = df['메모리'].apply(pre_mem).rename('메모리(GB)').astype(np.int16)
    
    #두께 열 수정
    #'nmm'형태로 되어있는 값에서 n만 추출하여 값으로 지정함. dtype은 float64로 변환하고 소숫점 2째자리까지만
    make_mm = df['두께'].apply(pre_mem).rename('두께(mm)').astype(np.float64).round(2)
    
    #가격 열 수정
    #중간중간에 ,를 지워주고 dtype는 int32로 변환
    def pre_pri(pr):
        return pr.replace(',','')
    make_pr = df['가격'].apply(pre_pri).astype(np.int32)
    
    #화면크기 열 수정
    #인치 숫자만 추출하기 위해 앞에 cm단위 값들고 '('')'값들을 모두 없애주고 dtypesm는 category로 변환
    def pre_ds(ds):
        return float(ds.split('(')[-1][:-3])
    make_ds = df['화면 크기'].apply(pre_ds).astype(np.float64).round(2)
    
    #웹캠 열 수정
    #'○'값은 1로 결측치는 '0'으로 지정.
    make_wc = df['웹캠(HD)'].fillna('0').replace('○','1').astype('category')
    
    #광시야각 수정
    #'○'값은 1로 결측치는 '0'으로 지정.
    make_la = df['광시야각'].fillna('0').replace('○','1').astype('category')
    
    #제조사 열 생성
    #제조사는 제품명 모두 맨 앞 단어가 제조사로 되어있는 것으로 판단하여 제품명에서 split[0]으로 추출
    def pre_md(md):
        return md.split(' ')[0]
    make_md = df['제품명'].apply(pre_md).rename('제조사')
    
    #배터리 수정
    #배터리에 na값이 있어 -1로 변환해주고 'nWh'형태로 되어 있는 값에서 n만 추출하여 값으로 지정함. 
    #dtype은 float64로 변환하고 소숫점 2째자리까지만
    def pre_bt(bt):
        if bt == '0':
            return '-1'
        else:
            return bt[:-2]
    make_bt = df['배터리'].fillna('0').apply(pre_bt).rename('배터리(Wh)').astype(np.float64).round(2)
    
    #마지막에 return 값을 pd.concat으로 변환되어 만들어진 list들을 합해줌.
    return pd.concat(
        [make_md,make_pr,make_usb,make_kg,make_ad,make_hdmi,make_sm,
         make_mem,make_mm,make_ds,make_wc,make_la,make_bt,make_wk,
        df['CPU 종류'],df['운영체제(OS)'],df['화면 비율'],df['메모리 타입'],df['저장장치 종류'],df['GPU 칩셋']],axis=1)
```


```python
notebooks.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 360 entries, 레노버 아이디어패드 Slim3-15ITL 5D WIN10 16GB램 to 한성컴퓨터 TFX3150U Pro
    Data columns (total 20 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   CPU 종류      360 non-null    object
     1   화면 비율       354 non-null    object
     2   광시야각        360 non-null    object
     3   메모리 타입      350 non-null    object
     4   저장장치 종류     360 non-null    object
     5   GPU 칩셋      353 non-null    object
     6   HDMI        360 non-null    object
     7   가격(원)       360 non-null    object
     8   화면 크기(인치)   360 non-null    object
     9   운영체제        360 non-null    object
     10  메모리(GB)     360 non-null    object
     11  저장 용량(GB)   360 non-null    object
     12  웹캠          360 non-null    object
     13  배터리(Wh)     351 non-null    object
     14  어댑터(W)      306 non-null    object
     15  두께(mm)      360 non-null    object
     16  무게(kg)      360 non-null    object
     17  USB(개)      360 non-null    object
     18  방향키(ㅗ형/ㅡ형)  360 non-null    object
     19  제조사         360 non-null    object
    dtypes: object(20)
    memory usage: 59.1+ KB


# Data Mining 

## 간단하게 가격 확인


```python
total_notebooks = notebooks.index.size
max_price = notebooks['가격(원)'].values.max()
min_price = notebooks['가격(원)'].values.min()
mean_price = notebooks['가격(원)'].values.mean()

print(f'현재 판매중인 전체 {total_notebooks}개의 노트북 중 \n최대가격은 {max_price}원이고\n최소가격은 {min_price}이며,\n평균 판매 금액은 {mean_price.round(2)}원이다.')
```

    현재 판매중인 전체 360개의 노트북 중 
    최대가격은 6990000원이고
    최소가격은 278990이며,
    평균 판매 금액은 1619070.67원이다.


## 세부적으로 가격 확인


```python
# 그룹화 이후 인덱스조정을 위한 함수 설정

def flat_cols(df):
    df.columns = ['_'.join(x) for x in df.columns.to_flat_index()]
    return df
```


```python
# 제조사별 가격의 집계함수들을 계산

notebooks_group_1 = (
    notebooks.groupby(['제조사'])[['가격(원)']]
    .agg(['size', 'max', 'min', 'mean', 'std'])
    .pipe(flat_cols)
    .reset_index()
)

notebooks_group_1
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
      <th>제조사</th>
      <th>가격(원)_size</th>
      <th>가격(원)_max</th>
      <th>가격(원)_min</th>
      <th>가격(원)_mean</th>
      <th>가격(원)_std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>APPLE</td>
      <td>10</td>
      <td>3287290</td>
      <td>1088300</td>
      <td>2.280912e+06</td>
      <td>8.815324e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ASUS</td>
      <td>42</td>
      <td>5590000</td>
      <td>278990</td>
      <td>1.725610e+06</td>
      <td>1.052265e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DELL</td>
      <td>7</td>
      <td>6039870</td>
      <td>1390000</td>
      <td>3.385696e+06</td>
      <td>1.635333e+06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GIGABYTE</td>
      <td>8</td>
      <td>4579000</td>
      <td>1279000</td>
      <td>2.656750e+06</td>
      <td>1.107711e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HP</td>
      <td>11</td>
      <td>2480000</td>
      <td>453710</td>
      <td>1.272491e+06</td>
      <td>7.409525e+05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>LG전자</td>
      <td>52</td>
      <td>2349000</td>
      <td>544370</td>
      <td>1.415076e+06</td>
      <td>5.049818e+05</td>
    </tr>
    <tr>
      <th>6</th>
      <td>MSI</td>
      <td>97</td>
      <td>6990000</td>
      <td>549000</td>
      <td>1.699242e+06</td>
      <td>1.067877e+06</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Microsoft</td>
      <td>1</td>
      <td>3408990</td>
      <td>3408990</td>
      <td>3.408990e+06</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Razer</td>
      <td>5</td>
      <td>5609000</td>
      <td>3090000</td>
      <td>3.803398e+06</td>
      <td>1.034101e+06</td>
    </tr>
    <tr>
      <th>9</th>
      <td>디클</td>
      <td>1</td>
      <td>299000</td>
      <td>299000</td>
      <td>2.990000e+05</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>레노버</td>
      <td>54</td>
      <td>6488990</td>
      <td>298990</td>
      <td>1.244582e+06</td>
      <td>8.919317e+05</td>
    </tr>
    <tr>
      <th>11</th>
      <td>삼성전자</td>
      <td>39</td>
      <td>2584060</td>
      <td>568990</td>
      <td>1.479044e+06</td>
      <td>5.086622e+05</td>
    </tr>
    <tr>
      <th>12</th>
      <td>에이서</td>
      <td>3</td>
      <td>1648820</td>
      <td>678180</td>
      <td>1.118400e+06</td>
      <td>4.915664e+05</td>
    </tr>
    <tr>
      <th>13</th>
      <td>주연테크</td>
      <td>11</td>
      <td>1449000</td>
      <td>281180</td>
      <td>9.742445e+05</td>
      <td>3.510498e+05</td>
    </tr>
    <tr>
      <th>14</th>
      <td>한성컴퓨터</td>
      <td>19</td>
      <td>2879000</td>
      <td>498000</td>
      <td>1.501684e+06</td>
      <td>7.434673e+05</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 판매수량이 많은순, 최고가격, 최소가격, 가격평균이 큰 순, 가격 표준편차가 높은 순으로 정렬
# 컬럼이름을 변경

sorted_group_1 = notebooks_group_1.sort_values(['가격(원)_size', '가격(원)_max', '가격(원)_min', '가격(원)_mean', '가격(원)_std'],
                              ascending = [False, False, True, False, False])


sorted_group_1 = sorted_group_1.rename(columns = {'가격(원)_size' : '판매 개수',
                                                  '가격(원)_max' : '최고 금액',
                                                  '가격(원)_min' : '최소 금액',
                                                  '가격(원)_mean' : '가격 평균',
                                                  '가격(원)_std' : '가격 표준편차'})
sorted_group_1
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
      <th>제조사</th>
      <th>판매 개수</th>
      <th>최고 금액</th>
      <th>최소 금액</th>
      <th>가격 평균</th>
      <th>가격 표준편차</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>MSI</td>
      <td>97</td>
      <td>6990000</td>
      <td>549000</td>
      <td>1.699242e+06</td>
      <td>1.067877e+06</td>
    </tr>
    <tr>
      <th>10</th>
      <td>레노버</td>
      <td>54</td>
      <td>6488990</td>
      <td>298990</td>
      <td>1.244582e+06</td>
      <td>8.919317e+05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>LG전자</td>
      <td>52</td>
      <td>2349000</td>
      <td>544370</td>
      <td>1.415076e+06</td>
      <td>5.049818e+05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ASUS</td>
      <td>42</td>
      <td>5590000</td>
      <td>278990</td>
      <td>1.725610e+06</td>
      <td>1.052265e+06</td>
    </tr>
    <tr>
      <th>11</th>
      <td>삼성전자</td>
      <td>39</td>
      <td>2584060</td>
      <td>568990</td>
      <td>1.479044e+06</td>
      <td>5.086622e+05</td>
    </tr>
    <tr>
      <th>14</th>
      <td>한성컴퓨터</td>
      <td>19</td>
      <td>2879000</td>
      <td>498000</td>
      <td>1.501684e+06</td>
      <td>7.434673e+05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HP</td>
      <td>11</td>
      <td>2480000</td>
      <td>453710</td>
      <td>1.272491e+06</td>
      <td>7.409525e+05</td>
    </tr>
    <tr>
      <th>13</th>
      <td>주연테크</td>
      <td>11</td>
      <td>1449000</td>
      <td>281180</td>
      <td>9.742445e+05</td>
      <td>3.510498e+05</td>
    </tr>
    <tr>
      <th>0</th>
      <td>APPLE</td>
      <td>10</td>
      <td>3287290</td>
      <td>1088300</td>
      <td>2.280912e+06</td>
      <td>8.815324e+05</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GIGABYTE</td>
      <td>8</td>
      <td>4579000</td>
      <td>1279000</td>
      <td>2.656750e+06</td>
      <td>1.107711e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DELL</td>
      <td>7</td>
      <td>6039870</td>
      <td>1390000</td>
      <td>3.385696e+06</td>
      <td>1.635333e+06</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Razer</td>
      <td>5</td>
      <td>5609000</td>
      <td>3090000</td>
      <td>3.803398e+06</td>
      <td>1.034101e+06</td>
    </tr>
    <tr>
      <th>12</th>
      <td>에이서</td>
      <td>3</td>
      <td>1648820</td>
      <td>678180</td>
      <td>1.118400e+06</td>
      <td>4.915664e+05</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Microsoft</td>
      <td>1</td>
      <td>3408990</td>
      <td>3408990</td>
      <td>3.408990e+06</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>디클</td>
      <td>1</td>
      <td>299000</td>
      <td>299000</td>
      <td>2.990000e+05</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(20, 10))
plt.grid(linestyle='--')
plt.title('제조사별 가격 평균 시각화')
sns.barplot(x='제조사', y='가격 평균', data=sorted_group_1);
```


    
![png](output_19_0.png)
    


### 우리에게 익숙한 3개 제조사의 시장 점유율


```python
top3 = sorted_group_1.loc[[5, 11, 0]].set_index('제조사')
top3
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
      <th>판매 개수</th>
      <th>최고 금액</th>
      <th>최소 금액</th>
      <th>가격 평균</th>
      <th>가격 표준편차</th>
    </tr>
    <tr>
      <th>제조사</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LG전자</th>
      <td>52</td>
      <td>2349000</td>
      <td>544370</td>
      <td>1.415076e+06</td>
      <td>504981.833535</td>
    </tr>
    <tr>
      <th>삼성전자</th>
      <td>39</td>
      <td>2584060</td>
      <td>568990</td>
      <td>1.479044e+06</td>
      <td>508662.162301</td>
    </tr>
    <tr>
      <th>APPLE</th>
      <td>10</td>
      <td>3287290</td>
      <td>1088300</td>
      <td>2.280912e+06</td>
      <td>881532.396352</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 28.06%를 차지한다

(top3['판매 개수'].values.sum() / sorted_group_1['판매 개수'].values.sum() * 100).round(2)
```




    28.06




```python
ratio = [28.08,71.92]
labels = ['Top3', 'Others']
colors = ['red', 'gray']
explode = [0.10, 0]

fig, ax = plt.subplots(figsize=(10,5))
plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260,
        explode = explode, counterclock=False, shadow=True, colors=colors)
plt.show()
```


    
![png](output_23_0.png)
    


### CPU종류와 GPU종류에 따른 가격


```python
notebooks_group_2 = (
     notebooks.groupby(['제조사', 'CPU 종류', 'GPU 칩셋'])[['가격(원)']]
    .agg(['max', 'min', 'mean'])
    .pipe(flat_cols)
    .reset_index(['CPU 종류', 'GPU 칩셋'])
)

notebooks_group_2
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
      <th>CPU 종류</th>
      <th>GPU 칩셋</th>
      <th>가격(원)_max</th>
      <th>가격(원)_min</th>
      <th>가격(원)_mean</th>
    </tr>
    <tr>
      <th>제조사</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>APPLE</th>
      <td>실리콘 M1</td>
      <td>M1 7 core</td>
      <td>1098260</td>
      <td>1088300</td>
      <td>1093280.0</td>
    </tr>
    <tr>
      <th>APPLE</th>
      <td>실리콘 M1</td>
      <td>M1 8 core</td>
      <td>1595320</td>
      <td>1424980</td>
      <td>1510150.0</td>
    </tr>
    <tr>
      <th>APPLE</th>
      <td>실리콘 M1 PRO</td>
      <td>M1 PRO 14 core</td>
      <td>2584000</td>
      <td>2584000</td>
      <td>2584000.0</td>
    </tr>
    <tr>
      <th>APPLE</th>
      <td>실리콘 M1 PRO</td>
      <td>M1 PRO 16 core</td>
      <td>3287290</td>
      <td>3003140</td>
      <td>3108565.0</td>
    </tr>
    <tr>
      <th>ASUS</th>
      <td>라이젠7-4세대</td>
      <td>RTX3050</td>
      <td>1170000</td>
      <td>1170000</td>
      <td>1170000.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>코어i7-12세대</td>
      <td>RTX3050 Ti</td>
      <td>1559000</td>
      <td>1559000</td>
      <td>1559000.0</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>코어i7-12세대</td>
      <td>RTX3060</td>
      <td>1899000</td>
      <td>1899000</td>
      <td>1899000.0</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>코어i7-12세대</td>
      <td>RTX3070 Ti</td>
      <td>2459000</td>
      <td>2359000</td>
      <td>2409000.0</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>코어i9-12세대</td>
      <td>RTX3060</td>
      <td>2259000</td>
      <td>2259000</td>
      <td>2259000.0</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>코어i9-12세대</td>
      <td>RTX3070 Ti</td>
      <td>2879000</td>
      <td>2749000</td>
      <td>2814000.0</td>
    </tr>
  </tbody>
</table>
<p>155 rows × 5 columns</p>
</div>



## 제조사별 화면크기 제작 성향 비교  
: 전반적인 화면 분포를 확인해보았을때,  
제조사별로 최소 13인치 ~ 최대 17인치의 화면을 제작하는 것으로 나타나며  
전반적인 화면의 평균값은 15.27인치에 해당한다.


```python
notebooks_group_3 = (
    notebooks.groupby('제조사')[['화면 크기(인치)']]
    .agg(['max', 'min', 'mean']).round()
    .pipe(flat_cols)
)

notebooks_group_3
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
      <th>화면 크기(인치)_max</th>
      <th>화면 크기(인치)_min</th>
      <th>화면 크기(인치)_mean</th>
    </tr>
    <tr>
      <th>제조사</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>APPLE</th>
      <td>16.0</td>
      <td>13.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>ASUS</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>DELL</th>
      <td>17.0</td>
      <td>16.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>GIGABYTE</th>
      <td>17.0</td>
      <td>16.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>HP</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>LG전자</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>MSI</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>Microsoft</th>
      <td>14.0</td>
      <td>14.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>Razer</th>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>디클</th>
      <td>14.0</td>
      <td>14.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>레노버</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>삼성전자</th>
      <td>16.0</td>
      <td>13.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>에이서</th>
      <td>16.0</td>
      <td>14.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>주연테크</th>
      <td>17.0</td>
      <td>14.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>17.0</td>
      <td>13.0</td>
      <td>16.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
notebooks_group_3['화면 크기(인치)_mean'].values.mean().round(2)
```




    15.27



## 제조사별 노트북의 무게 비교
: '가벼운 노트북'으로는 LG의 그램으로 대중들에게 익숙하지만,  
실제 가벼운 가장 노트북을 가진 제조사는 삼성전자에 해당한다.  
또, 전반적인 노트북들의 평균 무게는 1.83kg에 해당한다


```python
notebooks_group_4 = (
    notebooks.groupby('제조사')[['무게(kg)']]
    .agg(['max', 'min', 'mean']).round(3)
    .pipe(flat_cols)

)

notebooks_group_4
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
      <th>무게(kg)_max</th>
      <th>무게(kg)_min</th>
      <th>무게(kg)_mean</th>
    </tr>
    <tr>
      <th>제조사</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>APPLE</th>
      <td>2.10</td>
      <td>1.29</td>
      <td>1.637</td>
    </tr>
    <tr>
      <th>ASUS</th>
      <td>2.90</td>
      <td>1.14</td>
      <td>2.026</td>
    </tr>
    <tr>
      <th>DELL</th>
      <td>2.96</td>
      <td>1.81</td>
      <td>2.289</td>
    </tr>
    <tr>
      <th>GIGABYTE</th>
      <td>2.60</td>
      <td>2.20</td>
      <td>2.375</td>
    </tr>
    <tr>
      <th>HP</th>
      <td>2.46</td>
      <td>0.97</td>
      <td>1.812</td>
    </tr>
    <tr>
      <th>LG전자</th>
      <td>1.85</td>
      <td>0.98</td>
      <td>1.350</td>
    </tr>
    <tr>
      <th>MSI</th>
      <td>2.90</td>
      <td>1.29</td>
      <td>2.115</td>
    </tr>
    <tr>
      <th>Microsoft</th>
      <td>1.82</td>
      <td>1.82</td>
      <td>1.820</td>
    </tr>
    <tr>
      <th>Razer</th>
      <td>2.75</td>
      <td>1.78</td>
      <td>2.066</td>
    </tr>
    <tr>
      <th>디클</th>
      <td>1.30</td>
      <td>1.30</td>
      <td>1.300</td>
    </tr>
    <tr>
      <th>레노버</th>
      <td>2.90</td>
      <td>0.97</td>
      <td>1.909</td>
    </tr>
    <tr>
      <th>삼성전자</th>
      <td>2.39</td>
      <td>0.87</td>
      <td>1.317</td>
    </tr>
    <tr>
      <th>에이서</th>
      <td>2.50</td>
      <td>1.20</td>
      <td>1.697</td>
    </tr>
    <tr>
      <th>주연테크</th>
      <td>2.50</td>
      <td>1.12</td>
      <td>1.994</td>
    </tr>
    <tr>
      <th>한성컴퓨터</th>
      <td>2.60</td>
      <td>1.00</td>
      <td>1.774</td>
    </tr>
  </tbody>
</table>
</div>




```python
notebooks_group_4['무게(kg)_mean'].values.mean().round(2)
```




    1.83



## 3.5. 제조사별 GPU 칩셋 가격 비교
: 제조사별 GPU 칩셋의 값들을 가격이 싼 것부터 나열해봤다.


```python
notebooks.groupby(['제조사','GPU 칩셋'],as_index=False).apply(lambda df: df.sort_values(
    '가격',ascending=False
    ).head(1)
).droplevel(0)
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
      <th>제조사</th>
      <th>가격</th>
      <th>USB(개)</th>
      <th>무게(kg)</th>
      <th>어댑터(W)</th>
      <th>HDMI</th>
      <th>저장 용량(GB)</th>
      <th>메모리(GB)</th>
      <th>두께(mm)</th>
      <th>화면 크기</th>
      <th>웹캠(HD)</th>
      <th>광시야각</th>
      <th>배터리(Wh)</th>
      <th>방향키(ㅡ형/ㅗ형)</th>
      <th>CPU 종류</th>
      <th>운영체제(OS)</th>
      <th>화면 비율</th>
      <th>메모리 타입</th>
      <th>저장장치 종류</th>
      <th>GPU 칩셋</th>
    </tr>
    <tr>
      <th>제품명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>APPLE 2020 맥북에어 MGN63KH/A</th>
      <td>APPLE</td>
      <td>1098260</td>
      <td>2</td>
      <td>1.29</td>
      <td>30.0</td>
      <td>0</td>
      <td>256</td>
      <td>8</td>
      <td>16.1</td>
      <td>13.3</td>
      <td>1</td>
      <td>0</td>
      <td>49.90</td>
      <td>1</td>
      <td>실리콘 M1</td>
      <td>macOS Big Sur</td>
      <td>16:10</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 7 core</td>
    </tr>
    <tr>
      <th>APPLE 2020 맥북프로13 MYD82KH/A</th>
      <td>APPLE</td>
      <td>1595320</td>
      <td>2</td>
      <td>1.40</td>
      <td>61.0</td>
      <td>0</td>
      <td>256</td>
      <td>8</td>
      <td>15.6</td>
      <td>13.3</td>
      <td>1</td>
      <td>0</td>
      <td>58.20</td>
      <td>1</td>
      <td>실리콘 M1</td>
      <td>macOS Big Sur</td>
      <td>16:10</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 8 core</td>
    </tr>
    <tr>
      <th>APPLE 2021 맥북프로14 MKGP3KH/A</th>
      <td>APPLE</td>
      <td>2584000</td>
      <td>3</td>
      <td>1.60</td>
      <td>67.0</td>
      <td>1</td>
      <td>512</td>
      <td>16</td>
      <td>15.5</td>
      <td>14.2</td>
      <td>0</td>
      <td>0</td>
      <td>70.00</td>
      <td>1</td>
      <td>실리콘 M1 PRO</td>
      <td>macOS Monterey</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 PRO 14 core</td>
    </tr>
    <tr>
      <th>APPLE 2021 맥북프로16 MK193KH/A</th>
      <td>APPLE</td>
      <td>3287290</td>
      <td>3</td>
      <td>2.10</td>
      <td>140.0</td>
      <td>1</td>
      <td>1024</td>
      <td>16</td>
      <td>16.8</td>
      <td>16.2</td>
      <td>0</td>
      <td>0</td>
      <td>100.00</td>
      <td>1</td>
      <td>실리콘 M1 PRO</td>
      <td>macOS Monterey</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 PRO 16 core</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming F15 FX506LH-HN004</th>
      <td>ASUS</td>
      <td>849000</td>
      <td>4</td>
      <td>2.30</td>
      <td>150.0</td>
      <td>0</td>
      <td>512</td>
      <td>8</td>
      <td>24.9</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>48.00</td>
      <td>1</td>
      <td>코어i5-10세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>GTX1650</td>
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
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7275T</th>
      <td>한성컴퓨터</td>
      <td>1559000</td>
      <td>4</td>
      <td>2.60</td>
      <td>150.0</td>
      <td>0</td>
      <td>500</td>
      <td>16</td>
      <td>28.0</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.35</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3050 Ti</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7276LC</th>
      <td>한성컴퓨터</td>
      <td>2259000</td>
      <td>4</td>
      <td>2.50</td>
      <td>230.0</td>
      <td>0</td>
      <td>500</td>
      <td>16</td>
      <td>25.0</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>62.32</td>
      <td>1</td>
      <td>코어i9-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7277LCW</th>
      <td>한성컴퓨터</td>
      <td>2879000</td>
      <td>4</td>
      <td>2.50</td>
      <td>280.0</td>
      <td>0</td>
      <td>500</td>
      <td>16</td>
      <td>25.0</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>62.32</td>
      <td>1</td>
      <td>코어i9-12세대</td>
      <td>윈도우11홈</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3070 Ti</td>
    </tr>
    <tr>
      <th>한성컴퓨터 올데이롱 TFX5556UW</th>
      <td>한성컴퓨터</td>
      <td>1129000</td>
      <td>4</td>
      <td>1.40</td>
      <td>65.0</td>
      <td>1</td>
      <td>500</td>
      <td>16</td>
      <td>18.0</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>91.24</td>
      <td>1</td>
      <td>라이젠5-4세대</td>
      <td>윈도우11홈</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFX3412N</th>
      <td>한성컴퓨터</td>
      <td>498000</td>
      <td>2</td>
      <td>1.30</td>
      <td>24.0</td>
      <td>0</td>
      <td>250</td>
      <td>8</td>
      <td>16.0</td>
      <td>13.3</td>
      <td>1</td>
      <td>1</td>
      <td>47.12</td>
      <td>0</td>
      <td>셀러론</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4(온보드)</td>
      <td>M.2(SATA)</td>
      <td>UHD 600</td>
    </tr>
  </tbody>
</table>
<p>94 rows × 20 columns</p>
</div>



## 3.6. CPU별 배터리 성능의 가격 비교
: CPU별 배터리 성능의 값들을 가격이 비싼 것부터 나열해봤다.


```python
notebooks.groupby('CPU 종류',as_index=False).apply(lambda df: df.sort_values(
    '배터리(Wh)',ascending=False
    ).head(1)
).droplevel(0).sort_values('가격',ascending=False)

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
      <th>제조사</th>
      <th>가격</th>
      <th>USB(개)</th>
      <th>무게(kg)</th>
      <th>어댑터(W)</th>
      <th>HDMI</th>
      <th>저장 용량(GB)</th>
      <th>메모리(GB)</th>
      <th>두께(mm)</th>
      <th>화면 크기</th>
      <th>웹캠(HD)</th>
      <th>광시야각</th>
      <th>배터리(Wh)</th>
      <th>방향키(ㅡ형/ㅗ형)</th>
      <th>CPU 종류</th>
      <th>운영체제(OS)</th>
      <th>화면 비율</th>
      <th>메모리 타입</th>
      <th>저장장치 종류</th>
      <th>GPU 칩셋</th>
    </tr>
    <tr>
      <th>제품명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>레노버 씽크패드 P15 Gen2 20YQ007QKR 32GB램</th>
      <td>레노버</td>
      <td>6488990</td>
      <td>5</td>
      <td>2.87</td>
      <td>230.0</td>
      <td>0</td>
      <td>512</td>
      <td>32</td>
      <td>31.45</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>94.00</td>
      <td>1</td>
      <td>제온</td>
      <td>윈도우11프로</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX A5000</td>
    </tr>
    <tr>
      <th>DELL 프리시전 M7560 i9 11950 A3000 UHD 64GB램</th>
      <td>DELL</td>
      <td>6039870</td>
      <td>4</td>
      <td>2.45</td>
      <td>NaN</td>
      <td>0</td>
      <td>1024</td>
      <td>64</td>
      <td>27.36</td>
      <td>15.6</td>
      <td>0</td>
      <td>0</td>
      <td>95.00</td>
      <td>0</td>
      <td>코어i9-11세대</td>
      <td>윈도우10 프로</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX A3000</td>
    </tr>
    <tr>
      <th>MSI GE시리즈 레이더 GE76 12UHS-i9 4K 디럭스 에디션</th>
      <td>MSI</td>
      <td>5099000</td>
      <td>5</td>
      <td>2.90</td>
      <td>330.0</td>
      <td>1</td>
      <td>2048</td>
      <td>64</td>
      <td>25.90</td>
      <td>17.3</td>
      <td>0</td>
      <td>1</td>
      <td>99.90</td>
      <td>1</td>
      <td>코어i9-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3080 Ti</td>
    </tr>
    <tr>
      <th>MSI GE시리즈 레이더 GE76 12UHS 4K W11</th>
      <td>MSI</td>
      <td>4790000</td>
      <td>5</td>
      <td>2.90</td>
      <td>330.0</td>
      <td>1</td>
      <td>2048</td>
      <td>32</td>
      <td>25.90</td>
      <td>17.3</td>
      <td>0</td>
      <td>1</td>
      <td>99.90</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우11프로</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3080 Ti</td>
    </tr>
    <tr>
      <th>APPLE 2021 맥북프로16 MK193KH/A</th>
      <td>APPLE</td>
      <td>3287290</td>
      <td>3</td>
      <td>2.10</td>
      <td>140.0</td>
      <td>1</td>
      <td>1024</td>
      <td>16</td>
      <td>16.80</td>
      <td>16.2</td>
      <td>0</td>
      <td>0</td>
      <td>100.00</td>
      <td>1</td>
      <td>실리콘 M1 PRO</td>
      <td>macOS Monterey</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 PRO 16 core</td>
    </tr>
    <tr>
      <th>ASUS ROG STRIX G17 G713RS-LL012</th>
      <td>ASUS</td>
      <td>2780000</td>
      <td>4</td>
      <td>2.90</td>
      <td>280.0</td>
      <td>0</td>
      <td>1024</td>
      <td>16</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠9-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3080</td>
    </tr>
    <tr>
      <th>LG전자 2022 그램16(12세대) 16ZD90Q-EX56K</th>
      <td>LG전자</td>
      <td>2049000</td>
      <td>4</td>
      <td>1.28</td>
      <td>65.0</td>
      <td>1</td>
      <td>256</td>
      <td>16</td>
      <td>16.80</td>
      <td>16.0</td>
      <td>0</td>
      <td>1</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i5-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>LPDDR5(온보드)</td>
      <td>M.2(NVMe)</td>
      <td>RTX2050</td>
    </tr>
    <tr>
      <th>GIGABYTE AERO 15 OLED KD</th>
      <td>GIGABYTE</td>
      <td>1889000</td>
      <td>4</td>
      <td>2.20</td>
      <td>230.0</td>
      <td>0</td>
      <td>512</td>
      <td>16</td>
      <td>20.00</td>
      <td>15.6</td>
      <td>0</td>
      <td>0</td>
      <td>99.00</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7576XG</th>
      <td>한성컴퓨터</td>
      <td>1690000</td>
      <td>4</td>
      <td>2.20</td>
      <td>230.0</td>
      <td>0</td>
      <td>500</td>
      <td>16</td>
      <td>25.00</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>91.24</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG STRIX G17 G713RM-LL121</th>
      <td>ASUS</td>
      <td>1660000</td>
      <td>4</td>
      <td>2.90</td>
      <td>240.0</td>
      <td>0</td>
      <td>512</td>
      <td>8</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>APPLE 2020 맥북프로13 MYD82KH/A</th>
      <td>APPLE</td>
      <td>1595320</td>
      <td>2</td>
      <td>1.40</td>
      <td>61.0</td>
      <td>0</td>
      <td>256</td>
      <td>8</td>
      <td>15.60</td>
      <td>13.3</td>
      <td>1</td>
      <td>0</td>
      <td>58.20</td>
      <td>1</td>
      <td>실리콘 M1</td>
      <td>macOS Big Sur</td>
      <td>16:10</td>
      <td>NaN</td>
      <td>SSD</td>
      <td>M1 8 core</td>
    </tr>
    <tr>
      <th>ASUS 비보북 프로 16X OLED M7600QE-L2041</th>
      <td>ASUS</td>
      <td>1490000</td>
      <td>4</td>
      <td>1.95</td>
      <td>120.0</td>
      <td>1</td>
      <td>512</td>
      <td>16</td>
      <td>18.90</td>
      <td>16.0</td>
      <td>1</td>
      <td>0</td>
      <td>96.00</td>
      <td>1</td>
      <td>라이젠9-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3050 Ti</td>
    </tr>
    <tr>
      <th>LG전자 2021 그램17 17ZD90P-GX30K WIN10</th>
      <td>LG전자</td>
      <td>1489990</td>
      <td>4</td>
      <td>1.35</td>
      <td>65.0</td>
      <td>1</td>
      <td>256</td>
      <td>8</td>
      <td>17.80</td>
      <td>17.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i3-11세대</td>
      <td>윈도우10</td>
      <td>16:10</td>
      <td>LPDDR4x(온보드)</td>
      <td>M.2(NVMe)</td>
      <td>UHD Graphics</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5i 15IMH I7 Ultra PRO</th>
      <td>레노버</td>
      <td>1445000</td>
      <td>5</td>
      <td>2.30</td>
      <td>NaN</td>
      <td>0</td>
      <td>256</td>
      <td>8</td>
      <td>23.50</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i7-10세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>GTX1660 Ti</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 GF63 Thin 9SC-i7 파워팩 프로 WIN10</th>
      <td>MSI</td>
      <td>1268050</td>
      <td>0</td>
      <td>1.77</td>
      <td>120.0</td>
      <td>1</td>
      <td>256</td>
      <td>16</td>
      <td>21.70</td>
      <td>15.6</td>
      <td>0</td>
      <td>1</td>
      <td>51.00</td>
      <td>1</td>
      <td>코어i7-9세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>GTX1650</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming F15 FX506HM-HN130</th>
      <td>ASUS</td>
      <td>1169000</td>
      <td>4</td>
      <td>2.30</td>
      <td>200.0</td>
      <td>0</td>
      <td>512</td>
      <td>8</td>
      <td>24.50</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i5-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 올데이롱 TFX5556U</th>
      <td>한성컴퓨터</td>
      <td>999000</td>
      <td>4</td>
      <td>1.40</td>
      <td>65.0</td>
      <td>1</td>
      <td>500</td>
      <td>16</td>
      <td>18.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>91.24</td>
      <td>1</td>
      <td>라이젠5-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 GF63 Thin 10SC-i5 비전</th>
      <td>MSI</td>
      <td>858000</td>
      <td>4</td>
      <td>1.77</td>
      <td>120.0</td>
      <td>1</td>
      <td>256</td>
      <td>8</td>
      <td>21.70</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>51.00</td>
      <td>1</td>
      <td>코어i5-10세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>GTX1650</td>
    </tr>
    <tr>
      <th>LG전자 2020 울트라PC 15U40N-GR56K</th>
      <td>LG전자</td>
      <td>784990</td>
      <td>3</td>
      <td>1.80</td>
      <td>65.0</td>
      <td>1</td>
      <td>256</td>
      <td>8</td>
      <td>19.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>52.00</td>
      <td>1</td>
      <td>라이젠5-3세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
    <tr>
      <th>삼성전자 갤럭시북S NT767XCM-K38</th>
      <td>삼성전자</td>
      <td>759050</td>
      <td>2</td>
      <td>0.95</td>
      <td>25.0</td>
      <td>0</td>
      <td>256</td>
      <td>8</td>
      <td>11.80</td>
      <td>13.3</td>
      <td>0</td>
      <td>1</td>
      <td>42.00</td>
      <td>0</td>
      <td>코어i3-10세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>LPDDR4x(온보드)</td>
      <td>eUFS</td>
      <td>UHD Graphics</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFX5470UC</th>
      <td>한성컴퓨터</td>
      <td>729000</td>
      <td>4</td>
      <td>1.60</td>
      <td>65.0</td>
      <td>1</td>
      <td>500</td>
      <td>16</td>
      <td>20.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>49.00</td>
      <td>1</td>
      <td>라이젠7-3세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
    <tr>
      <th>LG전자 2022 울트라PC 13UD70Q-GX30K</th>
      <td>LG전자</td>
      <td>578430</td>
      <td>3</td>
      <td>0.98</td>
      <td>65.0</td>
      <td>1</td>
      <td>128</td>
      <td>8</td>
      <td>15.30</td>
      <td>13.3</td>
      <td>1</td>
      <td>1</td>
      <td>51.00</td>
      <td>1</td>
      <td>라이젠3-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
    <tr>
      <th>삼성전자 노트북 플러스2 NT550XDA-K24A</th>
      <td>삼성전자</td>
      <td>568990</td>
      <td>3</td>
      <td>1.81</td>
      <td>40.0</td>
      <td>1</td>
      <td>128</td>
      <td>4</td>
      <td>18.80</td>
      <td>15.6</td>
      <td>0</td>
      <td>1</td>
      <td>43.00</td>
      <td>1</td>
      <td>펜티엄 골드</td>
      <td>윈도우10 프로</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>UHD Graphics</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFX3412N</th>
      <td>한성컴퓨터</td>
      <td>498000</td>
      <td>2</td>
      <td>1.30</td>
      <td>24.0</td>
      <td>0</td>
      <td>250</td>
      <td>8</td>
      <td>16.00</td>
      <td>13.3</td>
      <td>1</td>
      <td>1</td>
      <td>47.12</td>
      <td>0</td>
      <td>셀러론</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4(온보드)</td>
      <td>M.2(SATA)</td>
      <td>UHD 600</td>
    </tr>
    <tr>
      <th>레노버 V15 ADA 82C700KPKR</th>
      <td>레노버</td>
      <td>379000</td>
      <td>3</td>
      <td>1.85</td>
      <td>NaN</td>
      <td>1</td>
      <td>256</td>
      <td>4</td>
      <td>19.90</td>
      <td>15.6</td>
      <td>0</td>
      <td>0</td>
      <td>35.00</td>
      <td>0</td>
      <td>라이젠3-2세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>Radeon Graphics</td>
    </tr>
  </tbody>
</table>
</div>



## 3.7. 가격, 무게, 배터리, 저장 용량, 메모리 들의 상관관계 비교
: 가격과 어떤 요소가 가장 연관이 있는지 중점으로 봤다.

GPU와 가격의 상관성을 비교하고 싶었지만, GPU가 문자열이라 비교하지 못하였다.


```python
fig, ax = plt.subplots(figsize=(8,8))
corr = notebooks[['가격','무게(kg)','배터리(Wh)','저장 용량(GB)','메모리(GB)']].corr()

mask= np.zeros_like(corr,dtype=bool)
mask[np.triu_indices_from(mask)] = True
sns.heatmap(
corr,
mask=mask,
fmt = '.2f',
annot=True, 
ax=ax,
cmap='RdBu',
vmin=-1,
vmax=1,
square=True
)
```




    <AxesSubplot:>




    
![png](output_37_1.png)
    


## 3.8. CPU 종류별 가격 시각화
: CPU 종류별 가격은 어떻게 분포되어있는지 보았다.

코어i7가 M1 PRO보다 더 비싸다는 것을 확인할 수 있었다.


```python
fig, ax = plt.subplots(figsize=(10,8))
plt.bar(notebooks['CPU 종류'],notebooks['가격'])
plt.xticks(rotation=90)
sns.barplot(notebooks['CPU 종류'],notebooks['가격'])

```

    /opt/homebrew/Caskroom/miniforge/base/lib/python3.9/site-packages/seaborn/_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(





    <AxesSubplot:xlabel='CPU 종류', ylabel='가격'>




    
![png](output_39_2.png)
    


## 3.9. GPU 칩셋별 가격 시각화
: GPU 칩셋별 가격은 어떻게 분포되어있는지 보았다.

CPU에서는 apple 제품이 다른 제품들에 비해 싼 가격이었지만, GPU에서는 비싼 가격대로 보인다.


```python
fig, ax = plt.subplots(figsize=(10,8))
#plt.bar(notebooks['GPU 칩셋'],notebooks['가격'])
sns.barplot(notebooks['GPU 칩셋'],notebooks['가격'])
plt.xticks(rotation=90)
```

    /opt/homebrew/Caskroom/miniforge/base/lib/python3.9/site-packages/seaborn/_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(





    (array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
            17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28]),
     [Text(0, 0, 'Iris Xe'),
      Text(1, 0, 'RTX2050'),
      Text(2, 0, 'MX450'),
      Text(3, 0, 'Radeon Graphics'),
      Text(4, 0, 'RTX3070'),
      Text(5, 0, 'RTX3060'),
      Text(6, 0, 'RTX3080'),
      Text(7, 0, 'Arc A350M'),
      Text(8, 0, 'Radeon 680M'),
      Text(9, 0, 'RTX3050 Ti'),
      Text(10, 0, 'M1 PRO 16 core'),
      Text(11, 0, 'M1 7 core'),
      Text(12, 0, 'RTX3050'),
      Text(13, 0, 'RTX3070 Ti'),
      Text(14, 0, 'UHD Graphics'),
      Text(15, 0, 'RTX3080 Ti'),
      Text(16, 0, 'GTX1650 Ti'),
      Text(17, 0, 'M1 PRO 14 core'),
      Text(18, 0, 'M1 8 core'),
      Text(19, 0, 'GTX1650'),
      Text(20, 0, '쿼드로 T500'),
      Text(21, 0, 'UHD 600'),
      Text(22, 0, 'MX570'),
      Text(23, 0, 'RTX A5000'),
      Text(24, 0, 'RTX A3000'),
      Text(25, 0, '라데온 RX 6600M'),
      Text(26, 0, 'MX350'),
      Text(27, 0, 'RTX2070 SUPER'),
      Text(28, 0, 'GTX1660 Ti')])




    
![png](output_41_2.png)
    


## 3.10. 360개의 인기 제품중 인기있는 CPU 종류 시각화
: 360개 인기 제품에서 어떤 CPU가 가장 인기 있는지 확인해 봤다.

인텔에 코어 i7 11세대가 가장 많은 인기를 보여줬다.


```python
#CPU 종류별 인기 제품 포함 개수
fig,ax = plt.subplots(figsize=(10,8))
sns.countplot(y='CPU 종류',data=notebooks,order= notebooks['CPU 종류'].value_counts().index)
```




    <AxesSubplot:xlabel='count', ylabel='CPU 종류'>




    
![png](output_43_1.png)
    


# 3.11. 소비자 선호 조건에 맞는 제품 정렬(boolean 과 mask 활용)

다나와 쇼핌몰에서는 제조사 / CPU 종류 / 화면 크기 / 메모리 / 저장 용량 / 운영체제 / 무게 를 선택하여 볼 수 있다.

하지만, GPU 종류 / 웹캠 유무 / HDMI 포트 유무 / 방향키 종류 / 두께 / 배터리 용량등의 세분화된 스펙은 선택할 수 없다.

boolean 개념과 mask 함수를 통해 선호하는 위 스펙을 선택하면 그에 맞는 스펙의 제품이 정렬될 수 있도록 하는 알고리즘을 만들어 보았다.

- 우선 category 로 변환할 수 있는 condition_category함수

- 연속형 데이터를 범위 지정으로 bool형태로 변환하는 condition_continuous 함수

위 두 함수를 우선적으로 생성하여 각 데이터에 맞는 함수를 적용한다.


```python
#GPU 칩셋ct / 웹캠(HD) ct/ HDMIct / 방향키() ct/ USB cont / 두께 cont/ 배터리cont

#category 데이터 boolean 진행
def condition_category(col_name):
    global notebooks
    #선택해야할 옵션을 보여주고 input값으로 순번을 선택하여 입력받게한다.
    print(pd.DataFrame(notebooks[col_name].value_counts().index,index = [i for i in range(1,len(list(notebooks[col_name].value_counts().index))+1)],columns = [col_name]))
    cond = int(input(col_name+'의 번호를 입력하세요. 상관이 없다면 0을 입력하세요 :'))
    print('*'*75)
    
    #notebooks[col_name]에 원한 옵션 문자열만 빼고 다 False로 변환, 상관없다고 0을 누를 시 모두 True인 Series인 객체를 통해 다 True로 변환할 수 있도록 함.
    condition = notebooks[col_name] == notebooks[col_name].value_counts().index[int(cond)-1] if cond >0 else pd.Series(index=notebooks.index, data=[True for i in range(360)])
    return condition


#연속형 데이터 boolean 진행
def condition_continuous(col_name):
    global notebooks
    #선택해야할 옵션을 보여주고 input값으로 순번을 선택하여 입력받게한다.
    print(pd.DataFrame([max(notebooks[col_name]),min(notebooks[col_name])],index = ['max','min'],columns = [col_name]))
    cond_h = float(input(col_name+'의 원하는 수치기준(~이상)을 입력하세요. 상관이 없다면 -1을 입력하세요 :'))
    cond_l = float(input(col_name+'의 원하는 수치기준(~이하)을 입력하세요. 상관이 없다면 -1을 입력하세요 :'))
    print('*'*75)
    
    #notebooks[col_name]에 원한 옵션 조건만 빼고 다 False로 변환, 상관없다고 0을 누를 시 모두 True인 Series인 객체를 통해 다 True로 변환할 수 있도록 함.
    if (cond_h == -1.0) & (cond_l == -1.0):
        condition = (min(notebooks[col_name]) <= notebooks[col_name]) & (notebooks[col_name] <= max(notebooks[col_name]))
    elif (cond_h != -1.0) & (cond_l == -1.0):
        condition = (cond_h <= notebooks[col_name]) & (notebooks[col_name] <= max(notebooks[col_name]))
    elif (cond_h == -1.0) & (cond_l != -1.0):
        condition = (min(notebooks[col_name]) <= notebooks[col_name]) & (notebooks[col_name] <= cond_l)
    else:
        condition = (cond_h <= notebooks[col_name]) & (notebooks[col_name] <= cond_l)

    return condition
```


```python
#bool 형태로 된 condition들을 다 가져와 & 로 묶어서 값들이 condition_final로 객체 할당
condition_final = condition_category('GPU 칩셋')&condition_category('웹캠(HD)')&condition_category('HDMI')&condition_category('방향키(ㅡ형/ㅗ형)')&condition_continuous('USB(개)')&condition_continuous('두께(mm)')&condition_continuous('배터리(Wh)')

#mask함수로 condition_final에서 True인 값들만 추출
notebooks.mask(~condition_final).dropna(how='all').sort_values('가격') #싼 제품부터 정렬되어 나오도록
```

                 GPU 칩셋
    1           Iris Xe
    2           RTX3060
    3   Radeon Graphics
    4        RTX3070 Ti
    5        RTX3050 Ti
    6           RTX3050
    7             MX450
    8           RTX3070
    9      UHD Graphics
    10          GTX1650
    11          RTX3080
    12       RTX3080 Ti
    13          RTX2050
    14        Arc A350M
    15          UHD 600
    16       GTX1650 Ti
    17   M1 PRO 16 core
    18        M1 7 core
    19   M1 PRO 14 core
    20        M1 8 core
    21        RTX A3000
    22    RTX2070 SUPER
    23            MX350
    24     라데온 RX 6600M
    25      Radeon 680M
    26        RTX A5000
    27            MX570
    28         쿼드로 T500
    29       GTX1660 Ti


    GPU 칩셋의 번호를 입력하세요. 상관이 없다면 0을 입력하세요 : 2


    ***************************************************************************
      웹캠(HD)
    1      1
    2      0


    웹캠(HD)의 번호를 입력하세요. 상관이 없다면 0을 입력하세요 : 0


    ***************************************************************************
      HDMI
    1    1
    2    0


    HDMI의 번호를 입력하세요. 상관이 없다면 0을 입력하세요 : 0


    ***************************************************************************
      방향키(ㅡ형/ㅗ형)
    1          1
    2          0


    방향키(ㅡ형/ㅗ형)의 번호를 입력하세요. 상관이 없다면 0을 입력하세요 : 0


    ***************************************************************************
         USB(개)
    max       6
    min       0


    USB(개)의 원하는 수치기준(~이상)을 입력하세요. 상관이 없다면 -1을 입력하세요 : 0
    USB(개)의 원하는 수치기준(~이하)을 입력하세요. 상관이 없다면 -1을 입력하세요 : 6


    ***************************************************************************
         두께(mm)
    max    32.4
    min    11.2


    두께(mm)의 원하는 수치기준(~이상)을 입력하세요. 상관이 없다면 -1을 입력하세요 : -1
    두께(mm)의 원하는 수치기준(~이하)을 입력하세요. 상관이 없다면 -1을 입력하세요 : -1


    ***************************************************************************
         배터리(Wh)
    max    100.0
    min     -1.0


    배터리(Wh)의 원하는 수치기준(~이상)을 입력하세요. 상관이 없다면 -1을 입력하세요 : -1
    배터리(Wh)의 원하는 수치기준(~이하)을 입력하세요. 상관이 없다면 -1을 입력하세요 : -1


    ***************************************************************************





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
      <th>제조사</th>
      <th>가격</th>
      <th>USB(개)</th>
      <th>무게(kg)</th>
      <th>어댑터(W)</th>
      <th>HDMI</th>
      <th>저장 용량(GB)</th>
      <th>메모리(GB)</th>
      <th>두께(mm)</th>
      <th>화면 크기</th>
      <th>웹캠(HD)</th>
      <th>광시야각</th>
      <th>배터리(Wh)</th>
      <th>방향키(ㅡ형/ㅗ형)</th>
      <th>CPU 종류</th>
      <th>운영체제(OS)</th>
      <th>화면 비율</th>
      <th>메모리 타입</th>
      <th>저장장치 종류</th>
      <th>GPU 칩셋</th>
    </tr>
    <tr>
      <th>제품명</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>레노버 게이밍 3 15ACH R5 3060 PRO</th>
      <td>레노버</td>
      <td>1131000.0</td>
      <td>3.0</td>
      <td>2.25</td>
      <td>NaN</td>
      <td>0</td>
      <td>256.0</td>
      <td>8.0</td>
      <td>24.20</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>60.00</td>
      <td>1</td>
      <td>라이젠5-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming F15 FX506HM-HN130</th>
      <td>ASUS</td>
      <td>1169000.0</td>
      <td>4.0</td>
      <td>2.30</td>
      <td>200.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>24.50</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i5-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming A15 FA507RM-R6725</th>
      <td>ASUS</td>
      <td>1179560.0</td>
      <td>4.0</td>
      <td>2.20</td>
      <td>240.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 게이밍 3 15ACH R7 3060 PRO</th>
      <td>레노버</td>
      <td>1294000.0</td>
      <td>3.0</td>
      <td>2.25</td>
      <td>NaN</td>
      <td>0</td>
      <td>256.0</td>
      <td>8.0</td>
      <td>24.20</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>60.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 화이트</th>
      <td>MSI</td>
      <td>1295000.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 블랙</th>
      <td>MSI</td>
      <td>1299000.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 블랙 WIN10 16GB램</th>
      <td>MSI</td>
      <td>1332840.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 블랙 16GB램</th>
      <td>MSI</td>
      <td>1345000.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>주연테크 리오나인 젠 L8CS36</th>
      <td>주연테크</td>
      <td>1349000.0</td>
      <td>4.0</td>
      <td>2.50</td>
      <td>230.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>32.40</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>48.96</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5 17ACH R7 3060</th>
      <td>레노버</td>
      <td>1357000.0</td>
      <td>6.0</td>
      <td>2.90</td>
      <td>NaN</td>
      <td>0</td>
      <td>256.0</td>
      <td>8.0</td>
      <td>26.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 화이트 WIN10 16GB램</th>
      <td>MSI</td>
      <td>1410930.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 화이트 WIN10 32GB램</th>
      <td>MSI</td>
      <td>1466390.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 블랙 WIN10</th>
      <td>MSI</td>
      <td>1468280.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 게이밍 3 15ACH R7 3060 PRO W10</th>
      <td>레노버</td>
      <td>1469000.0</td>
      <td>3.0</td>
      <td>2.25</td>
      <td>NaN</td>
      <td>0</td>
      <td>256.0</td>
      <td>8.0</td>
      <td>24.20</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>60.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 화이트 WIN10</th>
      <td>MSI</td>
      <td>1470820.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5 15ACH R7 3060 PRO</th>
      <td>레노버</td>
      <td>1499000.0</td>
      <td>6.0</td>
      <td>2.40</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>22.50</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5 15ACH R7 3060 PRO W10P</th>
      <td>레노버</td>
      <td>1530000.0</td>
      <td>6.0</td>
      <td>2.40</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>22.50</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>윈도우10 프로</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 블랙</th>
      <td>MSI</td>
      <td>1530000.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 화이트 32GB램</th>
      <td>MSI</td>
      <td>1533510.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5 Pro 16ACH R7 STORM</th>
      <td>레노버</td>
      <td>1539000.0</td>
      <td>6.0</td>
      <td>2.59</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>21.70</td>
      <td>16.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 화이트 WIN10 32GB램</th>
      <td>MSI</td>
      <td>1547250.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A11UE 블랙 WIN10 32GB램</th>
      <td>MSI</td>
      <td>1547260.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A11UE 블랙 WIN10 32GB램</th>
      <td>MSI</td>
      <td>1550000.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 게이밍 3 15ACH R7 3060 PRO W10 16GB램</th>
      <td>레노버</td>
      <td>1560000.0</td>
      <td>3.0</td>
      <td>2.25</td>
      <td>NaN</td>
      <td>0</td>
      <td>256.0</td>
      <td>16.0</td>
      <td>24.20</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>60.00</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG5576XG</th>
      <td>한성컴퓨터</td>
      <td>1599000.0</td>
      <td>4.0</td>
      <td>1.70</td>
      <td>230.0</td>
      <td>0</td>
      <td>500.0</td>
      <td>16.0</td>
      <td>23.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>62.32</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming F17 FX707ZM-HX037</th>
      <td>ASUS</td>
      <td>1630000.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>240.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>25.40</td>
      <td>17.3</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5i Pro 16ITH I7 STORM 3060</th>
      <td>레노버</td>
      <td>1640000.0</td>
      <td>6.0</td>
      <td>2.60</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>21.70</td>
      <td>16.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>에이서 니트로 5 AN515-58-71Z5</th>
      <td>에이서</td>
      <td>1648820.0</td>
      <td>4.0</td>
      <td>2.50</td>
      <td>230.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>26.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>HP 빅터스 16-d1142TX WIN11</th>
      <td>HP</td>
      <td>1650000.0</td>
      <td>4.0</td>
      <td>2.46</td>
      <td>200.0</td>
      <td>0</td>
      <td>256.0</td>
      <td>16.0</td>
      <td>23.50</td>
      <td>16.1</td>
      <td>1</td>
      <td>1</td>
      <td>70.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우11(설치)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS TUF Gaming A17 FA707RM-HX015</th>
      <td>ASUS</td>
      <td>1655920.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>240.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>24.50</td>
      <td>17.3</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG STRIX G17 G713RM-LL121</th>
      <td>ASUS</td>
      <td>1660000.0</td>
      <td>4.0</td>
      <td>2.90</td>
      <td>240.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>8.0</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG STRIX G15 G513RM-HQ077 16GB램</th>
      <td>ASUS</td>
      <td>1680000.0</td>
      <td>4.0</td>
      <td>2.30</td>
      <td>240.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>27.20</td>
      <td>15.6</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7576XG</th>
      <td>한성컴퓨터</td>
      <td>1690000.0</td>
      <td>4.0</td>
      <td>2.20</td>
      <td>230.0</td>
      <td>0</td>
      <td>500.0</td>
      <td>16.0</td>
      <td>25.00</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>91.24</td>
      <td>1</td>
      <td>라이젠7-4세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF66 A12UE 화이트</th>
      <td>MSI</td>
      <td>1698990.0</td>
      <td>4.0</td>
      <td>2.25</td>
      <td>240.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>24.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI Stealth 15M B12UE</th>
      <td>MSI</td>
      <td>1740000.0</td>
      <td>4.0</td>
      <td>1.80</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>17.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.80</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG STRIX G17 G713RM-KH053 WIN10 16GB램</th>
      <td>ASUS</td>
      <td>1780000.0</td>
      <td>4.0</td>
      <td>2.90</td>
      <td>240.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A12UE 32GB램</th>
      <td>MSI</td>
      <td>1820000.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>240.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS TUF Dash F15 FX517ZM-HQ076</th>
      <td>ASUS</td>
      <td>1840000.0</td>
      <td>4.0</td>
      <td>2.00</td>
      <td>200.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>20.70</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>76.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5i Pro 16ITH I7 STORM 3060 W11P</th>
      <td>레노버</td>
      <td>1869000.0</td>
      <td>6.0</td>
      <td>2.60</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>21.70</td>
      <td>16.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>윈도우11프로</td>
      <td>16:10</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG 제피러스 G15 GA503RM-HQ049</th>
      <td>ASUS</td>
      <td>1880000.0</td>
      <td>4.0</td>
      <td>1.90</td>
      <td>200.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>19.90</td>
      <td>15.6</td>
      <td>1</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>라이젠7-5세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>GIGABYTE AERO 15 OLED KD</th>
      <td>GIGABYTE</td>
      <td>1889000.0</td>
      <td>4.0</td>
      <td>2.20</td>
      <td>230.0</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>20.00</td>
      <td>15.6</td>
      <td>0</td>
      <td>0</td>
      <td>99.00</td>
      <td>1</td>
      <td>코어i7-11세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5i 15IAH7H I7 3060 WQHD PRO</th>
      <td>레노버</td>
      <td>1894000.0</td>
      <td>6.0</td>
      <td>2.40</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>19.90</td>
      <td>15.6</td>
      <td>0</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG5276XG</th>
      <td>한성컴퓨터</td>
      <td>1899000.0</td>
      <td>4.0</td>
      <td>1.70</td>
      <td>230.0</td>
      <td>0</td>
      <td>500.0</td>
      <td>16.0</td>
      <td>20.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>62.32</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A12UE WIN10 32GB램</th>
      <td>MSI</td>
      <td>2100000.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>240.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>32.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI GF시리즈 Sword GF76 A12UE WIN10 64GB램</th>
      <td>MSI</td>
      <td>2123990.0</td>
      <td>4.0</td>
      <td>2.60</td>
      <td>240.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>64.0</td>
      <td>25.20</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>53.50</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>레노버 LEGION 5i Pro 16IAH7H I7 STORM 3060 W11P</th>
      <td>레노버</td>
      <td>2169000.0</td>
      <td>6.0</td>
      <td>2.49</td>
      <td>NaN</td>
      <td>0</td>
      <td>512.0</td>
      <td>16.0</td>
      <td>19.90</td>
      <td>16.0</td>
      <td>1</td>
      <td>1</td>
      <td>80.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우11프로</td>
      <td>16:10</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG SCAR G733ZM-LL035</th>
      <td>ASUS</td>
      <td>2199000.0</td>
      <td>4.0</td>
      <td>2.90</td>
      <td>240.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>MSI Stealth 15M B12UE WIN11 64GB램</th>
      <td>MSI</td>
      <td>2250000.0</td>
      <td>4.0</td>
      <td>1.80</td>
      <td>180.0</td>
      <td>1</td>
      <td>512.0</td>
      <td>64.0</td>
      <td>17.00</td>
      <td>15.6</td>
      <td>1</td>
      <td>1</td>
      <td>53.80</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우11(설치)</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>한성컴퓨터 TFG7276LC</th>
      <td>한성컴퓨터</td>
      <td>2259000.0</td>
      <td>4.0</td>
      <td>2.50</td>
      <td>230.0</td>
      <td>0</td>
      <td>500.0</td>
      <td>16.0</td>
      <td>25.00</td>
      <td>17.3</td>
      <td>1</td>
      <td>1</td>
      <td>62.32</td>
      <td>1</td>
      <td>코어i9-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>GIGABYTE AERO 16 KE4 OLED</th>
      <td>GIGABYTE</td>
      <td>2549000.0</td>
      <td>3.0</td>
      <td>2.30</td>
      <td>230.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>22.40</td>
      <td>16.0</td>
      <td>0</td>
      <td>0</td>
      <td>99.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:10</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>ASUS ROG SCAR G733ZM-LL035 WIN10</th>
      <td>ASUS</td>
      <td>2975990.0</td>
      <td>4.0</td>
      <td>2.90</td>
      <td>240.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>28.30</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>90.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>Razer BLADE PRO 17 11Gen R3060 QHD</th>
      <td>Razer</td>
      <td>3179990.0</td>
      <td>5.0</td>
      <td>2.75</td>
      <td>230.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>19.90</td>
      <td>17.3</td>
      <td>0</td>
      <td>0</td>
      <td>70.50</td>
      <td>0</td>
      <td>코어i7-11세대</td>
      <td>윈도우10</td>
      <td>16:9</td>
      <td>DDR4</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>GIGABYTE AERO 17 KE5 Mini LED</th>
      <td>GIGABYTE</td>
      <td>3270000.0</td>
      <td>3.0</td>
      <td>2.60</td>
      <td>230.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>23.40</td>
      <td>17.3</td>
      <td>1</td>
      <td>0</td>
      <td>99.00</td>
      <td>1</td>
      <td>코어i7-12세대</td>
      <td>미포함(프리도스)</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>Razer BLADE 15 Advanced 12Gen R3060 QHD</th>
      <td>Razer</td>
      <td>3499000.0</td>
      <td>5.0</td>
      <td>2.01</td>
      <td>230.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>16.0</td>
      <td>16.99</td>
      <td>15.6</td>
      <td>0</td>
      <td>1</td>
      <td>80.00</td>
      <td>0</td>
      <td>코어i7-12세대</td>
      <td>윈도우11홈</td>
      <td>16:9</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
    <tr>
      <th>DELL XPS 17 9720-WP06KR</th>
      <td>DELL</td>
      <td>4350000.0</td>
      <td>4.0</td>
      <td>2.21</td>
      <td>130.0</td>
      <td>0</td>
      <td>1024.0</td>
      <td>32.0</td>
      <td>19.50</td>
      <td>17.0</td>
      <td>1</td>
      <td>1</td>
      <td>97.00</td>
      <td>0</td>
      <td>코어i9-12세대</td>
      <td>윈도우11프로</td>
      <td>16:10</td>
      <td>DDR5</td>
      <td>M.2(NVMe)</td>
      <td>RTX3060</td>
    </tr>
  </tbody>
</table>
</div>




```python
#위의 정렬 시각화

def add_value_label(y_list):
    for i in range(0,10):
        plt.text(i,y_list[i],y_list[i], ha="center")

fig, ax = plt.subplots(figsize=(10,8))
plt.bar(notebooks.mask(~condition_final).dropna(how='all').sort_values('가격')[:10].index,notebooks.mask(~condition_final).dropna(how='all').sort_values('가격')['가격'][:10])
add_value_label(notebooks.mask(~condition_final).dropna(how='all').sort_values('가격')['가격'][:10])
plt.xticks(rotation=90)

```




    ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
     [Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, ''),
      Text(0, 0, '')])




    
![png](output_48_1.png)
    

