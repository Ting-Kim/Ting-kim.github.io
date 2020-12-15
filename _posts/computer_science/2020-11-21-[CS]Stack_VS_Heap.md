# Stack VS Heap

개념을 까먹었고 오해하고 있어서 정리 파일을 만들었다.<br><br>

![https://cs50.harvard.edu/x/2020/notes/4/memory_layout.png](https://cs50.harvard.edu/x/2020/notes/4/memory_layout.png)

메모리 구조 (from CS50 Lecture)<br><br>

**Machine Code**

머신 코드 영역에는 프로그램이 실행될 때, 해당 프로그램이 컴파일 된 바이너리 코드가 저장된다.<br><br>

**Stack**

RAM(Random Access Memory) 영역 중 각 함수의 지역변수들을 LIFO(Last In First Out) 방식으로 저장하는 영역이다.

(Java에서는 heap 영역에 생성된 Object 타입의 데이터들에 대한 참조값들이 할당됨. 원시타입의 데이터들은 실제 값 할당.)

함수가 종료되면 스택에서 해당 함수가 pop() 되어지고, 함수에서 선언했던 지역변수도 메모리에서 사라지게 된다.

Stack 메모리는 Thread 하나당 하나씩 할당된다. 각 스레드에서 다른 스레드의 Stack 영역에는 접근할 수 없다.<br><br>

**Heap**

사용자가 직접 관리할 수 있는 메모리 영역으로, 사용자에 의해 메모리가 동적으로 할당되고 해제된다. C의 경우 malloc으로 할당된 메모리의 데이터가 저장된다.

- 모든 Object 타입은 heap 영역에 생성된다.
- 스레드 수와 관계없이 단 하나의 heap 영역만 존재한다.
  <br><br>

**(Java)Garbage Collector**

**Mark and Sweep**이라고도 한다.

Mark : Stack의 모든 변수를 스캔하며 각각 어떤 Object를 참조하고 있는지 찾는 과정. 참조되는 Object들은 Marking.

Sweep : Mark 되어있지 않은 모든 Object를 heap에서 제거하는 과정

Unreachable Object 를 우선적으로 메모리에서 제거하여 메모리 공간을 확보한다.

(Unreachable Object 란 Stack 에서 도달할 수 없는 Heap 영역의 객체를 말함)<br><br>
