https://github.com/aristofun/webdevdao/blob/master/interview/rails.md

# Вопросы по Ruby on Rails с собеседований

1. Что такое ActiveRecord, и какие средства предоставляет для работы с объектами?

    <details>
      <summary>Ответ</summary>
      ActiveRecord это паттерн программирования. AR является популярным способом доступа к данным реляционных баз данных в объектно-ориентированном программировании. ActiveRecord еще называют буквой M в MVC — которая является слоем в системе, ответственным за представление бизнес-логики и данных.

      Active Record упрощает создание и использование бизнес-объектов, данные которых требуют персистентного хранения в базе данных. Сама по себе эта реализация паттерна Active Record является описанием системы ORM (Object Relational Mapping). Active Record это фреймворк ORM.

      * http://rusrails.ru/active-record-basics
      * https://dic.academic.ru/dic.nsf/ruwiki/1264999
    </details>

1. Как работает роутинг? Что такое ресурсные роуты? Как они формируются?

    <details>
      <summary>Ответ</summary>
      Браузеры запрашивают страницы от Rails, выполняя запрос по URL, используя определенный метод HTTP, такой как GET, POST, PATCH, PUT и DELETE.

      Роутинг распознает запрос по методу и по URL и направляет его в экшн контроллера или в приложение Rack.

      Он также может генерировать пути и URL, избегая необходимость жестко прописывать строки в ваших вьюхах.

      Ресурсный роутинг позволяет быстро объявлять все общие маршруты для заданного ресурсного контроллера. Вместо объявления отдельных маршрутов для экшнов `index`, `show`, `new`, `edit`, `create`, `update` и `destroy`, ресурсный маршрут объявляет их одной строчкой кода.

      http://rusrails.ru/rails-routing
    </details>

1. Что подразумевает собой 'convention over configuration'?
    <details>
        <summary>Ответ</summary>Обратите внимание, что у нас нет явного рендеринга в конце действия индекса в соответствии с принципом «соглашение важнее конфигурации». Правило состоит в том, что если вы не визуализируете что-то явно в конце действия контроллера, Rails автоматически ищет шаблон action_name.html.erb в пути представления контроллера и визуализирует его. Так что в этом случае Rails отобразит файл app/views/books/index.html.erb.
    </details>
1. Как использовать `content_for` и `yield`?
    <details>
        <summary>Ответ</summary>

        ```rb
          <div>
            <h1> This is the wrapper!</h1>
            <%= yield :my_content %>
          </div>
        ```
            
        content_for — это то, как вы указываете, какой контент будет отображаться в какой области layout
        
        ```rb
        <% content_for :my_content do %>
          This is the content.
        <% end %>
        ```
            
        Результат:
        
        ```rb
        <div>
          <h1> This is the wrapper!</h1>
          This is the content.
        </div>
        ```
            
        Они являются противоположными концами процесса рендеринга, где yield указывает, куда идет контент, а content_for указывает, что представляет собой фактический контент.
        Лучше всего использовать yield в layout и content_for во view.
    </details>
3. Что такое скоупы (scopes)? Как использовать?

    <details>
      <summary>Ответ</summary>

      Скоупы позволяют задавать часто используемые запросы, к которым можно обращаться как к вызовам метода в связанных
      объектах или моделях. С помощью этих скоупов можно использовать такие методы как where, joins и includes.
      Все методы скоупов возвращают объект `ActiveRecord::Relation`, который позволяет вызывать на нем
      дополнительные методы (такие как другие скоупы).

      Для определения простого скоупа мы используем метод scope внутри класса, передав запрос, который хотим запустить при вызове этого скоупа:

      ```rb
      class Article < ApplicationRecord
        scope :published, -> { where(published: true) }
      end

      ```
      Подробнее [тут](http://rusrails.ru/active-record-query-interface#scopes)
    </details>

1. Что такое ActiveJob? Когда его использовать?
    <details>
      <summary>Ответ</summary>
      Active Job - это фреймворк для объявления заданий и их запуска на разных бэкендах очередей. Эти задания могут быть чем угодно: от регулярно запланированных чисток до списаний с карт или рассылок.
      В общем, всем, что может быть выделено в небольшие работающие части и запускаться параллельно.

      Имеет встроенные адаптеры для планировщиков фоновых задач:

      * Sidekiq
      * Resque
      * Delayed Job
      * и т.д.

      [Rails docs en](https://edgeguides.rubyonrails.org/active_job_basics.html)

      [Rails docs ru](http://rusrails.ru/active_job_basics)
    </details>

1. Что такое Asset Pipeline?
    <details>
      <summary>Ответ</summary>
      Asset Pipeline (файлопровод) - фреймворк для соединения и минимизации, или сжатия ассетов JavaScript и CSS.
      Он также добавляет возможность писать эти ассеты на других языках и препроцессорах, таких как CoffeeScript, Sass и ERB.
      Это позволяет автоматически комбинировать ассеты приложения с ассетами других гемов.

      Первой особенностью файлопровода является соединение ассетов, что может уменьшить количество запросов, необходимых браузеру для отображения страницы.
      Браузеры ограничены в количестве запросов, которые они могут выполнить параллельно, поэтому меньшее количество запросов может означать более быструю загрузку вашего приложения.

      Второй особенностью файлопровода является минимизация или сжатие ассетов. Для файлов CSS это выполняется путем удаления пробелов и комментариев. Для JavaScript могут быть применены более сложные процессы. Можно выбирать из набора встроенных опций или определить свои.

      Третьей особенностью файлопровода является то, что он позволяет писать эти ассеты на языке более высокого уровня с дальнейшей прекомпиляцией до фактического ассета. Поддерживаемые языки по умолчанию включают Sass для CSS, CoffeeScript для JavaScript и ERB для обоих.

      [Rails docs en](https://guides.rubyonrails.org/asset_pipeline.html)

      [Rails docs ru](http://rusrails.ru/asset-pipeline)
    </details>

1. Что такое валидации? Как написать свои валидации? Для чего нужны валидации? Где применяются валидации? Примеры Валидаций.
    <details>
      <summary>Ответ</summary>
      Валидации используются, чтобы быть уверенными, что только проверенные данные сохраняются в вашу базу данных.
      Например, для вашего приложения может быть важно, что каждый пользователь предоставил валидный электронный и почтовый адреса.

      Валидации на уровне модели - наилучший способ убедиться, что в базу данных будут сохранены только валидные данные. Они не зависят от базы данных, не могут быть обойдены конечными пользователями и удобны в тестировании и обслуживании.
      Rails представляет простоту в обслуживании, представляет встроенные хелперы для общих нужд, а также позволяет создавать свои собственные методы валидации.

      Пример простейшей валидации передачу в модель `Person` данных из поля `name`:

      ```rb
      class Person < ApplicationRecord
        validates :name, presence: true
      end

      Person.create(name: "John Doe").valid? # => true
      Person.create(name: nil).valid? # => false
      ```

      Разработчик так же в праве написать свои собственные правила валидации, которые будут располагаться в каталоге `app/validators`.

      Пример кастомной валидации `email` по определенному пользователем шаблону:

      ```rb
      class EmailValidator < ActiveModel::EachValidator
        def validate_each(record, attribute, value)
          unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
            record.errors[attribute] << (options[:message] || "is not an email")
          end
        end
      end

      class Person < ApplicationRecord
        validates :email, presence: true, email: true
      end
      ```
      [Rails docs ru](http://rusrails.ru/active-record-validations)

      [Rails docs en](https://guides.rubyonrails.org/active_record_validations.html)
    </details>
1. Есть ли у Rails механизм, который отслеживает изменения в базе данных?
    <details>
      <summary>Ответ</summary>
      у нас paper trail, активируется для модели директивой has_paper_trail
    </details>

1. Что такое Haml, Slim? Какие плюсы на ваш взгляд, их использования?

    <details>
      <summary>Ответ</summary>
      Haml и Slim — это шаблонизаторы, используются для удобства использования и минимизации написания кода в представлениях. Сокращает в несколько раз написание кода, нет проблем в закрывании тегов, не получится что тег не закрыт и код не работает. Меньше вероятность что можно ошибиться + лучше читаемость в коде.

      http://slim-lang.com

      https://haml.ru
    </details>

1. Что означает несколько расширений файла example.html.erb?

    <details>
      <summary>Ответ</summary>

      **example** — название файла

      **html** — расширение, которое позволяет использовать стандартный язык разметки HyperText Markup Language

      **erb** — позволяет включить использование кода написанного на языке Ruby вместе с языком разметки
    </details>

1. Знать и рассказать структуру папок Rails приложения.

    <details>
      <summary>Ответ</summary>

       📂 app — основные файлы приложения
       └📁 assets — картинки, стили, js
       └📁 controllers — контроллеры
       └📁 helpers — хелперы
       └📁 jobs — задания
       └📁 mailers — рассыльщики
       └📁 models — модели
       └📁 views — представления
        └📁 layouts — макеты
       📂 config — конфигурация маршрутов, базы данных и т.д
        └📁 environments — настройки сред приложения
        └📁 locales — интернационализация
       📂 db — текущая схема базы данных, сиды
        └📁 migrates — файлы миграции
       📂 lib — внешние модули
       📂 log — журналы логов
       📂 public — доступна извне как есть, статичные файлы и скомпилированные ассеты
       📂 test — структурирована по тестам моделей / контроллеров / интеграционным
        └📂 fixtures — вспомогательные данные (фикстуры)
       📂 tmp — временные файлы (такие как файлы кэша и pid)
       📂 vendor — код сторонних разработчиков, например, внешние гемы.
        └📂 plugins — внешние плагины

      http://rusrails.ru/getting-started-with-rails#sozdanie-prilozheniya-blog
    </details>

1. Какие связи для связывания моделей в приложении Rails вы знаете?

    <details>
      <summary>Ответ</summary>
      Rails поддерживает шесть типов связей:

      * `belongs_to`
      * `has_one`
      * `has_many`
      * `has_many :through`
      * `has_one :through`
      * `has_and_belongs_to_many`

      http://rusrails.ru/active-record-associations#tipy-svyazey
    </details>

1. Привести примеры использования `has_many`, `belongs_to`, `has_and_belongs_to_many`, `has_one`, `has_many :through`?

    <details>
      <summary>Ответ</summary>
      Фильм имеет имеет множество сезонов, сезон принадлежит фильму и имеет множество серий. У каждого фильма может быть только один официальный сайт. В каждом фильме снимается множество актёров, при этом каждый актёр снимается в разных фильмах:

      ```rb
      class Film < ApplicationRecord
        has_many :seasons
        has_many :episodes, through: :seasons

        has_one :official_site
        has_and_belongs_to_many :actors
      end

      class Season < ApplicationRecord
        belongs_to :film
        has_many :episodes
      end

      class Episode < ApplicationRecord
        belongs_to :season
      end

      class OfficialSite < ApplicationRecord
        belongs_to :film
      end

      class Actor < ApplicationRecord
        has_and_belongs_to_many :films
      end
      ```

      http://rusrails.ru/active-record-associations#tipy-svyazey
    </details>

1. Что лучше выбрать `has_many :through` или `has_and_belongs_to_many`?

    <details>
      <summary>Ответ</summary>

      Это зависит от контекста связи `many-to-many`.

      Если планируется использование дополнительной логики в этой связи, создание дополнительных полей в соединительной таблице, то лучше отдать предпочтение `has_many :through`. В этом случае применяются промежуточные модели-связки.

      В том случае, если достаточно простой соединительной таблицы, то можно обойтись `has_and_belongs_to_many` (т.н. HBTM).

      http://rusrails.ru/active-record-associations#dopolnitelnye-metody-stolbtsov
    </details>

1. Что такое `i18n` (интернационализация)?

    <details>
      <summary>Ответ</summary>
      Адаптация приложения к особенностям региона, в котором он будет использоваться.

      Название `i18n` происходит от английского слова _internationalization_, между первой и последней буквами _i_ и _n_ 18 букв.

      Гем `i18n`, поставляемый с Ruby on Rails (начиная с Rails 2.2), представляет простой и расширяемый фреймворк для перевода приложения на язык, отличный от английского, а также изменения формата даты, времени, валюты и т.д.

      Rails автоматически добавляет все файлы `.rb` и `.yml` из директории `config/locales` к пути загрузки переводов.

      http://rusrails.ru/rails-internationalization-i18n-api
    </details>

1. Что такое `t.references`?

    <details>
      <summary>Ответ</summary>
      Столбец таблицы в миграции, указывающий на принадлежность к другой таблице. Например, книга принадлежит автору:

      ```rb
      class CreateBooks < ActiveRecord::Migration[5.2]
        def change
          create_table :books do |t|
            t.references :author
          end
        end
      end
      ```

      http://rusrails.ru/active-record-associations
    </details>

1. Что такое scaffolding? Зачем он используется и где применяется?

    <details>
      <summary>Ответ</summary>
      Rails Scaffold - встроенный генератор, который запускает другие генераторы Rails, чтобы одной командой сгенерировать набор из модели, контроллера, вьюх, тестов, миграций и т.д.
      Предоставляется возможность создавать собственные предустановки генерации.

      [Rails docs en](https://guides.rubyonrails.org/v3.2/getting_started.html#getting-up-and-running-quickly-with-scaffolding)
    </details>

1. Как реализовано кеширование в рельсах?

    <details>
      <summary>Ответ</summary>

      Кэширование означает хранение контента, генерируемого в цикле запрос-отклик, и повторное использование его при ответе на подобные запросы.
      Кэширование значительно загрузку страниц, снижает количество запросов к серверу.

      Виды кэширования:

      * Кэширование страницы — начиная с Rails 4 добавляется гемом `actionpack-page_caching`
      * Кэширование экшна — начиная с Rails 4 добавляется гемом `actionpack-action_caching`
      * Кэширование фрагмента — позволяет фрагменту логики вьюхи быть обернутым в блок кэша и обслуженным из хранилища кэша для последующего запроса
      * Кэширование матрешкой (Russian doll caching) — Можно вкладывать кэшированные фрагменты в другие кэшированные фрагменты. Eсли обновляется отдельный продукт, другие внутренние фрагменты могут быть повторно использованы при регенерации внешнего фрагмента.

      Rails также предоставляет другие виды кэширования

      Подробнее:

      [Rails docs ru](http://rusrails.ru/caching-with-rails-an-overview)

      [Rails docs en](https://guides.rubyonrails.org/caching_with_rails.html)
    </details>

1. Представьте, что есть огромная табл. `users`. Как можно перебрать ее элементы максимально быстро?

    <details>
      <summary>Ответ</summary>
      Быстро можно перебрать с помощью find_each, стандартно по 1000 записей.

      * `batch_size` — сколько обрабатывать записей за раз
      * `start` — с какого id к примеру продолжить работу
      * `finish` — может использоваться совместно с `start`, к примеру чтобы выслать письма только пользователям с первичным ключом от 2000 до 10000:

      https://apidock.com/rails/ActiveRecord/Batches/find_each

      http://rusrails.ru/active-record-query-interface
    </details>

1. Что такое Service Objects, Form Objects, View Objects, Query Objects, для чего они нужны?

    <details>
      <summary>Ответ</summary>
      Это обычные классы Ruby, которые применяются для рефакторинга Rails-приложения, инкапсулируя часть логики моделей / представлений / контроллеров.

      Service Objects, например, используются, когда одновременно задействованы несколько моделей, когда производятся сложные действия с моделями.

      Form Objects используются, когда отправка одной формы изменяет несколько моделей.

      View Objects используются, например, когда большой метод внутри модели используется только отображения данных.

      Query Objects используются для сложных SQL запросов, утяжеляющих модели/контроллеры.

      https://habr.com/ru/post/158011/
    </details>

1.  Чем отличается after_save от after_commit? Почему это плохие паттерны?
    <details>
      <summary>Ответ</summary>
        
      * after_save is invoked when an object is created and updated
      * after_commit is called on create, update and destroy - if after_commit raises an exception, then the transaction won't be rolled back
      * after_create is only called when creating an object
        
    </details>
1.  Есть 2 версии приложения и в новой версии мы хотим поменять у уже существующей колонки значения на дефолтные, но чтоб в старом приложении все работало со старыми значениями (бд одна само собой). Как ты это будешь делать? Какую миграцию напишешь?

1.  Пустое приложение, как ты сделаешь регистрацию пользователя и отправку ему имейла сразу после регистрации? Доп вопрос - как гарантировать, что мы юзеру отошлем только 1 имейл?

Где искать ответы:

* http://guides.rubyonrails.org/
* http://rusrails.ru/
* http://ruby-doc.org
