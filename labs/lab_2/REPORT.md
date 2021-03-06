#№ Отчет по лабораторной работе №2
## по курсу "Логическое программирование"

## Решение логических задач

### студент: Дубинин А.О.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |      4        |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*


## Введение

Решение логических задач имеет 2 основных подхода: метод порождения и проверок и метод ветвей и границ. Главное их свойство в том, что они перебирают набор решений, и проверяют, удовлетворяет ли он заданным условиям. 
Их различие сводится к методам проверки удовлетворения заданным условиям. Метод проверок можно разделить на две отдельные части: первая часть генератор: она пытается угадать данные (генерирует множество исходных данных), и второй части -- проверяющего. Проверяющий, соответсвенно проверяет пришедшее ему решение. Второй метод серьёзно отличается от первого. В методе ветвей и границ проверяющий и генератор тесно связаны: генерация всех условий происходит не моментально (сразу все), а постепенно, шагами. Значительные части возможных, но неверных, решений отсекаются на ранних шагах. 

Например, в моём задании было использовано `different('Гек', 'Чук').` Сразу после генерации списка успехов в day -- это отсекает возмоможные варианты, где собеседник и приятель один и тот же человек. Второй метод, более сложен по сравнению с наивным первым, но значительно производительней, поэтому он предпочтительней.

## Задание

6. Может быть, вы и не поверите, но в одном городке жили два чудака: Чук и Гек. Чук совершенно не мог говорить правду по понедельникам, вторникам и средам, хотя в остальные дни он неизменно был правдив. А Гек врал по вторникам, четвергам и субботам, но в другие дни он говорил только правду. Как-то я повстречал эту неразлучную пару и спросил одного из них: Скажи пожалуйста, как тебя зовут? Тот без малейшего колебания ответил: Чук. А скажи-ка мне, какой сегодня день недели? Вчера было воскресенье,сказал мой собеседник. А завтра будет пятница,- добавил его приятель. Подожди, как же так? изумился я, обращаясь к приятелю моего собеседника.Ты уверен, что говоришь правду? Я всегда говорю правду по средам,- услышал я в ответ. Решив, что больше со мной говорить не о чем, приятели пошли дальше, оставив меня в полном недоумении. Но, подумав, я все-таки сообразил, кто из двух друзей был Чук, а кто Гек. Между прочим, по разговору можно установить и день недели, в который я встретился с ними. Попробуйте сообразить и вы.

## Принцип решения

```
solve :-
    day(First, Today, FirstTrue),
    different(First, Second),
    day(Second, Today, SecondTrue),
    
    expression1(FirstTrue, First),
    expression2(FirstTrue, Today),
    expression3(SecondTrue, Today),
    expression4(SecondTrue, Second),
    
    write('First: '), write(First), nl,
    write('Second: '), write(Second), nl,
    write('Today: '), write(Today), nl,
    !.
```
Главный предикат `solve`, он берет за основу факты о днях, когда врет первый или второй приятель. Например:

```
day('Чук','Воскресенье',1).%Чук говорит правду в воскресенье
day('Гек','Вторник',0).%А Гек во вторник врет
```

`solve` пробегает по всем дням недели, узнавая с помощью day, врут или говорят правду каждый из приятелей. И проверяет на выражения, которые приятели сказали про сегодняшний день. 

```
expression1(0, First) :- First \= 'Чук'.
expression1(1, First) :- First = 'Чук'.
 
expression2(0, Today) :- not(yesterdayWas('Воскресенье',Today)).
expression2(1, Today) :- yesterdayWas('Воскресенье',Today).
 
expression3(0, Today) :- not(yesterdayWas(Today,'Пятница')).
expression3(1, Today) :- yesterdayWas(Today,'Пятница').

expression4(SecondTrue, Second) :-
    day(Second, 'Среда', SecondTrue).
```
Например, первый факт проверяет, если первый собеседник сегодня говорит правду то, он Чук:

```
%(Говорит правду или нет, Собеседник)
expression1(0, First) :- First \= 'Чук'.
expression1(1, First) :- First = 'Чук'.
```

При успешном прохождении всех условий, решение явлется найденным и мы его выводим, и с помошью оператора !(cut), убираем лишнии попытки поиска других решений, так как мы знаем, что решение одно.

```
write('First: '), write(First), nl,
write('Second: '), write(Second), nl,
write('Today: '), write(Today), nl,
!.
```

Результаты выполнения программы:

```
?- solve.
First: Гек
Second: Чук
Today: Вторник
true.

```

## Выводы

Пролог очень удобен для решения логических задач. Подобные задачи человеку решать таким методом, которым их решает Прлог практически нереально: слишком много возможных вариантов. Вместо этого используются различные приёмы, например, метод исключения и логический квадрат. Если говорить о кретивной части решения Прологом, в программе отсутствуют изящные логические выводы и хитрости, задача ломается под мощью перебора (который тем не менее оптимизирован), это хороший пример использования скорости недоступной человеческому мышлению: работа программиста -- лишь объяснить задачу машине, и она сию же секунду выдаст правильное решение. Человек может просидеть с даннной задачей пару вечеров, получив, правда, несколько более "красивое" решение.

По поводу повторной используемости данного кода (и применения его в общем и целом), сложно придумать ситуацию, при которой бы возникала необходимость решать большое количество однотипных логических задач -- ситуацию в которой нужно применять данную программу. Тем не менее поскольку условия задачи чётко разделяются на типы, можно представить себе генератор, который будет либо создавать задачи подобного плана, для применения, например, в учебных целях, либо решать такие типовые задачи, самостоятельно генерируя проверки из условий задач. 




