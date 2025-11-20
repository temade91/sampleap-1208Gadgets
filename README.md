# Build and Deploy ASP.NET Core App to Azure Web App

![Build and Deploy](https://github.com/<YOUR_USERNAME>/<YOUR_REPO>/actions/workflows/deploy.yml/badge.svg)  

This repository contains a GitHub Actions workflow to **build** and **deploy** an ASP.NET Core application to an **Azure Web App**.

---

## Workflow Overview

The workflow `Build and deploy ASP.Net Core app to Azure Web App - test-web-app` is triggered by:

- **Pushes to the `main` branch**
- **Manual workflow dispatch** via the GitHub Actions UI

It consists of two main jobs:

1. **Build**: Compiles and publishes the ASP.NET Core application.
2. **Deploy**: Deploys the published application to the specified Azure Web App.

---

## Workflow Jobs

### 1. Build Job

**Runs on:** `windows-latest`  
**Steps:**

- **Checkout Repository**: Uses `actions/checkout@v4` to clone the repo.
- **Set up .NET Core**: Uses `actions/setup-dotnet@v4` with `.NET 9.x`.
- **Build Project**: Runs `dotnet build --configuration Release`.
- **Publish Project**: Runs `dotnet publish -c Release -o "${{env.DOTNET_ROOT}}/myapp"`.
- **Upload Artifact**: Uses `actions/upload-artifact@v4` to upload the published application for deployment.

### 2. Deploy Job

**Runs on:** `windows-latest`  
**Dependencies:** `build` job

**Steps:**

- **Download Artifact**: Uses `actions/download-artifact@v4` to fetch the published app from the build job.
- **Deploy to Azure Web App**: Uses `azure/webapps-deploy@v3` with:
  - `app-name`: `test-web-app`
  - `publish-profile`: `${{ secrets.AzureAppService_PublishProfile_1234 }}`
  - `package`: Current directory containing the published app

---

## Prerequisites

1. **Azure Web App**  
   Ensure an Azure Web App exists with a configured **Publish Profile**.

2. **GitHub Secret**  
   Store your Azure Web App publish profile in your repository secrets, e.g., `AzureAppService_PublishProfile_1234`.

3. **.NET 9.x**  
   Ensure your project is compatible with .NET 9.x.

---

## References

- [Azure Web Apps Deploy GitHub Action](https://github.com/Azure/webapps-deploy)  
- [Other GitHub Actions for Azure](https://github.com/Azure/actions)

---

## Notes

- The workflow uses **Windows runners** because ASP.NET Core projects often require Windows-specific dependencies.  
- The published output is stored as an **artifact** to decouple build and deploy jobs.  

---

✅ **Next Step:** Replace `temade91` and `-1208sampleapp` in the badge URL with your GitHub username and repository name to make the badge work correctly.
