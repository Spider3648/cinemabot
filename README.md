# Cinema Bot

Это бот для поиска фильмов и/или сериалов со ссылками на просмотр.
Работает на основе JustWatch API.
Поддерживаются следующие команды:

- простое сообщение — запрос на поиск
- `/start`, `/help` выводят справочную информацию
- `/todo` список фич которые хотелось бы ещё поддержать
- `/schedule N` выполняет поиск по истечении `N` секунд

Cinemabot'а можно найти в телеграмме по сложному нику [@yet_another_awesome_cinema_bot](https://t.me/yet_another_awesome_cinema_bot), а хостится он на Heroku с использованием webhook'ов, поэтому пока им практически не пользуются, он постоянно доступен.

## Детали

В inline-keyboard отображаются названия кинотеатров (потому что иконки поставить не получилось), которые могут быть разной ширины.
Так, три названия могут полностью влезть в строчку, а могут не влезть и два (часть названия будет заменена на многоточие).
Чтобы побороть этот неприятный спецэффект, помимо стандартной политики "не больше чем `row_width` кнопок в одной строке" была добавлена "не больше чем `symbols_limit` символов в сумме" (см. `WrappedInlineKeyboardMarkup`).

По запросу обычно показывается detailed-view с первым результатом поиска.
В случае, если этого недостаточно, можно на inline-keyboard выбрать кнопку "more", которая делает ищет обработчика `callback_query_handler` с запросом `list:{query}`.
После этого открывается список с 10 результатами в котором каждая кнопка является одним из двух запросов: `movie:{movie_id}` или `show:{show_id}`.

Кнопки клавиатуры могут нести только строку в качестве дополнительной информации.
Конечно, можно сериализовать/десериализовать любые данные в строку, но мы тут стараемся _keep it simple, stupid_.
Прямо сейчас правильный обработчик нажатия на кнопку клавиатуры выбирается по префиксу этих данных, но в целом можно использовать finite-state-machine по состояниям бота которую предоставляет `aiogram`.
Но пока у меня есть всего три (максимум четыре) состояния которые я могу выделить, это не представляется хорошим решением. 
