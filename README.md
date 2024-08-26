# Svelte

<p align="center"><img src="https://github.com/user-attachments/assets/a670c254-3eb0-4f59-a76d-dd3eeed29963" alt="svelte" width="200px"></p>

## Svelte란?

>스벨트(Svelte)는 자바스크립트 라이브러리 중 하나로, React보다 가볍고 Vue보다 쉽다는 장점을 내세운 최신 프론트엔드 기술이다.  
실제 DOM과 가상 DOM을 비교하여 변경 사항을 실제 DOM으로 업데이트하는 방식으로 작동하는 React나 Vue와 다르게  
Svelte는 이 과정을 생략하고 바로 실제 DOM을 반영한다. Svelte는 앱을 실행 시점(Run time)에서 해석하지 않고 빌드 시점에서 Vanilla JavaScript Bundle로 컴파일하여 속도가 빠르고 따로 라이브러리를 배포할 필요가 없어 간편하다.

***

## Svelte에서의 상태값(State)

1. 상태값과 같은 자바스크립트 코드는 스크립트 블록 안에 작성한다.
2. 상태값을 마크업 영역 표현 시 대괄호 안에 상태값 이름을 사용한다.
3. 상태값의 경우 기본이 되는 변수는 물론 배열, 객체, 객체의 배열 형태 모두 가능하다.  
(자바스크립트에서 사용할 수 있는 대부분의 데이터 형태가 가능하다.)

```js
// 기본 활용 방법
<script>
    let count = 0;

    function handleClick(){
        count += 1;
    }
</script>

<button>클릭 수 {count}</button>
```

```js
// 상태값의 형태
let count = 1; // 기본

let array = [1,2,3,4,5] // 배열

let objectState = { // 객체
    name: '오건희',
    age: 27,
    location: 'incheon'
}

let objectArray = [ // 객체의 배열
    {
        name: '오건희',
        age: 27,
        location: 'incheon'
    },
    {
        name: '홍길동',
        age: 30,
        location: 'seoul'
    },
]
```

## Reactivity (반응성)

> 스벨트에서는 ```$:``` 통해 리액티비티적인 기능을 구현할 수 있다.

**반응성 기호의 특징**

> 반응성 기호로 변수나 함수를 선언을 하면 대상이 되는 상태값의 변화를 자동으로 감지하여 필요한 이벤트를 실행할 수 있다.  

* 직접적인 콜 없이 자동으로 변화를 감지하고 대응하도록 하는 패턴 선언형 프로그램

* $: 기호로 선언된 변수의 값이 의존하는 다른 변수들 중 하나라도 변경될 때마다 자동으로 재계산 된다.

* 반응성 기호는 변수 뿐만 아니라 대괄호로 감싸서 특정 영역에 영향을 줄 수도 있다.

* 조건문과의 사용 또한 가능하다.

```js
<script>
    let count = 1;

    // 반응성 기호
    // 일반적인 상태값이 let으로 선언 되는 것과 다르게 doubled 변수는 반응성 기호로 선언 되어있다.
    // doubled는 count가 변동 될 때마다 이를 감지하고 값이 변동 된다.
    $: doubled = count * 2; 

    // 반응성 기호를 통해 의존 값이 변동 됨에 따라 조건문 또한 사용이 가능하다.
    $: if(count >= 10){
        alert('카운트가 10을 넘었습니다.');
        count = 9;
    }

    // 반응성 기호 안에 콘솔은 count와 doubled가 변동되면 로그를 찍어준다.
    $: {
        console.log(count);
        console.log(doubled);
    }

    function handleClick(){
        count += 1;
    }
</script>

<main>
    <button on:click={handleClick}>
        클릭수 {count} {count === 1 ? 'time' : 'times'}
    </button>

    <p>{count} 두배는 {doubled}</p>
</main>
```
***

## Svelte의 컴포넌트

> Svelte 또한 컴포넌트 기반으로 코드의 재사용성을 높혀서 사용이 가능하다.

* 컴포넌트 파일 분리 시 파일명은 대/소문자 둘 다 가능하다.
* 해당 컴포넌트를 불러올 때는 script 태그의 import를 사용한다.
* 불러온 컴포넌트의 이름을 정의할 때는 꼭 대문자로 시작한다.

```js
// components 폴더에 작성 된 Header.svelte 컴포넌트
<header>
    <h1>헤더 컴포넌트</h1>
</header>

<script>
    import Header from "./components/Header.svelte";
    import Content from "./components/Content.svelte";
    import Footer from "./components/Footer.svelte";
</script>

<main>
    <Header />
    <Content />
    <Footer />
</main>
```
***

## Svelte의 Props (컴포넌트 간 통신)

> Svelte도 컴포넌트 간의 Props의 전달 통해 상태값을 전달할 수 있다.

* 컴포넌트에 대괄호를 작성하고 그 안에 상태값의 이름을 작성하여 값을 넘길 수 있다.

* Props를 전달 받는 컴포넌트는 export let 으로 값을 받는다.

* Props는 단방향적인 성격을 갖기에 하위 컴포넌트에서 상위 컴포넌트의 상태값의 변경이 필요한 경우, 상위 컴포넌트에서 변경과 관련 된 메소드를 만들고 이를 전달한다.  
(단반향 바인딩)

```js
// App.svelte
<script>
    import Header from "./components/Header.svelte";
    import Content from "./components/Content.svelte";
    import Footer from "./components/Footer.svelte";
    import BtnCount from "./components/BtnCount.svelte";

    let count = 0;

    $: doubled = count * 2; 

    $: if(count >= 10){
        alert('카운트가 10을 넘었습니다.');
        count = 9;
    }

    function handleClick(){
        count += 1;
    }
</script>

<main>
    <Header />
    <BtnCount {count} {handleClick} />
    <Content {count} {doubled} />
    <Footer />
</main>
```

```js
// Content.svelte
<script>
    export let count;
    export let doubled;

    $: {
        console.log(count);
        console.log(doubled);
    }
</script>

<section>
    <p>{count} 두배는 {doubled}</p>
</section>
```

```js
// BtnCount.svelte
<script>
    export let count;
    export let handleClick;

    import LabelComp from "./LabelComp.svelte"
</script>

<button on:click={handleClick}>
    클릭수 {count} <LabelComp {count} />
</button>
```

```js
// LabelComp.svelte
<script>
    export let count;
</script>

{count <= 1 ? 'time' : 'times'}
```
***

## html 돔 제어

* **:one: 논리 블록**

    > ```{#if ...}``` 일반적인 프로그램의 if 와 비슷한 기능.  
    스크립트 영역이 아닌 html(마크업) 영역에서 상태 값에 따라 분기를 나누고 표현한다.

    **사용 방법**
    ```js
    {#if 조건}
        // 조건이 참일 때 내용
    {:else if 조건1}
        //  조건 1이 true일 때
    {:else if 조건2}
        //  조건 2이 true일 때
    {:else}
        //  조건이 false일 때 내용
    {/if}
    ```

    **활용**
    ```js
    <script>
        let auth = {
            loggedIn: false
        }

        const handleLogin = ()=> auth.loggedIn = true;
        const handleLogOUT = ()=> auth.loggedIn = false;
    </script>

    <main>
        <button on:click={handleLogin}>로그인</button>
        <button on:click={handleLogOUT}>로그아웃</button>

        {#if auth.loggedIn === true}
            <p>로그인 상태입니다.</p>
        {:else}
            <p>로그아웃 상태입니다.</p>
        {/if}
    </main>
    ```

* **:two: 반복 블록**

    > ```{#each ...}``` 일반적인 프로그램의 for 또는 each와 같은 반복문 기능으로 배열의 내용을 html 영역에서 반복적으로 나타내주는 역할을 한다.

    **사용 방법**
    ```js
    // each 작성 후 반복해서 작성할 배열 배치
    // as를 이용해 순서대로 들어오는 배열의 내용에 이름을 설정
    {#each datas as data}
        // 데이터가 객체일 경우 표현할 객체의 이름 작성
        {data.name}
    {/each}
    ```

    **활용**
    ```js
    <script>
        let todos = [
            {
                id:0,
                content: "첫 번째 할 일",
                done: false
            },
            {
                id:1,
                content: "두 번째 할 일",
                done: false
            },
            {
                id:2,
                content: "세 번째 할 일",
                done: true
            }
        ]

    </script>

    <main>
        <ul>
            {#each todos as todo}
                <li>
                    <span>{todo.id}</span>
                    <span>{todo.content}</span>
                    <span>{todo.done}</span>
                </li>
            {/each}
        </ul>
    </main>
    ```
***

## store (전역 상태 관리)

> 전역으로 사용 가능한 상태값 저장소.  
store를 통해 컴포넌트 간의 상태값을 더 간결히 제어하고 관리 할 수 있으며 이를 통해 Props Drilling 같은 문제를 방지한다.

1. store 파일 생성 (stores.js)

2. svelte store의 writable 모듈 가져오기

    > writable => 스토어 종류 중에서 가장 많이 사용되는 기본 형태의 스토어 모듈  
    **(읽기, 수정, 삭제가 모두 가능한 범용적인 스토어)**

    ```js
    import {writable} from "svelte/store";
    ```

3. 스토어 이름 정의 후 writable로 스토어 생성하기

    ```js
    export const sotreName = writable(초기값);
    ```

4. 외부에서 import를 통해 불러오기

    ```js
    <script>
        import { storeName } from "./스토어파일.js";
    </script>
    ```
### store 기본 사용법

1. stores.js 파일을 생성 후 전역으로 사용할 스토어를 선언하고 초기값을 정의한다.  
(만들어진 스토어는 외부에서의 사용을 위해 export 시켜준다.)

2. import를 이용해 사용할 스토어를 불러온다.

3. 스토어의 값을 변경하는 가장 기본적인 방법은 스토어 값을 변경 후 재할당 시키는 방법이다.

4. 스토어의 값의 경우 일반적인 상태값과 다르게 사용 시 꼭 ```$``` 기호를 앞에 붙여준다. 이때 스토어를 사용하기 위한 마크업 영역 또한 동일하게 사용해준다. (예시 $count)

    ```js
    // stores.js
    import {writable} from "svelte/store";

    export const count = writable(0);
    ```

    ```js
    // App.svelte
    <script>
        import CountResult from "./components/CountResult.svelte";
        import Decrement from "./components/Decrement.svelte";
        import Increment from "./components/Increment.svelte";
    </script>

    <main>
        <CountResult />
        <Increment />
        <Decrement />
    </main>
    ```

    ```js
    // Increment.svelte
    <script>
        import {count} from "../stores.js";

        const onIncre = ()=>{
            $count = $count + 1; // 값의 재할당
        }
    </script>

    <button on:click={onIncre}>+</button>
    ```

    ```js
    // CountResult.svelte
    <script>
        import {count} from "../stores.js";
    </script>

    <p>현재 count는 {$count} 입니다.</p> // html 사용
    ```

### store의 사용자 정의 메소드

> 스토어 내부에서 사용자 정의 메소드를 만들고 해당 메소드를 호출하는 방법  
사용자 정의 메소드를 사용시 오류를 줄이고 재사용이 가능해 더 효율적인 장점이 있다.

**writable 스토어의 기본 메소드**

* subscribe : 스토어의 값이 변경되면 자동으로 반영하는 역할
* set : 스토어의 값을 초기화하는 옵션
* update : set이 스토어 전체를 초기화 한다면 update는 값의 일부만 변경 시키는 역할  
(주로 커스텀 메소드를 만들 때 사용)

```js
const {subscribe, set, update} = writable(초기값);
```

**활용 예시**

* 사용자 정의 메소드와 함께 외부에서 사용할 writable의 기본 기능도 return에 포함한다. (subscribe)

* 외부에서 따로 조작이 필요한 경우 set 이나 update를 return할 경우도 있다.

```js
import {writable} from "svelte/store";

// 사용자 정의 메소드 제작
const createCount = ()=>{
    const {subscribe, set, update} = writable(0);

    const increment = () => update(count => count + 1); // update로 값 변경
    const decrement = () => update(count => count - 1); // update로 값 변경
    const reset = () => set(0); // reset을 이용한 초기화

    return { // 만들어진 사용자 정의 메소드 return
        subscribe, // 스토어의 값이 변경시 바로 적용
        increment,
        decrement,
        reset
    };
}

export const count = createCount(); // count에 연결
```

```js
// Increment.svelte
<script>
    import {count} from "../stores.js";

    const onIncre = ()=>{
        count.increment(); // 사용자 정의 메소드로 변경 $은 필요없다.
    }
</script>

<button on:click={onIncre}>+</button>
```
***