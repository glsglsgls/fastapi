# Header-параметры

Вы можете определить Header-параметры точно так же, как вы определяете `Query`, `Path` и `Cookie`-параметры.

## Импортируйте `Header`

Прежде всего, импортируйте `Header`:

```Python hl_lines="3"
{!../../../docs_src/header_params/tutorial001.py!}
```

## Объявите `Header`-параметры

Затем объявите параметры заголовков, используя такую же структуру, как и с `Path`, `Query` и `Cookie`.

Первое значение является значением по умолчанию, вы можете передать все дополнительные параметры валидации или аннотации:

```Python hl_lines="9"
{!../../../docs_src/header_params/tutorial001.py!}
```

!!! note "Технические Детали"
    `Header` является "родственным" классом для `Path`, `Query` и `Cookie`. Он также наследуется от того же общего класса `Param`.

    Но помните, что когда вы импортируете `Query`, `Path`, `Header` и др. из `fastapi`, на самом деле это функции, возвращающие специальные классы.

!!! info
    Для объявления заголовков вам необходимо использовать `Header`, в противном случае эти параметры будут интерпретироваться как параметры запроса.

## Автоматическое преобразование

`Header` включает в себя немного больше функциональности, чем та, которую предоставляют `Path`, `Query` и `Cookie`.

Большинство стандартных заголовков разделяются дефисом (или знаком минус) `-`.

Но переменная вроде `user-agent` является некорректной в Python.

Поэтому по умолчанию `Header` будет преобразовывать знаки подчеркивания (`_`) в названиях параметров в дефисы (`-`) для получения и документирования заголовков.

Кроме того, названия HTTP-заголовков являются регистронезависимыми, поэтому вы можете объявлять их в обычном Python-стиле (который также известен как "змеиная нотация").

Поэтому вы можете использовать `user_agent`, как обычно написали бы в Python-коде, вместо того, чтобы переводить первые буквы в верхний рагистр вроде `User_Agent` или каким-то другим образом.

Если по каким-то причинам вам не требуется автоматическое преобразование символов подчеркивания в дефисы, установите параметр `convert_underscores` функции `Header` в значение `False`:

```Python hl_lines="10"
{!../../../docs_src/header_params/tutorial002.py!}
```

!!! warning
    Перед тем, как устанавливать `convert_underscores` в значение `False`, имейте в виду, что некоторые HTTP-прокси и серверы запрещают использование заголовков со знаками подчеркивания.


## Повторяющиеся заголовки

Возможно получить повторяющиеся заголовки. Это означает один и тот же заголовок с множеством значений.

Вы можете определить эти случаи, используя список в объявлении типа.

Вы получите все значения для повторяющегося заголовка в виде обычного `list`.

Например, для объявления заголовка `X-Token`, который может появиться больше одного раза, вы можете написать:

```Python hl_lines="9"
{!../../../docs_src/header_params/tutorial003.py!}
```

Если вы взаимодействуете с подобным *обработчиком пути*, отправляя два HTTP-заголовка вроде:

```
X-Token: foo
X-Token: bar
```

Ответ будет выглядеть так:

```JSON
{
    "X-Token values": [
        "bar",
        "foo"
    ]
}
```

## Резюме

Объявляйте заголовки с `Header` так же, как и с `Query`, `Path` и `Cookie`.

И не волнуйтесь о знаках подчеркивания в ваших переменных, **FastAPI** позаботится об их преобразовании.