# ASP.NET Core MVC App Deployment via Terraform

## Description

This project is part of an exercise for the Exam Prep from the **Containers and Cloud** course at **SoftUni (February 2025)**.  
The task is to **deploy an ASP.NET Core MVC application** with a SQL Server backend to **Azure**, using **Terraform** for Infrastructure as Code (IaC).

The application is composed of two projects:
- A web application built with **ASP.NET Core MVC**
- A **SQL Server** database project

## Deployment Goal

Use **four Terraform configuration files** to deploy the infrastructure and the app:
- `main.tf` – Main configuration for Azure resources  
- `variables.tf` – Input variables  
- `values.tfvars` – Values assigned to the variables. Contains sensitive values for those variables (not included in repo)  
- `outputs.tf` – Outputs after deployment

---

## Technologies Used

- **ASP.NET Core MVC** (.NET)  
- **SQL Server**  
- **Azure App Service**  
- **Azure SQL Database**  
- **Terraform**  
- **Azure Resource Group & Storage**

---

## How to Deploy

1. **Login to Azure CLI:**
   ```bash
   az login
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Plan deployment:**
   ```bash
   terraform plan -var-file="values.tfvars"
   ```

4. **Apply configuration:**
   ```bash
   terraform apply -var-file="values.tfvars"
   ```

---

## Cleanup

To destroy the created Azure resources:

```bash
terraform destroy -var-file="values.tfvars"
```

---

## Outputs

After deployment, Terraform will output:
- The **App Service URL**
- The **SQL Server name**
- The **Resource Group**

---

## Security Notice and Usage Instructions

I have **excluded the `values.tfvars` file** from this repository on purpose, because it contains **sensitive data** such as:

- SQL Server administrator username and password  
- Azure resource names unique to my setup  

If you want to use or fork this repository, you’ll need to create your own `values.tfvars` file in the root of the project directory with content like:

```hcl
subscription_id             = "your-subscription-id"
resource_group_name         = "your-resource-group"
resource_group_location     = "your-resource-group-location"
app_service_plan_name       = "your-app-service-plan"
app_service_name            = "your-app-service"
sql_server_name             = "your-sql-server"
sql_database_name           = "your-database"
sql_admin_username          = "your-admin"
sql_admin_password          = "your-strong-password"
firewall_rule_name          = "your-firewall-rule"
github_repo_url             = "https://github.com/your-username/your-repo"
```

---

## App Monitoring with Prometheus, AlertManager & Grafana

As part of the exercise, **monitoring was set up** for the deployed ASP.NET Core MVC app using:

- **Prometheus** with **Blackbox Exporter** – scrapes the app every 15s via `/probe`
- **AlertManager** – sends alerts to a webhook receiver with a 1-minute timeout
- **Grafana Dashboard** – visualizes the HTTP probe duration as a histogram

Configuration files used:
- `prometheus-exam-prep-1.yml`
- `alertmanager-exam-prep-1.yml`
- `alert-rules-axam-prep-1.yml`

A custom Grafana dashboard was created and exported as a JSON file.