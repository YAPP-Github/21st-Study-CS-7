# String, StringBuffer, StringBuiler
<img width="685" alt="image" src="https://user-images.githubusercontent.com/62461857/201796787-3fc3932d-daaf-4af4-a3b8-7edbc341bdf9.png">

## String
자바에서 문자열을 다룰 때 사용하는 String은 private final char[] 형태로 저장됩니다.
이는 불변이므로, 한 번 생성되면 변경될 수 없습니다. 정확히는, 문자열이 할당된 메모리 공간이 변하지 않습니다.
그래서 아래와 같은 코드를 실행할 시 문제가 생깁니다.
``` java
String joinWords(String[] words) {
	String sentence = "";
	for(String w:words) {
		sentence = sentence + w;
	}
}
```
배열 안에 있는 단어들을 합칠 때, +가 간단해보이지만, 뒷 단에서는 헤비한 작업이 이뤄지고 있습니다. 
문장에 단어 하나를 붙일 때 마다, 두 개의 문자열을 합한 만큼의 새로운 크기의 문자열을 생성하고, 두 개의 문자열에 있는 문자들을 앞에서부터 한 단어씩 복사해서 붙이는 것입니다.
각 단어의 길이를 x라고 할 때, x + 2x + 3x … + nx. 따라서 이 로직의 시간 복잡도는 O(xN^2)이 됩니다.

새로운 문자열 객체를 만들고 그 객체를 참조하게 하면서 기존 레퍼런스가 가리키고 있던 문자열이 다른 문자열로 대체되면, 
참조가 사라져 Unreachable 상태가 되어 가비지 컬렉션(Garbage Collection) 대상이 됩니다. 
이는 힙 메모리 공간을 낭비하여 메모리 부족 상황을 유발할 수도 있습니다.

다만, 아래의 StringBuffer , StringBuilder 를 생성할 경우 buffer 의 크기를 초기에 설정하지 않으면 자동으로 16 크기로 설정되기 때문에, 만약 저장되는 문자열이 이보다 길 때는 16보다 큰 값이 지정되게 됩니다.
이와 같은 초기 크기 설정 작업으로 인해 String 객체보다 생성속도가 떨어질 수 있습니다.
물론 아래의 두 방식이 mutable 속성 덕분에 메모리 측면에서는 String 에 비해 이점이 있을 수 있지만, 
많은 양의 문자열 수정이 필요한 경우가 아니라면 내부적으로 많은 버퍼 조절 연산을 피하기 위해서 String 을 사용하는 것이 나을 수도 있습니다. 

## StringBuilder
문자열의 추가, 수정, 삭제 등 이 빈번히 발생하는 경우에는 String 이 아닌 StringBuilder 나 StringBuffer 를 사용하여 메모리 낭비를 방지하는 것이 좋습니다. 
``` java
String joinWords(String[] words) {
	StringBuilder Sb = new StringBuilder();
	for(String w:words) {
		Sb.append(w);
	}
	return Sb.toString();
}
```
StringBuilder는 클래스 안에 char 배열 공간을 미리 만들어놓고, append()가 호출되면 추가되는 문자열을 해당 배열 공간에 그냥 바로 추가합니다.
따라서 toString 메소드를 호출하여 불변한 String을 생성하기 전까지는 메모리 공간 안에서 char를 변경하고 추가하고 삭제할 수 있습니다.
배열 공간이 부족해질 때만 공간을 늘려서 복사해주는 식이므로 속도나 공간 측면에서 String보다 효율적입니다. 
## StringBuffer
StringBuffer는 동기화를 보장해줍니다. 
StringBuilder보다 속도는 느리지만 멀티 스레드 환경이면 동기화를 보장하기 위해 StringBuffer가 필요합니다.
거의 1만번 이상부터 차이가 나는 수준이므로, 초기 단계에서는 그냥 StringBuffer로 통일하는 것도 괜찮다는 의견도 있습니다.

문자열을 추가하는 append 메서드 구현을 보면 동기화 여부를 확인할 수 있습니다.
![image](https://user-images.githubusercontent.com/62461857/201796443-5930391e-753c-43dd-af9c-44b9103ad208.png)
synchronized 키워드가 선언되었으므로 한 번에 하나의 쓰레드만 동기화된 메서드에 접근 할 수 있도록 허용하고, 다른 쓰레드는 해당 메서드의 접근 권한이 없게 됩니다. 
쓰레드가 해당 메서드를 호출하면 lock을 얻으며, 실행을 완료해서 반납을 해야지 다른 쓰레드가 다시 lock을 얻어서 접근 권한을 얻을 수 있습니다.
이는 멀티스레드 환경에서 StringBuilder를 안전하게 사용할 수 있게 하지만, 동기화 작업을 거쳐야하기 때문에 StringBuilder 에 비해 속도와 성능이 떨어집니다. 
String 은 final 로 선언된 클래스이므로 값이 바뀔 일이 없기 때문에, 멀티스레드 환경에서도 안전하게 공유될 수 있습니다. 스레드가 만약 String 객체의 값을 변경한다면, 동일한 문자열을 수정하는 것이 아닌 String pool 에 새로운 문자열이 생성하므로 Thread-safe 를 지키게 되는 방식입니다.
