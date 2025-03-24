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

![67](https://github.com/user-attachments/assets/fe4af087-e975-44b1-91bd-610b900d0791)

Адрес ниже позволит в любой момент изменить конфигурацию vCenter. Сейчас он не нужен,но если перейти по адресу ``https://vcsa.local:5480/`` то увидите это

![image](https://github.com/user-attachments/assets/d0c0a596-a616-4a1b-85ea-168bd1abd40f)

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

![image](https://github.com/user-attachments/assets/f090d4e8-8685-4a70-aa22-f24834282108)






