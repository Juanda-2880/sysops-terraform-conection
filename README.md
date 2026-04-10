#  AWS SysOps Sandbox + Terraform

Guía para conectar el sandbox de AWS SysOps Academy con Terraform y desplegar tu primer recurso.

---

##  Prerrequisitos

Asegúrate de tener instalado lo siguiente antes de comenzar:

- [Terraform](https://developer.hashicorp.com/terraform/install) `>= 1.0`
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) `>= 2.0`

Verifica la instalación:

```bash
terraform -version
aws --version
```

---

##  1. Obtener credenciales del Sandbox

1. Inicia sesión en [AWS Academy](https://awsacademy.instructure.com) o la plataforma de tu institución.
2. Abre el módulo del curso → **Sandbox**.
3. Haz clic en **"Start Lab"** y espera a que el indicador se ponga en verde ✅.
4. Haz clic en **"AWS Details"** → **"Show"** junto a *AWS CLI*.
5. Copia los tres valores que aparecen:

```
aws_access_key_id     = ASIA...
aws_secret_access_key = xxxx...
aws_session_token     = IQoJ...
```

>  **Importante:** Las credenciales del sandbox expiran cada ~120 minutos. Debes repetir este paso en cada sesión.

---

##  2. Estructura de las carpetas

Crea la siguiente estructura de archivos:

```
terraform-sandbox/
├── providers.tf
├── variables.tf
├── main.tf
└── outputs.tf
```

---

##  4. Archivos de configuración

### `providers.tf`

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

### `variables.tf`

```hcl
variable "aws_region" {
  description = "Región de AWS"
  type        = string
  default     = "us-east-1"
}
```

### `main.tf` *(ejemplo: crear una VPC)*

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "sandbox-vpc"
  }
}
```

### `outputs.tf`

```hcl
output "vpc_id" {
  description = "ID de la VPC creada"
  value       = aws_vpc.main.id
}
```

---

##  5. Inicializar y desplegar

Ejecuta los siguientes comandos en orden dentro de la carpeta `terraform-sandbox/`:

```bash
# 1. Descarga el provider de AWS
terraform init

# 2. Previsualiza los cambios antes de aplicar
terraform plan

# 3. Aplica la infraestructura (confirma con "yes" cuando se solicite)
terraform apply
```

Para destruir los recursos al finalizar:

```bash
terraform destroy
```

---

##  Consideraciones del Sandbox

| Aspecto | Detalle |
|---|---|
| **Expiración de credenciales** | Se renuevan cada ~120 minutos; repite el paso 1 en cada sesión |
| **Región disponible** | El sandbox suele estar limitado a `us-east-1` |


---

##  `.gitignore` recomendado

```
.terraform/
.terraform.lock.hcl
terraform.tfstate
terraform.tfstate.backup
*.tfvars
```
