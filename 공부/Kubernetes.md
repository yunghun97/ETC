# 😀 Kubernetes

## 🧐 목차

[1. Pod](#Pod)
[2. Service](#Service)
[3. Volume](#Volume)
[4. ConfigMap, Secret](#ConfigMap,-Secret)
[5. Namespace, ResourceQuota, LimitRange](Namespace,-ResourceQuota,-LimitRange)
## Pod
### Container
- 하나의 독립적인 서비스를 구동할 수 있는 컨테이너가 여러개 있다.
- 그 컨테이너 들은 각각 서비스가 연결될 수 있도록 포트를 가지고 있다.(여러개 가질 수 있다.)
- 접근할 때 localhost:{다른 컨테이너 포트번호}
- Pod 생성시 고유의 IP주소 할당 **Kubernetes Cluster**내에서만 해당 IP로 Pod 접근 가능(외부에서는 해당 IP로 접근 불가능)
- Pod 문제 발생 시 시스템이 이를 감지하여 Pod 재생성 및 삭제 진행(IP는 생성할 때 마다 계속 바뀐다.)

```bash
apiVersion: v1
kind: Pod
metadata:
    name: pod-1
spec:
    containers:
    - name: container1 # Container 관련
      image: tmkube/p8000
      ports:
      - containerPort: 8000  # Container 관련
    - name: container2
      image: tmkube/p8080
      ports:
      - containerPort: 8080
```

### Label
- 모든 오브젝트에 Label 설정 가능
- 목적에 따라 오브젝트 분류 및 따로 관리 및 연결
- **Key : Value**쌍 한 Pod에는 여러개 Label 가능 ex) type:web io:dev
```bash
# Pod 만들 때
apiVersion: v1
kind: Pod
metadata:
    name: svc-1
    labels: # Label 관련
        type: web # Label 관련 Key: Value 형식
        lo: dev  # Label 관련

# Service에 만들 때
apiVersion: v1
kind: Service
metadata:
    name: svc-1
spec:
    selector: # Label 관련 selector에 Key와 Value를 넣으면 해당 Key Value와 매칭되는 Pod와 연결이 됩니다.
        type: web # Label 관련
    ports:
      - port: 8080
```
### Node Schedule
> Pod는 여러 노드들 중 한 노드에 올라가야 한다.
1. 직접선택
- Pod를 만들 때 Node를 직접 지정한다.
```bash
apiVersion: v1
kind: Service
metadata:
    name: pod-3
spec:
    nodeSelector:  # Node Schedule 관련
      hostname: node1 # Node Schedule 관련
```
2. 스케쥴러가 판단
Pod가 사용하는 메모리 크기보고 판단해서 적절한 Node에 집어 넣는다.
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
        requests:  # Node Schedule 관련
            memory: 2Gi  # Node Schedule 관련 (메모리 2GB를 요구)
        limits:
            memory: 3Gi # 최대 메모리 3GB를 요구
# CPU Memory 초과시
# Memory : Pod 종료
# CPU :  초과시 request로 낮춤, Over시 종료되지 않음
```
![image](https://user-images.githubusercontent.com/71022555/168082602-5ab53940-3bd3-4c99-a201-6e759bb8849c.png)

## Service
- 자신의 Cluster IP를 가지고 있다.
- 이 서비스를 Pod와 연결시키면 이 서비스를 통해 Pod 접근 가능
- Service는 Pod와 다르게 사용자가 삭제하지 않으면 삭제 되지 않는다.
- 이를 통해 Service로 접근 시 항상 연결된 Pod와 접촉 가능

### Service 종류
1. ClusterIP
- Pod IP 처럼 Cluster내 접근가능 한 IP가 있다.
- 외부에서는 연결 불가능
- 여러 Pod를 연결 가능 여러개 Pod 연결 시 Service가 트래픽을 분산해서 Pod와 연결 시켜준다.
> 사용처 : 인가된 사용자(운영자), 내부 대쉬보드, Pod의 서비스상태 디버깅
```bash
# Service
apiVersion: v1
kind: Service
metadata:
    name: svc-1
spec:
    selector:
        app: pod # 위 Label에 설명 처럼 해당 라벨을 가진 것을 선택
    ports:
        - port: 9000 # 9000 포트로 접근 시 8080포트와 연결
        targetPort: 8080
    type: ClusterIP # type은 Optional값으로 생략 가능 기본 = ClusterIP
```
2. NodePort
- Kubernetes 클러스터에 연결되어 있는 모든 노드에게 똑같은 Port를 할당
- 외부에서 포트로 접근시 해당 Service로 연결
- Pod가 있는 Node에만 포트 할당이 아닌 모든 노드에 Port 설정
> 사용처 : 내부망 연결, 데모나 임시 연결용
```bash
apiVersion: v1
kind: Service
metadata:
    name: svc-2
spec:
    selector:
        app: pod # 위 Label에 설명 처럼 해당 라벨을 가진 것을 선택
    ports:
        - port: 9000 # 9000 포트로 접근 시 8080포트와 연결
        targetPort: 8080
        nodePort: 30000 # 30,000 ~ 32,767
    type: NodePort # type은 Optional값으로 생략 가능 기본 = ClusterIP

# externalTrafficPolicy: Local # 특정 노드 포트에 IP로 접근하는 트래픽은 Service가 해당 노드에 올려져 있는 Pod에만 트래픽을 전달해준다.
```
3. Load Balancer
- 기존의 NodePort의 특징을 가지고 있다.
- Load Balancer를 추가로 달아서 트래픽을 분산시켜 준다.
- Load Balancer 접근하기 위한 외부 IP주소는 별도로 외부 접근 IP를 할당하는 플러그인을 설치해야한다. 
> 사용처 : 외부 시스템 노출용
```bash
apiVersion: v1
kind: Service
metadata:
    name: svc-3
spec:
    selector:
        app: pod # 위 Label에 설명 처럼 해당 라벨을 가진 것을 선택
    ports:
        - port: 9000 # 9000 포트로 접근 시 8080포트와 연결
        targetPort: 8080
    type: LoadBalancer # type은 Optional값으로 생략 가능 기본 = ClusterIP
```

## Volume
컨테이너끼리 데이터를 공유하기 위해 사용
1. emptyDir
- Pod 생성시 만들어지고 삭제시 없어짐
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
        mountPath: /mount1 # mount1 이 컨테이너가 이 Path로 불륨을 연결하겠다.
    - name: container2
      image: tmkube/init
      volumeMounts:
      - name: empty-dir
        ountPath: /mount2  # mount2 Path가 다르더라도 같은 volume name을 mount하므로 같은 한 volume을 mount하고 있다.
    volumes:
    - name : empty-dir
      emptyDir: {} # Volume의 속성
```
2. hostPath
- Pod들이 올라가져 있는 Node를 Path로 사용한다.
- Pod가 죽어도 node에 있는 데이터는 사라지지 않는다.
- **Pod 재생성시 어떤 노드에 다시 재생성 될 수 도 있다. 따라서 기존의 Node를 Mount하기 힘들다. -> Node추가시 Node끼리 Mount로 해결 가능(부적절)** 
- 각각의 Node에는 자신을 위해서 사용되는 파일들(시스템 파일, 설정파일) Pod 자신이 할당되어 있는 호스트의 데이터를 읽거나 쓸 때 사용한다.
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
        mountPath: /mount1 # mount1 이 컨테이너가 이 Path로 불륨을 연결하겠다.
    - name: container2
      image: tmkube/init
      volumeMounts:
      - name: host-path
        ountPath: /mount2  # mount2 Path가 다르더라도 같은 volume name을 mount하므로 같은 한 volume을 mount하고 있다.
    volumes:
    - name : host-path # host-path
      hostPath:
        path: /node-v # 사전에 해당 Node에 이 경로가 있어야 오류 발생 X
        type: Directory
      
```

3. PVC / PV
- NFS, git, AWS 처럼 외부에 연결 가능
- Persistent Volume Claim을 통해 Pod와 PV를 연결
- Volume 사용시 Admin과 User로 나눠저 있다.
    - Admin : 쿠버네티스 담당 운영자
    - User : Pod에 서비스 개발 및 배포 담당자

![image](https://user-images.githubusercontent.com/71022555/168121170-e68eeb9d-bb78-4af7-b958-25fdd517f43d.png)

## ConfigMap, Secret
- A 서비스가 일반 접근과 보안 접근을 둘 다 허용하는 상황에서
    - 개발 환경에서는 보안 접근해제
    - 배포 환경에서는 보안 접근설정
    - 이를 각각의 이미지로 관리하기에는 낭비가 크다.
- 일반적으로 분류하는 상수를 ConfigMap에 저장
- 보안키들을 Secret에 저장
- 이를 연결하여 사용
1. Env(Literal)
- 상수로 저장하기
```bash
# ConfigMap
Key : Value
SSH : False
user : dev
# Secret 최대 1MB만 가능(메모리에 저장)
Key : Value
Pw : Base64 # Base64인코딩 후 저장해야 한다. -> Pod에서는 디코딩 후 저장됨
```
2. Env(File)
- File을 환경변수에 저장

3. Volume Mount(File)
- File을 Mount 한다.
![image](https://user-images.githubusercontent.com/71022555/168123696-bb379063-431c-44e1-bb4f-095f74153f3c.png)

## Namespace, ResourceQuota, LimitRange
- Pod의 크기를 제한해서 Pod의 사용량이 NameSpace설정 보다 높으면 접근 X
- Cluter에도 달아서 전체 자원 제한 가능
![image](https://user-images.githubusercontent.com/71022555/168124175-a41c809a-71a7-4d81-aea3-ed1908476653.png)

1. Namespace
- Namespace안의 Pod이름 중복 X
- 다른 Namespace와 분리되어서 관리된다. (select와 lable이 같아도 X)
- Namespace삭제 시 이 안의 자원이 모두 삭제된다. 
- Pod마다 IP를 가지고 있다. 
2. ResourceQuota
- Namespace에 자원 한계를 설정
- ResourceQuota가 명시된 Namespace 안의 Pod는 자원 제한을 설정해주어야 한다.
3. LimitRange
- 자원의 최소 최대를 설정
![image](https://user-images.githubusercontent.com/71022555/168125282-bff21c92-6c97-408f-8a69-bda8e049aaa4.png)
  
참고자료  
---
[대세는 쿠버네티스](https://www.inflearn.com/course/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EA%B8%B0%EC%B4%88)

