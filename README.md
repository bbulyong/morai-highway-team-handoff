# MORAI highway lane strategy — team handoff snapshot

## 무엇이 들어 있나

`purepursuit_mgeo/` 아래 ROS Noetic 제어 패키지에서 사용하는 **확보된 코드 파일**과 launch 스냅샷을 모았습니다.

- `scripts/highway_lane_strategy_node.py`: 2026-10-22 사용자가 직접 선택하여 업로드한 **잘 작동했던 V9 코드와 바이트 단위로 동일**합니다. (이전 smooth-lanehold/continuity-rollback 소스와 SHA-256 일치)
- `scripts/purepursuit_mgeo_node.py`: **이전 대화에서 사용한 센서팀 Pure Pursuit 17KB 버전**을 업로드했습니다. 별도로 준비한 [대화 첨부 ZIP]의 Lookahead preview 24KB 버전과 다릅니다. 실제 팀 PC에서 사용 중인 버전과 비교 후 필요하면 교체하세요.
- 다른 `scripts/*.py`와 `src/purepursuit_mgeo`: 사용자가 이전에 검증한 V8 지원 코드 묶음에서 가져왔습니다. 실제 팀 PC에서 바뀐 부분이 있다면 비교 후 선택하세요.
- `launch/`: 이전에 공유된 final, diagnostic_test, frenet_avoidance_debug launch 스냅샷입니다. 최근 주행 중 임의 변경한 모든 인자 값이 반영된 상태라는 보장은 없습니다.

원본을 덮어쓰기 전에 `SOURCE_MANIFEST.md`의 출처를 검토하세요.

## 중요 — 전체 ROS 워크스페이스 복제본이 아닙니다

이 저장소에는 대화에서 확보한 **제어 측 코드**가 들어 있습니다. 아래 실행 의존 요소는 별도로 필요한데 원본 전체 파일을 여기에서 확보하지 못했습니다.

- `morai_bringup` 및 `morai_udp_ekf_purepursuit_lidar_camera.launch` (launch에서 include)
- `lidar_perception` (특히 커스텀 메시지 `LidarObstacleArray`, 관련 노드)
- `camera_perception` (lane-info publisher와 카메라 모델·설정 파일)
- `ekf_local_enu`, `morai_udp_bridge`, `morai_udp_drive_bridge`
- `gps_mgeo_converter`, `traffic_light_stop`, `pedestrian_crossing_fusion`, 기타 센서팀 패키지
- `data/routes/2026_molit_comp_global_path.txt` 및 K-city MGeo 데이터
- 원래 패키지의 `package.xml`, `CMakeLists.txt`와 ROS 빌드 설정

**이 저장소를 클론한 것만으로 `catkin_make`나 전체 launch가 바로 성공한다고 보장할 수 없습니다.** 팀이 쓰는 전체 `morai_ws/src`와 합쳐야 합니다.

## 기존 워크스페이스 적용 방법

팀원은 기존 ROS Noetic 워크스페이스를 백업하고 `purepursuit_mgeo`의 동일 파일들과 비교하세요. 파일을 임의로 전체 교체하기 전에 `diff -u`로 확인하세요.

```bash
# 코드는 참조용입니다. 현재 실제 설치된 패키지 경로를 확인한 후 실행하세요.
rospack find purepursuit_mgeo
# diff -u /path/to/existing/scripts/highway_lane_strategy_node.py \
#          ./purepursuit_mgeo/scripts/highway_lane_strategy_node.py
```

MORAI 설정 IP는 환경마다 다릅니다. 실제 시험을 시작할 때는 `enable_control:=false`, 낮은 `target_speed_mps`로 경로와 토픽부터 검증하세요.

```bash
roslaunch purepursuit_mgeo morai_avoidance_highway_roundabout_diagnostic_test.launch \
  morai_host_ip:=<MORAI_PC_IP> enable_control:=false target_speed_mps:=2.0
```

고속 차선변경과 `INNER_HOLD`는 이전 시험에서 실선 침범 및 차로 인식 오류가 있었으므로, 이 코드는 **안전 보증된 자율주행 시스템이 아닙니다.** 시뮬레이터에서 확인한 뒤 사용해야 합니다.

## GitHub 브랜치 사용

새 빈 **Private** 저장소를 만든 뒤 `main`에는 프로젝트 설명을 두고, 개발 코드 파일은 `team-handoff` 브랜치에 게시하는 것을 권장합니다. 기존 작업 저장소는 수정할 필요가 없습니다.

`GITHUB_UPLOAD_GUIDE.md` 참조.


> **GitHub 업로드 주의:** 이 브랜치의 Pure Pursuit 파일은 이전 17KB 소스이며, 원래 handoff ZIP에 담긴 24KB preview 패치와 동일하지 않습니다. `SOURCE_MANIFEST.md`의 초기 SHA-256 목록은 ZIP 스냅샷 기준이므로, GitHub 게시본의 `purepursuit_mgeo_node.py`에는 그대로 적용되지 않습니다.
