Решение тестового задания.
Первая версия задания сдела с использованием QWidget

Разрабатывалось и тестировалось на Ubuntu 16.04.4 desktop amd64 LTS

Проект состоит из следующих директорий:
- calc_widget - калькулятор выполненный с использованием QWidget
Для сборки необходимо перейти в подкаталог calculator и последовательно выполнить qmake и make. 

- lib - исходные тексты динамической библиотеки
Для сборки необходимо выполнить make.

- test_lib - исходные тексты для тестирования библиотеки. Так как библиотека не большая сторонние библиотеки тестирования не использовались.
Для сборки необходимо выполнить make.

Задание:
I. Программа калькулятор на Qt.

Аналог калькулятора Windows (вид простой, не научный)
Операции калькулятора: сложение, вычитание, деление, умножение.

Все операции вычисления выполняются посредством вызова функции из ВНЕШНЕЙ DLL, написанной на С++ в любой среде разработки.
Прототип этой функции:
double DoIt (int TypeWork, double OperandA, double OperandB, int& ErrorCode)
TypeWork - Тип операции (+,-,/,*)

Ввести 2 ПОТОКОНЕЗАВИСИМЫХ очереди.
QueueRequests  и QueueResults
В первую складываются запросы на вычисления.
Во вторую результаты вычислений.
Размер очередей отображать на интерфейсе

Операция вычисления выполняется в ОТДЕЛЬНОМ ПОТОКЕ.
Каждое вычисление длится определенное время, которое можно задать в секундах с интерфейса. Это нужно для симуляции длительных вычислений
и  накопления очереди запросов.
Принцип такой:
Поток 1
1. по прошествии времени предыдущего вычисления из QueueRequests берется следующий элемент (если он там есть). 
2. Извлеченный элемент отправляется на обработку. 
3. Ожидание окончания вычисления и укладка  в QueueResults.
4. далее по кругу.
Поток 2
 Формирует новые запросы в очередь запросов (QueueRequests)
 Выводит результаты изх очередеи результатов (QueueResults)

Для вывода запросов на вычисление, результатов вычислений, ошибок
использовать некую консоль, желательно с цветным текстом
Запросы - зеленые, результаты - синие, ошибки - красные

Каждое нажатие "=" приводит к складыванию в очередь запросов текущего введенного запроса на вычисление, если оно существует конечно :)
иначе ошибку в консоль.

Интерфейс делается по вкусу, без нелепостей и лишних галочек.
Красота и эргономичность приветствуется.
При выходе программа сохраняет свои размеры и положение.
При запуске восстанавливает свои размеры и положение, если есть из чего восстанавливать.

Важно:
При разработке пользовательского интерфейса использовать QML 
Обработка всех ошибок:
	Ошибки ввода.
    Многократное нажатие кнопок (типа тупое тыркание).
    Ошибки вычисления (их не так уж и много, например деление на 0).
    Вывод ошибок НЕ осуществлять через МessageBox. Очень раздражает обычных пользователей.
