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
Конфиг Раннера

```
concurrent = 1
check_interval = 0
shutdown_timeout = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "NEW_RUNNER"
  url = "http://gitlab.localdomain"
  id = 1
  token = "y1-gx65oTi91y7NCofhP"
  token_obtained_at = 2024-03-16T11:15:36Z
  token_expires_at = 0001-01-01T00:00:00Z
  executor = "docker"
  [runners.cache]
    MaxUploadedArchiveSize = 0
  [runners.docker]
    tls_verify = false
    image = "go:1.17"
    privileged = true
    extra_hosts = ["gitlab.localdomain:192.168.56.10"]
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
    shm_size = 0
    network_mtu = 0
```

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/59c98595560721d2cb459dbca34dbf71778aa155/img/Gitlab.png)`

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/59c98595560721d2cb459dbca34dbf71778aa155/img/Runner.png)`


### Задание 2
1. Запушили репозиторий на GitLab, изменив origin

```
vagrant@ubuntu-bionic:~/sdvps-materials$ git remote add gitlab http://gitlab.localdomain/root/gitlab_hw.git
vagrant@ubuntu-bionic:~/sdvps-materials$ git branch another_branch
vagrant@ubuntu-bionic:~/sdvps-materials$ git checkout another_branch
Switched to branch 'another_branch'
vagrant@ubuntu-bionic:~/sdvps-materials$ git push gitlab
Username for 'http://gitlab.localdomain': root
Password for 'http://root@gitlab.localdomain':
Counting objects: 97, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (89/89), done.
Writing objects: 100% (97/97), 27.02 KiB | 5.40 MiB/s, done.
Total 97 (delta 38), reused 0 (delta 0)
To http://gitlab.localdomain/root/gitlab_hw.git
 * [new branch]      another_branch -> another_branch
```
2. Создайте .gitlab-ci.yml И также его загрузили в репозиторий

```
vagrant@ubuntu-bionic:~/sdvps-materials$ git status
On branch another_branch
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   .gitlab-ci.yml
vagrant@ubuntu-bionic:~/sdvps-materials$ git config --global user.email "dvpergunov@mail.ru"
vagrant@ubuntu-bionic:~/sdvps-materials$ git config --global user.name "root"
vagrant@ubuntu-bionic:~/sdvps-materials$ git commit -am "NEW FILE"
[another_branch 162763c] ADD CI FILE
 1 file changed, 15 insertions(+)
 create mode 100644 .gitlab-ci.yml
vagrant@ubuntu-bionic:~/sdvps-materials$ git push gitlab
Username for 'http://gitlab.localdomain': root
Password for 'http://root@gitlab.localdomain':
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 371 bytes | 371.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To http://gitlab.localdomain/root/gitlab_hw.git
   223dbc3..162763c  another_branch -> another_branch
```
Код  .gitlab-ci.yml

```yml
stages:
  - test
  - build

test:
  stage: test
  image: golang:1.17
  script: 
   - go test .

build:
  stage: build
  image: docker:latest
  script:
   - docker build .
```
3. Запустили Pipeline в gitlab

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/350fb988daa97e9cc977e6abcc8bed088a37c6f7/img/RESULT1.png)`

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/350fb988daa97e9cc977e6abcc8bed088a37c6f7/img/Result2.png)`

![Название скриншота 1](https://github.com/dimindrol/GitLab_HW03/blob/350fb988daa97e9cc977e6abcc8bed088a37c6f7/img/Result3.png)`

---
