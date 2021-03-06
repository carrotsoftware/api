# Авторизация

> Здесь и далее нерасшифрованные поля не участвуют в работе API и используются внутренними компонентами Carrot

Отправьте информацию, необходимую для авторизации.

Для этого отправьте на сервер сообщение вида:

A. В случае, если используется шифрование пароля:

  ```xml
    <Command CmdGroup="Users" CmdName="Login" MessageId="2">
      <UserName>admin</UserName>
      <PassWord>KFlXF0fLFEbYmY2f17==</PassWord>  
    </Command>
  ```

B. В случае, если шифрование пароля не используется:

  ```xml
    <Command CmdGroup="Users" CmdName="LoginUnsecure" MessageId="2">
      <UserName>admin</UserName>
      <PassWord>adminPassword</PassWord>  
    </Command>
  ```

  где:

- `MessageId` - идентификатор сообщения. Тип `long`. В каждом последующем    сообщении клиент должен увеличивать параметр на 1.
- `UserName` - логин. Тип `String`.
- `PassWord` - пароль. Тип `String`.

В ответ вы получите сообщение об успешном или неуспешном прохождении авторизации.

Если авторизация прошла успешно, сообщение будет иметь вид:

```xml
<Result CmdGroup="Users" CmdName="LoginOk" MessageId="2" Token="6c3540c6-a1dd-4496-abf8-b5c95a6281e0">
  <User>
    <Id>29108d95-76f4-4309-9ebc-d6545a6c81d6</Id>
    <Name>Administrator</Name>
    <Created>17.06.2021 12:32:26</Created>
    <Changed>17.06.2021 12:32:26</Changed>
    <IsSystem>true</IsSystem>
    <Login>admin</Login>
    <Password>KFlXF0fLFEbYmY2f17==</Password>
    <GroupId>9376adbf-fe5c-4acc-b87a-512e6d4c864d</GroupId>
    <IsAdmin>false</IsAdmin>
    <FavTemplatesList>
      <TemplateId>2445d973-02f1-4f31-b23f-afdbcedf042c</TemplateId>
      <TemplateId>355b3b70-2ab6-415d-b10d-c7c786238f5d</TemplateId>
      <TemplateId>610c7382-388f-4ee3-8199-c9b5061a489b</TemplateId>
      <TemplateId>56b959b3-d0f6-47b7-bbed-9c3ef29d54ca</TemplateId>
    </FavTemplatesList>
    <FavMediasList>
      <MediaId>d61f5185-c236-4f58-8c1c-aaafad742536</MediaId>
      <MediaId>d3256b2b-1ecb-432f-a65e-26aadc73921b</MediaId>
      <MediaId>adadc280-b99a-443d-a689-1e532501d76d</MediaId>
      <MediaId>098e1d89-13d2-4895-8364-26f1d138a33f</MediaId>
    </FavMediasList>
  </User>
</Result>
```

где:

- `MessageId` - идентификатор сообщения. Тип `long`. Совпадает с идентификатором сообщения-запроса.
- `Id` - идентификатор учетной записи. Необходим для полной остановки плейлиста
- `FavTemplatesList` - список идентификаторов избранных шаблонов для быстрого доступа.
- `TemplateId` - идентификатор избранного шаблона.
- `FavMediasList` - список идентификаторов избранных медиа файлов для быстрого доступа.
- `MediaId` - идентификатор избранного медиа файла.

Если авторизация прошла неуспешно, сообщение будет иметь вид:

```xml
<Result CmdGroup="Users" CmdName="LoginError" MessageId="2">
  <Message>Incorrect Password!</Message>
</Result>
```

где:

- `MessageId` - идентификатор сообщения. Тип `long`. Совпадает с идентификатором сообщения-запроса.
- `Message` - описание ошибки.
