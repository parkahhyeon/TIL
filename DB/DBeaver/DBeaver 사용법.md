#MySQL에 데이터베이스 생성하기

* Charset: 
    * utf8mb4 - utf8은 이모지 문자가 입력되지 않는 charset 이다.
* Collation: 
    * utf8mb4_0900_ai_ci - MySQL 8.0버전의 기본 Collation이다. 따로 대소문자나 악센트를 구분할 일이 없으면 사용.
    * Unicode9.0 문자를 표현 (0900)
    * Accent Insensitive Mode (ai, 악센트 구분하지 않음)
    * Case Insensitive Mode (ci, 대소문자 구분하지 않음)



## Charset 과 Collation 기본 개념

### [Charset]

 문자 집합을 뜻하며, 각 문자 집합의 크기를 어떻게 설정할 것인지 정하는 것.

* MySQL의 utf-8의 경우, 3Byte 가변 자료형으로 설계되어 있다. 이는, 전세계 언어가 3Byte보다 크지 않아 설정한 자료형 크기이다.
다만 일반적인 언어가 아닐 때 4Byte 문자열(ex: Emoji)이 존재하여 utf-8에 저장했을 때 값이 손실되는 현상이 발생했다.
이로 인해, utf8mb4라는 4Byte 가변 자료형을 저장할 수 있는 자료형을 추가했다. 


### [Collation]

text data를 정렬(ORDER BY)할 때 사용한다.
 

* *utf8_bin & utf8mb4_bin*

바이너리 저장 값 그대로 정렬한다.

즉, A는 41이고, B는 42, a는 61이기 때문에 오름차순 정렬 시 A가 a보다 먼저 오고 B가 온 뒤 a가 오게 된다.


* *utf8_general_ci & utf8mb4_general_ci*

텍스트 정렬 시 대소문자가 구분되지 않는 것이라고 생각할 수 있다. 

즉, 악센트가 없어 A 순서에 a가 같이 오게 된다.


*utf8_unicode_ci & utf8mb4_unicode_ci*

general_ci 보다 좀 더 잘 정렬된 Collation이다. 

한글이나 영어, 중국어 등에서는 general_ci나 unicode_ci 결과가 동일하다.

다만, 악센트가 있는 문자와 같은 문자를 제대로 정렬할 필요성이 있을 때에는 unicode_ci를 사용해야 한다.



