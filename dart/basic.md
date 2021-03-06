# 다트 기본

온라인 다트 편집기에서 실행가능하다.

[https://dartpad.dartlang.org/](https://dartpad.dartlang.org/)

### 함수편

> 기본 출력

```dart
void main() {
    var test = "test";
    print('test = $test');
}
```

> 기본값 설정

```dart
void printMsg(String msg, [String value = 'undefined']) {
  print('msg = $msg, value = $value');
}
void main() {
  printMsg('1234', '5678');
  printMsg('1234');
}

........................
msg = 1234, value = 5678
msg = 1234, value = undefined
```

> name, value 쌍으로 map 형태로 인수 넣기

```dart
void printMsg({String msg, String value}) {
  print('msg = $msg, value = $value');
}
void main() {
  printMsg(
    msg: "1234",
    value: "5678"
  );
}
..............................
msg = 1234, value = 5678
```

> 함수를 인자로 넣을수도 있다.

```dart
double add(double a, double b) => a + b;

//Function 정의자를 유의해서 보자
Function makeAdder(double value) {
  return (x) => add(value, x);
}

void main() {
  var adder1 = makeAdder(10.0);
  var adder2 = makeAdder(20.0);

  print('adder1 = ${adder1(5)} , adder2 = ${adder2(10)}');
}
...................................
adder1 = 15 , adder2 = 30
```

### 리스트

> 일반 리스트에 추가
>
> **유의할 점은 같은 타입만 추가 가능하다.**

```dart
void main() {
  List<int> list = [2, 1, 3];
  list.add(4);
  print(list);
  print(list[0]);
}
..............

[2, 1, 3, 4]
2
```

> for문 사용해서 출력

```dart
void main() {
  List<int> list = [2, 1, 3];
  printList(list);
}

void printList(List<int> list) {
  for(var i=0; i< list.length; i++) {
    print(i);
  }
}
```

> foreach 사용

```dart
void main() {
  List<int> list = [2, 1, 3];
  usingForEach(list);
}

void usingForEach(List<int> list) {
  list.forEach( (x) {
    print(x);
  });
}

//짧게 설정 가능
void usingForEachShort(List<int> list) {
  list.forEach( (x) => print(x));
}
```

#### 맵

```dart
void main() {
  Map<String, int> cats = {
    'ABC': 1,
    'DEF': 2
  };

  print(cats);
}
```

#### Class

> toString은 이미 Object에서 정의되어 있어서 오버라이딩 방식으로 다시 정의할 수 있다.

```dart
class Cat {
  String name;
  int age;

  @override
  String toString() {
    return '$name , $age';
  }
}

void main() {
  final cat = new Cat();
  cat.name = 'steve';
  cat.age = 10;

  print(cat.toString());
}
```

> 생성자를 바로 입력도 가능하다.

```dart
class Cat {
  String name;
  int age;

  //이 부분 살펴보자. 
  Cat(this.name, this.age);   

 // 또는 타입 지정도 가능하다. 
  Cat(String this._name, int this._age); 

  @override
  String toString() {
    return '$name , $age';
  }
}

void main() {
  final cat = new Cat('steve', 10);  

  print(cat.toString());
}
```

## Try / Catch

> 기본적으로 예외를 잡는 부분

```dart
void main() {

  try {
    String userInput = '3,14';

    double doubleNum = double.parse(userInput);

    print(doubleNum);  
  } catch (e) {
    print(e);
  }  
}

.................
FormatException: Invalid double
3,14
```

> 특정 예외를 잡고 싶다면

```dart
void main() {

  try {
    String userInput = '3,14';

    double doubleNum = double.parse(userInput);

    print(doubleNum);  
  } on FormatException catch (e) {
    print("FormatException inside => $e");   
  } catch (e) {
    print("Global Exception => $e");
  }  
}

..................
FormatException inside => FormatException: Invalid double
3,14
```

## Async Request

> 비동기 요청에 대해서 알아보자.

```dart
import "dart:html";

void handleSuccess(String response) {
  print('Request was ok');
  print(response);
}

void handleError(String error) {
  print('The request was not ok');
  print(error);
}

void main() {

  var result = HttpRequest.getString('https://rebounce.online/api/time');
  result.then(handleSuccess);
  result.catchError(handleError);

  print('Main function finshed');
}

.............
Main function finshed //비동기로 실행된 부분 체크하자...
Request was ok
{"time":"2018-05-01T05:06:35+02:00"}
```

> 체인 형태로도 가능하다.

```dart
var result = HttpRequest
    .getString('https://rebounce.online/api/time')
    .then((String response) {
      print(response);
    })
    .catchError( (dynamic error) {
      print(error);
    });
```

> 동기적으로 바꿀수도 있다.
>
> **`Future, Async, Await`** 를 유의해서 보자.

```dart
import "dart:async"; //이 부분 추가됨
import "dart:html";

//Future 확인
 Future main() async {

  try {
      final response = await HttpRequest.getString('https://rebounce.online/api/time');   
      print('Request was ok => $response');   
  } catch (e) {
    print('Request error => + $e');
  }

  print('Main function has ended');

}
..........................

Request was ok => {"time":"2018-05-01T05:13:52+02:00"}
Main function has ended
```



