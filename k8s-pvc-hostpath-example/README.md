# PVC + hostPath Example (Minikube)

ตัวอย่างนี้ใช้ PersistentVolume (PV) แบบ `hostPath` ซึ่งจะเก็บข้อมูลไว้บน Node ของ Minikube ที่ path `/mnt/data`

## 🔧 ขั้นตอน

### 1. เริ่ม Minikube
```bash
minikube start
```

### 2. แตก zip แล้วเข้าโฟลเดอร์
```bash
unzip k8s-pvc-hostpath-example.zip
cd k8s-pvc-hostpath-example
```

### 3. Apply ทั้งหมด
```bash
kubectl apply -f .
```

### 4. ตรวจสอบ
```bash
kubectl get pv
kubectl get pvc
kubectl get pod nginx-pv
```

### 5. เขียนไฟล์ใน container
```bash
kubectl exec -it nginx-pv -- /bin/sh
echo "hello from PVC" > /usr/share/nginx/html/index.html
exit
```

### 6. ดูไฟล์จาก Minikube
```bash
minikube ssh
cat /mnt/data/index.html
```

---

## 🧹 ลบทั้งหมด
```bash
kubectl delete -f .
```

