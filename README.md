# ☁️ Securing Kubernetes Deployments in Azure (AKS)

## 📌 Scenario
As an IT administrator, you are responsible for securing a production Kubernetes cluster hosted on **Azure Kubernetes Service (AKS)**.

The environment is already deployed but lacks proper security controls. Your goal is to:
- Review AKS configuration
- Deploy a multi-service application
- Identify security risks
- Implement container security best practices
- Validate remediation

---

## 🎯 Objectives
- Review AKS deployment configuration
- Deploy application using Kubernetes YAML manifest
- Identify insecure container behavior
- Apply security hardening (least privilege)
- Validate security improvements

---

## 🖥️ Lab Environment
- Azure Kubernetes Service (AKS)
- Windows 11 VM
- Azure Portal
- Azure Cloud Shell (PowerShell)
- Visual Studio Code

---

# 🔍 TASK A: Review AKS Deployment

## 🔐 Login Steps
- Sign in to Windows VM
- Open Azure Portal: https://portal.azure.com
- Navigate to:
  - **Resource Groups → RG1 → CompTIA-AKS**

---

---

## 🔎 Node Pool Review
- Navigate to **Node Pools → agentpool**
- Verify:
  - Node Count: 2
  - OS: Ubuntu

---



---

## ✅ Answer
**Node Size:**  
```bash
Standard_D2ds_v5
🚀 TASK B: Deploy Application (Kubernetes)
📂 Step 1: Open YAML File
C:\LabFiles\aks-store-quickstart.yaml
🖼️ Screenshot: YAML File

☁️ Step 2: Upload to Cloud Shell
Open Azure Cloud Shell
Upload YAML file
⚙️ Step 3: Deploy Application
az aks get-credentials --name CompTIA-AKS --resource-group RG1

kubectl apply -f aks-store-quickstart.yaml
🖼️ Screenshot: Deployment Output

🔍 Step 4: Verify Pods
kubectl get pods
🌐 Step 5: Get External IP
kubectl get service store-front
🖼️ Screenshot: External IP

🌍 Access Application
http://<EXTERNAL-IP>
✅ Answers
🔹 Order-Service Port
3000
🔹 Service Type
LoadBalancer
🔐 TASK C: Enhance Container Security
⚠️ Step 1: Identify Security Issue

Access container:

kubectl exec -it <order-pod-name> -- /bin/sh

Run:

chown root:root /etc/modules
su -

❌ Both commands work → Container running as ROOT (INSECURE)


🛡️ Step 2: Apply Security Context

Update YAML:

securityContext:
  runAsUser: 1000
  runAsGroup: 3000
  allowPrivilegeEscalation: false
💾 Save New File
aks-store-quickstart-secure.yaml
☁️ Step 3: Deploy Secure Version
kubectl apply -f aks-store-quickstart-secure.yaml


🔍 Step 4: Validate Security
kubectl exec -it <new-pod-name> -- /bin/sh

Test again:

chown root:root /etc/modules   ❌ FAIL
su -                           ❌ FAIL


✅ Result
Privilege escalation blocked
Root access restricted
Container hardened
🧠 Key Security Concepts
🔒 Least Privilege

Containers should NOT run as root unless required

🚫 Privilege Escalation Prevention
allowPrivilegeEscalation: false
👤 Non-root Execution
runAsUser: 1000
