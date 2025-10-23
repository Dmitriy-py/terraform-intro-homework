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
Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

## Ответ:

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


*   Ресурс: ` random_password.random_string `
*   Конкретный ключ: ` result `
*   Его значение: ` xWtBW2Uyr9B4JY10 `


























