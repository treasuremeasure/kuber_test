Общее описание

Цель проекта — развернуть простое FastAPI-приложение в Kubernetes и подключить систему мониторинга с использованием Prometheus и Grafana.

Алгоритм выполнения

Часть 1 — Развёртывание сервиса

1. Запустил Kubernetes-кластер через Docker Desktop

2. Написал простое FastAPI-приложение (/app/main.py)

3. Создал файл requirements.txt с зависимостями приложения

4. Написал Dockerfile (/app/Dockerfile)

5. Собрал образ Docker командой docker build -t treasuremeasure/my-app:latest .

6. Загрузил образ на Docker Hub командой docker push treasuremeasure/my-app:latest

7. Создал deployment.yaml для запуска пода (/kub/deployment.yaml)

8. Применил deployment.yaml командой kubectl apply -f deployment.yaml

9. Проверил работу поды командой kubectl get pods

10. Создал service.yaml для маршрутизации внутренних запросов на поду (/kub/service.yaml)

11. Применил service.yaml командой kubectl apply -f service.yaml

12. Создал ingress.yaml для маршрутизации внешних запросов (/kub/ingress.yaml)

13. Применил ingress командой kubectl apply -f ingress.yaml

14. Проверил доступ к приложению через браузер по localhost

Часть 2 — Мониторинг

1. Добавил в requirements.txt зависимость prometheus_fastapi_instrumentator

2. Добавил код для сбора метрик в main.py

3. Проверил отдаются ли метрики по адресу localhost:8000/metrics

4. Пересобрал Docker-образ командами: docker build -t treasuremeasure/my-app:latest . и docker push treasuremeasure/my-app:latest

5. Обновил поду командой kubectl rollout restart deployment myapp-deployment

6. Установил Helm командой choco install kubernetes-helm

7. Добавил repo Prometheus командами helm repo add prometheus-community https://prometheus-community.github.io/helm-charts и helm repo update

8. Установил Prometheus командой helm install prometheus prometheus-community/kube-prometheus-stack

9. Создал ingress для Grafana для доступа извне (/kub/grafana-ingress.yaml)

10. Написал ServiceMonitor для подключения метрик от приложения (/kub/service-monitor.yaml)

11. Убедился, что таргет myapp-servicemonitor появился в Prometheus (/targets)

12. Создал dashboard в Grafana

    ![image](https://github.com/user-attachments/assets/afde428f-b2ae-4531-a381-ad9d6649038b)



Возможные улучшения:

- Добавить alerts в Prometheus

- Вынести конфиги в Helm chart

- Добавить стратегию отката в Deployment (kubectl rollout undo)

- CI/CD для пуша образов и apply манифестов
