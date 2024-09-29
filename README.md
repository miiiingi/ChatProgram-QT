# ChatProgram-QT

## 목표
```
본 프로젝트는 채팅과 영상 전송 및 스트리밍이 가능한 프로그램을 만드는 것을 목표로한다.
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
- 두 명의 클라이언트가 채팅을 진행하는 모습. 각자 GST_RTSP 서버로부터 수신한 영상을 재생할 수 있다.
```
<p align="center">
  <img width="1370" alt="스크린샷 2024-09-29 오후 12 36 31" src="https://github.com/user-attachments/assets/ff61095f-ed86-4169-a852-7b384d25e588">
</p> 

## 배운 점
```
- RTSP, RTP
- GStreaming
- 영상 코덱(e.g. H.264)
- 멀티스레딩과 readyRead메소드를 이용한 비동기 처리
- UDP/IP 스택
```

## 구현 중 발생한 문제점과 해결책
```
- 여러 개의 클라이언트가 접속한 상황에서, 영상을 일시정지한 후, 다시 재생하였을 때, 패킷이 손실되어 영상의 딜레이가 발생
- RTSP를 통해 전송할 때, 버퍼링을 설정(zerolatency 옵션과 queue 요소를 추가하는 방법)하여 영상 딜레이 문제를 해결
```

## 구현 중 어려웠던 점
```
- Gstreamer의 사용: 초기에는 스트리밍 서버를 사용해야한다는 것을 모르는 상태였기 때문에 데이터 베이스등을 사용하였음. 그 사실을 알게된 후, Gstreamer를 통해서 스트리밍을 진행하였음. Gst 스트리밍 서버를 실행시키기 위해 라이브러리를 설정하고, QT프로그램 내에서 코드를 작성하는 것이 어려웠음.
- 다양한 소스로부터의 스트리밍: 애플 웹캠을 이용하거나 원래 영상 파일을 이용하여 스트리밍하는 것은 오류가 발생하여 Gst에서 기본적으로 제공하는 비디오를 이용하여 구현을 진행. 추후에 애플 웹캠이나 영상 파일을 이용하여 스트리밍하는 기능을 추가할 예정.(아마 영상 크기, 압축의 종류, 받아오는 방법에서의 문제일 것이라고 생각함)
```
