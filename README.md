<h1>Regular_Expression</h1>
<br>
<h2>1. 정규 표현식(Regular Expression)의 정의</h2>
<p>: 정규식이라고도 불리는 '정규 표현식'은 복잡한 문자열을 처리할 때 사용하는 기법 중의 하나입니다. 이는 파이썬만의 고유 문법이 아니며 문자열을 처리하는 모든 곳에서 사용됩니다.</p>
<p>정규 표현식은 주로 문자열에서 원하는 패턴을 찾아내거나, 특정 규칙에 맞는 문자열을 생성하거나, 문자열을 변경하거나, 분할하는 등의 작업에 사용됩니다.</p>
<br>

<h2>2. 정규 표현식의 필요성(re_necessity.py)</h2>
<p>: 정규 표현식을 이용하면 불필요하고 복잡한 로직을 작성할 필요가 없어지며, 보다 직관적이고 간결하게 코드를 작성할 수 있게 됩니다. </p>

<br>

<p>1. 정규 표현식을 사용하지 않은 코드</p>

```python
text = "my email is jbljw02@naver.com"

# 공백을 기준으로 문자를 구분하여, @가 포함된 문자를 출력해주는 함수
def find_email(text):
    words = text.split()

    for word in words:
        if '@' in word:
            return word

# find_email 함수에서 출력된 문자를 email 변수에 저장
email = find_email(text)

# email 주소를 출력
if email:
    print("찾은 이메일 주소 : ", email)
else:
    print("이메일 주소 찾기 실패")
```
<br>
<p>2. 정규 표현식을 사용한 코드</p>

```python
import re

text = "my email is jbljw02@naver.com"

# 이메일 주소의 패턴을 정규 표현식으로 작성
pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'

# search() 메소드를 이용하여 text에서 이메일 주소를 찾음
match = re.search(pattern, text)

# email 주소를 출력
if match:
    email = match.group()
    print("찾은 이메일 주소 : ", email)
else:
    print("이메일 주소 찾기 실패")
```

<p>1번 예제에서는 find_email이라는 함수를 생성하고 그 내부에서 split()과 조건문을 이용한 로직을 작성했습니다. 그러나 2번 예제에서는 정규 표현식을 이용한 단 한 줄의 코드로 함수 전체를 대체했습니다. </p>
<p>물론 예제 코드는 로직이 복잡하지 않고 코드의 양이 많지 않기 때문에 큰 차이를 불러오진 않습니다. 그러나 코드의 양이 길어질수록 정규 표현식을 사용하는 코드와 그렇지 않은 코드의 차이는 매우 커질 것입니다.<p>

<br>
  
<h2>3. 파이썬에서 정규 표현식의 기본 사용법(re_howTo.py)</h2>
<p>정규 표현식을 사용하기 전에는 필수적으로 re 모듈을 import 해주어야 합니다.</p>

```python
import re
```
<br>

<h3> ** 메타 문자(Meta characters)</h3>

<b>1. '.'(Dot) : 임의의 한 문자와 매치됩니다.</b>

```python
pattern = 'gr.y'
text = 'gray'

match = re.search(pattern, text)

if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>'.'이 a와 매치되어 patern과 text의 값이 같아지기 때문에 "매칭 성공"이 출력됩니다.</p>

<br>

<b>2. '*' : 바로 앞 문자가 0번 이상 반복되면 매치됩니다.</b>

```python
pattern = 'xy*z'
texts = ['xz', 'xyz', 'xyyyyyyyyz']

for text in texts:
    match = re.search(pattern, text)
    if match:
        print("매칭 성공")
    else:
        print("매칭 실패")
```

<p>'*' 앞에는 'y'가 있습니다. 'xz'는 y가 0번, 'xyz'는 1번, 'xyyyyyyyyz'는 8번 반복되므로 "매칭 성공"이 3회 출력됩니다. </p>

<br>

<b>3. '+' : 바로 앞 문자가 1번 이상 반복되면 매치됩니다.</b>

```python
pattern = 'xy+z'
texts = ['xz', 'xyz', 'xyyyyyyyyz']

for text in texts:
    match = re.search(pattern, text)
    if match:
        print("매칭 성공")
    else:
        print("매칭 실패")
```

<p>'+' 앞에는 y가 있습니다. 'xz'는 y가 0번 반복되므로 "매칭 실패"가 출력되고, 'xyz', 'xyyyyyyyyz'는 각각 1, 8번 반복되므로 "매칭 성공"이 2회 출력됩니다.</p>

<br>

<b>4. '?' : 바로 앞 문자가 0번 혹은 1번 나타나면 매치됩니다.</b>

```python
pattern = 'xy?z'
texts = ['xyz', 'xz', 'xyyz']

for text in texts:
    match = re.search(pattern, text)
    if match:
        print("매칭 성공")
    else:
        print("매칭 실패")
```

<p>'?' 앞에는 y가 있습니다. 'xyz', 'xz'는 각각 y가 1, 0번 반복되므로 "매칭 성공"이 2회 출력됩니다. 그러나 'xyyz'는 y가 2번 반복되기 때문에 "매칭 실패"가 출력됩니다.</p>

<br>

<b>5. '[]' : 대괄호 안에 있는 문자 중 하나와 매치됩니다.</b>

```python
pattern = '[abcde]'
texts = ['apple', 'banana', 'fx']

for text in texts:
    match = re.search(pattern, text)
    if match:
        print("매칭 성공")
    else:
        print("매칭 실패")
```

<p>'apple'과 'banana'는 'abcde'를 이루고 있는 최소 하나의 문자와 매치되므로 "매칭 성공이" 2번 출력됩니다. 그러나 fx는 매치되는 문자가 없기 때문에 "매칭 실패"를 출력합니다.</p>

<br>

<b>6. '$' : 문자열의 끝 부분과 매칭됩니다.</b>

```python
pattern = 'Bomb$'
text = 'Water Bomb!'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>pattern = 'Bomb'라면 'Water Bomb!'에 'Bomb'라는 문자열이 있기 때문에 "매칭 성공"이 출력됩니다. 그러나 '$'는 문자열의 끝을 의미합니다. text의 끝이 'Bomb'로 끝나지는 않으므로 "매칭 실패"가 출력됩니다.</p>

<br>

<b>7. '^' : 문자열의 시작 부분과 매칭됩니다.</b>

```python
pattern = '^Bomb'
text = 'Water Bomb!'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>pattern = 'Bomb'라면 'Water Bomb!'에 'Bomb'라는 문자열이 있기 때문에 "매칭 성공"이 출력됩니다. 그러나 '^'는 문자열의 시작을 의미합니다. text의 시작이 'Bomb'는 아니므로 "매칭 실패"가 출력됩니다.</p>

<br>

<b>8. \ : 특수 문자를 이스케이프 하거나, 특수한 의미를 가진 문자와 매칭됩니다.</b>

```python
pattern = '\$10'
text = 'Price is $10'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>'\'가 없다면 text 문자열의 끝 부분이 '10'으로 끝나지 않기 때문에 "매칭 실패"가 출력됩니다. 그러나 '\'가 '$'을 단순 문자로 만들어주기 때문에 "매칭 성공"이 출력됩니다.</p>

<br>

<b>9. '|' : 둘 중 하나의 패턴에 매칭됩니다.</b>

```python
pattern = 'chrome|naver'
texts = ['chrome', 'naver', 'firefox']

for text in texts:
    match = re.search(pattern, text)
    if match:
        print("매칭 성공")
    else:
        print("매칭 실패")
```

<p>'chrome'과 'naver'은 '|'로 이루어진 pattern 문자열의 두 문자중 하나에 해당되므로 "매칭 성공"이 2회 출력됩니다. 그러나 'firefox'는 pattern 문자열에 존재하지 않기 때문에 "매칭 실패"가 출력됩니다.</p>

<br>

<b>10. '()' : 패턴을 그룹화합니다.</b>

```python
pattern = '(ab)+'
text = 'ababac'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>'()'을 이용하여 'ab'를 그룹화합니다. text 문자열에는 'ab'가 두 번 반복되기 때문에 "매칭 성공"이 출력됩니다.</p>

<br>

<b>11. '{}' : 패턴의 반복을 나타내는 용도로 사용됩니다.</b>

```python
pattern = 'a{2,4}'
text = 'aaa'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>'a{2,4}'는 a가 2회에서 4회까지 반복될 수 있다는 의미입니다. text 문자열에서는 'a'가 총 3회 반복되고 있으므로 "매칭 성공"을 출력합니다/</p>

<br>

<h3>** 정규 표현식의 메소드</h3>

<b>1. search : pattern과 text에서 첫번째로 매칭되는 부분을 찾습니다. 매칭에 성공할 경우 re.Match 객체를 반환하고, 실패할 경우 None을 반환합니다.</b>

```python
pattern = 'Bomb'
text = 'Water Bomb!'

match = re.search(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>text에 'Bomb'가 존재하기 때문에 "매칭 성공"이 출력됩니다.</p>

<br>

<b>2. match : pattern과 text가 일치하는지 문자열의 시작부터 검사합니다. 매칭에 성공할 경우 re.Match 객체를 반환하고, 실패할 경우 None을 반환합니다.</b>

```python
pattern = 'Water'
text = 'Water Bomb!'
match = re.match(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>text도 'Water' 문자로 시작하기 때문에 "매칭 성공"이 출력됩니다. match() 메소드는 문자열의 시작부터 매칭하기 때문에 만약 pattern='Bomb'라면 "매칭 실패"가 출력됩니다.</p>

<br>

<b>3. fullmatch : pattern과 text가 완전히 일치하는지 검사합니다. 매칭에 성공할 경우 re.Match 객체를 반환하고, 실패할 경우 None을 반환합니다.</b>

```python
pattern = 'Water Bomb!'
text = 'Water Bomb!'

match = re.fullmatch(pattern, text)
if match:
    print("매칭 성공")
else:
    print("매칭 실패")
```

<p>pattern과 text가 완전히 일치하기 때문에 "매칭 성공"이 출력됩니다. 문자열이 한 글자라도 바뀐다면 "매칭 실패"가 출력됩니다.</p>

<br>

<b>4. findall : pattern과 text에서 매칭되는 모든 부분을 리스트로 반환합니다. 매칭된 부분이 없을 경우 빈 리스트를 반환합니다.</b>

```python
pattern = 'ab'
text = 'abcdefg'

match_list = re.findall(pattern, text)
print(match_list)
```

<p>pattern과 text에서 매칭되는 부분은 'ab'입니다. 출력 결과는 ['ab']입니다.</p>

<br>

<b>5. finditer : pattern과 string에서 매칭되는 모든 부분을 찾아서 이터레이터로 반환합니다. 매칭된 부분은 re.Match 객체로 나타나며 매칭된 부분이 없을 경우 빈 이터레이터를 반환합니다.</b>

```python
pattern = 'ab'
text = 'abcabcabc'

matches = re.finditer(pattern, text)
for match in matches:
    print(match.span())
```

<p>단순 match를 출력하면 <re.Match object; span=(0, 2), match='ab'>와 같은 값이 출력됩니다. text에서 'ab'의 인덱스는 0,1 / 3,4 / 6,7이므로 이를 .span()을 이용하여 출력하면 (0, 2), (3, 5), (6, 8)이 차례로 출력됩니다. 단순 인덱스가 아닌 이터레이터를 반환하여 출력하기 때문에 이와 같은 출력값이 도출됩니다.</p>
    
<br>
   
<b>6. group : 그룹 인덱스를 지정하여 매칭된 부분을 반환합니다.</b>
    
```python
pattern = r'(\d+)/(\d+)/(\d+)'
text = '2000/09/17'

match = re.search(pattern, text)
if match:
    print(match.group())      
    print(match.group(1))     
    print(match.group(2, 3))  
```
    
<p>group()은 매개변수를 지정하지 않았기 때문에 '2000/09/17'을 반환합니다. 그러나 1 및 2,3으로 매개변수를 지정한 부분은 각각 인덱스에 일치하는 '2000'와 ('09', '17')을 반환합니다.</p>

<br>

<b>7. start : 매칭된 부분의 시작 인덱스를 반환합니다.</b>

```python
pattern = 'Bomb'
text = 'Water Bomb!'

match = re.search(pattern, text)
if match:
    print(match.start())
```

<p>text에서 'Bomb'가 매칭되는 부분의 시작 인덱스는 6번째 인덱스이기 때문에 6이 출력됩니다.</p>

<br>

<b>8. end : 매칭된 부분의 끝의 다음 인덱스를 반환합니다.</b>

```python
pattern = 'Water'
text = 'Water Bomb!'

match = re.search(pattern, text)
if match:
    print(match.end())
```

<p>pattern과 text는 'Water' 문자열이 매칭되며 pattern의 마지막 인덱스는 4입니다. 4의 다음 인덱스는 5이므로 출력값은 5입니다.</p>

<br>

<b>9. span : 매칭된 부분의 시작 및 끝의 다음 인덱스를 튜플로 반환해줍니다.</b>

```python
pattern = 'Water'
text = 'Water Bomb'

match = re.search(pattern, text)
if match:
    print(match.span())
```

<p>'Water'의 시작 인덱스는 0이고 마지막 인덱스는 4입니다. 0과 4 다음 인덱스를 반환하여 (0,5)의 튜플을 반환하여 출력합니다.</p>
