# custom_dgl_v1
DGL의 기존 BFS 방식의 분산 GNN 학습을 DFS 방식으로 고도화

[개요] 우리가 만들 아키텍처
개발 환경: 로컬 PC의 VS Code (Remote-SSH 확장 사용)
Master Node (VM 1): 코드 수정, NFS 서버(소스 공유), 학습 컨트롤 타워
Worker Node (VM 2~N): NFS를 마운트하여 Master의 코드를 그대로 실행

