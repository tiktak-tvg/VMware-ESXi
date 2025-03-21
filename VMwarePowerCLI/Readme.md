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
