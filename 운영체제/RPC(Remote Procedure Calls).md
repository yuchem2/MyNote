
네트워크로 연결된 시스템 사이의 프로시저 호출을 추상화한 시스템

서버의 실제 프로시저를 위한 클라이언츠 측의 proxy를 Stubs이라고 하며 이 Stubs이 서버의 위치를 찾고, 파라미터를 암호화한다(marshal)

서버측 Stubs은 이 메시지를 받아 암호화된 패킷을 해석하고 서버에서 프로시저를 수행한 뒤 결과를 반환
