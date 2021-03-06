# Полномасштабное веб-приложение с Node.js

## Что должно делать наше приложение

Возьмём что-нибудь попроще, но приближенное к реальности:

- Пользователь должен иметь возможность использовать наше приложение с браузером;
- Пользователь должен видеть страницу приветствия по адресу `http://domain/start`;
- Когда запрашивается `http://domain/upload`, пользователь должен иметь возможность загрузить картинку со своего компьютера и просмотреть её в своем браузере.

Вполне достаточно. Конечно, вы могли бы достичь этой цели, немного погуглив и поговнокодив. Но это не то, что нам нужно.

Кроме того, мы не хотим писать только простой код для достижения цели, каким бы он элегантным и корректным ни был. Мы будем интенсивно наращивать больше абстракции, чем это необходимо, чтобы понять как создавать более сложные Node.js-приложения.

## Задачи

Давайте проанализируем наше приложение. Что нужно, чтобы его реализовать:

- У нас — онлайн веб-приложение, поэтому нам нужен HTTP-сервер;
- Нашему серверу необходимо обслуживать различные запросы в зависимости от URL, по которому был сделан запрос. Для этого нам нужен какой-нибудь роутер (маршрутизатор), чтобы иметь возможность направлять запросы определенным обработчикам;
- Для выполнения запросов, пришедших на сервер и направляемые роутером, нам нужны действующие обработчики запросов;
- Роутер, вероятно, должен иметь дело с разными входящими POST-данными и передавать их обработчикам запросов в удобной форме. Для этого нам нужен какой-нибудь обработчик входных данных;
- Мы хотим не только обрабатывать запросы, но и показывать пользователю контент по запрошенным URL-адресам, поэтому нам нужна некая логика отображения для обработчиков запросов, чтобы иметь возможность отправлять контент пользовательскому браузеру;
- Последнее, но не менее важное — пользователь сможет загружать картинки, поэтому нам нужен какой-нибудь обработчик загрузки, который возьмёт на себя заботу о деталях.

Давайте подумаем о том, как бы мы реализовали это на PHP. Скорее всего, типичное решение будет на HTTP-сервере Apache с установленным mod_php5.

Это относится к первому пункту наших задач, то есть, «принимать HTTP-запросы и отправлять готовые веб-странички пользователю» — вещи, которые PHP сам не делает.

С Node.js — немного иначе. Потому что в Node.js мы не только создаем наше приложение, мы также реализуем полноценный HTTP-сервер. Действительно, наше веб-приложение и веб-сервер — в сущности, одно и тоже.

Может показаться, что это приведет к лишней работе, но сейчас вы увидите, что с Node.js это не так.

Давайте просто начнём реализовывать нашу первую задачу — HTTP-сервер.
