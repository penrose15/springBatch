### Spring Batch Job Flow

[참고한 자료](https://github.com/jojoldu/spring-batch-in-action/blob/master/4_BATCH_JOB_FLOW.md)

* `.on()`
  * 캐치할 ExitStatus 지정
  * `*` 인 경우 모든 ExitStatus가 지정된다.
* `to()`
  * 다음으로 이동할 Step 지정
* `from()`
  * 일종의 이벤트 리스너 역할을 한다.
  * 상태값을 보고 일치하는 상태라면 `to()`에 해당되는 step를 호출한다.
  * 추가로 이벤트 캐치를 하려면 `from`을 써야 한다.
* `end()`
  * end는 FlowBuilder를 end와 FlowBuilder를 종료하는 end 두가지 종류가 있다.
  