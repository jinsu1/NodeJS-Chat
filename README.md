# NodeJS-Chat

-javascript에서 socket.io 라이브러리를 사용하여 웹소켓을 구현, node js로 웹서버를 구현하여 만든 채팅앱  

![nodejs 채팅](https://user-images.githubusercontent.com/69416518/115988828-66ca3d80-a5f6-11eb-9d96-e59635a142b0.JPG)  
***

코드
----
***
enter키로도 메세지를 전송 할 수있게하는 이벤트  
```
chat.js 

chatInput.addEventListener("keypress", (event) => {
    if(event.keyCode === 13) {
        send();
    }
});
```

***
값을 받아와서 메세지를 전송 후 메세지입력칸을 초기화  

```
chat.js


function send() {
    const param = {
        name: nickname.value,
        msg: chatInput.value
    }
    socket.emit("chatting", param)
    chatInput.value= null;
}
```
***  
메세지가 overflow 된 경우 현재 스크롤 값을 읽어서 그 위치로 이동하여 최신 메세지부터 보여줌
```
chat.js


displayContainer.scrollTo(0, displayContainer.scrollHeight);
```
***
메세지 전송시 나오는 인터페이스  
dom이라는 변수에 html코드를 백터로 넣고 이름, 내용, 시간만 받아오고 innerHTML로 넣음  
메세지와 프로필을 li로 만들어서 chatList의 li에 추가시킴  
서버에서 넘겨받은 이름과 nickname이같으면 보내는 글로 판단하여 클래스를 보내는사람으로 설정하여 css적용
```
chat.js


function LiModel(name, msg, time) {
    this.name = name;
    this.msg = msg;
    this.time = time;

    this.makeLi = () => {
        const li = document.createElement("li");
        li.classList.add(nickname.value === this.name ? "sent":"received");
        const dom = `<span class="profile">
        <span class="user">${this.name}</span>
        <img class="image" src="https://placeimg.com/50/50/any" alt="any">
        </span>
        <span class="message">${this.msg}</span>
        <span class="time">${this.time}</span>`;
    li.innerHTML = dom;
    chatList.appendChild(li);
```
***
메모  
----

**npm init -y**

package.json 을 만듬 어떤 npm을 다운 받았는지 등 기록이 남아서 커밋할 때 깃에 node_module의
용량을 줄일 수 있음

***

**npm install -g nodemon**

-g = 모든 프로젝트에서 적용   
nodemon = 코드에 변경이 있다면 서버를 자동으로 재실행 시킴  

설치 후 터미널에서 서버를 실행시킬 때 node app.js가아닌 nodemon app.js 로 실행  

***

package.json 에 있는 script에 변수처럼 설정 가능   
 
"scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1",  
    "start": "nodemon app.js"  
  },  
  
===> 터미널에 npm start 치면 app.js 서버실행  

***

npm install -g ngrok 

외부에서 나의 서버로 접근할 수 있도록 임시포트를 만들어주는  라이브러리

cmd를 실행시키고 ngrok http 포트번호 를 입력하면 url을 만들어준다 

***
