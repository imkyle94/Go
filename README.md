# Go

Go 예제(하이퍼레저 패브릭)

### Go / TypeScript / Solidity

공통점을 가지고 개발을 진행하는 것이 여러모로 도움이 될 듯 하다.
다양한 주제를 이야기할 수 있겠지만, 이 레포지토리에서는 객체지향 만을 생각에 두고 기술한다.

객체지향적 사고, 관점, 개발, 코드 등 예전부터 쭉 이어져오고 있는 개발 트렌드이고, 물론 다른 관점이 제시되고 핫하고 이런 상황도 분명히 있지만
가장 대중적인 객체지향에 대한 원론적인 궁금증은 한 번은 꼭 가져야하는 경우라고 생각한다.

객체지향적인 접근법을 가지게 되면 데이터베이스의 테이블, 프로세스, 프로토콜, 코드 등 모든 곳에서의 인터페이스 혹은 타입들.
이들의 존재이유에 대해서 알 수 있고, 확고한 개발 방향성을 하나 가지게 되는 것이다.

### Go Data Type

1. 부울린 타입
   bool
2. 문자열 타입
   string: string은 한번 생성되면 수정될 수 없는 Immutable 타입임
3. 정수형 타입
   int int8 int16 int32 int64
   uint uint8 uint16 uint32 uint64 uintptr
4. Float 및 복소수 타입
   float32 float64 complex64 complex128
5. 기타 타입
   byte: uint8과 동일하며 바이트 코드에 사용
   rune: int32과 동일하며 유니코드 코드포인트에 사용한다

중요한 건 문자열 / 바이트 배열의 차이
문자열을 바이트배열로 변경하기 위하여 []byte("ABC") 처럼 표현할 수 있다.
문자열은 컴퓨터에 직접 저장할 수 없으며 먼저 인코딩 해야합니다 (바이트 문자열로 변환).
컴퓨터가 저장할 수있는 유일한 것은 바이트입니다.
여러 인코딩이 있습니다 UTF-8.

String은 유니코드 단위, 즉 2byte단위입니다.
그리고 byte는 1byte단위입니다.
만일 소켓통신시 서버와 클라이언트가 둘다 자바를 쓴다면 뭐 굳이 String, byte 안나눠도 되지만 자바와 c같이 String 하나를 나타내는데 자료의 크기가 다르다면 byte로 맞추어야 합니다.
다른분들이 더 자세하게 답변을 달아 주실지도 ^^

### 연산자

포인터연산자는 C++와 같이 & 혹은 \* 을 사용하여 해당 변수의 주소를 얻어내거나 이를 반대로 Dereference 할 때 사용한다. Go 는 비록 포인터연산자를 제공하지만 포인터 산술 즉 포인터에 더하고 빼는 기능은 제공하지 않는다.

var k int = 10
var p = &k //k의 주소를 할당
println(\*p) //p가 가리키는 주소에 있는 실제 내용을 출력

### 반복문

for, for range
names := []string{"홍길동", "이순신", "강감찬"}

for index, name := range names {
println(index, name)
}

### 함수

#### Pass By Reference

1. Pass By Value
   package main
   func main() {
   msg := "Hello"
   say(msg)
   }

func say(msg string) {
println(msg)
}

2. Pass By Reference(포인터)
3. package main
   func main() {
   msg := "Hello"
   say(&msg)
   println(msg) //변경된 메시지 출력
   }

func say(msg *string) {
println(*msg)
\*msg = "Changed" //메시지 변경
}

#### Variadic Function (가변인자함수)

package main
func main() {  
 say("This", "is", "a", "book")
say("Hi")
}

func say(msg ...string) {
for \_, s := range msg {
println(s)
}
}

#### 리턴값

Go는 복수의 리턴값을 상정할 수 있다.

package main

func main() {
count, total := sum(1, 7, 3, 5, 9)
println(count, total)  
}

func sum(nums ...int) (int, int) {
s := 0 // 합계
count := 0 // 요소 갯수
for \_, n := range nums {
s += n
count++
}
return count, s
}

이렇게도 사용할 수 있음.
func sum(nums ...int) (count int, total int) {
for \_, n := range nums {
total += n
}
count = len(nums)
return
}

### Interface / Type
