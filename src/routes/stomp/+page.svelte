<script>
    import SockJS from 'sockjs-client';
    import Stomp from 'stompjs';

    let socket = null;
    let stompClient = null;

    function connectSocket() {
        // // SockJS를 사용하여 웹소켓 연결 설정
        socket = new SockJS('http://192.168.10.206:12067/stream');
        console.log(socket,"socket")

        // 연결이 되면 url 표기도 변경됨 -> ws://192.168.10.206:12067/stream/216/zboq1ww3/websocket
        stompClient = Stomp.over(socket);
        console.log(stompClient,"stompClient")
        // 연결 시작
        stompClient.connect({}, (frame) => {
            console.log('Connected: ' + frame);

            // 메시지 구독
            stompClient.subscribe('/sub/state/outer/device', (message) => {
                console.log('Received: ' + message.body);
            });

            // 메시지 발행
            stompClient.send("/pub/sendMessage", {}, JSON.stringify({
                message: "Hello, Server!"
            }));
        }, (error) => {
            console.log(error, "error")
        });
    }

    function disConnectSocket() {
        if(socket != null && stompClient != null){
            stompClient.disconnect(()=>{
                socket = null;
                stompClient = null;
            });
        }
    }

</script>

<div>
    SockJS
    <ol>
        <li>
            역할 : 안정적인 웹소켓 연결을 제공. 폴백 기능을 통해 브라우저가 웹소켓을 지원하지 않아도 연결할 수 있게 함.
        </li>
        <li>
            기능 : 웹소켓을 지원하지 않는 경우에 HTTP 폴링 등을 통해 웹소켓과 유사한 통신 방식을 제공.
        </li>
        <li>
            제공하지 않는 기능 : 메시지 프로토콜. 단순한 연결만 지원.
        </li>
    </ol>
    STOMP.js
    <ol>
        <li>
            역할 : STOMP 프로토콜을 사용하여 메시지를 발행하고 구독하는 작업을 간편하게 구현.
        </li>
        <li>
            기능 : 구독/발행 패턴을 통한 메시지 관리, 메시지 헤더 처리, 연결 해제 등의 작업을 쉽게 처리
        </li>
    </ol>
    <button on:click={connectSocket}>connect</button>
    <button on:click={disConnectSocket}>disConnect</button>
</div>