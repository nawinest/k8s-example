ตัวอย่าง Deployment ใช้ PVC หลายตัวบน AKS
ตัวอย่างนี้สอนการใช้ PersistentVolumeClaim (PVC) สองตัวกับ Deployment ใน Kubernetes บน AKS โดยใช้ Azure Disk ผ่าน dynamic provisioning

เนื้อหาไฟล์ในโปรเจกต์นี้
pvc.yaml — สร้าง PVC 2 ตัว (config และ logs)

deployment.yaml — Deployment ที่สร้าง Pod 2 ตัว ใช้ PVC ทั้งสองตัว

ขั้นตอนใช้งาน
1. เตรียม AKS Cluster (ถ้ายังไม่มี)
   bash
   Copy
   Edit
   az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
   az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
2. ตรวจสอบ StorageClass ใน AKS
   bash
   Copy
   Edit
   kubectl get storageclass
   ให้มั่นใจว่ามี StorageClass แบบ dynamic provisioning เช่น managed-csi

3. สร้าง PersistentVolumeClaims (PVC)
   bash
   Copy
   Edit
   kubectl apply -f pvc.yaml
4. สร้าง Deployment
   bash
   Copy
   Edit
   kubectl apply -f deployment.yaml
5. ตรวจสอบสถานะ
   bash
   Copy
   Edit
   kubectl get pvc
   kubectl get deployment
   kubectl get pods
   kubectl describe pvc pvc-config
   kubectl describe pvc pvc-logs
6. เข้า Pod เพื่อตรวจสอบ volume mount (ตัวอย่าง)
   bash
   Copy
   Edit
   kubectl exec -it <pod-name> -- sh
   ls /data/config
   ls /data/logs
   exit
7. ลบ Resource เมื่อต้องการ
   bash
   Copy
   Edit
   kubectl delete -f deployment.yaml
   kubectl delete -f pvc.yaml
   หมายเหตุ
   PVC สร้าง Azure Disk จริงๆ ผ่าน dynamic provisioning

ข้อมูลใน PVC จะคงอยู่แม้ Pod ถูกลบ จนกว่าจะลบ PVC

Deployment นี้มี 2 replicas ทำให้ Pod สองตัวใช้ PVC เหมือนกัน (ในโหมด ReadWriteOnce)

หากต้องการหลาย Pod เขียนพร้อมกัน อาจต้องใช้ StorageClass แบบ ReadWriteMany (เช่น AzureFile)

