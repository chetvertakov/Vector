# Vector

Реализован небольшой аналог шаблонного класса vector из стандартной библиотеки. \
Данные хранятся в памяти, динамически выделяемой в куче. Память выделяется неинициализированная, инициализация происходит при фактическом добавлении элементов в вектор.\
При добавлении новых элементов, если выделенной памяти недостаточно - выделяется новый участок памяти размером в два раза больше предыдущего, в который перемещаются (либо копируются) данные из старого участка памяти, после чего старый участок удаляется.

## Функционал класса
### Создание вектора
* конструктор по-умолчанию, создающий пустой вектор
```c++
Vector<int> vec;
std::cout << vec.Capacity() << " " << vec.Size();
``` 
* конструктор, создающий вектор заданного размера (инициализирован значениями по умолчанию)
```c++
Vector<int> vec(10);
std::cout << vec.Capacity() << " " << vec.Size() << std::endl;
std::cout << vec;
``` 
* копирующий конструктор и перемещающий конструктор
```c++
Vector<int> vec1(5);
Vector<int> vec2(vec1);
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
Vector<int> vec3(std::move(vec1));
std::cout << "After moving vec1 to vec3:\n";
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
std::cout << "vec3: " << vec3;
```
* операторы копирующего присваивания и перемещающего присваивания
```c++
Vector<int> vec1(5);
Vector<int> vec2 = vec1;
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
Vector<int> vec3 = std::move(vec1);
std::cout << "After moving vec1 to vec3:\n";
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
std::cout << "vec3: " << vec3;
```
* обмен двух векторов
```c++
Vector<int> vec1(5);
Vector<int> vec2(10);
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
vec1.Swap(vec2);
std::cout << "After swap:\n";
std::cout << "vec1: " << vec1;
std::cout << "vec2: " << vec2;
```

### Методы
* Добавление/удаление элементов в конец вектора
```c++
Vector<int> vec;
vec.PushBack(1);
vec.PushBack(3);
vec.PushBack(5);
vec.PushBack(7);
vec.PopBack();
std::cout << vec;
```

* Конструирование элемента в векторе
```c++
class TestStruct {
public:
    explicit TestStruct(int a, int b) : a_{a}, b_{b} {}
    friend std::ostream& operator<<(std::ostream& out, TestStruct t) {
        out << "{a = " << t.a_ << ", b = " << t.b_ << "}";
        return out;
    }
private:
    int a_ = 0, b_ = 0;
};


Vector<TestStruct> vec;
// здесь мы конструируем новые элементы прямо в памяти вектора
vec.EmplaceBack(1,2);
vec.EmplaceBack(3,4);
std::cout << vec;
```

* добавление/удаление элемента в произвольное место в вектора (по итератору)
```c++
Vector<int> a;
a.Insert(a.cbegin(), 1);
// метод Emplace работает аналогично методу Insert, но конструирует элемент сразу по месту в векторе
a.Emplace(a.cbegin(), 2);
a.Insert(a.cbegin(), 3);
a.Insert(a.cbegin(), 4);
a.Erase(a.begin() + 1);
std::cout << a;
```
* Резервирование места в векторе под новые элементы
```c++
Vector<int> a;
std::cout << a.Capacity() << " " << a.Size() << std::endl;
// без резервирования при превышении размера будет происходить реаллокация
a.PushBack(1);
std::cout << a.Capacity() << " " << a.Size() << std::endl;
a.PushBack(2);
std::cout << a.Capacity() << " " << a.Size() << std::endl;
a.PushBack(3);
std::cout << a.Capacity() << " " << a.Size() << std::endl;
a.PushBack(4);
std::cout << a.Capacity() << " " << a.Size() << std::endl;

Vector<int> b;
b.Reserve(10);
std::cout << a.Capacity() << " " << a.Size() << std::endl;
// с резервированием реаллокации происходить не будет, т.к. места достаточно для размещения новых элементов
b.PushBack(1);
std::cout << b.Capacity() << " " << b.Size() << std::endl;
b.PushBack(2);
std::cout << b.Capacity() << " " << b.Size() << std::endl;
b.PushBack(3);
std::cout << b.Capacity() << " " << b.Size() << std::endl;
b.PushBack(4);
std::cout << b.Capacity() << " " << b.Size() << std::endl;
```
* Изменение размера вектора
```c++
Vector<int> a;
a.PushBack(1);
a.PushBack(2);
a.PushBack(3);
a.PushBack(4);
std::cout << a;
// при увеличении размера вектора новые элементы будут добавлены в конец вектора со значениями по умолчанию
a.Resize(10);
std::cout << a;
std::cout << a.Capacity() << " " << a.Size() << std::endl;
// при уменьшении размера вектора лишние элементы будут удалены, при этом ёмкость вектора не уменьшится
a.Resize(3);
std::cout << a;
std::cout << a.Capacity() << " " << a.Size() << std::endl;
```

* Итерирование по элементам вектора в for-range цикле
```c++
Vector<int> a(10);
int i = 0;
// присваивем элементам вектора значения 0..9
for (auto &elem : a) {
    elem = i++;
}
std::cout << a;
```

## Установка и использование
Просто скопируйте файл vector.h в папку с вашим проектом и подключите через директиву **#include<vector.h>**

## Тесты
Тесты находятся в файле test.cpp. При желании можно собрать тестовую программу с использованием приложенного CMakeLists.txt

## Системные требования
Компилятор С++ с поддержкой стандарта C++17 или новее