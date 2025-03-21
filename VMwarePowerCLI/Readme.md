##### Как установить VMware PowerCLI для vSphere Management Automation

PowerCLI — один из самых мощных инструментов для управления и автоматизации VMware vSphere и vCloud.<br> 
Вы можете управлять подготовкой виртуальных машин, хранением, сетевыми подключениями, операционными системами на хостах и ​​гостях, изменениями и любыми другими аспектами VMware vSphere. <br>
Давайте рассмотрим, что такое PowerCLI и основы установки этого инструмента управления VMware vSphere.

##### Как установить модуль PowerCLI

Найдите модуль PowerCLI в репозиториях PowerShell Gallery:
```python
Find-Module -Name VMware.PowerCLI
```
Чтобы установить модуль PowerCLI для всех пользователей, выполните команду:
```python
Install-Module -Name VMware.PowerCLI
```
По умолчанию установлена ​​последняя версия PowerCLI.
> [Warning]
> Модуль «VMware.VimAutomation.Sdk» не может быть установлен или обновлен, поскольку подпись authenticode файла «VMware.VimAutomation.Sdk.cat» недействительна

```python
В этом случае выполните следующую команду, чтобы установить PowerCLI без ошибок:

install-module VMware.PowerCLI -scope AllUsers -force -SkipPublisherCheck -AllowClobber
```
Проверьте версию PowerCLI после завершения установки:
```python
Get-PowerCLIVersion
```
Вы можете указать, следует ли участвовать в программе улучшения качества обслуживания клиентов VMware. 

Чтобы сказать «Нет»

выполните команду:
```python
Set-PowerCLIConfiguration -Scope User -ParticipateInCEIP $false
```
Выведите список всех доступных командлетов после установки PowerCLI:
```python
Get-Command -Module *VMWare*
```
или
```python
Get-Module -ListAvailable VMware* | Select Name,version
```
Команда для обновления модуля PowerCLI в PowerShell:
```python
Update-Module -Name VMware.PowerCLI
```
Если вы хотите установить определенную версию, используйте команды, показанные ниже.

Проверьте доступные версии vSphere PowerCLI в онлайн-репозиториях:
```python
Find-Module -Name VMware.PowerCLI -AllVersions|select version
```
ведите необходимую версию, выбранную из вывода предыдущей команды, например,
12.4.1.18769701
```python
Install-Module -Name VMware.PowerCLI -RequiredVersion 12.4.1.18769701
```
После завершения установки PowerCLI вы сможете использовать командлеты VMware vSphere в PowerShell.

Проверьте соединение с сервером с помощью команды
```python
Connect-VIServer
```
и посмотрите, нет ли ошибки сертификата. Попробуйте решить проблему с помощью команды:
```python
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore
```
Если все правильно, вы можете подключиться к vCenter Server или хосту ESXi с помощью команды:
```python
Connect-VIServer 10.10.10.11
```
Используйте имя хоста или IP-адрес нужного сервера.

Получите список виртуальных машин VMware, управляемых сервером, к которому вы подключились:
```python
Get-VM
```
Теперь вы можете запускать другие команды и создавать скрипты с помощью VMware PowerCLI.

##### Как установить PowerCLI с помощью Chocolatey

Chocolatey — это менеджер пакетов для Windows. Он используется в PowerShell для установки пакетов программного обеспечения из онлайн-репозиториев.<br> 
Chocolatey (choco) использует NuGet для упаковки программного обеспечения и создан на основе других технологий Windows.<br> 
Этот менеджер пакетов помогает вам легко управлять программным обеспечением, включая установку и удаление пакетов.

Задайте правильную политику для установки choco, если вы еще не установили его:
```python
Set-ExecutionPolicy AllSigned
```
Выполните эту сложную команду для установки choco:
```python
Set-ExecutionPolicy Bypass -Scope Process -Force;
[System.Net.ServicePointManager]::SecurityProtocol =
[System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object
System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
Найдите модуль VMware в онлайн-репозитории резервных копий:
```python
choco search vmware
```
Установите VMware PowerCLI с Chocolatey из онлайн-репозитория программного обеспечения:
```python
choco install vmware-powercli-psmodule
```
```python
Введите
Y
или
A,
чтобы продолжить.
```
Дождитесь загрузки и установки всех компонентов пакета










