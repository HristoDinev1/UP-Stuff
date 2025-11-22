### Пространства от имена 
---
Това съм го писал миналата година - мега дървено - имам и да добавям неща



def: Scope в който се дефинират полета.

Семантика: Инструмент за избягване на конфликти от имена

Синтаксис: 
namespace <име на пространство> {};

```cpp
namespace ns 
{
int a;
int b = 5;
void f() {};
}
```

Достъпване на полетата става чрез оператор за резолюция '::'

``` cpp
int main ()
{
  ns::a = 5;
  int b = ns::b;
  ns::f();

  return 0;
}
```


Ключовата дума using се използва за да вмъкне определен scope или символ в текущия scope. Тогава не е нужно да се използва оператор за резолюция за да достъпим полетата на пространството от имена

``` cpp
using namespace std;
using namespace ns;

int main()
{
  cin>>b;
  f();
}
```

``` cpp

using ns::a;
using std::cout;

int main()
{
  a = 5;
  cout<<"Hello World!";
}
```



#### Вложени пространства от имена

``` cpp


namespace a
{
    void f() {}
    namespace b
    {
        void f() {}
    }
}


int main()
{
    a::f();
    a::b::f();
}
```
Използване на Using

``` cpp
using namespace A::B;
```



#### Aнонимни пространства от имена

Свързват се с една компилационна единица.

```cpp
#include <iostream>
//File 1
namespace
{
  void f() {}
}

namespace
{
  int a = 6;
}

int main()
{
  return 0;
}
```
```cpp
#include <iostream>
//File 2
namespace
{
  void f() {}
}

namespace
{
  int a = 5;
}

int main()
{
  return 0;
} // Няма конфликт!
```
#### Shadow
``` cpp
namespace ns
{
int a = 5;
}

int main()
{
  int a = 6;
  std::cout<< a; // 6
}
```
Пространство от имена НЕ пречи създаването на локални променливи чиито съвпадат с това на техни полета (или самото пространство)


#### Възможни конфликти

* При вмъкване на пространства с променливи или функции с еднакви сигнатури може да стане конфликт от имена 

``` cpp
#include <iostream>

namespace A
{
  void f() {}
}

namespace B
{
  void f() {}
}

using namespace A; // вмъкването на двете пространства по само себе си не дава грешка 
using namespace B; // докато не се опитаме да достъпим поле което има еднаква сигнатура

int main()
{
  f();  //Error (active)	E0308	more than one instance of overloaded function "f" matches the argument list:
        //Error	C2668	'A::f': ambiguous call to overloaded function
        // Компилационна грешка : identifier has multiple overloads  // bla bla bla NERD
}
```
* Не използването на оператор за резолюция
``` cpp
namespace ns { void f();}
int main()
{
  f(); // компилационна грешка: identifier not found
}
```

* Конфликт на имена в анонимни пространства
``` cpp
#include <iostream>

namespace
{
    int a;
}
namespace
{
    int a;
}

//Error	C2086	'int `anonymous-namespace'::a': redefinition
```

* ODR (One Definition Rule) Violations:
Дефиниране на едно и съшо поле в няколко компилационни едини може да доведе до linking грешки
```cpp
namespace MyNamespace {
    int value = 10; // If 'value' is defined in multiple translation units, it causes a linker error.
}                  
```

* Некоректно използване на оператор за резолюция при вложени пространства
``` cpp
namespace a
{
    void f() {}
    namespace b
    {
        void f() {}
    }
}

int main()
{
a::b::f(); \\ коректно
b::f(); некоректно // компилационна грешка : identifier not found
}
```


