# map-game

| Дата выдачи | Deadline         | Folder name   | Branch name   |
| ------------| ---------------- | ------------- | ------------- |
| 06.04.2021  | 12.04.2021 23:59 | map-game      | map-game      |

В первую очередь ознакомьтесь с [инструкцией к заданию](tasks/js-projects.md) 

## Task 4. map-game

![screenshot](images/map-game.png)

![screenshot](images/map-game2.png)

[Демо](https://www.geoguessr.com/free)

В ходе выполнения задания вам необходимо создать приложение - игру с географической картой. Игра начинается с того, что на странице отображается панорама рандомной точки планеты. Нужно на карте кликнуть на точку, где по вашему мнению находится это место. Отображается расстояние между точкой, по которой кликнули, и реальным положением места. Ведётся статистика лучших результатов.

## Структура и работа приложения
- на странице отображается 3D панорама рандомной точки планеты, занимающая весь экран. При перезагрузке страницы или клике по кнопке New game будет отображаться другая панорама.
- в углу страницы отображается карта мира. При наведении курсора окно с картой увеличивается. Карта интерактивная - её можно увеличивать, уменьшать, перетаскивать. На карте можно выбрать точку и кликнуть по ней. После выбора точки на карте становится активной кнопка Guess
- при клике по кнопке Guess 3D панорама скрывается, карта увеличивается во весь экран. На карте отображаются две точки - та, по которой кликнули, и координаты места, где снималась панорама. Отображается расстояние между этими точками в километрах. 
- ведётся статистика лучших 10 результатов в формате - дата и время игры, расстояние между точками в километрах. Для хранения статистики на компьютере пользователя используется local storage
- есть кнопка Fullscreen при клике по которой можно развернуть приложение во весь экран. Повторный клик по кнопке выводит приложение из полноэкранного режима. В зависимости от того, находится приложение в обычном или полноэкранном режиме, меняется иконка на кнопке

## Критерии оценки

**Максимальный балл за задание +50**
- на странице отображается 3D панорама рандомной точки планеты +10
- фото для панорамы получаем от Flickr API +10
- на странице есть интерактивная карта на которой кликом можно выбрать точку +10
- на карте отображаются две точки и расстояние между ними +10
- ведётся статистика лучших результатов +5
- возможность развернуть приложение во весь экран +5

## Ключевые навыки
- работа с API
- работа с картой
- работа с библиотеками

## Советы по выполнению задания
1. Google Street View для создания панорам нам не подходит, ресурс платный, а на курсе могут использоваться только бесплатные ресурсы, не требующие подключения платёжных документов. Поэтому панорамы будем делать сами (P.S. может и есть где-то бесплатная альтернатива Google Street View, но как-то не нашлась. Яндекс Панорамы разве что можно посмотреть)

2. 3D панорама на JavaScript реализуется библиотекой GSAP
- [Статья](https://atuin.ru/blog/3d-panorama-na-js/)
- [Демо на codepen](https://codepen.io/creativeocean/pen/wvMRVxE)
- [О библиотеке](http://odinokun.com/2020-02-07-kak-sdelat-animaciyu-v-vebe-s-pomoshyu-greensock.html)
- [Документация](https://greensock.com/)

3. Где взять фото для панорам? Собирание коллекции фото вручную не научит программированию. Будем использовать Flickr API. У Flickr не всегда очевидная и понятная документация, зато большая коллекция фото.
- https://www.flickr.com/services/
    - регистрируемся на сайте
    - подтверждаем email (переходим по ссылке, которая пришла на почту)
    - создаём приложение `https://www.flickr.com/services/apps/create/apply/`
    - получаем API Key
    - [flickr.photos.search](https://www.flickr.com/services/api/flickr.photos.search.html)
    - получаем список фото `https://www.flickr.com/services/rest/?method=flickr.photos.search&api_key=0f15ff623f1198a1f7f52550f8c36057&has_geo=1&tags=panorama,country,landscape,geography&extras=url_h,url_o,geo&format=json&nojsoncallback=1`
    - в данном запросе
      - has_geo=1 - у фото есть геотеги (долгота и широта) и они не нулевые
      - tags=panorama,country,landscape,geography - слова для запроса, можно указать другие
      - extras=url_h,url_o,geo - в описании фото есть его размеры в большом (url_h) и очень большом (url_o) размере, а также геотеги - latitude и longitude
    - Для отображения фото на панораме выбираем фото в горизонтальном формате - ширина больше высоты, указываем минимальное и максимальное значение размера фото.

4. Работа с асинхронным запросом

JS-код для получения ссылки на изображение и координат где его сняли (данные выводятся в консоль, но такой код работает только на сервере, LiveServer vscode подойдёт)
```js
getFlickrImage = async () => {
    const url = `https://www.flickr.com/services/rest/?method=flickr.photos.search&api_key=0f15ff623f1198a1f7f52550f8c36057&has_geo=1&tags=panorama,country,landscape,geography&extras=url_h,url_o,geo&format=json&nojsoncallback=1`;
    const res = await fetch(url);
    const data = await res.json();
    console.log(data.photos.photo[0].url_h, data.photos.photo[0].latitude, data.photos.photo[0].longitude)
  }
```

5. GoogleMaps платные. Очень хорошая бесплатная альтернатива - Mapbox
- https://www.mapbox.com
  - регистрируемся на сайте  
    `https://account.mapbox.com/auth/signup/`
  - подтверждаем email (переходим по ссылке, которая пришла на почту)
  - получаем Access token  
    `https://account.mapbox.com/`
  - выбираем понравившийся дизайн  
    `https://docs.mapbox.com/mapbox-gl-js/examples/`
  - [API Docs](https://docs.mapbox.com/api/maps/)
- [Получить координаты клика по карте Mapbox](https://stackoverflow.com/questions/63158744/display-lat-lng-coordinates-on-click-on-mapbox-gl-js)
- [Провести линию и найти расстояние между двумя точками на карте Mapbox](https://docs.mapbox.com/mapbox-gl-js/example/measure/)


## Материалы:
[Документ для вопросов](https://docs.google.com/spreadsheets/d/1dMDLBC4-1XPaVMehZB6DqetToXZhq4x0PiZtj-jvLRc/edit#gid=1785130185)

## Cross-check
- инструкция по проведению cross-check: https://docs.rs.school/#/cross-check-flow
- ссылки на лучшие работы, добавьте, пожалуйста, в эту форму [https://forms.gle/T1VvyfMVNrFnFcR37](https://docs.google.com/forms/d/e/1FAIpQLSdVt05mFxh7oL6gEyl4-NQ19pN8tGU4QNqp71w3vo5Uxs89mg/viewform?usp=sf_link)