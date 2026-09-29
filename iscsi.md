### Подключение iSCSI хранилища (LUN) в VMWare ESXi

#### Как подключить iSCSI LUN с вашей СХД (или сервера) к хосту VMWare ESXi? 
Сначала нужно создать отдельный VMkernel сетевой интерфейс, который будет испоьзоваться ESXi хостом для доступа к iSCSI хранилищу. Перейдите в раздел Networking -> VMkernel NICs -> Add VMkernel NIC.

<img width="914" height="217" alt="image" src="https://github.com/user-attachments/assets/14e649be-9747-4beb-837b-73bba9e00006" />

Кроме vmk порта нужно сразу создать новая группа портов (New port group). Укажите имя для этой группы – iSCSI и назначьте статический IP адрес для вашего интерфейса vmkernel.

<img width="550" height="561" alt="image" src="https://github.com/user-attachments/assets/0b88c24f-5cbc-4cf6-a39e-e6fa4c216c6d" />

Теперь перейдите в настройки вашего стандартного коммутатора vSwitch0 (Networking -> Virtual Switches). Проверьте, что второй физический интерфейс сервера vmnic1 добавлен в конфигурацию и активен (если нет, нажмите кнопку Add uplink и добавьте его)

<img width="384" height="203" alt="image" src="https://github.com/user-attachments/assets/a7bde5cc-4d5f-45cd-b8b5-6ca9c1db6c75" />

Проверьте в секции Nic Teaming что оба физических сетевых интерфейса находятся в статусе Active.

<img width="703" height="668" alt="image" src="https://github.com/user-attachments/assets/e5494320-114c-44b2-829a-a0bd182873fb" />

Теперь в настройки группу портов iSCSI вам нужно разрешить использовать для iSCSI трафика только второй интерфейс. Перейдите в Networking -> Port groups -> iSCSI —> Edit settings. Разверните секцию NIC teaming, выберите Override failover order = Yes. Оставьте активной только vmnic1, порт vmnic0 переведите в состояние Unused.

<img width="1111" height="651" alt="image" src="https://github.com/user-attachments/assets/8bf5b5ea-478c-4b86-bdda-fd82e36f2f53" />

В результате ваш ESXi хост будет использовать для доступа к вашему iSCSI LUN только один интерфейс сервера.

#### Настройка программного iSCSI адаптера в VMWare ESXi
По умолчанию в ESXi отключен программный адаптер iSCSI. Чтобы включить его, перейдите в раздел Storage -> Adapters. Нажмите на кнопку Software iSCSi.

<img width="1091" height="238" alt="image" src="https://github.com/user-attachments/assets/309dc459-1ac8-42ad-b2b9-07dcc0c6e95e" />

Затем в секции Dynamic targets добавьте IP адрес вашего iSCSI хранилища и порт подключения (по-умолчанию для iSCSI трафика используется порт TCP 3260). ESXi просканирует все iSCSI таргеты на этом хосте и выведет их в списке Static Targets.

<img width="970" height="512" alt="image" src="https://github.com/user-attachments/assets/83a3d7f8-8608-4b98-80ab-962ecde92a20" />

Сохраните настройки. Обратите внимание, что на вкладке Storage -> Adapters появился новый HBA vmhba65 типа iSCSI Software Adapter.

<img width="1728" height="398" alt="image" src="https://github.com/user-attachments/assets/a61684db-d870-45ae-944c-be593a9bf345" />

<img width="1269" height="479" alt="image" src="https://github.com/user-attachments/assets/a322d3e8-6e54-4bc3-83b5-ca455fb67793" />

Если вы не видите список iSCSI таргетов на СХД, можно продиагностировать доступность iSCSI диска через консоль ESXi.

Включите SSH на VMware ESXi хосте и подключитесь к нему с помощью любого SSH клиента (я использую встроенный SSH клиент Windows 10)
```powershell
ssh root@192.168.13.50
```
С помощью следующей команды можно выполнить проверку доступности вашего iSCSI хранилища (192.168.13.10) с указанного vmkernel порта (vmk1) :
```powershell
# vmkping -I vmk1 192.168.13.10
```
<img width="533" height="149" alt="image" src="https://github.com/user-attachments/assets/a696d81e-70ae-4346-8fc4-64b15321868e" />

В этом примере iSCSI хранилище отвечает на ping.

Теперь нужно проверить, что на хранилище доступен iSCSI порт TCP 3260 (в этом примере 192.168.13.60 это IP адреса интерфейса vmk1):
```powershell
# nc -s 192.168.13.60 -z 192.168.13.10 3260
```
Connection to 192.168.13.10 3260 port [tcp/*] succeeded!

<img width="440" height="43" alt="image" src="https://github.com/user-attachments/assets/9d93c8dd-5a94-4b45-9285-ef8af79bc31e" />

esxi shell проверка доступности iscsi порта 3260

Проверьте, что на хосте включен программный iSCSI:
```powershell
# esxcli iscsi software get

true
```
Если нужно, включите его:
```powershell
# esxcli iscsi software set -e true

Software iSCSI Enabled
```
Также можно получить текущие параметры программного HBA адаптера iSCSI:
```powershell
# esxcli iscsi adapter get -A vmhba65
```
<img width="796" height="506" alt="image" src="https://github.com/user-attachments/assets/a14cb99b-b9f3-4657-99fa-f203c9aa7a13" />

#### Создаем VMFS хранилище на iSCSI LUN в VMWare ESXi
Теперь на доступном iSCSI диске можно создать VMFS (Virtual Machine File System) хранилище для размещения файлов виртуальных машин.

Перейдите в раздел Storage -> Datastores -> New datastore.

<img width="468" height="259" alt="image" src="https://github.com/user-attachments/assets/e58ce431-7d2d-458b-a224-b9c1eb963a00" />

Задайте имя VMFS хранилища и выберите iSCSI LUN, на котором его создать.

<img width="991" height="360" alt="image" src="https://github.com/user-attachments/assets/833cf674-d55f-4d5b-b9ad-0d767c82124e" />

создать vmfs Datastores на iscsi диске

Выберите тип файловой системы VMFS 6 и укажите, что для хранилища нужно использовать весь объем iSCSI диска. Через несколько секунд новое VMFS хранилище станет доступно из ESXi.

<img width="809" height="273" alt="image" src="https://github.com/user-attachments/assets/b531c6e2-1766-4413-acae-24176bee710a" />

новое vmfs хранилище для размещеия файлов виртуальных машин esxi

Если на данном LUN уже создано VMFS хранилище, оно сразу появится в списке доступных Storage Devices хоста.
```powershell
msft iscsi disk в vmware esxi
```

<img width="809" height="273" alt="image" src="https://github.com/user-attachments/assets/f5019177-633a-4dfd-bde1-0b71a0dbc288" />

Итак, вы подключили iSCSI диск к вашему ESXi хосту и создали на нем VMFS хранилище. Это хранилище могут одновременно использовать несколько ESXi серверов. Теперь у вас есть общее хранилище, и если вы настроите VMware vCenter server, вы сможете использовать vMotion для перемещения запущенных ВМ между хостами.











