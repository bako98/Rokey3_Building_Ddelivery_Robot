# 📦 Last Mile: 아파트 단지 내 자율주행 배송로봇

## 프로젝트 개요

**Last Mile** 프로젝트는 아파트 단지 내 택배기사와 주민 간 갈등을 해결하고, 배송 효율성을 극대화하기 위한 **자율주행 배달 로봇 시스템**입니다. 본 시스템은 **SLAM, 객체 인식, 상태 기반 FSM, 실시간 웹 모니터링** 등을 결합하여 아파트 단지 내에서 **안전하고 정확한 라스트 마일 배송**을 수행합니다.

> **팀원**: 이진원, 박창범, 최정현, 정석환, 변재승, 조성훈, 김동건, 김현창  
> **사용 장비**: TurtleBot4, USB Camera, PC  
> **개발 환경**: Ubuntu 22.04, ROS2 Humble, Python3, OpenCV, YOLOv8, Flask  

---

## 프로젝트 구조 및 주요 기능

### Finite State Machine (FSM)

- **IDLE → MOVING**: 목표 좌표 수신 시 이동 시작
- **MOVING → WAIT**: 두 로봇 간 거리가 1.4m 이내일 경우 `robot9`만 대기
- **WAIT → MOVING**: 10초 대기 후 좌표 재할당
- **MOVING → FINISHED**: 목표 좌표 도달 시
- **FINISHED → RETURNING**: 초기 좌표 수신 시 복귀
- **RETURNING → IDLE**: 초기 위치 복귀 완료 시 대기 상태 전환

### 기능 구성

- **자율 주행**: SLAM 및 Nav2 기반 경로 탐색
- **객체 인식**: YOLOv8을 활용한 사람, 차량, QR 코드 인식
- **멀티 로봇 협업**: 네임스페이스 분리로 토픽 충돌 방지
- **웹 기반 모니터링 시스템**: Flask + SQLite를 이용한 실시간 제어 및 상태 확인
- **QR 인식 기반 목적지 추론**: QR 코드 분석 후 배송 위치 매핑

---

## 🔍 학습 데이터 요약

| Class  | Count |
|--------|-------|
| Number | 837   |
| Car    | 287   |
| Person | 248   |
| **Total** | **1372** |

- **YOLOv8s fine-tuned** (pretrained on `yolov8s.pt`)
- Epoch: 100  
- 자동 배치 사이즈 설정

---

## 🛠️ 개발 환경

### 🖥️ OS 및 개발 도구

- **OS**: Ubuntu 22.04 (PC, TurtleBot4 공통)
- **Tools**: VSCode, LabelImg, Git/GitHub
- **Language**: Python3
- **Network**: Wifi 기반 통신

### 💻 PC 패키지

- Python3
- ROS2 Humble
- OpenCV
- Ultralytics YOLOv8
- Flask
- SQLite3

### 🤖 AMR (TurtleBot4) 패키지

- Python3
- ROS2 Humble
- OpenCV
- Ultralytics YOLOv8

> 📄 **TurtleBot4 매뉴얼**: [공식 문서 보기](https://turtlebot.github.io/turtlebot4-user-manual/overview/features.html)

---

## 🧪 문제 해결 및 기술적 도전

| 문제 | 해결 방법 |
|------|-----------|
| Nav2 초기화/도킹 오류 | 상태 체크 및 로깅을 통한 디버깅 |
| 멀티 로봇 간 토픽 충돌 | ROS2 네임스페이스 분리 |
| 액션 서버 통신 지연 | 상태 플래그 및 주기적 상태 체크 구현 |

---

## 🔧 향후 개선 방향

- 장애물 예측 알고리즘 고도화
- 보행자 반응 속도 개선
- 사용자 앱 UI 고도화
- 실환경 유사 테스트 범위 확대

---

## 🚀 기대 효과 및 활용 방안

- **물류 스타트업 PoC**로 기술 검증 및 사업화 기반 확보
- **교육 커리큘럼 적용**: SLAM, 객체 인식, ROS 통신 등 실습형 콘텐츠
- **정부지원사업/IR 자료 활용**: 기술성과 시장성을 갖춘 물류 자동화 과제로 확장 가능

---

## 📂 디렉토리 구조 요약
```bash
.
├── build
├── install
├── log
├── run_steps.sh
└── src
    ├── mini_project
    │   ├── launch
    │   │   ├── detection_system.launch.py
    │   │   ├── navigation_system.launch.py
    │   │   └── setting_system.launch.py
    │   ├── map
    │   │   ├── best_1.json
    │   │   ├── best_2.json
    │   │   ├── best_3.json
    │   │   ├── map_old.pgm
    │   │   ├── map.pgm
    │   │   └── map.yaml
    │   ├── mini_project
    │   │   ├── detection
    │   │   │   ├── DetectionManager.py
    │   │   │   ├── DisplayManager.py
    │   │   │   ├── ImageConvertor.py
    │   │   │   ├── __init__.py
    │   │   │   ├── __pycache__
    │   │   │   │   ├── detectionDummy.cpython-310.pyc
    │   │   │   │   ├── detectionManager.cpython-310.pyc
    │   │   │   │   ├── displayManager.cpython-310.pyc
    │   │   │   │   ├── __init__.cpython-310.pyc
    │   │   │   │   ├── test_image_publisher.cpython-310.pyc
    │   │   │   │   ├── trackingManager.cpython-310.pyc
    │   │   │   │   └── yolov8_obj_det_track.cpython-310.pyc
    │   │   │   ├── QRDetection.py
    │   │   │   └── TrackingManager.py
    │   │   ├── __init__.py
    │   │   ├── navigation
    │   │   │   ├── FollowingCar_croppedDepth.py
    │   │   │   ├── GoalManager.py
    │   │   │   ├── GoalPoseExtractor.py
    │   │   │   ├── __init__.py
    │   │   │   ├── NavToPose.py
    │   │   │   ├── __pycache__
    │   │   │   │   ├── TaskManager.cpython-310.pyc
    │   │   │   │   ├── TaskManager_test.cpython-310.pyc
    │   │   │   │   ├── TaskManager_ver2.cpython-310.pyc
    │   │   │   │   └── utils.cpython-310.pyc
    │   │   │   ├── RobotNavigator.py
    │   │   │   ├── TaskManager.py
    │   │   │   └── utils.py
    │   │   └── tmp.zip
    │   ├── model
    │   │   └── real_final_best.pt
    │   ├── package.xml
    │   ├── param
    │   │   └── mini_project.yaml
    │   ├── resource
    │   │   └── mini_project
    │   ├── setup.cfg
    │   ├── setup.py
    │   └── test
    │       ├── test_copyright.py
    │       ├── test_flake8.py
    │       └── test_pep257.py
    ├── qr_detector_camera
    │   ├── package.xml
    │   ├── qr_detector_camera
    │   │   ├── __init__.py
    │   │   ├── qr_detector_camera_node.py
    │   │   ├── qr_detector_webcam.py
    │   │   └── test_server.py
    │   ├── resource
    │   │   └── qr_detector_camera
    │   ├── setup.cfg
    │   ├── setup.py
    │   └── test
    │       ├── test_copyright.py
    │       ├── test_flake8.py
    │       └── test_pep257.py
    ├── qr_interfaces
    │   ├── CMakeLists.txt
    │   ├── include
    │   │   └── qr_interfaces
    │   ├── msg
    │   │   └── QRInfo.msg
    │   ├── package.xml
    │   ├── src
    │   └── srv
    │       ├── GetTargetRoom.srv
    │       └── Step.srv
    └── test_website
        ├── app.py
        ├── __pycache__
        │   └── app.cpython-310.pyc
        ├── TaskManager.py
        ├── templates
        │   ├── dashboard.html
        │   ├── login.html
        │   └── logs.html
        ├── test.db
        └── test_webcam.py
```

---

## 🔄 실행 방법 (예시)

```bash
# 패키지 빌드
colcon build --symlink-install
source install/setup.bash

# 전체 시스템 실행
ros2 launch mini_project setting_system.launch.py
ros2 launch mini_project detection_system.launch.py
ros2 launch mini_project navigation_system.launch.py
```

🙌 Special Thanks
이 프로젝트는 팀원 모두의 협업과 열정으로 완성되었습니다. 특히 TurtleBot4와 ROS2의 결합을 통해 실제 자율주행 배송 시스템의 가능성을 체험할 수 있었습니다.


---

필요 시 다음 항목을 추가로 보완할 수 있습니다:
- 각 노드(`DetectionManager.py`, `NavToPose.py` 등)에 대한 개별 설명
- 웹 대시보드 화면 예시
- 영상 / 데모 링크
- 라이선스 (MIT 등)

원하시면 추가 포맷팅(한글/영문 혼용, 기업 제출용 등)도 가능합니다.











--------------------------------------------------
--------------------------------------------------
--------------------------------------------------
--------------------------------------------------

# 📚 Rokey3 Library Book Organizing Collaborative Robot

도서관 책 정리 업무를 자동화하여 사서의 업무 부담을 줄이고, 도서 정리의 정확성과 효율을 향상시키는 ROS2 기반 협동로봇 프로젝트입니다.

---

## 프로젝트 개요

### **프로젝트 주제 및 선정 배경**

강북구 등 공공도서관의 사서 수는 전국 평균에 비해 절반에도 못 미치며, 1인이 모든 업무를 도맡는 비정상적 상황이 지속되고 있습니다.  
이에 따라 반납된 책을 자동으로 정리하는 협동로봇을 개발하여 사서의 업무 부담을 줄이고 도서관 운영의 효율성을 향상시키고자 하였습니다.

---

## 사용 장비 및 기술 스택

- **로봇**: 두산 M0609 협동로봇
  
![image](https://github.com/user-attachments/assets/e6115195-658b-4864-abc5-638ba14c0478)
  
- **그리퍼**: OnRobot RG2

![image](https://github.com/user-attachments/assets/fe5fe635-e9d0-42cb-b05a-40d173ad6d6e)

- **통신**: Simple Socket Tool [(링크)](https://sourceforge.net/projects/simple-socket-tool/)  
- **소프트웨어**
  - ROS2 Humble (Ubuntu 22.04)
  - Python3
  - Flask (웹 시각화 서버)
  - Gazebo (시뮬레이션 환경)
- **외부 패키지**: [DoosanBootcamInt1](https://github.com/ROKEY-SPARK/DoosanBootcamInt1)

---

## 프로젝트 트리 구조

```bash
cobotics_ws/
├── book_visualizer/ # 책 정보를 시각화하는 웹앱 (Flask)
│   ├── app_book.py
│   └── templates/
│   └── index.html
├── src/
│   ├── book_organizing_bot/ # ROS2 패키지: 책 정리 봇
│   │   ├── book_organizing_bot/
│   │   │   ├── BookOrganizing.py
│   │   │   └── init.py
│   │   ├── package.xml
│   │   ├── resource/
│   │   │   └── book_organizing_bot
│   │   ├── setup.cfg
│   │   └── setup.py
│   └── DoosanBootcamInt1/ # 의존 프로젝트
│   └── ... (외부 종속 요소 포함)
├── build/ # colcon 빌드 디렉토리
├── install/
├── log/
```

---

## 사전 요구 사항


### ROS2 및 필수 패키지 설치

Ubuntu 22.04 + ROS2 Humble 환경에서 개발되었습니다. 다음 패키지를 설치해 주세요:

```bash
sudo apt-get update
sudo apt-get install -y \
  libpoco-dev libyaml-cpp-dev wget \
  ros-humble-control-msgs ros-humble-realtime-tools ros-humble-xacro \
  ros-humble-joint-state-publisher-gui ros-humble-ros2-control \
  ros-humble-ros2-controllers ros-humble-gazebo-msgs ros-humble-moveit-msgs \
  dbus-x11 ros-humble-moveit-configs-utils ros-humble-moveit-ros-move-group \
  ros-humble-gazebo-ros-pkgs ros-humble-ros-gz-sim ros-humble-ign-ros2-control
```

## Gazebo 시뮬레이터 설치
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install -y \
  libignition-gazebo6-dev \
  ros-humble-gazebo-ros-pkgs \
  ros-humble-ros-gz-sim \
  ros-humble-ros-gz
```

## 외부 패키지 DoosanBootcamInt1 설치
이 프로젝트는 다음 외부 패키지의 설치를 요구합니다:

[DoosanBootcamInt1 GitHub](https://github.com/ROKEY-SPARK/DoosanBootcamInt1)

```
cd cobotics_ws_/
cd src/
git clone https://github.com/ROKEY-SPARK/DoosanBootcamInt1.git
```

## 사용한 소켓 툴

<img src="https://github.com/user-attachments/assets/1704d6bb-76c2-4ff9-86c6-52c667c6a4cc" width="400"/>

[simple socket tool](https://sourceforge.net/projects/simple-socket-tool/)


---


## 빌드 및 실행 방법
```
# 워크스페이스 루트로 이동
cd ~/cobotics_ws
# 의존성 설치
rosdep install --from-paths src --ignore-src -r -y
# 빌드
colcon build 
# 환경 설정
source install/setup.bash



## 협동로봇 연결 및 rviz 화면 열기 예시

# 1번 터미널 (협동로봇 연결 및 rviz 화면 열기)
ros2 launch dsr_bringup2 dsr_bringup2_rviz.launch.py mode:=real host:=192.168.1.100 port:=12345 model:=m0609

# 2번 터미널
# 노드 실행 (책 정리 봇)
ros2 run book_organizing_bot BookOrganizing

# 3번 터미널
## 소켓툴 열기 예시
java -jar /home/bako98/cobotics_ws/SST_1v3.jar

# 소켓툴을 클라이언트로 127.0.0.1에 2000포트로 연결
# 소켓툴에 입력으로 red, purple, red, brown 등 입력 (사용한 책 예시)

# 4번 터미널
## 웹 시각화 서버 실행
# 접속 주소: http://localhost:5000 를 웹 주소에 입력
cd book_visualizer
python3 app_book.py

```
1. 협동로봇 연결
2. 책 정리 봇 노드 실행
3. 웹 시각화 서버 실행
4. 소켓툴 실행
5. 소켓툴을 클라이언트로 127.0.0.1에 2000포트로 연결
6. 소켓툴에 입력으로 red, purple, red, brown 등 입력 (사용한 책 예시)
![image](https://github.com/user-attachments/assets/fbd92686-016a-4e8a-ba93-a18ab77a92be)

---

## 동작 예시
## 소켓에서 입력받은 구역으로 책 정리

<img src="https://github.com/user-attachments/assets/c9140dc1-b196-4005-a6d1-2db99a74ae05" width="400"/>

## 높이 측정 및 집기 동작

![image](https://github.com/user-attachments/assets/8d5aef60-5066-4196-8b7a-daf5afe3984b)

## 웹 시각화 결과
![image](https://github.com/user-attachments/assets/30f49cf6-ee17-4482-a90e-0fa77f6dfd95)


## 예시영상

[![Video Label](http://img.youtube.com/vi/mOouOeEV-3M/0.jpg)](https://youtu.be/mOouOeEV-3M)


---

## 참고
ROS2: https://docs.ros.org/en/humble/index.html

Gazebo: https://gazebosim.org/

DoosanBootcamInt1: https://github.com/ROKEY-SPARK/DoosanBootcamInt1

---

## 🙋‍♂️ 팀 소개
Team Cobotics
김동건 · 김현창 · 변재승 · 조성훈

