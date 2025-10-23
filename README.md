# Домашнее задание к занятию «Введение в Terraform»

Цели задания
Установить и настроить Terrafrom.
Научиться использовать готовый код.
Чек-лист готовности к домашнему заданию
Скачайте и установите Terraform версии >=1.8.4 . Приложите скриншот вывода команды terraform --version.
Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории 01/src.
Убедитесь, что в вашей ОС установлен docker.

<img width="1920" height="1080" alt="Снимок экрана (1671)" src="https://github.com/user-attachments/assets/868a0a75-9480-474e-8379-55cb79360d58" />

## Задание 1
1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
2. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

## Ответ:

## 1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.

### Вывод команды ` terraform init `
```terraform
Initializing the backend...
Initializing provider plugins...
- Finding kreuzwerker/docker versions matching "~> 3.0.1"...
- Finding latest version of hashicorp/random...
- Installing kreuzwerker/docker v3.0.2...
- Installed kreuzwerker/docker v3.0.2 (unauthenticated)
- Installing hashicorp/random v3.7.2...
- Installed hashicorp/random v3.7.2 (unauthenticated)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

╷
│ Warning: Incomplete lock file information for providers
│
│ Due to your customized provider installation methods, Terraform was forced to calculate
│ lock file checksums locally for the following providers:
│   - hashicorp/random
│   - kreuzwerker/docker
│
│ The current .terraform.lock.hcl file only includes checksums for linux_amd64, so
│ Terraform running on another platform will fail to install these providers.
│
│ To calculate additional checksums for another platform, run:
│   terraform providers lock -platform=linux_amd64
│ (where linux_amd64 is the platform to generate)
╵
Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

### Вывод команды ` cat .terraform.lock.hcl `

```hcl
# This file is maintained automatically by "terraform init".
# Manual edits may be lost in future updates.

provider "registry.terraform.io/hashicorp/random" {
  version = "3.7.2"
  hashes = [
    "h1:356j/3XnXEKr9nyicLUufzoF4Yr6hRy481KIxRVpK0c=",
  ]
}

provider "registry.terraform.io/kreuzwerker/docker" {
  version     = "3.0.2"
  constraints = "~> 3.0.1"
  hashes = [
    "h1:cT2ccWOtlfKYBUE60/v2/4Q6Stk1KYTNnhxSck+VPlU=",
  ]
}
```

## 2. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)

### Вывод команды ` cat .gitignore `

```terrafform
# Local .terraform directories and files
**/.terraform/*
.terraform*

!.terraformrc

# .tfstate files
*.tfstate
*.tfstate.*

# own secret vars store.
personal.auto.tfvars
```

Файл .gitignore определяет, какие файлы не должны попадать в систему контроля версий (Git). Если файл игнорируется Git, то он остается только на вашем локальном компьютере, и его можно безопасно использовать для хранения конфиденциальных данных (в данном контексте).

### Строка из ` .gitignore: `

```terraform
# own secret vars store.
personal.auto.tfvars
```
Функция в Terraform: Файлы, заканчивающиеся на .auto.tfvars или .tfvars, автоматически считываются Terraform для предоставления значений переменных. Поскольку personal.auto.tfvars игнорируется Git, мы можем создать этот файл локально, поместить туда свои логины, пароли, ключи и токены, и быть уверенным, что они не будут случайно закоммичены и отправлены в публичный репозиторий.


## 3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.

*   Ресурс: ` random_password.random_string `
*   Конкретный ключ: ` result `
*   Его значение: ` xWtBW2Uyr9B4JY10 `

## 4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.

Ошибка 1: Error: Missing name for resource

Намеренная ошибка: Блок resource "docker_image" { не имеет локального имени. В Terraform каждый ресурс должен иметь тип (docker_image) и уникальное локальное имя (например, "nginx"), чтобы на него можно было ссылаться.
Сообщение об ошибке: All resource blocks must have 2 labels (type, name).
Ошибка 2: Error: Invalid resource name

Намеренная ошибка: Локальное имя ресурса docker_container было задано как "1nginx". Локальные имена ресурсов в Terraform должны начинаться с буквы или подчеркивания. Начинать имя с цифры недопустимо.
Сообщение об ошибке: A name must start with a letter or underscore and may contain only letters, digits, underscores, and dashes.

(Примечание: В вашем коде также была логическая ошибка в ссылке на пароль (random_string_FAKE.resulT), но terraform validate первым делом обнаружил синтаксические ошибки и ошибки именования, прежде чем дойти до проверки ссылок.)

## 5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.

### Исправленный фрагмент кода файла ` main.tf `
```terraform

resource "random_password" "random_string" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "docker_image" "nginx" { # ИСПРАВЛЕНИЕ 1: Добавлено имя ресурса "nginx"
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx_container" { # ИСПРАВЛЕНИЕ 2: Корректное имя ресурса
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}" # ИСПРАВЛЕНИЕ 3: Корректная ссылка на пароль

  ports {
    internal = 80
    external = 9090
  }
}

```

### Вывод команды ` docker ps `

```docker
vm1@vm1-VirtualBox:~/ter-homeworks/01/src$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                         NAMES
ef550265fc2e   657fdcd1c365         "/docker-entrypoint.…"   19 seconds ago   Up 14 seconds   0.0.0.0:9090->80/tcp                          example_xWtBW2Uyr9B4JY10

```

## 6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.

* Опасность применения ключа -auto-approve: Ключ -auto-approve автоматически подтверждает выполнение плана, минуя ручную      проверку. Это опасно, так как может привести к непреднамеренному удалению или изменению критически важных ресурсов, если    в плане есть ошибка или если он вызван ошибочно, что ведет к простоям или потере данных.

* Зачем может пригодиться данный ключ: Он необходим в CI/CD пайплайнах (системах автоматического развертывания) для           автоматического применения изменений, когда план уже прошел предварительную проверку.

### Вывод команды ` docker ps `

```docker
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                         NAMES
68d25694fd6b   657fdcd1c365         "/docker-entrypoint.…"   36 seconds ago   Up 34 seconds   0.0.0.0:9090->80/tcp                          hello_world

```

### 7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate

### Вывод команды ` docker ps `
```docker
CONTAINER ID   IMAGE                COMMAND                  CREATED      STATUS      PORTS                                         NAMES
```

### Содержимое ` terraform.tfstate: `
```json
{
  "version": 4,
  "terraform_version": "1.13.0",
  "serial": 11,
  "lineage": "0e3dd2a9-a40e-2e88-2a80-a514e0dbf301",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```
Содержимое `resources: []` (пустой массив) подтверждает, что в файле состояния больше нет записей об управляемых ресурсах.

## 8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

### Ответ в предоставленном коде `main.tf:`
```terraform
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true  // <-- ЭТА СТРОКА
}
```
Docker-образ nginx:latest не был удален при выполнении terraform destroy потому, что в конфигурации ресурса docker_image "nginx" был установлен аргумент keep_locally = true. Эта строка кода явно указывает Terraform сохранить образ локально, даже если ресурс уничтожается. Это подтверждается документацией провайдера Docker:

keep_locally (Optional, bool) If true, then the Docker image won’t be deleted on terraform destroy if it has not been used by any local container.

















