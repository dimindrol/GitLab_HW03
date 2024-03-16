# Домашнее задание к занятию "`HW_03 Gitlab`" - `Пергунов Дмитрий Владимирович`

### Задание 1

1. Разверните GitLab локально, используя Vagrantfile
2. Создали Пустой репозиторий
3. Зарегистрировали gitlab-runner для этого проекта и запустите его в режиме Docker

```
vagrant@ubuntu-bionic:~/sdvps-materials$    sudo docker run -ti --rm --name gitlab-runner \
>      --network host \
>      -v /srv/gitlab-runner/config:/etc/gitlab-runner \
>      -v /var/run/docker.sock:/var/run/docker.sock \
>      gitlab/gitlab-runner:latest register
Runtime platform                                    arch=amd64 os=linux pid=7 revision=782c6ecb version=16.9.1
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
http://gitlab.localdomain/
Enter the registration token:
GR134894194yhekTHQX9ZgB-ekoci
Enter a description for the runner:
[ubuntu-bionic]:
Enter tags for the runner (comma-separated):

Enter optional maintenance note for the runner:

WARNING: Support for registration tokens and runner parameters in the 'register' command has been deprecated in GitLab Runner 15.6 and will be replaced with support for authentication tokens. For more information, see https://docs.gitlab.com/ee/ci/runners/new_creation_workflow
Registering runner... succeeded                     runner=GR134894194yhekTH
Enter an executor: kubernetes, docker-autoscaler, instance, custom, shell, ssh, docker, parallels, virtualbox, docker-windows, docker+machine:
docker
Enter the default Docker image (for example, ruby:2.7):
go:1.17
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
vagrant@ubuntu-bionic:~/sdvps-materials$ sudo nano /srv/gitlab-runner/config
vagrant@ubuntu-bionic:~/sdvps-materials$ cd /srv/gitlab-runner/config
vagrant@ubuntu-bionic:/srv/gitlab-runner/config$ ls
config.toml
vagrant@ubuntu-bionic:/srv/gitlab-runner/config$ sudo nano config.toml
vagrant@ubuntu-bionic:/srv/gitlab-runner/config$    sudo   docker run -d --name gitlab-runner --restart always \
>      --network host \
>      -v /srv/gitlab-runner/config:/etc/gitlab-runner \
>      -v /var/run/docker.sock:/var/run/docker.sock \
>      gitlab/gitlab-runner:latest
029be5751e2dc3555821ea56222ec8a71797b735d34ac1cb1834edcf28fe8119
```

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/59c98595560721d2cb459dbca34dbf71778aa155/img/Gitlab.png)`

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/59c98595560721d2cb459dbca34dbf71778aa155/img/Runner.png)`


---

### Задание 2
1. `Заполните здесь этапы выполнения, если требуется ....`

```
лброллроолр
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`


---
