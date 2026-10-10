### Настройка и подключение iSCSI-хранилища
Для начала, что требуется сделать.

Чтобы не возникло проблем, когда у LUN-устройства нет достаточного количества путей доступа, настроим два сетевых пути. 
- Это не обязательно означает полный отказ оборудования — система работает, но защита от сбоев на этом пути снижена.

#### Настраиваем сеть в VMware ESXi

- vSwitch3 (ISCSI-1): vmk1 (192.168.25.185) → vmnic1.
- vSwitch4 (ISCSI-2): vmk2 (192.168.25.186) → vmnic2.
- ARP и ping работают с обоих интерфейсов.
- На сервере Windows порт 3260 слушается и брандмауэр включен.

<img width="1367" height="594" alt="image" src="https://github.com/user-attachments/assets/52a9adba-ae00-4647-9624-4b409fa30c67" />

***
<img width="1361" height="593" alt="image" src="https://github.com/user-attachments/assets/7bb83da5-8eca-49e6-b67c-535f0c0af503" />

---
<img width="1105" height="657" alt="1" src="https://github.com/user-attachments/assets/5e8f9ef4-c726-42ec-9d24-c7d81871dd7e" />

---
<img width="1103" height="667" alt="2" src="https://github.com/user-attachments/assets/17eabda2-28da-4a4f-a513-cbd6c001ecc8" />

#### Настройка и подключение iSCSI-хранилища в Windows Server 

<img width="1352" height="708" alt="3" src="https://github.com/user-attachments/assets/ace4bfc2-396b-4173-9e95-7ec938a52a72" />

---
<img width="1160" height="379" alt="4" src="https://github.com/user-attachments/assets/4392b189-7456-4701-babb-ad49eccad41b" />

---
<img width="1128" height="552" alt="5" src="https://github.com/user-attachments/assets/eb7e775a-8bd8-4e61-bce6-08c4e7b813ce" />

---
<img width="1352" height="661" alt="6" src="https://github.com/user-attachments/assets/c29b8306-c3b0-43e5-9fbe-2a817e50b6a3" />

#### Подготавливаем хранилища(диски), чтобы они были видны в списке хранилищ при подключении по ISCSI

<img width="1117" height="667" alt="10" src="https://github.com/user-attachments/assets/948f8656-09ef-4f65-b45c-7c05492dabe6" />

---
<img width="1323" height="869" alt="7" src="https://github.com/user-attachments/assets/8cc2699d-02fb-45c2-85f9-610c05acdf78" />

---
<img width="1309" height="866" alt="8" src="https://github.com/user-attachments/assets/ccaef49a-4e5b-4666-bf2e-cb750e17c809" />

---
<img width="1164" height="590" alt="9" src="https://github.com/user-attachments/assets/3c0b9e90-bf36-4f87-8cce-99213ee75016" />

---
<img width="1140" height="859" alt="11" src="https://github.com/user-attachments/assets/f9e5909f-0311-4269-a708-8ce58793b1f7" />

---
<img width="1142" height="839" alt="12" src="https://github.com/user-attachments/assets/8ca28395-e7e4-4907-9098-1cf49cd64868" />

#### Создаём первый таргет

<img width="1115" height="902" alt="image" src="https://github.com/user-attachments/assets/fdacba49-6db6-41a7-8a52-e1a431852789" />

---
<img width="1113" height="901" alt="image" src="https://github.com/user-attachments/assets/373345d1-e1c1-460c-b855-69b7998f08f8" />

---
<img width="1117" height="903" alt="image" src="https://github.com/user-attachments/assets/abd5f310-8047-4f21-99a8-76e286e4d1c2" />

---

#### Инициализируем от куда к таргету будем подключаться

<img width="1109" height="883" alt="image" src="https://github.com/user-attachments/assets/8d391731-77a1-4947-9a38-059b8478470a" />

---
<img width="1117" height="903" alt="image" src="https://github.com/user-attachments/assets/b0bfb9a4-1658-4434-875a-709c1d50956e" />

---
<img width="1109" height="889" alt="image" src="https://github.com/user-attachments/assets/9d74d25d-5f6c-4c7e-b384-e8d76814071e" />

#### Проверяем как настроен брандмауэр windows

<img width="1258" height="662" alt="image" src="https://github.com/user-attachments/assets/1a427ef5-6dc5-4acf-a8b7-34d08fce863b" />

---

#### Настраиваем ISCSI на VMware ESXi

<img width="1014" height="701" alt="image" src="https://github.com/user-attachments/assets/322c4d53-f73d-4c09-8f6c-cf43816b6b52" />

---
#### Настраиваем брандмауэр windows

<img width="1284" height="799" alt="image" src="https://github.com/user-attachments/assets/31c9ba37-7d56-40c8-91ef-4320d15d5d3d" />

---
<img width="1167" height="670" alt="image" src="https://github.com/user-attachments/assets/d165cb52-36e5-4fa0-8565-9b9241eec279" />

---
<img width="1254" height="563" alt="image" src="https://github.com/user-attachments/assets/eef6ba89-b4d1-48ba-ab41-cb3e8261a0d7" />

---

*Сейчас всё работает: цель подключена, диск виден, осталось только создать на нем хранилище.*

1. На скриншоте с устройствами MSFT iSCSI Disk (naa.60003ff44dc75adca7f572ae1b6ff28c) нажмите кнопку New datastore (Новое хранилище).

<img width="1237" height="395" alt="image" src="https://github.com/user-attachments/assets/8aa50853-6a33-4b92-8d7a-effe8ae3759f" />

2. Выберите тип VMFS.

3. Дайте хранилищу имя (например, iSCSI-DB1).

4. Выберите в списке устройств ваш диск MSFT iSCSI Disk (naa.60003ff44dc75adca7f572ae1b6ff28c).

5. Выберите версию VMFS (обычно VMFS 6).

6. Завершите мастер.

После этого новое хранилище появится в разделе Storage слева, и вы сможете создавать на нем виртуальные машины или перемещать туда существующие.

<img width="1154" height="383" alt="image" src="https://github.com/user-attachments/assets/31973191-f8fc-4ea9-b909-68649e976c4d" />

***













