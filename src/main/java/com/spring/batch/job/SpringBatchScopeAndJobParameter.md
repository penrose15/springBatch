### Spring Batch Scope & Job Parameter

@StpeScope, @JobScope

JobScope는 Step 선언문에
StepScope는 Tasklet, ItemReader, ItemWriterm ItemProcessor에서 사용가능

아 참고로 둘의 기본 Scope는 singleton이라 빈 생성 시점이 job실행 시점이라 한다...
