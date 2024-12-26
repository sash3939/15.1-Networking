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
 - Создать route table. Добавить статический маршрут, направляющий весь исходящий трафик private сети в NAT-инстанс.
 - Создать в этой приватной подсети виртуалку с внутренним IP, подключиться к ней через виртуалку, созданную ранее, и убедиться, что есть доступ к интернету.

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
