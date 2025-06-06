# Отчет по реализации градиентного бустинга

## 1. Описание алгоритма градиентного бустинга

Градиентный бустинг (Gradient Boosting) — это метод машинного обучения, который строит ансамбль слабых моделей (обычно решающих деревьев) последовательно, где каждая новая модель обучается на остаточных ошибках предыдущих моделей.

## 2. Описание датасета

В экспериментах использовался датасет "Titanic Dataset" с платформы Kaggle. Это классический набор данных, содержащий информацию о пассажирах корабля Титаник:

- **Задача**: предсказать выживание пассажиров (бинарная классификация)
- **Признаки**: возраст, пол, класс билета, количество родственников на борту, стоимость билета и др.
- **Размер**: 891 пассажир с 12 характеристиками

Предобработка данных включала:
- Заполнение пропущенных числовых значений медианой
- Заполнение пропущенных категориальных значений наиболее частыми значениями
- One-hot кодирование категориальных признаков
- Удаление нерелевантных столбцов (PassengerId, Name, Ticket, Cabin)

Финальный датасет был разделен на обучающую (80%) и тестовую (20%) выборки.

## 3. Результаты экспериментов

Для оценки качества модели использовалась кросс-валидация (5 фолдов) и метрика F1-score.

**Результаты пользовательской реализации градиентного бустинга:**
- Отдельные оценки F1 по фолдам: [0.7667, 0.7023, 0.7395, 0.7460, 0.7626]
- Средняя F1-мера: 0.7434

**Время выполнения:**

193 ms ± 1.92 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

**Параметры модели:**
- Количество базовых алгоритмов (деревьев): 50
- Скорость обучения: 0.02
- Максимальная глубина деревьев: 5


## 4. Сравнение с эталонной реализацией

Для сравнения использовалась реализация `GradientBoostingClassifier` из библиотеки scikit-learn с аналогичными параметрами.

**Результаты scikit-learn реализации:**
- Отдельные оценки F1 по фолдам: [0.7563, 0.6789, 0.7227, 0.7069, 0.6977]
- Средняя F1-мера: 0.7125

**Время выполнения:**

59.6 ms ± 509 μs per loop (mean ± std. dev. of 7 runs, 10 loops each)


**Замечание:** В работе не представлено явное измерение времени обучения моделей, что требовалось в задании. Однако можно предположить, что оптимизированная реализация scikit-learn работает быстрее, особенно учитывая использование минимизации функции потерь в пользовательской реализации для каждого дерева.

## 5. Выводы

1. **Качество модели**: Пользовательская реализация градиентного бустинга показала более высокую точность на тестовом наборе данных по сравнению с эталонной реализацией из scikit-learn. Это может быть связано с более точной оптимизацией весов для каждого дерева.

2. **Особенности реализации**: Ключевыми особенностями реализованного алгоритма являются:
   - Использование оптимизатора L-BFGS-B для поиска оптимальных весов деревьев
   - Реализация логистической функции потерь и её градиента для бинарной классификации
   - Использование сигмоидальной функции для преобразования предсказаний

3. **Датасет Титаник**: Данный набор данных является хорошим примером "плохого" датасета. 

4. **Потенциальные улучшения**: Для дальнейшего совершенствования модели можно:
   - Реализовать различные функции потерь
   - Добавить регуляризацию для предотвращения переобучения
   - Оптимизировать процесс поиска оптимальных параметров
   - Улучшить эффективность вычислений для ускорения обучения

