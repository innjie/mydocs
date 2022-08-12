## Singleton 개념

>Singleton이란?
인스턴스를 한개만 제공하는 클래스
만들어진 1개의 인스턴스에 글로벌하게 접근할 수 있는 방법이 필요함
-> 오직 1개만 존재해야하는 경우 (설정 화면, 메뉴 등)


#### 일반적으로 객체를 생성하는 방법

```java
public class App {
	public static void main(String[] args) {
    	Settings settings1 = new Settings();
        Settings settings2 = new Settings();
        
        //true
        System.out.println(settings1 != settings2);
   }
}
```
생성한 두개의 인스턴스는 각각 다르다.
따라서 싱글톤 패턴을 만들 때는 new를 사용하면 안된다.


#### 싱글톤 객체를 생성하는 방법
```java
public class Settings {
	private static Settings instance;
	private Settings() {}
    public static getInstance() {
    	//instance가 비어있는 경우에만 새로 생성하여 반환
    	if(instance == null) {
        	instance = new Settings();
        }
    	return instance;
    }
}

//main class
public class App {
	public static void main(String[] args) {
    	Settings settings1 = Settings.getInstance();
        
        //true
        System.out.println(settings1 != settings2);
   }
}

```

## 기존 방식의 단점
웹 애플리케이션을 만들 때 보통 멀티스레드를 사용하는데 이 방법은 안전하지 않다.

#### Case Timeline
1. instance가 `null`인 상태에서
2. `스레드1`이 `if`문을 통과하여 객체를 만들기 전
3. `스레드2`가 `if`문 안으로 들어온 경우
4. 두 스레드 모두 `new` 객체를 생성하게 된다.


### 해결 1. Synchronized - 메소드 동기화하기
가장 간단한 방법은 `synchronized`를 사용하여 메소드들을 동기화시키는 것이다.

```java
public class Settings {
	private static Settings instance;
	private Settings() {}
    public static synchronized getInstance() {
    	//instance가 비어있는 경우에만 새로 생성하여 반환
    	if(instance == null) {
        	instance = new Settings();
        }
    	return instance;
    }
}
```
한번에 한개의 스레드만 접근가능하게 하여 한개의 인스턴스만 보장할 수 있다.
`getInstance` 동안 동기화 처리하는 일 때문에 성능이 떨어질 수 있다.

### 해결 2. Eager Initialization - 객체 미리 생성하기

```java
public class Settings {
	//미리 초기화하기
	private static final Settings INSTANCE = new Settings();
	private Settings() {}
    public static getInstance() {
    	return instance;
    }
}
```

`INSTANCE`는 `Settings` 클래스가 로딩될 때 초기화된다. 미리 만들어 멀티스레드에서도 안전하게 리턴할 수 있다.
단점은 미리 만드는 과정 자체가 메모리 사용이 많고 오래걸리고, 애플리케이션에서 사용하지 않는다면 손해다.

### 해결 3. Double Checked Locking - 두번 체크하기
```java
public class Settings {
	private static volatile Settings instance;
	private Settings() {}
    public static synchronized getInstance() {
    	//instance가 비어있는 경우에만 새로 생성하여 반환
    	if(instance == null) {
        	synchronized (Settings.class) {
            	if(instance == null) {
                	instance = new Settings();
                }
            }
        }
    	return instance;
    }
}
```

`getInstance`메소드를 호출할 때 매번 호출하지 않는 장점이 있다. 이미 `instance`가 있다면 if문을 통과하기 때문에 해결 1에 비해 성능에 유리하다.
`instance`를 필요한 시점에 만들 수 있다.


[volatile 정리](https://velog.io/@sillutt/Java-Volatile)

### 해결 4. static inner class - 권장
`double checked locking` 방법은 Java 1.5 이상에서 동작한다. 따라서 1.4 버전보다 아래이거나, 조금 더 간단한 코드를 작성하는 방법이 있다. (권장!)
멀티스레드 환경에서도 안전하고, getInstance 호출 시 인스턴스를 생성할 수 있다.

```java
public class Settings {
	private Settings() {}
    //static 인스턴스 선언
    private static class SettingsHolder {
    	private static final Settings INSTANCE = new Settings();
    }
    
    public static synchronized getInstance() {
    	return SettingsHolder.INSTANCE;
    }
}
```

<hr/>

이상의 방법들의 사용이 잘못되면 Singleton의 조건이 깨진다.
Singleton 방식으로 만들려면 `new` 키워드를 사용할 수 없지만, Java의 문법으로 새 객체를 만들어 기존 객체와 달라 Singleton을 깨트릴 수 있는 방법이 존재한다.

### 방법 1. Reflection 사용하기
```java
//main class
public class App {
	public static void main(String[] args) {
    	Settings settings1 = Settings.getInstance();
        
        //Settings에 선언된 생성자를 가져와 private의 접근을 가능하게 변경
        Constructor<Settings> constructor = 
        	Settings.class.getDeclareConstructor();
        constructor.setAccessible(true);
        Settings settings2 = constructor.newInstance();
        
        //true
        System.out.println(settings1 != settings2);
   }
}

```

### 방법2. 직렬화 / 역직렬화 사용하기

Java에는 `file`형태로 디스크에 저장했다가(직렬화) 읽어들일 때 변환(역직렬화) 한다.

Settings에 `Serializable` 인터페이스를 구현한다.

```java
//App clas

//file (serializable)
try (ObjectOutput out = 
	new ObjectOutputStream(new FileOutputStream("settings.obj"))) {
	out.writeObject(settings);
}

// get : 역직렬화 시 생성자로 새 객체를 만들어 저장한다.
try(ObjectInput in = 
	new ObjectInputStream(new FileInputStream("settings.obj))) {
	in.readObject();
}
```

#### 역직렬화 대응 방안

```java
// Settings class
// 아래 메소드를 구현하면 역직렬화 시 이 메소드를 사용하게 된다. 

protected Object readResolve() {
	return getInstance();
}
```


<hr/>

## 코드 단순화

`enum`을 사용해서 reflection에도 안전한 코드를 구현할 수 있다.
`enum`은 private 생성자의 접근을 가능하게 변경하더라도 생성이 불가능해 유일한 인스턴스를 보장할 수 있다. 단, 클래스를 로딩하는 순간 enum 인스턴스가 생성된다.

```java
public enum Settings {
	INSTANCE;
}
```


<hr/>

## > Think
> Q. 자바에서 enum을 사용하지 않고 싱글톤 패턴을 구현하는 방법
A. Synchronized / 객체 미리 생성

> Q. private 생성자와 static 메소드를 사용하는 방법의 단점
A. reflection으로 파괴될 수 있고, 객체를 미리 생성함

> Q. enum을 사용해 싱글톤 패턴을 구현하는 방법의 장단점
A. 코드가 간결하고 refletion 파괴에도 방어할 수 있지만, 객체가 미리 생성되고 상속을 사용할 수 없음

> Q. static inner 클래스를 사용해 싱글톤 패턴을 구현하는 방법
A. SettingsHolder를 사용하여 상수화된 Instance를 생성하고, 호출 시 생성 후 반환한다.

<hr/>

## 사용예제

### Runtime
Java에서 제공하는 Runtime은 `new` 키워드 대신 `getRuntime()`으로만 생성 가능하다.

### Spring Bean
`ApplicationContext`을 통해서 만들어지는 Bean의 인스턴스가 항상 같다. 엄밀히 따지자면 Singleton은 아니지만, 적어도 이 객체가 `ApplicationContext` 내에서는 유일하게 관리된다.

### 다른 디자인 패턴
빌더, 퍼사드, 추상팩토리 등 다른 디자인 패턴의 일부로 사용할 수 있다.