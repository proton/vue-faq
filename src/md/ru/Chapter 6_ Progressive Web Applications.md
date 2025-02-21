# Прогрессивные веб-приложения

В этой главе мы познакомимся со следующим шагом эволюции веб-приложений:
**прогрессивные веб-приложения** (**PWA**). Этот термин может показаться
недостаточно описательным, но он относится к группе технологий, которые
создают общую концепцию и могут быть реализованы постепенно или
частично. Основная идея заключается в том, чтобы вывести веб-приложение
из контекста браузера и реализовать его на любом типе устройства, чтобы
оно действовало и вело себя максимально похоже на нативное приложение.
Это достигается за счет внедрения новых API в браузерные движки, а также
интеграции с наиболее популярными операционными системами для настольных
и мобильных устройств. Отправной точкой для PWA, конечно же, является
**одностраничное** **приложение**<span class="No-Break"> (</span><span
class="No-Break">**SPA**</span>

).

В конце статьи мы рассмотрим, что такое PWA.

К концу этой главы мы узнаем следующее:

-   Что превращает SPA в PWA, и какие технологии при этом используются
-   Как вручную реализовать отзывчивый SPA, файл манифеста, рабочие
    службы, автономное хранилище и так далее
-   Что такое *работники сервисов*
    и какие они бывают
-   Как использовать плагины Vite для автоматизации создания PWA
-   Как проверить готовность приложения с помощью *Google Lighthouse*

Из предыдущего списка мы сосредоточимся на изучении "строительных лесов"
для нескольких технологий, закладывающих основу для их последующего
использования, подробно реализованного в [*главе
7*](https://learning.oreilly.com/library/view/vue-js-3-design/9781803238074/B18602_07.xhtml#_idTextAnchor173),
*Управление потоками данных*, и [<span class="No-Break">*главе
8*</span>](https://learning.oreilly.com/library/view/vue-js-3-design/9781803238074/B18602_08.xhtml#_idTextAnchor186),
*Многопоточность с Web Workers*. К концу этих глав вы будете знать, как
создавать PWA, которые эффективно используют современные вычислительные
мощности, делая их отзывчивыми, надежными,

и производительными.

Технические требования

Для работы с этой главой вам понадобятся примеры кода, расположенные в
репозитории по адресу
<https://github.com/PacktPublishing/Vue.js-3-Design-Patterns-and-Best-Practices/tree/main/Chapter06>.
Текстовых примеров кода в этом разделе может быть недостаточно для
создания работающего примера без дополнительного кода из репозитория.

Посмотрите следующее видео, чтобы увидеть код в действии:

https://packt.link/SBZys

.

## PWA или устанавливаемые SPA

PWA - это не отдельная настройка или технология, а систематическое
усовершенствование веб-приложения для соблюдения определенных условий,
будь то **многостраничное приложение** (**MPA**) или SPA. Однако
по-настоящему они проявляются и оживают, когда эти технологии
применяются к SPA, давая нам мощные приложения, которые стирают грань
между онлайном и оффлайном, настольными и веб-приложениями. Термин
**прогрессивный**, используемый здесь, имеет тот же оттенок, который мы
обсуждали ранее применительно к фреймворку Vue, - постепенное применение
веб-технологий.

PWA - это то, что нужно для создания новых приложений.

PWA как-то по-особому воспринимаются браузерами и операционными
системами. Они могут устанавливаться рядом с "родными" или настольными
приложениями и управлять сетевыми коммуникациями (отправлять, получать,
кэшировать файлы и даже получать push-уведомления с сервера). Здесь
важно отметить, что речь идет уже не только о настольных компьютерах, но
и о мобильных устройствах, таких как планшеты и телефоны, а также о
различных операционных системах. Именно в связи с такой
многоплатформенностью, если предполагается охватить базу пользователей
на различных устройствах, необходимо уделить особое внимание
использованию специальных правил CSS для адаптации пользовательского
интерфейса к различным размерам (так называемые **резонансные
приложения**), различных иконок и цветов для согласования с локальными
настройками пользователя на уровне операционной системы (например,
светлый и темный режимы) и т.д. Кроме того, PWA имеют возможность (как и
SPA) хранить контент для автономного использования и, надеюсь, должны
предоставлять определенную функциональность для автономного
использования. Для выполнения всего этого, как минимум, PWA должен
соответствовать следующим требованиям:

-   Веб-приложение должно обслуживаться через защищенное соединение
    (HTTPS).
-   Приложение должно предоставлять файл манифеста.
-   Приложение должно предоставить и установить рабочий сервис.

При выполнении всех этих условий браузер или операционная система могут
предложить пользователю "установить" приложение. Если пользователь
соглашается, то с помощью файла манифеста настраивается внешний вид
приложения в соответствии с локальной операционной системой (значки,
названия, цвета и т.д.), и оно появляется рядом с другими приложениями в
системе. При запуске оно будет открываться в собственном окне (если оно
выбрано) вне рамок веб-браузера, как и обычное "родное" приложение.
Внутри приложение по-прежнему будет работать на движке браузера с
использованием веб-технологий, но предполагается, что это будет
прозрачно для пользователя, обеспечивая лучшее из двух миров. Есть
вероятность того, что пользователь, сам того не зная, использует PWA
вместо обычных приложений. Успешными примерами такого подхода являются
Starbucks, Trivago и Tinder ([<span
class="No-Break">https://medium.com/@addyosmani/a-tinder-progressive-web-app-performance-case-study-78919d98ece0</span>](https://medium.com/@addyosmani/a-tinder-progressive-web-app-performance-case-study-78919d98ece0)

)

.

Это создает целый ряд преимуществ, которые перекрывают сложности
создания веб-приложения под различные сценарии установки:

-   Единая кодовая база для установки приложения на различные устройства
    (настольные, мобильные, ...) и операционные системы (Windows, Linux,
    macOS, Android, iOS и так далее)
-   Поддерживают push-уведомления с сервера, ручную обработку
    кэширования, автономное использование и так далее
-   Они интегрируются с локальной операционной системой
-   Обновления прозрачны для пользователя и происходят гораздо быстрее,
    чем в традиционном приложении (в большинстве случаев)
-   Разработка PWA требует гораздо меньших затрат, чем создание
    аналогичных целевых индивидуальных приложений для каждой платформы
-   Вы можете использовать все доступные веб-технологии, фреймворки и
    библиотеки
-   Могут индексироваться поисковыми системами, а их распространение и
    установка не зависят от проприетарных магазинов приложений
-   Отзывчивые приложения
-   Отзывчивые, безопасные и быстрые, ими можно поделиться с помощью
    одной лишь ссылки
-   Вы можете обращаться к локальным устройствам с помощью стандартных
    веб-интерфейсов API, например к локальной файловой системе и
    USB-устройствам, использовать аппаратное ускорение графики и т.д.
-   Вы можете работать с локальными устройствами с помощью стандартных
    веб-интерфейсов API.
-   Некоторые фирменные магазины приложений позволяют переупаковывать
    PWA и распространять его как обычное приложение (Microsoft Store,
    Amazon Store, Android Store и др.)

Есть и другие преимущества, но этих, пожалуй, достаточно, чтобы привести
их в качестве аргумента. Кроме того, в SPA проще добавить необходимые
элементы, чтобы превратить его в PWA. В результате PWA могут показаться
"серебряной пулей" среди приложений, однако следует учитывать и
некоторые недостатки:

.

-   Производительность PWA хороша, но в некоторых конкретных сценариях
    она всегда будет отставать от "родного" приложения. То же самое
    может произойти и на устаревшем оборудовании - они будут работать,
    но производительность может пострадать.
-   Устройства Apple немного отстают в освоении некоторых веб-технологий
    или специально ограничивают их применение для PWA (например,
    серверные push-уведомления).
-   Необходимо приложить немного больше усилий, чтобы охватить различные
    сценарии работы пользователей на разных устройствах (но немного
    больше, чем для обычного отзывчивого веб-приложения).
-   Некоторые магазины приложений не допускают PWA (в частности, на
    момент написания статьи это Apple App Store). Кроме того, приложение
    не получит выгоды от экспозиции и *пешеходного трафика* из магазина
    приложений.
-   

В целом, преимущества значительно перевешивают недостатки. По мере
развития веб-технологий PWA будут получать все больше преимуществ и
становиться все более распространенными. Теперь, имея более полное
представление о том, что такое PWA и что он может делать, давайте
модернизируем наши SPA в PWA.

Увеличение SPA до уровня PWA

.

Первое требование, о котором уже говорилось, - это обслуживание
приложения по защищенному соединению. Как это сделать, установив на
сервере бесплатный SSL-сертификат с помощью **Let's Encrypt**, мы
рассмотрим в [*главе
10*](https://learning.oreilly.com/library/view/vue-js-3-design/9781803238074/B18602_10.xhtml#_idTextAnchor224),
*Развертывание приложения*. Учитывая это, давайте посмотрим, как
выполнить

другие требования.

### Файл манифеста

Добавление файла манифеста - это отправная точка для превращения нашего
приложения в PWA. Это не что иное, как J<span
id="_idTextAnchor155"></span>SON-файл с известными полями, которые
указывают браузеру или операционной системе, как приложение должно быть
установлено на настольном или мобильном устройстве. Этот файл должен
быть связан в секции **head** нашего файла **index.html**, и хотя он
может иметь произвольное название, принято использовать имя
**manifest.json** или **app.webmanifest**. Официальная спецификация
предлагает расширение **.webmanifest**, но при этом уточняет, что имя не
имеет особого значения, если файл принимается правильно с типом
**application/manifest+json -** **Multipurpose Internet Mail
Extensions** (**MIME**) (см. <https://www.w3.org/TR/appmanifest/>,
раздел *§1.1.2*). В наших примерах кода мы будем использовать имя
**manifest.json** для простоты:

``` source-code
<link rel="manifest" href="/manifest.json">
```

.

Заметим из предыдущего кода, что файл размещается в корне нашего
приложения, а атрибут **rel** должен быть **manifest**. Атрибуты полей в
нашем файле манифеста могут располагаться в любом порядке, и все они
считаются *o<span id="_idTextAnchor156"></span> опциональными* согласно
вышеупомянутой спецификации. Однако некоторые платформы все же
предполагают минимальный набор атрибутов, который мы будем считать
*необходимым*. Обычная практика также требует наличия других
атрибутов<span id="_idTextAnchor157"></span>, которые мы будем относить
к *рекомендуемым*, и, наконец, некоторые атрибуты в спецификации часто
используются в магазинах приложений, социальных сетях и т.д. для
представления или описания приложения, поэтому мы будем относить их к
*описательным* полям. Эта классификация не является частью спецификации,
но может быть полезна при реализации. Вот список наиболее
распространенных и полезных атрибутов:

<table id="table001-4" class="No-Table-Style _idGenTablePara-1">
<tbody>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p>Классификация</p></td>
<td class="No-Table-Style"><p>Атрибут</p></td>
</tr>
<tr class="even No-Table-Style">
<td colspan="2" class="No-Table-Style"><p>Необходимо</p></td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>короткое_имя</strong></p></td>
<td class="No-Table-Style"><p>Короткое имя, используемое в тех случаях,
когда не хватает места для отображения полного имени приложения. В
мобильных устройствах часто используется для названия значка.</p></td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>имя</strong></p></td>
<td class="No-Table-Style"><p>Полное имя приложения.</p></td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>иконки</strong></p></td>
<td class="No-Table-Style"><p>Массив объектов, каждый из которых
представляет собой отдельную иконку, используемую в различных
контекстах. Каждый объект имеет как минимум два атрибута:</p>
<ul>
<li><strong>src</strong>: путь к изображению</li>
<li><strong>sizes</strong>: Строка с размерами изображения</li>
</ul></td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>start_url</strong></p></td>
<td class="No-Table-Style"><p>URL-адрес, с которого должно стартовать
приложение, заданный разработчиком.</p></td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>дисплей</strong></p></td>
<td class="No-Table-Style"><p>Строка, представляющая способ
представления приложения:</p>
<ul>
<li><strong>fullscreen</strong>: В полноэкранном режиме, но с
отображением пользовательского интерфейса браузера.</li>
<li><strong>standalone</strong>: Аналогично <strong>fullscreen</strong>,
но без элементов управления браузера. На рабочем столе элементы
управления windows все равно будут отображаться.</li>
<li><strong>minimal-ui</strong>: Как <strong>standalone</strong>, но с
базовой навигацией для перемещения вперед и назад, печати, обмена и
т.д.</li>
<li><strong>browser</strong>: Приложение открыто в браузере по
умолчанию.</li>
</ul></td>
</tr>
<tr class="even No-Table-Style">
<td colspan="2" class="No-Table-Style"><p>Рекомендуется</p></td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>цвет_темы</strong></p></td>
<td class="No-Table-Style"><p>Строка, представляющая собой цвет CSS для
приложения. ОС сама решает, как использовать это значение (обычно оно
применяется в строке заголовка окна).</p></td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>цвет_фона</strong></p></td>
<td class="No-Table-Style"><p>Строка, представляющая цвет фона
приложения при его запуске и до применения стилей приложения.</p></td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>ориентация</strong></p></td>
<td class="No-Table-Style"><p>В основном используется в мобильных
устройствах и определяет ориентацию, которую должно использовать
приложение - например, <strong>ландшафтная, портретная, любая</strong> и
т.д.</p>
.</td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>lang</strong></p></td>
<td class="No-Table-Style"><p>Строка, определяющая основной язык
приложения.</p></td>
</tr>
<tr class="odd No-Table-Style">
<td colspan="2" class="No-Table-Style"><p>Описательный</p></td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>короткие пути</strong></p></td>
<td class="No-Table-Style">Это массив объектов, определяющих пункты
прямого меню для тесной интеграции с операционной системой. Обычно они
появляются в контекстном меню, например, когда пользователь щелкает
правой кнопкой мыши на значке приложения. Каждый объект ярлыка должен
содержать как минимум <strong>имя</strong> и <strong>URL</strong>, а
также - опционально - <strong>описание</strong> и массив
<strong>иконок</strong>
.</td>
</tr>
<tr class="odd No-Table-Style">
<td class="No-Table-Style"><p><strong>описание</strong></p></td>
<td class="No-Table-Style"><p>Строка с кратким описанием
приложения.</p></td>
</tr>
<tr class="even No-Table-Style">
<td class="No-Table-Style"><p><strong>скриншоты</strong></p>
.</td>
<td class="No-Table-Style"><p>Массив объектов, содержащий следующие
поля:</p>
<ul>
<li><strong>src</strong>: URL-адрес изображения</li>
<li><strong>type</strong>: MIME-тип изображения</li>
<li><strong>sizes</strong>: Строка с размерами изображения</li>
</ul></td>
</tr>
</tbody>
</table>

Таблица 6.1 - Поля манифеста

.

На практике я бы рекомендовал заполнять необходимые и рекомендуемые поля
для каждого PWA, а описательные поля использовать по мере необходимости,
исходя из контекста приложения. Кроме того, следует изучить целевые
платформы на предмет наличия дополнительных поддерживаемых полей,
которые не входят в стандартную спецификацию.

Следуя предыдущей таблице, приведем пример файла **manifest.json**

:

``` source-code
{
      "short_name": "Пример PWA",
      "name": "Глава 6: Пример прогрессивного веб-приложения",
      "start_url":"/",
      "display": "standalone",
      "theme_color": "#2979FF",
      "background_color": "#000",
      "orientation": "portrait"
}
```

Как видите, создание файла манифеста не требует особых дополнительных
усилий и является простым дополнением к нашему SPA.

### Тестирование манифеста

После того как вы создали файл манифеста и связали его с файлом
**index.html**, вы можете использовать инструменты разработчика в
браузере, чтобы проверить, правильно ли он загрузился. Например, в
браузере Google Chrome в меню **Application** мы видим, что файл примера
загрузился правильно:

<div>

<div>

<div>

"Манифест".

<div>

<div>

<img src="images/Figure_6.01_B18602.jpg" width="1098" height="488"
alt="Рисунок 6.1 - Инструменты разработчика в Google Chrome, показывающий файл манифеста" />

</div>

</div>

Рисунок 6.1 - Инструменты разработчика в Google Chrome, показывающие
файл манифеста

.

Но есть еще одна тема, связанная с установкой приложения, которую мы
должны рассмотреть: когда и как пользователь узнает, что веб-приложение
может быть установлено? Здесь на помощь приходит *Install prompt*,
который мы рассмотрим далее.

### Подсказка к установке

Каждая платформа (мобильная или настольная) имеет свой собственный метод
определения того, когда PWA, удовлетворяющее критериям установки, может
быть установлено. Он может включать уведомление о том, что пользователь
должен согласиться на установку через определенное время, или
предоставлять только пользовательский интерфейс для этого. На мобильных
устройствах установленный PWA будет размещен на главном экране рядом с
другими собственными приложениями, а на настольных компьютерах он может
быть размещен внутри браузера и/или в главном меню. Кроме того, в
мобильных операционных системах, таких как Android, автоматически
создается заставка с указанными в манифесте темой, цветами фона и
иконкой приложения. Независимо от того, как и когда PWA может быть
установлено, важно знать, что это может быть сделано только с согласия и
по инициативе пользователя. Мы не можем запустить установку
автоматически из кода без согласия пользователя.

Основной поток установки PWA - это установка из кода.

Основной процесс установки выглядит следующим образом:

1.  Когда платформа обнаруживает, что наше приложение может быть
    установлено, она запускает событие в объекте окна под названием
    **beforeinstallprompt**. Мы можем кэшировать это событие, чтобы
    позже вызвать подсказку из нашего приложения.
2.  Пользователь инициирует установку.
3.  Пользователь инициирует установку либо через пользовательский
    интерфейс платформы, либо через наш метод, предоставляемый PWA
    (например, кнопку).
4.  Платформа предложит пользователю принять или отклонить установку.
5.  Если пользователь соглашается, он устанавливает PWA и запускает
    другое событие с именем <span
    class="No-Break">**appinstalled**</span>
    .

Это достаточно простой рабочий процесс. Однако событие
**beforeinstallprompt** срабатывает только один раз, поэтому, если
пользователь отказывается от установки, нам необходимо дождаться
повторного срабатывания этого события в браузере.

Теперь

Теперь<span id="_idTextAnchor162"></span>, когда мы поняли, как все
будет работать, пришло время посмотреть на это в коде. Рассмотрим, что в
шаблоне нашего компонента Vue 3 есть следующие элементы:

``` source-code
<p v-show="_install_ready && !_app_installed">
   Установите это приложение
   <button @click="installPWA()">Install</button>
</p>
<p v-show="_app_installed">
   Прогрессивное веб-приложение установлено
</p>
```

Как видите, у нас есть два параграфа, которые будут отображаться в
зависимости от значения реактивных переменных **\_install_ready** и
**\_app_installed**, обе они булевские. Первая появится, когда PWA будет
готов к установке, и предоставит кнопку для запуска установки с помощью
функции **installPWA()**. Второй будет отображаться после ее выполнения.

Наш код в секции скриптов также достаточно прост:

``` source-code
import { onMounted, ref, onBeforeUnmount } from 'vue'
const
    _install_ready=ref(false),
    _install_prompt=ref(null),
    _app_installed=ref(false)
// Определяем возможность установки PWA
onMounted(()=>{
     window.addEventListener("beforeinstallprompt",savePrompt)
    window.addEventListener("appinstalled",handleAppInstalled)})
function savePrompt(event){
    event.preventDefault(); // Предотвращает мобильную подсказку
     // Сохраняем ссылку на событие, чтобы активировать его позже
    _install_prompt.value=event;
     // Уведомить пользовательский интерфейс о том, что приложение может быть установлено
    _install_ready.value=true;
}
function installPWA(){
     // Запуск приглашения к установке
    if(_install_prompt.value){
         _install_prompt.value.prompt()
     }
}
function handleAppInstalled(){
    _install_prompt.value=null;
    _app_installed.value=true;
}
```

В предыдущем коде мы регистрируем два слушателя при установке нашего
компонента на страницу: один для управления и кэширования запроса на
установку, а другой - для определения того, когда приложение было
установлено. Некоторые части были опущены для упрощения кода, но полный
вариант компонента со стилями можно найти в репозитории GitHub.

Хотя в предыдущем коде мы регистрируем два слушателя при установке
компонента на страницу, один из них управляет кэшированием подсказки об
установке, а другой определяет момент установки приложения.

Хотя предыдущий пример является довольно упрощенным, существует
несколько известных шаблонов для продвижения или представления
возможности установки конечному пользователю. Все они основаны на одной
и той же логике - перехвате события и последующем pr<span
id="_idTextAnchor165"></span>омптировании его при показе триггерного
элемента. Реализация тривиальна и имеет больше отношения к дизайну, чем
к шаблону кодирования, поэтому мы <span id="_idTextAnchor166"></span>
рассмотрим здесь только макеты:

.

-   Простая кнопка **Установить** (как в нашем примере приложения):

<div>

<div>

<img src="images/Figure_6.02_B18602.jpg" width="480" height="106"
alt="Рисунок 6.2 - Простая кнопка установки" />

</div>

</div>

Рисунок 6.2 - Простая кнопка установки

-   Кнопка меню **Install** размещается в основной навигации:

<div>

<div>

<img src="images/Figure_6.03_B18602.jpg" width="480" height="137"
alt="Рисунок 6.3 - Кнопка установки главного меню" />

</div>

</div>

Рисунок 6.3 - Кнопка установки главного меню

-   Накладное уведомление:

<div>

<div>

<img src="images/Figure_6.04_B18602.jpg" width="480" height="137"
alt="Рисунок 6.4 - Накладное уведомление" />

</div>

</div>

Рисунок 6.4 - Накладное уведомление

-   Наложенный сверху элемент, например, баннер установки (либо перед
    заголовком, либо в нижней части области просмотра):

<div>

<div>

<img src="images/Figure_6.05_B18602.jpg" width="480" height="137"
alt="Рисунок 6.5 - Баннер подсказки по установке" />

</div>

</div>

Рисунок 6.5 - Баннер приглашения к установке

После того как приложение будет установлено, мы хотим предотвратить
постоянный запрос пользователя на установку. В этом случае рекомендуется
сохранить флаг автономности в **localStorage**, cookie на **indexeDB,**
или обозначить стартовый URL нашего приложения в определенном месте.
Варианты автономного постоянного хранилища мы <span
id="_idTextAnchor167"></span> рассмотрим в [*главе
7*](https://learning.oreilly.com/library/view/vue-js-3-design/9781803238074/B18602_07.xhtml#_idTextAnchor173),
*Управление потоками данных*. Теперь настало время рассмотреть последний
элемент, позволяющий превратить наш SPA в настоящий PWA:

рабочие службы.

## Рабочие службы

Рабочий сервис - это JavaScript-сценарий, который выполняется в
отдельном потоке, как фоновый процесс для вашего приложения. Он
действует как прокси для сети, перехватывая все вызовы и действуя в
соответствии с запрограммированной стратегией обслуживания страниц и
данных.

Мы можем иметь несколько рабочих служб, поскольку каждая из них отвечает
за свою область видимости. Область видимости определяется как каталог
(URL-путь), в котором находится исходный файл для сервисного работника.
Таким образом, сервис-рабочий, размещенный в корне приложения, будет
работать со всем SPA/PWA.

Сервисные рабочие устанавливаются без вмешательства пользователя,
поэтому их можно использовать, даже если пользователь не установил PWA.
Они имеют четко определенный жизненный цикл (см.
https://web.dev/service-worker-lifecycle/), инициируя события для
каждого завершенного состояния. Для начала сервис-рабочий должен быть
сначала *зарегистрирован*, затем он становится *активированным*, и, в
конце концов, мы можем также *снять с регистрации* его. После активации
сервисного работника он не будет брать на себя управление
взаимодействием с приложением до следующего обращения к сайту.

Самыми распространенными стратегиями программирования сервисного
работника являются следующие:

-   Обслуживать только кэш
-   Обслуживать только сеть
-   Попытаться сначала обслужить кэш, а затем вернуться к сети
-   Попробуйте сначала обслужить сеть, а затем вернуться к кэшу
-   Сначала обслуживать кэш, затем обновлять кэш

При рассмотрении стратегий кэширования и автономной работы нам
необходимо учитывать, какие файлы и активы, необходимые нашему
приложению для работы, будут изменяться мало или вообще не будут
изменяться, чтобы кэшировать их. Также необходимо определить маршруты,
которые никогда не следует кэшировать.

Чтобы использовать сервис-рабочий, мы регистрируем его в нашем файле
**main.js** следующими строками:

``` source-code
if(navigator.serviceWorker){
   navigator.serviceWorker.register("/service_worker.js")
}
```

В этих строках мы сначала проверяем, есть ли в текущем браузере
возможность использовать сервис-воркеры, и если есть, то регистрируем
его. Как мы видим, мы поместили рабочий в корень. Для данного примера мы
будем использовать стратегию "кэш - первый, сеть - обратный ход" вручную
для всех сетевых вызовов:

``` source-code
// Устанавливаем стратегию, сначала кэш, потом сеть.
const CACHE_NAME="MyCache"
self.addEventListener("fetch", event=>{
     // Перехватывает событие для реагирования
    event.respondWith((async ()=>{
     // Пытается найти запрос в кэше
    const found=await caches.match(event.request);
     if(found){
          return found;
     }else{
         // Не кэшированный фонт, возвращаемся к сети
         const response=await fetch(event.request);
         // Открываем кэш
         const cache=await caches.open(CACHE_NAME);
         // Поместить ответ сети в кэш.
         cache.put(event.request, response.clone());
         // Возврат ответа
          return response;
     }
  })())
})
```

Предыдущий код почти дословно основан на примере, приведенном в
документации **Mozilla Developer Network** по адресу
<https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Offline_Service_workers>.
Комментарии в коде помогут понять логику реализации стратегии. Однако
использование базовых API, доступных работнику сервиса, может оказаться
громоздким, а то и многословным. Вместо этого удобнее использовать
фреймворк или библиотеку для работы с ними и реализации более сложных
стратегий. Стандартом сегодня является использование **Workbox**,
созданного **Google** (https://developer.chrome.com/docs/workbox/). Мы
будем использовать его не напрямую, а через плагин для Vite, который мы
рассмотрим в следующем разделе.

После того как мы рассмотрели весь код, наш PWA готов к работе и
установке. Если мы запустим пример приложения на сервере разработки, то
увидим, что его можно установить. Используя пользовательский интерфейс
браузера или нашу кнопку **Install**, мы получим следующее сообщение:

<div>

<div>

<img src="images/Figure_6.06_B18602.jpg" width="758" height="200"
alt="Рисунок 6.6 - Запрос на установку PWA с localhost" />

</div>

</div>

Рисунок 6.6 - приглашение к установке PWA с localhost

Ручная адаптация нашего SPA для превращения его в PWA не является
сложной, но требует некоторой ручной работы. Однако с тем выбором
инструментов, который у нас есть, мы можем сделать это лучше. Существует
более простой способ сгенерировать и внедрить файл манифеста и рабочий
сервис в рамках рабочего процесса непосредственно в наш SPA: с помощью
плагина Vite.

Vite-PWA

## Плагин Vite-PWA

В экосистеме плагинов Vite есть отличный плагин Vite-PWA с нулевой
конфигурацией (<https://vite-pwa-org.netlify.app/>). Уже из коробки он
предоставляет нам отличную функциональность без особых усилий. Мы
устанавливаем плагин как зависимость разработчика с помощью следующей
команды в терминале:

``` console
$ npm install --save-dev vite-plugin-pwa
```

.

После установки необходимо зарегистрировать его в конфигурации Vite.
Измените

**vite.config.js** файл следующим образом:

``` source-code
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { VitePWA } from 'vite-plugin-pwa'
export default defineConfig({
plugins: [
vue(),
VitePWA({ {
    registerType: "autoUpdate",
    injectRegister: 'auto',
    devOptions: { enabled:true },
    рабочий блок: {
         globPatterns: ['**/*.{js,css,html,ico,png,svg}']
     }
    includeAssets:
               ['fonts/*.ttf','images/*.png','css/*.css'],
    manifest: {
          "short_name": "Пример PWA",
           "name": "Глава 6 - Пример прогрессивного веб-приложения",
           "start_url": "/",
           "display": "standalone",
           "theme_color": "#333333",
           "background_color": "#000000",
           "orientation": "портрет",
           "icons": [
             {
               "src": "/images/chapter_6_icon_192x192.png",
               "sizes": "192x192",
                "type": "image/png"
              }
              {
               "src": "/images/chapter_6_icon.png",
               "sizes": "512x512",
                "type": "image/png"
              }
              {
               "src": "/images/chapter_6_icon.png",
               "sizes": "512x512",
                "type": "image/png",
               "назначение": "маскируемый"
               }
           ],
            "prefer_related_applications": false
     }
  })]
})
```

Используя этот плагин, мы снимаем с бандлера бремя генерации рабочего
сервиса и веб-манифеста. Это необходимо, поскольку при каждой
промышленной сборке Vite будет генерировать разные имена файлов для
каждого скрипта в соответствии с нашей стратегией "ленивой" загрузки
компонентов, о которой мы говорили в предыдущей главе.

В приведенном примере мы видим, что в результате сборки скриптов Vite
будет генерировать разные имена файлов для каждого скрипта.

В предыдущем примере мы передаем в плагин **VitePWA()** объект с
некоторыми разумными опциями для автоматического создания и внедрения
манифеста и рабочего скрипта. Если нам нужен более тонкий контроль над
создаваемой стратегией рабочего сервиса, а также над веб-манифестом, то
можно использовать плагин в "режиме инъекции" и предоставить базовый
файл для нашего рабочего сервиса. В этом случае в скрипт будут
инжектироваться сгенерированные в процессе сборки файлы. Внизу плагин
использует **Workbox**, инструмент, о котором мы уже упоминали и который
мы можем настраивать непосредственно через поле **workbox**. Более
подробное рассмотрение различных реализаций и стратегий выходит за рамки
данной книги, но читателю следует обратиться к документации по плагину
**Vite-PWA** и **Workbox** для конкретных контекстов и случаев
использования.

Тестирование.

## Тестирование показателей PWA с помощью Google Lighthouse

В браузерах на основе хрома вместе с инструментами разработчика
поставляется утилита Lighthouse, специально предназначенная для
тестирования и оценки веб-страниц, а также готовности PWA. Чтобы
получить доступ к этому инструменту, после открытия своего приложения в
браузере выполните следующие действия:

1.  Откройте инструменты разработчика (нажав *F12* в Windows/Linux,
    *Fn* + *F12* в Mac, или через меню браузера).
2.  Выберите меню **Маяк** в правом верхнем углу.
3.  Выберите **Mobile** или **Desktop**, а также убедитесь, что отмечена
    категория **Progressive Web App**.
4.  Нажмите **Анализировать загрузку страницы** в правом верхнем углу
    инструмента.

Инструменты разработчика должны выглядеть примерно так:

<div>

<div>

<img src="images/Figure_6.07_B18602.jpg" data-маяк""="" width="921"
height="377" alt="Рисунок 6.7 - Утилита " />

</div>

</div>

Рисунок 6.7 - Утилита "Маяк"

.

Утилита проведет ряд тестов, и в каждой категории будет отображаться
рейтинг, а также подробный список элементов, которые прошли или не
прошли тест. Если наше приложение не соответствует критериям PWA, то в
пунктах, отмеченных красным цветом, будет указано, почему и как это
исправить:

.

<div>

<div>

<img src="images/Figure_6.08_B18602.jpg" width="904" height="472"
alt="Рисунок 6.8 - Рейтинги примера кода главы 6 в Lighthouse" />

</div>

</div>

Рисунок 6.8 - Оценки примера кода главы 6 в Lighthouse

.

Наш пример кода приложения полностью соответствует требованиям PWA и
успешно проходит все тесты. Конечно, этого легче добиться с небольшими
приложениями. На практике каждый рейтинг выше 90 является отличным.

## Обзор

В этой главе мы рассмотрели простой SPA и научились превращать его в PWA
как вручную, так и с помощью плагина в Vite. Пользователи могут
устанавливать PWA на свои платформы наряду с родными приложениями и
взаимодействовать с ними, даже если они не подключены к Интернету. PWA
обладают множеством преимуществ по сравнению с приложениями, работающими
только в Интернете. Мы также рассмотрели, как с помощью Lighthouse можно
измерить и оценить наше приложение в нескольких стандартных для отрасли
категориях. В этой главе мы закончили рассмотрение поэтапного создания
приложений с использованием веб-технологий и впредь будем уделять
основное внимание паттернам и моделям для повышения внутренней
производительности и эффективности.

Вопросы к обзору

## Вопросы для обзора

Для закрепления понятий, изученных в этой главе, ответьте на следующие
вопросы:

-   В чем разница между SPA и PWA?
-   Каковы преимущества PWA?
-   Каким основным трем требованиям должно соответствовать
    веб-приложение, чтобы считаться PWA?
-   Какие инструменты можно использовать для постепенной подготовки
    приложения к работе в качестве PWA?
-   Что такое рабочий сервис и каковы некоторые стратегии его
    использования?
-   Что такое веб-манифест и зачем он нужен?

</div>

</div>

</div>
