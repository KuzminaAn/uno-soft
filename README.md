# Ansible Playbook для установки OpenLDAP

Этот Ansible playbook автоматизирует установку и настройку OpenLDAP сервера на Ubuntu с созданием пользователей и групп.

## Структура проекта

ldap/
├── inventory.yml         # Инвентарь с хостами
├── playbook.yml          # Основной playbook
├── vars.yml              # Переменные (домен, пользователи, группы)
├── vault.yml             # Зашифрованные пароли (Ansible Vault)
├── ldap-check.yml        # playbook для проверки корректности результата 
└── README.md             # Этот файл

## Подготовка перед запуском

1. Настройте SSH-доступ

На сервере должен быть разрешён вход по SSH под пользователем, который указан в инвентори.

2. Создайте зашифрованный файл vault.yml

Файл vault.yml содержит пароли и должен быть зашифрован с помощью Ansible Vault:

```
ansible-vault create vault.yml
```

Пример содержимого vault.yml:

```
ldap_admin_password: "admin"

ldap_passwords:
  user1: "pass2"
  user2: "pass2"
```

3. Отредактируйте инвентори файл и файл с переменными 

### Запуск playbook

1. Проверка подключения к серверу
```
ansible -i inventory.yaml ldap -m ping
```

2. Запуск установки и настройки

При запуске будет запрошен пароль для Ansible Vault.

```
ansible-playbook -i inventory.yml playbook.yml --ask-vault-pass
```

## Проверка установки

Для автоматической проверки LDAP после установки можно запустить плейбук ldap-check.yml

```
ansible-playbook -i inventory.yml ldap-check.yml
```
