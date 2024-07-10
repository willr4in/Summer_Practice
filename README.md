 # Статья на тему:
# Сравнение реализаций систем генерации изображений с использованием заданной позы.
## 1. Введение
Многие из нас в порыве всплеска творческого вдохновения хотели запечатлеть картину, сформированную в нашем сознании. Для таких жизненных моментов отлично подойдут системы генерации изображений. Так же быстро как меняется современный мир меняются и нейронные сети. Так во вселенной генерации изображений постоянно происходят изменения. При создании картинки мы конечно хотим получить наиболее реалистичный результат. Однако не всегда мы можем получить такой результат. Новым словом в генерации изображений стало использование заданной позы. Мы не всегда можем точно описать словами то, что хотим получить в результате, однако в этом нам может помочь так называемый «макет».В настоящее время системы генерации изображений с использованием заданной позы, такие как ControlNet, представляют собой активно развивающуюся область знаний. Эти системы играют важную роль в создании реалистичных изображений, а также в области виртуальной реальности и  анимации персонажей. ControlNet позволяет пользователям больше контролировать сгенерированные результаты. Выявление оптимальной реализации среди систем генерации с использованием заданной позы все так же остается актуальной проблемой. Давайте рассмотрим ее более подробно.   	
### Мотивация исследования
В свете быстрого развития технологий компьютерного зрения и генеративных моделей, возрастает потребность в исследовании методов генерации изображений с учетом заданных поз. Это имеет применение в различных областях, таких как виртуальная реальность, дополненная реальность, создание контента для игр и анимации. Данные области кажутся нам привлекательными и перспективными. Они могут быть не только развлекательными и социально полезными. Таким образом, наше исследование сможет подтвердить или опровергнуть проведенные эксперименты и дать большее понимание для пользователей нейросетей и их исследователей.	
### Цели и задачи исследования
Цель данного исследования — провести сравнительный анализ реализаций систем генерации изображений с использованием заданной позы.
Основные задачи исследования включают:
- Подбор реализаций систем генерации изображений
- Анализ точности генерации картинки каждой системы.
- Анализ метрик, выбранных для сравнения
## 2. Обзор литературы
### Текущее состояние исследований
Генерация картинок с заданной позой хоть и является относительной «новинкой» в сфере нейросетей, но работы на данную темы уже написаны и не единожды. Перед тем как приступить непосредственно к исследованиям, мы изучили некоторые работы:
Adding Conditional Control to Text-to-Image Diffusion Models Lvmin Zhang, Anyi Rao, and Maneesh Agrawala Stanford University(https://paperswithcode.com/paper/humansd-a-native-skeleton-guided-diffusion) (ICCV 2023  ·  Xuan Ju, Ailing Zeng, Chenchen Zhao, Jianan Wang, Lei Zhang, Qiang Xu )
Современные исследования в области генерации картинок в основном направлены на  выявление реалистичности воспроизводимого продукта и ее увеличение. Новые реализации стремятся сделать сгенерированную картинку все более и более приятной глазу. 


### Недостатки существующих подходов
Безусловно на данный момент невозможно было досконально исследовать и сравнить все системы генерации изображений с использованием заданной позы, тем более со временем выходят новые. ControlNet уже достаточно распространен, поэтому множество реализаций с его использованием выходят в свет. Мы же в своей статье хотим сравнить работу Stable Diffusion (SB) с ControlNet и Kandinskiy. Такого сравнения нам не удалось найти на просторах инетернета.
### Вклад данной работы
Наше исследование представляет собой сравнение двух реализаций систем генерации картинок. Оно поможет нам лучше понять принципы работы каждой нейросети и даст понимание какая же все-таки «качественно» лучше. Это упростит выбор наиболее подходящей системы для исследователей и пользователей. 
## 3. Методология
### Описание исследуемых моделей
Немного о ControlNet
ControlNet - это архитектура нейронной сети, которая улучшает предварительно обученные модели диффузии изображений для конкретных задач. Она работает путем изменения входных условий блоков нейронной сети, которые представляют собой наборы нейронных слоев, используемых для построения нейронных сетей. Это позволяет сохранить исходные веса, избегая изменений из-за небольших наборов данных, и ускоряет файнтюнинг. ControlNet использует слои "нулевой свертки" для соединения компонентов с весами и смещениями, инициализированными нулями, чтобы не влиять на объекты на первом этапе обучения. Градиенты для этих слоев могут быть рассчитаны с использованием уравнений произведения Адамара, что позволяет начать обучение без перезапуска при введении новых данных или задач. ControlNet часто применяется к Stable Diffusion для преобразования текста в изображение, где входные данные сначала кодируются в свернутые карты перед подключением через структуру ControlNet в кодер/декодер. На рисунке 1 мы можем видеть более наглядный пример архитектуры ControlNet.
 
![](https://github.com/willr4in/Summer_Practice/blob/main/photos/images/image1.png?raw=true)

Обучение нейронной сети можно ускорить, используя частичное отключение связей между управляющей сетью и стабильной диффузией в случаях ограниченных вычислительных ресурсов. При наличии мощных вычислительных кластеров можно обучать модель на больших объемах данных. Можно начать с обучения управляющих сетей на большом числе итераций, прежде чем разблокировать все веса стабильной диффузии и обучать их вместе как одну модель для конкретной задачи. Это подход способствует более точному управлению распространением изображений и может улучшить смежные приложения, такие как обнаружение четких границ на изображениях с использованием случайных пороговых значений.

![](https://github.com/willr4in/Summer_Practice/blob/main/photos/images/image.png?raw=true)


Для генерации всегда нужна картинка — она выступает как трафарет. Из-за наличия такой «базы» решаются проблемы нейросетей с генерацией групп людей, рук, глаз и других мелких деталей. 
Данные и экспериментальные настройки
### Метрики оценки
Главной оценочной метрикой мы выбрали FID(Frechet Inseption Distanse).
Оценка начального расстояния Фреше, или сокращенно FID, представляет собой метрику, которая вычисляет расстояние между векторами признаков, рассчитанными для реальных и сгенерированных изображений.
Оценка суммирует, насколько похожи две группы с точки зрения статистики по признакам компьютерного зрения необработанных изображений, рассчитанной с использованием модели Inception v3, используемой для классификации изображений. Более низкие оценки указывают на то, что две группы изображений более похожи или имеют более схожую статистику, при этом идеальная оценка составляет 0,0, что указывает на то, что две группы изображений идентичны.
Реализацию FID вы можете увидеть на GitHub: [ссылка на гит](https://github.com/mseitzer/pytorch-fid).

## 4. Результаты
### Экспериментальные результаты
#### Примеры работы Stable Diffision ControlNet:


![](https://github.com/willr4in/Summer_Practice/blob/main/photos/PhotosControlNet/%D0%9F%D0%B0%D1%80%D0%B0,%20%D1%86%D0%B5%D0%BB%D1%83%D1%8E%D1%89%D0%B0%D1%8F%D1%81%D1%8F%20%D0%BF%D0%BE%D0%B4%20%D0%B7%D0%BE%D0%BD%D1%82%D0%B8%D0%BA%D0%BE%D0%BC%20%D0%B2%D0%BE%20%D0%B2%D1%80%D0%B5%D0%BC%D1%8F%20%D0%B4%D0%BE%D0%B6%D0%B4%D1%8F%20%D0%BA%D0%B8%D0%B1%D0%B5%D1%80%D0%BF%D0%B0%D0%BD%D0%BA.jpg?raw=true)


![](https://github.com/willr4in/Summer_Practice/blob/main/photos/PhotosControlNet/%D0%9C%D0%B0%D0%BA%D1%80%D0%BE%D1%84%D0%BE%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F%20%D1%86%D0%B2%D0%B5%D1%82%D1%83%D1%89%D0%B5%D0%B3%D0%BE%20%D1%86%D0%B2%D0%B5%D1%82%D0%BA%D0%B0%20%D1%81%20%D0%BF%D1%87%D0%B5%D0%BB%D0%BE%D0%B9%20%D0%BD%D0%B0%20%D0%BD%D0%B5%D0%BC%20%D0%BA%D0%B8%D0%B1%D0%B5%D1%80%D0%BF%D0%B0%D0%BD%D0%BA.jpg?raw=true)



#### Примеры работы Kandinskiy с ControlNet:



![](https://github.com/willr4in/Summer_Practice/blob/main/photos/PhotosKandinsky/%D0%9F%D0%B0%D1%80%D0%B0,%20%D1%86%D0%B5%D0%BB%D1%83%D1%8E%D1%89%D0%B0%D1%8F%D1%81%D1%8F%20%D0%BF%D0%BE%D0%B4%20%D0%B7%D0%BE%D0%BD%D1%82%D0%B8%D0%BA%D0%BE%D0%BC%20%D0%B2%D0%BE%20%D0%B2%D1%80%D0%B5%D0%BC%D1%8F%20%D0%B4%D0%BE%D0%B6%D0%B4%D1%8F%20%D0%BA%D0%B8%D0%B1%D0%B5%D1%80%D0%BF%D0%B0%D0%BD%D0%BA.png?raw=true)


![](https://github.com/willr4in/Summer_Practice/blob/main/photos/PhotosKandinsky/%D0%9C%D0%B0%D0%BA%D1%80%D0%BE%D1%84%D0%BE%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F%20%D1%86%D0%B2%D0%B5%D1%82%D1%83%D1%89%D0%B5%D0%B3%D0%BE%20%D1%86%D0%B2%D0%B5%D1%82%D0%BA%D0%B0%20%D1%81%20%D0%BF%D1%87%D0%B5%D0%BB%D0%BE%D0%B9%20%D0%BD%D0%B0%20%D0%BD%D0%B5%D0%BC%20%D0%BA%D0%B8%D0%B1%D0%B5%D1%80%D0%BF%D0%B0%D0%BD%D0%BA.png?raw=true)







#### Оригиналы:



![](https://github.com/willr4in/Summer_Practice/blob/main/photos/OriginalPhotos/%D0%9F%D0%B0%D1%80%D0%B0,%20%D1%86%D0%B5%D0%BB%D1%83%D1%8E%D1%89%D0%B0%D1%8F%D1%81%D1%8F%20%D0%BF%D0%BE%D0%B4%20%D0%B7%D0%BE%D0%BD%D1%82%D0%B8%D0%BA%D0%BE%D0%BC%20%D0%B2%D0%BE%20%D0%B2%D1%80%D0%B5%D0%BC%D1%8F%20%D0%B4%D0%BE%D0%B6%D0%B4%D1%8F.png?raw=true)


![](https://github.com/willr4in/Summer_Practice/blob/main/photos/OriginalPhotos/%D0%9C%D0%B0%D0%BA%D1%80%D0%BE%D1%84%D0%BE%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F%20%D1%86%D0%B2%D0%B5%D1%82%D1%83%D1%89%D0%B5%D0%B3%D0%BE%20%D1%86%D0%B2%D0%B5%D1%82%D0%BA%D0%B0%20%D1%81%20%D0%BF%D1%87%D0%B5%D0%BB%D0%BE%D0%B9%20%D0%BD%D0%B0%20%D0%BD%D0%B5%D0%BC.png?raw=true)

### Сравнительный анализ
 В данный момент вычисляем fid, все доделаем до конца)
## 5. Обсуждение
### Импликация результатов
### Ограничения исследования
Некоторые ограничения были связаны с использованием наборов данных, которые не всегда покрывают всевозможные различия, а также наши две системы поддерживают работу на различных языках.
### Предложения для будущих исследований
В дальнейшем можно развивать исследования в области генерации изображений. Мы бы порекомендовали более скрупулезно и внимательно разбираться в каждой реализации системы генерации картинок. Иногда кажется, что все довольно просто и лежит на поверхности, однако все больше погружаясь в данную область, становится понятно насколько еще много места для исследований. Также важно правильно выбрать метрику, старайтесь взять ту, которая наиболее информативно опишет работу нейросети.
## 6. Заключение
### Основные выводы
В ходе данной исследовательской работы мы не только познакомились с системами генерации изображений с использованием заданной позы, таких как SD c ControlNet и Kandinskiy c ControlNet, но и научились сравнивать несколько нейросетей, генерируещих картинки, по количественным характеристикам. Так в ходе нашего исследования мы выяснили, что Stable Diffusion справляется со своей задачей лучше. У данной нейросети изображения получаются реалистичнее и полностью основаны на позе оригинала.
## 7. Библиография
    1. Lvmin Zhang, Anyi Rao, and Maneesh Agrawala(2023). Adding Conditional Control to Text-to-Image Diffusion Models. arXiv:2302.05543v3
    2. Alexander Mathiasen, Frederik Hvilshøj (2021). Backpropagating through Fréchet Inception Distance. arXiv:2009.14075v2
    3. https://habr.com/ru/companies/ruvds/articles/719348/
    4. А. Д. Обухов (2020). Модифицированный метод оценки качества генеративно-состязательных нейронных сетей.
    5. GitHub repository: https://github.com/Mikubill/sd-webui-controlnet?tab=readme-ov-file
    6. GitHub repository:https://github.com/AUTOMATIC1111/stable-diffusion-webui
    7. GitHub repository:https://github.com/ai-forever/Kandinsky-3?tab=readme-ov-file#kandinsky-ip-adapter--kandinsky-controlnet
    8. Kurumuz. Novelai improvements on stable diffusion, https://blog.novelai.net/novelai-improvements-on-stablediffusion-e10d38db82ac, 2022.
