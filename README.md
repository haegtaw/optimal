Итак. Я начала работу над тестовым заданием в воскресенье, и, так как было бы преступлением использовать гитлаб и раннеры моей текущей компании, а собстенного сервака у меня по сумме событий пока нет, то я развернула гитлаб и гитлаб-раннер на локальной машинке с убунту 22.
Гитлаб я подняла и пайпы со сборкой и тестом успешно прокатила (только из-за специфики вм на локальной машинке, это было очень, очень, очень медленно)
Запоролась на том, чтобы раннер нашел по ссш мою вм-ку с миникубом (что он прекрасно делал по ссш руками, но не в пайпе, подложить ключ в виде переменной мой чудовищно медленный гитлаб на локалке так и не дал, в виде переменной тоже приватный ключ выкладывать было бы нехорошо)

![локальный гитлаб, очень медленный](img/build.jpg)
![ключ раннера на миникубовой тачке не видать](img/but-no-deploy.jpg)
![ключ раннера на миникубовой тачке не видать1](img/ssh-troubles.jpg)
![ключ раннера на миникубовой тачке не видать2](img/ssh-denied.jpg)


Если образ, собранный пайпом, перекинуть tar по scp на машинку с миникубом, там собрать обратно в докер-образ и дать разрешение миникубу использовать небезопасные реджистри, то руками в миникубе задеплоить аппку можно
![образ, который я собрала для аппки на вм с гитлабом, могу перкинуть и собрать в миникубе теми же командами, что дала в сиайке](img/hands-in-kube.jpg)
Требуемые в тестовом под, деплоймент и метрики
![под и деплоймент](img/get-po.png.png)
![метрики](img/metrics.png)
![метрики2](img/expose-and-metrics.png)

Но это не то, что нужно, ведь деплоить тоже должен пайп, а не девопс.
Таким образом, в воскресенье мной был приобретен чрезвычайно интересный и воспитывающий долготерпение опыт локально развернутого гитлаба при помощи snap. Конечно, я сделала в пн ещё одну попытку, но уже в докер-композе
![ключ раннера на миникубовой тачке не видать3](img/gitlab-compose.jpg)
Это оказалось ещё медленнее, почти невыносимо. Пришло время все-таки отправиться на настоящий gitlab.com

Но на гитлабе граждан РФ сейчас не принимают, поэтому я обратилась к гитхаб actions. Сиайки там пишутся специфически, иначе, чем на гитлабе, но принципиально разница невелика.

![ушла на гитхаб](img/actions.png)
Тут мой код по проекту
https://github.com/haegtaw/optimal/tree/main
Вот успешный пайп
https://github.com/haegtaw/optimal/actions/runs/11786069696

С пайплайном пришлось повозиться, потому что не сразу догадалась подсунуть кубконфиг и работать кубектлом сразу с машины с раннером
![пайпы](img/all-pipes.png)
![пайп успешный](img/good-pipe.png)
![работа моего раннера](img/runner-data.png)

Метрики от задеплоенной аппки
![метрики](img/good-metrics.png)

Очень полезное задание, хотя на продовых мощностях деплоить на порядок проще :)


АПД. для сервиса манифест не писала, просто применила 

`kubectl expose deployment tryingflask --type=NodePort --port=5000`

И не делала пвц, потому что при перезапуске пода хранила бы только метрики, вроде ни к чему (было б в задании - сделала бы)