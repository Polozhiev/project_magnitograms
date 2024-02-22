# project_magnitograms

Image segmentation

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ctmywu1Ko0iojE4HnimdZibSgFnda5nV?usp=sharing)

Для проверки газовых труб на наличие дефектов используются специальные устройства-дефектоскопы, которые движутся по трубам и снимают магнитограммы при помощи датчиков.

![](https://github.com/Polozhiev/project_magnitograms/blob/main/example.png)

### Цель работы:
* Выделение конструктивных элементов
  + Сварные швы
  + Разветвления
  + Сгибы
  + Заплатки
* Выделение дефектов

[Данные](https://disk.yandex.ru/d/MvqsNJL3zY-MnA) размечены по столбцам. Все изображения черно-белые размера 128x4096 и для каждого изображения даны два вектора длиной 4096, с метками конструктивных элементов и метками дефектов.

### Метрики и критерии оценки
Во всех метриках $y$ -- метка класса, $y_i$ -- реальный класс $i$-того участка, $\hat{y}_i$ -- предсказанный класс $i$-того участка

Метрики для оценки качества выявления конструктивных элементов выглядит следующим образом:

$$ P_{el} = 0.25 \sum \frac{ 	\text{card} (\hat{y}_i=y|y_i=y) }{\text{card}(\hat{y}_i=y|y_i=y)+\text{card}(\hat{y}_i=y|y_i\neq y)}$$

$$ R_{el} = 0.25 \sum \frac{ 	\text{card} (\hat{y}_i=y|y_i=y) }{\text{card}(\hat{y}_i=y|y_i=y)+\text{card}(\hat{y}_i\neq y|y_i = y)}$$

$$ F_{el} = \frac{2}{P_{el}^{-1}+R_{el}^{-1}} $$

Метрики для оценки качества выявления дефектов выглядит следующим образом:

$$ P_{defect} = \frac{ 	\text{card} (\hat{y}_i=1|y_i=1) }{\text{card}(\hat{y}_i=1|y_i=1)+\text{card}(\hat{y}_i=1|y_i\neq 1)}$$

$$ R_{defect} = \frac{ 	\text{card} (\hat{y}_i=1|y_i=1) }{\text{card}(\hat{y}_i=1|y_i=1)+\text{card}(\hat{y}_i\neq 1|y_i = 1)}$$

$$ F_{defect} = \frac{4}{3R_{defect}^{-1}+0.9 \min(P_{defect},0.9)^{-1}} $$

Итоговая метрика, по которой оценивается решение:
$$Q=\frac{F_{el}+2F_{defect}}{3}$$

Рассмотрены различные подходы решения задачи:
- CatBoost
- CNN
- U-Net
- DeepLabV3
- SegFormer

### Результаты

|   | $F_{el}$  | $F_{defect}$  | $Q$  |
|:-:|:-:|:-:|:-:|
| CatBoost  |  0.68 | 0.18  | 0.35  |
| CNN  | 0.88  | 0.34  |  0.52 |
| U-Net  |  0.95 |  **0.62** |  **0.73** |
| DeepLabV3  | **0.97**  | 0.6  |  0.72 |
| SegFormer  |  0.92 |  0.48 | 0.63  |


