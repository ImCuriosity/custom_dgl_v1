# custom_dgl_v1
DGL의 기존 BFS 방식의 분산 GNN 학습을 DFS 방식으로 고도화

[개요] 우리가 만들 아키텍처
개발 환경: 로컬 PC의 VS Code (Remote-SSH 확장 사용)
Master Node (VM 1): 코드 수정, NFS 서버(소스 공유), 학습 컨트롤 타워
Worker Node (VM 2~N): NFS를 마운트하여 Master의 코드를 그대로 실행

1. GCP 인스턴스 5대 생성
   - GCP Console -> Compute Engine
   - 인스턴스 생성 (총 5개):
     * Master Node: dgl-master (n1-standard-4 이상, Deep Learning Image)
     * Worker Nodes: dgl-worker-1 ~ dgl-worker-4 (Master와 동일 사양 권장)
     * 주의: 5대 모두 같은 리전(Region), 같은 VPC에 있어야 합니다.
   - 네트워크/방화벽 (필수):
     * default-allow-internal (소스: 10.0.0.0/8 등 내부 IP 대역) 규칙이 있어서 모든 포트가 열려있는지 꼭 확인하세요.
2. 공유 스토리지 설정
   - 분산 학습 시 모든 노드가 그래프 데이터와 코드에 접근해야 합니다. 데이터를 각 노드에 일일이 복사하는 것보다 공유 파일 시스템을 쓰는 것이 관리하기 쉽습니다.
   - Google Cloud Filestore (NFS): GCP의 관리형 NFS 서비스입니다. 이를 생성하여 모든 VM의 /workspace 같은 경로에 마운트하면, 한 곳에서만 데이터를 수정해도 모든 노드에 반영됩니다.






