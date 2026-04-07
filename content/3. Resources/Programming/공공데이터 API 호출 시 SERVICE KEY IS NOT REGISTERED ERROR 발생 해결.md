---
publish: true
created: 2025-05-21T03:23:04.362+09:00
modified: 2026-04-03T01:53:47.301+09:00
tags:
  - resource
---

Links:

공공데이터 포털의 API로 제공되는 정보들은 사용 승인 후 발급받은 KEY를 활용한다.
python으로 데이터를 확인하기 위해 requests 라이브러리를 이용하여 호출을 하였으나
아무리 API 키를 입력해도 SERVICE KEY IS NOT REGISTERED ERROR가 나왔다.

생각지도 못한 곳에서 시간을 많이 하였는데 인터넷 검을 통해서 해결하였다.
에러가 발생한 이유는 API 키에 포함된 url [percent-encoding](https://developer.mozilla.org/en-US/docs/Glossary/Percent-encoding)이었다.
이미 encoding된 API 키에 포함된 % 문자열을 다시 encoding해서 발생하는 문제이다.

```python
base_url = 'http://apis.data.go.kr/B552115/SmpWithForecastDemand/getSmpWithForecastDemand'

api_key = '~%2D%2D~'

api_key_unquote = unquote('~%2D%2D~')

param_data = {'serviceKey': api_key,
              'pageNo':'1',
              'numOfRows':'50',
              'dataType':'json',
              'date':'20250411'}
              
param_data_unquote = {'serviceKey': api_key_unquote,
                      'pageNo':'1',
                      'numOfRows':'50',
                      'dataType':'json',
                      'date':'20250411'}

request_data = requests.get(base_url, params=param_data, timeout=3)
request_data_unquote = requests.get(base_url, params=param_data_unquote, timeout=3)
```

결과

```python
print(request_data.url)

# http://apis.data.go.kr/B552115/SmpWithForecastDemand/getSmpWithForecastDemand?serviceKey=~%252D%252D~&pageNo=1&numOfRows=50&dataType=json&date=20250411

print(request_data_unquote.url)

# http://apis.data.go.kr/B552115/SmpWithForecastDemand/getSmpWithForecastDemand?serviceKey=~%2D%2D~&pageNo=1&numOfRows=50&dataType=json&date=20250411
```

실제로 텍스트로 바로 입력한 url과 params로 입력 url과 입력이 다른 것을 확인할 수 있다.
아래와 같이 percent-encoding을 decoding 해주는 unquote 함수를 사용해 API 키를 변환하면 해결할 수 있다.

```python
from urllib.parse import unquote
API_KEY = unquote(API_KEY)
```
