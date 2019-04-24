# ReactiveProgramming
Hi

## ObserverPattern
### Example Code
```typescript
type TMessage = {
  message: string;
};

function ownForEach<T> (array: T[], predicate: (value?: any, index?: number, predicateArray?: T[]) => any) {
  if (!Array.isArray(array)) {
    return [];
  }

  const arrLeng = array.length;

  if (!arrLeng) {
    return [];
  }

  let idx = 0;

  while (idx < arrLeng) {
    predicate(array[idx], idx, array);
    idx += 1;
  }
}

class YoutubeChannel {
  private _subscribedUsers: Observer[] = [];
  protected _state: TMessage = {
    message: ''
  };

  // Observer 등록(구독)
  addSubscriber(observer: Observer) {
    this._subscribedUsers = [...this._subscribedUsers, observer];
    console.log('- 유튜브 채널 구독 완료 -', observer);
    console.log('- 현재 채널 구독자 리스트 -', this._subscribedUsers);
  }

  // Observer 삭제(구독 해지)
  removeSubscriber(observer: Observer) {
    this._subscribedUsers = this._subscribedUsers.filter(o => o !== observer);
    console.log('- 유튜브 채널 구독 해지 -', observer);
    console.log('- 현재 채널 구독자 리스트 -', this._subscribedUsers);
  }

  // 구독한 모든 observer의 update 메서드를 호출하여 데이터를 전파
  protected notify(state: TMessage) {
    ownForEach(this._subscribedUsers, o => {
      console.log(`${o.constructor.name} 구독자에게 알림을 전파한다!`, state);
      o.update(state);
    });
  }
}

class MyYoutubeChannel extends YoutubeChannel {
  // 구독한 모든 observer에게 알림 메시지를 전파
  setMessage(message: string) {
    this._state.message = message;
    this.notify(this._state);
  }
}

abstract class Subscriber {
  abstract update(message: TMessage): void;
}

class CadenSubscriber extends Subscriber {
  update(message: TMessage) {
    console.log(`구독자 ${this.constructor.name}에게 알림이 도착하였다!`, message);
  }
}

class DayzenSubscriber extends Subscriber {
  update(message: TMessage) {
    console.log(`구독자 ${this.constructor.name}에게 알림이 도착하였다!`, message);
  }
}

const PewwwTV = new MyYoutubeChannel();
console.log('- PewwwTV -', PewwwTV);

const caden = new CadenSubscriber();
const dayzen = new DayzenSubscriber();

// 구독
PewwwTV.addSubscriber(caden);
PewwwTV.addSubscriber(dayzen);

// 데이터 전파
PewwwTV.setMessage('구독자분들 환영합니다!');

// 구독 취소
PewwwTV.removeSubscriber(caden);

// 데이터 전파
PewwwTV.setMessage('비록 1명 뿐이지만 환영합니다..!');
```

### Result in Console
<img src="./imgs/observer-test.png" alt="Console Result Image">

### Explanation
Hi, How are you