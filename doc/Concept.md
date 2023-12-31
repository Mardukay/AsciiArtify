# Порівняльний аналіз minikube, kind та k3d

## Зміст
1. [Вступ](#вступ)
2. [Характеристики](#Характеристики)
3. [Переваги та недоліки](#Переваги-та-недоліки)
4. [Порівнювальна таблиця](#Порівнювальна-таблиця)
5. [Висновок](#Висновок)




## Вступ ##

### K3d ###
K3d - це незалежна від платформи, легка обгортка, яка запускає K3s в контейнері docker. Вона допомагає швидко запускати та масштабувати одновузлові або багатовузлові кластери K3S без подальшого налаштування, зберігаючи режим високої доступності.
### Kind ###
Основним призначенням Kind (Kubernetes in Docker) є тестування Kubernetes, але він також допомагає запускати локальні кластери Kubernetes та CI-конвеєри, використовуючи контейнери Docker як “вузли”.

Це сертифікований CNFS інсталятор Kubernetes з відкритим вихідним кодом, який підтримує високодоступні багатовузлові кластери та створює релізи Kubernetes з його джерела.

### Minikube ###
miniKube - це найбільш популярний локальний інсталятор Kubernetes. Він пропонує простий у використанні посібник з встановлення та запуску одновузлових середовищ Kubernetes на різних операційних системах. Він розгортає Kubernetes як контейнер, віртуальну машину або bare-metal і реалізує точку доступу до Docker API, яка допомагає швидше передавати образи контейнерів. Він має розширені функції, такі як балансування навантаження, монтування файлових систем і FeatureGates, що робить його улюбленим для локального запуску Kubernetes.

## Характеристики

### Minikube

- Підтримує останню версію Kubernetes (+6 попередніх мінорних версій)

- Кросплатформенний (Linux, macOS, Windows) 

- Розгортайте як віртуальну машину, контейнер або на фізичному сервері 

- Кілька контейнерних рантаймів (CRI-O, containerd, docker)

- Прямий ендпоінт API для швидкого завантаження та побудови образів

- Розширені функції, такі як LoadBalancer, монтування файлових систем, FeatureGates та мережева політика

- Аддони для легкого встановлення додатків Kubernetes

- Підтримує загальні середовища CI

  

### Kind

- підтримка мультинодових кластерів (включаючи HA)
-  підтримка збирання релізів Kubernetes з вихідного коду(підтримка для make/bash чи docker, додатково до передрелізних збірок)
- підтримка Linux, macOS та Windows
- сертифікований CNCF інсталятор Kubernetes



### k3d 

- підтримка мультинодових кластерів (включаючи HA)
- сертифікований CNCF інсталятор Kubernetes
- швидкий
- потребує мало пам'яті
- підтримка Linux

## Переваги та недоліки

### Minikube

Переваги:

- має велику популярність - minikube: >25.8k stars on GitHub
- не залежить від платформи
- має підтримку аддонів від спільноти
- підтримує декілька рантаймів контейнерів
- хороша документація(містить туторіали, які розглядають різні кейси застосування)

Недоліки:

- однонодовий кластер
- повільний - розгортання кластеру займає приблизно 30s
- погано автоматизується
- менш зручний в користуванні, із-за того що не підтримує розгортання кластерів з конфігураційних файлів



### Kind

Переваги:

- має непогану популярність  - kind: >11.1k stars on GitHub
- мультинодовий кластер
- добре автоматизується
- підтримує rootless podman

Недоліки:

- не підтримує аддони
- не дуже добре задокументований



### k3d

Переваги:

- мультинодовий кластер
- добре автоматизується
- швидкий - розгортання кластеру займає 15,576 s
- потребує менше ресурсів
- потребує мало місця на диску(менше ніж 40MiB)
- має дуже хорошу документацію
- добре автоматизується
- підтримує розгортання кластерів з конфігураційних файлів
- простіший в користуванні

Недоліки:

- не підтримує аддони
- менш популярний  - 4.1k stars on GitHub
- не підтримує створення декількох кластерів



#### Порівнювальна таблиця

| Особливості                       |             minikube              |         kind          |         k3d         |
| --------------------------------- | :-------------------------------: | :-------------------: | :-----------------: |
| Сертифікація CNCF                 |                так                |          так          |         так         |
| Підтримувана архітектура          | amd64, arm64, armv7, ppc64, s390x |         amd64         | amd64, armv7, arm64 |
| Підтримувані рантайми контейнерів |    Docker, CRI-O, containerd,     |        Docker         |        CRI-O        |
| Керування нодами                  |                так                |          так          |         так         |
| Однонодовий кластер               |                так                |          так          |         так         |
| Багатонодовий кластер             |                ні                 |          так          |         так         |
| HA                                |                ні                 |          так          |         так         |
| Аддони                            |                так                |          ні           |         ні          |
| Автоматизація                     |                ні                 |          так          |         так         |
| Мінімальний об'єм пам'яті         |                2GB                |          8GB          |        512MB        |
| ОС                                |       Linux, Windows, macOS       | Linux, Windows, macOS |        Linux        |
| Підтримка декількох кластерів     |                так                |          так          |         ні          |

## Висновок

З аналізу було зроблено висновок, що для PoC стартапу "AsciiArtify" краще підійде k3d, так як він простіший у використанні, добре задокументований, потребує мало ресурсів. 

Демонстрація розгортання в кластері імеджу hellow-world
![alt-text](https://github.com/Mardukay/AsciiArtify/blob/main/doc/assets/demo.gif)


