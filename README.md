# react-intersection-observer

### Intersection Observer API 란?
어떤 Element가 화면(viewport)에 노출되었는지를 감지할 수 있는 API이다. 무한 스크롤(Infinite Scroll)을 만들수 있다.

### 무한 스크롤을 구현하는 이유

- Scroll Event를 사용해서 구현할 때 사용하는 debounce & throttle 을 사용하지 않아도 됩니다..
- Scroll Event를 사용해서 구현할 때 구하는 offsetTop 값을 구할 때 는 정확한 값을 구하기 위해서 매번 layout을 새로 그리는데 이를 Reflow라 합니다. Intersection Observer를 사용하면 Reflow를 하지 않습니다.
- Scroll Event를 사용하는것 보다 비교적 이해 및 사용하기가 쉽습니다.

### Intersection Observer - Options

```
let observer = new IntersectionObserver(callback, options);
```

Intersection Observer를 생성할 때는 옵션을 설정할 수 있습니다.
옵션에는 root, rootMargin, threshold가 있습니다.

![](https://velog.velcdn.com/images/bunny/post/819b328a-2298-40f5-b4d1-4f71d52bfbd0/image.png)


### root
- 이 옵션에 정의된 Element를 기준으로 Target Element가 노출되었는지 노출 되지 않았는지를 판단합니다. </br>
- 기본값은 Browser Viewport이며, root 값이 null 또는 지정되지 않았을 때 기본값으로 설정됩니다.</br>
### rootMargin
- root에 정의된 Element가 가진 마진값을 의미합니다. 사용법은 CSS의 margin 속성과 매우 유사합니다. </br>
- threshold를 계산할 때 rootMargin 만큼 더 계산합니다.</br>
### threshold
- Target Element가 root에 정의된 Element에 얼만큼 노출되었을 때 Callback함수를 실행시킬지 정의하는 옵션입니다. </br>
- number 또는 number[]로 정의할 수 있습니다.</br>
- number 로 정의할 경우, Target Element 의 노출 비율에 따라 Callback Function을 한번 호출할 수 있지만, number[] 로 정의할 경우, 각각의 비율로 노출될 때마다 Callback Function을 호출합니다.</br>

# debounce & throttle
스로틀(Throttle)과 디바운스(Debounce)란 무엇일까?

이 두 가지 방법 모두 DOM 이벤트를 기반으로 실행하는 **자바스크립트를 성능상의 이유**로 **JS의 양적인 측면, 즉 이벤트(event)를 제어(제한)하는 방법**입니다.
Throttle 과 Debounce 는 이벤트 핸들러가 많은 연산(예 : 무거운 계산 및 기타 DOM 조작)을 수행(이벤트 핸들러의 과도한 횟수가 발생하는 것)하는 경우에 대해 제약을 걸어 시간이 지남에 따라 함수를 몇 번이나 실행 할지를 제어하는 유사한 기술이지만 서로 다릅니다.

사용 예는 아래와 같습니다.
- 사용자가 창 크기 조정을 멈출 때까지 기다렸다가 resizing event 사용하기 위해
- 사용자가 키보드 입력을 중지(예: 검색창) 할 때까지 ajax 이벤트를 발생시키지 않기 위해
- 페이지의 스크롤 위치를 측정하고 최대 50ms 마다 응답하기를 바랄 경우에
- 앱에서 요소를 드래그 할 때 좋은 성능을 보장하기 위해

## Debounce    
Debounce 는 이벤트를 그룹화하여 특정시간이 지난 후 이벤트를 한번 발생하도록 하는 기술입니다. 즉, 순차적 호출을 하나의 그룹으로 "그룹화"할 수 있습니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F994614365C17654319"/>

## Throttle

Throttle은 이벤트를 일정한 주기마다 발생하도록 하는 기술입니다. 예를 들어 Throttle의 설정시간으로 1ms를 주게되면 해당 이벤트는 1ms 동안 최대 한번만 발생하게 됩니다. 

특성 자체가 실행 횟수에 제한을 거는 것이기 때문에 일반적으로 성능 문제 때문에 많이 사용합니다. </br>
스크롤을 올리거나 내릴 때 scroll 이벤트 핸들러 경우에 매우 많이 발생합니다. </br>
scroll 이벤트가 발생할 때 뭔가 복잡한 작업을 하도록 설정했다면 매우 빈번하게 실행되기 때문에 큰 버퍼링이 걸릴 지도 모를 것입니다. </br>
그럴 때 쓰로틀링을 사용할 수 있스빈다. 몇 초에 한 번, 또는 몇 밀리초에 한 번씩만 실행되게 제한을 두는 것입니다. </br>
    
## Debounce 와 Throttle 차이점
디바운싱과 스로틀의 가장 큰 차이점은 스로틀은 적어도 X 밀리 초마다 정기적으로 기능 실행을 보장한다는 것입니다. </br>
Debounce 는 아무리 많은 이벤트가 발생해도 모두 무시하고 특정 시간사이에 어떤 이벤트도 발생하지 않았을 때 딱 한번만 마지막 이벤트를 발생시키는 기법입니다.  </br>
따라서 5ms가 지나기전에 계속 이벤트가 발생할 경우 콜백에 반응하는 이벤트는 발생하지 않고 계속 무시됩니다. </br>



# Reflow

# react-query 

## 설치

```shell
npm i @tanstack/react-query @tanstack/react-query-devtools
```

## useQuery 사용법

쿼리 함수가 변수에 따라 달라진다면 해당 변수를 쿼리 키에 포함시켜야 합니다. 쿼리 키는 검색하는 데이터를 고유하게 식별하며, 쿼리 함수에서 사용하는 변수가 변경될 경우 해당 변수를 쿼리 키에 포함시켜야 합니다. 
쿼리 키에 종속 변수를 추가하면 해당 변수에 따라 독립적으로 캐시되며, 변수가 변경될 때마다 자동으로 다시 쿼리되어 새로운 데이터를 가져오게 됩니다. (staleTime 설정에 따라 달라집니다).

```shell
import { useQuery } from '@tanstack/react-query'

function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),
  })
}

```

- queryKey는 조회하고 싶은 데이터의 캐시 키를 넣습니다. 
리액트 쿼리에서는 이 캐시 키를 사용하여 데이터를 캐싱합니다. 한번 받아온 다음 나중에 같은 요청을 해야 하는 상황에서 데이터가 이미 존재한다면 기존에 있던 데이터를 바로 보여 줍니다. 
그리고 설정에 따라 데이터를 새로 요청할 수도 있습니다.

- queryFn는 쿼리 키 값에 따라 실행 될 비동기 함수 입니다.
</br>
</br>
useQuery Hook를 사용하여 반환된 result 객체는 다음 값을 가지고 있습니다.
</br></br>

- isPending : 아직 데이터를 받아오지 않았고, 현재 데이터를 요청 중
- isError : 오류발생
- isSuccess : 데이터 요청 성공
- error - 요류가 발생했을 때 오류 정보를 지닙니다.
- data - 요청 성공한 데이터를 가리킵니다.
- isFetching - 데이터가 요청 중일때 true가 됩니다. 데이터가 이미 존재하는 상태에서 재요청을 할때 isLoding은 false이지만 inFatching는 true입니다.


```shell
function Todos() {
  const { isPending, isError, data, error } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodoList,
  })

  if (isPending) {
    return <span>Loading...</span>
  }

  if (isError) {
    return <span>Error: {error.message}</span>
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

## useQuery의 option

useQuery를 사용할 때 세 번째 파라미터에 options 객체를 넣어서 해당 Hook의 작동 방식을 설정할 수 있습니다. 

```shell
import { useQuery } from '@tanstack/react-query'

function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),{
        enabled:true;
        refetchOnMount:true;
    }
  })
}
```

options에 설정할 수 있는 필드들은 다음과 같습니다.
|option|내용|
|:---:|:---|
|enabled|`boolean` 타입으리 값을 설정하며, 이 값이 `false`라면 컴포넌트가 마운트 될 때 자동으로 요청하지 않습니다. refetch 함수로만 요청이 시작 됩니다.|
|retry| `boolean` or `number` or `(failureCount : number, error : TError) => boolean` 타입의 값을 설정하며, 요청이 실패 했을 때 재요청 할지 설정할 수 있습니다.</br>- 이 값을 true로 하면 실패 했을때 성공할 때까지 계속 반복 요청 합니다.</br>- 이 값을 false로 하면 실패 했을때 재요청하지 않습니다.</br>- 이 값을 3으로 하면 3번 재요청 합니다.</br>- 이 값을 함수 타입으로 설정하면 실패 횟수와 오류타입에 따라 재요청 할지 함수 내에서 결정할 수 있습니다.|
|retryDelay| `number` or `(retryAttemp:number, error: TError) => number` 타입의 값을 설정하며, 요청이 실패한 후 재요청할 때 지연 시간을 얼마나 가질지 설정할 수 있습니다.</br>- 시간 단위는 ms(밀리세컨드)입니다.</br>- 이 값의 기본값은 (retryAttempt) => Math.min(1000 * 2 ** failureCount, 3000) 입니다. 실패 횟수 n에 따라 2의 n제곱 초 만큼 기다렸다가 재요청 합니다. 그리고 최대 30초까지 기다립니다.|
|staleTime|데이터의 유효 기간을 ms 단위로 설정합니다. 기본값은 0입니다.|
|cacheTime|데이터의 캐시 시간을 ms 단위로 설정합니다. 기본값은 5분입니다. 캐시 시간은 Hook을 사용하는 컴포넌트가 언마운트 되고 나서 해당 데이터를 얼마나 유지할지 결정 합니다.|
|refetchInterval|`false or number` 타입의 값을 설정하며, 이 설정으로 n초마다 데이터를 새로고침 하도록 설정할 수 있습니다. 시간단위는 ms입니다.|
|refetchOnmount|`boolean` `'always'` 타입의 값을 설정하며 이 설정으로 컴포넌트가 마운트 될때 재요청하는 방식을 설정할 수 있습니다. 기본값은 true 입니다.</br>- true일 때는 데이터가 유효하지 않을 때 재요청 합니다.</br>- false일 때는 컴포넌트가 다시 마운트 되어도 재요청 하지 않습니다.</br>- 'always'일 때는 데이터의 유효 여부와 관계없이 무조건 재요청 합니다.|
|onSucess|`(data:Data) => void` 타입의 함수를 설정 합니다. 데이터 요청이 성공하고 나서 해야할 일들을 입력해 줄수 있습니다.|
|onError|`(error:Error) => void` 타입의 함수를 설정 합니다. 데이터 요청이 실패하고 나서 해야할 일들을 입력해 줄수 있습니다.|
|onSettled|`(data?: Data, error?: Error) => void` 타입의 함수를 설정합니다. 데이터 요청의 성공 여부와 관계없이 요청이 끝나면 특정 함수를 호출 하도록 설정합니다.|
|initialData|`Data or () => Data` 타입의 값을 설정합니다. Hook에서 사용할 데이터의 초깃값을 지정하고 싶을때 사용합니다.|

https://tanstack.com/query/v5/docs/react/guides/queries

revalidatePath('/home) > home 폴더에 있는 캐시 전체 삭제
revalidateTag
