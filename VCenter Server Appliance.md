##### Установка и настройка VCenter Server Appliance.

Откройте скачанный ISO-образ, перейдите в папку ``vcsa-ui-installer -> win32``(при установке с компьютера с Windows) и запустите файл ``installer.exe``

![image](https://github.com/user-attachments/assets/4b97e794-f1e7-47ae-89de-f7ea429a90a4)

![image](https://github.com/user-attachments/assets/9b1be3dd-77f2-493c-b5bc-7357a277c302)

Выбираем Install.

Далее 1 и 2 вкладка стандартный выбор

3-я вкладка

![image](https://github.com/user-attachments/assets/b50fe5fe-bd70-4288-9bbd-3c4e86b53a9d)

Далее 4-ая вкладка - вводим данные для подключения к серверу ESXI

Вот эти

![image](https://github.com/user-attachments/assets/1c06f9e6-3365-4eaf-9375-1f981ca53d7f)

![image](https://github.com/user-attachments/assets/5d40679f-f897-4a59-9167-36c82111b9c7)

![image](https://github.com/user-attachments/assets/58f1a3cf-7bfb-4e5a-adc6-be3f93638534)

Принимаем сертификат

Далее указываем название виртуальной машины и root пароль для доступа к консоли этой машины

![image](https://github.com/user-attachments/assets/7497ef34-93c9-4b8c-ae57-ab5efdf5b435)
Название виртуальной машины можно оставить по умолчанию

или написать своё, например так

![image](https://github.com/user-attachments/assets/eaa7e707-81fc-4d90-82f4-335f45cf0e5e)

Далее, выбираем конфигурацию нашей ВМ в зависимости от размера нашей инфраструктуры

![image](https://github.com/user-attachments/assets/d284bad5-d3f6-4213-8bcd-370d82423832)

Далее, указываем datastore, на котором будут располагаться файлы виртуальной машины VCSA(здесь можно выбрать «тонкий» формат дисков ВМ, установив соответствующий чекбокс).

![58](https://github.com/user-attachments/assets/c39be1f3-f5c8-458c-9cdb-502813e366af)

Дальше нам понадобится внутренний FQDN для vCenter.

> [!Warning]
> Смотрим, какой установлен DNS у рабочей станции.<br>
> Если у вас доменная машина, заходим на контроллер домена и добавляем DNS запись для vCenter, например: vcsa.dc01.local и ip адрес 192.168.25.202.

Если не в домене как у меня, значит заполняем так

![image](https://github.com/user-attachments/assets/2a406571-063c-4e55-a704-4dedbf3cb1b1)

![image](https://github.com/user-attachments/assets/eb7ce573-e2c2-4d9b-b616-223c2ba72919)

![image](https://github.com/user-attachments/assets/d1778bfe-2d3d-4ffc-afbf-4761b5dc0224)

Адрес ниже позволит в любой момент изменить конфигурацию vCenter. Сейчас он не нужен,но если перейти по адресу ``https://192.168.25.202:5480/`` то увидите это

![70](https://github.com/user-attachments/assets/417c3262-aa27-4bb7-8989-fff47263f06b)

##### Конфигурирование vCenter

Продолжим конфигурировать vCenter

![image](https://github.com/user-attachments/assets/f6964797-52e0-4b32-8caf-6dd98e733497)

Определяем настройки синхронизации времени, с хостом или с серверами точного времени в Интернете<br>
В случае синхронизации с серверами в Интернете, укажите имена серверов через запятую
![image](https://github.com/user-attachments/assets/e69b8bf2-cd38-4b80-b254-596632e92797)

Также, можно включить или отключить SSH доступ

![image](https://github.com/user-attachments/assets/dfbb016d-6beb-475e-8b18-567ec1f513cd)

Дальше идут настройки SSO-домена(Single Sign On). Можно создать новый SSO-domain или присоединиться к существующему.
Не рекомендуют использовать по умолчанию ``vsphere.local``, поэтому задаем своё имя домена, сайта и пароль администратора

![image](https://github.com/user-attachments/assets/c6fa4f24-609c-4cfc-8d8e-5475ef1bfb5b)

На следующем экране снимаем галку, если не хотим присоединяться к программе VMware Customer Experience Improvement Program

![image](https://github.com/user-attachments/assets/da52b89f-22cb-4d50-8268-e1ba95896352)

Итак в результате

![image](https://github.com/user-attachments/assets/1bdd0221-f33e-4218-beed-0474da6694eb)

Нажимаем ``Finish`` и начнется настройка VCSA

![image](https://github.com/user-attachments/assets/a865716f-2a6f-454c-a9f6-354625988711)

После ее завершения вы увидите сообщение об успешной установке и ссылку для перехода в интерфейс управления ``VCenter`` 
```python 
https://<FQDN or IP>:443
```
![image](https://github.com/user-attachments/assets/b9ee529e-f5dc-45c8-8305-d4b3fdc1f56c)

Вместо https://photon-machine:443 прописать  https://IP адрес вашей машины<br>
У меня, например будет так  https://192.168.25.202 Перейдём по ссылке

![image](https://github.com/user-attachments/assets/79dbce47-b8e1-4807-91db-5e0d3c1c96c8)

Перейдя по ссылке, можно выбрать либо Web-client либо HTML5-client(с ограниченной функциональностью).

vSphere Web Client откроется страница входа, введя учетные данные administrator@vsp.local попадаем в в интерфейс управления VCenter

![image](https://github.com/user-attachments/assets/38d0f144-9024-44d9-98e0-830ef25451e2)

Если хотите изменить конфигурацию vCenter перейти по адресу ``https://192.168.25.202:5480/``

Введя учетные данные administrator@vsp.local

![image](https://github.com/user-attachments/assets/7d9b5569-5e2b-4f8e-9a3b-e9d006984c28)

