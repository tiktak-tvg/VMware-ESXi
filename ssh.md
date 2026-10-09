### Подключение SSH на VMware ESXi / vCenter
На самом хосте ESXi SSH-сервер (обычно это Dropbear) управляется иначе, чем в Linux.

#### Настраиваем сеть 

<img width="1438" height="564" alt="image" src="https://github.com/user-attachments/assets/706ec99f-62fa-481c-b5d5-c76e5f35aee1" />

> - Проверка порта: По умолчанию SSH на ESXi использует стандартный порт 22.

#### Управление службой, как запустить:

Через веб-интерфейс: **Host (Хост) -> Manage (Управление) -> Services (Службы) -> найдите SSH -> Start (Запустить)**.

Через консоль **ESXi (DCUI)**: зайдите в **Troubleshooting Options -> Enable SSH**.

<img width="1168" height="399" alt="image" src="https://github.com/user-attachments/assets/52aa8687-970c-4a99-a593-0aa9044ecbd0" />

#### Проверяем службы

<img width="1099" height="579" alt="image" src="https://github.com/user-attachments/assets/dfe22394-a6f2-4a17-a98a-f152d92871f6" />

#### Ошибки подключения, например

<img width="1478" height="419" alt="image" src="https://github.com/user-attachments/assets/45bd9b85-c05a-4835-adff-93b11de44082" />

или

<img width="1052" height="62" alt="image" src="https://github.com/user-attachments/assets/d0d41f63-a01c-4382-b0ee-dc9ddf26bbc7" />

или

<img width="1102" height="195" alt="image" src="https://github.com/user-attachments/assets/fc4741df-5138-47f1-9b1d-a9f314456fe0" />

#### Что можно проверить, если нет подключения:

##### 1.Доступность сервера — с другого терминала выполните:

```bash
ping 192.168.25.209
```
##### 2.Включить SSH через PowerCLI
Раз у вас уже работает Connect-VIServer, можно включить SSH прямо из PowerShell:

```powershell
# Подключение (если ещё не подключены)
Connect-VIServer 192.168.25.209

# Запустить службу SSH
Get-VMHost 192.168.25.209 | Get-VMHostService | Where-Object {$_.Key -eq "TSM-SSH"} | Start-VMHostService

# Проверить статус
Get-VMHost 192.168.25.209 | Get-VMHostService | Where-Object {$_.Key -eq "TSM-SSH"}

# (Опционально) сделать автозапуск
Get-VMHost 192.168.25.209 | Get-VMHostService | Where-Object {$_.Key -eq "TSM-SSH"} | Set-VMHostService -Policy "On"
```

<img width="1052" height="195" alt="image" src="https://github.com/user-attachments/assets/922f0a6e-981b-4f6d-9469-22fa083361f1" />

##### 3.Проверить доступ до другого порта, например 443, правда если вы зашли через Connect-VIServer, то его проверять нет смысла, он открыт
```powershell
Test-NetConnection -ComputerName 192.168.25.209 -Port 22
Test-NetConnection -ComputerName 192.168.25.209 -Port 443
```

<img width="1023" height="337" alt="image" src="https://github.com/user-attachments/assets/25edef78-6e35-499a-8501-198c992911fc" />

##### 4.Проверить правила файрвола на ESXi
ESXi имеет собственный файрвол. Убедитесь, что SSH разрешён:

```powershell
Get-VMHostFirewallException -VMHost 192.168.25.209 | Where-Object {$_.Name -like "*SSH*"}
```
Если Enabled: False, включите:

```powershell
Get-VMHostFirewallException -VMHost 192.168.25.209 -Name "SSH Server" | Set-VMHostFirewallException -Enabled $true
```

<img width="1073" height="318" alt="image" src="https://github.com/user-attachments/assets/60883131-05cb-4aa4-91d3-085222f9c8fb" />

##### 5.На самом деле, если соединение зависло на строке «Подключение к root@192.168.25.209…», или подключение по SSH к серверу root@192.168.25.209, вот что можно проверить:
Правила файервола

##### Пример добавления вашей подсети 192.168.25.0/24:

##### Скрипт
```powershell
# 1. Подключаемся к ESXi (если ещё не подключены)
Connect-VIServer 192.168.25.209

# 2. ОБЯЗАТЕЛЬНО создаём объект ESXCLI
$esxcli = Get-EsxCli -VMHost 192.168.25.209 -V2

# 3. Запрещаем доступ всем IP для SSH
$args = $esxcli.network.firewall.ruleset.set.CreateArgs()
$args.rulesetid = "sshServer"
$args.allowedall = $false
$esxcli.network.firewall.ruleset.set.Invoke($args)

# 4. Добавляем вашу подсеть в белый список
$args = $esxcli.network.firewall.ruleset.allowedip.add.CreateArgs()
$args.rulesetid = "sshServer"
$args.ipaddress = "192.168.25.0/24"
$esxcli.network.firewall.ruleset.allowedip.add.Invoke($args)

# 5. Проверяем результат
$esxcli.network.firewall.ruleset.allowedip.list.Invoke(@{rulesetid="sshServer"})
```

<img width="1172" height="872" alt="image" src="https://github.com/user-attachments/assets/d9fbb2f4-8cb9-4254-86e5-d929fee7a24c" />

или ручками 

<img width="1050" height="632" alt="image" src="https://github.com/user-attachments/assets/114c1275-dd3a-425e-b91b-df57f6a13986" />

<img width="1314" height="453" alt="image" src="https://github.com/user-attachments/assets/cc1bad8d-3fee-43c9-87df-482e4cf5dc08" />

<img width="1020" height="267" alt="image" src="https://github.com/user-attachments/assets/5379a781-c4d6-4747-ab02-08547c6158db" />
