# nodejs-shpargalka
shpargalka - node.js  https://www.codementor.io/mattgoldspink/nodejs-best-practices-du1086jja
1.Запуск всех проектов с помощью npm init 
Большинство людей знакомы с NPM как способ установки зависимостей, но это намного больше, чем это. Во-первых, я настоятельно рекомендую создать новый проект, используя npm init, например

    $ mkdir my-new-project
    $ cd my-new-project
    $ npm init
    
Это создаст новый пакет.json для вас, который позволит вам добавить кучу метаданных, чтобы помочь другим, работающим над проектом, иметь ту же настройку, что и вы

Например, я обычно открываю package.json и добавляю определенную версию Node.js, над которой я планирую работать, добавив:

    "engines": {
      "node": "6.2.0"
    }

2. Настройка .npmrc 

Если вы использовали npm раньше, вы можете встретить флаг -save, который обновляет package.json с зависимостью. Из-за этого, когда другие разработчики клонируют проект, они могут быть уверены в правильности зависимостей. К сожалению, помнить о добавлении флага может быть проблемой.

Кроме того, NPM добавляет ведущую каретку ^ во все версии. Следовательно, когда кто-то запускает установку npm, они могут получать разные версии модулей, чем то, что у вас есть.

Несмотря на то, что обновление модулей modules всегда является хорошей практикой, команда разработчиков, работающая с несколькими разными версиями зависимостей, может привести к различиям в поведении или доступности API. Поэтому рекомендуется иметь всех в одной версии. Чтобы сделать это проще для всех, файл .npmrc имеет некоторые полезные свойства, которые могут гарантировать, что npm install всегда обновляет package.json и обеспечивает соответствие версии установленной зависимости точно.

Просто запустите следующие строки в терминале:

    $ npm config set save=true
    $ npm config set save-exact=true
    
Теперь, когда вы запускаете npm install, вы можете быть уверены, что зависимость сохранена и будет заблокирована до установленной вами версии.

3. Добавьте скрипты в свой пакет package.json

Если есть что-то, что нужно всем приложениям, это сценарий запуска.Зная, какой файл вызывать первым и какие аргументы могут быть эпическим приключением открытия в некоторых проектах.Хорошо, что NPM имеет стандартный способ запуска всех node приложений.
Просто добавьте свойство scripts и объект к вашему package.json с ключом запуска. Это значение должно быть командой для запуска вашего приложения. Например:

    "scripts": {
      "start": "node myapp.js"
    }
    
 Как только кто-то запускает npm start, NPM запустит node myapp.js со всеми зависимостями от node_modules / .bin на вашей $ PATH.
 Это означает, что вы можете избежать необходимости выполнять глобальные установки модулей NPM.
 
 Есть пара других скриптов, которые стоит знать:
 
 
     "scripts": {
      "postinstall": "bower install && grunt build",
      "start": "node myapp.js",
      "test": "node ./node_modules/jasmine/bin/jasmine.js"
    }
    
Сценарий postinstall запускается после запуска установки npm. Также есть preinstall, если вам нужно что-то запустить, прежде чем будут установлены все зависимости NPM.

Сценарий тестирования запускается, когда кто-то запускает проверку на npm. Это хороший простой способ, чтобы кто-то мог выполнять ваши тесты, не выясняя, выбрали ли вы использовать Jasmine, Mocha, Selenium и т. Д. Здесь вы можете добавить свои собственные скрипты. Затем их можно запустить с помощью npm run-script {name} - простой способ предоставить вашей команде центральный набор сценариев запуска.

4. Используйте переменные среды 

Управление конфигурацией всегда является большой темой на любом языке.
Как вы отделяете свой код от баз данных, сервисов и т. Д., Которые он должен использовать во время разработки, контроля качества и производства?

Рекомендуемым способом в Node.js является использование переменных среды и поиск значений из process.env в вашем коде.Например, чтобы выяснить, в какой среде вы работаете, проверьте переменные среды NODE_ENV:

    console.log("Running in :"  + process.env.NODE_ENV);
Теперь это стандартное имя переменной, используемое большинством поставщиков облачных хостингов.    
Если вам нужно загрузить дополнительные конфигурации, вы можете использовать модуль, например https://github.com/indexzero/nconf
Другой популярный вариант загрузки переменных среды https://github.com/motdotla/dotenv (Thanks to @szabi)

5. Используйте руководство по стилю


Я знаю, что у всех нас были моменты, когда мы впервые открываем новый файл из другого проекта или файл поступает от другого разработчика,мы затем проводим следующий час, переформатируя фигурные скобки, чтобы быть на разных линиях, изменяя пробелы на вкладки и наоборот.Проблема здесь - это смесь упрямых разработчиков и руководство по стандартным стилям команды / компании.

Это также уменьшает когнитивные издержки, следует ли писать письма с помощью вкладок или пробелов.Если стиль продиктован (и применяется с использованием JSHint, ESlint или JSCS), то все внезапно, кодовая база становится намного более управляемой.
Вам также не нужно выходить со своими собственными правилами, иногда лучше выбирать существующий набор рекомендаций и следовать им. Вот несколько хороших примеров:

Airbnb - https://github.com/airbnb/javascript
Google - https://google.github.io/styleguide/javascriptguide.xml
jQuery - https://contribute.jquery.org/style-guide/js/
Standard JS - http://standardjs.com/ - thanks to @szabi for pointing out this one 
Просто выберите один и придерживайтесь его!

6. Объявите async

Я уверен, что вы слышали всю шумиху о promis, возможно, даже немного слышали об async / await и генераторах в ES2016. Основная идея всех этих методов - сделать ваш код асинхронным.

Проблема с синхронными функциями в JavaScript заключается в том, что они блокируют запуск любого другого кода до его завершения.Однако синхронный код упрощает понимание потока логики приложения.С другой стороны, асинхронные структуры, такие как promis, действительно возвращают много таких рассуждений, сохраняя при этом ваш код без блокировок. 


Во-первых, я настоятельно рекомендую запустить приложение (только при разработке) с флагом 

    -trace-sync-io.

Это приведет к печати предупреждений и трассировки стека всякий раз, когда ваше приложение использует синхронный API.

Существует множество замечательных статей о том, как использовать promises, generators and async / await. Мне не нужно дублировать другую замечательную работу, которая уже доступна, поэтому вот несколько ссылок, которые помогут вам начать:

Promises - http://www.html5rocks.com/en/tutorials/es6/promises/
Async / Await - https://www.twilio.com/blog/2015/10/asyncawait-the-hero-javascript-deserved.html
Generators - https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Iterators_and_Generators

7. Обработать ошибки


Ошибка при сбое всего вашего приложения в производстве никогда не является отличным опытом.Хорошее управление исключениями важно для любого приложения, и лучший способ справиться с ошибками - использовать асинхронные структуры выше.
Например, обещания предоставляют обработчик .catch (), который будет распространять все ошибки, с которыми нужно обращаться, чисто.

Допустим, у вас есть цепочка promis, и любой из них может неожиданно провалиться, вы можете легко справиться с ошибкой:

    doSomething()
        .then(doNextStage)
        .then(recordTheWorkSoFar)
        .then(updateAnyInterestedParties)
        .then(tidyUp)
        .catch(errorHandler);
        
В приведенном выше примере не имеет значения, какая из ранних функций могла быть неудачной, любая ошибка закончится в errorHandler.

8. Убедитесь, что приложение автоматически перезапускается


Хорошо, поэтому вы следовали лучшей практике для обработки ошибок. К сожалению, некоторая ошибка из-за зависимости все-таки сбила ваше приложение 

Здесь важно убедиться, что вы используете диспетчер процессов, чтобы убедиться, что приложение изящно восстанавливается из-за ошибки времени выполнения (runtime error). Другой сценарий, в котором вы нуждаетесь, чтобы перезапустить, - это если весь сервер, на котором вы работаете, спустился. В этой ситуации вам нужно минимальное время простоя и для запуска приложения, как только сервер снова будет жив!
Я бы рекомендовал использовать PM2 KeyMetric http://pm2.keymetrics.io/ для управления процессом.  Хотя другие варианты включают (Nodemon)[http://nodemon.io/] (thanks @szabi) and (Forever)[https://github.com/foreverjs/forever].

Сначала установите его как глобальный модуль:

    $ npm install pm2 -g
    
Затем, чтобы запустить ваш процесс, вы должны запустить:

    $ pm2 start myApp.js
    
Чтобы обработать перезапуск после сбоя сервера, вы можете следовать руководству PM2 для вашей платформы:

http://pm2.keymetrics.io/docs/usage/startup/


9. Кластерное приложение для повышения производительности и надежности


По умолчанию Node.js запускается в одном процессе. В идеале вам нужен один процесс для каждого ядра ЦП, чтобы вы могли распределять рабочую нагрузку по всем ядрам. Это улучшает масштабируемость веб-приложений, обрабатывающих HTTP-запросы и производительность в целом.
В дополнение к этому, если один worker падает, остальные все еще доступны для обработки запросов.

Одним из других преимуществ использования диспетчера процессов, такого как PM2, является то, что он поддерживает кластеризацию из коробки:

http://pm2.keymetrics.io/docs/usage/cluster-mode/

Чтобы запустить несколько экземпляров вашего приложения для каждого ядра на машине, вы просто запускаете:

        $ pm2 start myApp.js -i max
        
Следует иметь в виду, что каждый процесс является автономным - они не разделяют память или ресурсы.Например, каждый процесс откроет свои собственные подключения к базам данных. Всегда помните об этом при кодировании.Например, полезным инструментом, который люди используют для совместного использования состояния сеанса, является Redis, это обеспечивает хранилище данных в памяти, которое может быть быстро доступно всеми процессами для хранения данных, связанных с сеансом.


10. Require all your dependencies up front

Я видел, как многие разработчики пишут код следующим образом:

        app.get("/my-service", function(request, response) {
            var datastore = require("myDataStoreDep")(someConfig);

            datastore.get(req.query.someKey)
            // etc, ...
        });

Проблема с приведенным выше кодом заключается в том, что когда кто-то делает запрос / my-service, код теперь загружает все файлы, необходимые для myDataStoreDep - любой из которых может генерировать исключение.Кроме того, когда конфигурация передается, также может быть ошибка в этой точке, которая может снизить весь процесс.Кроме того, мы не знаем, как долго будет проходить синхронная настройка ресурса. На этом этапе кода мы по существу блокируем все остальные запросы от обработки!

Поэтому вы всегда должны загружать все свои зависимости заранее и настраивать их заранее.Таким образом, вы узнаете из запуска, если есть проблема, а не через три-четыре часа после того, как ваше приложение вышло в производство!

11. Use a logging library to increase errors visibility
console.log is great but it has limits in a production application. Trying to sift through thousands of lines of logs to find the cause of the bug… which I guarantee you will have to do at some point, is painful!

A mature logging library can help with this. First, they allow you to set levels for each log message — whether it’s a debug, info, warning, or error. In addition, they typically allow you to log to different files or even remote datastore.

In my applications, for example, I typically log to https://www.loggly.com/. Loggly allows me to quickly search all my log messages using patterns. In addition, it can alert me if a threshold is reached — for example, if my web application starts returning 500 SERVER ERROR messages to my users for a period longer than 30 seconds, Loggly can send me a message and I can figure out what’s going on.

enter image description here

So what library should you use? Again this is always up for opinions. I personally like to use winston - https://github.com/winstonjs/winston.

12. Use Helmet if you’re writing a web app
If you’re writing a web application, there are a lot of common best practices that you should follow to secure your application:

XSS Protection
Prevent Clickingjacking using X-Frame-Options
Enforcing all connections to be HTTPS
Setting a Context-Security-Policy header
Disabling the X-Powered-By header so attackers can’t narrow down their attacks to specific software
Instead of remembering to configure all these headers, Helmet will set them all to sensible defaults for you, and allow you to tweak the ones that you need.

enter image description here

It’s incredibly simple to set up on an Express.js application:

$ npm install helmet
And then in your code when setting up Express add:

var helmet = require('helmet');
app.use(helmet());
13. Monitor your applications
Getting notified when something goes wrong with your application is critical on production applications. You don’t want to check your Twitter feed and see thousands of angry users telling you your servers are down or your app is broken and has been for the last few hours. So having something monitoring and alerting you to critical issues or abnormal behaviour is important.

We already discussed PM2 for process management. In addition it’s developers KeyMetrics.io run a process monitoring SaaS with integration with PM2 baked in. It’s very simple to enable and they have a free plan which is a great starting point for a lot of developers. Once you’ve signed up for KeyMetrics, you can simply run:

$ pm2 interact [public_key] [private_key] [machine_name]
This will start sending memory & CPU usage data, plus exception reporting to key metrics servers to view from their dashboard. You can also view latency of your http requests, or set up events when problems occur (for example timeouts to downstream dependencies).

enter image description here

In addition, Loggly (that we mentioned earlier) also provides monitoring based off logs. Both tools in combination can provide you with a way to quickly react to problems before they get out of hand.

14. Test your code
Yeah, yeah, yeah - I know I should be testing. TDD and all that jazz!

Seriously though, testing will save your ass on many occasions. Like creating any new habit, it’s painful to start and keep up the momentum. It gets in the way of your speed of development. However, I can talk from experience that once the first few production issues occur on a project with no tests, you’ll wish you had in the first place.

No matter what stage you are on a project, it’s never too late to introduce testing. My advice is start small, start simple. I’d also highly recommend writing a test for every bug that gets reported. That way you know:

How to reproduce the bug (make sure your test fails first!)
That the bug is fixed (make sure you test passes after you fix the issue)
That the bug will never occur again (make sure you run your tests on every new deployment)
There’s a lot of testing libraries. I personally stick with Jasmine because I’ve used it for a long time now, but Mocha, chai or any other libraries are great too. If you’re writing a web application too I’d also highly recommend Supertest to black box test your web end points.

Wrapping up
And with that, ladies and gentlemen, those are my nominees for the "top 14 best practices" of Node.js.

If you would like to nominate an additional Node.js best practices, please do so in the comments. Let's save the world of Node.js projects together!

 
 
 
 
 
 
 
 
 
 
 
 
