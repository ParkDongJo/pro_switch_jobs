

server.use() 와 server.resetHandler 의 차이점

node 런타임이 존재하는 한 server 인스턴스에 미리 추가된 handler 들이 존재하고, use() 를 사용하면 handler 가 추가되는 반면에, resetHandler 를 하면 이전에 추가된 handler 들을 모두 제거한다.

https://mswjs.io/docs/api/setup-server/use
https://mswjs.io/docs/api/setup-server/resetHandler