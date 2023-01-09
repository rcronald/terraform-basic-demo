# Creando Cluster de AKS con Terraform

## Pre-requisitos

* Azure Account
* Azure CLI (curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash)
* Terraform CLI (https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

## Obtener credenciales de Azure

Sobre el archivo terraform.tfvars insertar los valores de las credenciales de la cuenta de Azure.

Para crear una credencial ejecutar:

```console
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                            --sdk-auth
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/130071de-7766-4767-84bc-14d7ca9a654a \
                            --sdk-auth
```
Reemplazar {subscription-id}, {resource-group} con el identificador de subscripción y nombre del resorce group.

Más detalles: https://github.com/Azure/login


## Inicializar Terraform

```
terraform init
```

## Crear plan de ejecución

```
terraform plan -out main.tfplan
```

## Ejecutar plan de terraform

```
terraform apply main.tfplan
```

## Validar ejecución

```
echo "$(terraform output resource_group_name)"
```

```
echo "$(terraform output kube_config)" > ./azurek8s
```

```
cat ./azurek8s
```

```
export KUBECONFIG=./azurek8s
```

```
kubectl get nodes
```

## Eliminar plan

Para eliminar ejecutar el siguiente comando:
```
terraform plan -destroy -out main.destroy.tfplan
```

