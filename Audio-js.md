## 자바스크립트에서 오디오 재생기능 구현
  - audio라는 변수를 선언해 Audio 객체를 생성하고, play() 메소드를 사용해서 음악을 재생하는 방법이다. 
  - new Audio('audio_file.mp3')에 ()안에는 자신이 넣고 싶은 음악 파일의 src를 적어주면 된다. 

만약 html 에 Audio 객체가 있고, 자바스크립트에서 이를 불러와 재생하고 싶다면 다음 방법을 쓸 수 있다.
document.getElementById('myAudio').play();

## 자주 쓰이는 Audio 객체의 속성
  1. audio.autoplay = true;
  -  // audio가 load 될 때 자동재생 됨
  2. audio.currentTime = 5;
  -  audio의 재생시점을 5초로 설정함
  3. audio.duration;
  -  audio의 길이를 초(seconds) 단위로 반환
  4. audio.loop = true;
  -  audio를 반복 재생함
  5. audio.src = "my_audio.mp3";
  -  audio의 경로를 지정함(URL)
  6. audio.volume = 0.2;
-  audio의 음량을 0.2로 지정함
-  음량은 0.0 ~ 1.0 사이 값으로 지정할 수 있고, 1.0이 가장 큰 음량
 

 

<자주 쓰이는 Audio 객체의 메소드>
  - audio.play(); // audio를 재생함
  - audio.pause(); // audio를 일시정지함
  - audio.load(); // audio를 다시 load함 // 주로 audio의 src나 설정이 바뀌었을 경우에 사용

### ex)
````js
	//실패시 사운드
	function palySoundFail(){
		var audio = new Audio('/sound/jungnang_gateFail.MP3');
		audio.play();
	}
````




