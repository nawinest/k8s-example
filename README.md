# Kubernetes Namespace + ResourceQuota Example

ตัวอย่างนี้สาธิตการใช้งาน Namespace, LimitRange, ResourceQuota และ Deployment + Service ใน Kubernetes

## 📁 ไฟล์ที่อยู่ใน zip

| ไฟล์ | รายละเอียด |
|------|-------------|
| 1-namespace.yaml      | สร้าง Namespace ชื่อ `team-a` |
| 2-limitrange.yaml     | กำหนดค่า request/limit เริ่มต้น และกรอบสูงสุด/ต่ำสุดต่อ container |
| 3-resourcequota.yaml  | กำหนดโควตาการใช้ทรัพยากรภายใน namespace เช่น จำนวน pods, memory, storage ฯลฯ |
| 4-deployment.yaml     | สร้าง nginx deployment 3 replicas ใช้ resource limit |
| 5-service.yaml        | สร้าง ClusterIP service ชื่อ `web-service` |

---

## 🧪 วิธีรันบน Minikube

### 1. เริ่มต้น Minikube
```bash
minikube start
```

### 2. ตรวจสอบ kubectl เชื่อมต่อกับ Minikube
```bash
kubectl config current-context
kubectl cluster-info
```

### 3. แตก zip และเข้าโฟลเดอร์
```bash
unzip k8s-namespace-quota-example.zip
cd k8s-namespace-quota-example
```

### 4. Apply ไฟล์ทั้งหมด
```bash
kubectl apply -f 1-namespace.yaml
kubectl apply -f 2-limitrange.yaml
kubectl apply -f 3-resourcequota.yaml
kubectl apply -f 4-deployment.yaml
kubectl apply -f 5-service.yaml
```

หรือใช้คำสั่งเดียว:
```bash
kubectl apply -f .
```

### 5. ตรวจสอบผลลัพธ์
```bash
kubectl get ns
kubectl get pods -n team-a
kubectl describe resourcequota team-a-quota -n team-a
kubectl describe limitrange container-defaults -n team-a
```

---

## 🧹 ล้างทั้งหมด
```bash
kubectl delete ns team-a
```

---

## ✅ หมายเหตุ
- ใช้ LimitRange เพื่อให้ Pod ที่ไม่ระบุ requests/limits ก็ยังมีค่ามาตรฐาน
- ถ้าเกินโควตา เช่น จำนวน Pods > 20 หรือ Memory > 8Gi จะสร้างไม่ได้

