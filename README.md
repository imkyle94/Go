# Go

Go 예제(하이퍼레저 패브릭)

### Go / TypeScript / Solidity

공통점을 가지고 개발을 진행하는 것이 여러모로 도움이 될 듯 하다.
다양한 주제를 이야기할 수 있겠지만, 이 레포지토리에서는 객체지향 만을 생각에 두고 기술한다.

객체지향적 사고, 관점, 개발, 코드 등 예전부터 쭉 이어져오고 있는 개발 트렌드이고 다른 패러다임이 새롭게 제시되거나 하는 상황에서,
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
여러 인코딩이 있습니다 ex. UTF-8.

String은 유니코드 단위, 즉 2byte단위입니다.
그리고 byte는 1byte단위입니다.
만일 소켓통신시 서버와 클라이언트가 둘다 자바를 쓴다면 뭐 굳이 String, byte 안나눠도 되지만 자바와 c같이 String 하나를 나타내는데 자료의 크기가 다르다면 byte로 맞추어야 합니다.
다른분들이 더 자세하게 답변을 달아 주실지도 ^^

### 연산자

포인터연산자는 C++와 같이 & 혹은 \* 을 사용하여 해당 변수의 주소를 얻어내거나 이를 반대로 Dereference 할 때 사용한다. Go 는 비록 포인터연산자를 제공하지만 포인터 산술 즉 포인터에 더하고 빼는 기능은 제공하지 않는다.

```go
var k int = 10
var p = &k //k의 주소를 할당
println(\*p) //p가 가리키는 주소에 있는 실제 내용을 출력
```

### 반복문

```go
for, for range
names := []string{"홍길동", "이순신", "강감찬"}

for index, name := range names {
	println(index, name)
}
```
### 함수

#### Pass By Reference

1. Pass By Value
```go
package main

func main() {
	msg := "Hello"
	say(msg)
}

func say(msg string) {
	println(msg)
}
```

2. Pass By Reference(포인터)
3.
```go
package main

func main() {
	msg := "Hello"
	say(&msg)
	println(msg) //변경된 메시지 출력
}

func say(msg *string) {
	println(*msg)
	*msg = "Changed" //메시지 변경
}
```

#### Variadic Function (가변인자함수)
```go
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
```
#### 리턴값

Go는 복수의 리턴값을 상정할 수 있다.
```go
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
```
### 일급함수

Go 역시 TypeScript와 마찬가지로 함수는 일급 함수다.

### 클로저(Closure)

Closure는 함수 바깥에 있는 변수를 참조하는 함수값(function value)를 일컫는데, 이때의 함수는 바깥의 변수를 마치 함수 안으로 끌어들인 듯이 그 변수를 읽거나 쓸 수 있게 된다.

```go
package main

func nextValue() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}

func main() {
	next := nextValue()

	println(next()) // 1
	println(next()) // 2
	println(next()) // 3

	anotherNext := nextValue()
	println(anotherNext()) // 1 다시 시작
	println(anotherNext()) // 2
}
```
### Array

배열은 연속적인 메모리 공간에 동일한 타입의 데이타를 순서적으로 저장하는 자료구조이다.

```go
package main

func main() {
	var a [3]int //정수형 3개 요소를 갖는 배열 a 선언
	a[0] = 1
	a[1] = 2
	a[2] = 3
	println(a[1]) // 2 출력
}
```
### Go Slice

Go Slice 선언은 배열을 선언하듯이 "var v []T" 처럼 하는데 배열과 달리 크기는 지정하지 않는다. 예를 들어, 정수형 Slice 변수 a를 선언하기 위해서 "var a []int" 처럼 선언할 수 있다.

```go
package main

import "fmt"

func main() {
	var a []int        //슬라이스 변수 선언
	a = []int{1, 2, 3} //슬라이스에 리터럴값 지정
	a[1] = 10
	fmt.Println(a) // [1, 10, 3]출력
}
```
### Go Map

Map은 키(Key)에 대응하는 값(Value)을 신속히 찾는 해시테이블(Hash table)을 구현한 자료구조이다.

```go
package main

func main() {
	var m map[int]string

	m = make(map[int]string)
	//추가 혹은 갱신
	m[901] = "Apple"
	m[134] = "Grape"
	m[777] = "Tomato"

	// 키에 대한 값 읽기
	str := m[134]
	println(str)

	noData := m[999] // 값이 없으면 nil 혹은 zero 리턴
	println(noData)

	// 삭제
	delete(m, 777)
}
```

```go
package main

func main() {
	tickers := map[string]string{
		"GOOG": "Google Inc",
		"MSFT": "Microsoft",
		"FB":   "FaceBook",
		"AMZN": "Amazon",
	}

	// map 키 체크
	val, exists := tickers["MSFT"]
	if !exists {
		println("No MSFT ticker")
	}
}
```
### Go Package

Go는 패키지(Package)를 통해 코드의 모듈화, 코드의 재사용 기능을 제공한다.
다른 패키지를 프로그램에서 사용하기 위해서는 import 를 사용하여 패키지를 포함시킨다.

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello")
}
```
개발자가 패키지를 작성할 때, 패키지 실행시 처음으로 호출되는 init() 함수를 작성할 수 있다.
즉, init 함수는 패키지가 로드되면서 실행되는 함수로 별도의 호출 없이 자동으로 호출된다.

```go
package testlib

var pop map[string]string

func init() { // 패키지 로드시 map 초기화
	pop = make(map[string]string)
}
```
경우에 따라 패키지를 import 하면서 단지 그 패키지 안의 init() 함수만을 호출하고자 하는 케이스가 있다. 이런 경우는 패키지 import 시 \_ 라는 alias 를 지정한다.

```go
package main
import \_ "other/xlib"
```
### Go Struct

Go에서 struct는 Custom Data Type을 표현하는데 사용되는데, Go의 struct는 필드들의 집합체이며 필드들의 컨테이너이다. Go에서 struct는 필드 데이타만을 가지며, (행위를 표현하는) 메서드를 갖지 않는다.

Go 언어는 객체지향 프로그래밍(Object Oriented Programming, OOP)을 고유의 방식으로 지원한다. 즉, Go에는 전통적인 OOP 언어가 가지는 클래스, 객체, 상속 개념이 없다. 전통적인 OOP의 클래스(class)는 Go 언어에서 Custom 타입을 정의하는 struct로 표현되는데, 전통적인 OOP의 클래스가 필드와 메서드를 함께 갖는 것과 달리 Go 언어의 struct는 필드만을 가지며, 메서드는 별도로 분리하여 정의한다 (Go Method 에서 설명).

```go
package main

import "fmt"

// struct 정의
type person struct {
name string
age int
}

func main() {
// person 객체 생성
p := person{}

    // 필드값 설정
    p.name = "Lee"
    p.age = 10

    fmt.Println(p)

}

var p1 person
p1 = person{"Bob", 20}
p2 := person{name: "Sean", age: 50}
```

때로 구조체(struct)의 필드가 사용 전에 초기화되어야 하는 경우가 있다.
생성자(constructor) 함수를 이용한다.

```go
package main

type dict struct {
	data map[int]string
}

//생성자 함수 정의
func newDict() \*dict {
	d := dict{}
	d.data = map[int]string{}
	return &d //포인터 전달
}

func main() {
	dic := newDict() // 생성자 호출
	dic.data[1] = "A"
}
```

### Go Method

흔히 receiver로 불리우는 이 부분은 메서드가 속한 struct 타입과 struct 변수명을 지정하는데, struct 변수명은 함수 내에서 마치 입력 파라미터처럼 사용된다.

```go
package main

//Rect - struct 정의
type Rect struct {
	width, height int
}
	
//Rect의 area() 메소드
func (r Rect) area() int {
	return r.width \* r.height  
}
		
func main() {
	rect := Rect{10, 20}
	area := rect.area() //메서드 호출
	println(area)
}
```

#### Value vs 포인터 receiver

Value receiver는 struct의 데이타를 복사(copy)하여 전달하며, 포인터 receiver는 struct의 포인터만을 전달한다. Value receiver의 경우 만약 메서드내에서 그 struct의 필드값이 변경되더라도 호출자의 데이타는 변경되지 않는 반면, 포인터 receiver는 메서드 내의 필드값 변경이 그대로 호출자에서 반영된다.

```go
// 포인터 Receiver
func (r _Rect) area2() int {
	r.width++
	return r.width _ r.height
}
	
func main() {
	rect := Rect{10, 20}
	area := rect.area2() //메서드 호출
	println(rect.width, area) // 11 220 출력
}
```

### Go Interface

구조체(struct)가 필드들의 집합체라면, interface는 메서드들의 집합체이다.
interface는 타입(type)이 구현해야 하는 메서드 원형(prototype)들을 정의한다. 하나의 사용자 정의 타입이 interface를 구현하기 위해서는 단순히 그 인터페이스가 갖는 모든 메서드들을 구현하면 된다.

인터페이스는 struct와 마찬가지로 type 문을 사용하여 정의한다.

```go
type Shape interface {
	area() float64
	perimeter() float64
}

//Rect 정의
type Rect struct {
	width, height float64
}

//Circle 정의
type Circle struct {
	radius float64
}

//Rect 타입에 대한 Shape 인터페이스 구현
func (r Rect) area() float64 { return r.width _ r.height }
func (r Rect) perimeter() float64 {
	return 2 _ (r.width + r.height)
}

//Circle 타입에 대한 Shape 인터페이스 구현
func (c Circle) area() float64 {
	return math.Pi _ c.radius _ c.radius
}
func (c Circle) perimeter() float64 {
	return 2 _ math.Pi _ c.radius
}
```

Go 프로그래밍을 하다보면 흔히 빈 인터페이스(empty interface)를 자주 접하게 되는데, 흔히 인터페이스 타입(interface type)으로도 불리운다.
빈 interface는 interface{} 와 같이 표현한다.

Empty interface는 메서드를 전혀 갖지 않는 빈 인터페이스로서, Go의 모든 Type은 적어도 0개의 메서드를 구현하므로, 흔히 Go에서 모든 Type을 나타내기 위해 빈 인터페이스를 사용한다. 즉, 빈 인터페이스는 어떠한 타입도 담을 수 있는 컨테이너라고 볼 수 있으며, 여러 다른 언어에서 흔히 일컫는 Dynamic Type 이라고 볼 수 있다. (주: empty interface는 C#, Java 에서 object라 볼 수 있으며, C/C++ 에서는 void\* 와 같다고 볼 수 있다)

### Go Error

Go는 내장 타입으로 error 라는 interface 타입을 갖는다.

```go
type error interface {
	Error() string
}
```

### Go루틴

Go루틴(goroutine)은 Go 런타임이 관리하는 Lightweight 논리적 (혹은 가상적) 쓰레드(주1)이다. Go에서 "go" 키워드를 사용하여 함수를 호출하면, 런타임시 새로운 goroutine을 실행한다. goroutine은 비동기적으로(asynchronously) 함수루틴을 실행하므로, 여러 코드를 동시에(Concurrently) 실행하는데 사용된다.

아래 예제에서 main 함수를 보면, 먼저 say()라는 함수를 동기적으로 호출하고, 다음으로 동일한 say() 함수를 비동기적으로 3번 호출하고 있다. 첫번째 동기적 호출은 say() 함수가 완전히 끝났을 때 다음 문장으로 이동하고, 다음 3개의 go say() 비동기 호출은 별도의 Go루틴들에서 동작하면서, 메인루틴은 계속 다음 문장(여기서는 time.Sleep)을 실행한다. 여기서 goroutine들은 그 실행순서가 일정하지 않으므로 프로그램 실행시 마다 다른 출력 결과를 나타낼 수 있다.

```go
package main

import "time"

import (
	"fmt"
	"time"
	)
	
func say(s string) {
	for i := 0; i < 10; i++ {
		fmt.Println(s, "\*\*\*", i)
	}
}

func main() {
	// 함수를 동기적으로 실행
	say("Sync")

	// 함수를 비동기적으로 실행
	go say("Async1")
	go say("Async2")
	go say("Async3")

	// 3초 대기
	time.Sleep(time.Second * 3)
}
```

### 다중 CPU

Go는 디폴트로 1개의 CPU를 사용한다. 즉, 여러 개의 Go 루틴을 만들더라도, 1개의 CPU에서 작업을 시분할하여 처리한다 (Concurrent 처리). 만약 머신이 복수개의 CPU를 가진 경우, Go 프로그램을 다중 CPU에서 병렬처리 (Parallel 처리)하게 할 수 있는데, 병렬처리를 위해서는 아래와 같이 runtime.GOMAXPROCS(CPU수) 함수를 호출하여야 한다 (여기서 CPU 수는 Logical CPU 수를 가리킨다).

```go
package main

import (
"runtime"  
)

func main() {
	// 4개의 CPU 사용
	runtime.GOMAXPROCS(4)
    //...
}
```

### Go 채널

Go 채널은 그 채널을 통하여 데이타를 주고 받는 통로라 볼 수 있는데, 채널은 make() 함수를 통해 미리 생성되어야 하며, 채널 연산자 <- 을 통해 데이타를 보내고 받는다. 채널은 흔히 goroutine들 사이 데이타를 주고 받는데 사용되는데, 상대편이 준비될 때까지 채널에서 대기함으로써 별도의 lock을 걸지 않고 데이타를 동기화하는데 사용된다.

### Interface / Type

type 문은 구조체(struct), 인터페이스 등 Custom Type(혹은 User Defined Type)을 정의하기 위해 사용된다.
type 문은 또한 함수 원형을 정의하는데 사용될 수 있다.

```go
// 원형 정의
type calculator func(int, int) int

// calculator 원형 사용
func calc(f calculator, a int, b int) int {
	result := f(a, b)
	return result
}
```

이렇게 함수의 원형을 정의하고 함수를 타 메서드에 전달하고 리턴받는 기능을 타 언어에서 흔히 델리게이트(Delegate)라 부른다. Go는 이러한 Delegate 기능을 제공하고 있다.
