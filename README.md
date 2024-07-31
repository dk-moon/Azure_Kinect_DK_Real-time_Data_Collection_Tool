# Azure_Kinect_DK_Real-time_Data_Collection_Tool

- Azure Kinect DK Real-time Data Collection Tool

- Azure Kinect DK를 최대 2대를 사용하여 Syncronize 및 Standalone Mode로 동작시켜 설정한 Frame Rate로 촬영을 하면서 1 Frame 마다 데이터를 수집하는 도구

## 소스 코드(C# WPF)

- AKCalibration.cs : Azure Kinect DK의 Calibration 정보 추출

- AKConfig.cs : Azure Kinect DK의 Sensor 및 Tracker 설정

- AKDataSave.cs : 촬영 Data 저장

- AKImageConvert.cs : Azure Kinect SDK의 Image 객체를 WPF의 Writeablebitmap 객체로 변환

- AKPower.cs : Azure Kinect DK의 전원 On/Off

- AKTracker.cs : Azure Kinect Body Tracking SDk의 Tracker 정보 추출

- CocoCreater.cs : 전체 수집 Data를 하나의 coco dataset format으로 annotaton json파일로 정보를 수합

- MainWindow.xaml : UI 구성 xaml 파일

- MainWindow.xaml.cs : WPF Main 프로그램 동작 제어 및 구성 파일

## 시스템 요구사항

- GPU : RTX 3080 이상

- CPU : intel i5, i7 10th 이상

- RAM : 16GB 이상

- SSD 추천(1Frame마다 Data를 저장하기에 디스크의 읽기/쓰기 기능이 원활해야함)

- C드라이브에 FFMPEG

## 데이터 유형

- Color Image(jpg)

- Depth Image(16bit png)

- Transformed Depth Image(16bit png)

- Infrared Image(16bit png)

- Calibration Information(json)

  - Id : Azure Kinect Index ID

  - Location : Azure Kinect Setting Location

  - Serial_number : Azure Kinect Serial Number

  - Extrinsics : Extrinsics Information(rotation, translation, metric radius, resolution width & height)

  - Instrinsics : Instrinsics Information(color & depth focal information)
  
  - Convert_mode : Convert Mode Information

- Joint Tracker Data(json)

  - categories

    - supercategory : “person”

    - id : category 의 id

    - name : “person”

    - keypoints : joint의 순서 및 명칭의 리스트

    - skeleton : skeleton 구성 관계도

    - vector_angle : vector 각도 값의 순서

    - axies : joint axis의 순서

  - annotations

    - camera_id : Azure Kinect 매칭 번호

    - video_id : video 항목과 매칭 정보

    - v_annotation

      - category_id : categories의 매칭 번호

      - id : annotation ID

      - body_id : tracker의 body id

      - target : 촬영 대상자인지 여부

      - iscrowd

      - keypoints : joint 위치 정보

      - Vector_angle : joint 각도 정보

      - area

      - bbox

- Color Video(mp4) --> FFMPEG Library 이용

- CoCo Dataset Format Annotation Data(json)

## 사용 가이드

1. Setting버튼을 선택하여 Azure Kinect Dk의 Sensor 및 Tracker, Data 저장소 위치를 설정한다.

![image](https://user-images.githubusercontent.com/59715960/218610162-756ad42e-5800-4295-9a09-2bb33769b88f.png)

2. Data 저장 기능 활성화 여부에 관한 토글 버튼으로 Deactivate 상태로 전환해준다.

![image](https://user-images.githubusercontent.com/59715960/218610100-0389adf1-1cd8-4c37-9f0c-4603a7da90fa.png)

3. Azure Kinect DK의 전원을 켜주기 위하여 왼쪽의 버튼을 먼저 누르고 파란불로 전환이 되었다면, 우측의 촬영버튼을 눌러서 촬영을 시작한다.
![image](https://user-images.githubusercontent.com/59715960/218610280-6f3caa13-80dc-43bf-9b6f-114747c44935.png)

4. 촬영을 하면서 Azure Kinect DK의 정상 동작 여부 및 촬영 각도, 위치, 조명 등의 환경을 세팅한다.

5. 세팅이 완료가 되었다면 우측의 촬영버튼을 눌러서 촬영을 종료하고 좌측의 전원 버튼을 눌러서 Azure Kinect DK를 종료를 해준다.

6. Data 저장 기능을 Deactivate에서 Activate로 토글 버튼을 전환시켜 준다.

![image](https://user-images.githubusercontent.com/59715960/218610100-0389adf1-1cd8-4c37-9f0c-4603a7da90fa.png)

7. PA Data 수집 장소의 위치, 촬영 대상자의 고유 아이디 번호, PA Game의 Module 번호를 선택 및 기입한 후 체크 버튼을 선택하여 준다.

![image](https://user-images.githubusercontent.com/59715960/218610905-e5223d80-b4be-4813-b2e3-a37697dfe9cf.png)

8. Azure Kinect DK의 전원을 켜주기 위하여 왼쪽의 버튼을 먼저 누르고 파란불로 전환이 되었다면,
PA Game Module의 시작에 맞춰서 우측의 촬영버튼을 눌러서 촬영을 시작한다.

![image](https://user-images.githubusercontent.com/59715960/218610280-6f3caa13-80dc-43bf-9b6f-114747c44935.png)


9. PA Game Module 한가지를 완료했다면, 우측의 촬영버튼을 눌러서 촬영을 종료하여준다.

10. 다음 PA Game Module을 시작하기 전에 촬영 대상자 정보 입력 항목 중 Game Module을 재설정 해주고 체크 버튼을 선택하여 준 후 PA Game 시작 시 다시 촬영 버튼을 선택하여 데이터를 수집한다. 이를 PA Game의 전체 Module이 종료될 때까지 반복하여 준다.

11. PA Game Module이 전체 수행했다면, 촬영 종료 후 Azure Kinect DK의 전원을 꺼준다.

12. 수집 데이터의 정상 여부를 확인하다.
수집 경로 : (지정한 저장소)/AzureKinectData/(대상자 ID)/(촬영일자)/(PA Game Module)/..
Video의 Folder가 생성되어있지 않거나, Video의 일부 파일이 생성되어있지 않다면, cmd에서 FFMPEG 명령어를 통해 직접 생성하여 준다.
(ffmpeg -f image2 -r (전체 이미지 수 / 촬영 시간(초)) -i (color image 폴더 경로) -c:v h264_nvenc (video 폴더))
