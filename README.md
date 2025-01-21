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
<br />Плюсы: более простая и понятная реализация.
<br />Минусы: менее очевидно для понимания кода другим человеком.
```python
def isEven(value):
      return "{0:b}".format(value)[-1] == '0'
```
Функция использует бинарный формат числа для определения его четности, неявно преобразуя число в двоичный формат и проверяет последний бит, чтобы определить, является ли число четным.<br /> 
Плюсы: алгоритм не использует  операцию деления. <br />
Минусы: 1. менее очевидно для понимания кода другим человеком. <br />2. требуется приведение числа к бинарному представлению.

      
Вопрос №2

На языке Python написать минимум по 2 класса реализовывающих циклический буфер FIFO. Объяснить плюсы и минусы каждой реализации.
Оценивается:
<br />Полнота и качество реализации
<br />Оформление кода
<br />Наличие сравнения и пояснения по быстродействию
<br />Ответ
1. Реализация с использованием указателей чтения и записи
```python
class FIFO:
  def __init__(self, lenf, stindex):
        self.fifo = [None] * lenf
        self.lenfifo = lenf
        self.write = stindex #откуда начинать записывать новые элементы
        self.read = stindex #откуда начинать стирать элементы

  #Записать новый элемент в буфер
  def write_new(self, num):
        if ((self.write == self.read) and  (self.fifo[self.read-1] != None)):
           if (self.read ==  self.lenfifo):
              self.read = 0
           else:
              self.read = self.read + 1

        if (self.write ==  self.lenfifo-1):
          self.fifo[self.write] = num
          self.write = 0
        else:
          self.fifo[self.write] = num
          self.write = self.write + 1

  #Очистить самый ранний элемент в количестве count
  def read_old(self, count):
      for i in range(count):
        if self.fifo.count(None) != self.lenfifo:
          self.fifo[self.read] = None
          if (self.read ==  self.lenfifo - 1):
            self.read = 0
          else:
            self.read = self.read + 1
        else:
          print('Буфер пуст')

  #Очистка буфера
  def reset(self):
    self.fifo = [None] * self.lenfifo
    self.read = 0

  #Вывести следующий считываемый из буфера элемент
  def next(self):
    return self.fifo[self.read]

  #Проверка буфера на заполненность
  def empty(self):
    if self.fifo.count(None) == 0:
      print('Буфер полон')
    if self.fifo.count(None) == self.lenfifo:
      print('Буфер пуст')

  #Подсчет количества элементов в буфере
  def fifocount(self):
    return self.lenfifo - self.fifo.count(None)


```
2. Реализация с использованием указателя записи и подсчетом элементов буфера
```python
class FIFO:
  def __init__(self, lenf, stindex):
        self.fifo = [None] * lenf
        self.lenfifo = lenf
        self.write = stindex #откуда начинать записывать новые элементы

  #Записать новый элемент в буфер
  def write_new(self, num):
    self.fifo[self.write] = num
    if self.fifo.count(None) == 0:
      self.write =  self.write - self.lenfifo +1
      if self.write < 0: self.write = self.write + self.lenfifo
    else:
      self.write =  self.write + 1
      if self.write == self.lenfifo:
        self.write = 0

  #Очистить самый ранний элемент в количестве count
  def read_old(self, count):
    for i in range(count):
      self.fifo[self.write - (self.lenfifo - self.fifo.count(None))] = None


  #Очистка буфера
  def reset(self):
    self.fifo = [None] * self.lenfifo

  #Вывести следующий считываемый из буфера элемент
  def next(self):
    return self.fifo[self.write - (self.lenfifo - self.fifo.count(None))]

  #Проверка буфера на заполненность
  def empty(self):
    if self.fifo.count(None) == 0:
      print('Буфер полон')
    if self.fifo.count(None) == self.lenfifo:
      print('Буфер пуст')

  #Подсчет количества элементов в буфере
  def fifocount(self):
    return self.lenfifo - self.fifo.count(None)

```
Первый класс основан на использовании маркера - позиции чтения самого старого элемента
Плюсы:
1. Нет зависимости от количества записанных элементов в буфере, для перехода на новый круг достаточно знать размер буфера как границу.

Минусы:
1. При записи и чтении элемента также отслеживается указатель чтения.

Второай класс основан на использовании счетчика заполнения
Плюсы:
1. Простота и понятность логики, основанной на подсчете количества пустых ячеек в буфере.
2. Отсутствие необходимости в дополнительном указателе чтения.
3. Быстрее реализации с указателем чтения.

Вопрос №3

На языке Python предложить алгоритм, который быстрее всего (по процессорным тикам) отсортирует данный ей массив чисел. Массив может быть любого размера со случайным порядком чисел (в том числе и отсортированным). Объяснить, почему вы считаете, что функция соответствует заданным критериям.

Пройти методом подсчета по массиву. Сделать второй массив размером равный максимальному значению в массиве, отсортированному по возрастанию. Провизвести повтор массива с подсчетом на второй массив. Данный способ будет достаточно быстрым за 
