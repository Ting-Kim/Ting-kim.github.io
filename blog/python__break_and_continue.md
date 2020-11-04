# 파이썬에서의 break과 continue

매번 반복 루프를 사용할 때 마다, 'break이 한번 탈출하는 거였나?', '모든 반복문을 탈출하는 거였나?' 하고 찾아보고선 기억을 계속 못하는 것 같아 기록한다.

- break : 하나의 반복문을 탈출함
- continue : 하나의 반복문 안에서 진행 중인 루프의 끝지점으로 이동

결국 둘 다 딱 한 루프에서만 작용한다.

하지만, 반복 루프에서 사용하는 방법을 알아왔다.

```
# 코딩테스트 예제

def solution(priorities, location):
    answer = 0
    ch = ord('a')
    documents = [chr(ch + i) for i in range(len(priorities))]

    # 목표한 출력 문서 기호
    target = documents[location]

    break_check = True
    
    # >>> 1번 루프 <<< 
    while priorities:
        J = priorities.pop(0)
        J_alpha = documents.pop(0)
        answer += 1

        # 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하는지 체크
        # >>> 2번 루프 <<<
        for i in range(len(priorities)):
            if J < priorities[i]:
                priorities.append(J)
                documents.append(J_alpha)
                answer -= 1

                # >>>> break 처리 <<<<
                break_check = False
                break
        
        # >>>> break 처리가 되어서 나온건지 체크 <<<<
        if break_check == False:
            break_check = True
            continue

        # J보다 중요도가 높은게 없으니 그대로 J 인쇄 ( pop된 상태이므로 내비둠
        if J_alpha == target:
            break

    return answer

```  

위는 프로그래머스에서 코딩테스트를 풀었던 코드이다.<br>
주석처리한 부분을 보면 2번 루프에서 break 처리를 했음을 알 수 있는데, 여기서 break_check의 boolean 형태의 변수에다가 False를 준 것을 확인할 수 있다.<br>
이는 이중반복문 형태로 사용하기 위해서 준 것이고, 밑에선 그 변수를 조건문으로 받아서 continue를 연계해서 사용한 것을 알 수 있다.<br>
기억해두면 용이하게 쓰지 않을까 싶다.

<hr>
reference<br>
- 'python 이중 for문 빠져나가기' - https://redmuffler.tistory.com/446