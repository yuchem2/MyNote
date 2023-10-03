
[[Process]]들 간의 메시지가 직접 연결된 mailbox(port)를 통해서만 전달되는 [[Message-Passing System]]

Mailbox는 [[운영체제(Operating System)]] 내부에 위치하며 messsage를 임시 저장하는 객체(유일한 ID를 가짐)

오직 Mailbox의 링크만이 공유되며 여러 process 쌍들이 하나의 mailbox로도 공유 가능하다
링크는 단방향이나 양방향일 수 있다

그 mailbox를 소유한 유저만 메시지를 받을 수 있고, 다른 사용자들은 메시지를 보낼 수만 있다

