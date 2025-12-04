### Как писать комментарии в JSON запросе теле в Postman

Postman при работе с API и тестах своих контроллеров.
написать большой JSON кусок и отправить для запроса. из набора uuid и в разных версиях данных пометить, как для человека, что там за ними скрывается.

// Не используйте 42 в полях
{
    "key": "value", //описание поля, заметки
    "yet-another-key": true //true for truth
}

Но такой запрос сервер не принял - мне ваши комментарии синтаксически не нужны. Присылайте без них.
в Postman Pre-request Script. JS скрипт, который исполняется перед запросом. Там можно очистить запрос от комментов.

<img width="1135" height="319" alt="image" src="https://github.com/user-attachments/assets/da397c89-31df-42df-b4b1-0fd27c216812" />

// Strip JSON Comments
if (pm?.request?.body?.options?.raw?.language === 'json') {
    const rawData = pm.request.body.toString();
    const strippedData = rawData.replace(
        /\\"|"(?:\\"|[^"])*"|(\/\/.*|\/\*[\s\S]*?\*\/)/g,
        (m, g) => g ? "" : m
    );

    pm.request.body.update(JSON.stringify(JSON.parse(strippedData)));
}

Скрипт распаковывает json в строку, удаляет комментарии и обновляет тело запроса.
формат переданных данных не application/json, а text/plain. в postman в Headers был установлен формат Content-Type. после манипуляций формат ломался.
Решается добавлением своего параметра в Headers

Content-Type : application/json

<img width="1136" height="597" alt="image" src="https://github.com/user-attachments/assets/0d15f8db-afae-4251-bc14-48991f971a6f" />

не запоминать, за что отвечают наборы нечитаемых значений в json.
