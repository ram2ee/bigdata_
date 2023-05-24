# 정규표현식
###  ■ 정규표현식 이란?
정규표현식(Regular Expression)이란 복잡한 문자열을 다룰 때 특정 텍스트를 찾거나 교체하기 위한 문자열 검색도구이다.

###  ■ 정규표현식의 필요성
정규 표현식은
- 문자열 검색과 매칭 : 원하는 패턴의 문자열을 효과적으로 찾을 수 있다.
- 문자열 치환과 변경 : 특정 패턴을 찾아 다른 문자열로 대체 할 수 있다.
- 문자열 추출 : 문자열에서 원하는 부분을 추출할 수 있다.
- 유효성 검사 : 문자열이 형식과 규칙을 준수하는지 확인할 수 있다.

등의 다양한 작업에 활용할 수 있다.

###  ■ 정규표현식의 기본 사용법

파이썬에서 정규표현식을 사용하려면 <code>re모듈</code>을 임포트해야한다.
```python
import re
```
###  ■ re모듈에서 제공되는 메소드

<b> 1.search(pattern, string)</b> : 문자열에서 패턴과 첫 번째로 일치하는 부분을 찾아 ‘Match’로 반환한다. 실패할 경우 ‘None’을 반환
```python
pattern = re.compile(r‘hope')
text = "I hope to be like you"

result = pattern.search(text)
if result:
    print(result.group())     # hope
 ```

<b> 2. match(pattern, string)</b> : 문자열의 시작 부분에서 패턴과 일치하는지 확인하여 일치할 경우 ’Match’로 반환한다. 실패할 경우 ‘None’을 반환한다.
```python
pattern = re.compile(r'hope')
text = "I hope to be like you"
result = pattern.match(text)
if result:
    print(result.group())
else:
    print("No match")       # No match
```

<b> 3.findall(pattern, string)</b> : 문자열에서 패턴과 일치하는 부분을 모두 리스트로 반환한다.
```python
pattern = 'o'
text = 'I hope to be like you'

match_list = re.findall(pattern, text)
print(match_list)     # ['o', 'o', 'o']
```

<b> 4.split(pattern, string, maxsplit=0)</b> : 문자열을 패턴을 기준으로 분할하여 리스트로 반환한다. maxsplit 매개 변수를 사용하여 분할 횟수를 제한할 수 있다.
```python
pattern = re.compile(r', ')
text = "I, love, U"
results = pattern.split(text)
print(results)      # ['I', 'love', 'U']
```

<b> 5.compile(pattern)</b> : 패턴을 컴파일하여 ‘Pattern’객체를 반환한다. 패턴은 재사용할 수 있다.
```python
pattern = re.compile(r'o')
text = "I hope to be like you"
results = pattern.findall(text)
print(results)      # ['o', 'o', 'o']

# 같은 패턴으로 다시 사용
text = "I love you"
results = pattern.findall(text)
print(results)      # ['o', 'o']
```

<b> 6.sub(pattern, repl, string, count)</b> : 문자열에서 패턴과 일치하는 부분을 repl 텍스트로 교체한다. count 매개변수를 사용하여 교체 횟수를 제한할 수 있다.
```python
pattern = re.compile(r'hope')
text = "I hope to be like you"
result = pattern.sub('want', text)
print(result)      # I want to be like you
```

### ■ 메타문자(Meta Character)
정규표현식에서 메타문자는 특별한 의미를 갖는 문자를 뜻한다.
메타문자를 사용하여 패턴을 정의하거나 검색할 수 있다.

<b> 1. ‘.’ </b>: 임의의 한 문자와 매치된다. 단, 줄바꿈 문자는 제외
```python
pattern = 'h.ppy'
text = 'happy'

match = re.search(pattern, text)
print(match)

# <re.Match object; span=(0, 5), match='happy'> -> 매칭성공
```

<b> 2. ‘*’ </b>: 바로 앞에 있는 문자나 패턴이 0번 이상 반복되면 매치된다.
```python
pattern = 'ca*t'
text = 'ct'
match=re.search(pattern,text)
print(match)
text = 'cat'
match=re.search(pattern, text)
print(match)
text ='caat'
match=re.search(pattern,text)
print(match)

#<re.Match object; span=(0, 2), match='ct'>
#<re.Match object; span=(0, 3), match='cat'>
#<re.Match object; span=(0, 4), match='caat'>
```

<b> 3. ‘+’ </b>: 바로 앞에 있는 문자나 패턴이 1번 이상 반복되면 매치된다.
```python
pattern = 'ca+t'
text = 'ct'
match=re.search(pattern,text)
print(match)
text = 'cat'
match=re.search(pattern, text)
print(match)
text ='caat'
match=re.search(pattern,text)
print(match)

#None
#<re.Match object; span=(0, 3), match='cat'>
#<re.Match object; span=(0, 4), match='caat'>
```

<b> 4. ‘?’ </b>: 바로 앞에 있는 문자나 패턴이 0번 또는 1번 나타나면 매치된다.
```python
pattern = 'ca?t'
text = 'ct'
match=re.search(pattern,text)
print(match)
text = 'cat'
match=re.search(pattern, text)
print(match)
text ='caat'
match=re.search(pattern,text)
print(match)

#<re.Match object; span=(0, 2), match='ct'>
#<re.Match object; span=(0, 3), match='cat'>
#None
```

<b> 5. ‘|’ </b>: A|B 는 A또는 B에 매치된다.
```python
pattern = 'cat|dog'
text = 'lovely cat'
match=re.search(pattern,text)
print(match)
text = 'lovely dog'
match=re.search(pattern, text)
print(match)
text ='lovely mouse'
match=re.search(pattern,text)

#<re.Match object; span=(7, 10), match='cat'>
#<re.Match object; span=(7, 10), match='dog'>
#None
```

<b> 6. ‘^’ </b>: 문자열의 시작과 매치된다.
```python
pattern = ‘^cat’
text = ‘lovely cat’
match=re.search(pattern,text)
print(match)
#None
```

<b> 7.‘$’ </b>: 문자열의 끝과 매치된다.
```python
pattern = ‘cat$’
text = ‘lovely cat’
match=re.search(pattern,text)
print(match)

# <re.Match object; span=(7, 10), match='cat'>
```

<b> 8. ‘[]’ </b>: 대괄호 안에 있는 문자 집합 중 하나와 매치된다.
```python
pattern = ‘[abde]’
text = ‘cat’
match=re.search(pattern, text)
print(match)

#<re.Match object; span=(1, 2), match='a'>
```

<b> 9. ‘[^]’ </b>: 대괄호 안에 있는 문자를 제외한 문자와 매치된다.
```python
pattern = '[^abde]'
text = 'cat'
match=re.search(pattern, text)
print(match)

#<re.Match object; span=(0, 1), match='c'>
```

<b> 10. ‘()’ </b>: 그룹을 지정하고 그룹화된 문자열을 ‘group()’메서드로 추출한다.
```python
pattern = '(ca)'
text = 'catcatt'
match = re.search(pattern, text)
print (match)

#<re.Match object; span=(0, 2), match='ca'>
```

<b> 11. ‘{}’ </b>: 패턴의 반복을 나타낸다.
```python
pattern = 'a{2,4}'
text = 'caaat'

match = re.search(pattern, text)
print(match)

#<re.Match object; span=(1, 4), match='aaa'>
#a가 2~4회 반복 가능하다는 의미
```

### ■ 역슬래쉬
‘\’를 사용하여 이스케이프 문자 또는 특수한 문자의 의미를 해제할 수 있다.


### ■ 메타 시퀀스(Meta Sequence)
: 다양한 패턴을 표현하기 위해 사용되는 특수한 문자열

- \d: 숫자를 나타낸다. [0-9]와 동일한 의미

- \D: 숫자가 아닌 것을 나타낸다. [^0-9]와 동일한 의미

- \w: 알파벳 문자와 숫자, 언더스코어(_)를 나타낸다. [a-zA-Z0-9_]와 동일한 의미

- \W: 알파벳 문자와 숫자, 언더스코어(_)가 아닌 것을 나타낸다. [^a-zA-Z0-9_]와 동일한 의미

- \s: 공백 문자를 나타낸다. 공백, 탭, 개행문자 등을 포함

- \S: 공백 문자가 아닌 것을 나타낸다. [^ \t\n\r\f\v]와 동일한 의미

- \b : 단어 경계를 나타낸다.

- \B : 단어 경계가 아닌 부분을 나타낸다.

- \A : 문자열의 시작 부분을 나타낸다.

- \Z : 문자열의 끝 부분을 나타낸다.


