В данной лабораторной работе необходимо было выполнить зашумление изображения (шум соли и перца) и обработку полученного зашумленного изображения при помощи медианного фильтра на cpu, gpu, сравнить время работы.
Исходный код был написан на языке Python, для использования графического процессора применялась библиотека Numba.

# Ход работы:
1. Производим зашумление изображения при помощи функции (`def noise_generator`), задав параметры для работы (p - значение вероятности, от которого зависит, в какой цвет (черный или белый) будут окрашены выбранные пиксели)
2. Применяем написанный медианный фильтр к зашумленному изображению на центральном процессоре при помощи функции (`def salt_and_pepper_cpu`)
3. Производим подсчет времени работы
4. Применяем написанный медианный фильтр к зашумленному изображению на графическом процессоре при помощи (`def salt_and_pepper_gpu`)
5. Производим подсчет времени работы
6. Визуально оцениваем результаты работы
7. Производим расчет ускорения работы программы
8. Меняем размеры изображения и повторяем вышеописанные действия
9. Строим графики зависимости ускорения и времени от размеров изображений

Распараллеливанию подлежали математические операции (преимущественно сравнения), производящие расчет значений нового пикселя (сортировка значений параметров цвета окружающих пикселей (маска из 9 пикселей - замена центрального) и поиск медианного значения в отсортированном массиве). Для этого изображение (набор пикселей) передавалось
на девайс, чтобы все созданные нити могли выполнить функцию ядра (`def salt_and_pepper_kern`) над данными. Полученные данные возвращались обратно на хост.
Распараллеливание операций производилось по следующему принципу: каждый поток (ядро cuda) вычислял единственный элемент результирующей матрицы (пиксель в новом изображении).

# Полученные результаты:

график времени:

![image](https://github.com/user-attachments/assets/a3680818-0689-4f5b-b993-9c90447bfdc4)

график ускорения:

![image](https://github.com/user-attachments/assets/6f0bf65e-d30f-40bf-9922-ef8cef0633f8)

шумное изображение

![image](https://github.com/user-attachments/assets/ba3b3783-6982-43f7-89d6-a80f52cd2f50)

изображение на cpu

![image](https://github.com/user-attachments/assets/2dd2c600-3f19-4781-9756-e9801290c2f0)

изображение на gpu

![image](https://github.com/user-attachments/assets/c084422c-ec24-4b9b-9860-ce21ee09679f)
