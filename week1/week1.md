# Week 1

```
📅 2022.11.15 09:30
```

## 📚 주제 
1. String, StringBuffer, StringBuilder
2. JVM

<br/>

## 📝 질문

### String, StringBuffer, StringBuilder


<details>
<summary>String Constants Pool이란? (String Interning)
</summary>
<div markdown="1">       
<br/>
    
- String 객체를 new 키워드를 통해 생성하지 않고, 리터럴로 선언하게 되면, Heap 내의 특별한 공간인 String Pool에 문자열이 저장된다.
- String Constants Pool은 일종의 HashMap으로, 리터럴 선언을 통해 문자열 객체를 생성할 때 다음의 과정을 거친다.
    1. string constant pool에 동일한 값이 있는지 확인한다.
    2. 동일한 값이 이미 존재하는 경우, 참조만 반환한다.
       없는 경우, 새로운 문자열 객체를 생성하고 string pool에 추가한 다음 참조를 반환한다.
- 문자열을 생성할 때 시간이 더 걸리지만 공간 절약을 할 수 있다. 
   
</div>
</details>

<details>
<summary>String은 왜 불변으로 설계되었을까?</summary>
<div markdown="1">       
<br/>
    
- String Pool을 사용할 수 있다.
- 해커가 값을 변경할 수 없으므로 보안이 강화된다. 데이터베이스의 중요한 정보(회원 비밀번호 등)를 저장할 경우 사용하기 좋다.
- 멀티스레드 환경에서 스레드 안전하고, 동기화할 필요가 없다.
    
</div>
</details>

<details>
<summary>String의 연결 연산은 왜 느린가? (어떻게 동작하는가) </summary>
<div markdown="1">       
<br/>

- String은 문자열을 연결할 때 새로운 문자열을 할당하기 때문에 느리다.
- 문자열 n개를 잇는 시간복잡도는 $O(n^2)$ 이다.

+) 참고
```java
public String concat(String str) {
    if (str.isEmpty()) {
        return this;
    }
    if (coder() == str.coder()) {
        byte[] val = this.value;
        byte[] oval = str.value;
        int len = val.length + oval.length;
        byte[] buf = Arrays.copyOf(val, len);
        System.arraycopy(oval, 0, buf, val.length, oval.length);
        return new String(buf, coder);
    }
    int len = length();
    int olen = str.length();
    byte[] buf = StringUTF16.newBytesFor(len + olen);
    getBytes(buf, 0, UTF16);
    str.getBytes(buf, len, UTF16);
    return new String(buf, UTF16);
}
```

<br/>
</div>
</details>

<details>
<summary> StringBuffer에는 synchronized 키워드가 붙어있는데, StringBuilder와 비교했을 때 동작의 차이점은?
</summary>
<div markdown="1">       
<br/>
    
- StringBuffer는 공유 자원에 lock을 거는 방식으로 멀티스레드 환경에서도 스레드 안전하게 동작한다.
    
</div>
</details>

### JVM

<details>
<summary>JVM은 Stack 기반 VM인데, Stack 기반 VM의 장점이 뭘까요?</summary>
<div markdown="1">       
<br/>
    
  - VM Stack 기반 방식과 레지스터 기반 방식이 있음
  - Stack 기반 방식 레지스터 기반 방식에 비해서 명령어 실행 속도가 느릴 수 있고 명령어 최적화할 수 없음
  - 피연산자의 메모리 주소를 알 필요가 없어서 명령어에 주소가 들어가지 않아 명령어 길이가 짧아짐
    
</div>
</details>


<details>
<summary> JRE가 클래스 로더를 통해 메모리를 로딩한 후 JVM을 실행, 또는 JVM 안에 클래스 로더로 로딩하는 걸까요? </summary>
<div markdown="1"> 
<br/>
    
- Bootstrap Loader는 JVM 내부에 있고 여러가지 Class Loader일 경우 JRE에 있을 수 있다고 추측
    
</div>
</details>

<details>
<summary>스크립트언어랑 컴파일언어랑 자바언어의 차이점을 JVM 동작 기반으로 설명하시오.</summary>
<div markdown="1">       
<br/>
    
  - 인터프리터 언어는 코드 한줄 한줄마다 해석해서 실행
  - 컴파일 언어는 시작부터 코드를 기계어로 변환해서
  - 처음에는 .class 파일로 컴파일된 이후로 인터프리터 방식으로 실행
    - 더 최적화하는 실행 방법에 대해서 설명해주세요?
    - 반복되는 코드가 일정 이상일 경우 JIT 컴파일러로 네이티브 코드로 변환해서 실행 가능
    
</div>
</details>

<details>
<summary>GC가 Heap에서 안쓰는 메모리를 없앨 때 주의사항이 무엇일까요?</summary>
<div markdown="1">       
<br/>
    
  - Eden → Survivor → Old → Permenant (Java 8 이후 없어짐)
  - GC 일어날 때 Stop the world 단계로 GC 일어나는 스레드 제외하고 중지됨
  - GC 튜닝으로 Stop the world가 일어나는 시간 줄여야한다
    
</div>
</details>

<br/>

## 👥 참여
- 발표 & 대답 : le2sky, jihyehann
- 질문 : jinsim, binimini	
