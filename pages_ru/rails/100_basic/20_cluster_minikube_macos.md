### Установка и запуск minikube

Установим minikube, [следуя инструкциям здесь](https://minikube.sigs.k8s.io/docs/start/).

Порт 80 должен быть свободен.

Создаём новый Kubernetes-кластер с minikube:
```bash
minikube delete  # Удалим существующий minikube-кластер (если он есть).
minikube start --driver=hyperkit --namespace werf-guided-rails
```

Если утилита kubectl всё ещё не установлена, то установим её, [следуя инструкциям](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/).

Теперь проверим работоспособность нового кластера Kubernetes:
```bash
kubectl get --all-namespaces pod  # Должно показать список всех запущенных в кластере Pod'ов.
```

Если все Pod'ы из полученного списка находятся в состояниях `Running` или `Completed` (4-й столбец), а в 3-м столбце в выражениях вроде `1/1` цифра слева от `/` равна цифре справа (т.е. контейнеры Pod'а успешно запустились) — значит кластер Kubernetes запущен и работает. Если не все Pod'ы успешно запустились, то подождите и снова выполните команду выше для получения статуса всех Pod'ов.

### Установка NGINX Ingress Controller

Устанавливаем NGINX Ingress Controller:
```bash
minikube addons enable ingress
```

Немного подождём, после чего убедимся, что Ingress Controller успешно запустился:
```bash
kubectl -n ingress-nginx get pod
```

### Обновление файла hosts

Для доступа к приложению мы будем использовать домен `example.com`, для этого обновим файл hosts:
```bash
echo "$(minikube ip) example.com kubernetes-basics-app.example.com" | sudo tee -a /etc/hosts
```