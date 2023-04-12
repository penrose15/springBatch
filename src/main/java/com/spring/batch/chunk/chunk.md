## Chunk

한번에 하나씩 데이터를 읽어 Chunk라는 덩어리를 만든 뒤, Chunk 단위로 트랜잭션을 다루는 것

* Reader에서 데이터를 하나씩 읽어온다
* 읽어온 데이터를 Processor에서 가공한다.
* 가공된 데이터들을 별도의 공간에 모은 뒤, Chunk 단위만큼 쌓이게 되면 Writer에 전달하고 Writer는 일괄 저장한다.

### ChunkOrientedTasklet 엿보기
Chunk 지향 처리의 전체 로직을 다루는 것은 ChunkOriedtedTasklet 클래스이다.
`chunkProvider.provide()` 로 Reader에서 chunk size 만큼 데이터를 가져온다.
`chunkProcessor.process()`에서 Reader로 받은 데이터를 가공하고 저장한다. 

`chunkProvider.provide()`를 까보면 

`inputs`이 chunkSize만큼 쌓일 때까지 `read()`를 호출하고
`read()`는 내부를 보면 `ItemReader.read`를 호출한다.

### SimpleChunkProcessor
Processor, Writer 로직을 담고 있다.

`process()`내부에는 다음과 같은 일이 일어나고 있다.
* `Chunk<T> inputs`를 파라미터로 받는다
* `transform()`에서는 전달받은 `inputs`를 `doProcess()`로 전달하고 반환값을 받는다.
* `transform()`을 통해 가공된 대량의 데이터는 `write()`를 통해 일괄 저장된다.

`doProcess()`를 처리하는데 만약 ItemProcessor가 없다면 item을 그대로 반환하고 있다면 ItemProcessor의 `process()`로 가공하여 반환한다.
이후 SimpleChunkProcessor의 `doWrite()`를 호출하여 일괄 처리한다.

### PageSize vs Chunk Size

PageSize는 한번에 조회할 Item의 양
ChunkSize는 한번에 처리될 트랜잭션 단위

이 둘의 값이 다르면 성능 이슈가 발생할 수 있다.

예를 들어 PageSize가 10이고 ChunkSize가 50이면 Page 조회가 5번 발생할 때마다 Chunk가 1번 일어난다는 뜻으로 
한번의 트랜잭션 처리에 5번의 쿼리 조회가(...) 발생한다는 뜻이다.

그러므로 저 둘의 의미가 다르더라도 값을 같게 하는게 좋다