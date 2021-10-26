# API для работы с плейлистами

## Введение

### Возможности API

Используя данный API вы сможете полноценно управлять плейлистами для прогрывания графики реального времени:

- создание;
- наполнение;
- изменение;
- удаление;

## Получение списка плейлистов, хранящихся на сервере

Для этого отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetPlaylists" MessageId="659">
  <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
</Command>
```

где:

- `MessageId` - иденификатор сообщения. Тип `long`. Каждое последующее сообщение увеличивает параметр на 1.
- `PlaylistGroupId` - иденификатор группы плейлистов. Тип `Guid`. Guid.Empty соответствует корневому разделу.

В ответ вы получите сообщение вида:

```xml
<Result CmdGroup="Playlists" CmdName="GetPlaylists" MessageId="659">
  <PlaylistGroups>
    <ObjectGroup>
      <Id>9477a1ba-520f-4d8e-a777-4bae961d52b8</Id>
      <Name>SomeTV</Name>
      <Created>28.08.2021 7:21:26</Created>
      <Changed>14.10.2021 12:58:56</Changed>
      <IsSystem>false</IsSystem>
      <ParentId>00000000-0000-0000-0000-000000000000</ParentId>
    </ObjectGroup>
    <ObjectGroup>
      <Id>96f6fcac-0cfc-48b3-80f1-05dfb53fc4c9</Id>
      <Name>Tests</Name>
      <Created>31.08.2021 14:42:45</Created>
      <Changed>06.10.2021 14:55:58</Changed>
      <IsSystem>false</IsSystem>
      <ParentId>00000000-0000-0000-0000-000000000000</ParentId>
    </ObjectGroup>
    <ObjectGroup>
      <Id>e0d38c66-4488-4aeb-bc3d-e2cbb2bf3c0c</Id>
      <Name>_projects</Name>
      <Created>08.09.2021 12:05:30</Created>
      <Changed>08.09.2021 12:05:30</Changed>
      <IsSystem>false</IsSystem>
      <ParentId>00000000-0000-0000-0000-000000000000</ParentId>
    </ObjectGroup>
  </PlaylistGroups>
  <Playlists>
    <Playlist>
      <Id>0a3044d6-682f-4b0f-9a23-078998af187b</Id>
      <Name>Playlist_Name_19</Name>
      <Created>11.08.2021 13:35:24</Created>
      <Changed>21.08.2021 8:44:31</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
      <ScenarioList>
        <ScenarioId>9bb9773e-4057-45f7-b399-3f0dd65b8e69</ScenarioId>
        <ScenarioId>f83ab270-dcc4-4f02-bbf8-19bf7a6ba0c5</ScenarioId>
        <ScenarioId>2798a15d-ffef-4e43-b629-50e52136f999</ScenarioId>
        <ScenarioId>25963dee-befd-459c-8f55-c25f93162353</ScenarioId>
        <ScenarioId>0e5abfd6-f158-4e8f-b01a-20f72eca776b</ScenarioId>
      </ScenarioList>
    </Playlist>
    <Playlist>
      <Id>17f2435c-f61e-46b6-8890-c516d9bacb24</Id>
      <Name>ABC</Name>
      <Created>06.09.2021 18:34:09</Created>
      <Changed>08.09.2021 12:03:22</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
      <ScenarioList>
        <ScenarioId>4e71648c-8239-49a6-9813-7ed5e8cf5e8b</ScenarioId>
        <ScenarioId>10984755-2cf3-4518-9eb3-ac507cc58ca5</ScenarioId>
      </ScenarioList>
    </Playlist>
    <Playlist>
      <Id>4af269ee-8538-41bb-948d-d317acd6d8c0</Id>
      <Name>Some_Playlist</Name>
      <Created>14.08.2021 14:32:24</Created>
      <Changed>14.08.2021 14:33:55</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
      <ScenarioList>
        <ScenarioId>e982d869-616b-4da7-9218-561451c9d51f</ScenarioId>
      </ScenarioList>
    </Playlist>   
  </Playlists>
</Result>
```

где:

- `MessageId` - иденификатор сообщения. Тип `long`. Совпадает с идентификатором сообщения-запроса.
- `PlaylistGroups` - список дочерних групп запрашиваемой группы.
- `Id` - иденификатор объекта. Тип `Guid`.
- `Name` - имя объекта. Тип `String`.
- `Created` - время создания объекта. Тип `DateTime`.
- `Changed` - время последнего изменения объекта. Тип `DateTime`.
- `IsSystem` - идентификатор системного объекта. Тип `Boolean`. Если `true`, то данный объект нельзя удалить.
- `ParentId` - идентификатор родительской группы.
- `Playlists` - список плейлистов запрашиваемой группы.
- `PlaylistGroupId` - идентификатор группы плейлистов, к которой относится плейлист.
- `ScenarioList` - список историй данного плейлиста.
- `ScenarioId` - идентификатор истории. Тип `Guid`.
