# Uploading modules to the terraform registry

1. Create an empty directory and inside create an empty main.tf file.
   
2. In the directory add a README.md to serve as the module description. For the purposes of this lab you can write "Test module description." and save.

3. Copy the following code into your empty main.tf file and save.

```
# Variables
variable "name" {
  type    = string
  default = "deployment"
}

variable "namespace" {
  type    = string
  default = "ns"
}

# Provider
provider "kubernetes" {
  config_context_cluster = "minikube"
  config_path            = "~/.kube/config"
}

# Namespace
resource "kubernetes_namespace" "namespaces" {
  metadata {
    name = var.namespace
  }
}

# Deployment
resource "kubernetes_deployment" "nginx" {
  metadata {
    name      = var.name
    namespace = var.namespace
    labels = {
      App = var.name
    }
  }

  spec {
    replicas = 1
    selector {
      match_labels = {
        App = var.name
      }
    }
    template {
      metadata {
        labels = {
          App = var.name
        }
      }
      spec {
        container {
          image = "nginx:1.7.8"
          name  = "example"

          port {
            container_port = 80
          }

          resources {
            limits = {
              cpu    = "0.5"
              memory = "512Mi"
            }
            requests = {
              cpu    = "250m"
              memory = "50Mi"
            }
          }
        }
      }
    }
  }
}

#  Service
resource "kubernetes_service" "nginx-svc" {
  metadata {
    name      = "nginx-example"
    namespace = var.namespace
  }
  spec {
    selector = {
      App = var.name
    }
    port {
      port        = 80
      target_port = 80
    }
    type = "NodePort"
  }
}
```

4. Login into github and create a new repository and name it according to the terraform naming convention, i.e: terraform-PROVIDER-NAME 
   (example: terraform-aws-ec2-instance).

5. Follow the instructions in your newly created git repository to upload your module files to the repository.

6. After the repository contains your terraform file and README, navigate to the Releases section on the right side of the repository page and click "Create new release".

7. Click "Choose a tag" and enter whatever release tag you want. More information about tagging releases can be found on the right side of the page. We will choose v1.0.0 for this lab. and click "Publish release"

8. After our release is created you can navigate to the terraform repository and register/login to your account.

9. Under "Publish" select module and in the selection screen select your newly created repository, agree to the terms of service and click Publish.

10. You should now see your module as published on the terraform registry with instructions on how to use the module on the right side of the module page. Done!