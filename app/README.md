# Getting Started with Gamja

감자로 간단한 todo app 을 생성한다.

## pre requirements
- java version
- maven or gradle

## dependencies
``` groovy
implementation 'io.agistep:core:0.1.7'
implementation 'io.agistep:storage:0.1.7'
```

## 1. events 정의
- TodoCreatedEvent
- TodoUpdatedEvent
- TodoDeletedEvent

## 2. commands 정의
- CreateTodoCommand
- UpdateTodoCommand
- DeleteTodoCommand

## 3. aggregates 정의 

- command 생성
- command 를 처리하는 event handler 생성
- event 가 발생하면 aggregate 의 상태를 바꾼다

```java

public class App {
    public static void main(String[] args) {
        
        AggregateRepository<TodoAggregate> aggregateRepository = new AggregateRepository(TodoAggregate.class, eventStore);
        CommandProcessor<TodoCommand> commandProcessor = new CommandProcessor(TodoCommand.class, aggregateRepository);
        long createdAggregateId = commandProcessor.process(new CreateTodoCommand());

        commandProcessor.process(createdAggregateId, new UpdateTodoCommand());
        commandProcessor.process(createdAggregateId, new DeleteTodoCommand());

        // 사용자는 aggregateId 로 aggregate 상태를 확인할 수 있어야한다.
        // command 처리, event 발행됐음을 확인하는 핸들러 필요
        // 커맨드 발생 리스너
        // 커맨드 .. 스레드 로컬 ..
        // 인메모리 상에서 인메모리 view 를 만들어서 보여준다. 그 리스너를 인메모리용으로 만들어주고, 웹용으로도 생성할 수 있게
        // 감자의 비전 : 사용자가 많은 걸 알지않아도 부담없이 쓸 수 있도록.
    }
}

```
