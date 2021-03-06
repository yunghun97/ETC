# ๐ Kubernetes

## ๐ง ๋ชฉ์ฐจ

[1. Pod](#Pod)
[2. Service](#Service)
[3. Volume](#Volume)
[4. ConfigMap, Secret](#ConfigMap,-Secret)
[5. Namespace, ResourceQuota, LimitRange](Namespace,-ResourceQuota,-LimitRange)
## Pod
### Container
- ํ๋์ ๋๋ฆฝ์ ์ธ ์๋น์ค๋ฅผ ๊ตฌ๋ํ  ์ ์๋ ์ปจํ์ด๋๊ฐ ์ฌ๋ฌ๊ฐ ์๋ค.
- ๊ทธ ์ปจํ์ด๋ ๋ค์ ๊ฐ๊ฐ ์๋น์ค๊ฐ ์ฐ๊ฒฐ๋  ์ ์๋๋ก ํฌํธ๋ฅผ ๊ฐ์ง๊ณ  ์๋ค.(์ฌ๋ฌ๊ฐ ๊ฐ์ง ์ ์๋ค.)
- ์ ๊ทผํ  ๋ localhost:{๋ค๋ฅธ ์ปจํ์ด๋ ํฌํธ๋ฒํธ}
- Pod ์์ฑ์ ๊ณ ์ ์ IP์ฃผ์ ํ ๋น **Kubernetes Cluster**๋ด์์๋ง ํด๋น IP๋ก Pod ์ ๊ทผ ๊ฐ๋ฅ(์ธ๋ถ์์๋ ํด๋น IP๋ก ์ ๊ทผ ๋ถ๊ฐ๋ฅ)
- Pod ๋ฌธ์  ๋ฐ์ ์ ์์คํ์ด ์ด๋ฅผ ๊ฐ์งํ์ฌ Pod ์ฌ์์ฑ ๋ฐ ์ญ์  ์งํ(IP๋ ์์ฑํ  ๋ ๋ง๋ค ๊ณ์ ๋ฐ๋๋ค.)

```bash
apiVersion: v1
kind: Pod
metadata:
    name: pod-1
spec:
    containers:
    - name: container1 # Container ๊ด๋ จ
      image: tmkube/p8000
      ports:
      - containerPort: 8000  # Container ๊ด๋ จ
    - name: container2
      image: tmkube/p8080
      ports:
      - containerPort: 8080
```

### Label
- ๋ชจ๋  ์ค๋ธ์ ํธ์ Label ์ค์  ๊ฐ๋ฅ
- ๋ชฉ์ ์ ๋ฐ๋ผ ์ค๋ธ์ ํธ ๋ถ๋ฅ ๋ฐ ๋ฐ๋ก ๊ด๋ฆฌ ๋ฐ ์ฐ๊ฒฐ
- **Key : Value**์ ํ Pod์๋ ์ฌ๋ฌ๊ฐ Label ๊ฐ๋ฅ ex) type:web io:dev
```bash
# Pod ๋ง๋ค ๋
apiVersion: v1
kind: Pod
metadata:
    name: svc-1
    labels: # Label ๊ด๋ จ
        type: web # Label ๊ด๋ จ Key: Value ํ์
        lo: dev  # Label ๊ด๋ จ

# Service์ ๋ง๋ค ๋
apiVersion: v1
kind: Service
metadata:
    name: svc-1
spec:
    selector: # Label ๊ด๋ จ selector์ Key์ Value๋ฅผ ๋ฃ์ผ๋ฉด ํด๋น Key Value์ ๋งค์นญ๋๋ Pod์ ์ฐ๊ฒฐ์ด ๋ฉ๋๋ค.
        type: web # Label ๊ด๋ จ
    ports:
      - port: 8080
```
### Node Schedule
> Pod๋ ์ฌ๋ฌ ๋ธ๋๋ค ์ค ํ ๋ธ๋์ ์ฌ๋ผ๊ฐ์ผ ํ๋ค.
1. ์ง์ ์ ํ
- Pod๋ฅผ ๋ง๋ค ๋ Node๋ฅผ ์ง์  ์ง์ ํ๋ค.
```bash
apiVersion: v1
kind: Service
metadata:
    name: pod-3
spec:
    nodeSelector:  # Node Schedule ๊ด๋ จ
      hostname: node1 # Node Schedule ๊ด๋ จ
```
2. ์ค์ผ์ฅด๋ฌ๊ฐ ํ๋จ
Pod๊ฐ ์ฌ์ฉํ๋ ๋ฉ๋ชจ๋ฆฌ ํฌ๊ธฐ๋ณด๊ณ  ํ๋จํด์ ์ ์ ํ Node์ ์ง์ด ๋ฃ๋๋ค.
```bash
apiVersion: v1
kind: Service
metadata:
    name: pod-3
spec:
  containers:
    - name: container1
      image: tmkube/p8000
      resources:
        requests:  # Node Schedule ๊ด๋ จ
            memory: 2Gi  # Node Schedule ๊ด๋ จ (๋ฉ๋ชจ๋ฆฌ 2GB๋ฅผ ์๊ตฌ)
        limits:
            memory: 3Gi # ์ต๋ ๋ฉ๋ชจ๋ฆฌ 3GB๋ฅผ ์๊ตฌ
# CPU Memory ์ด๊ณผ์
# Memory : Pod ์ข๋ฃ
# CPU :  ์ด๊ณผ์ request๋ก ๋ฎ์ถค, Over์ ์ข๋ฃ๋์ง ์์
```
![image](https://user-images.githubusercontent.com/71022555/168082602-5ab53940-3bd3-4c99-a201-6e759bb8849c.png)

## Service
- ์์ ์ Cluster IP๋ฅผ ๊ฐ์ง๊ณ  ์๋ค.
- ์ด ์๋น์ค๋ฅผ Pod์ ์ฐ๊ฒฐ์ํค๋ฉด ์ด ์๋น์ค๋ฅผ ํตํด Pod ์ ๊ทผ ๊ฐ๋ฅ
- Service๋ Pod์ ๋ค๋ฅด๊ฒ ์ฌ์ฉ์๊ฐ ์ญ์ ํ์ง ์์ผ๋ฉด ์ญ์  ๋์ง ์๋๋ค.
- ์ด๋ฅผ ํตํด Service๋ก ์ ๊ทผ ์ ํญ์ ์ฐ๊ฒฐ๋ Pod์ ์ ์ด ๊ฐ๋ฅ

### Service ์ข๋ฅ
1. ClusterIP
- Pod IP ์ฒ๋ผ Cluster๋ด ์ ๊ทผ๊ฐ๋ฅ ํ IP๊ฐ ์๋ค.
- ์ธ๋ถ์์๋ ์ฐ๊ฒฐ ๋ถ๊ฐ๋ฅ
- ์ฌ๋ฌ Pod๋ฅผ ์ฐ๊ฒฐ ๊ฐ๋ฅ ์ฌ๋ฌ๊ฐ Pod ์ฐ๊ฒฐ ์ Service๊ฐ ํธ๋ํฝ์ ๋ถ์ฐํด์ Pod์ ์ฐ๊ฒฐ ์์ผ์ค๋ค.
> ์ฌ์ฉ์ฒ : ์ธ๊ฐ๋ ์ฌ์ฉ์(์ด์์), ๋ด๋ถ ๋์ฌ๋ณด๋, Pod์ ์๋น์ค์ํ ๋๋ฒ๊น
```bash
# Service
apiVersion: v1
kind: Service
metadata:
    name: svc-1
spec:
    selector:
        app: pod # ์ Label์ ์ค๋ช ์ฒ๋ผ ํด๋น ๋ผ๋ฒจ์ ๊ฐ์ง ๊ฒ์ ์ ํ
    ports:
        - port: 9000 # 9000 ํฌํธ๋ก ์ ๊ทผ ์ 8080ํฌํธ์ ์ฐ๊ฒฐ
        targetPort: 8080
    type: ClusterIP # type์ Optional๊ฐ์ผ๋ก ์๋ต ๊ฐ๋ฅ ๊ธฐ๋ณธ = ClusterIP
```
2. NodePort
- Kubernetes ํด๋ฌ์คํฐ์ ์ฐ๊ฒฐ๋์ด ์๋ ๋ชจ๋  ๋ธ๋์๊ฒ ๋๊ฐ์ Port๋ฅผ ํ ๋น
- ์ธ๋ถ์์ ํฌํธ๋ก ์ ๊ทผ์ ํด๋น Service๋ก ์ฐ๊ฒฐ
- Pod๊ฐ ์๋ Node์๋ง ํฌํธ ํ ๋น์ด ์๋ ๋ชจ๋  ๋ธ๋์ Port ์ค์ 
> ์ฌ์ฉ์ฒ : ๋ด๋ถ๋ง ์ฐ๊ฒฐ, ๋ฐ๋ชจ๋ ์์ ์ฐ๊ฒฐ์ฉ
```bash
apiVersion: v1
kind: Service
metadata:
    name: svc-2
spec:
    selector:
        app: pod # ์ Label์ ์ค๋ช ์ฒ๋ผ ํด๋น ๋ผ๋ฒจ์ ๊ฐ์ง ๊ฒ์ ์ ํ
    ports:
        - port: 9000 # 9000 ํฌํธ๋ก ์ ๊ทผ ์ 8080ํฌํธ์ ์ฐ๊ฒฐ
        targetPort: 8080
        nodePort: 30000 # 30,000 ~ 32,767
    type: NodePort # type์ Optional๊ฐ์ผ๋ก ์๋ต ๊ฐ๋ฅ ๊ธฐ๋ณธ = ClusterIP

# externalTrafficPolicy: Local # ํน์  ๋ธ๋ ํฌํธ์ IP๋ก ์ ๊ทผํ๋ ํธ๋ํฝ์ Service๊ฐ ํด๋น ๋ธ๋์ ์ฌ๋ ค์ ธ ์๋ Pod์๋ง ํธ๋ํฝ์ ์ ๋ฌํด์ค๋ค.
```
3. Load Balancer
- ๊ธฐ์กด์ NodePort์ ํน์ง์ ๊ฐ์ง๊ณ  ์๋ค.
- Load Balancer๋ฅผ ์ถ๊ฐ๋ก ๋ฌ์์ ํธ๋ํฝ์ ๋ถ์ฐ์์ผ ์ค๋ค.
- Load Balancer ์ ๊ทผํ๊ธฐ ์ํ ์ธ๋ถ IP์ฃผ์๋ ๋ณ๋๋ก ์ธ๋ถ ์ ๊ทผ IP๋ฅผ ํ ๋นํ๋ ํ๋ฌ๊ทธ์ธ์ ์ค์นํด์ผํ๋ค. 
> ์ฌ์ฉ์ฒ : ์ธ๋ถ ์์คํ ๋ธ์ถ์ฉ
```bash
apiVersion: v1
kind: Service
metadata:
    name: svc-3
spec:
    selector:
        app: pod # ์ Label์ ์ค๋ช ์ฒ๋ผ ํด๋น ๋ผ๋ฒจ์ ๊ฐ์ง ๊ฒ์ ์ ํ
    ports:
        - port: 9000 # 9000 ํฌํธ๋ก ์ ๊ทผ ์ 8080ํฌํธ์ ์ฐ๊ฒฐ
        targetPort: 8080
    type: LoadBalancer # type์ Optional๊ฐ์ผ๋ก ์๋ต ๊ฐ๋ฅ ๊ธฐ๋ณธ = ClusterIP
```

## Volume
์ปจํ์ด๋๋ผ๋ฆฌ ๋ฐ์ดํฐ๋ฅผ ๊ณต์ ํ๊ธฐ ์ํด ์ฌ์ฉ
1. emptyDir
- Pod ์์ฑ์ ๋ง๋ค์ด์ง๊ณ  ์ญ์ ์ ์์ด์ง
```bash
apiVersion: v1
kind: Pod
metadata:
    name: pod-volume-1
spec:
    containers:
    - name: container1
      image: tmkube/init
      volumeMounts:
      - name: empty-dir
        mountPath: /mount1 # mount1 ์ด ์ปจํ์ด๋๊ฐ ์ด Path๋ก ๋ถ๋ฅจ์ ์ฐ๊ฒฐํ๊ฒ ๋ค.
    - name: container2
      image: tmkube/init
      volumeMounts:
      - name: empty-dir
        ountPath: /mount2  # mount2 Path๊ฐ ๋ค๋ฅด๋๋ผ๋ ๊ฐ์ volume name์ mountํ๋ฏ๋ก ๊ฐ์ ํ volume์ mountํ๊ณ  ์๋ค.
    volumes:
    - name : empty-dir
      emptyDir: {} # Volume์ ์์ฑ
```
2. hostPath
- Pod๋ค์ด ์ฌ๋ผ๊ฐ์ ธ ์๋ Node๋ฅผ Path๋ก ์ฌ์ฉํ๋ค.
- Pod๊ฐ ์ฃฝ์ด๋ node์ ์๋ ๋ฐ์ดํฐ๋ ์ฌ๋ผ์ง์ง ์๋๋ค.
- **Pod ์ฌ์์ฑ์ ์ด๋ค ๋ธ๋์ ๋ค์ ์ฌ์์ฑ ๋  ์ ๋ ์๋ค. ๋ฐ๋ผ์ ๊ธฐ์กด์ Node๋ฅผ Mountํ๊ธฐ ํ๋ค๋ค. -> Node์ถ๊ฐ์ Node๋ผ๋ฆฌ Mount๋ก ํด๊ฒฐ ๊ฐ๋ฅ(๋ถ์ ์ )** 
- ๊ฐ๊ฐ์ Node์๋ ์์ ์ ์ํด์ ์ฌ์ฉ๋๋ ํ์ผ๋ค(์์คํ ํ์ผ, ์ค์ ํ์ผ) Pod ์์ ์ด ํ ๋น๋์ด ์๋ ํธ์คํธ์ ๋ฐ์ดํฐ๋ฅผ ์ฝ๊ฑฐ๋ ์ธ ๋ ์ฌ์ฉํ๋ค.
```bash
apiVersion: v1
kind: Pod
metadata:
    name: pod-volume-2
spec:
    containers:
    - name: container1
      image: tmkube/init
      volumeMounts:
      - name: host-path
        mountPath: /mount1 # mount1 ์ด ์ปจํ์ด๋๊ฐ ์ด Path๋ก ๋ถ๋ฅจ์ ์ฐ๊ฒฐํ๊ฒ ๋ค.
    - name: container2
      image: tmkube/init
      volumeMounts:
      - name: host-path
        ountPath: /mount2  # mount2 Path๊ฐ ๋ค๋ฅด๋๋ผ๋ ๊ฐ์ volume name์ mountํ๋ฏ๋ก ๊ฐ์ ํ volume์ mountํ๊ณ  ์๋ค.
    volumes:
    - name : host-path # host-path
      hostPath:
        path: /node-v # ์ฌ์ ์ ํด๋น Node์ ์ด ๊ฒฝ๋ก๊ฐ ์์ด์ผ ์ค๋ฅ ๋ฐ์ X
        type: Directory
      
```

3. PVC / PV
- NFS, git, AWS ์ฒ๋ผ ์ธ๋ถ์ ์ฐ๊ฒฐ ๊ฐ๋ฅ
- Persistent Volume Claim์ ํตํด Pod์ PV๋ฅผ ์ฐ๊ฒฐ
- Volume ์ฌ์ฉ์ Admin๊ณผ User๋ก ๋๋ ์  ์๋ค.
    - Admin : ์ฟ ๋ฒ๋คํฐ์ค ๋ด๋น ์ด์์
    - User : Pod์ ์๋น์ค ๊ฐ๋ฐ ๋ฐ ๋ฐฐํฌ ๋ด๋น์

![image](https://user-images.githubusercontent.com/71022555/168121170-e68eeb9d-bb78-4af7-b958-25fdd517f43d.png)

## ConfigMap, Secret
- A ์๋น์ค๊ฐ ์ผ๋ฐ ์ ๊ทผ๊ณผ ๋ณด์ ์ ๊ทผ์ ๋ ๋ค ํ์ฉํ๋ ์ํฉ์์
    - ๊ฐ๋ฐ ํ๊ฒฝ์์๋ ๋ณด์ ์ ๊ทผํด์ 
    - ๋ฐฐํฌ ํ๊ฒฝ์์๋ ๋ณด์ ์ ๊ทผ์ค์ 
    - ์ด๋ฅผ ๊ฐ๊ฐ์ ์ด๋ฏธ์ง๋ก ๊ด๋ฆฌํ๊ธฐ์๋ ๋ญ๋น๊ฐ ํฌ๋ค.
- ์ผ๋ฐ์ ์ผ๋ก ๋ถ๋ฅํ๋ ์์๋ฅผ ConfigMap์ ์ ์ฅ
- ๋ณด์ํค๋ค์ Secret์ ์ ์ฅ
- ์ด๋ฅผ ์ฐ๊ฒฐํ์ฌ ์ฌ์ฉ
1. Env(Literal)
- ์์๋ก ์ ์ฅํ๊ธฐ
```bash
# ConfigMap
Key : Value
SSH : False
user : dev
# Secret ์ต๋ 1MB๋ง ๊ฐ๋ฅ(๋ฉ๋ชจ๋ฆฌ์ ์ ์ฅ)
Key : Value
Pw : Base64 # Base64์ธ์ฝ๋ฉ ํ ์ ์ฅํด์ผ ํ๋ค. -> Pod์์๋ ๋์ฝ๋ฉ ํ ์ ์ฅ๋จ
```
2. Env(File)
- File์ ํ๊ฒฝ๋ณ์์ ์ ์ฅ

3. Volume Mount(File)
- File์ Mount ํ๋ค.
![image](https://user-images.githubusercontent.com/71022555/168123696-bb379063-431c-44e1-bb4f-095f74153f3c.png)

## Namespace, ResourceQuota, LimitRange
- Pod์ ํฌ๊ธฐ๋ฅผ ์ ํํด์ Pod์ ์ฌ์ฉ๋์ด NameSpace์ค์  ๋ณด๋ค ๋์ผ๋ฉด ์ ๊ทผ X
- Cluter์๋ ๋ฌ์์ ์ ์ฒด ์์ ์ ํ ๊ฐ๋ฅ
![image](https://user-images.githubusercontent.com/71022555/168124175-a41c809a-71a7-4d81-aea3-ed1908476653.png)

1. Namespace
- Namespace์์ Pod์ด๋ฆ ์ค๋ณต X
- ๋ค๋ฅธ Namespace์ ๋ถ๋ฆฌ๋์ด์ ๊ด๋ฆฌ๋๋ค. (select์ lable์ด ๊ฐ์๋ X)
- Namespace์ญ์  ์ ์ด ์์ ์์์ด ๋ชจ๋ ์ญ์ ๋๋ค. 
- Pod๋ง๋ค IP๋ฅผ ๊ฐ์ง๊ณ  ์๋ค. 
2. ResourceQuota
- Namespace์ ์์ ํ๊ณ๋ฅผ ์ค์ 
- ResourceQuota๊ฐ ๋ช์๋ Namespace ์์ Pod๋ ์์ ์ ํ์ ์ค์ ํด์ฃผ์ด์ผ ํ๋ค.
3. LimitRange
- ์์์ ์ต์ ์ต๋๋ฅผ ์ค์ 
![image](https://user-images.githubusercontent.com/71022555/168125282-bff21c92-6c97-408f-8a69-bda8e049aaa4.png)
  
์ฐธ๊ณ ์๋ฃ  
---
[๋์ธ๋ ์ฟ ๋ฒ๋คํฐ์ค](https://www.inflearn.com/course/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EA%B8%B0%EC%B4%88)

