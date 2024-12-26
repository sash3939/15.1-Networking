# Домашнее задание к занятию «Организация сети»

### Подготовка к выполнению задания

1. Домашнее задание состоит из обязательной части, которую нужно выполнить на провайдере Yandex Cloud, и дополнительной части в AWS (выполняется по желанию). 
2. Все домашние задания в блоке 15 связаны друг с другом и в конце представляют пример законченной инфраструктуры.  
3. Все задания нужно выполнить с помощью Terraform. Результатом выполненного домашнего задания будет код в репозитории. 
4. Перед началом работы настройте доступ к облачным ресурсам из Terraform, используя материалы прошлых лекций и домашнее задание по теме «Облачные провайдеры и синтаксис Terraform». Заранее выберите регион (в случае AWS) и зону.

---
### Задание 1. Yandex Cloud 

**Что нужно сделать**

1. Создать пустую VPC. Выбрать зону.

<img width="532" alt="VPC" src="https://github.com/user-attachments/assets/e78efdc9-f278-4f69-9934-3a89fa431fe5" />

<img width="1227" alt="VPC_browser" src="https://github.com/user-attachments/assets/655ae4e6-f12b-43be-bffa-33977e2db275" />

<img width="1199" alt="Zone" src="https://github.com/user-attachments/assets/8e78012c-88e4-4468-b79d-5aba5da34dbd" />
   
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 192.168.10.0/24.
 
  <img width="551" alt="public" src="https://github.com/user-attachments/assets/33cb20f5-ff70-4963-8c33-f63db903643a" />

  <img width="463" alt="public addresses" src="https://github.com/user-attachments/assets/411e28b2-6627-475f-8a80-436288bd5c1f" />

  <img width="1196" alt="subnet-public-browser" src="https://github.com/user-attachments/assets/f6af6cf7-2c99-42fd-8950-290d2a6de632" />

 - Создать в этой подсети NAT-инстанс, присвоив ему адрес 192.168.10.254. В качестве image_id использовать fd80mrhj8fl2oe87o4e1.
 
  <img width="492" alt="nat instance" src="https://github.com/user-attachments/assets/0d16be5c-19ac-4e9f-97d5-1636b6709fb5" />

  <img width="463" alt="Outputs" src="https://github.com/user-attachments/assets/c4ff7433-f9a9-4685-98c0-4f2dcf8a4e93" />

  <img width="506" alt="vars-nat-instance" src="https://github.com/user-attachments/assets/5b12d230-9808-4929-8d08-dc6e132e1ff3" />

  <img width="769" alt="vm-nat-instance" src="https://github.com/user-attachments/assets/de04be8e-ed19-4874-8e1d-d04e99258d6f" />

 - Создать в этой публичной подсети виртуалку с публичным IP, подключиться к ней и убедиться, что есть доступ к интернету.

  <img width="467" alt="public-vm" src="https://github.com/user-attachments/assets/49d07b40-1e09-4dc5-b635-4d1b0d59695e" />

  <img width="254" alt="public-vm-address" src="https://github.com/user-attachments/assets/74895503-b5d4-434a-a3a3-0c15f50c00b4" />

  <img width="1114" alt="vm-public" src="https://github.com/user-attachments/assets/0862b35b-80b8-47c9-bd00-b7e4dfe23e44" />

  <img width="577" alt="connect to public-vm" src="https://github.com/user-attachments/assets/55b95bc9-5124-41b4-a1ea-0c6bded2d622" />

3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 192.168.20.0/24.

  <img width="287" alt="private addresses" src="https://github.com/user-attachments/assets/0bc6c8b6-a9d6-4a18-ae33-d0962dfc11b5" />

  <img width="492" alt="private-vm" src="https://github.com/user-attachments/assets/c789774c-dbff-47e4-8701-aae41f37cb3a" />

  <img width="606" alt="subnet-private-browser" src="https://github.com/user-attachments/assets/38f22bba-b535-412e-911f-074f583ed4b7" />
 
 - Создать route table. Добавить статический маршрут, направляющий весь исходящий трафик private сети в NAT-инстанс.

  <img width="486" alt="route table" src="https://github.com/user-attachments/assets/0fb00cf7-07e7-4e75-93ae-7f464fcef6eb" />

  <img width="297" alt="nat-instance-ip" src="https://github.com/user-attachments/assets/7215023e-1c39-40bf-9d70-6608535ee372" />

  <img width="468" alt="nat plan" src="https://github.com/user-attachments/assets/bfb7d01b-29d3-4033-b5fd-8df546747953" />

  <img width="1100" alt="next hop" src="https://github.com/user-attachments/assets/a9ca0fbb-7de7-4a8c-9523-bfd03413efd5" />

 - Создать в этой приватной подсети виртуалку с внутренним IP, подключиться к ней через виртуалку, созданную ранее, и убедиться, что есть доступ к интернету.

  **Обязательно используем кманду ssh -J user1@jump-host user2@remote-host дл подключения через jump**
  
  <img width="1053" alt="private-vm-browser" src="https://github.com/user-attachments/assets/cc30792d-c26f-493e-891a-e9106167c83f" />

  <img width="580" alt="ping private-vm" src="https://github.com/user-attachments/assets/55bf289b-d130-4afa-ac72-316467223f41" />

Resource Terraform для Yandex Cloud:

- [VPC subnet](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_subnet).
- [Route table](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/vpc_route_table).
- [Compute Instance](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_instance).

---
### Задание 2. AWS* (задание со звёздочкой)

Это необязательное задание. Его выполнение не влияет на получение зачёта по домашней работе.

**Что нужно сделать**

1. Создать пустую VPC с подсетью 10.10.0.0/16.
2. Публичная подсеть.

 - Создать в VPC subnet с названием public, сетью 10.10.1.0/24.
 - Разрешить в этой subnet присвоение public IP по-умолчанию.
 - Создать Internet gateway.
 - Добавить в таблицу маршрутизации маршрут, направляющий весь исходящий трафик в Internet gateway.
 - Создать security group с разрешающими правилами на SSH и ICMP. Привязать эту security group на все, создаваемые в этом ДЗ, виртуалки.
 - Создать в этой подсети виртуалку и убедиться, что инстанс имеет публичный IP. Подключиться к ней, убедиться, что есть доступ к интернету.
 - Добавить NAT gateway в public subnet.
3. Приватная подсеть.
 - Создать в VPC subnet с названием private, сетью 10.10.2.0/24.
 - Создать отдельную таблицу маршрутизации и привязать её к private подсети.
 - Добавить Route, направляющий весь исходящий трафик private сети в NAT.
 - Создать виртуалку в приватной сети.
 - Подключиться к ней по SSH по приватному IP через виртуалку, созданную ранее в публичной подсети, и убедиться, что с виртуалки есть выход в интернет.

Resource Terraform:

1. [VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc).
1. [Subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet).
1. [Internet Gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway).

### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
