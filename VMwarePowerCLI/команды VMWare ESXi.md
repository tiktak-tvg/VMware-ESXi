Справочный список полезных консольных команд VMWare ESXi (в том числе ESXCLI), которые часто используется при траблшутинге и тонкой настройке гипервизора. По мере необходимости список команд будет расширяться и обновляется.

Доступные команды консоли ESXi можно посмотреть в каталоге /usr/sbin.

cd /usr/sbin
ls

Совет. Обратите внимание, что все команды esxi регистрозависимы.
все консольные команды esxi 

Полный список команд esxcli можно вывести с помощью команды:

esxcli esxcli command list

вывести все команды esxcli 

Для начала команды ESXi, которые вы можете выполнять через ssh доступ.
```bash
reboot — перезагрузить хост
poweroff — выключить хост
esxcli system version get — узнать версию (номер) инсталлированной версии VMware ESXi
uname -a — так же узнать версию VMware ESXi
узнать версию VMware ESXi

vmware –vl – и еще один способ узнать версию и релиз VMware ESXi
получить номер установленной версии ESXi

esxcli hardware pci list | more — полная информация об установленных PCI устройствах
lspci — краткая информация обо всех установленных PCI устройствах
esxtop — диспетчер процессов top для vmware esxi (быстрые клавиши для переключения дисплея: c:cpu, i:interrupt, m:memory, n:network, d:disk adapter, u:disk device, v:disk VM,p:power mgmt)
vmkerrcode -l — расшифровка кодов ошибок
esxcfg-nics -l — информация о сетевых картах
esxcfg-vswitch -l — информация о виртуальных коммутаторах
find . -name libstorelib.so — найти файл libstorelib.so
поиск файла на ESXi

dcui — работа с консолью сервера через ssh сессию
chkconfig -l — статус работы демонов
esxcli hardware memory get — размер установленной памяти
esxcli software vib list — список установленных vib-пакетов
esxcli network ip connection list — состояние активных соединений (аналог netstat)
esxcli storage vmfs extent list — информация о примонтированных/подключенных томах VMFS
esxcli hardware clock (get/set) — отображение/установка времени esxi-хоста
cd - Смена текущей директории;
cp - Копирование файла.cp [файл 1] [файл2];
find - Поиск файлов по критериям;
ls - Список файлов и директорий в текущей или явно указанной директории.ls /vmfs/volumes/ ключи: -l подробная информация -a отображение скрытых файлов;
mkdir — Создание директории;
mv — Перемещение файла. Переименование файла.mv [путь и имя файла] [путь, куда перемещать];
ps — Информация о запущенных процессах. ps -ef;
rm - Удаление файлов;
shutdown — Выключение или перезагрузка сервера shutdown nowshutdown –r now;
vi — Текстовый редактор;
nano — Дружелюбный к новичкам текстовый редактор, отсутствует на ESXi;
cat — Вывод содержимого файла на экран. cat /etc/hosts;
more — Вывод содержимого файла на экран, по странице за раз. more /etc/hosts;
man — Справка по командам man <команда, по которой есть вопрос>, для некоторых команд помощь выводится при запуске самой команды без параметров;
useradd — Создание пользователя. useradd <имя пользователя>;
passwd -Задание пароля пользователю passwd <имя пользователя>;
esxcli storage nfs list — список подключеных nfs- хранлилищ на хосте
esxcli software vib list — cписок установленных vib-пакетов
esxcli hardware memory get — информация об использовании памяти на хосте ESXi, включая общий объем RAM
esxcli hardware cpu list — информация о количестве процессоров на хосте ESXi
esxli iscsi adapter list — список iSCSI-адаптеров и их имена
 esxcli network nic list — список сетевых адаптеров
esxcli network ip interface list — Информация об IP-интерфейсах хоста
esxcli network ip dns search list — Информация о настройках DNS
ist — Состояние активных соединений (аналог netstat)
network neighbors list — #Вывод ARP-таблицы
esxcli network firewall get
esxcli network firewall ruleset list — Состояние сетевого экрана (файервола) ESXi и активные правила для портов и сервисов;
esxcli storage vmfs extent list — Информация о VMFS разделах, подключенных к хосту
esxcli storage filesystem list — Мапинг VMFS-томов к устройствам
esxcli storage core path list
esxcli storage core device list — Вывод информации о путях и устройствах Fibre Channel (FC)
esxcli storage core plugin list — Список плагинов NMP, загруженных в систему
esxcli storage core adapter rescan – Выполнить рескан HBA-адаптеров
esxcli vm process list — получаем ID виртуальной машины
esxcli vm process kill --type=[soft,hard,force] --world-id=WorldID убиваем процесс виртуальной машины ID (помогает от зависших и не отвечающих в vSphere Client ВМ)
esxcli system welcomemsg get
esxcli system welcomemsg set — Получить текст и изменить приветственное сообщение ESXi
esxcli system settings advanced list | grep smth — Поискать что-нибудь в Advanced Settings хоста
esxcli hardware clock get — Текущее аппаратное время хоста
esxcli hardware bootdevice list — Порядок загрузки с устройств
esxcli hardware pci list — Список PCI-устройств
esxcli iscsi adapter discovery rediscover — Сканирование iSCSI-адаптеров
esxcli storage core adapter rescan [-A | -all] — Рескан iSCSI
Команды для работы с виртуальными машинами:

vim-cmd vmsvc/getallvms — вывод информации обо всех VM
vim-cmd vmsvc/power.getstate 1 — включена/выключена VM с Vmid 1
vim-cmd vmsvc/power.on 1 — включить VM с Vmid 1
vim-cmd vmsvc/power.off 1 — выключить (по питанию) VM с Vmid 1
vim-cmd vmsvc/power.reset 1 — перезагрузка (аналогично нажатию клавиши RESET на реальном сервере) VM с Vmid 1
vim-cmd vmsvc/power.shutdown 1 — корректное выключение VM с Vmid 1. Действует только, если установлены VMware Tools!
vim-cmd vmsvc/power.reboot 1 — перезагрузка VM с Vmid 1. Действует только, если установлены VMware Tools!
vim-cmd vmsvc/get.summary 1 — получение полной информации о VM с Vmid 1.
vim-cmd vmsvc/get.summary 1 | egrep ‘(name|power|ip)’ — получение отфильтрованной информации о VM с Vmid 1. Выводится имя, состояние питания, IP-адрес
vim-cmd vmsvc

Набрав эту команду, вы увидите все возможные варианты ее использования. Ниже список команд, которые мне показались полезными:

vim-cmd vmsvc/power.getstate <vmid> статус питания виртуальной машины с указанным ID. Увидеть список ВМ и их ID вы можете при помощи команды;
vim-cmd vmsvc/getallvms — Выключить питание виртуальной машины;
vim-cmd vmsvc/power.off vmid — Включить питание виртуальной машины;
vim-cmd vmsvc/power.on vmid — Перезагрузить виртуальную машину;
vim-cmd vmsvc/power.reboot vmid — Удалить файлы виртуальной машины;
vim-cmd vmsvc/destroy vmid — Удалить файлы виртуальной машины;
vim-cmd vmsvc/power.shutdown <vmid> — Выключение виртуальной машины (shutdown guest);
vim-cmd vmsvc/power.reset <vmid> — Перезагрузка виртуальной машины;
vim-cmd vmsvc/get.summary <vmid> — Общая информация о виртуальной машине;
vim-cmd solo/registervm /vmfs/vol/datastore/dir/vm.vmx — Подключить виртуальную машину;
vim-cmd vmsvc/unregister vmid — Убрать виртуальную машину из гипервизора;
vim-cmd vmsvc/tools.install vmid — Установка vmware tools;
vim-cmd hostsvc/net/info — информация о сети гипервизора;
vim-cmd hostsvc/maintenance_mode_enter — Переключить хост в режим обслуживания;
vim-cmd hostsvc/maintenance_mode_exit — Выйти из режима обслуживания;
chkconfig -l — Показать службы запущенные на гипервизоре;
esxtop — Список процессов;
vmkerrcode -l — посмотреть vmkernel ошибки;
esxcfg-info — Посмотреть информацию о хосте;
esxcfg-nics -l — Посмотреть информацию о сетевых адаптерах;
esxcfg-vswitch -l — Посмотреть информацию о виртуальных сетевых адаптерах;
dcui — Стартовая консоль ESXI по ssh;
vsish — Vmware интерактивная консоль;
cat /etc/chkconfig.db — посмотреть состояние сервисов на хосте;
/sbin/services.sh restart — перезагрузить все сервисы на хосте;
vmkload_mod --list — Показать загруженные драйвера;
vmkload_mod -s /mod/your_driver — Показать параметры драйверов;
vmkfstools -i /vmfs/volumes/san_vmfs/my_vm/large_disk.vmdk -d thin /vmfs/volumes/san_vmfs/my_vm/new_thin_disk.vmdk — Конвертировать существующий диск в thin формат;
```
