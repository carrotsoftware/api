# Подключение

1. Установите соединение WebSocket-соединение для обмена сообщениями (например, `ws://localhost:24710`).

В ответ вы получите сообщение вида:

```xml
<Command CmdGroup="ClientID">
  <ID>ee4d2052-18c1-478d-8e92-0e7ac24398fe</ID>
  <ReplicationHosts>192.168.1.203,192.168.1.39</ReplicationHosts>
  <ReplicationNumber>0</ReplicationNumber>
</Command>
```

где:

- `ID` - идентифиактор соединения на стороне сервера. Тип `Guid`.
- `ReplicationHosts` - список ip-адресов всех серверов (в случае использования резервирования). Тип `String`.
- `ReplicationNumber` - статус резервирования у подключаенного сервера (0 = master, остальное = slave). Тип `Int`.

2. Установите соединение WebSocket-соединение для обмена файлами (например, `ws://localhost:24710`).

В ответ вы получите сообщение вида:

```xml
<Command CmdGroup="ClientID">
  <ID>c3c8bbf2-3562-4853-ad6e-9a56bfd5a1fd</ID>
</Command>
```

где:

- `ID` - идентифиактор подключения на стороне сервера. Тип `Guid`.

3. После подключения отправьте информацию о клиентской части подключения.

Для этого отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="HandShake" MessageId="1">
  <AppName>Carrot Web Playlist</AppName>
  <SessionID>e12f42c8-140a-4ca7-aad6-36e5b5cf7e48</SessionID>
</Command>
```

где:

- `MessageId` - иденификатор сообщения. Тип `long`. Каждое последующее сообщение увеличивает параметр на 1.
- `AppName` - имя приложения, использующее подключение. Тип `String`.
- `SessionID` - идентификатор подключения на стороне клиента. Тип `Guid`.
