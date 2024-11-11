# ChatProgram-QT

## 목표
```
본 프로젝트는 실시간 영상 전송 및 객체 검출이 가능한 프로그램을 만드는 것을 목표로한다.
```

## 구현과정
<ul>
  <li>RTSP서버 실행 및 비디오 스트리밍</li>
  <ul>
    <li>클라이언트 ‘Stream Video to Server’버튼을 누르면 GST_RTSP 서버를 실행하고, RTSP를 통해 비디오를 스트리밍</li>
     <ul>
      <li>이때, GST_RTSP 서버를 실행하면서 채팅 프로그램도 동작해야하기 때문에 멀티 스레드로 서버를 실행</li>
      <li>영상 압축을 위해 H.264 코덱을 사용</li>
    </ul>
  </ul>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/b1259c66-e671-4112-b473-b7c5782e1a80" alt="RTSP서버 실행 및 비디오 스트리밍" width="150">
</p> 

<ul>
  <li>RTSP전달 및 영상 재생</li>
  <ul>
    <li>클라이언트가 ‘Play Video’버튼을 누르면 서버에서 RTSP 주소를 클라이언트에게 전송</li>
  </ul>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/adf6da39-d969-4cbc-b2dd-d6c8cb446faa" alt="RTSP전달 및 영상 재생1" width="350">
</p>

<ul>
  <li>RTSP전달 및 영상 재생</li>
  <ul>
    <li>클라이언트가 RTSP 주소를 수신하면, 미디어 플레이어에서 영상을 재생</li>
  </ul>
</ul>
<p align="center">
  <img src="https://github.com/user-attachments/assets/b60cea1d-574b-4e7d-965b-2a295526da3f" alt="RTSP전달 및 영상 재생2" width="350">
</p> 

## 구현 결과
```
- 서버로부터 카메라의 영상을 받아 객체를 검출한 모습
```
<p align="center">
  <img width="350" alt="객체 검출 모델 추론" src="https://github.com/user-attachments/assets/de8ba4c5-2b45-49c6-be25-4be7359babc7">
</p> 

## 배운 점
```
- RTSP, RTP, RTCP 내용에 대한 이해
- GStreamer
- h.264에 대한 이해
- 멀티스레딩과 readyRead 메소드를 이용한 비동기 처리
- UDP를 이용한 비디오 전송
- libtorch의 사용법
```  

## 구현 중 어려웠던 점
```
- 여러 개의 클라이언트가 접속한 상황에서, 영상을 일시정지한 후, 다시 재생하였을 때, 패킷이 손실되어 영상의 딜레이가 발생
- Gstreamer의 사용: 초기에는 스트리밍 서버를 사용해야한다는 것을 모르는 상태였기 때문에 데이터 베이스등을 사용하였음. 스트리밍 서버를 사용해야한다는 알게된 후, Gstreamer를 통해서 스트리밍을 진행하였음. Gstreamer RTSP 서버를 실행시키기 위해 라이브러리를 설정하여 영상을 전송
- 다양한 소스로부터의 스트리밍: 카메라마다 사용할 수 있는 색상 공간이나 src가 다르므로 그것을 정확히 지정해주어야만 스트리밍이 성공적으로 수행되었음. 이것을 찾는데 어려움을 겪음.
- Pytorch를 C++에서 사용할 수 있도록 libtorch라는 라이브러리를 제공함. 하지만, Pytorch에서 공식적으로 제공하는 libtorch는 x86-64 아키텍쳐이기 때문에 arm64 아키텍쳐에서 사용하기 위해선 새롭게 빌드를 해주어야했음. 또한, yolov8 모델을 사용했었는데, 실행하는 환경의 스크립트와 export하는 환경의 스크립트를 맞춰줘야 제대로 동작하였음. 이것을 맞추는 것이 어려웠다. 
```
