🔗 [[페이지 링크]](https://dart.dev/language/generics)

# **Generics 제네릭**

**If you look at the API documentation for the basic array type, [`List`](https://api.dart.dev/stable/dart-core/List-class.html), you'll see that the type is actually `List<E>`. The <...> notation marks List as a _generic_ (or _parameterized_) type—a type that has formal type parameters. [By convention](https://dart.dev/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters), most type variables have single-letter names, such as E, T, S, K, and V.**

기본 배열 타입인 List의 API 문서를 보면, 실제 타입이 List<E>임을 알 수 있다. <...> 표기법은 List를 제네릭(또는 매개변수화된) 타입으로 표시하는데, 이는 형식 매개변수를 가지는 타입을 의미한다. 관례적으로 대부분의 타입 변수는 E, T, S, K, V와 같이 한 글자 이름을 가진다.

-   **[Q] 매개변수화 된다는게?**
    
    “매개변수화된다”는 것은 어떤 값을 특정하지 않고 매개변수로 받아서, 그 값에 따라 동작이 달라지는 것을 의미한다. 제네릭(Generics)에서는 타입을 매개변수화하여, 코드가 다양한 데이터 타입을 처리할 수 있도록 한다. 즉, 코드 작성 시 구체적인 데이터 타입을 명시하지 않고, 대신 **타입 매개변수를 사용하여 여러 타입을 지원(대응)하는 코드를 작성하는 것**을 말한다.
    
    예를 들어, List 클래스는 제네릭 클래스이다. 이 클래스는 요소의 타입을 미리 정하지 않고, 나중에 사용할 때 타입을 지정한다. 이렇게 하면 같은 List 클래스를 사용해 여러 타입의 리스트를 만들 수 있다.
    
    따라서 “매개변수화된다”는 것은 코드 작성 시 구체적인 값을 지정하지 않고 나중에 사용할 때 값을 지정하는 것으로, 코드가 다양한 타입이나 값을 처리할 수 있도록 일반화되는 것을 의미한다.
    
-   **[Q] 형식 매개변수란?**
    
    형식 매개변수는 제네릭(또는 매개변수화된) 타입에서 사용되는 매개변수로, 데이터 타입을 일반화하는 데 사용된다. 이를 통해 클래스나 메서드가 다양한 데이터 타입을 처리할 수 있게 된다.
    
    **예시 설명**
    
    예를 들어, Dart의 List는 제네릭 타입이다. 이를 통해 List가 다양한 타입의 요소를 포함할 수 있다. 다음 예시는 형식 매개변수를 사용하여 List를 정의하는 방법을 보여준다:
    
    ```dart
    // 문자열을 포함하는 리스트
    List<String> stringList = ['Alice', 'Bob'];
    
    // 정수를 포함하는 리스트
    List<int> intList = [1, 2, 3];
    
    ```
    
    위의 코드에서 List<String>의 String과 List<int>의 int가 형식 매개변수이다. List 클래스는 이러한 형식 매개변수를 사용하여 어떤 타입의 요소든 저장할 수 있게 된다.
    
    **형식 매개변수의 장점**
    
    -   타입 안전성: 컴파일러가 잘못된 타입의 데이터가 사용되는 것을 방지한다.
    -   코드 재사용성: 동일한 코드가 여러 타입의 데이터를 처리할 수 있게 한다.
    -   가독성 향상: 코드의 의도를 명확히 하고, 유지보수를 용이하게 한다.

## **Why use generics?[#](https://dart.dev/language/generics#why-use-generics) 왜 제네릭을 사용할까?**

**Generics are often required for type safety, but they have more benefits than just allowing your code to run:**

제네릭은 종종 타입 안전성을 위해 필요하지만, 단순히 코드 실행을 가능하게 하는 것 이상의 이점을 가지고 있다:

-   **Properly specifying generic types results in better generated code.**
    
    제네릭 타입을 제대로 지정하면 더 나은 생성된 코드가 나온다.
    
-   **You can use generics to reduce code duplication.**
    
    제네릭을 사용하여 코드 중복을 줄일 수 있다.
    

**If you intend for a list to contain only strings, you can declare it as `List<String>` (read that as "list of string"). That way you, your fellow programmers, and your tools can detect that assigning a non-string to the list is probably a mistake. Here's an example:**

리스트에 문자열만 포함되도록 하려면 List<String>으로 선언할 수 있다(이를 “문자열 리스트”로 읽습니다). 이렇게 하면 당신과 동료 프로그래머, 그리고 도구들이 리스트에 비문자열을 할당하는 것이 아마도 실수라는 것을 감지할 수 있다. 다음은 그 예다:

```dart
// ✗ static analysis: failure
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error

```

**Another reason for using generics is to reduce code duplication. Generics let you share a single interface and implementation between many types, while still taking advantage of static analysis. For example, say you create an interface for caching an object:**

제네릭을 사용하는 또 다른 이유는 코드 중복을 줄이기 위해서이다. 제네릭을 사용하면 여러 타입 간에 단일 인터페이스와 구현을 공유할 수 있으며, 여전히 정적 분석의 이점을 누릴 수 있다. 예를 들어, 객체를 캐시하기 위한 인터페이스를 만든다고 가정해 보자.

```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}

```

**You discover that you want a string-specific version of this interface, so you create another interface:**

이 인터페이스의 문자열 전용 버전이 필요하다는 것을 발견하고, 또 다른 인터페이스를 만든다.

```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}

```

**Later, you decide you want a number-specific version of this interface... You get the idea.**

나중에 이 인터페이스의 숫자 전용 버전이 필요하다고 결정하게 된다… 이제 아이디어를 이해했을 것이다.

**Generic types can save you the trouble of creating all these interfaces. Instead, you can create a single interface that takes a type parameter:**

제네릭 타입은 이러한 인터페이스를 모두 만드는 번거로움을 덜어줄 수 있다. 대신, 타입 매개변수를 받는 단일 인터페이스를 만들 수 있다.

```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}

```

-   **[note] Cache<T> 사용해보기**
    
    ```dart
    abstract class Cache<T> {
      T getByKey(String key);
    
      void setByKey(String key, T value);
    }
    
    class StringCacheImpl implements Cache<String> {
      @override
      String getByKey(String key) {
        // TODO: implement getByKey
        throw UnimplementedError('StringCacheImpl');
      }
    
      @override
      void setByKey(String key, String value) {
        // TODO: implement setByKey
      }
    
    }
    
    class IntCacheImpl implements Cache<int> {
      @override
      int getByKey(String key) {
        // TODO: implement getByKey
        throw UnimplementedError('IntCacheImpl');
      }
    
      @override
      void setByKey(String key, int value) {
        // TODO: implement setByKey
      }
    
    }
    
    void main() {
      Cache<String> stringCache = StringCacheImpl();
      Cache<int> intCache = IntCacheImpl();
    }
    
    
    ```
    

**In this code, T is the stand-in type. It's a placeholder that you can think of as a type that a developer will define later.**

이 코드에서 T는 대체 타입이다. 이는 개발자가 나중에 정의할 타입을 나타내는 플레이스홀더라고 생각할 수 있다.

## **Using collection literals**[#](https://dart.dev/language/generics#using-collection-literals) **컬렉션 리터럴 사용하기**

-   **[Q] literals 리터럴이란?**
    
    “리터럴(literals)“은 프로그래밍에서 고정된 값을 의미한다. 코드 내에서 직접 사용되는 값으로, 변수를 통해 저장되거나 계산된 값이 아닌, 코드에 명시적으로 작성된 값을 말한다. 예를 들어, 숫자, 문자열, 불리언 값 등이 리터럴에 해당한다.
    
    -   **숫자 리터럴**: 42, 3.14
    -   **문자열 리터럴**: "hello", 'world'
    -   **불리언 리터럴**: true, false
    -   **리스트 리터럴**: [1, 2, 3]
    -   **맵 리터럴**: {'key': 'value'}
    
    **예시 코드**
    
    ```dart
    void main() {
      // 숫자 리터럴
      int number = 42;
      
      // 문자열 리터럴
      String greeting = 'hello';
      
      // 불리언 리터럴
      bool isTrue = true;
      
      // 리스트 리터럴
      List<int> numbers = [1, 2, 3];
      
      // 맵 리터럴
      Map<String, String> user = {
        'name': 'John',
        'email': 'john@example.com'
      };
    
      print(number);        // 42
      print(greeting);      // hello
      print(isTrue);        // true
      print(numbers);       // [1, 2, 3]
      print(user);          // {name: John, email: john@example.com}
    }
    
    ```
    
    위 예제에서 42, 'hello', true, [1, 2, 3], {'name': 'John', 'email': 'john@example.com'} 등이 모두 리터럴이다. 이 값들은 코드에 직접 작성된 고정된 값들이다.
    
-   **[Q] Collection literals 컬렉션 리터럴이란?**
    
    컬렉션 리터럴(Collection Literals)은 리스트(List), 세트(Set), 맵(Map)과 같은 컬렉션을 생성할 때 사용되는 리터럴 표현을 의미한다.
    
    **리스트 리터럴**
    
    리스트는 중괄호([])를 사용하여 정의된다.
    
    ```dart
    void main() {
      // 정수형 리스트
      List<int> intList = [1, 2, 3, 4, 5];
      
      // 문자열 리스트
      List<String> stringList = ['apple', 'banana', 'cherry'];
      
      print(intList);       // [1, 2, 3, 4, 5]
      print(stringList);    // [apple, banana, cherry]
    }
    
    ```
    
    **셋 리터럴**
    
    세트는 중괄호({})를 사용하여 정의된다. 세트는 중복을 허용하지 않는 컬렉션이다.
    
    ```dart
    void main() {
      // 정수형 셋
      Set<int> intSet = {1, 2, 3, 4, 5};
      
      // 문자열 셋
      Set<String> stringSet = {'apple', 'banana', 'cherry'};
      
      print(intSet);       // {1, 2, 3, 4, 5}
      print(stringSet);    // {apple, banana, cherry}
    }
    
    ```
    
    **맵 리터럴**
    
    맵은 중괄호({})를 사용하여 정의되며, 키-값 쌍으로 구성된다.
    
    ```dart
    void main() {
      // 키가 문자열이고 값이 정수인 맵
      Map<String, int> fruitPrices = {
        'apple': 1000,
        'banana': 800,
        'cherry': 1500
      };
      
      print(fruitPrices);    // 출력: {apple: 1000, banana: 800, cherry: 1500}
    }
    
    ```
    
    **매개변수화된 컬렉션 리터럴**
    
    Dart에서는 컬렉션 리터럴을 매개변수화하여 특정 타입의 요소만 포함하는 컬렉션을 만들 수 있다. 타입 매개변수를 사용하여 컬렉션의 요소 타입을 지정할 수 있다.
    

**List, set, and map literals can be parameterized. Parameterized literals are just like the literals you've already seen, except that you add `<*type*>` (for lists and sets) or `<*keyType*, *valueType*>` (for maps) before the opening bracket. Here is an example of using typed literals:**

리스트, 세트, 맵 리터럴은 매개변수를 사용할 수 있다. 매개변수화된 리터럴은 이미 보아온 리터럴과 같지만, 여는 대괄호 앞에 <type> (리스트와 세트의 경우) 또는 <keyType, valueType> (맵의 경우)을 추가한다. 다음은 타입이 지정된 리터럴을 사용하는 예제이다:

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};

```

## **Using parameterized types with constructors[#](https://dart.dev/language/generics#using-parameterized-types-with-constructors) 생성자에서 매개변수화된 타입 사용하기**

**To specify one or more types when using a constructor, put the types in angle brackets (`<...>`) just after the class name. For example:**

생성자를 사용할 때 하나 이상의 타입을 지정하려면, 클래스 이름 바로 뒤에 꺾쇠 괄호(<…>) 안에 타입을 넣는다. 예를 들어:

```dart
var nameSet = Set<String>.from(names);

```

**다음 코드는 정수 키와 View 타입의 값을 가지는 맵을 생성한다:**

```dart
var views = Map<int, View>();

```

## **Generic collections and the types they contain[#](https://dart.dev/language/generics#generic-collections-and-the-types-they-contain) 제네릭 컬렉션과 그들이 포함하는 타입들**

**Dart generic types are _reified_, which means that they carry their type information around at runtime. For example, you can test the type of a collection:**

Dart의 제네릭 타입은 구체화(reified)된다. 이는 실행 시점(runtime)에 타입 정보를 가지고 다닌다는 것을 의미한다. 예를 들어, 컬렉션의 타입을 테스트할 수 있다:

```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true

```

-   **[Q] 제네릭 타입이 구체화(reified) 된다는 것은?**
    
    Dart에서 제네릭 타입이 구체화(reified)된다는 것은, **실행 시점(런타임)에도 타입 정보를 유지한다는 것**을 의미한다. 실행 시점(런타임)에 타입 검사를 할 수 있어 코드의 타입 안전성을 높일 수 있다.
    

⚠️ **Note 주의**

**In contrast, generics in Java use _erasure_, which means that generic type parameters are removed at runtime. In Java, you can test whether an object is a List, but you can't test whether it's a `List<String>`.**

반면에, Java의 제네릭은 타입 소거(erasure)를 사용하여 실행 시에 제네릭 타입 매개변수가 제거됩니다. Java에서는 객체가 List인지 여부를 테스트할 수 있지만, List<String>인지 여부는 테스트할 수 없습니다.

## **Restricting the parameterized type[#](https://dart.dev/language/generics#restricting-the-parameterized-type) 매개변수화된 타입 제한하기**

**When implementing a generic type, you might want to limit the types that can be provided as arguments, so that the argument must be a subtype of a particular type. You can do this using `extends`.**

제네릭 타입을 구현할 때, 특정 타입의 서브타입만 인수로 제공되도록 제한하고 싶을 수 있다. 이를 위해 extends를 사용할 수 있다.

-   **[Q] 서브타입만 인수로 제공되도록 제한한다는 것은?**
    
    제네릭 타입을 사용할 때, 특정 타입의 하위 타입만 사용할 수 있도록 제한하는 것을 의미한다. 즉, 제네릭 타입 매개변수가 특정 타입 또는 그 타입의 서브타입(하위 클래스)이어야만 한다는 것을 뜻한다.
    
    **예시 코드**
    
    예를 들어, 동물(Animal) 클래스와 그 하위 클래스인 개(Dog)와 고양이(Cat)가 있다고 가정해보자. 우리가 동물 타입의 제네릭 케이지(Cage)를 만들고, 케이지에는 오직 동물(Animal)의 서브타입만 넣을 수 있도록 하고 싶다면 이렇게 코드를 작성할 수 있다:
    
    ```dart
    // Animal 클래스 정의
    class Animal {
      void makeSound() {
        print('Animal sound');
      }
    }
    
    // Animal의 서브클래스 Dog
    class Dog extends Animal {
      @override
      void makeSound() {
        print('멍멍');
      }
    }
    
    // Animal의 서브클래스 Cat
    class Cat extends Animal {
      @override
      void makeSound() {
        print('냥냥');
      }
    }
    
    // 제네릭 클래스 Cage<T> 정의, T는 Animal의 서브타입이어야 함
    class Cage<T extends Animal> {
      T animal;
    
      Cage(this.animal);
    
      void displayAnimalSound() {
        animal.makeSound();
      }
    }
    
    void main() {
      // Dog 객체를 포함하는 케이지 생성
      Cage<Dog> dogCage = Cage(Dog());
      dogCage.displayAnimalSound(); // 멍멍
    
      // Cat 객체를 포함하는 케이지 생성
      Cage<Cat> catCage = Cage(Cat());
      catCage.displayAnimalSound(); // 냥냥
    
      // String은 Animal의 서브타입이 아니므로, 컴파일 오류 발생
      // Cage<String> stringCage = Cage('Not an animal'); // 오류
    }
    
    ```
    
    **Animal 클래스와 서브클래스 정의:**
    
    -   Animal 클래스는 기본 동물 소리를 출력하는 메서드를 가진다.
    -   Dog와 Cat 클래스는 Animal을 상속(extends)받아 각각의 소리를 출력하는 메서드를 재정의(override)한다.
    
    **Cage 클래스 정의**:
    
    -   Cage 클래스는 제네릭 클래스이며, 타입 매개변수 T는 Animal의 서브타입이어야 한다(T extends Animal).
    -   Cage 클래스는 animal이라는 멤버 변수를 가지며, displayAnimalSound 메서드를 통해 동물 소리를 출력한다.
    
    **main 함수**:
    
    -   Dog 객체를 포함하는 케이지를 생성하고 소리를 출력
    -   Cat 객체를 포함하는 케이지를 생성하고 소리를 출력
    -   String 타입은 Animal의 서브타입이 아니므로, Cage<String>을 생성하려고 하면 컴파일 오류가 발생
    
    이 예제는 제네릭 타입을 사용할 때 특정 타입의 서브타입만 허용하도록 제한하는 방법을 보여준다. 이를 통해 불필요한 타입 오류를 방지하고, 더 안전한 코드를 작성할 수 있다.
    

**A common use case is ensuring that a type is non-nullable by making it a subtype of `Object` (instead of the default, [`Object?`](https://dart.dev/null-safety/understanding-null-safety#top-and-bottom)).**

일반적인 사용 사례는 타입을 Object의 서브타입으로 만들어(기본값인 Object? 대신) non-nullable 타입이 되도록 보장하는 것이다.

```dart
class Foo<T extends Object> {
  // Any type provided to Foo for T must be non-nullable.
}

```

**You can use `extends` with other types besides `Object`. Here's an example of extending `SomeBaseClass`, so that members of `SomeBaseClass` can be called on objects of type `T`:**

Object 외에도 다른 타입과 함께 extends를 사용할 수 있다. 다음은 SomeBaseClass를 확장하는 예제이다. 이를 통해 SomeBaseClass의 멤버를 타입 T의 객체에서 호출할 수 있다:

```dart
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}

```

**It's OK to use `SomeBaseClass` or any of its subtypes as the generic argument:**

제네릭 인수로 SomeBaseClass 또는 그 하위 타입을 사용하는 것은 가능하다:

```dart
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();

```

**It's also OK to specify no generic argument:**

제네릭 인수를 지정하지 않는 것도 가능하다:

```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'

```

**Specifying any non-`SomeBaseClass` type results in an error:**

SomeBaseClass가 아닌 다른 타입을 지정하면 오류가 발생한다:

```dart
// ✗ static analysis: failure
var foo = Foo<Object>();

```

## **Using generic methods[#](https://dart.dev/language/generics#using-generic-methods) 제네릭 메서드 사용하기**

**Methods and functions also allow type arguments:**

메서드와 함수도 타입 인수를 허용한다:

```dart
T first<T>(List<T> ts) {
  // Do some initial work or error checking, then...
  T tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}

```

**Here the generic type parameter on `first` (`<T>`) allows you to use the type argument `T` in several places:**

여기서 첫 번째 제네릭 타입 매개변수 <T>는 다음과 같이 여러 곳에서 타입 인수 T를 사용할 수 있게 해준다:

-   **In the function's return type (`T`).**
    
    함수의 반환 타입 (T)
    
-   **In the type of an argument (`List<T>`).**
    
    인수의 타입 (List<T>)
    
-   **In the type of a local variable (`T tmp`).**
    
    로컬 변수의 타입 (T tmp)
