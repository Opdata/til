# API Versioning 방법

만약 같은 엔드 포인트 주소이지만 버전에 따라 다르게 가져오고 싶다면?

예를 들어 다음과 같은 내용으로 만들었다.

```
>> Name.java

public class Name {
    private String firstName;
    private String lastName;    
}
```

```
>> Version1


public class PersonV1 {
    private String name;        
}
```

```
>> Version2


public class PersonV2 {
    private Name name;        
}
```

* V1 에서는 String 으로 가져오고 있으며 V2에서는 Name 클래스에서 가져오고 있다. 

```java
@RestController
public class PersonalVersioningController {

    @GetMapping("v1/person")
    public PersonV1 personV1() {
        return new PersonV1("Bob Charlie");
    }

    @GetMapping("v2/person")
    public PersonV2 personV2() {
        return new PersonV2(new Name("Bob", "Charlie"));
    }
}
```

버전에 따른 호출시

```java
>> v1
{
    "name": "Bob Charlie"
}

>> v2
{
    "name": {
        "firstName": "Bob",
        "lastName": "Charlie"
    }
}
```

---

만약 param 형태로 버전을 주고 싶을때는

```java
@GetMapping(value="/person/param", params="version=1")
public PersonV1 paramV1() {
	return new PersonV1("Bob Charlie");
}

@GetMapping(value="/person/param", params="version=2")
public PersonV2 paramV2() {
	return new PersonV2(new Name("Bob", "Charlie"));
}
```

으로 하고 호출시 다음과 같이 한다. 

```
http://localhost:8080/person/param?version=1

http://localhost:8080/person/param?version=2
```


