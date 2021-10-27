# API для работы с плейлистами

## Введение

### Возможности API

Используя данный API вы сможете полноценно управлять плейлистами для прогрывания графики реального времени:

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

Для этого отправьте на сервер сообщение вида:

```xml
<Command CmdGroup="Playlists" CmdName="GetPlaylists" MessageId="659">
  <PlaylistGroupId>00000000-0000-0000-0000-000000000000</PlaylistGroupId>
</Command>
```

где:

- `MessageId` - иденификатор сообщения. Тип `long`. В каждом последующем сообщении клиент должен увеличивать параметр на 1.
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

- `PlaylistGroups` - список дочерних групп запрашиваемой группы.
- `Id` - иденификатор объекта. Тип `Guid`. Используется для запросов содержимого объекта с сервера.
- `Name` - имя объекта. Тип `String`.
- `ParentId` - идентификатор родительской группы, к которой относится дочерняя группа. Используется для отслеживания состояния родительского объекта.
- `Playlists` - список плейлистов запрашиваемой группы.
- `PlaylistGroupId` - идентификатор группы плейлистов, к которой относится плейлист. Используется для отслеживания состояния родительского объекта.
- `ScenarioList` - список идентификаторов историй данного плейлиста.
- `ScenarioId` - идентификатор истории. Тип `Guid`. Используется для запроса содержимого истории.

## Получение полного содержимого плейлиста

Для этого отправьте на сервер сообщение вида:

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
      <XmlData>
        <AEProject>
          <Carrot>
            <Data name="01_breaking_news%20(converted)" compname="ae_01_breaking_news" compid="13" ismaster="false">
              <Variable name="Color" id="0#13#1//&quot;Blue Solid 1&quot;//&quot;Effects&quot;//&quot;Fill&quot;//&quot;Color&quot;" FieldId="2341742c-7e5f-4b71-bd4b-3d73aa2aa74d" type="Color" DefaultValue="1,0,0,1" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="Position" id="0#13#8//&quot;logo&quot;//&quot;Transform&quot;//&quot;Position&quot;" FieldId="4c6e8a64-80bf-4bf2-b905-1768ffb3c31e" type="Point3D" DefaultValue="214.5,903.5,0" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="Source Text" id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Text&quot;//&quot;Source Text&quot;" FieldId="a38170e0-ae8d-4ae8-9d49-5a15f63a587f" type="Text" DefaultValue="CARROT ENGINE FLYING HIGH" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="Layer 3" id="1#13#17//&quot;Layer 3&quot;" FieldId="4f1d06ff-06f1-4ac8-bb62-b0902de90a48" type="Media" DefaultValue="" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="X Position" id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;X Position&quot;" FieldId="5e7758a3-239a-429b-855c-3b383648f728" type="Float" DefaultValue="0" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="UseOpacity" id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Opacity&quot;" FieldId="b09309d1-42c8-41ef-8771-651d5a8a74a2" type="Boolean" DefaultValue="0" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="Scale" id="0#13#13//&quot;CARROT ENGINE FLYING HIGH&quot;//&quot;Transform&quot;//&quot;Scale&quot;" FieldId="9537c341-379c-47a7-92dd-cc1d8d7bc111" type="Point2D" DefaultValue="100,100" MinValue="" MaxValue="" ListValues="" UseDataVars="false" />
              <Variable name="Source Prepared Text" id="0#13#14//&quot;BREAKING NEWS&quot;//&quot;Text&quot;//&quot;Source Text&quot;" FieldId="77fe9d37-000b-4434-81f6-01a2226508b7" type="ComboBox" DefaultValue="BREAKING NEWS" MinValue="" MaxValue="" ListValues="First Prepared Text;Second Prepared Text" UseDataVars="false" />
            </Data>
            <Scripts>
              <Script Type="Startup" Place="Local" Enabled="true">
                <ScriptSource></ScriptSource>
                <Filename></Filename>
              </Script>
              <Script Type="SetState" Place="Local" Enabled="true">
                <ScriptSource></ScriptSource>
                <Filename></Filename>
              </Script>
              <Script Type="ProcessFrame" Place="Local" Enabled="true">
                <ScriptSource></ScriptSource>
                <Filename></Filename>
              </Script>
              <Script Type="Command" Place="Local" Enabled="true">
                <ScriptSource></ScriptSource>
                <Filename></Filename>
              </Script>
            </Scripts>
          </Carrot>
          <Compositions>
            <Composition name="ae_01_breaking_news" id="13" comment="" typeName="Composition" pixelAspect="1" duration="6" frameDuration="0.04" frameRate="25" height="1080" width="1920" hasAudio="false" hasVideo="false">
              <Attributes bgColor="0,0,0" displayStartTime="0" motionBlur="false" workAreaStart="0" workAreaDuration="5.04" activeCameraIndex="-1" />
              <Markers>
                <Property name="Marker" matchName="ADBE Marker" active="true" propertyValueType="MARKER">
                  <MarkerValue chapter="" comment="OUT" cuePointName="" duration="0" eventCuePoint="true" frameTarget="" url="" time="0" />
                  <MarkerValue chapter="" comment="IN" cuePointName="" duration="0" eventCuePoint="true" frameTarget="" url="" time="1.4" />
                  <MarkerValue chapter="" comment="LOOP" cuePointName="" duration="0" eventCuePoint="true" frameTarget="" url="" time="3.28" />
                  <MarkerValue chapter="" comment="OUT" cuePointName="" duration="0" eventCuePoint="true" frameTarget="" url="" time="5" />
                </Property>
              </Markers>
              <Layers>
                <Layer name="Blue Solid 1" type="AVLayer">
                  <Attributes enabled="true" index="1" inPoint="0" isNameSet="false" locked="false" name="Blue Solid 1" nullLayer="false" outPoint="6" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="200" height="200" isNameFromSource="true" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="154" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="1">
                      <Properties name="Fill" matchName="ADBE Fill" active="true" isEffect="true" isMask="false" enabled="true" numProperties="8">
                        <Property name="Fill Mask" matchName="ADBE Fill-0001" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="MASK_INDEX" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="All Masks" matchName="ADBE Fill-0007" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                        <Property name="Color" matchName="ADBE Fill-0002" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="COLOR" expressionEnabled="false" numKeys="0" minValue="1,0,0,1" maxValue="1,0,0,1" value="1,0,0,1" />
                        <Property name="Invert" matchName="ADBE Fill-0006" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                        <Property name="Horizontal Feather" matchName="ADBE Fill-0003" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                        <Property name="Vertical Feather" matchName="ADBE Fill-0004" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                        <Property name="Opacity" matchName="ADBE Fill-0005" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        <Properties name="Compositing Options" matchName="ADBE Effect Built In Params" active="true" isEffect="false" isMask="false" enabled="true" numProperties="3">
                          <Properties name="Masks" matchName="ADBE Effect Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                          <Property name="Effect Opacity" matchName="ADBE Effect Mask Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                          <Property name="GPU Rendering" matchName="ADBE Force CPU GPU" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        </Properties>
                      </Properties>
                    </Properties>
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="100,100,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="960,540,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="logo 4" type="AVLayer">
                  <Attributes enabled="false" index="2" inPoint="3.28" isNameSet="true" locked="false" name="logo 4" nullLayer="false" outPoint="4" parentindex="-1" samplingQuality="BILINEAR" startTime="3.28" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="235" height="235" isNameFromSource="false" isTrackMatte="true" motionBlur="false" preserveTransparency="false" quality="BEST" source="149" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="117.5,117.5,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="214.5,894.5,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="1">
                        <Value value="105,105,100" time="3.6" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="flare 3" type="AVLayer">
                  <Attributes enabled="true" index="3" inPoint="3.28" isNameSet="true" locked="false" name="flare 3" nullLayer="false" outPoint="4" parentindex="8" samplingQuality="BILINEAR" startTime="3.28" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="8" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="true" width="757" height="422" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="148" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="ALPHA" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="378.5,211,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="-79.0136692106739,243.760032467858,0" time="3.28" />
                        <Value value="-55.893655823325,243.760032467858,0" time="3.32" />
                        <Value value="-32.7749997925656,243.760032467858,0" time="3.36" />
                        <Value value="-9.65587118772093,243.760032467858,0" time="3.4" />
                        <Value value="13.46527926494,243.760032467858,0" time="3.44" />
                        <Value value="36.5828082932369,243.760032467858,0" time="3.48" />
                        <Value value="59.7014925354715,243.760032467858,0" time="3.52" />
                        <Value value="82.8193829278779,243.760032467858,0" time="3.56" />
                        <Value value="105.940365327743,243.760032467858,0" time="3.6" />
                        <Value value="129.638088848998,243.760032467858,0" time="3.64" />
                        <Value value="153.920526651916,243.760032467858,0" time="3.68" />
                        <Value value="178.188341441301,243.760032467858,0" time="3.72" />
                        <Value value="202.463249365042,243.760032467858,0" time="3.76" />
                        <Value value="226.737440422002,243.760032467858,0" time="3.8" />
                        <Value value="251.013701128664,243.760032467858,0" time="3.84" />
                        <Value value="275.288616838044,243.760032467858,0" time="3.88" />
                        <Value value="299.563222050825,243.760032467858,0" time="3.92" />
                        <Value value="323.839352671208,243.760032467858,0" time="3.96" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="1">
                        <Value value="98.2568346053369,98.2568346053369,100" time="3.62" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="3.28" />
                        <Value value="29.3620071684588" time="3.32" />
                        <Value value="58.7240143369175" time="3.36" />
                        <Value value="88.0860215053764" time="3.4" />
                        <Value value="96" time="3.44" />
                        <Value value="96" time="3.48" />
                        <Value value="96" time="3.52" />
                        <Value value="96" time="3.56" />
                        <Value value="96" time="3.6" />
                        <Value value="96" time="3.64" />
                        <Value value="96" time="3.68" />
                        <Value value="96" time="3.72" />
                        <Value value="96" time="3.76" />
                        <Value value="96" time="3.8" />
                        <Value value="88.0860215053764" time="3.84" />
                        <Value value="58.7240143369176" time="3.88" />
                        <Value value="29.3620071684588" time="3.92" />
                        <Value value="0" time="3.96" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="logo 3" type="AVLayer">
                  <Attributes enabled="false" index="4" inPoint="2" isNameSet="true" locked="false" name="logo 3" nullLayer="false" outPoint="2.72" parentindex="-1" samplingQuality="BILINEAR" startTime="2" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="235" height="235" isNameFromSource="false" isTrackMatte="true" motionBlur="false" preserveTransparency="false" quality="BEST" source="149" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="117.5,117.5,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="214.5,894.5,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="1">
                        <Value value="105,105,100" time="2.32" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="flare 2" type="AVLayer">
                  <Attributes enabled="true" index="5" inPoint="2" isNameSet="true" locked="false" name="flare 2" nullLayer="false" outPoint="2.72" parentindex="8" samplingQuality="BILINEAR" startTime="2" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="8" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="true" width="757" height="422" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="148" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="ALPHA" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="378.5,211,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="-79.0136692106739,243.760032467858,0" time="2" />
                        <Value value="-55.893655823325,243.760032467858,0" time="2.04" />
                        <Value value="-32.7749997925656,243.760032467858,0" time="2.08" />
                        <Value value="-9.65587118772093,243.760032467858,0" time="2.12" />
                        <Value value="13.46527926494,243.760032467858,0" time="2.16" />
                        <Value value="36.5828082932369,243.760032467858,0" time="2.2" />
                        <Value value="59.7014925354715,243.760032467858,0" time="2.24" />
                        <Value value="82.8193829278779,243.760032467858,0" time="2.28" />
                        <Value value="105.940365327743,243.760032467858,0" time="2.32" />
                        <Value value="129.638088848998,243.760032467858,0" time="2.36" />
                        <Value value="153.920526651916,243.760032467858,0" time="2.4" />
                        <Value value="178.188341441301,243.760032467858,0" time="2.44" />
                        <Value value="202.463249365042,243.760032467858,0" time="2.48" />
                        <Value value="226.737440422002,243.760032467858,0" time="2.52" />
                        <Value value="251.013701128664,243.760032467858,0" time="2.56" />
                        <Value value="275.288616838044,243.760032467858,0" time="2.6" />
                        <Value value="299.563222050825,243.760032467858,0" time="2.64" />
                        <Value value="323.839352671208,243.760032467858,0" time="2.68" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="1">
                        <Value value="98.2568346053369,98.2568346053369,100" time="2.34" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="2" />
                        <Value value="29.3620071684588" time="2.04" />
                        <Value value="58.7240143369175" time="2.08" />
                        <Value value="88.0860215053764" time="2.12" />
                        <Value value="96" time="2.16" />
                        <Value value="96" time="2.2" />
                        <Value value="96" time="2.24" />
                        <Value value="96" time="2.28" />
                        <Value value="96" time="2.32" />
                        <Value value="96" time="2.36" />
                        <Value value="96" time="2.4" />
                        <Value value="96" time="2.44" />
                        <Value value="96" time="2.48" />
                        <Value value="96" time="2.52" />
                        <Value value="88.0860215053764" time="2.56" />
                        <Value value="58.7240143369176" time="2.6" />
                        <Value value="29.3620071684588" time="2.64" />
                        <Value value="0" time="2.68" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="logo 2" type="AVLayer">
                  <Attributes enabled="false" index="6" inPoint="0" isNameSet="true" locked="false" name="logo 2" nullLayer="false" outPoint="0.72" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="235" height="235" isNameFromSource="false" isTrackMatte="true" motionBlur="false" preserveTransparency="false" quality="BEST" source="149" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="117.5,117.5,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="214.5,903.5,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="3">
                        <Value value="0,0,100" time="0" />
                        <Value value="1.08322602918233,1.08322602918233,100" time="0.04" />
                        <Value value="4.63314048367077,4.63314048367077,100" time="0.08" />
                        <Value value="11.2223863638677,11.2223863638677,100" time="0.12" />
                        <Value value="21.6471878811208,21.6471878811208,100" time="0.16" />
                        <Value value="37.0077779932525,37.0077779932525,100" time="0.2" />
                        <Value value="58.609980538911,58.609980538911,100" time="0.24" />
                        <Value value="86.3077072841111,86.3077072841111,100" time="0.28" />
                        <Value value="110,110,100" time="0.32" />
                        <Value value="108.293501507128,108.293501507128,100" time="0.36" />
                        <Value value="106.23239526786,106.23239526786,100" time="0.4" />
                        <Value value="104.394698968879,104.394698968879,100" time="0.44" />
                        <Value value="102.90988969943,102.90988969943,100" time="0.48" />
                        <Value value="101.774090730344,101.774090730344,100" time="0.52" />
                        <Value value="100.951787869505,100.951787869505,100" time="0.56" />
                        <Value value="100.404247606368,100.404247606368,100" time="0.6" />
                        <Value value="100.096786513692,100.096786513692,100" time="0.64" />
                        <Value value="100,100,100" time="0.68" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="flare" type="AVLayer">
                  <Attributes enabled="true" index="7" inPoint="0" isNameSet="true" locked="false" name="flare" nullLayer="false" outPoint="0.72" parentindex="8" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="8" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="true" width="757" height="422" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="148" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="ALPHA" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="378.5,211,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="-79.0136692106739,243.760032467858,0" time="0" />
                        <Value value="-55.893655823325,243.760032467858,0" time="0.04" />
                        <Value value="-32.7749997925656,243.760032467858,0" time="0.08" />
                        <Value value="-9.65587118772093,243.760032467858,0" time="0.12" />
                        <Value value="13.46527926494,243.760032467858,0" time="0.16" />
                        <Value value="36.5828082932369,243.760032467858,0" time="0.2" />
                        <Value value="59.7014925354715,243.760032467858,0" time="0.24" />
                        <Value value="82.8193829278779,243.760032467858,0" time="0.28" />
                        <Value value="105.940365327743,243.760032467858,0" time="0.32" />
                        <Value value="129.638088848998,243.760032467858,0" time="0.36" />
                        <Value value="153.920526651916,243.760032467858,0" time="0.4" />
                        <Value value="178.188341441301,243.760032467858,0" time="0.44" />
                        <Value value="202.463249365042,243.760032467858,0" time="0.48" />
                        <Value value="226.737440422002,243.760032467858,0" time="0.52" />
                        <Value value="251.013701128664,243.760032467858,0" time="0.56" />
                        <Value value="275.288616838044,243.760032467858,0" time="0.6" />
                        <Value value="299.563222050825,243.760032467858,0" time="0.64" />
                        <Value value="323.839352671208,243.760032467858,0" time="0.68" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="1">
                        <Value value="98.2568346053369,98.2568346053369,100" time="0.34" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="0" />
                        <Value value="29.3620071684588" time="0.04" />
                        <Value value="58.7240143369175" time="0.08" />
                        <Value value="88.0860215053764" time="0.12" />
                        <Value value="96" time="0.16" />
                        <Value value="96" time="0.2" />
                        <Value value="96" time="0.24" />
                        <Value value="96" time="0.28" />
                        <Value value="96" time="0.32" />
                        <Value value="96" time="0.36" />
                        <Value value="96" time="0.4" />
                        <Value value="96" time="0.44" />
                        <Value value="96" time="0.48" />
                        <Value value="96" time="0.52" />
                        <Value value="88.0860215053764" time="0.56" />
                        <Value value="58.7240143369176" time="0.6" />
                        <Value value="29.3620071684588" time="0.64" />
                        <Value value="0" time="0.68" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="logo" type="AVLayer">
                  <Attributes enabled="true" index="8" inPoint="0" isNameSet="true" locked="false" name="logo" nullLayer="false" outPoint="6" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="235" height="235" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="149" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="117.5,117.5,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="214.5,903.5,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="3">
                        <Value value="0,0,100" time="0" />
                        <Value value="1.08322602918233,1.08322602918233,100" time="0.04" />
                        <Value value="4.63314048367077,4.63314048367077,100" time="0.08" />
                        <Value value="11.2223863638677,11.2223863638677,100" time="0.12" />
                        <Value value="21.6471878811208,21.6471878811208,100" time="0.16" />
                        <Value value="37.0077779932525,37.0077779932525,100" time="0.2" />
                        <Value value="58.609980538911,58.609980538911,100" time="0.24" />
                        <Value value="86.3077072841111,86.3077072841111,100" time="0.28" />
                        <Value value="110,110,100" time="0.32" />
                        <Value value="108.293501507128,108.293501507128,100" time="0.36" />
                        <Value value="106.23239526786,106.23239526786,100" time="0.4" />
                        <Value value="104.394698968879,104.394698968879,100" time="0.44" />
                        <Value value="102.90988969943,102.90988969943,100" time="0.48" />
                        <Value value="101.774090730344,101.774090730344,100" time="0.52" />
                        <Value value="100.951787869505,100.951787869505,100" time="0.56" />
                        <Value value="100.404247606368,100.404247606368,100" time="0.6" />
                        <Value value="100.096786513692,100.096786513692,100" time="0.64" />
                        <Value value="100,100,100" time="0.68" />
                      </Property>
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="3">
                        <Value value="100" time="0" />
                        <Value value="100" time="0.04" />
                        <Value value="100" time="0.08" />
                        <Value value="100" time="0.12" />
                        <Value value="100" time="0.16" />
                        <Value value="100" time="0.2" />
                        <Value value="100" time="0.24" />
                        <Value value="100" time="0.28" />
                        <Value value="100" time="0.32" />
                        <Value value="100" time="0.36" />
                        <Value value="100" time="0.4" />
                        <Value value="100" time="0.44" />
                        <Value value="100" time="0.48" />
                        <Value value="100" time="0.52" />
                        <Value value="100" time="0.56" />
                        <Value value="100" time="0.6" />
                        <Value value="100" time="0.64" />
                        <Value value="100" time="0.68" />
                        <Value value="100" time="0.72" />
                        <Value value="100" time="0.76" />
                        <Value value="100" time="0.8" />
                        <Value value="100" time="0.84" />
                        <Value value="100" time="0.88" />
                        <Value value="100" time="0.92" />
                        <Value value="100" time="0.96" />
                        <Value value="100" time="1" />
                        <Value value="100" time="1.04" />
                        <Value value="100" time="1.08" />
                        <Value value="100" time="1.12" />
                        <Value value="100" time="1.16" />
                        <Value value="100" time="1.2" />
                        <Value value="100" time="1.24" />
                        <Value value="100" time="1.28" />
                        <Value value="100" time="1.32" />
                        <Value value="100" time="1.36" />
                        <Value value="100" time="1.4" />
                        <Value value="100" time="1.44" />
                        <Value value="100" time="1.48" />
                        <Value value="100" time="1.52" />
                        <Value value="100" time="1.56" />
                        <Value value="100" time="1.6" />
                        <Value value="100" time="1.64" />
                        <Value value="100" time="1.68" />
                        <Value value="100" time="1.72" />
                        <Value value="100" time="1.76" />
                        <Value value="100" time="1.8" />
                        <Value value="100" time="1.84" />
                        <Value value="100" time="1.88" />
                        <Value value="100" time="1.92" />
                        <Value value="100" time="1.96" />
                        <Value value="100" time="2" />
                        <Value value="100" time="2.04" />
                        <Value value="100" time="2.08" />
                        <Value value="100" time="2.12" />
                        <Value value="100" time="2.16" />
                        <Value value="100" time="2.2" />
                        <Value value="100" time="2.24" />
                        <Value value="100" time="2.28" />
                        <Value value="100" time="2.32" />
                        <Value value="100" time="2.36" />
                        <Value value="100" time="2.4" />
                        <Value value="100" time="2.44" />
                        <Value value="100" time="2.48" />
                        <Value value="100" time="2.52" />
                        <Value value="100" time="2.56" />
                        <Value value="100" time="2.6" />
                        <Value value="100" time="2.64" />
                        <Value value="100" time="2.68" />
                        <Value value="100" time="2.72" />
                        <Value value="100" time="2.76" />
                        <Value value="100" time="2.8" />
                        <Value value="100" time="2.84" />
                        <Value value="100" time="2.88" />
                        <Value value="100" time="2.92" />
                        <Value value="100" time="2.96" />
                        <Value value="100" time="3" />
                        <Value value="100" time="3.04" />
                        <Value value="100" time="3.08" />
                        <Value value="100" time="3.12" />
                        <Value value="100" time="3.16" />
                        <Value value="100" time="3.2" />
                        <Value value="100" time="3.24" />
                        <Value value="100" time="3.28" />
                        <Value value="100" time="3.32" />
                        <Value value="100" time="3.36" />
                        <Value value="100" time="3.4" />
                        <Value value="100" time="3.44" />
                        <Value value="100" time="3.48" />
                        <Value value="100" time="3.52" />
                        <Value value="100" time="3.56" />
                        <Value value="100" time="3.6" />
                        <Value value="100" time="3.64" />
                        <Value value="100" time="3.68" />
                        <Value value="100" time="3.72" />
                        <Value value="100" time="3.76" />
                        <Value value="100" time="3.8" />
                        <Value value="100" time="3.84" />
                        <Value value="100" time="3.88" />
                        <Value value="100" time="3.92" />
                        <Value value="100" time="3.96" />
                        <Value value="75" time="4" />
                        <Value value="50" time="4.04" />
                        <Value value="25" time="4.08" />
                        <Value value="0" time="4.12" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 11" type="AVLayer">
                  <Attributes enabled="true" index="9" inPoint="0.32" isNameSet="true" locked="false" name="Layer 11" nullLayer="false" outPoint="1.08" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="6" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="253" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="151" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="126.5,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="2">
                        <Value value="258.101867675781,905.315582275391,0" time="0.32" />
                        <Value value="345.059127140814,905.315582275391,0" time="0.36" />
                        <Value value="441.69434647849,905.315582275391,0" time="0.4" />
                        <Value value="540.209577038045,905.315582275391,0" time="0.44" />
                        <Value value="637.802030628299,905.31558227539,0" time="0.48" />
                        <Value value="732.872051501674,905.315582275391,0" time="0.52" />
                        <Value value="824.194615823141,905.315582275391,0" time="0.56" />
                        <Value value="910.637266108722,905.315582275391,0" time="0.6" />
                        <Value value="990.97147899829,905.315582275391,0" time="0.64" />
                        <Value value="1063.74822968168,905.315582275391,0" time="0.68" />
                        <Value value="1127.10910836424,905.315582275391,0" time="0.72" />
                        <Value value="1178.51050464454,905.315582275391,0" time="0.76" />
                        <Value value="1214.19132877677,905.315582275391,0" time="0.8" />
                        <Value value="1228.10186767578,905.315582275391,0" time="0.84" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="3">
                        <Value value="0" time="0.32" />
                        <Value value="28.0330085890392" time="0.36" />
                        <Value value="56.250000000125" time="0.4" />
                        <Value value="82.3223304704794" time="0.44" />
                        <Value value="100" time="0.48" />
                        <Value value="92.6088396130214" time="0.52" />
                        <Value value="79.7809208756011" time="0.56" />
                        <Value value="65.448391989846" time="0.6" />
                        <Value value="50.8365748991889" time="0.64" />
                        <Value value="36.6595514736564" time="0.68" />
                        <Value value="23.5328637311766" time="0.72" />
                        <Value value="12.1751548711712" time="0.76" />
                        <Value value="3.65665785095847" time="0.8" />
                        <Value value="0" time="0.84" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 10" type="AVLayer">
                  <Attributes enabled="true" index="10" inPoint="0.36" isNameSet="true" locked="false" name="Layer 10" nullLayer="false" outPoint="1.08" parentindex="-1" samplingQuality="BILINEAR" startTime="0.04" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="6" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="253" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="151" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="126.5,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="2">
                        <Value value="258.101867675781,905.315582275391,0" time="0.36" />
                        <Value value="345.059127140814,905.315582275391,0" time="0.4" />
                        <Value value="441.69434647849,905.315582275391,0" time="0.44" />
                        <Value value="540.209577038045,905.315582275391,0" time="0.48" />
                        <Value value="637.802030628299,905.31558227539,0" time="0.52" />
                        <Value value="732.872051501674,905.315582275391,0" time="0.56" />
                        <Value value="824.194615823141,905.315582275391,0" time="0.6" />
                        <Value value="910.637266108722,905.315582275391,0" time="0.64" />
                        <Value value="990.97147899829,905.315582275391,0" time="0.68" />
                        <Value value="1063.74822968168,905.315582275391,0" time="0.72" />
                        <Value value="1127.10910836424,905.315582275391,0" time="0.76" />
                        <Value value="1178.51050464454,905.315582275391,0" time="0.8" />
                        <Value value="1214.19132877677,905.315582275391,0" time="0.84" />
                        <Value value="1228.10186767578,905.315582275391,0" time="0.88" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="3">
                        <Value value="0" time="0.36" />
                        <Value value="28.0330085890392" time="0.4" />
                        <Value value="56.250000000125" time="0.44" />
                        <Value value="82.3223304704794" time="0.48" />
                        <Value value="100" time="0.52" />
                        <Value value="92.6088396130214" time="0.56" />
                        <Value value="79.7809208756011" time="0.6" />
                        <Value value="65.448391989846" time="0.64" />
                        <Value value="50.8365748991889" time="0.68" />
                        <Value value="36.6595514736564" time="0.72" />
                        <Value value="23.5328637311766" time="0.76" />
                        <Value value="12.1751548711712" time="0.8" />
                        <Value value="3.65665785095847" time="0.84" />
                        <Value value="0" time="0.88" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 9" type="AVLayer">
                  <Attributes enabled="true" index="11" inPoint="0.32" isNameSet="true" locked="false" name="Layer 9" nullLayer="false" outPoint="1.08" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="6" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="253" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="151" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="126.5,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="2">
                        <Value value="258.101867675781,905.315582275391,0" time="0.32" />
                        <Value value="345.059127140814,905.315582275391,0" time="0.36" />
                        <Value value="441.69434647849,905.315582275391,0" time="0.4" />
                        <Value value="540.209577038045,905.315582275391,0" time="0.44" />
                        <Value value="637.802030628299,905.31558227539,0" time="0.48" />
                        <Value value="732.872051501674,905.315582275391,0" time="0.52" />
                        <Value value="824.194615823141,905.315582275391,0" time="0.56" />
                        <Value value="910.637266108722,905.315582275391,0" time="0.6" />
                        <Value value="990.97147899829,905.315582275391,0" time="0.64" />
                        <Value value="1063.74822968168,905.315582275391,0" time="0.68" />
                        <Value value="1127.10910836424,905.315582275391,0" time="0.72" />
                        <Value value="1178.51050464454,905.315582275391,0" time="0.76" />
                        <Value value="1214.19132877677,905.315582275391,0" time="0.8" />
                        <Value value="1228.10186767578,905.315582275391,0" time="0.84" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="3">
                        <Value value="0" time="0.32" />
                        <Value value="28.0330085890392" time="0.36" />
                        <Value value="56.250000000125" time="0.4" />
                        <Value value="82.3223304704794" time="0.44" />
                        <Value value="100" time="0.48" />
                        <Value value="92.6088396130214" time="0.52" />
                        <Value value="79.7809208756011" time="0.56" />
                        <Value value="65.448391989846" time="0.6" />
                        <Value value="50.8365748991889" time="0.64" />
                        <Value value="36.6595514736564" time="0.68" />
                        <Value value="23.5328637311766" time="0.72" />
                        <Value value="12.1751548711712" time="0.76" />
                        <Value value="3.65665785095847" time="0.8" />
                        <Value value="0" time="0.84" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 8" type="AVLayer">
                  <Attributes enabled="true" index="12" inPoint="0.36" isNameSet="true" locked="false" name="Layer 8" nullLayer="false" outPoint="1.08" parentindex="-1" samplingQuality="BILINEAR" startTime="0.04" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="6" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="253" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="151" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="126.5,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="2">
                        <Value value="258.101867675781,905.315582275391,0" time="0.36" />
                        <Value value="345.059127140814,905.315582275391,0" time="0.4" />
                        <Value value="441.69434647849,905.315582275391,0" time="0.44" />
                        <Value value="540.209577038045,905.315582275391,0" time="0.48" />
                        <Value value="637.802030628299,905.31558227539,0" time="0.52" />
                        <Value value="732.872051501674,905.315582275391,0" time="0.56" />
                        <Value value="824.194615823141,905.315582275391,0" time="0.6" />
                        <Value value="910.637266108722,905.315582275391,0" time="0.64" />
                        <Value value="990.97147899829,905.315582275391,0" time="0.68" />
                        <Value value="1063.74822968168,905.315582275391,0" time="0.72" />
                        <Value value="1127.10910836424,905.315582275391,0" time="0.76" />
                        <Value value="1178.51050464454,905.315582275391,0" time="0.8" />
                        <Value value="1214.19132877677,905.315582275391,0" time="0.84" />
                        <Value value="1228.10186767578,905.315582275391,0" time="0.88" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="3">
                        <Value value="0" time="0.36" />
                        <Value value="28.0330085890392" time="0.4" />
                        <Value value="56.250000000125" time="0.44" />
                        <Value value="82.3223304704794" time="0.48" />
                        <Value value="100" time="0.52" />
                        <Value value="92.6088396130214" time="0.56" />
                        <Value value="79.7809208756011" time="0.6" />
                        <Value value="65.448391989846" time="0.64" />
                        <Value value="50.8365748991889" time="0.68" />
                        <Value value="36.6595514736564" time="0.72" />
                        <Value value="23.5328637311766" time="0.76" />
                        <Value value="12.1751548711712" time="0.8" />
                        <Value value="3.65665785095847" time="0.84" />
                        <Value value="0" time="0.88" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="CARROT ENGINE FLYING HIGH" type="TextLayer" StepCount="4">
                  <Attributes enabled="true" index="13" inPoint="0.52" isNameSet="true" locked="false" name="CARROT ENGINE FLYING HIGH" nullLayer="false" outPoint="6.52" parentindex="-1" samplingQuality="BILINEAR" startTime="0.52" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="true" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="1920" height="1080" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="-1" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Properties name="Text" matchName="ADBE Text Properties" active="true" isEffect="false" isMask="false" enabled="true" numProperties="4">
                      <Property name="Source Text" matchName="ADBE Text Document" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="TEXT_DOCUMENT" expressionEnabled="false" numKeys="0">
                        <TextValue allCaps="false" applyFill="true" applyStroke="false" baselineShift="0" boxText="false" fauxBold="false" fauxItalic="false" font="MullerMedium" fontFamily="Muller" fontLocation="C:\WINDOWS\Fonts\Fontfabric - MullerMedium.otf" fontSize="36" fontStyle="Medium" horizontalScale="1" leading="35" pointText="true" smallCaps="false" subscript="false" superscript="false" tracking="20" tsume="0" verticalScale="1" justification="LEFT_JUSTIFY" verticalAlignment="TOP" text="CARROT ENGINE FLYING HIGH" baselineLocs="0, 0, 544.783996582031, 0" fillColor="0.21569000184536, 0.22744999825954, 0.29020002484322" />
                      </Property>
                      <Properties name="Path Options" matchName="ADBE Text Path Options" active="true" isEffect="false" isMask="false" enabled="true" numProperties="6">
                        <Property name="Path" matchName="ADBE Text Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="MASK_INDEX" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Reverse Path" matchName="ADBE Text Reverse Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Perpendicular To Path" matchName="ADBE Text Perpendicular To Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                        <Property name="Force Alignment" matchName="ADBE Text Force Align Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="First Margin" matchName="ADBE Text First Margin" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Last Margin" matchName="ADBE Text Last Margin" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      </Properties>
                      <Properties name="More Options" matchName="ADBE Text More Options" active="true" isEffect="false" isMask="false" enabled="true" numProperties="4">
                        <Property name="Anchor Point Grouping" matchName="ADBE Text Anchor Point Option" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        <Property name="Grouping Alignment" matchName="ADBE Text Anchor Point Align" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="TwoD" expressionEnabled="false" numKeys="0" value="0,0" />
                        <Property name="Fill &amp; Stroke" matchName="ADBE Text Render Order" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        <Property name="Inter-Character Blending" matchName="ADBE Text Character Blend Mode" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                      </Properties>
                      <Properties name="Animators" matchName="ADBE Text Animators" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    </Properties>
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="432.5,971,0" time="0.52" />
                        <Value value="420.095641792128,971,0" time="0.56" />
                        <Value value="406.692498672383,971,0" time="0.6" />
                        <Value value="393.892882727314,971,0" time="0.64" />
                        <Value value="382.296028582989,971,0" time="0.68" />
                        <Value value="372.528342045877,971,0" time="0.72" />
                        <Value value="365.432306969265,971,0" time="0.76" />
                        <Value value="362.5,971,0" time="0.8" />
                        <Value value="362.5,971,0" time="0.84" />
                        <Value value="362.5,971,0" time="0.88" />
                        <Value value="362.5,971,0" time="0.92" />
                        <Value value="362.5,971,0" time="0.96" />
                        <Value value="362.5,971,0" time="1" />
                        <Value value="362.5,971,0" time="1.04" />
                        <Value value="362.5,971,0" time="1.08" />
                        <Value value="362.5,971,0" time="1.12" />
                        <Value value="362.5,971,0" time="1.16" />
                        <Value value="362.5,971,0" time="1.2" />
                        <Value value="362.5,971,0" time="1.24" />
                        <Value value="362.5,971,0" time="1.28" />
                        <Value value="362.5,971,0" time="1.32" />
                        <Value value="362.5,971,0" time="1.36" />
                        <Value value="362.5,971,0" time="1.4" />
                        <Value value="362.5,971,0" time="1.44" />
                        <Value value="362.5,971,0" time="1.48" />
                        <Value value="362.5,971,0" time="1.52" />
                        <Value value="362.5,971,0" time="1.56" />
                        <Value value="362.5,971,0" time="1.6" />
                        <Value value="362.5,971,0" time="1.64" />
                        <Value value="362.5,971,0" time="1.68" />
                        <Value value="362.5,971,0" time="1.72" />
                        <Value value="362.5,971,0" time="1.76" />
                        <Value value="362.5,971,0" time="1.8" />
                        <Value value="362.5,971,0" time="1.84" />
                        <Value value="362.5,971,0" time="1.88" />
                        <Value value="362.5,971,0" time="1.92" />
                        <Value value="362.5,971,0" time="1.96" />
                        <Value value="362.5,971,0" time="2" />
                        <Value value="362.5,971,0" time="2.04" />
                        <Value value="362.5,971,0" time="2.08" />
                        <Value value="362.5,971,0" time="2.12" />
                        <Value value="362.5,971,0" time="2.16" />
                        <Value value="362.5,971,0" time="2.2" />
                        <Value value="362.5,971,0" time="2.24" />
                        <Value value="362.5,971,0" time="2.28" />
                        <Value value="362.5,971,0" time="2.32" />
                        <Value value="362.5,971,0" time="2.36" />
                        <Value value="362.5,971,0" time="2.4" />
                        <Value value="362.5,971,0" time="2.44" />
                        <Value value="362.5,971,0" time="2.48" />
                        <Value value="362.5,971,0" time="2.52" />
                        <Value value="362.5,971,0" time="2.56" />
                        <Value value="362.5,971,0" time="2.6" />
                        <Value value="362.5,971,0" time="2.64" />
                        <Value value="362.5,971,0" time="2.68" />
                        <Value value="362.5,971,0" time="2.72" />
                        <Value value="362.5,971,0" time="2.76" />
                        <Value value="362.5,971,0" time="2.8" />
                        <Value value="362.5,971,0" time="2.84" />
                        <Value value="362.5,971,0" time="2.88" />
                        <Value value="362.5,971,0" time="2.92" />
                        <Value value="362.5,971,0" time="2.96" />
                        <Value value="362.5,971,0" time="3" />
                        <Value value="362.5,971,0" time="3.04" />
                        <Value value="362.5,971,0" time="3.08" />
                        <Value value="362.5,971,0" time="3.12" />
                        <Value value="362.5,971,0" time="3.16" />
                        <Value value="362.5,971,0" time="3.2" />
                        <Value value="362.5,971,0" time="3.24" />
                        <Value value="362.5,971,0" time="3.28" />
                        <Value value="362.5,971,0" time="3.32" />
                        <Value value="362.5,971,0" time="3.36" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="0.52" />
                        <Value value="17.7153371116494" time="0.56" />
                        <Value value="36.8478112834817" time="0.6" />
                        <Value value="55.155262745459" time="0.64" />
                        <Value value="71.7207264931884" time="0.68" />
                        <Value value="85.675994068619" time="0.72" />
                        <Value value="95.8141535641431" time="0.76" />
                        <Value value="100" time="0.8" />
                        <Value value="100" time="0.84" />
                        <Value value="100" time="0.88" />
                        <Value value="100" time="0.92" />
                        <Value value="100" time="0.96" />
                        <Value value="100" time="1" />
                        <Value value="100" time="1.04" />
                        <Value value="100" time="1.08" />
                        <Value value="100" time="1.12" />
                        <Value value="100" time="1.16" />
                        <Value value="100" time="1.2" />
                        <Value value="100" time="1.24" />
                        <Value value="100" time="1.28" />
                        <Value value="100" time="1.32" />
                        <Value value="100" time="1.36" />
                        <Value value="100" time="1.4" />
                        <Value value="100" time="1.44" />
                        <Value value="100" time="1.48" />
                        <Value value="100" time="1.52" />
                        <Value value="100" time="1.56" />
                        <Value value="100" time="1.6" />
                        <Value value="100" time="1.64" />
                        <Value value="100" time="1.68" />
                        <Value value="100" time="1.72" />
                        <Value value="100" time="1.76" />
                        <Value value="100" time="1.8" />
                        <Value value="100" time="1.84" />
                        <Value value="100" time="1.88" />
                        <Value value="100" time="1.92" />
                        <Value value="100" time="1.96" />
                        <Value value="100" time="2" />
                        <Value value="100" time="2.04" />
                        <Value value="100" time="2.08" />
                        <Value value="100" time="2.12" />
                        <Value value="100" time="2.16" />
                        <Value value="100" time="2.2" />
                        <Value value="100" time="2.24" />
                        <Value value="100" time="2.28" />
                        <Value value="100" time="2.32" />
                        <Value value="100" time="2.36" />
                        <Value value="100" time="2.4" />
                        <Value value="100" time="2.44" />
                        <Value value="100" time="2.48" />
                        <Value value="100" time="2.52" />
                        <Value value="100" time="2.56" />
                        <Value value="100" time="2.6" />
                        <Value value="100" time="2.64" />
                        <Value value="100" time="2.68" />
                        <Value value="100" time="2.72" />
                        <Value value="100" time="2.76" />
                        <Value value="100" time="2.8" />
                        <Value value="100" time="2.84" />
                        <Value value="100" time="2.88" />
                        <Value value="100" time="2.92" />
                        <Value value="100" time="2.96" />
                        <Value value="100" time="3" />
                        <Value value="100" time="3.04" />
                        <Value value="100" time="3.08" />
                        <Value value="100" time="3.12" />
                        <Value value="100" time="3.16" />
                        <Value value="100" time="3.2" />
                        <Value value="100" time="3.24" />
                        <Value value="100" time="3.28" />
                        <Value value="100" time="3.32" />
                        <Value value="100" time="3.36" />
                        <Value value="100" time="3.4" />
                        <Value value="100" time="3.44" />
                        <Value value="100" time="3.48" />
                        <Value value="100" time="3.52" />
                        <Value value="100" time="3.56" />
                        <Value value="80" time="3.6" />
                        <Value value="60" time="3.64" />
                        <Value value="40" time="3.68" />
                        <Value value="20" time="3.72" />
                        <Value value="0" time="3.76" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="BREAKING NEWS" type="TextLayer" StepCount="4">
                  <Attributes enabled="true" index="14" inPoint="0.36" isNameSet="true" locked="false" name="BREAKING NEWS" nullLayer="false" outPoint="6.36" parentindex="-1" samplingQuality="BILINEAR" startTime="0.36" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="true" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="1920" height="1080" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="-1" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Properties name="Text" matchName="ADBE Text Properties" active="true" isEffect="false" isMask="false" enabled="true" numProperties="4">
                      <Property name="Source Text" matchName="ADBE Text Document" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="TEXT_DOCUMENT" expressionEnabled="false" numKeys="0">
                        <TextValue allCaps="false" applyFill="true" applyStroke="false" baselineShift="0" boxText="false" fauxBold="false" fauxItalic="false" font="MullerBold" fontFamily="Muller" fontLocation="C:\WINDOWS\Fonts\Fontfabric - MullerBold.otf" fontSize="66" fontStyle="Bold" horizontalScale="1" leading="35" pointText="true" smallCaps="false" subscript="false" superscript="false" tracking="20" tsume="0" verticalScale="1" justification="LEFT_JUSTIFY" verticalAlignment="TOP" text="BREAKING NEWS" baselineLocs="0, 0, 575.448913574219, 0" fillColor="0.98430997133255, 0.70981001853943, 0.0862699970603" />
                      </Property>
                      <Properties name="Path Options" matchName="ADBE Text Path Options" active="true" isEffect="false" isMask="false" enabled="true" numProperties="6">
                        <Property name="Path" matchName="ADBE Text Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="MASK_INDEX" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Reverse Path" matchName="ADBE Text Reverse Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Perpendicular To Path" matchName="ADBE Text Perpendicular To Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                        <Property name="Force Alignment" matchName="ADBE Text Force Align Path" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="First Margin" matchName="ADBE Text First Margin" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                        <Property name="Last Margin" matchName="ADBE Text Last Margin" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      </Properties>
                      <Properties name="More Options" matchName="ADBE Text More Options" active="true" isEffect="false" isMask="false" enabled="true" numProperties="4">
                        <Property name="Anchor Point Grouping" matchName="ADBE Text Anchor Point Option" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        <Property name="Grouping Alignment" matchName="ADBE Text Anchor Point Align" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="TwoD" expressionEnabled="false" numKeys="0" value="0,0" />
                        <Property name="Fill &amp; Stroke" matchName="ADBE Text Render Order" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                        <Property name="Inter-Character Blending" matchName="ADBE Text Character Blend Mode" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="1" maxValue="1" value="1" />
                      </Properties>
                      <Properties name="Animators" matchName="ADBE Text Animators" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    </Properties>
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="432.5,904,0" time="0.36" />
                        <Value value="420.095641792128,904,0" time="0.4" />
                        <Value value="406.692498672383,904,0" time="0.44" />
                        <Value value="393.892882727314,904,0" time="0.48" />
                        <Value value="382.296028582989,904,0" time="0.52" />
                        <Value value="372.528342045877,904,0" time="0.56" />
                        <Value value="365.432306969265,904,0" time="0.6" />
                        <Value value="362.5,904,0" time="0.64" />
                        <Value value="362.5,904,0" time="0.68" />
                        <Value value="362.5,904,0" time="0.72" />
                        <Value value="362.5,904,0" time="0.76" />
                        <Value value="362.5,904,0" time="0.8" />
                        <Value value="362.5,904,0" time="0.84" />
                        <Value value="362.5,904,0" time="0.88" />
                        <Value value="362.5,904,0" time="0.92" />
                        <Value value="362.5,904,0" time="0.96" />
                        <Value value="362.5,904,0" time="1" />
                        <Value value="362.5,904,0" time="1.04" />
                        <Value value="362.5,904,0" time="1.08" />
                        <Value value="362.5,904,0" time="1.12" />
                        <Value value="362.5,904,0" time="1.16" />
                        <Value value="362.5,904,0" time="1.2" />
                        <Value value="362.5,904,0" time="1.24" />
                        <Value value="362.5,904,0" time="1.28" />
                        <Value value="362.5,904,0" time="1.32" />
                        <Value value="362.5,904,0" time="1.36" />
                        <Value value="362.5,904,0" time="1.4" />
                        <Value value="362.5,904,0" time="1.44" />
                        <Value value="362.5,904,0" time="1.48" />
                        <Value value="362.5,904,0" time="1.52" />
                        <Value value="362.5,904,0" time="1.56" />
                        <Value value="362.5,904,0" time="1.6" />
                        <Value value="362.5,904,0" time="1.64" />
                        <Value value="362.5,904,0" time="1.68" />
                        <Value value="362.5,904,0" time="1.72" />
                        <Value value="362.5,904,0" time="1.76" />
                        <Value value="362.5,904,0" time="1.8" />
                        <Value value="362.5,904,0" time="1.84" />
                        <Value value="362.5,904,0" time="1.88" />
                        <Value value="362.5,904,0" time="1.92" />
                        <Value value="362.5,904,0" time="1.96" />
                        <Value value="362.5,904,0" time="2" />
                        <Value value="362.5,904,0" time="2.04" />
                        <Value value="362.5,904,0" time="2.08" />
                        <Value value="362.5,904,0" time="2.12" />
                        <Value value="362.5,904,0" time="2.16" />
                        <Value value="362.5,904,0" time="2.2" />
                        <Value value="362.5,904,0" time="2.24" />
                        <Value value="362.5,904,0" time="2.28" />
                        <Value value="362.5,904,0" time="2.32" />
                        <Value value="362.5,904,0" time="2.36" />
                        <Value value="362.5,904,0" time="2.4" />
                        <Value value="362.5,904,0" time="2.44" />
                        <Value value="362.5,904,0" time="2.48" />
                        <Value value="362.5,904,0" time="2.52" />
                        <Value value="362.5,904,0" time="2.56" />
                        <Value value="362.5,904,0" time="2.6" />
                        <Value value="362.5,904,0" time="2.64" />
                        <Value value="362.5,904,0" time="2.68" />
                        <Value value="362.5,904,0" time="2.72" />
                        <Value value="362.5,904,0" time="2.76" />
                        <Value value="362.5,904,0" time="2.8" />
                        <Value value="362.5,904,0" time="2.84" />
                        <Value value="362.5,904,0" time="2.88" />
                        <Value value="362.5,904,0" time="2.92" />
                        <Value value="362.5,904,0" time="2.96" />
                        <Value value="362.5,904,0" time="3" />
                        <Value value="362.5,904,0" time="3.04" />
                        <Value value="362.5,904,0" time="3.08" />
                        <Value value="362.5,904,0" time="3.12" />
                        <Value value="362.5,904,0" time="3.16" />
                        <Value value="362.5,904,0" time="3.2" />
                        <Value value="362.5,904,0" time="3.24" />
                        <Value value="362.5,904,0" time="3.28" />
                        <Value value="362.5,904,0" time="3.32" />
                        <Value value="362.5,904,0" time="3.36" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="0.36" />
                        <Value value="14.2857142857143" time="0.4" />
                        <Value value="28.5714285714286" time="0.44" />
                        <Value value="42.8571428571429" time="0.48" />
                        <Value value="57.1428571428571" time="0.52" />
                        <Value value="71.4285714285714" time="0.56" />
                        <Value value="85.7142857142857" time="0.6" />
                        <Value value="100" time="0.64" />
                        <Value value="100" time="0.68" />
                        <Value value="100" time="0.72" />
                        <Value value="100" time="0.76" />
                        <Value value="100" time="0.8" />
                        <Value value="100" time="0.84" />
                        <Value value="100" time="0.88" />
                        <Value value="100" time="0.92" />
                        <Value value="100" time="0.96" />
                        <Value value="100" time="1" />
                        <Value value="100" time="1.04" />
                        <Value value="100" time="1.08" />
                        <Value value="100" time="1.12" />
                        <Value value="100" time="1.16" />
                        <Value value="100" time="1.2" />
                        <Value value="100" time="1.24" />
                        <Value value="100" time="1.28" />
                        <Value value="100" time="1.32" />
                        <Value value="100" time="1.36" />
                        <Value value="100" time="1.4" />
                        <Value value="100" time="1.44" />
                        <Value value="100" time="1.48" />
                        <Value value="100" time="1.52" />
                        <Value value="100" time="1.56" />
                        <Value value="100" time="1.6" />
                        <Value value="100" time="1.64" />
                        <Value value="100" time="1.68" />
                        <Value value="100" time="1.72" />
                        <Value value="100" time="1.76" />
                        <Value value="100" time="1.8" />
                        <Value value="100" time="1.84" />
                        <Value value="100" time="1.88" />
                        <Value value="100" time="1.92" />
                        <Value value="100" time="1.96" />
                        <Value value="100" time="2" />
                        <Value value="100" time="2.04" />
                        <Value value="100" time="2.08" />
                        <Value value="100" time="2.12" />
                        <Value value="100" time="2.16" />
                        <Value value="100" time="2.2" />
                        <Value value="100" time="2.24" />
                        <Value value="100" time="2.28" />
                        <Value value="100" time="2.32" />
                        <Value value="100" time="2.36" />
                        <Value value="100" time="2.4" />
                        <Value value="100" time="2.44" />
                        <Value value="100" time="2.48" />
                        <Value value="100" time="2.52" />
                        <Value value="100" time="2.56" />
                        <Value value="100" time="2.6" />
                        <Value value="100" time="2.64" />
                        <Value value="100" time="2.68" />
                        <Value value="100" time="2.72" />
                        <Value value="100" time="2.76" />
                        <Value value="100" time="2.8" />
                        <Value value="100" time="2.84" />
                        <Value value="100" time="2.88" />
                        <Value value="100" time="2.92" />
                        <Value value="100" time="2.96" />
                        <Value value="100" time="3" />
                        <Value value="100" time="3.04" />
                        <Value value="100" time="3.08" />
                        <Value value="100" time="3.12" />
                        <Value value="100" time="3.16" />
                        <Value value="100" time="3.2" />
                        <Value value="100" time="3.24" />
                        <Value value="100" time="3.28" />
                        <Value value="100" time="3.32" />
                        <Value value="100" time="3.36" />
                        <Value value="80" time="3.4" />
                        <Value value="60" time="3.44" />
                        <Value value="40" time="3.48" />
                        <Value value="20" time="3.52" />
                        <Value value="0" time="3.56" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 6" type="AVLayer">
                  <Attributes enabled="true" index="15" inPoint="0.68" isNameSet="true" locked="false" name="Layer 6" nullLayer="false" outPoint="6.36" parentindex="-1" samplingQuality="BILINEAR" startTime="0.36" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="236" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="152" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="118,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="1432,907,0" time="0.68" />
                        <Value value="1415.79858150894,907,0" time="0.72" />
                        <Value value="1397.9477303862,907,0" time="0.76" />
                        <Value value="1380.28750682537,907,0" time="0.8" />
                        <Value value="1363.48417695517,907,0" time="0.84" />
                        <Value value="1348.01109095387,907,0" time="0.88" />
                        <Value value="1334.35446003329,907,0" time="0.92" />
                        <Value value="1323.10612743748,907,0" time="0.96" />
                        <Value value="1315.16144263841,907,0" time="1" />
                        <Value value="1312,907,0" time="1.04" />
                        <Value value="1312,907,0" time="1.08" />
                        <Value value="1312,907,0" time="1.12" />
                        <Value value="1312,907,0" time="1.16" />
                        <Value value="1312,907,0" time="1.2" />
                        <Value value="1312,907,0" time="1.24" />
                        <Value value="1312,907,0" time="1.28" />
                        <Value value="1312,907,0" time="1.32" />
                        <Value value="1312,907,0" time="1.36" />
                        <Value value="1312,907,0" time="1.4" />
                        <Value value="1312,907,0" time="1.44" />
                        <Value value="1312,907,0" time="1.48" />
                        <Value value="1312,907,0" time="1.52" />
                        <Value value="1312,907,0" time="1.56" />
                        <Value value="1312,907,0" time="1.6" />
                        <Value value="1312,907,0" time="1.64" />
                        <Value value="1312,907,0" time="1.68" />
                        <Value value="1312,907,0" time="1.72" />
                        <Value value="1312,907,0" time="1.76" />
                        <Value value="1312,907,0" time="1.8" />
                        <Value value="1312,907,0" time="1.84" />
                        <Value value="1312,907,0" time="1.88" />
                        <Value value="1312,907,0" time="1.92" />
                        <Value value="1312,907,0" time="1.96" />
                        <Value value="1312,907,0" time="2" />
                        <Value value="1312,907,0" time="2.04" />
                        <Value value="1312,907,0" time="2.08" />
                        <Value value="1312,907,0" time="2.12" />
                        <Value value="1312,907,0" time="2.16" />
                        <Value value="1312,907,0" time="2.2" />
                        <Value value="1312,907,0" time="2.24" />
                        <Value value="1312,907,0" time="2.28" />
                        <Value value="1312,907,0" time="2.32" />
                        <Value value="1312,907,0" time="2.36" />
                        <Value value="1312,907,0" time="2.4" />
                        <Value value="1312,907,0" time="2.44" />
                        <Value value="1312,907,0" time="2.48" />
                        <Value value="1312,907,0" time="2.52" />
                        <Value value="1312,907,0" time="2.56" />
                        <Value value="1312,907,0" time="2.6" />
                        <Value value="1312,907,0" time="2.64" />
                        <Value value="1312,907,0" time="2.68" />
                        <Value value="1312,907,0" time="2.72" />
                        <Value value="1312,907,0" time="2.76" />
                        <Value value="1312,907,0" time="2.8" />
                        <Value value="1312,907,0" time="2.84" />
                        <Value value="1312,907,0" time="2.88" />
                        <Value value="1312,907,0" time="2.92" />
                        <Value value="1312,907,0" time="2.96" />
                        <Value value="1312,907,0" time="3" />
                        <Value value="1312,907,0" time="3.04" />
                        <Value value="1312,907,0" time="3.08" />
                        <Value value="1312,907,0" time="3.12" />
                        <Value value="1312,907,0" time="3.16" />
                        <Value value="1312,907,0" time="3.2" />
                        <Value value="1312,907,0" time="3.24" />
                        <Value value="1312,907,0" time="3.28" />
                        <Value value="1312,907,0" time="3.32" />
                        <Value value="1312,907,0" time="3.36" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="0.68" />
                        <Value value="13.4992469767442" time="0.72" />
                        <Value value="28.3757687409893" time="0.76" />
                        <Value value="43.0935197865549" time="0.8" />
                        <Value value="57.0966332860423" time="0.84" />
                        <Value value="69.9915733505781" time="0.88" />
                        <Value value="81.377409547972" time="0.92" />
                        <Value value="90.7481252667398" time="0.96" />
                        <Value value="97.365750142712" time="1" />
                        <Value value="100" time="1.04" />
                        <Value value="100" time="1.08" />
                        <Value value="100" time="1.12" />
                        <Value value="100" time="1.16" />
                        <Value value="100" time="1.2" />
                        <Value value="100" time="1.24" />
                        <Value value="100" time="1.28" />
                        <Value value="100" time="1.32" />
                        <Value value="100" time="1.36" />
                        <Value value="100" time="1.4" />
                        <Value value="100" time="1.44" />
                        <Value value="100" time="1.48" />
                        <Value value="100" time="1.52" />
                        <Value value="100" time="1.56" />
                        <Value value="100" time="1.6" />
                        <Value value="100" time="1.64" />
                        <Value value="100" time="1.68" />
                        <Value value="100" time="1.72" />
                        <Value value="100" time="1.76" />
                        <Value value="100" time="1.8" />
                        <Value value="100" time="1.84" />
                        <Value value="100" time="1.88" />
                        <Value value="100" time="1.92" />
                        <Value value="100" time="1.96" />
                        <Value value="100" time="2" />
                        <Value value="100" time="2.04" />
                        <Value value="100" time="2.08" />
                        <Value value="100" time="2.12" />
                        <Value value="100" time="2.16" />
                        <Value value="100" time="2.2" />
                        <Value value="100" time="2.24" />
                        <Value value="100" time="2.28" />
                        <Value value="100" time="2.32" />
                        <Value value="100" time="2.36" />
                        <Value value="100" time="2.4" />
                        <Value value="100" time="2.44" />
                        <Value value="100" time="2.48" />
                        <Value value="100" time="2.52" />
                        <Value value="100" time="2.56" />
                        <Value value="100" time="2.6" />
                        <Value value="100" time="2.64" />
                        <Value value="100" time="2.68" />
                        <Value value="100" time="2.72" />
                        <Value value="100" time="2.76" />
                        <Value value="100" time="2.8" />
                        <Value value="100" time="2.84" />
                        <Value value="100" time="2.88" />
                        <Value value="100" time="2.92" />
                        <Value value="100" time="2.96" />
                        <Value value="100" time="3" />
                        <Value value="100" time="3.04" />
                        <Value value="100" time="3.08" />
                        <Value value="100" time="3.12" />
                        <Value value="100" time="3.16" />
                        <Value value="100" time="3.2" />
                        <Value value="100" time="3.24" />
                        <Value value="100" time="3.28" />
                        <Value value="100" time="3.32" />
                        <Value value="100" time="3.36" />
                        <Value value="66.6666666666667" time="3.4" />
                        <Value value="33.3333333333333" time="3.44" />
                        <Value value="0" time="3.48" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 5" type="AVLayer">
                  <Attributes enabled="true" index="16" inPoint="0.48" isNameSet="true" locked="false" name="Layer 5" nullLayer="false" outPoint="6.16" parentindex="-1" samplingQuality="BILINEAR" startTime="0.16" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="253" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="151" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="126.5,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="3">
                        <Value value="1349.5,907,0" time="0.48" />
                        <Value value="1333.29953750127,907,0" time="0.52" />
                        <Value value="1315.44867000959,907,0" time="0.56" />
                        <Value value="1297.78763004,907,0" time="0.6" />
                        <Value value="1280.98429668705,907,0" time="0.64" />
                        <Value value="1265.51207583374,907,0" time="0.68" />
                        <Value value="1251.85305959966,907,0" time="0.72" />
                        <Value value="1240.60436912866,907,0" time="0.76" />
                        <Value value="1232.66140788045,907,0" time="0.8" />
                        <Value value="1229.5,907,0" time="0.84" />
                        <Value value="1229.5,907,0" time="0.88" />
                        <Value value="1229.5,907,0" time="0.92" />
                        <Value value="1229.5,907,0" time="0.96" />
                        <Value value="1229.5,907,0" time="1" />
                        <Value value="1229.5,907,0" time="1.04" />
                        <Value value="1229.5,907,0" time="1.08" />
                        <Value value="1229.5,907,0" time="1.12" />
                        <Value value="1229.5,907,0" time="1.16" />
                        <Value value="1229.5,907,0" time="1.2" />
                        <Value value="1229.5,907,0" time="1.24" />
                        <Value value="1229.5,907,0" time="1.28" />
                        <Value value="1229.5,907,0" time="1.32" />
                        <Value value="1229.5,907,0" time="1.36" />
                        <Value value="1229.5,907,0" time="1.4" />
                        <Value value="1229.5,907,0" time="1.44" />
                        <Value value="1229.5,907,0" time="1.48" />
                        <Value value="1229.5,907,0" time="1.52" />
                        <Value value="1229.5,907,0" time="1.56" />
                        <Value value="1229.5,907,0" time="1.6" />
                        <Value value="1229.5,907,0" time="1.64" />
                        <Value value="1229.5,907,0" time="1.68" />
                        <Value value="1229.5,907,0" time="1.72" />
                        <Value value="1229.5,907,0" time="1.76" />
                        <Value value="1229.5,907,0" time="1.8" />
                        <Value value="1229.5,907,0" time="1.84" />
                        <Value value="1229.5,907,0" time="1.88" />
                        <Value value="1229.5,907,0" time="1.92" />
                        <Value value="1229.5,907,0" time="1.96" />
                        <Value value="1229.5,907,0" time="2" />
                        <Value value="1229.5,907,0" time="2.04" />
                        <Value value="1229.5,907,0" time="2.08" />
                        <Value value="1229.5,907,0" time="2.12" />
                        <Value value="1229.5,907,0" time="2.16" />
                        <Value value="1229.5,907,0" time="2.2" />
                        <Value value="1229.5,907,0" time="2.24" />
                        <Value value="1229.5,907,0" time="2.28" />
                        <Value value="1229.5,907,0" time="2.32" />
                        <Value value="1229.5,907,0" time="2.36" />
                        <Value value="1229.5,907,0" time="2.4" />
                        <Value value="1229.5,907,0" time="2.44" />
                        <Value value="1229.5,907,0" time="2.48" />
                        <Value value="1229.5,907,0" time="2.52" />
                        <Value value="1229.5,907,0" time="2.56" />
                        <Value value="1229.5,907,0" time="2.6" />
                        <Value value="1229.5,907,0" time="2.64" />
                        <Value value="1229.5,907,0" time="2.68" />
                        <Value value="1229.5,907,0" time="2.72" />
                        <Value value="1229.5,907,0" time="2.76" />
                        <Value value="1229.5,907,0" time="2.8" />
                        <Value value="1229.5,907,0" time="2.84" />
                        <Value value="1229.5,907,0" time="2.88" />
                        <Value value="1229.5,907,0" time="2.92" />
                        <Value value="1229.5,907,0" time="2.96" />
                        <Value value="1229.5,907,0" time="3" />
                        <Value value="1229.5,907,0" time="3.04" />
                        <Value value="1229.5,907,0" time="3.08" />
                        <Value value="1229.5,907,0" time="3.12" />
                        <Value value="1229.5,907,0" time="3.16" />
                        <Value value="1229.5,907,0" time="3.2" />
                        <Value value="1229.5,907,0" time="3.24" />
                        <Value value="1229.5,907,0" time="3.28" />
                        <Value value="1229.5,907,0" time="3.32" />
                        <Value value="1229.5,907,0" time="3.36" />
                      </Property>
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="4">
                        <Value value="0" time="0.48" />
                        <Value value="13.4992469767442" time="0.52" />
                        <Value value="28.3757687409893" time="0.56" />
                        <Value value="43.0935197865549" time="0.6" />
                        <Value value="57.0966332860423" time="0.64" />
                        <Value value="69.9915733505781" time="0.68" />
                        <Value value="81.377409547972" time="0.72" />
                        <Value value="90.7481252667398" time="0.76" />
                        <Value value="97.365750142712" time="0.8" />
                        <Value value="100" time="0.84" />
                        <Value value="100" time="0.88" />
                        <Value value="100" time="0.92" />
                        <Value value="100" time="0.96" />
                        <Value value="100" time="1" />
                        <Value value="100" time="1.04" />
                        <Value value="100" time="1.08" />
                        <Value value="100" time="1.12" />
                        <Value value="100" time="1.16" />
                        <Value value="100" time="1.2" />
                        <Value value="100" time="1.24" />
                        <Value value="100" time="1.28" />
                        <Value value="100" time="1.32" />
                        <Value value="100" time="1.36" />
                        <Value value="100" time="1.4" />
                        <Value value="100" time="1.44" />
                        <Value value="100" time="1.48" />
                        <Value value="100" time="1.52" />
                        <Value value="100" time="1.56" />
                        <Value value="100" time="1.6" />
                        <Value value="100" time="1.64" />
                        <Value value="100" time="1.68" />
                        <Value value="100" time="1.72" />
                        <Value value="100" time="1.76" />
                        <Value value="100" time="1.8" />
                        <Value value="100" time="1.84" />
                        <Value value="100" time="1.88" />
                        <Value value="100" time="1.92" />
                        <Value value="100" time="1.96" />
                        <Value value="100" time="2" />
                        <Value value="100" time="2.04" />
                        <Value value="100" time="2.08" />
                        <Value value="100" time="2.12" />
                        <Value value="100" time="2.16" />
                        <Value value="100" time="2.2" />
                        <Value value="100" time="2.24" />
                        <Value value="100" time="2.28" />
                        <Value value="100" time="2.32" />
                        <Value value="100" time="2.36" />
                        <Value value="100" time="2.4" />
                        <Value value="100" time="2.44" />
                        <Value value="100" time="2.48" />
                        <Value value="100" time="2.52" />
                        <Value value="100" time="2.56" />
                        <Value value="100" time="2.6" />
                        <Value value="100" time="2.64" />
                        <Value value="100" time="2.68" />
                        <Value value="100" time="2.72" />
                        <Value value="100" time="2.76" />
                        <Value value="100" time="2.8" />
                        <Value value="100" time="2.84" />
                        <Value value="100" time="2.88" />
                        <Value value="100" time="2.92" />
                        <Value value="100" time="2.96" />
                        <Value value="100" time="3" />
                        <Value value="100" time="3.04" />
                        <Value value="100" time="3.08" />
                        <Value value="100" time="3.12" />
                        <Value value="100" time="3.16" />
                        <Value value="100" time="3.2" />
                        <Value value="100" time="3.24" />
                        <Value value="100" time="3.28" />
                        <Value value="100" time="3.32" />
                        <Value value="100" time="3.36" />
                        <Value value="100" time="3.4" />
                        <Value value="100" time="3.44" />
                        <Value value="66.6666666666667" time="3.48" />
                        <Value value="33.3333333333333" time="3.52" />
                        <Value value="0" time="3.56" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
                <Layer name="Layer 3" type="AVLayer">
                  <Attributes enabled="true" index="17" inPoint="0.32" isNameSet="true" locked="false" name="Layer 3" nullLayer="false" outPoint="6" parentindex="-1" samplingQuality="BILINEAR" startTime="0" stretch="100" adjustmentLayer="false" audioActive="false" audioEnabled="true" autoOrient="NO_AUTO_ORIENT" blendingModeInt="0" collapseTransformation="false" effectsActive="true" environmentLayer="false" frameBlending="false" frameBlendingType="NO_FRAME_BLEND" guideLayer="false" hasAudio="false" hasVideo="true" hasTrackMatte="false" width="996" height="156" isNameFromSource="false" isTrackMatte="false" motionBlur="false" preserveTransparency="false" quality="BEST" source="150" threeDLayer="false" threeDPerChar="false" timeRemapEnabled="false" trackMatteType="NO_TRACK_MATTE" />
                  <Properties>
                    <Property name="Time Remap" matchName="ADBE Time Remapping" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                    <Properties name="Masks" matchName="ADBE Mask Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="1">
                      <Properties name="Mask 1" matchName="ADBE Mask Atom" active="true" isEffect="false" isMask="true" enabled="true" numProperties="4" color="0.70980392156863,0.21960784313725,0.21960784313725" inverted="false" locked="false" rotoBezier="false" maskFeatherFalloff="FFO_SMOOTH" maskMode="ADD" maskMotionBlur="SAME_AS_LAYER">
                        <Property name="Mask Path" matchName="ADBE Mask Shape" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="SHAPE" expressionEnabled="false" numKeys="4">
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-199.062957763672,155.767623901367,-19.1445770263672,-0.23237609863281" time="0.32" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-138.315048217773,155.781524658203,41.6033630371094,-0.21847534179688" time="0.36" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0.00001525878906,0,0.00001525878906,156,214.039093017578,155.862182617188,393.957611083984,-0.1378173828125" time="0.4" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,497.077331542969,155.926971435547,676.996032714844,-0.07302856445313" time="0.44" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,636.097778320313,155.958801269531,816.016540527344,-0.04119873046875" time="0.48" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,717.202392578125,155.97737121582,897.121276855469,-0.02262878417969" time="0.52" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,766.854797363281,155.988723754883,946.773681640625,-0.01126098632813" time="0.56" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,796.333190917969,155.995483398438,976.252075195313,-0.0045166015625" time="0.6" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,811.56298828125,155.998962402344,991.481872558594,-0.00103759765625" time="0.64" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.68" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.72" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.76" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.8" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.84" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.88" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.92" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="0.96" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.04" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.08" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.12" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.16" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.2" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.24" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.28" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.32" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.36" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.4" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.44" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.48" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.52" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.56" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.6" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.64" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.68" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.72" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.76" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.8" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.84" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.88" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.92" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="1.96" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.04" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.08" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.12" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.16" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.2" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.24" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.28" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.32" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.36" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.4" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.44" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.48" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.52" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.56" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.6" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.64" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.68" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.72" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.76" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.8" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.84" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.88" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.92" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="2.96" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.04" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.08" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.12" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.16" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.2" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.24" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.28" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.32" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.36" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.4" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.44" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.48" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,816.081176757813,156,996,0" time="3.52" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,813.71435546875,155.999465942383,993.633239746094,-0.00054931640625" time="3.56" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,805.735900878906,155.997634887695,985.65478515625,-0.00236511230469" time="3.6" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,790.316467285156,155.994110107422,970.235229492188,-0.00590515136719" time="3.64" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,764.47021484375,155.988189697266,944.389099121094,-0.01181030273438" time="3.68" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,722.775390625,155.978622436523,902.694213867188,-0.0213623046875" time="3.72" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,653.616638183594,155.962814331055,833.535583496094,-0.03718566894531" time="3.76" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,524.216552734375,155.933181762695,704.13525390625,-0.06680297851563" time="3.8" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,242.858428955078,155.868789672852,422.777099609375,-0.13121032714844" time="3.84" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0.00001525878906,0,0.00001525878906,156,18.2096557617188,155.817367553711,198.128204345703,-0.18264770507813" time="3.88" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0.00001525878906,0,0.00001525878906,156,-81.8620452880859,155.794448852539,98.0564270019531,-0.20555114746094" time="3.92" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-136.410293579102,155.781982421875,43.5082092285156,-0.21803283691406" time="3.96" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-168.461868286133,155.774627685547,11.4566040039063,-0.22537231445313" time="4" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-186.963836669922,155.770385742188,-7.04534912109375,-0.22959899902344" time="4.04" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-196.326843261719,155.768249511719,-16.4083404541016,-0.23175048828125" time="4.08" />
                          <ShapeValue closed="true" inTangents="0,0,0,0,0,0,0,0" outTangents="0,0,0,0,0,0,0,0" vertices="0,0,0,156,-199.06298828125,155.767623901367,-19.1445922851563,-0.23237609863281" time="4.12" />
                        </Property>
                        <Property name="Mask Feather" matchName="ADBE Mask Feather" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="TwoD" expressionEnabled="false" numKeys="0" minValue="0,0" maxValue="0,0" value="0,0" />
                        <Property name="Mask Opacity" matchName="ADBE Mask Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="100" maxValue="100" value="100" />
                        <Property name="Mask Expansion" matchName="ADBE Mask Offset" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="0" minValue="0" maxValue="0" value="0" />
                      </Properties>
                    </Properties>
                    <Properties name="Effects" matchName="ADBE Effect Parade" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                    <Properties name="Transform" matchName="ADBE Transform Group" active="true" isEffect="false" isMask="false" enabled="true" numProperties="12">
                      <Property name="Anchor Point" matchName="ADBE Anchor Point" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="498,78,0" />
                      <Property name="Position" matchName="ADBE Position" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="766,907,0" />
                      <Property name="X Position" matchName="ADBE Position_0" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Position" matchName="ADBE Position_1" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Z Position" matchName="ADBE Position_2" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Scale" matchName="ADBE Scale" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD" expressionEnabled="false" numKeys="0" value="100,100,100" />
                      <Property name="Orientation" matchName="ADBE Orientation" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="ThreeD_SPATIAL" expressionEnabled="false" numKeys="0" value="0,0,0" />
                      <Property name="X Rotation" matchName="ADBE Rotate X" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Y Rotation" matchName="ADBE Rotate Y" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Rotation" matchName="ADBE Rotate Z" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="0" />
                      <Property name="Opacity" matchName="ADBE Opacity" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="true" hasMin="true" propertyValueType="OneD" expressionEnabled="false" numKeys="2">
                        <Value value="100" time="3.72" />
                        <Value value="90.1" time="3.76" />
                        <Value value="80.2" time="3.8" />
                        <Value value="70.3" time="3.84" />
                        <Value value="60.4" time="3.88" />
                        <Value value="50.5" time="3.92" />
                        <Value value="40.6" time="3.96" />
                        <Value value="30.7" time="4" />
                        <Value value="20.8" time="4.04" />
                        <Value value="10.9" time="4.08" />
                        <Value value="1" time="4.12" />
                      </Property>
                      <Property name="Appears in Reflections" matchName="ADBE Envir Appear in Reflect" active="true" isEffect="false" isMask="false" enabled="true" dimensionsSeparated="false" hasMax="false" hasMin="false" propertyValueType="OneD" expressionEnabled="false" numKeys="0" value="1" />
                    </Properties>
                    <Properties name="Sets" matchName="ADBE Layer Sets" active="true" isEffect="false" isMask="false" enabled="true" numProperties="0" />
                  </Properties>
                </Layer>
              </Layers>
            </Composition>
          </Compositions>
          <Footages>
            <Footage name="flare.png" id="148" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="422" width="757" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="File">
              <FileSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="true" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" filename="08ea45ce-4221-44d7-8bf0-fa4d4e9630e9" />
            </Footage>
            <Footage name="logo.png" id="149" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="235" width="235" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="File">
              <FileSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="true" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" filename="1b6987c4-d91b-4adb-9273-78ddd1e6a4fe" />
            </Footage>
            <Footage name="plane1.png" id="150" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="156" width="996" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="File">
              <FileSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="true" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" filename="3a428c2b-59fc-4d98-be81-57739d903999" />
            </Footage>
            <Footage name="plane2.png" id="151" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="156" width="253" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="File">
              <FileSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="true" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" filename="d8266d3f-ed94-4010-8cda-183a69fde1be" />
            </Footage>
            <Footage name="plane3.png" id="152" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="156" width="236" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="File">
              <FileSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="true" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" filename="0f44f45b-e99f-45db-9366-b30212cea0c9" />
            </Footage>
            <Footage name="Blue Solid 1" id="154" comment="" typeName="Footage" pixelAspect="1" duration="0" frameDuration="1" frameRate="0" height="200" width="200" hasAudio="false" hasVideo="true" footageMissing="false" sourceType="Solid">
              <SolidSource alphaMode="STRAIGHT" conformFrameRate="0" displayFrameRate="0" fieldSeparationType="OFF" hasAlpha="false" invertAlpha="false" isStill="true" loop="1" nativeFrameRate="0" premulColor="0,0,0" color="0.1822379976511,0.34046301245689,0.92941200733185" />
            </Footage>
          </Footages>
        </AEProject>
      </XmlData>
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
  <Contents />
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
    - `EventGroupId` - идентификатор группы элемента. Тип `Guid`. Используется для групировки элементов внутри истории.
    - `Variables` - список переменных шаблона.
      - `Variable` - структура конкретной переменной.
        - `Name` - имя переменной. Тип `String`.
        - `Type` - тип переменной. Тип `String`. Используется для отображения соответсвующего типа контроля: "Text", "Media", "Float", "Boolean", "Color", "Point2D", "Point3D", "ComboBox".
        - `Value` - значение переменной. Тип `String`.
        - `ResetMedia` - настройка сброса времени прогрывания при запуске элемента, если тип переменной "Media" и в качестве значения указан видеофайл или секвенция. Тип `Boolean`. Если `true`, то происходит сброс на начало.
        - `Loop` - настройка цикличного воспроизведения, если тип переменной "Media" и в качестве значения указан видеофайл или секвенция. Тип `Boolean`. Если `true`, то цикличное воспроизведение включено.
        - `EditableFieldSettings` - настройки переменной.
          - `ListValues` - список подготовленных значений. Если тип переменной `ComboBox`.
            - `ListValue` - подготовленное значение. Тип `String`. Используется для отображения подготовленных значений в контроле.