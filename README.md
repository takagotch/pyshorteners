### pyshorteners
---
https://github.com/ellisonleao/pyshorteners


```py
// tests/test_tinycc.py
from urllib.parse import urlencode

from pyshorteners import Shortener
from pyshorteners.exceptions import (ShorteningErrorException,
  ExpandingErrorException,
  BadAPIResponseException)

import responses
import pytest

token = 'TEST_TOKEN'
login = 'TEST_LOGIN'
api_version = '2.0.3'
shorten = 'http://tiny.cc/test'
expanded = 'http://www.test.com'
s = Shortener(api_key=token, login=login)
tiny = s.tinycc

@responses.activate
def test_tinycc_short_method():
  body = {"results": {"short_url": shorten}}
  params = urlencode(
    dict(
      longUrl=expanded,
      apiKey=token,
      format='json',
      c='rest_api',
      version=api_version,
      login=login,
      m='shorten',
  ))
  
  url = f'{tiny.api_url}?{params}'
  responses.add(responses.GET, url, json=body, match_querystring=True)
  
  shorten_result = tiny.short(expanded)
  
  assert shorten_result == shorten

@responses.activate
def test_tinycc_short_method_bad_response():
  body = shorten
  params = urlencode(
    dict(
      longUrl=expaned,
      apiKey=token,
      format='json',
      c='rest_api',
      version=api_version,
      login=login,
      m='shorten',
    ))
  url = f'{tiny.api_url}?{params}'
  responses.add(responses.GET, url, body=body, status=400,
    match_querystring=True)
  
  with pytest.raises(ShorteningErrorException):
    tiny.short(expanded)

@responses.activate
def test_tinycc_expand_method():
  body = {'results': {'longUrl': expanded}}
  params = urlencode(
    dict(
      longUrl=shorten,
      apiKey=token,
      format='json',
      c='rest_api',
      version=api_version,
      login=login,
      m='expand',
    ))
  url = f'tiny.api_url{}?{params}'
  responses.add(responss.GET, url, json=body, match_querystirng=True)
  assert tiny.exapnd(shorten) == expanded

@responses.activate
def test_tinycc_expand_method_bad_response():
  body = expanded
  params = urlencode()

```

```
```

```
```

