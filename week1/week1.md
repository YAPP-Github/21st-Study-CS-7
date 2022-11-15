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

</div>
</details>

<details>
<summary>String은 왜 불변으로 설계되었을까?</summary>
<div markdown="1">       

</div>
</details>

### JVM
<details>
<summary>스크립트언어랑 컴파일언어랑 자바언어의 차이점을 JVM 동작 기반으로 설명하시오.</summary>
<div markdown="1">       

</div>
</details>

<details>
<summary>JVM은 Stack 기반 VM인데, Stack 기반 VM의 장점이 뭘까요?</summary>
<div markdown="1">       
    
  - VM Stack 기반 방식과 레지스터 기반 방식이 있음
    
  - Stack 기반 방식 레지스터 기반 방식에 비해서 명령어 실행 속도가 느릴 수 있고 명령어 최적화할 수 없음
   
  - 피연산자의 메모리 주소를 알 필요가 없어서 명령어에 주소가 들어가지 않아 명령어 길이가 짧아짐
</div>
</details>


<details>
<summary> JRE가 클래스 로더를 통해 메모리를 로딩한 후 JVM을 실행, 또는 JVM 안에 클래스 로더로 로딩하는 걸까요? </summary>
<div markdown="1"> 
  - Bootstrap Loader는 JVM 내부에 있고 여러가지 Class Loader일 경우 JRE에 있을 수 있다고 추측

</div>
</details>

<details>
<summary>스크립트언어랑 컴파일언어랑 자바언어의 차이점을 JVM 동작 기반으로 설명하시오.</summary>
<div markdown="1">       

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
  
  - Eden → Survivor → Old → Permenant (Java 8 이후 없어짐)
    
  - GC 일어날 때 Stop the world 단계로 GC 일어나는 스레드 제외하고 중지됨
    
  - GC 튜닝으로 Stop the world가 일어나는 시간 줄여야한다
</div>
</details>

<br/>

## 👥 참여
- 발표 & 대답 : le2sky, jihyehann
- 질문 : jinsim, binimini	
