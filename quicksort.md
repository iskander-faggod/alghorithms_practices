#### Быстрая сортировка - Quick sort

В чем суть?

Сначала в массиве выбирается элемент, который мы называем опорным. Затем мы находим элементы больше и меньше него и выделяем их в два подмассива. 

Что мы имеем?

1) Подмассив, с элементами меньше опорного;
2) Опорный элемент;
3) Подмассив, с элементами больше опорного;

ВАЖНО! Подмассивы разделены относительно опорного элемента, но внутри ещё никак не отсортированы.

Мы применили к массиву разделение... А это заявочка на принцип "Разделяй и властвуй". Собственно, это он и есть! Далее мы рекурсивно применим быструю сортировку к двум полученным подмассивам. 

Всё сведётся к задаче о сравнивании двух одноэлементных массивов, прямо как в Merge Sort (Принцип ведь тот же).

### Псевдкод

1 Вариант (Схема Хоара):
    int partition(a: T[n], int l, int r)
        T v = a[(l + r) / 2]
        int i = l
        int j = r
        while (i ⩽ j) 
            while (a[i] < v)
            i++
            while (a[j] > v)
            j--
            if (i ⩾ j) 
            break
            swap(a[i++], a[j--])
        return j

    void quicksort(a: T[n], int l, int r)
        if l < r
            int q = partition(a, l, r)
            quicksort(a, l, q)
            quicksort(a, q + 1, r)

2 Вариант (Его я увидел в книге "Грокаем алгоритмы"):
    def quicksort(array):
        if len(array) < 2:
            return array
        else:
            pivot = array[0]
            less = [i for i in array[1:] if i <= pivot]

            greate = [i for i in array[1:] if i > pivot]

            return quicksort(less) + [pivot] + quicksort(greater)
 
### Пример

Пусть дан массив, ну, скажем 

3 5 2 1 4

В качестве опорного элемента будем выбирать средний элемент. Это у нас 2 
"Отрабатываем" алгоритм разделения на два подмассива:

1 5 2 3 4

1 2 5 3 4 

Так нам получилось разбить массив: 1 2 5 3 4 
Действительно, мы получили две части: [1] < 2 < [5, 3, 4];

Далее мы лишь рекурсивно сортируем эти подмассивы тем же образом

[1] уже отсортирован, его не трогаем

[5, 3, 4] - выберем опорным элементом 3

На выходе получаем готовый массив
1 2 3 4 5 :)

### Как мы выбираем опорный элемент?

Вообще опорный элемент играет очень важную роль в быстродействии этой сортировки. Предположим, опорным элементом всегда выбирается первый элемент, а быстрая сортировка применяется к уже отсортированному массиву. Она не проверяет, отсортирован ли массив и в любом случае пытается его отсортировать.

1 2 3 4 5
[1] 2 3 4 5
    ↓  
    [2] 3 4 5
        ↓
        [3] 4 5
            ↓ 
            [4] 5
                ↓
                [5]
                end
            
А если выберем средний?
 1 2 3 4 5
     ↓
1 2 [3] 4 5
 ↓       ↓
1 [2]    4 [5]

Как минимум, тут стек рекурсий поменьше))

И всё-таки как не ошибиться с выбором опорного элемента?
Никак :) Вы никогда не можете быть уверены в том, какие входные данные вам прилетят, но вы можете минимизировать "потери"

Для этого существует так называемый "рандомизированный алгоритм" быстрой сортировки

Суть та же, только pivot = rand(l, r);
И swap(a[r], a[pivot])
Да и всё)

Такая модификация обеспечивает любому из r - l + 1 элементов подмассива равную вероятность оказаться опорным. Благодаря случайному выбору опорного элемента можно ожидать, что разбиение входного массива в среднем окажется довольно хорошо сбалансированным. 


### Оценка сложности работы

По времени:

    В худшем случае мы разбиваем массив так, что одна часть будет состоять из n - 1 элементов, а другая из 1. Т.к. процедура разбиения занимает Thetta(n), для времени работы T(n) получится что-то типа:
    T(n) = T(n-1) + Thetta(n) = Thetta(n^2);

    При максимально несбалансированном разбиении время работы составляет Thetta(n^2).

    Среднее время работы алгоритма составит O(nlog(n)), где n у нас уходит на partiton, a log(n) на рекурсивный спуск.

    В лучшем случае алгоритм отработает так же Ω(nlog(n))

По памяти:
    Мы ничего не используем (кроме стека рекурсий), так что O(1);

### Устойчивость

Оно свапает одинаковые элементы, так что не устойчива :)

### Для задров

У быстрой сортировки есть какой-то странненький инвариант

1) если left <= любой индекс k <= i, то a[k] <=pivot;
2) если i + 1 <= k <= j - 1, то a[k] > pivot
3) если k = right, то a[k] = pivot

### Ссылки

https://neerc.ifmo.ru/wiki/index.php?title=Быстрая_сортировка

https://neerc.ifmo.ru/wiki/index.php?title=Сортировки


KrÜgar