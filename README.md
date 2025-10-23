# Домашнее задание к занятию «Введение в Terraform»

Цели задания
Установить и настроить Terrafrom.
Научиться использовать готовый код.
Чек-лист готовности к домашнему заданию
Скачайте и установите Terraform версии >=1.8.4 . Приложите скриншот вывода команды terraform --version.
Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории 01/src.
Убедитесь, что в вашей ОС установлен docker.

<img width="1920" height="1080" alt="Снимок экрана (1671)" src="https://github.com/user-attachments/assets/868a0a75-9480-474e-8379-55cb79360d58" />

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
