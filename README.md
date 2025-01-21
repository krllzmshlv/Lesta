# Lesta
Тестовое задание
Вопрос №1

На языке Python написать алгоритм (функцию) определения четности целого числа, который будет аналогичен нижеприведенному по функциональности, но отличен по своей сути. Объяснить плюсы и минусы обеих реализаций. 

Пример: 
```python
def isEven(value):
      return value % 2 == 0
```

Ответ:
Начальная функция проверяет четность числа путем проверки остатка от деления его на 2.
Плюс: более простая и понятная реализация.
Минус: менее очевидно для понимания кода другим человеком.
```python
def isEven(value):
      return "{0:b}".format(value)[-1] == '0'
```
Функция использует бинарный формат числа для определения его четности, неявно преобразуя число в двоичный формат и проверяет последний бит, чтобы определить, является ли число четным. 
Плюс: алгоритм не использует количество операций деления .
Минус: менее очевидно для понимания кода другим человеком.

Однако, использование двоичного представления числа может быть сложно и неочевидно для понимания кода другими разработчиками. Также этот метод может быть менее надежным, так как он предполагает, что число ровно в двоичном формате, что может вызвать ошибки при работе с числами, например, вещественными числами.

Вторая реализация проверяет четность числа путем проверки остатка от деления числа на 2. Этот метод является более прямым и понятным для других разработчиков, что делает его легче для понимания и отладки кода. Он также учитывает больший диапазон чисел, включая вещественные числа.

Недостатком второго подхода может быть небольшое уменьшение производительности из-за операции деления, хотя современные компиляторы и исполнители обычно оптимизируют такие операции.

Таким образом, каждая из реализаций имеет свои плюсы и минусы, и выбор между ними зависит от конкретных требований проекта и предпочтений разработчика.
      
Вопрос №2

На языке Python написать минимум по 2 класса реализовывающих циклический буфер FIFO. Объяснить плюсы и минусы каждой реализации.
Оценивается:

Полнота и качество реализации
Оформление кода
Наличие сравнения и пояснения по быстродействию
1.
```python
class FIFO:
  def __init__(self, lenf, stindex):
        self.fifo = [None] * lenf
        self.lenfifo = lenf
        self.start = stindex #откуда начинать записывать новые
        self.end = stindex #откуда начинать стирать

  def add_new(self, num):
        if ((self.start == self.end) & (self.fifo[self.end-1] != None)):
           if (self.end ==  self.lenfifo):
              self.end == 0
           else:
              self.end = self.end + 1

        if (self.start ==  self.lenfifo-1):
          self.fifo[self.start] = num
          self.start = 0
        else:
          self.fifo[self.start] = num
          self.start = self.start + 1


  def del_last(self, count):
      for i in range(count):
        self.fifo[self.end] = None
        if (self.end ==  self.lenfifo - 1):
          self.end = 0
        else:
          self.end = self.end + 1

  def reset(self):
    self.fifo = [None] * self.lenfifo

  def next(self):
    return self.fifo[self.end]

  def empty(self):
    if self.fifo.count(None) == 0:
      print('Буфер полон')
    if self.fifo.count(None) == self.lenfifo:
      print('Буфер пуст')

  def fifocount(self):
    return self.lenfifo - self.fifo.count(None)
```
2.
```python
class FIFO:
  def __init__(self, lenf, stindex):
        self.fifo = [None] * lenf
        self.lenfifo = lenf
        self.start = stindex #откуда начинать записывать новые

  def add_new(self, num):
    self.fifo[self.start] = num
    if self.fifo.count(None) == 0:
      self.start =  self.start - self.lenfifo +1
      if self.start < 0: self.start = self.start + self.lenfifo
    else:
      self.start =  self.start + 1
      if self.start == self.lenfifo:
        self.start = 0
    print(self.fifo, self.start)

  def del_last(self, count):
    for i in range(count):
      c = self.fifo.count(None)
      self.fifo[x.start - (x.lenfifo - x.fifo.count(None))] = None


  def reset(self):
    self.fifo = [None] * self.lenfifo

  def next(self):
    return self.fifo[self.end]

  def empty(self):
    if self.fifo.count(None) == 0:
      print('Буфер полон')
    if self.fifo.count(None) == self.lenfifo:
      print('Буфер пуст')

  def fifocount(self):
    return self.lenfifo - self.fifo.count(None)
```

Вопрос №3

На языке Python предложить алгоритм, который быстрее всего (по процессорным тикам) отсортирует данный ей массив чисел. Массив может быть любого размера со случайным порядком чисел (в том числе и отсортированным). Объяснить, почему вы считаете, что функция соответствует заданным критериям.

Пройти методом подсчета по массиву. Сделать второй массив размером равный максимальному значению в массиве, отсортированному по возрастанию. Провизвести повтор массива с подсчетом на второй массив. Данный способ будет достаточно быстрым за 
