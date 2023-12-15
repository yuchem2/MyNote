
[[운영체제(Operating System)]]가 CPU에 대한 제어권을 유지하기 위한 장치이다

[[운영체제(Operating System)]]는 유저 응용 프로그램이 무한 루프에 빠지거나 system service를 부르지 않거나, 제어권을 돌려주지 않는 것을 방지해야할 필요가 있다. 이를 위해 사용하는 것이 Timer이다.

무한 루프, [[운영체제/Process]]가 리소스를 점유하는 것을 방지한다

일정한 주기, 또는 명령어가 지난 후 [[인터럽트(Interrupt)]]를 발생시켜 CPU로 제어권이 넘어갈 수 있도록 한다
+ Fixed Timer: 특정한 주기가 지난 후 [[인터럽트(Interrupt)]]를 발생시킨다
+ Variable Timer: 일정한 양의 instruction을 지나게 되면 [[인터럽트(Interrupt)]]를 발생시킨다. 이때 OS가 counter 변수를 통해 감지한다

사용자가 제어권을 넘기기 전에 [[운영체제(Operating System)]]는 Timer가 [[인터럽트(Interrupt)]]되도록 설정되어있는지 확인하고, Timer가 [[인터럽트(Interrupt)]]를 하면 자동으로 제어권이 [[운영체제(Operating System)]]으로 전달된다

