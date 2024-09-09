<script>
    // 서버와 웹소켓 연결 생성
    const ws = new WebSocket('ws://192.168.10.206:12067');

    // 연결이 열렸을 때 발생하는 이벤트
    ws.onopen = () => {
        console.log('서버와 연결되었습니다.');
        ws.send('안녕하세요 서버!');
    };

    // 서버로부터 메시지를 받을 때 발생하는 이벤트
    ws.onmessage = (event) => {
        console.log(`서버로부터 메시지: ${event.data}`);
    };

    // 연결이 닫혔을 때 발생하는 이벤트
    ws.onclose = () => {
        console.log('서버와의 연결이 종료되었습니다.');
    };
</script>

<h1>WebSocket Client (Without Libraries)</h1>