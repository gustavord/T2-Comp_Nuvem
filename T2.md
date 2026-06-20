# Trabalho 2 - Computação em Nuvem

## Integrantes:

- Gustavo André Simon
- Gustavo Geyer
- Pedro Kassick Soares

---

1. Criar repositório ECR

```bash
aws ecr create-repository \
  --repository-name juice-shop \
  --region us-east-1
```

1. Anotar repositoryUri
2. Autenticar Docker no ECR

```bash
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin \
  099519111553.dkr.ecr.us-east-1.amazonaws.com
```

1. Pull da imagem

```bash
# Pull da imagem oficial
docker pull bkimminich/juice-shop:latest

# Tagueamento para o ECR
docker tag bkimminich/juice-shop:latest \
  099519111553.dkr.ecr.us-east-1.amazonaws.com/juice-shop:latest

# Push para o ECR
docker push \
  099519111553.dkr.ecr.us-east-1.amazonaws.com/juice-shop:latest
```

1. Criar arquivo de stack (disponível no repositório)
2. Deploy

`export ECR_URI="099519111553.dkr.ecr.us-east-1.amazonaws.com/juice-shop:latest"`

```bash
aws cloudformation deploy \
  --template-file juice-shop-stack.yaml \
  --stack-name juice-shop-poc \
  --parameter-overrides ECRImageURI=$ECR_URI \
  --region us-east-1
```

1. Cleanup

```bash
aws cloudformation delete-stack \
  --stack-name juice-shop-poc \
  --region us-east-1

# Remover imagem do ECR
aws ecr delete-repository \
  --repository-name juice-shop \
  --force \
  --region us-east-1
```
