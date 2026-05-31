# Лабораторная работа: Запуск микросервисного приложения в Kubernetes 

## Цель

Цель работы
Развернуть микросервисное приложение мессенджера в Kubernetes, настроить GitOps-деплой через Argo CD, S3 CSI хранилище и kustomize конфигурации для dev/prod окружений.

## Запуск
1. k3d cluster create labk8smessenger
2. kubectl apply -k k8s/overlays/dev
3. kubectl apply -f argocd/application.yaml

## Архитектура
- frontend → bff → user-service/message-service → postgres
- message-service → S3 (MinIO) через CSI

## Итог работы

Изучен полный цикл GitOps в Kubernetes: от управления конфигурациями и хранилищами до автоматизации доставки через Argo CD
```
📁 argocd
└── 📄 application.yaml
```

```
📁 bff
├── 📁 cmd
│   └── 📁 bff
│       └── 📄 main.go
├── 📁 internal
│   ├── 📁 config
│   │   └── 📄 config.go
│   ├── 📁 controller
│   │   └── 📁 http
│   │       └── 📄 handler.go
│   └── 📁 external
│       ├── 📁 messageservice
│       │   └── 📄 client.go
│       └── 📁 userservice
│           └── 📄 client.go
├── 📄 .env.example
├── 📄 Dockerfile
├── 📄 go.mod
├── 📄 go.sum
└── 📄 Makefile
```

```
📁 docs
├── 📄 01-architecture-and-resources.md
├── 📄 02-k8s-manifests-examples.md
├── 📄 03-s3-csi.md
├── 📄 04-node-affinity-task.md
├── 📄 05-kustomize-task.md
├── 📄 06-argocd-task.md
└── 📄 07-checklist-and-defense.md
```

```
📁 frontend
├── 📄 app.js
├── 📄 docker-entrypoint.sh
├── 📄 Dockerfile
├── 📄 index.html
└── 📄 nginx.conf
```

```
📁 k8s
├── 📁 base
│   ├── 📄 bff.yaml
│   ├── 📄 configmap.yaml
│   ├── 📄 frontend.yaml
│   ├── 📄 init-db-configmap.yaml
│   ├── 📄 kustomization.yaml
│   ├── 📄 message-service.yaml
│   ├── 📄 messages-migrations-configmap.yaml
│   ├── 📄 migrate-messages-job.yaml
│   ├── 📄 migrate-users-job.yaml
│   ├── 📄 minio.yaml
│   ├── 📄 namespace.yaml
│   ├── 📄 postgres.yaml
│   ├── 📄 s3-pv-pvc.yaml
│   ├── 📄 secret.yaml
│   ├── 📄 user-service.yaml
│   └── 📄 users-migrations-configmap.yaml
└── 📁 overlays
    ├── 📁 dev
    │   └── 📄 kustomization.yaml
    └── 📁 prod
        ├── 📁 patches
        │   ├── 📄 affinity.yaml
        │   └── 📄 replicas.yaml
        └── 📄 kustomization.yaml
```

```
📁 k8s/base
├── 📄 bff.yaml
├── 📄 configmap.yaml
├── 📄 frontend.yaml
├── 📄 init-db-configmap.yaml
├── 📄 kustomization.yaml
├── 📄 message-service.yaml
├── 📄 messages-migrations-configmap.yaml
├── 📄 migrate-messages-job.yaml
├── 📄 migrate-users-job.yaml
├── 📄 minio.yaml
├── 📄 namespace.yaml
├── 📄 postgres.yaml
├── 📄 s3-pv-pvc.yaml
├── 📄 secret.yaml
├── 📄 user-service.yaml
└── 📄 users-migrations-configmap.yaml
```

```
📁 k8s/overlays
├── 📁 dev
│   └── 📄 kustomization.yaml
└── 📁 prod
    ├── 📁 patches
    │   ├── 📄 affinity.yaml
    │   └── 📄 replicas.yaml
    └── 📄 kustomization.yaml
```

```
📁 k8s/overlays/dev
└── 📄 kustomization.yaml
```

```
📁 k8s/overlays/prod
└── 📄 kustomization.yaml
```

```
📁 k8s/overlays/prod/patches
├── 📄 affinity.yaml
└── 📄 replicas.yaml
```

```
📁 message-service
├── 📄 .env.example
├── 📄 Dockerfile
├── 📄 go.mod
├── 📄 go.sum
└── 📄 Makefile
```

```
📁 message-service/cmd
└── 📁 message-service
    └── 📄 main.go
```

```
📁 message-service/internal
├── 📁 config
│   └── 📄 config.go
├── 📁 controller
│   └── 📁 http
│       ├── 📄 handler.go
│       └── 📄 response.go
├── 📁 domain
│   ├── 📄 errors.go
│   └── 📄 message.go
├── 📁 repository
│   ├── 📁 files
│   │   └── 📄 repository.go
│   └── 📁 messages
│       └── 📄 repository.go
└── 📁 usecase
    ├── 📁 deletemessage
    │   └── 📄 usecase.go
    ├── 📁 editmessage
    │   └── 📄 usecase.go
    ├── 📁 getconversations
    │   └── 📄 usecase.go
    ├── 📁 getmessages
    │   └── 📄 usecase.go
    ├── 📁 sendmessage
    │   └── 📄 usecase.go
    └── 📁 uploadfile
        └── 📄 usecase.go
```

```
📁 message-service/migrations
├── 📄 001_init.sql
└── 📄 002_add_file_name.sql
```

```
📁 user-service
├── 📄 .env.example
├── 📄 Dockerfile
├── 📄 go.mod
├── 📄 go.sum
└── 📄 Makefile
```

```
📁 user-service/cmd/user-service
└── 📄 main.go
```

```
📁 user-service/internal
├── 📁 config
│   └── 📄 config.go
├── 📁 controller
│   └── 📁 http
│       ├── 📄 contracts.go
│       ├── 📄 handler.go
│       └── 📄 response.go
├── 📁 domain
│   ├── 📄 errors.go
│   └── 📄 user.go
├── 📁 repository
│   └── 📁 users
│       └── 📄 repository.go
└── 📁 usecase
    ├── 📁 getuser
    │   └── 📄 usecase.go
    ├── 📁 register
    │   └── 📄 usecase.go
    └── 📁 search
        └── 📄 usecase.go
```

```
📁 user-service/migrations
└── 📄 001_init.sql
```

```
📁 /
├── 📄 .gitignore
├── 📄 docker-compose.hub.yml
├── 📄 docker-compose.yml
├── 📄 init-db.sh
└── 📄 README.md
```

## NodeAffinity:
postgres/minio → workload=system
остальные → workload=app
message-service → hard: workload=app + soft: disk=fast

