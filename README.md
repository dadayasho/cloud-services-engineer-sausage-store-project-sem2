# Sausage Store

![image](https://user-images.githubusercontent.com/9394918/121517767-69db8a80-c9f8-11eb-835a-e98ca07fd995.png)


## Technologies used

* Frontend – TypeScript, Angular.
* Backend  – Java 16, Spring Boot, Spring Data.
* Database – H2.

## Installation guide
### Backend

Install Java 16 and maven and run:

```bash
cd backend
mvn package
cd target
java -jar sausage-store-0.0.1-SNAPSHOT.jar
```

### Frontend

Install NodeJS and npm on your computer and run:

```bash
cd frontend
npm install
npm run build
npm install -g http-server
sudo http-server ./dist/frontend/ -p 80 --proxy http://localhost:8080
```

Then open your browser and go to [http://localhost](http://localhost)


---

Вот мой nexus репозиторий - `https://nexus.cloud-services-engineer.education-services.ru/#browse/browse:dadayasho-sausage-store-helm`
https://front-dadayasho.2sem.students-projects.ru

Вообщем, все сделано

Докерфайлы корректны, чарт собирается пушится в nexus, раскатывается.

VPA работает
```bash
Name:         sausage-store-backend-vpa
Namespace:    r-devops-magistracy-project-2sem-1571688726
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  meta.helm.sh/release-name: sausage-store
              meta.helm.sh/release-namespace: r-devops-magistracy-project-2sem-1571688726
API Version:  autoscaling.k8s.io/v1
Kind:         VerticalPodAutoscaler
Metadata:
  Creation Timestamp:  2026-05-30T10:42:42Z
  Generation:          1
  Resource Version:    289108270
  UID:                 8bca9dfa-1a47-45fe-a23f-46acab61dede
Spec:
  Resource Policy:
    Container Policies:
      Container Name:  *
      Controlled Resources:
        cpu
        memory
      Max Allowed:
        Cpu:     200m
        Memory:  256Mi
      Min Allowed:
        Cpu:     0m
        Memory:  0Mi
      Mode:      Auto
  Target Ref:
    API Version:  apps/v1
    Kind:         Deployment
    Name:         sausage-store-backend
  Update Policy:
    Update Mode:  Off
Status:
  Conditions:
    Last Transition Time:  2026-05-30T10:43:13Z
    Status:                True
    Type:                  RecommendationProvided
  Recommendation:
    Container Recommendations:
      Container Name:  backend
      Lower Bound:
        Cpu:     25m
        Memory:  256Mi
      Target:
        Cpu:     25m
        Memory:  256Mi
      Uncapped Target:
        Cpu:     25m
        Memory:  272061154
      Upper Bound:
        Cpu:     119m
        Memory:  256Mi
Events:          <none>
```

HPA работает
```bash
NAME                               REFERENCE                                 TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
sausage-store-backend-report-hpa   Deployment/sausage-store-backend-report   cpu: 1%/80%   1         3         1          54m
```
Probes то же.

Миграции сделаны - ![Миграции](backend/src/main/resources/db/migration)

Заказы создаются.

Так же реализовал свой удаленный vault, правда с сертификатами не смог разобраться, так как spring жалорвался на самоподписанные сертификаты. Пришлось делать через http.

Все секреты запичкал в secrets.

```bash
maxim@ubuntu:~/cloud-services-engineer-sausage-store-project-sem2$ kubectl get pod
NAME                                            READY   STATUS    RESTARTS      AGE
mongodb-0                                       1/1     Running   0             69m
postgresql-0                                    1/1     Running   0             69m
sausage-store-backend-78f7ff6656-sh485          1/1     Running   0             69m
sausage-store-backend-report-5dbbc7b9d4-4kc7m   1/1     Running   4 (68m ago)   69m
sausage-store-frontend-7c48874865-77sgd         1/1     Running   0             69m
```


