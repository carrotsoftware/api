# API для работы с плейлистами

## Введение

### Возможности API

Используя данный API вы сможете полноценно управлять плейлистами для проигрывания графики реального времени:

- создание;
- наполнение;
- изменение;
- удаление;

## Структура плейлиста

Структура плейлиста представляет из себя следующую связь:

- группа плейлистов
  - группа плейлистов
    - плейлист (Playlist)
      - история (Story)
        - элемент (Item)
          - шаблон (Template)

Каждая группа плейлистов может включать в себя другие группы плейлистов и сами плейлисты.

Каждый плейлист может включать в себя несколько историй (Story).

Каждая история может включать в себя несколько элементов (Item).

Каждый элемент связан только с одним шаблоном (Template).

У шаблона есть проигрываемые состояния (State) и переменные.

Элемент может изменять переменные шаблона и вызывать одно проигрываемое состояние.

## Получение списка плейлистов, хранящихся на сервере

> Здесь и далее нерасшифрованные поля не участвуют в работе API и используются внутренними компонентами Carrot

Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetPlaylists" MessageId="659">
  <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
</Command>
```

где:

- `MessageId` - идентификатор сообщения. Тип `long`. В каждом последующем сообщении клиент должен увеличивать параметр на 1.
- `PlaylistGroupId` - идентификатор группы плейлистов. Тип `Guid`. Guid.Empty соответствует корневому разделу.

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

- `PlaylistGroups` - список дочерних групп запрашиваемой группы.
- `Id` - идентификатор объекта. Тип `Guid`. Используется для запросов содержимого объекта с сервера.
- `Name` - имя объекта. Тип `String`.
- `ParentId` - идентификатор родительской группы, к которой относится дочерняя группа. Используется для отслеживания состояния родительского объекта.
- `Playlists` - список плейлистов запрашиваемой группы.
- `PlaylistGroupId` - идентификатор группы плейлистов, к которой относится плейлист. Используется для отслеживания состояния родительского объекта.
- `ScenarioList` - список идентификаторов историй данного плейлиста.
- `ScenarioId` - идентификатор истории. Тип `Guid`. Используется для запроса содержимого истории.

## Получение списка шаблонов

Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Templates" CmdName="GetTemplates" MessageId="238">
  <TemplateGroupId>62824b96-d977-4b16-92ce-966eba09bd27</TemplateGroupId>
</Command>
```

где:

- `MessageId` - идентификатор сообщения. Тип `long`. В каждом последующем сообщении клиент должен увеличивать параметр на 1.
- `TemplateGroupId` - идентификатор группы шаблонов. Тип `Guid`. Guid.Empty соответствует корневому разделу.

В ответ вы получите сообщение вида:

```xml
<Result CmdGroup="Templates" CmdName="GetTemplates" MessageId="238">
  <TemplateGroups>
    <ObjectGroup>
      <Id>79983fb0-1cb0-4f66-ad77-7abb16554a9a</Id>
      <Name>Second Folder</Name>
      <Created>28.10.2021 11:50:15</Created>
      <Changed>28.10.2021 11:50:15</Changed>
      <IsSystem>false</IsSystem>
      <ParentId>62824b96-d977-4b16-92ce-966eba09bd27</ParentId>
      <ChannelId>00000000-0000-0000-0000-000000000000</ChannelId>
    </ObjectGroup>
    <ObjectGroup>
      <Id>bea3bca2-1499-4a9c-80bd-049dff037256</Id>
      <Name>Example Folder</Name>
      <Created>28.10.2021 11:50:05</Created>
      <Changed>28.10.2021 11:50:05</Changed>
      <IsSystem>false</IsSystem>
      <ParentId>62824b96-d977-4b16-92ce-966eba09bd27</ParentId>
      <ChannelId>00000000-0000-0000-0000-000000000000</ChannelId>
    </ObjectGroup>
  </TemplateGroups>
  <Templates>
    <Template>
      <Id>065a20e1-e60a-4872-b212-ed5c5bd1e266</Id>
      <Name>PluginDev_03_2Inputs</Name>
      <Created>29.03.2021 18:42:37</Created>
      <Changed>29.03.2021 18:42:37</Changed>
      <IsSystem>false</IsSystem>
      <XmlData></XmlData>
      <TemplateTypeInt>3</TemplateTypeInt>
      <TemplateGroupId>62824b96-d977-4b16-92ce-966eba09bd27</TemplateGroupId>
      <Rev>0</Rev>
      <IsMaster>False</IsMaster>
      <ClosingState></ClosingState>
      <DefaultInState></DefaultInState>
      <DefaultOutState></DefaultOutState>
      <ContentId>00000000-0000-0000-0000-000000000000</ContentId>
      <DataUpdateType>Static</DataUpdateType>
      <DataUpdateState></DataUpdateState>
      <LinkVars>
        <LinkVar Id="rttInput1" Name="rttInput1" Type="MediaSource" FieldId="a94dd4f4-1f79-4de0-a12b-2fd603f65df8" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="rttInput2" Name="rttInput2" Type="MediaSource" FieldId="6221ff2d-1bb6-462b-9038-efc16fa76a47" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
      </LinkVars>
      <StateInfos />
    </Template>
    <Template>
      <Id>f25650da-71f7-443a-b62a-0d050f3db6f9</Id>
      <Name>titles_blue_1</Name>
      <Created>01.03.2021 15:55:26</Created>
      <Changed>28.10.2021 11:57:06</Changed>
      <IsSystem>false</IsSystem>
      <XmlData></XmlData>
      <TemplateTypeInt>1</TemplateTypeInt>
      <TemplateGroupId>62824b96-d977-4b16-92ce-966eba09bd27</TemplateGroupId>
      <Rev>5</Rev>
      <IsMaster>False</IsMaster>
      <ClosingState>OUT</ClosingState>
      <DefaultInState>IN</DefaultInState>
      <DefaultOutState>OUT</DefaultOutState>
      <ContentId>6053fa4d-6ea0-4bd0-a940-b1ea2e3c4493</ContentId>
      <DataUpdateType>Static</DataUpdateType>
      <DataUpdateState></DataUpdateState>
      <LinkVars>
        <LinkVar Id="0#66#2//&quot;Name&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Name" Type="Text" FieldId="7222cba1-e6dc-4eb0-a05b-4e0d698e01fc" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#66#1//&quot;Title&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" FieldId="879d7e0a-86e3-4ae1-8fd9-8a7c48164dc5" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
      </LinkVars>
      <StateInfos>
        <StateInfo>
          <State>IN</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>2.16</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>OUT</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>2.08</StateDuration>
        </StateInfo>
      </StateInfos>
    </Template>
  </Templates>
</Result>
```

где:

- `TemplateGroups` - список дочерних групп запрашиваемой группы.
- `Id` - идентификатор объекта. Тип `Guid`. Используется для запросов содержимого объекта с сервера.
- `Name` - имя объекта. Тип `String`.
- `ParentId` - идентификатор родительской группы, к которой относится дочерняя группа. Используется для отслеживания состояния родительского объекта.
- `Templates` - список шаблонов запрашиваемой группы.
  - `Template` - структура конкретного шаблона.
    - `Id` - идентификатор шаблона. Тип `Guid`. Используется для создания нового элемента плейлиста.
    - `Name` - имя шаблона. Тип `String`.
    - `TemplateTypeInt` - тип шаблона. Тип `Int`. (AE = 1, UE4 = 3, FBX = 4).
    - `TemplateGroupId` - идентификатор группы шаблонов, к которой относится данный шаблон. Тип `Guid`. Используется для запросов содержимого объекта с сервера.
    - `LinkVars` - список переменных шаблона. Используется для создания нового элемента плейлиста.
      - `LinkVar` - структура переменной шаблона.
        - `Name` - имя переменной. Тип `String`.
        - `Type` - тип переменной. Тип `String`.
        - `DefaultValue` - значение переменной. Тип `String`.
        - `ListValues` - список подготовленных значений. Если тип переменной `ComboBox`.
          - `ListValue` - подготовленное значение. Тип `String`.
    - `StateInfos` - список состояний шаблона. Используется для создания нового элемента плейлиста.
      - `StateInfo` - структура состояния шаблона.
        - `State` - имя состояния. Тип `String`.

## Получение списка медиа файлов

Для этого [по соединению для обмена файлами](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="MediaAssetLibrary" CmdName="GetAssetFolder" MessageId="99">
  <FolderID>b30f78f1-4f06-4fdb-b7d5-8ab1d496ce71</FolderID>
</Command>
```

где:

- `MessageId` - идентификатор сообщения. Тип `long`. В каждом последующем сообщении клиент должен увеличивать параметр на 1.
- `FolderID` - идентификатор группы медиа файлов. Тип `Guid`. Guid.Empty соответствует корневому разделу.

В ответ вы получите сообщение вида:

```xml
<Command CmdGroup="MediaAssetLibrary" CmdName="GetAssetFolder" MessageId="99">
  <FolderStructure>
    <Folder>
      <Name>Inner Folder 1</Name>
      <ID>871e7278-77e4-4b9d-ae07-4aeabad7bb92</ID>
      <ParentID>b30f78f1-4f06-4fdb-b7d5-8ab1d496ce71</ParentID>
    </Folder>
    <Folder>
      <Name>Inner Folder 2</Name>
      <ID>9016174c-9875-4edc-9368-bc5c94c990ae</ID>
      <ParentID>b30f78f1-4f06-4fdb-b7d5-8ab1d496ce71</ParentID>
    </Folder>
    <Asset>
      <Name>Untitled 03.avi</Name>
      <ID>9262d3ec-5753-438b-9cff-95ecd39c44ed</ID>
      <Rev>0</Rev>
      <AssetTypeInt>1</AssetTypeInt>
      <ParentID>b30f78f1-4f06-4fdb-b7d5-8ab1d496ce71</ParentID>
    </Asset>
    <Asset>
      <Name>White.png</Name>
      <ID>7326d600-ded3-445c-83e1-c33f782ecfed</ID>
      <Rev>0</Rev>
      <AssetTypeInt>0</AssetTypeInt>
      <ParentID>b30f78f1-4f06-4fdb-b7d5-8ab1d496ce71</ParentID>
    </Asset>
  </FolderStructure>
</Command>
```

где:

- `Folder` - структура дочерней группы медиа файлов.
  - `ID` - идентификатор группы. Тип `Guid`. Используется для запросов содержимого группы с сервера.
  - `Name` - имя группы. Тип `String`.
  - `ParentID` - идентификатор родительской группы, к которой относится дочерняя группа. Используется для отслеживания состояния родительского объекта.
- `Asset` - структура конкретного медиа файла.
  - `ID` - идентификатор медиа файла. Тип `Guid`. Используется для присвоения значения переменной шаблона в элементе плейлиста.
  - `Name` - имя шаблона. Тип `String`.
  - `AssetTypeInt` - тип медиа файла. Тип `Int`. (Image = 0, Video = 1, ImageSequence = 2, FBX = 4).
  - `ParentID` - идентификатор группы медиа файлов, к которой относится данный медиа файл. Тип `Guid`. Используется для отслеживания состояния родительского объекта.

## Получение полного содержимого плейлиста

Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetFullPlaylist" MessageId="38">
  <PlaylistId>f37aa5a3-3dca-48cf-aece-d110ffe9e80e</PlaylistId>
</Command>
```

где:

- `PlaylistId` - идентификатор плейлиста. Тип `Guid`.

В ответ вы получите сообщение вида:

```xml
<Result CmdGroup="Playlists" CmdName="GetFullPlaylist" MessageId="38">
  <Playlist>
    <Id>f37aa5a3-3dca-48cf-aece-d110ffe9e80e</Id>
    <Name>TestDefault</Name>
    <Created>12.04.2021 16:46:48</Created>
    <Changed>27.10.2021 13:37:09</Changed>
    <IsSystem>false</IsSystem>
    <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
    <EngineList />
    <ScenarioList>
      <ScenarioId>7aeda54e-c0d5-4044-9aba-6d8c4603a6a4</ScenarioId>
      <ScenarioId>a1455623-e373-47e1-a893-5383cdee6308</ScenarioId>
    </ScenarioList>
  </Playlist>
  <Scenarios>
    <Scenario>
      <Id>a1455623-e373-47e1-a893-5383cdee6308</Id>
      <Name>New Story</Name>
      <Created>27.10.2021 13:37:09</Created>
      <Changed>27.10.2021 13:37:30</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistId>f37aa5a3-3dca-48cf-aece-d110ffe9e80e</PlaylistId>
      <Duration>0</Duration>
      <CurrentTime>0</CurrentTime>
      <IsDefault>false</IsDefault>
      <EventList>
        <EventId>a30dc6a3-7d83-4f17-8775-005351b4fb93</EventId>
      </EventList>
    </Scenario>
  </Scenarios>
  <Events>
    <Event>
      <Id>a30dc6a3-7d83-4f17-8775-005351b4fb93</Id>
      <Name>BreakingNews_8_Vars</Name>
      <Created>27.10.2021 13:37:30</Created>
      <Changed>27.10.2021 13:37:30</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistId>f37aa5a3-3dca-48cf-aece-d110ffe9e80e</PlaylistId>
      <ScenarioId>a1455623-e373-47e1-a893-5383cdee6308</ScenarioId>
      <TemplateId>07f5bbb0-432c-47f4-8b74-e9b519790dfc</TemplateId>
      <ContentId>e615d85b-19fc-4613-a7bd-521517e82b05</ContentId>
      <Duration>0</Duration>
      <CurrentTime>0</CurrentTime>
      <StatusInt>0</StatusInt>
      <TemplateTypeInt>1</TemplateTypeInt>
      <State>IN</State>
      <BeginTime>0</BeginTime>
      <BeginTimeOffset>0</BeginTimeOffset>
      <EventGroupId>00000000-0000-0000-0000-000000000000</EventGroupId>
      <DesiredStatusInt>0</DesiredStatusInt>
      <AllowRuntimeChange>false</AllowRuntimeChange>
      <Comment />
      <Variables>
        <Variable Id="0#13#1//&quot;Blue Solid 1&quot;//&quot;Effects&quot;//&quot;Fill&quot;//&quot;Color&quot;" Name="Color" Type="Color" Value="1,0,0,1" FieldId="2341742c-7e5f-4b71-bd4b-3d73aa2aa74d" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="1,0,0,1" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="1#13#17//&quot;Layer 3&quot;" Name="Layer 3" Type="Media" Value="" FieldId="4f1d06ff-06f1-4ac8-bb62-b0902de90a48" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
        <Variable Id="0#13#8//&quot;logo&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="Position" Type="Point3D" Value="214.5,903.5,0" FieldId="4c6e8a64-80bf-4bf2-b905-1768ffb3c31e" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="214.5,903.5,0" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
        <Variable Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale" Type="Point2D" Value="100,100" FieldId="9537c341-379c-47a7-92dd-cc1d8d7bc111" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="100,100" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
        <Variable Id="0#13#14//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Source Prepared Text" Type="ComboBox" Value="BREAKING NEWS" FieldId="77fe9d37-000b-4434-81f6-01a2226508b7" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue>First Prepared Text</ListValue>
            <ListValue>Second Prepared Text</ListValue>
          </ListValues>
        </Variable>
        <Variable Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Source Text" Type="Text" Value="CARROT ENGINE FLYING HIGH" FieldId="a38170e0-ae8d-4ae8-9d49-5a15f63a587f" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
        <Variable Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Opacity&quot;" Name="UseOpacity" Type="Boolean" Value="0" FieldId="b09309d1-42c8-41ef-8771-651d5a8a74a2" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="0" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
        <Variable Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;X Position&quot;" Name="X Position" Type="Float" Value="0" FieldId="5e7758a3-239a-429b-855c-3b383648f728" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="0" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue />
          </ListValues>
        </Variable>
      </Variables>
    </Event>
  </Events>
  <Templates>
    <Template>
      <Id>07f5bbb0-432c-47f4-8b74-e9b519790dfc</Id>
      <Name>BreakingNews_8_Vars</Name>
      <Created>27.10.2021 13:02:03</Created>
      <Changed>27.10.2021 13:02:03</Changed>
      <IsSystem>false</IsSystem>
      <XmlData></XmlData>
      <TemplateTypeInt>1</TemplateTypeInt>
      <TemplateGroupId>012b0032-398f-4687-b603-323fa66d2d52</TemplateGroupId>
      <Rev>0</Rev>
      <IsMaster>False</IsMaster>
      <ClosingState>OUT</ClosingState>
      <DefaultInState>IN</DefaultInState>
      <DefaultOutState>OUT</DefaultOutState>
      <ContentId>e615d85b-19fc-4613-a7bd-521517e82b05</ContentId>
      <DataUpdateType>Static</DataUpdateType>
      <DataUpdateState></DataUpdateState>
      <LinkVars>
        <LinkVar Id="0#13#1//&quot;Blue Solid 1&quot;//&quot;Effects&quot;//&quot;Fill&quot;//&quot;Color&quot;" Name="Color" Type="Color" FieldId="2341742c-7e5f-4b71-bd4b-3d73aa2aa74d" DefaultValue="1,0,0,1" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="1#13#17//&quot;Layer 3&quot;" Name="Layer 3" Type="Media" FieldId="4f1d06ff-06f1-4ac8-bb62-b0902de90a48" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#8//&quot;logo&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="Position" Type="Point3D" FieldId="4c6e8a64-80bf-4bf2-b905-1768ffb3c31e" DefaultValue="214.5,903.5,0" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale" Type="Point2D" FieldId="9537c341-379c-47a7-92dd-cc1d8d7bc111" DefaultValue="100,100" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#14//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Source Prepared Text" Type="ComboBox" FieldId="77fe9d37-000b-4434-81f6-01a2226508b7" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue>First Prepared Text</ListValue>
            <ListValue>Second Prepared Text</ListValue>
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Source Text" Type="Text" FieldId="a38170e0-ae8d-4ae8-9d49-5a15f63a587f" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Opacity&quot;" Name="UseOpacity" Type="Boolean" FieldId="b09309d1-42c8-41ef-8771-651d5a8a74a2" DefaultValue="0" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;X Position&quot;" Name="X Position" Type="Float" FieldId="5e7758a3-239a-429b-855c-3b383648f728" DefaultValue="0" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue />
          </ListValues>
        </LinkVar>
      </LinkVars>
      <StateInfos>
        <StateInfo>
          <State>IN</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.4</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>LOOP</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.88</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>OUT</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.72</StateDuration>
        </StateInfo>
      </StateInfos>
    </Template>
  </Templates>
  <Contents>
    <Content>
      <Id>48ca644a-32d2-441a-ba29-759506b2003d</Id>
      <Name>GV_AE_1</Name>
      <Created>22.09.2021 12:49:13</Created>
      <Changed>18.10.2021 13:57:25</Changed>
      <IsSystem>false</IsSystem>
      <ContentTypeInt>1</ContentTypeInt>
      <X>0</X>
      <Y>0</Y>
      <Width>1920</Width>
      <Height>1080</Height>
      <EngineName>GV_1</EngineName>
      <EngineId>b2b99fed-4fec-461e-8ed7-a52e134c649c</EngineId>
      <Alias>video</Alias>
      <MasterTemplateId>00000000-0000-0000-0000-000000000000</MasterTemplateId>
      <MasterPlaceHolderName></MasterPlaceHolderName>
      <InputsNames>
        <InputName Name="Tracking Data 2" />
      </InputsNames>
      <Contents />
      <OrderedInputs>
        <OrderedInput Id="Tracking Data 2" />
      </OrderedInputs>
    </Content>
  </Contents>
</Result>
```

где:

- `Scenarios` - список историй плейлиста.
  - `Scenario` - структура конкретной истории.
    - `Name` - имя истории. Тип `String`.
    - `EventList` - список идентификаторов элементов истории.
      - `EventId`- идентификатор элемента. Тип `Guid`. Используется для отображения элементов внутри истории.
- `Events` - список элементов плейлиста.
  - `Event` - структура конкретного элемента.
    - `Name` - имя элемента. Тип `String`.
    - `State` - вызываемое состояние шаблона. Тип `String`. Используется для вызова состояния шаблона при запуске элемента.
    - `Comment` - комментарий к элементу. Тип `String`. Используется для отображения сопроводительной информации об элементе.
    - `EventGroupId` - идентификатор группы элемента. Тип `Guid`. Используется для группировки элементов внутри истории.
    - `Variables` - список переменных шаблона.
      - `Variable` - структура конкретной переменной.
        - `Name` - имя переменной. Тип `String`.
        - `Type` - тип переменной. Тип `String`. Используется для отображения соответствующего типа контроля: "Text", "Media", "Float", "Boolean", "Color", "Point2D", "Point3D", "ComboBox".
        - `Value` - значение переменной. Тип `String`.
        - `ResetMedia` - настройка сброса времени проигрывания при запуске элемента, если тип переменной "Media" и в качестве значения указан видеофайл или секвенция. Тип `Boolean`. Если `true`, то происходит сброс на начало.
        - `Loop` - настройка цикличного воспроизведения, если тип переменной "Media" и в качестве значения указан видеофайл или секвенция. Тип `Boolean`. Если `true`, то цикличное воспроизведение включено.
        - `EditableFieldSettings` - настройки переменной.
          - `ListValues` - список подготовленных значений. Если тип переменной `ComboBox`.
            - `ListValue` - подготовленное значение. Тип `String`. Используется для отображения подготовленных значений в контроле.
- `Templates` - список шаблонов, используемых элементами плейлиста. [Описание представлено выше](##Получение-списка-шаблонов)
- `Contents` - список контейнеров, на которых проигрываются шаблоны элементов истории.
  - `Name` - имя контейнера. Тип `String`. Используется для отображения контейнера, на котором проигрывается шаблон.

## Получение содержимого истории

Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetScenario" MessageId="324">
  <ScenarioId>db246fe8-b911-4d03-819c-930a241a26f3</ScenarioId>
</Command>
```

где:

- `ScenarioId`- идентификатор истории. Тип `Guid`. Используется для запроса её содержимого.

В ответ вы получите сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetScenario" MessageId="324">
  <Scenario>
    <Id>db246fe8-b911-4d03-819c-930a241a26f3</Id>
    <Name>New Story</Name>
    <Created>28.10.2021 12:51:32</Created>
    <Changed>28.10.2021 12:58:36</Changed>
    <IsSystem>false</IsSystem>
    <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
    <Duration>0</Duration>
    <CurrentTime>0</CurrentTime>
    <IsDefault>false</IsDefault>
    <EventList>
      <EventId>774b3626-6247-4c7f-9506-179063fdb8ac</EventId>
      <EventId>34a00dab-b626-438c-aaba-a9942264a0fe</EventId>
    </EventList>
  </Scenario>
  <Events>
    <Event>
      <Id>34a00dab-b626-438c-aaba-a9942264a0fe</Id>
      <Name>BreakingNews_1</Name>
      <Created>28.10.2021 12:52:53</Created>
      <Changed>28.10.2021 12:52:53</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
      <ScenarioId>db246fe8-b911-4d03-819c-930a241a26f3</ScenarioId>
      <TemplateId>c2a743ef-161b-4444-8f3f-68a44e5062db</TemplateId>
      <ContentId>59b9725f-5030-4028-8fb3-314f5e4f24fb</ContentId>
      <Duration>0</Duration>
      <CurrentTime>0</CurrentTime>
      <StatusInt>0</StatusInt>
      <TemplateTypeInt>1</TemplateTypeInt>
      <State>IN</State>
      <BeginTime>0</BeginTime>
      <BeginTimeOffset>0</BeginTimeOffset>
      <EventGroupId>00000000-0000-0000-0000-000000000000</EventGroupId>
      <DesiredStatusInt>0</DesiredStatusInt>
      <AllowRuntimeChange>false</AllowRuntimeChange>
      <Comment />
      <Variables>
        <Variable Id="0#13#14//&quot;Layer 6&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="@Color" Type="Color" Value="some Value 765" FieldId="1e72687d-5577-4dce-a40e-4858abe1aa25" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Rotation&quot;" Name="Back Rotation" Type="Float" Value="some Value 765" FieldId="0be3bd9d-7848-4436-9f98-f107f1422d1a" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Y Position&quot;" Name="Back Y Position" Type="Boolean" Value="some Value 765" FieldId="afc1c7be-2bad-4047-ae11-1d950f0a168b" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="0" MaxValue="100" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale Title" Type="Point2D" Value="some Value 765" FieldId="71c15a7a-2235-4e0c-8a7f-43d042c106dc" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Anchor Point&quot;" Name="Subtitle Anchor Point" Type="Point3D" Value="some Value 765" FieldId="41c718c5-d758-4738-bcb3-bacc11bbd5c6" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="ComboBox" Value="CARROT ENGINE FLYING HIGH" FieldId="ded556f8-d996-4e72-96df-8ba3d6990ce0" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues>
            <ListValue>value 1</ListValue>
            <ListValue>value 2</ListValue>
            <ListValue>value 3</ListValue>
            <ListValue>value 4</ListValue>
            <ListValue>value 5</ListValue>
            <ListValue>value 6</ListValue>
          </ListValues>
        </Variable>
        <Variable Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" Value="BREAKING NEWS" FieldId="e32f9bd9-7029-4154-a9bd-e13b63d6efcc" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
      </Variables>
    </Event>
    <Event>
      <Id>774b3626-6247-4c7f-9506-179063fdb8ac</Id>
      <Name>BreakingNews_7_Pos</Name>
      <Created>28.10.2021 12:58:36</Created>
      <Changed>28.10.2021 12:58:36</Changed>
      <IsSystem>false</IsSystem>
      <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
      <ScenarioId>db246fe8-b911-4d03-819c-930a241a26f3</ScenarioId>
      <TemplateId>2cce380f-99f5-426a-be96-fd8a902335fc</TemplateId>
      <ContentId>48ca644a-32d2-441a-ba29-759506b2003d</ContentId>
      <Duration>0</Duration>
      <CurrentTime>0</CurrentTime>
      <StatusInt>0</StatusInt>
      <TemplateTypeInt>1</TemplateTypeInt>
      <State>IN</State>
      <BeginTime>0</BeginTime>
      <BeginTimeOffset>0</BeginTimeOffset>
      <EventGroupId>00000000-0000-0000-0000-000000000000</EventGroupId>
      <DesiredStatusInt>0</DesiredStatusInt>
      <AllowRuntimeChange>false</AllowRuntimeChange>
      <Comment />
      <Variables>
        <Variable Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Back Plate Scale" Type="Point3D" Value="100,100,100" FieldId="30b3c78a-d1d5-4573-853a-16e83dcf7d1f" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="100,100,100" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#7//&quot;logo&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="Logo Position" Type="Point3D" Value="214.5,903.5,0" FieldId="7176a257-32af-4918-83c0-3e9675877969" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="214.5,903.5,0" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="Text" Value="" FieldId="a9bdab6c-bd0b-4546-845d-4b9679bff7e3" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
        <Variable Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" Value="" FieldId="59f4ba7d-9f73-4370-9e17-47bff562512d" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <EditableFieldSettings>
            <FieldTypeInt>0</FieldTypeInt>
            <IsStill>true</IsStill>
            <NativeFrameRate>50</NativeFrameRate>
            <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
            <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
          </EditableFieldSettings>
          <ListValues />
        </Variable>
      </Variables>
    </Event>
  </Events>
  <Templates>
    <Template>
      <Id>c2a743ef-161b-4444-8f3f-68a44e5062db</Id>
      <Name>BreakingNews_1</Name>
      <Created>26.02.2021 21:21:52</Created>
      <Changed>29.04.2021 18:01:56</Changed>
      <IsSystem>false</IsSystem>
      <XmlData></XmlData>
      <TemplateTypeInt>1</TemplateTypeInt>
      <TemplateGroupId>012b0032-398f-4687-b603-323fa66d2d52</TemplateGroupId>
      <Rev>32</Rev>
      <IsMaster>False</IsMaster>
      <ClosingState>OUT</ClosingState>
      <DefaultInState>IN</DefaultInState>
      <DefaultOutState>OUT</DefaultOutState>
      <ContentId>59b9725f-5030-4028-8fb3-314f5e4f24fb</ContentId>
      <DataUpdateType>Static</DataUpdateType>
      <DataUpdateState></DataUpdateState>
      <LinkVars>
        <LinkVar Id="0#13#14//&quot;Layer 6&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="@Color" Type="Color" FieldId="1e72687d-5577-4dce-a40e-4858abe1aa25" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Rotation&quot;" Name="Back Rotation" Type="Float" FieldId="0be3bd9d-7848-4436-9f98-f107f1422d1a" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Y Position&quot;" Name="Back Y Position" Type="Boolean" FieldId="afc1c7be-2bad-4047-ae11-1d950f0a168b" DefaultValue="some Value 765" MinValue="0" MaxValue="100" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale Title" Type="Point2D" FieldId="71c15a7a-2235-4e0c-8a7f-43d042c106dc" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Anchor Point&quot;" Name="Subtitle Anchor Point" Type="Point3D" FieldId="41c718c5-d758-4738-bcb3-bacc11bbd5c6" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="ComboBox" FieldId="ded556f8-d996-4e72-96df-8ba3d6990ce0" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues>
            <ListValue>value 1</ListValue>
            <ListValue>value 2</ListValue>
            <ListValue>value 3</ListValue>
            <ListValue>value 4</ListValue>
            <ListValue>value 5</ListValue>
            <ListValue>value 6</ListValue>
          </ListValues>
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" FieldId="e32f9bd9-7029-4154-a9bd-e13b63d6efcc" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
      </LinkVars>
      <StateInfos>
        <StateInfo>
          <State>IN</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.4</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>LOOP</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.88</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>OUT</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.72</StateDuration>
        </StateInfo>
      </StateInfos>
    </Template>
    <Template>
      <Id>2cce380f-99f5-426a-be96-fd8a902335fc</Id>
      <Name>BreakingNews_7_Pos</Name>
      <Created>19.07.2021 14:59:24</Created>
      <Changed>18.10.2021 13:52:37</Changed>
      <IsSystem>false</IsSystem>
      <XmlData></XmlData>
      <TemplateTypeInt>1</TemplateTypeInt>
      <TemplateGroupId>012b0032-398f-4687-b603-323fa66d2d52</TemplateGroupId>
      <Rev>28</Rev>
      <IsMaster>False</IsMaster>
      <ClosingState>OUT</ClosingState>
      <DefaultInState>IN</DefaultInState>
      <DefaultOutState>OUT</DefaultOutState>
      <ContentId>48ca644a-32d2-441a-ba29-759506b2003d</ContentId>
      <DataUpdateType>Static</DataUpdateType>
      <DataUpdateState></DataUpdateState>
      <LinkVars>
        <LinkVar Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Back Plate Scale" Type="Point3D" FieldId="30b3c78a-d1d5-4573-853a-16e83dcf7d1f" DefaultValue="100,100,100" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#7//&quot;logo&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="Logo Position" Type="Point3D" FieldId="7176a257-32af-4918-83c0-3e9675877969" DefaultValue="214.5,903.5,0" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="Text" FieldId="a9bdab6c-bd0b-4546-845d-4b9679bff7e3" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
        <LinkVar Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" FieldId="59f4ba7d-9f73-4370-9e17-47bff562512d" DefaultValue="" MinValue="" MaxValue="" UseDataVars="false">
          <ListValues />
        </LinkVar>
      </LinkVars>
      <StateInfos>
        <StateInfo>
          <State>IN</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.4</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>LOOP</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.88</StateDuration>
        </StateInfo>
        <StateInfo>
          <State>OUT</State>
          <PreviousState />
          <NextState />
          <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
          <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
          <IsSimultaneous>false</IsSimultaneous>
          <Loop>false</Loop>
          <FinishPlay>false</FinishPlay>
          <StateDuration>1.72</StateDuration>
        </StateInfo>
      </StateInfos>
    </Template>
  </Templates>
  <Contents>
    <Content>
      <Id>48ca644a-32d2-441a-ba29-759506b2003d</Id>
      <Name>GV_AE_1</Name>
      <Created>22.09.2021 12:49:13</Created>
      <Changed>18.10.2021 13:57:25</Changed>
      <IsSystem>false</IsSystem>
      <ContentTypeInt>1</ContentTypeInt>
      <X>0</X>
      <Y>0</Y>
      <Width>1920</Width>
      <Height>1080</Height>
      <EngineName>GV_1</EngineName>
      <EngineId>b2b99fed-4fec-461e-8ed7-a52e134c649c</EngineId>
      <Alias>video</Alias>
      <MasterTemplateId>00000000-0000-0000-0000-000000000000</MasterTemplateId>
      <MasterPlaceHolderName></MasterPlaceHolderName>
      <InputsNames>
        <InputName Name="Tracking Data 2" />
      </InputsNames>
      <Contents />
      <OrderedInputs>
        <OrderedInput Id="Tracking Data 2" />
      </OrderedInputs>
    </Content>
  </Contents>
</Command>
```

где:

- `Scenario` - структура истории.
- `Events` - список элементов плейлиста.
- `Templates` - список шаблонов, используемых элементами истории. [Описание представлено выше](##Получение-списка-шаблонов)
- `Contents` - список контейнеров, на которых проигрываются шаблоны элементов истории. [Описание представлено выше](##Получение-полного-содержимого-плейлиста)

## Получение содержимого элемента

Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetEvent" MessageId="340">
  <EventId>34a00dab-b626-438c-aaba-a9942264a0fe</EventId>
</Command>
```

где:

- `EventId` - идентификатор запрашиваемого элемента. Тип `Guid`.

В ответ вы получите сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetEvent" MessageId="340">
  <Event>
    <Id>34a00dab-b626-438c-aaba-a9942264a0fe</Id>
    <Name>BreakingNews_1</Name>
    <Created>28.10.2021 12:52:53</Created>
    <Changed>28.10.2021 13:13:38</Changed>
    <IsSystem>false</IsSystem>
    <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
    <ScenarioId>db246fe8-b911-4d03-819c-930a241a26f3</ScenarioId>
    <TemplateId>c2a743ef-161b-4444-8f3f-68a44e5062db</TemplateId>
    <ContentId>59b9725f-5030-4028-8fb3-314f5e4f24fb</ContentId>
    <Duration>0</Duration>
    <CurrentTime>0</CurrentTime>
    <StatusInt>0</StatusInt>
    <TemplateTypeInt>1</TemplateTypeInt>
    <State>IN</State>
    <BeginTime>0</BeginTime>
    <BeginTimeOffset>0</BeginTimeOffset>
    <EventGroupId>00000000-0000-0000-0000-000000000000</EventGroupId>
    <DesiredStatusInt>0</DesiredStatusInt>
    <AllowRuntimeChange>false</AllowRuntimeChange>
    <Comment />
    <Variables>
      <Variable Id="0#13#14//&quot;Layer 6&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="@Color" Type="Color" Value="some Value 765" FieldId="1e72687d-5577-4dce-a40e-4858abe1aa25" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
      <Variable Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Rotation&quot;" Name="Back Rotation" Type="Float" Value="0.01" FieldId="0be3bd9d-7848-4436-9f98-f107f1422d1a" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
      <Variable Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Y Position&quot;" Name="Back Y Position" Type="Boolean" Value="0" FieldId="afc1c7be-2bad-4047-ae11-1d950f0a168b" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="0" MaxValue="100" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
      <Variable Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale Title" Type="Point2D" Value="0,0" FieldId="71c15a7a-2235-4e0c-8a7f-43d042c106dc" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
      <Variable Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Anchor Point&quot;" Name="Subtitle Anchor Point" Type="Point3D" Value="0,0,0" FieldId="41c718c5-d758-4738-bcb3-bacc11bbd5c6" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
      <Variable Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="ComboBox" Value="CARROT ENGINE FLYING HIGH" FieldId="ded556f8-d996-4e72-96df-8ba3d6990ce0" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues>
          <ListValue>value 1</ListValue>
          <ListValue>value 2</ListValue>
          <ListValue>value 3</ListValue>
          <ListValue>value 4</ListValue>
          <ListValue>value 5</ListValue>
          <ListValue>value 6</ListValue>
        </ListValues>
      </Variable>
      <Variable Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" Value="BREAKING NEWS" FieldId="e32f9bd9-7029-4154-a9bd-e13b63d6efcc" ResetMedia="true" UseTimecode="false" Loop="false" Locked="false" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
        <EditableFieldSettings>
          <FieldTypeInt>0</FieldTypeInt>
          <IsStill>true</IsStill>
          <NativeFrameRate>50</NativeFrameRate>
          <DataStreamTableId>00000000-0000-0000-0000-000000000000</DataStreamTableId>
          <DataStreamCellId>00000000-0000-0000-0000-000000000000</DataStreamCellId>
        </EditableFieldSettings>
        <ListValues />
      </Variable>
    </Variables>
  </Event>
  <Template>
    <Id>c2a743ef-161b-4444-8f3f-68a44e5062db</Id>
    <Name>BreakingNews_1</Name>
    <Created>26.02.2021 21:21:52</Created>
    <Changed>29.04.2021 18:01:56</Changed>
    <IsSystem>false</IsSystem>
    <XmlData></XmlData>
    <TemplateTypeInt>1</TemplateTypeInt>
    <TemplateGroupId>012b0032-398f-4687-b603-323fa66d2d52</TemplateGroupId>
    <Rev>32</Rev>
    <IsMaster>False</IsMaster>
    <ClosingState>OUT</ClosingState>
    <DefaultInState>IN</DefaultInState>
    <DefaultOutState>OUT</DefaultOutState>
    <ContentId>59b9725f-5030-4028-8fb3-314f5e4f24fb</ContentId>
    <DataUpdateType>Static</DataUpdateType>
    <DataUpdateState></DataUpdateState>
    <LinkVars>
      <LinkVar Id="0#13#14//&quot;Layer 6&quot;//&quot;Transform&quot;//&quot;Position&quot;" Name="@Color" Type="Color" FieldId="1e72687d-5577-4dce-a40e-4858abe1aa25" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues />
      </LinkVar>
      <LinkVar Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Rotation&quot;" Name="Back Rotation" Type="Float" FieldId="0be3bd9d-7848-4436-9f98-f107f1422d1a" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues />
      </LinkVar>
      <LinkVar Id="0#13#16//&quot;Layer 3&quot;//&quot;Transform&quot;//&quot;Y Position&quot;" Name="Back Y Position" Type="Boolean" FieldId="afc1c7be-2bad-4047-ae11-1d950f0a168b" DefaultValue="some Value 765" MinValue="0" MaxValue="100" UseDataVars="false">
        <ListValues />
      </LinkVar>
      <LinkVar Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Transform&quot;//&quot;Scale&quot;" Name="Scale Title" Type="Point2D" FieldId="71c15a7a-2235-4e0c-8a7f-43d042c106dc" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues />
      </LinkVar>
      <LinkVar Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Anchor Point&quot;" Name="Subtitle Anchor Point" Type="Point3D" FieldId="41c718c5-d758-4738-bcb3-bacc11bbd5c6" DefaultValue="some Value 765" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues />
      </LinkVar>
      <LinkVar Id="0#13#12//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="SubTitle" Type="ComboBox" FieldId="ded556f8-d996-4e72-96df-8ba3d6990ce0" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues>
          <ListValue>value 1</ListValue>
          <ListValue>value 2</ListValue>
          <ListValue>value 3</ListValue>
          <ListValue>value 4</ListValue>
          <ListValue>value 5</ListValue>
          <ListValue>value 6</ListValue>
        </ListValues>
      </LinkVar>
      <LinkVar Id="0#13#13//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" Name="Title" Type="Text" FieldId="e32f9bd9-7029-4154-a9bd-e13b63d6efcc" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" UseDataVars="false">
        <ListValues />
      </LinkVar>
    </LinkVars>
    <StateInfos>
      <StateInfo>
        <State>IN</State>
        <PreviousState />
        <NextState />
        <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
        <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
        <IsSimultaneous>false</IsSimultaneous>
        <Loop>false</Loop>
        <FinishPlay>false</FinishPlay>
        <StateDuration>1.4</StateDuration>
      </StateInfo>
      <StateInfo>
        <State>LOOP</State>
        <PreviousState />
        <NextState />
        <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
        <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
        <IsSimultaneous>false</IsSimultaneous>
        <Loop>false</Loop>
        <FinishPlay>false</FinishPlay>
        <StateDuration>1.88</StateDuration>
      </StateInfo>
      <StateInfo>
        <State>OUT</State>
        <PreviousState />
        <NextState />
        <DoNotPlayPreviousStateIfNotShowing>false</DoNotPlayPreviousStateIfNotShowing>
        <ApplyVariableToPreviousState>false</ApplyVariableToPreviousState>
        <IsSimultaneous>false</IsSimultaneous>
        <Loop>false</Loop>
        <FinishPlay>false</FinishPlay>
        <StateDuration>1.72</StateDuration>
      </StateInfo>
    </StateInfos>
  </Template>
</Command>
```

где:

- `Event` - структура запрашиваемого элемента. [Описание представлено выше](##Получение-полного-содержимого-плейлиста)
- `Template` - структура шаблона, используемого элементом. [Описание представлено выше](##Получение-списка-шаблонов)

## Виды сообщений-уведомлений

Данный вид сообщений получают все клиенты, когда в базе данных на сервере происходят изменения.

1. Изменение группы плейлистов. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
      <ParentId>00000000-0000-0000-0000-000000000000</ParentId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор группы плейлистов, в которую добавлен новый     плейлист. Тип `Guid`. Необходим для [обновления содержимого группы]   (##Получение-списка-плейлистов,-хранящихся-на-сервере).
    - `ParentId` - идентификатор родительской группы изменённой группы плейлистов.    Тип `Guid`. Необходим для [обновления содержимого родительской группы]  (##Получение-списка-плейлистов,-хранящихся-на-сервере).

1. Изменение плейлиста. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistChanged">
      <PlaylistId>0f3acd25-7dc5-4596-bd68-8e3b2b09e1d2</PlaylistId>
      <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, который был изменён. Тип `Guid`.    Необходим для [обновления содержимого плейлиста]  (##Получение-полного-содержимого-плейлиста).
    - `PlaylistGroupId` - идентификатор группы, к которой относится плейлист. Тип     `Guid`. Необходим для [обновления содержимого родительской группы]    (##Получение-списка-плейлистов,-хранящихся-на-сервере).

1. Удаление плейлиста. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistRemoved">
      <PlaylistId>0f3acd25-7dc5-4596-bd68-8e3b2b09e1d2</PlaylistId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, в который был удален. Тип `Guid`.     Необходим для актуализации содержимого открытой группы плейлистов.

1. Изменение истории. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="ScenarioChanged">
      <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
      <ScenarioId>db246fe8-b911-4d03-819c-930a241a26f3</ScenarioId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, к которому относится история. Тип     `Guid`. Необходим для [обновления содержимого плейлиста]    (##Получение-полного-содержимого-плейлиста).
    - `ScenarioId` - идентификатор истории, которая была изменена. Тип `Guid`.    Необходим для [получения содержимого истории](##Получение-содержимого-истории).

1. Изменение элемента плейлиста. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="EventChanged">
      <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
      <EventId>34a00dab-b626-438c-aaba-a9942264a0fe</EventId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, к которому относится элемент. Тип `Guid`. Необходим для [обновления содержимого плейлиста]    (##Получение-полного-содержимого-плейлиста).
    - `EventId` - идентификатор элемента, который был изменён. Тип `Guid`.    Необходим для [получения содержимого элемента](##Получение-содержимого-элемента).

1. Блокировка переменной элемента. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="EventFieldLockChange">
      <EventId>d0c29618-ce79-43b2-bb4e-0d336a22961c</EventId>
      <FieldId>0be3bd9d-7848-4436-9f98-f107f1422d1a</FieldId>
      <Locked>True</Locked>
    </Command>
    ```

    где:

    - `EventId` - идентификатор элемента, который был изменён. Тип `Guid`.    Необходим для [получения содержимого элемента](##Получение-содержимого-элемента).
    - `FieldId` - идентификатор переменной, к которой было применено воздействие    блокировки. Тип `Guid`. Необходим для поиска переменной среди списка переменных элемента.
    - `Locked` - состояние блокировки. Тип `Boolean`. Равно `true`, если переменная заблокирована. Если `false` - переменная доступна для редактирования.

1. Изменение текущего статуса элемента. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="EventStatusChanged">
      <PlaylistId>9eb8a28f-fc2e-49c5-b710-9c15a4fdf5e8</PlaylistId>
      <EventId>04394e9e-63fb-4c88-a85e-15633935d9b6</EventId>
      <Status>1</Status>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, к которому относится элемент. Тип     `Guid`. Необходим для [обновления содержимого плейлиста]    (##Получение-полного-содержимого-плейлиста).
    - `EventId` - идентификатор элемента, статус которого был изменён. Необходим для [получения содержимого элемента](##Получение-содержимого-элемента).
    - `Status` - статус элемента. Тип `Int`. Необходим для отображения состояния элемента и шаблона (Unloaded = 0, Loading = 1, Ready = 2, Active = 3).

1. Завершение проигрывания состояния шаблона, вызванного запуском элемента. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="EventStateCompleted">
      <PlaylistId>9eb8a28f-fc2e-49c5-b710-9c15a4fdf5e8</PlaylistId>
      <EventId>04394e9e-63fb-4c88-a85e-15633935d9b6</EventId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, к которому относится элемент. Тип `Guid`. Необходим для [обновления содержимого плейлиста]    (##Получение-полного-содержимого-плейлиста).
    - `EventId` - идентификатор элемента, статус которого был изменён. Тип `Guid`. Необходим для [получения содержимого элемента](##Получение-содержимого-элемента).

1. Изменение группы шаблонов. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="TemplateGroupChanged">
      <ParentId>62824b96-d977-4b16-92ce-966eba09bd27</ParentId>
      <TemplateGroupId>bea3bca2-1499-4a9c-80bd-049dff037256</TemplateGroupId>
    </Command>
    ```

    где:

    - `ParentId` - идентификатор родительской группы, к которой относится изменённая группа. Тип    `Guid`. Необходим для [обновления содержимого родительской группы](##Получение-списка-шаблонов)   .
    - `TemplateGroupId` - идентификатор группы, которая была изменена. Тип `Guid`. Необходим для    [получения содержимого группы](##Получение-списка-шаблонов).

1. Изменение шаблона. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="TemplateChanged">
      <TemplateGroupId>62824b96-d977-4b16-92ce-966eba09bd27</TemplateGroupId>
      <TemplateId>065a20e1-e60a-4872-b212-ed5c5bd1e266</TemplateId>
    </Command>
    ```

    где:

    - `ParentId` - идентификатор группы, к которой относится изменённый шаблон. Тип `Guid`.     Необходим для [обновления содержимого группы](##Получение-списка-шаблонов).
    - `TemplateGroupId` - идентификатор шаблона, который был изменён. Тип `Guid`. Необходим для     актуализации отображения содержимого группы шаблонов.

1. Изменение контейнера, в котором проигрываются привязанные шаблоны. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="ContentChanged">
      <ContentId>48ca644a-32d2-441a-ba29-759506b2003d</ContentId>
    </Command>
    ```

    где:

    - `ContentId` - идентификатор контейнера. Тип `Guid`. Необходим для актуализации отображения    содержимого имени контейнера в плейлисте.

1. Изменение списка доступных серверов (в случае использования функции резервирования). Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="ReplicationHostsChanged">
      <ReplicationHosts>,192.168.1.39</ReplicationHosts>
    </Command>
    ```

    где:

    - `ReplicationHosts` - список ip-адресов доступных серверов. Тип `String`. Необходим для   актуализации локального списка список ip-адресов.

1. Команда на переключение к другому серверу. Приходит, если мастером стал другой сервер. Сообщение имеет вид:

    ```xml
    <Command CmdGroup="Notifications" CmdName="ReconnectToMaster">
      <ReplicationHosts>192.168.1.39,192.168.1.203</ReplicationHosts>
    </Command>
    ```

    где:

    - `ReplicationHosts` - список ip-адресов доступных серверов. Тип `String`. Необходим для   актуализации локального списка список ip-адресов. После обновления списка следует   переподключиться на первый ip-адрес в списке.

## Добавление нового плейлиста

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="AddPlaylist" MessageId="16">
      <Playlist>
        <Id>29497412-9092-4efa-b975-3f513968f8d0</Id>
        <Name>NewPlaylistName</Name>
        <Created>01.01.0001 0:00:00</Created>
        <Changed>01.01.0001 0:00:00</Changed>
        <IsSystem>false</IsSystem>
        <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
        <ScenarioList />
      </Playlist>
    </Command>
    ```

    где:

    - `Playlist` - структура плейлиста.
      - `Id` - идентификатор плейлиста. Тип `Guid`. Генерируется на стороне клиента.
      - `Name` - имя плейлиста. Тип `String`. Задается пользователем.
      - `PlaylistGroupId` - идентификатор существующей группы плейлистов, к которой будет     относиться новый плейлист. Тип `Guid`. Выбирается из идентификаторов групп, полученных при    [запросе списка плейлистов](##Получение-списка-плейлистов,-хранящихся-на-сервере).

1. В ответ вы получите сообщение-уведомление вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
      <ParentId>00000000-0000-0000-0000-000000000000</ParentId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор группы плейлистов, в которую добавлен новый плейлист. Тип     `Guid`. Необходим для [обновления содержимого группы](##Получение-списка-плейлистов,    -хранящихся-на-сервере).
    - `ParentId` - идентификатор родительской группы изменённой группы плейлистов. Тип `Guid`.    Необходим для [обновления содержимого родительской группы](##Получение-списка-плейлистов,   -хранящихся-на-сервере).

1. [Запросите созданный плейлист с сервера](##Получение-полного-содержимого-плейлиста).

## Изменение имени существующего плейлиста

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="UpdatePlaylist" MessageId="42">
      <Playlist>
        <Id>81cfb903-5c9c-4f60-9c21-59f9f2065198</Id>
        <Name>5</Name>
        <Created>27.10.2021 15:39:28</Created>
        <Changed>27.10.2021 15:39:28</Changed>
        <IsSystem>false</IsSystem>
        <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
        <EngineList />
        <ScenarioList />
      </Playlist>
    </Command>
    ```

    где:

    - `Playlist` - структура плейлиста.
      - `Id` - идентификатор плейлиста. Тип `Guid`. Генерируется на стороне клиента.
      - `Name` - имя плейлиста. Тип `String`. Задается пользователем.
      - `PlaylistGroupId` - идентификатор группы плейлистов, к которой относится плейлист. Тип    `Guid`.

1. В ответ вы получите сообщение-уведомление вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistChanged">
      <PlaylistId>81cfb903-5c9c-4f60-9c21-59f9f2065198</PlaylistId>
      <PlaylistGroupId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</PlaylistGroupId>
    </Command>>
    ```

    где:

    - `PlaylistId` - идентификатор изменённого плейлиста. Тип `Guid`. Необходим для [обновления     содержимого плейлиста](##Получение-полного-содержимого-плейлиста).
    - `PlaylistGroupId` - идентификатор группы плейлистов, в которой плейлист был изменен. Тип    `Guid`. Необходим для [обновления содержимого группы](##Получение-списка-плейлистов,  -хранящихся-на-сервере).

## Копирование плейлиста в новую группу

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="CopyPlaylist" MessageId="53">
      <PlaylistId>fb36f71e-8ab5-4c10-a82d-1eba0df9659b</PlaylistId>
      <NewGroupId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</NewGroupId>
      <DeleteOriginal>false</DeleteOriginal>
      <NewPlaylistId>2dd928bb-9897-47a5-b781-1c5a7684678d</NewPlaylistId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, который необходимо скопировать. Тип `Guid`.
    - `NewGroupId` - идентификатор группы плейлистов, в которую необходимо скопировать плейлист.    Тип `Guid`.
    - `DeleteOriginal` - команда, удаляющая плейлист, копия которого создается. Тип `Boolean`.    Если `true` - то будет произведено удаление плейлиста в исходной группе.
    - `NewPlaylistId` - идентификатор копии плейлиста. Тип `Guid`. Генерируется на стороне клиента.

1. В ответ вы получите сообщения-уведомление вида (два сообщения в случае `DeleteOriginal` = `true`):

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</PlaylistGroupId>
      <ParentId>73f49bb5-55e6-4ef1-8191-e0dc39edb389</ParentId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор изменённой группы плейлистов, в которую добавлена копия     плейлиста / из которой удален плейлист (если `DeleteOriginal` = `true`). Тип `Guid`. Необходим    для [обновления содержимого группы](##Получение-списка-плейлистов,-хранящихся-на-сервере).
    - `ParentId` - идентификатор родительской группы изменённой группы плейлистов. Тип `Guid`.    Необходим для [обновления содержимого родительской группы](##Получение-списка-плейлистов,   -хранящихся-на-сервере).

## Удаление плейлиста

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="DeletePlaylist" MessageId="11">
      <PlaylistId>8ed1af3b-1323-434e-b47a-ddbafe028810</PlaylistId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор плейлиста, который необходимо удалить. Тип `Guid`.

1. В ответ вы получите два сообщения-уведомления вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistRemoved">
      <PlaylistId>8ed1af3b-1323-434e-b47a-ddbafe028810</PlaylistId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор удалённого плейлиста. Тип `Guid`. Необходим для актуализации     содержимого открытой группы.

    и

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</PlaylistGroupId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор изменённой группы плейлистов из которой удален плейлист.    Тип `Guid`. Необходим для [обновления содержимого группы](##Получение-списка-плейлистов,  -хранящихся-на-сервере).

## Добавление новой группы плейлистов

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="AddPlaylistGroup" MessageId="13">
  <ObjectGroup>
    <Id>944dd2a8-8ad3-4aea-8fc3-9e7601b2b673</Id>
    <Name>NewFolderName</Name>
    <Created>01.01.0001 0:00:00</Created>
    <Changed>01.01.0001 0:00:00</Changed>
    <IsSystem>false</IsSystem>
    <ParentId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</ParentId>
  </ObjectGroup>
</Command>
```

где:

- `Id` - идентификатор группы плейлистов. Тип `Guid`. Генерируется на стороне клиента.
- `Name` - желаемое имя группы. Тип `String`.
- `ParentId` - идентификатор родительской группы плейлистов, где будет создана новая группа. Тип `Guid`.

. В ответ вы получите сообщение-уведомление вида:

```xml
<Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
  <PlaylistGroupId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</PlaylistGroupId>
</Command>
```

где:

- `PlaylistGroupId` - идентификатор родительской группы плейлистов, где была создана новая группа. Тип `Guid`. Необходим для актуализации содержимого открытой группы.

## Изменение имени группы плейлистов

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="UpdatePlaylistGroup" MessageId="39">
      <ObjectGroup>
        <Id>944dd2a8-8ad3-4aea-8fc3-9e7601b2b673</Id>
        <Name>NewName</Name>
        <Created>28.10.2021 15:34:13</Created>
        <Changed>28.10.2021 15:34:13</Changed>
        <IsSystem>false</IsSystem>
        <ParentId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</ParentId>
      </ObjectGroup>
    </Command>
    ```

    - `Id` - идентификатор группы плейлистов, которую необходимо изменить. Тип `Guid`.
    - `Name` - новое имя изменяемой группы плейлистов. Тип `String`.

1. В ответ вы получите сообщение-уведомление вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>944dd2a8-8ad3-4aea-8fc3-9e7601b2b673</PlaylistGroupId>
      <ParentId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</ParentId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор изменённой группы плейлистов, в которую добавлена копия     плейлиста / из которой удален плейлист (если `DeleteOriginal` = `true`). Тип `Guid`. Необходим    для [обновления содержимого группы](##Получение-списка-плейлистов,-хранящихся-на-сервере).
    - `ParentId` - идентификатор родительской группы изменённой группы плейлистов. Тип `Guid`.    Необходим для [обновления содержимого родительской группы](##Получение-списка-плейлистов,   -хранящихся-на-сервере).

## Удаление группы плейлистов

1. Для этого [по соединению для обмена сообщениями](connection.md) отправьте на сервер сообщение вида:

    ```xml
    <Command CmdGroup="Playlists" CmdName="DeletePlaylistGroup" MessageId="47">
      <PlaylistGroupId>944dd2a8-8ad3-4aea-8fc3-9e7601b2b673</PlaylistGroupId>
    </Command>
    ```

    - `PlaylistGroupId` - идентификатор группы плейлистов, которую необходимо     удалить. Тип `Guid`.

1. В ответ вы получите сообщение-уведомление вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistGroupChanged">
      <PlaylistGroupId>f0d24e1a-32b2-4cef-85a5-3b69e59df93a</PlaylistGroupId>
    </Command>
    ```

    где:

    - `PlaylistGroupId` - идентификатор изменённой группы плейлистов, в которую добавлена копия плейлиста / из которой удален плейлист. Тип `Guid`. Необходим для [обновления содержимого группы](##Получение-списка-плейлистов,-хранящихся-на-сервере).

    Если в группе были плейлисты, они тоже удалятся. На каждый удалённый плейлист также придет сообщение-уведомление вида:

    ```xml
    <Command CmdGroup="Notifications" CmdName="PlaylistRemoved">
      <PlaylistId>8ed1af3b-1323-434e-b47a-ddbafe028810</PlaylistId>
    </Command>
    ```

    где:

    - `PlaylistId` - идентификатор удалённого плейлиста. Тип `Guid`. Необходим для актуализации содержимого открытой группы.
