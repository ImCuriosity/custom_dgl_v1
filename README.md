# custom_dgl_v1
DGL의 기존 BFS 방식의 분산 GNN 학습을 DFS 방식으로 고도화

[개요] 우리가 만들 아키텍처
개발 환경: 로컬 PC의 VS Code (Remote-SSH 확장 사용)
Master Node (VM 1): 코드 수정, NFS 서버(소스 공유), 학습 컨트롤 타워
Worker Node (VM 2~N): NFS를 마운트하여 Master의 코드를 그대로 실행

1. GCP 인프라 구성 (Compute Engine)
   - VM 인스턴스 생성:
     -- 최소 2대 이상의 VM이 필요합니다 (1대는 마스터/Trainer, 나머지는 워커).
     -- 이미지로 Deep Learning VM Image를 선택하면 CUDA, Python, PyTorch 등이 미리 설치되어 있어 편합니다.
     ■ 최소 2대 이상의 VM이 필요합니다 (1대는 마스터/Trainer, 나머지는 워커).
     ■ 팁: 이미지로 Deep Learning VM Image를 선택하면 CUDA, Python, PyTorch 등이 미리 설치되어 있어 편합니다.
   - 네트워크 설정 (필수):
     ■ 모든 VM은 서로 통신이 가능해야 합니다. 같은 VPC 내에 배치하세요.
     ■ 방화벽(Firewall): DGL은 통신을 위해 특정 포트(기본 30050 등)를 사용합니다. 내부 IP 간의 모든 트래픽을 허용하거나 해당 포트를 열어줘야 합니다.
   - SSH 접속 설정:
     ■ DGL의 실행 스크립트(launch.py)는 마스터 노드에서 다른 노드로 SSH를 통해 명령을 내립니다.
     ■ 마스터 노드에서 다른 모든 노드로 **비밀번호 없이 SSH 접속(Passwordless SSH)**이 되도록 SSH 키(~/.ssh/id_rsa)를 생성하고 공개키를 각 노드의 authorized_keys에 등록해야 합니다.
2. 공유 스토리지 설정
   - 분산 학습 시 모든 노드가 그래프 데이터와 코드에 접근해야 합니다. 데이터를 각 노드에 일일이 복사하는 것보다 공유 파일 시스템을 쓰는 것이 관리하기 쉽습니다.
   - Google Cloud Filestore (NFS): GCP의 관리형 NFS 서비스입니다. 이를 생성하여 모든 VM의 /workspace 같은 경로에 마운트하면, 한 곳에서만 데이터를 수정해도 모든 노드에 반영됩니다.






