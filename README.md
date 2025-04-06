# ☁️ Lambda Terminator – AWS EC2 Auto Shutdown

Este projeto contém uma função Lambda desenvolvida para automatizar o encerramento de instâncias EC2 em múltiplas regiões da AWS.

🔧 Desenvolvido pela **Escola da Nuvem®** como parte do curso **AWS Developer Associate**.

## 📋 Descrição

A função `lambda_handler` invoca a função `terminator()`, que percorre uma lista de regiões específicas (EUA e América do Sul) e encerra automaticamente todas as instâncias EC2 em estados **`running`**, **`stopping`** ou **`stopped`**.

Esse recurso é ideal para automatizar o controle de custos em ambientes de desenvolvimento, testes ou laboratórios de treinamento.

## ⚙️ Funcionalidades

- ✅ Percorre múltiplas regiões AWS pré-definidas
- ✅ Identifica instâncias EC2 em execução, parando ou paradas
- ✅ Encerra instâncias de forma automática
- ✅ Utiliza a biblioteca `boto3` para interação com os serviços da AWS
- ✅ Loga no console as ações realizadas (sucesso ou erro)

## 🌎 Regiões Suportadas

A função percorre as seguintes regiões:

- `us-east-1`
- `us-east-2`
- `us-west-1`
- `us-west-2`
- `sa-east-1`

## 📦 Pré-requisitos

- Conta AWS com permissões para listar e encerrar instâncias EC2
- Função Lambda com runtime Python 3.x
- Biblioteca `boto3` disponível no ambiente (no AWS Lambda, já está incluída por padrão)
- Papel IAM associado à Lambda com permissões mínimas necessárias:
  - `ec2:DescribeInstances`
  - `ec2:TerminateInstances`

## 🧪 Estrutura do Código

```python
import boto3

def terminator():
    # Lista de regiões suportadas
    regions = ['us-east-1', 'us-east-2', 'us-west-1', 'us-west-2', 'sa-east-1']

    for region in regions:
        ec2 = boto3.resource('ec2', region_name=region)
        instances = ec2.instances.filter(Filters=[{'Name': 'instance-state-name', 'Values': ['running', 'stopped', 'stopping']}])

        for instance in instances:
            try:
                instance.terminate()
                print(f'{instance.id} terminated')
            except:
                print(f'Erro ao terminar {instance.id}')

def lambda_handler(event, context):
    terminator()
```

## 🚀 Como usar

1. Acesse o console da AWS.
2. Crie uma nova função Lambda com o runtime Python 3.x.
3. Copie o código acima para o editor da Lambda.
4. Ajuste as permissões IAM conforme necessário.
5. Configure um **gatilho** (ex: agendamento via EventBridge) se quiser que a função rode automaticamente.
6. Salve e execute.

## 📬 Contato

Feito com 💡 como parte da formação pela **Escola da Nuvem®**.  
Testado e aprovado por Thiago Piassi  
[LinkedIn](https://www.linkedin.com/in/thiagopiassi)
