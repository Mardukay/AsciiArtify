# Встановлення argocd

1. ### Встановлення кластеру kubernetes

   Розгортати кластер будемо за допомогою k3d

   Створюємо конфігураційний файл з описом кластеру:

   ```yaml
   apiVersion: k3d.io/v1alpha5
   kind: Simple
   metadata:
     name: argo.cluster
   servers: 1
   agents: 2
   ports:
     - port: 8081:80
       nodeFilters:
         - loadbalancer
   ```

   Створюємо кластер з конфігу:

   ```bash
   k3d cluster create --config argo-cluster.yaml
   ```

   Створюємо неймспейс для argocd:

   ```bash
   kubectl create namespace argocd
   ```

   Встановлюємо argocd з офіційного маніфесту:

   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

   Дивимось які є сервіси argocd:

   ```bash
   kubectl get services --namespace argocd
   ```

   У виводі маємо отримати наступне:

   ```bash
   mardukay@mardukay-pc:/mnt/d/DevOPS_tutor/Learning-notes/DevOPS/AsciiArtify$ kubectl get services --namespace argocd
   NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
   argocd-applicationset-controller          ClusterIP   10.43.185.142   <none>        7000/TCP,8080/TCP            36m
   argocd-dex-server                         ClusterIP   10.43.65.30     <none>        5556/TCP,5557/TCP,5558/TCP   36m
   argocd-metrics                            ClusterIP   10.43.209.229   <none>        8082/TCP                     36m
   argocd-notifications-controller-metrics   ClusterIP   10.43.236.142   <none>        9001/TCP                     36m
   argocd-redis                              ClusterIP   10.43.25.94     <none>        6379/TCP                     36m
   argocd-repo-server                        ClusterIP   10.43.13.198    <none>        8081/TCP,8084/TCP            36m
   argocd-server                             ClusterIP   10.43.65.167    <none>        80/TCP,443/TCP               36m
   argocd-server-metrics                     ClusterIP   10.43.42.30     <none>        8083/TCP                     36m
   ```

   Для доступу до web UI потрібно зробити форвардінг портів на argocd-server:

   ```bash
   kubectl port-forward -n argocd svc/argocd-server 9000:80
   ```

   Якщо ми закриємо термінал, то сервер перестане працювати і тому можна запустити процес в бекграунді:

   ```bash
   kubectl port-forward -n argocd svc/argocd-server 9000:80 &
   ```

   Для того, щоб закрити процес, який працю в бекграунді треба подивитись його PID:

   ```bash
   ps -ef | grep port-forward
   ```

   А потім знищити процес:

   ```bash
   kill -9 [PID]
   ```

   Далі переходимо в UI argocd https://127.0.0.1:9000

   ![Екран логіну ArgoCD](https://github.com/Mardukay/AsciiArtify/blob/main/doc/assets/argocd_login.jpg)

   Дефолтний пароль для входу - admin, а логін можна дізнатись з secret argocd:

   ```bash
   kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
   ```

   У виводі отримаємо наступне:

   ```bash
   mardukay@mardukay-pc:/mnt/d/DevOPS_tutor/Learning-notes/DevOPS/AsciiArtify$ kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
   apiVersion: v1
   data:
     password: MFplbDl1VVBKaUt6TEx0OQ==
   kind: Secret
   metadata:
     creationTimestamp: "2023-11-29T11:34:46Z"
     name: argocd-initial-admin-secret
     namespace: argocd
     resourceVersion: "1198"
     uid: d3f03851-c6db-4c9c-9d35-4af4b14ae8ab
   type: Opaque
   ```

   Пароль зашифрований в base64, для розшифрування робимо наступне:

   ```bash
   echo MFplbDl1VVBKaUt6TEx0OQ== | base64 --decode
   ```

   У виводі отримаємо пароль за яким входимо в ArgoCD

   ![argocd](https://github.com/Mardukay/AsciiArtify/blob/main/doc/assets/argocd.jpg)
