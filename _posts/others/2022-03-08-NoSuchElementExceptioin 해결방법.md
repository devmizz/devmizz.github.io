---
title: "NoSuchElementException 해결방법"

categories:
  - Others

tags:
  - Python
  - Selenium

toc: true
toc_label: 목차
toc_sticky: true
toc_icon: list

date: 2022-03-08
last_modified_at: 2022-03-08
---

Selenium을 이용하여 크롤링을 하다보면 NoSuchElementException을 빈번하게 만나곤 한다.

그 때의 해결 방법 두 가지.

## 1. 페이지가 로딩 될 때까지 기다려주기

```html
<input type="text" id="txtUserID" name="userID" />
<input type="password" id="txtPwd" name="password" />
<button type="button" class="btn-login" id="btn-login">로그인</button>
```

이렇게 짜여져 있는 로그인 페이지가 있다고 하면, 보통은 아래와 같이 코드를 짤 수 있다.

```python
driver.get(url)

name = 'userName'
pw = 'userPw'

idBox = driver.find_element(By.CSS_SELECTOR, 'txtUserID')	# 6 line
pwBox = driver.find_element(By.CSS_SELECTOR, '#txtPwd')

idBox.send_keys(name)
pwBox.send_keys(pw)
pwBox.send_keys(Keys.RETURN)
```

이럴 때 NoSuchElementException이 발생한다면, 6번 라인에서 발생한다. (내 경우는 그랬다.)

url로 이동한 뒤 페이지가 아직 로딩되지 않았는데 find_element가 실행되면서 NoSuchElementException이 발생한 것이다. 그러므로 이런 경우엔 간단하게

```python
driver.get(url)
time.sleep(2)

name = 'userName'
pw = 'userPw'

idBox = driver.find_element(By.CSS_SELECTOR, 'txtUserID')	# 6 line
pwBox = driver.find_element(By.CSS_SELECTOR, '#txtPwd')

idBox.send_keys(name)
pwBox.send_keys(pw)
pwBox.send_keys(Keys.RETURN)
```

time.sleep()을 넣어서 로딩을 기다리는 방법이 있다.

## 2. iframe 태그 확인

위의 경우처럼 간단히 time.sleep()으로 해결되면 좋겠지만, 그렇지 않은 경우가 있다.

```html
<html>
  <head></head>
  <body>
    <iframe name="Main" id="Main">
      #document
      <html>
        <body>
          <div id="ho" class="ha">호하</div>
        </body>
      </html>
    </iframe>
  </body>
</html>
```

이런 구조를 가진 html이 있는 경우, '호하'라는 텍스트를 가져오기 위해 이런 식의 코드를 짤 것이다.

```python
ho = driver.find_element(By.CSS_SELECTOR, '#ho').text
```

그러나 이 역시 NoSuchElementException이 뜬다.

이 문제는 4번째 줄의 `<iframe>` 태그 때문인데,

html 안에 또 다른 html을 넣는 거라고 보면 된다.

이 경우엔 selenium에서도 frame을 전환시켜줘야 한다.

```python
from selenium.webdriver.support.ui import Select

...생략

driver.switch_to.frame(driver.find_element(By.CSS_SELECTOR, '#Main'))

ho = driver.find_element(By.CSS_SELECTOR, '#ho').text
```
