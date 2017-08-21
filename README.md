[원문:https://github.com/atlassian/react-beautiful-dnd](https://github.com/atlassian/react-beautiful-dnd)

# react-beautiful-dnd

아름답고 접근 용이한 Rreact([`React.js`](https://facebook.github.io/react/)) 리스트 드래그 앤 드랍

[![Build Status](https://travis-ci.org/atlassian/react-beautiful-dnd.svg?branch=master)](https://travis-ci.org/atlassian/react-beautiful-dnd) [![dependencies](https://david-dm.org/atlassian/react-beautiful-dnd.svg)](https://david-dm.org/atlassian/react-beautiful-dnd) [![SemVer](https://img.shields.io/badge/SemVer-2.0.0-brightgreen.svg)](http://semver.org/spec/v2.0.0.html)

![example](https://raw.githubusercontent.com/alexreardon/files/master/resources/dnd.small.gif?raw=true)

## Examples(예제) 🎉

얼마나 아름다운지 직접 보세요 - [have a play with the examples!](https://react-beautiful-dnd.netlify.com)

## Core characteristics(핵심 특징):

- 아름답고 자연스러운 아이템 이동
- 깔끔하고 강력한 API 를 통한 간단한 사용
- 독착정인 스타일
- 추가 DOM 생성이 필요 없음 - 친숙한 flexbox 와 focus 관리
- anchor 태그 등 기존 노드와 잘 작동
- 상태(state) 주도의 드래깅 - 이 부분은 프로그래밍(programatic) 방식으로 많은 input 타입에서 드래깅(dragging)을 허용 한다. 현재는 마우스 와 키보드 드래깅(dragging)만 지원합니다.

## Not for everyone(참고사항)

React를 사용한 많은 드래그 앤 드랍 라이브러리가 있습니다. 그중 가장 주목할 것은  [`react-dnd`](https://github.com/react-dnd/react-dnd) 입니다. 그것은 HTML5 드래그 앤 드랍 feature([wildly inconsistent](https://www.quirksmode.org/blog/archives/2009/09/the_html5_drag.html))과 훌륭하게 매치되는 드래그 앤 드랍을 지원 합니다. **`react-beautiful-dnd` 는 vertical 과 horizontal 리스트를 위해 특별히 high level로 추상화 되어있습니다**.
`react-beautiful-dnd`는 기능적인 subset으로 파워풀하고 자연스럽고 아름다운 드래그 앤 드랍을 제안합니다. 그러나, 그것은 react-dnd 처럼 많은 기능들을 제공하지 않습니다. 그래서 이 라이브러리가 당신의 경우에 맞지 않을 수 있습니다.

## Still young!(아직 젊습니다!)

이 라이브러리는 새것이기 때문에 상대적으로 작은 기능만 제공 합니다. 기달려 주세요! 빠르게 움직일 것입니다!

### Currently supported feature set(현재 지원하는 특징)

- 단일 vertical 리스트 아이템 드래깅
- 한 페이지 안의 멀티 독립 리스트
- 마우스 🐭 와 **키보드 🎹** 드래깅
- 가변 높이 아이템 (아이템은 서로 다른 높이를 가질수 있다.)
- 커스텀 드래그 핸들 (일부 아이템만 드래그 할 수 있다.)
- vertical 리스트는 scoll container가 될 수 있다. (스크롤 부모 컨테이너 없이) 혹은 자식이 scoll container가 될 수 있다. (역시 스크롤 가능한 부모 없다)

### Short term backlog(단기 backlog)

- horizontal 리스트 드래깅
- vertical 리스트 아이템 이동 (드랍할 수 있는 위치 까지)

### Medium term backlog(중기 backlog)

- horizontal 리스트 아이템 이동
- vertical 리스트 에서 horizontal 리스트로의 `드래그 이동`
- 한번에 여러 아이템 드래깅

### Long term backlog(장기 backlog)

- 터치 지원
- 프레임이 임계값 이하로 떨어질 경우 자동 애니메이션 비활성화.
- 사용자 입력 없는 프로그래밍 방식 드래깅
- 그리고 많은 것들!

## Basic usage example(기본 사용 예제)

이것은 간단한 재정렬 리스트 입니다. [You can play with it on webpackbin](https://www.webpackbin.com/bins/-Kr9aE9jnUeWlphY8wsw)

![basic example](https://github.com/alexreardon/files/blob/master/resources/dnd-basic-example-small.gif?raw=true)

```js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { DragDropContext, Droppable, Draggable } from 'react-beautiful-dnd';

// fake data generator(가짜 데이터 제너레이터)
const getItems = (count) => Array.from({length: count}, (v, k) => k).map(k => ({
  id: `item-${k}`,
  content: `item ${k}`
}));

// a little function to help us with reordering the result(결과 재정렬을 돕는 함수)
const reorder =  (list, startIndex, endIndex) => {
  const result = Array.from(list);
  const [removed] = result.splice(startIndex, 1);
  result.splice(endIndex, 0, removed);

  return result;
};

// using some little inline style helpers to make the app look okay(보기좋게 앱을 만드는 인라인 스타일 헬퍼)
const grid = 8;
const getItemStyle = (draggableStyle, isDragging) => ({
  // some basic styles to make the items look a bit nicer(아이템을 보기 좋게 만드는 몇 가지 기본 스타일)
  userSelect: 'none',
  padding: grid * 2,
  marginBottom: grid,

  // change background colour if dragging(드래깅시 배경색 변경)
  background: isDragging ? 'lightgreen' : 'grey',

  // styles we need to apply on draggables(드래그에 필요한 스타일 적용)
  ...draggableStyle
});
const getListStyle = (isDraggingOver) => ({
  background: isDraggingOver ? 'lightblue' : 'lightgrey',
  padding: grid,
  width: 250
});

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      items: getItems(10)
    }
    this.onDragEnd = this.onDragEnd.bind(this);
  }

  onDragEnd (result) {
    // dropped outside the list(리스트 밖으로 드랍한 경우)
    if(!result.destination) {
      return;
    }

    const items = reorder(
      this.state.items,
      result.source.index,
      result.destination.index
    );

    this.setState({
      items
    });
  }

  // Normally you would want to split things out into separate components.(일반적으로 당신은 컴포넌트를 나눌 것입니다.)
  // But in this example everything is just done in one place for simplicity(그러나 예제를 간단하게 하기 위해 한곳에 적용했습니다.)
  render() {
    return (
      <DragDropContext onDragEnd={this.onDragEnd}>
        <Droppable droppableId="droppable">
          {(provided, snapshot) => (
            <div
              ref={provided.innerRef}
              style={getListStyle(snapshot.isDraggingOver)}
            >
              {this.state.items.map(item => (
                <Draggable
                  key={item.id}
                  draggableId={item.id}
                >
                  {(provided, snapshot) => (
                    <div>
                      <div
                        ref={provided.innerRef}
                        style={getItemStyle(
                          provided.draggableStyle,
                          snapshot.isDragging
                        )}
                        {...provided.dragHandleProps}
                      >
                        {item.content}
                      </div>
                      {provided.placeholder}
                    </div>
                  )}
                </Draggable>
              ))}
            </div>
          )}
        </Droppable>
      </DragDropContext>
    );
  }
}

// Put the thing into the DOM!(DOM 에 앱 적용)
ReactDOM.render(<App />, document.getElementById('app'));
```

### Physicality(물리적)

`react-beautiful-dnd`의 핵심 디자인은 물리적입니다: 우리는 사용자가 그들이 물리적 오브젝트를 움직인다고 느끼길 원합니다.

#### Application 1: no instant movement(즉각적인 움직임이 없습니다.)

이것은 사용자 드래그에 반응하는 것은 표준 드래그 앤 드랍 패턴입니다. 보다운자연스러운 드래그 애니메이션을 위해 명확한 드래그 효과를 보여줍니다. 우리는 역시 새로운 위치로 아이템을 드랍할수 있게 합니다. 아무 곳으로나 아이템을 이동할 수는 없습니다. 드래그 하고 말고는 관련 없습니다.

#### Application 2: knowing when to move(언제 움직일까요.)

드래그 앤 드랍 인터렉션은 사용자가 드래그 시작한 곳부터 시작하는 것이 일반적입니다.

`react-beautiful-dnd`의 아이템 드래깅 효과는 아이템의 중력 효과를 기본으로 합니다. - 당신이 아이템을 어디에서 잡았는지는 관계 없습니다. 아이템 드래깅 효과는 스케일 설정⚖️과 유사한 규칙을 따릅니다. 가변 높이 아이템의 자연스러운 드래그를 위해 몇 가지 규칙을 허용 합니다.

- 리스트는 리스트의 바운더리까지 아이템 *드래깅 가능*합니다.
- 드래그 아이템의 중심 포지션이 아이템의 경게로 드래그 되면 나머지 아이템들은 움직이게 됩니다. 다르게 말하자면: (A) 아이템의 중심 포지션이 끝으로 이동되면 다른 아이템 (B)는 움직이게 됩니다.

#### Application 3: no drop shadows(드랍 쉐도우는 없습니다.)

드랍 쉐도우는 아이템과 그것의 목표가 바뀌는 환경에서 유용합니다. 그러나 `react-beautiful-dnd`는 어디에 아이템이 드랍될지가 명확해야 합니다. 이것은 추후에 변경될 것입니다. - 그러나 이런 효과 없이 어디 까지 볼수 있을지 실험해 봐야 합니다.

#### Application 4: maximise interactivity(쌍방향 극대화)

`react-beautiful-dnd`는 최대한 인터렉티브가 불가능한 기간을 피하려고 노력합니다. 그래서 사용자는 애니메이션이 끝나기를 기다릴 필요 없이 지속적으로 인터페이스를 통해 인터렉션 할 수 있습니다. 그러나, 모두가 분별할 수 있게 정확성과 힘 사이에서 균형을 유지해야 합니다. 여기에 인터렉티브 하지 않은 몇 가지 상황이 있습니다.

1. 사용자가 드랍 애니메이션을 완료할 때 드래그를 취소할 경우, 그들을 최대한 되돌립니다. 올바르지 않은 위치에 드래그를 하면 안됩니다.
2. 드랍되는 아이템이 드래그 됩니다. 간단하게 이 경우 입니다 - 원래 위치로 움직이고 있는 아이템을 잡기는 엄청 힘듭니다. 이것은 코딩 가능합니다 - 그러나 많은 복잡한 케이스가 있을 것입니다.

항상 비활성 기간이 존재하는 것은 아닙니다.

#### Application 5: no drag axis locking(드래그 축 잠금 없음)

지금 까지 이 라이브러리는 드래그 축 자금을 지원하지 않았습니다. (드래그 레일스(rails)라고도 불립니다.) 이것은 사용자가 한 축으로만 드래그 가능하도록 제안하는 것입니다. 현재 생각으로는 이것은 물리적 비유를 헤칩니다. 우리는 물린적인 물체를 움직이는 것이 아니라 메세지를 사용자에게 보내면서 상호작용 합니다. 그것은 사용자가 `type` 과 `isDropEnable` props를 사용하여 하나의 리스트에만 드랍할 수 있도록 가능합니다. 당신은 `onDragStart` 리스트 시각적으로 처리하여 인터렉트할 수 있는 유일한 장소임을 보여줄 수 있습니다.

### Sloppy clicks and click blocking(엉성한 클릭 및 클릭 차단) 🐱🎁

사용자가 엘리멘트에 마우스 다운을 누를 경우, 우리는 사용자가 클릭을 한 건지 드래그를 하는 건지 알 수 없습니다. 때때로 사용자가 클릭할 때 커서를 약간 움직이는 경우도 있습니다. - 엉성한 클릭. 그래서 우리는 일정한 거리를 마우스 다운과 함께 움직일 경우 드래그를 시작합니다. - 엉성한 클릭을 만드는 것보다 낫습니다. 만약 드래그 한계시점을 넘기지 않게 되면 사용자 인터렉션은 일반적인 클릭으로 작동합니다. 만약 드래그 한계시점이 지나면 인터렉션은 드래그 되고 기본 클릭 액션은 일어나지 않습니다.

이것은 사용자가 인터렉티브 엘리번트를 래핑할 수 있게 하며 자연스러운 방식으로 기본 앵커뿐와 드래그 가능한 아이템을 가질 수 있게 합니다.

(🐱🎁 is a [schrodinger's cat](https://www.youtube.com/watch?v=IOYyCHGWJq4) joke)

### Focus management(포커스 관리)

`react-beautiful-dnd`은 어떤 래퍼 엘리먼트도 생성하지 않습니다. 즉 문서의 일반적인 탭 흐름에 영향을 미치지 않습니다. 예를 들어 *anchor(앵커)* 태그를 래핑하는 경우 사용자는 *anchor(앵커)*를 둘러싼 요소가 아닌 앵커에 직접 탭을 겁니다. 당신이 어떤 엘리먼트에 래핑을 하던지 키보드 드래깅을 위해 `tab-index` 가 주어집니다.

### Accessibility(접근성)

전통적으로 드래그 앤 드랍 인터렉션은 오직 마우스와 터치 인터렉션만 있습니다. 이 라이브러리는 **using only a keyboard(오직 키보드를 통한)** 드래그 인터렉션도 지원합니다. 이를 통해 고급 사용자는 키보드를 통해 드래그 앤 드랍을 경험할 수 있습니다. 또한 이전에 제외 되었던 사용자들에게도 제공할수 있게 됩니다.

키보드 지원 외에도 우리는 표준 브라우저 키보드 인터렉션 방식으로 키보드 단축키를 검사 했습니다. 사용자가 드래그 하지 않을 경우 사용자는 보통 키보드를 사용합니다. 드래그 하는 동안 우리는 브라우저 단축키(`tab`과 같은)를 무시하고 비활성하여 사용자에게 부드러운 경험을 제공합니다.

#### Shortcuts(단축키)

현재는 키보드 처리가 하드 코딩되어 있습니다. 이것은 추후에 커스터마이징할 수 있도록 수정될 것입니다. 이것이 현재의 키보드 매핑입니다.:

- **tab** <kbd>tab ↹</kbd> - 표준 브라우저 탭은 `Droppable` 네비게이트 합니다. 이 라이브러리는 사용자가 `tab`을 선택하는 동안 어떤 화려한 동작도 하지 않습니다. 한번 드래그를 시작하면 드래그 지속을 위해 `tab`은 블락 됩니다.
- **spacebar** <kbd>space</kbd> - 포커스된 `Draggable`을 들어 올립니다. 또한 `spacebar`로 드래그가 시작된 곳에서 드래그 하는 `Draggable`을 드랍합니다.
- **Up arrow** <kbd>↑</kbd> - vertical 리스트에서 `Draggable`을 위로 이동합니다.
- **Down arroa** <kbd>↓</kbd> - vertical 리스트에서 `Draggable`을 아래로 이동합니다.
- **Escape** <kbd>esc</kbd> - 드래그 중인 것을 취소 합니다. - 사용자가 키보드 또는 마우스로 드래그 하고 있는것과 관계 없이.

#### Limitations of keyboard dragging(키보드 드래그시 제안 사항)

현재 키보드 드래그의 제안 사항 입니다: **사용자가 윈도우를 스크롤하면 드래그가 취소 됩니다**. 이것은 일어날 수 있습니다. 지금으로서는 가장 기본적인 접근 방법입니다.

## Carefully designed animations(조심스럽게 디자인된 애니메이션)

애니메이션을 통해 많은 것을 움직일때 사용자는 산만해 지며 방해가 됩니다. 우리는 밸런스와 인터랙션 퍼포먼스를 보장하기 위해 많은 애니메이션을 수정하였습니다.

#### Dropping(드랍)

당신이 움직이는 아이템을 드랍할 때의 움직임은 물리학을 기초로 하고 있습니다 (thanks [`react-motion`](https://github.com/chenglou/react-motion)). 결과적으로 드랍 느낌이 더 가중되고 물리적으로 나타납니다.

#### Moving out of the way(움직임)

드래그 하는 아이템의 움직임은 CSS transition으로 표현하는 것이 물리적인 것보다 좋습니다. 이것은 GPU으로 움직임을 핸들하며 퍼포먼스를 극대화할 수 있습니다. CSS 애니메이션 곡선은 방해받지 않고 드래그 하는 아이템이 밖으로 움직이면 CSS transition 이 물리적 인 좋습니다. 이것은 GPU로 움직임을 핸들하게 하면서 퍼포먼스를 극대화 합니다. CSS 애니메이션 곡선은 커뮤니케이션 할 수 있도록 설계 되어 있습니다.

그것은 이렇게 구성되었습니다:

1. 자연스러운 응답 속도로 보이는 준비 시간
2. 빠르게 움직일수 있는 작은 단계
3. 긴 후반부 그래서 사람들은 후반부에나 애니메이션 된 텍스트를 읽을 수 있다

![animation curve(애니메이션 곡선))](https://raw.githubusercontent.com/alexreardon/files/master/resources/dnd-ease-in-out-small.png?raw=true)
> 애니메이션 곡선는 움직일때 사용된다.

## Installation(설치)

```bash
# yarn
yarn add react-beautiful-dnd

# npm
npm install react-beautiful-dnd --save
```

## API

그래서 어떻게 라이브러리를 사용할까요?

## `DragDropContext`

드래그 앤 드롭을 사용하기 위해서 당신은 랩핑되어 드래그 앤 드랍을 할 수 있는 `DragDropContext` `React` 트리가 필요합니다. 그것은 `DragDropContext`으로 전체 애플리케이션을 랩하는 것이 좋습니다. 중첩(nested) `DragDropContext`는 *지원되지 않습니다*. 당신은 `Droppable` 과 `Draggable` props 를 사용하여 조건부 드래그 드랍을 할수 있습니다. `DragDropContext`는 [react-redux Provider component](https://github.com/reactjs/react-redux/blob/master/docs/api.md#provider-store)와 비슷한 용도라고 보시면 됩니다.

### Prop type information(Prop 타입 정보)

```js
type Hooks = {|
  onDragStart?: (id: DraggableId, location: DraggableLocation) => void,
  onDragEnd: (result: DropResult) => void,
|}

type Props = Hooks & {|
  children?: ReactElement,
|}
```

### Basic usage(기본 사용법)

```js
import { DragDropContext } from 'react-beautiful-dnd';

class App extends React.Component {
  onDragStart = () => {...}
  onDragEnd = () => {...}

  render() {
    return (
      <DragDropContext
        onDragStart={this.onDragStart}
        onDragEnd={this.onDragEnd}
      >
        <div>Hello world</div>
      </DragDropContext>
    )
  }
}
```

### `Hook`s

당신의 스테이트를 수정할 수 있는 많은 탑 레벨 어플리케이션 이벤트가 있습니다.

### `onDragStart` (optional)

이 함수는 드래그를 시작할 때 알림받을 수 있습니다. 당신은 아래 사항들을 제공받게 됩니다.

- `id`: 현재 드래그중인 `Draggable`의 id.
- `location`: location은 (`droppableId` 와 `index`) `Droppable`로 시작된 드래그 아이템의 위치입니다.

그것은 당신이 드래그 하는 동안 이 함수를 사용하는 모든 `Draggable` 과 `Droppable` 컴포넌트의 업데이트를 차단하는 것이 **highly recommended(강력히 추천)** 됩니다. (*Best `hooks` practices* 를 보세요.)

**Type information(타입 정보)**

```js
onDragStart?: (id: DraggableId, location: DraggableLocation) => void

// supporting types(지원하는 타입)
type Id = string;
type DroppableId: Id;
type DraggableId: Id;
type DraggableLocation = {|
  droppableId: DroppableId,
  // the position of the draggable within a droppable(droppable 내에서 droppable 의 위치)
  index: number
|};
```

### `onDragEnd` (required 필수)

이 함수는 *엄청* 중요하며 애플리케이션의 라이프사이클에서 lifecycle 위험한 역할을 담당하고 있습니다. **이 함수는 반드시 `Draggables` 리스트의 *동기* 방식으로 재정렬해야 합니다.**

그것은 드래그에 대한 모든 정보가 제공됩니다:

### `result: DragResult`

- `result.draggableId`: 드래그한 `Draggable`의 id.
- `result.source`: `Draggable` 이 시작된 위치(location).
- `result.destination`: `Draggable`이 끝난 위치(location). 만약에 `Draggable`이 시작한 위치와 같은 위치로 돌아오면 이 `destination`값은 `null`이 될것입니다.

### Synchronous reordering(동기방식 재정렬)

왜냐하면 이 라이브러리는 당신의 state를 제어할 수 없기 때문에 `결과`에 따라 *동기식* 결과를 재정렬 하는 것은 당신에 달려있습니다.

*여기에 당신이 사용해야 하는것이 있습니다.!*
- 만약 `destination`이 `null`이면: 모두 완료!
- 만약 `source.droppableId` 가 `destination.droppableId`와 같으면 당신은 당신의 리스트에서 아이템을 제거하고 올바른 위치에 삽입해야 합니다.
- 만약 `source.droppableId`가 `destination.droppable`과 다르면 당신은 `source.droppableId` 리스트에서 `Draggable` 하고 그것을 `destination.droppableId` 리스트의 올바른 위치에 추가해야 합니다.

### Type information(타입 정보)

```js
onDragEnd: (result: DropResult) => void

// supporting types(지원 타입)
type DropResult = {|
  draggableId: DraggableId,
  source: DraggableLocation,
  // may not have any destination (drag to nowhere)(어떤 destination 도 없을 것입니다.(어디로도 드래그 되지 않습니다.))
  destination: ?DraggableLocation
|}

type Id = string;
type DroppableId: Id;
type DraggableId: Id;
type DraggableLocation = {|
  droppableId: DroppableId,
  // the position of the droppable within a droppable(droppable 동안 droppable의 position)
  index: number
|};
```

### Best practices for `hooks` (`hooks`을 위한 최고의 방법)

**Block updates during a drag(드래그 하는 동안 업데이트를 블락 하세요)**

사용자가 드래깅 하는 동안 `Draggable`과 `Droppable` 혹은 치수의 결과에 영향을 줄 수 있는 모든 업데이트를 차단하는 것을 **강력히** 추천합니다. `onDragStart` 에서 `onDragEnd` 까지 `Draggable` 과 `Droppable` 의 업데이트를 차단해 주세요.

사용자가 드래깅을 시작할 때 우리는 애플리케이션의 `Draggable` 과 `Droppable` 노드 차수의 모든 스냅샷을 얻을 수 있습니다. 만약 드래그 하는 동안 그것들이 변경되면 우리는 그것에 대해 모를 것 입니다.

여기에 당신이 *드래그 하는 동안* 변경을 해서 발생할 수 있는 몇 가지 부족한 사용자 경험이 있습니다:

- 당신이 노드의 수를 늘리면 라이브러리는 그것에 대해 알지 못하게 되며 사용자가 예상하는데로 동작하지 않게 됩니다.
- 당신이 노드의 수를 줄이면 당신의 리스트에서 예상하지 못한 움직임과 차이가 생기게 됩니다.
- 당신이 노드의 크기를 변경하게 되면 그것은 변경된 노드와 다른 노드 모두 잘못된 시간에 이동하게 됩니다.
- 당신이 드래그 노드의 크기를 변경하게 되면 정확한 시간에 다른 노드들이 움직이지 않게 됩니다.

**`onDragStart` and `onDragEnd` pairing(쌍)**

우리는 `onDragStart` 이벤트가 단일 `onDragEnd` 이벤트와 쌍을 이루는 것을 보장합니다. 그러나 이것이 틀린 경우가 있습니다. - 그것은 버그 입니다. 현재 외부에서 드래그를 취소할 메커니즘이 없습니다..

**Style(스타일)**

드래그 중에 두가지 스타일을 바디(body)에 추가하는을것을 추천 합니다.

1. `user-select: none;` 그리고
2. `cursor: grab;` (혹은 드래그 중에 당신이 원하는 커서(cursor))

`user-select: none;` 텍스트를 드래그하지 못하게 합니다.

`cursor: [your desired cursor];` 은 필요합니다. 왜냐하면 `pointer-events: none;` 이 드래이 아이템에 적용되기 때문이다. 이렇게 되면 `snapshot.isDragging`을 기반으로 드래그에 당신의 커서를 설정하지 못하게 된다. (see `Draggable`).

**Dynamic hooks**

당신의 *hook* 함수는 *오직 시작할 때*만 캡처 된다. 그렇기 때문에 그 후에 이 함수를 변경하면 안됩니다. 만약 합당한 경우가 있게되면 훅을 지원할 것입니다. 그러나 지금은 아닙니다.

## `Droppable`
`Droppable` 컴포넌트는 `Draggable`에 의해 **드랍(dropped)**될 수 있습니다. 또한 `Draggable`을 **포함** 합니다.. `Draggable`은 반드시 `Droppable`안에 포함되어야 합니다.

```js
import { Droppable } from 'react-beautiful-dnd';

<Droppable
  droppableId="droppable-1"
  type="PERSON"
>
  {(provided, snapshot) => (
    <div
      ref={provided.innerRef}
      style={{backgroundColor: snapshot.isDraggingOver ? 'blue' : 'grey'}}
    >
      I am a droppable!
    </div>
  )}
</Droppable>
```

### Props

- `droppableId`: *필수* `DroppableId(string)`, 애플리케이션에 대한 드랍 가능 여부를 식별하는 유니크 식별자 입니다. 이 prop는 수정하지 마세요 - 특히 드래그 중
- `type`: *옵션* `TypeId(string)`, `Draggable` 클래스를 받기 위기 사용 됩니다. 예를 들어, `PERSON`을 사용하면 `PERSON`타입의 `Draggable`만 드랍될 수 있습니다. `TASK`타입 `Draggable`은 `PERSON` `Droppable`에 드랍될 수 없습니다. 만약 `type`이 제공되지 않으면 그것은 `DEFAULT`로 설정됩니다. 현재 `Droppable` 중에 `Draggable`의 의type`은 **반드시 같아야 합니다**. 만약 필요한 경우가 생기면 이 제한은 느슨해질 수 있습니다.
- `isDropDisabled`: *옵션*, `Droppable`에 드랍이 허용되는지 제어하는 플래그 입니다. 당신의 조건부 드랍 로직을 구현할 수 있습니다. 기본값은 `false`입니다.

### Children function(자식 함수)

`Droppable`의 `React` 자식들은 반드시 `ReactElement`를 반환하는 함수여야 합니다.

```js
<Droppable droppableId="droppable-1">
  {(provided, snapshot) => (
    // ...
  )}
</Droppable>
```

이 함수는 두 argument를 제공합니다.

**1. provided: (Provided)**

```js
type Provided = {|
  innerRef: (HTMLElement) => void,
|}
```
droppable이 올바르게 작동하려면, **당신은 반드시** `provided.innerRef`를 `ReactElement`의 최상단 DOM 노드에 바인드(bind)해야 합니다. 우리는 `ReactDOM`를 사용하지 않기 위해 당신의 DOM 노드를 찾을 것입니다.

```js
<Droppable droppableId="droppable-1">
  {(provided, snapshot) => (
    <div ref={provided.innerRef}>
      Good to go
    </div>
  )}
</Droppable>
```

**2. snapshot: (StateSnapshot)**

```js
type StateSnapshot = {|
  isDraggingOver: boolean,
|}
```

또한 `children` 함수는 현재 드래그 state와 관련된 몇 가지 state를 제공 합니다. 이것은 당신의 컴포넌트를 향상시키기 위해 사용될 수 있습니다(옵션). 일반적으로 상요 하는 경우는 드래그 중에 `Droppable` 모양을 변경하기 위해 사용 합니다.

```js
<Droppable droppableId="droppable-1">
  {(provided, snapshot) => (
    <div
      ref={provided.innerRef}
      style={{backgroundColor: snapshot.isDraggingOver ? 'blue' : 'grey'}}
    >
      I am a droppable!
    </div>
  )}
</Droppable>
```

### Conditionally dropping(조건부 드랍)

> 유의 하세요. 이번에 이것은 지원되지 않습니다. 현재 초기 버전에서는 오직 단일 리스트의 재정렬만 지원됩니다.

- `Droppable`은 오직 같은 `type`을 공유하는 `Draggable`로만 드랍될수 있습니다. 이것은 조건부 드랍을 허용하는 간단한 방법입니다. 만약 당신이 `Draggable`을 위한 `type`을 제공하지 않는다면 그것은 default type을 가진 `Draggable`만 허용할 것입니다. `Draggable` 과 `Droppable`에 어떤 `type`도 제공하지 않을 경우 `DEFAULT`로 설정 됩니다. 현재 다수의 'type' 혹은 `Draggable`에 와일드카드(wildcard) `type`을 설정하는 방법은 없습니다. 이것은 필요한 경우가 생기면 추가될 것입니다.
- `isDropDisabled` prop 을 사용해서 조건적으로 드랍을 허용할 수 있습니다. 이것은 당신 임의로 조건 수행을 할수 있는것을 허용합니다. 이것은 `Droppable`의 `type`이 현재 드랍되고 있는 `Draggable`의 `type`과 같을 경우 고려할 수 있습니다. 
- `isDropDisabled`를 false로 설정하면서 `Droppable`에 드랍되는 것을 막을 수 있습니다. 이렇게 하면 생성한 리스트에 절대 드랍할수 없지만 `Draggable`을 포함할 수 있습니다.
- 기술적으로 당신은 `type`을 사용할 필요 없으며 `isDropDisabled` 함수로 모든 조건부 드랍 로직을 수행할 수 있습니다. `type` 파라미터는 일반적인 경우에 간단한 방법입니다.

### Scroll containers(scroll container)

이 라이브러리는 scroll container 내에서의 드래그를 지원 합니다. (DOM 엘리먼트는 `overflow: auto;` 혹은 `overflow: scroll;`를 가집니다.) **유일 하게** 지원 되는 경우는 다음과 같습니다:

1. `Droppable` 자체가 **부모 스크롤이 없는** scroll container일 수 있습니다.
2. `Droppable`이 **하나의 스크롤할 수 있는 부모**를 가지고 있습니다.*

**Auto scrolling is not provided(auto scroll은 제공되지 않습니다)**

현재 scroll container의 auto scroll은 이 라이브러리의 영역이 아닙니다. auto scroll은 container가 scroll container의 끝 근처로 드래그 할때 스스로가 아이템 드래그를 위한 공간을 만듭니다. 당신은 auto scroll 리스트를 만들거나 만약 당신이 필요하면 우리는 auto scroll `Droppable`을 제공할 수 있습니다.

사용자들은 그들의 트랙패드 혹은 마우스 휠로 드래그 중에 scroll container를 스크롤 할 수 있습니다.

**Keyboard dragging limitation(키보드 드래그 제한)**

scroll container로 작업하기 위해 키보드 드래그를 가져오는 것은 상당히 어렵 습니다. 현재 여기에는 제한이 있습니다. 당신은 키보드로 scroll container 모서리를 넘어서 드래그를 할 수 없습니다. 이 제한은 autu scrolling 이 소개되면 없어질 것입니다.

## `Draggable`

`Draggable` 컴포넌트는 `Droppable`에 드래그 및 드래그 할 수 있습니다. `Draggable`은 반드시 `Droppable`을 포함해야 합니다. 이것은 **가능한** 그것의 `Droppable`이 다른 `Droppable`로 움직이는 중에 `Draggable`을 재정렬 할 것입니다. 그것은 **가능한** 입니다. 왜냐하면 `Droppable`은 그것에 드랍되는 것을 컨트롤 하는 것에서 자유롭기 때문입니다.

> 주의: `Droppable`들 사이에서 이동하는 것은 이 초기버전에서 지원하지 않습니다.

```js
import { Draggable } from 'react-beautiful-dnd';

<Draggable
  draggableId="draggable-1"
  type="PERSON"
>
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
        {...provided.dragHandleProps}
      >
        <h4>My draggable</h4>
      </div>
      {provided.placeholder}
    </div>
  )}
</Draggable>
```

> 주의: 이 라이브러리는 Reac가 16 버전이 될 경우 랩핑 엘리먼트를 만들 필요 없이 당신의 자식 함수의 sibling으로 placeholder`를 반환할 수 있기 때문에 조금 정리될 수 있을 것입니다.

### Props

- `draggableId`: *필수* `DraggableId(string)`, 애플리케이션을 위한 `Draggable`을 고유하게 식별합니다. 이 prop를 수정하지 마세요. - 특히 드래그 중에.
- `type`: *옵션* `Draggable`의 type (`TypeId(string)`), 이것은 `Droppable`의 `Draggable`이 드랍을 허용하기 위해 사용됩니다. `Draggable`은 오직 `Droppable`과 같은 `type`을 공유할 경우 드랍됩니다. 만약 `type`이 제공되지 않으면 기본으로 `'DEFAULT'`로 설정됩니다. 현재 `Draggable`의 `type`은 **반드시** 그것의 `Droppable` container의 `type`과 같아야 합니다. 이 제한은 추후 사용이 필요한 경우가 생기면 느슨해질 수 있습니다.
- `isDragDisabled`: *옵션* `Draggable`이 드래그를 허용할 수 있는지 여부를 제어하는 플래그 입니다. 당신의 드래그 제어 로직을 사용할 수 있습니다. 기본값은 `false`입니다.

### Children function(자식 함수)

`Draggable`의 `React` 자식들은 반드시 `ReactElement`를 반환하는 함수여야 합니다.

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
        {...provided.dragHandleProps}
      >
        Drag me!
      </div>
      {provided.placeholder}
    </div>
  )}
</Draggable>
```

이 함수는 두 argument를 제공합니다:

**1. provided: (Provided)**

```js
type Provided = {|
  innerRef: (HTMLElement) => void,
  draggableStyle: ?DraggableStyle,
  dragHandleProps: ?DragHandleProvided,
  placeholder: ?ReactElement,
|}
```

모든 *제공된* 오브젝트는 `Draggable`이 올바르게 적용되어야 합니다.

- `provided.innerRef (innerRef: (HTMLElement) => void)`: `Droppable`이 올바르게 작동하려면, **당신은 반드시** `innerRef` 함수를 당신이 원하는 `Draggable` 노드의 `ReactElement`에 바인딩 해야 합니다. 우리는 `ReactDOM`를 사용할 필요 없도록 하기 위해 당신의 DOM 노드를 찾을 것입니다.

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div ref={provided.innerRef}>
      Drag me!
    </div>
  )}
</Draggable>
```

**Type information(타입 정보)**

```js
innerRef: (HTMLElement) => void
```

- `provided.draggableStyle (?DraggableStyle)`: 이것은 `Object` 혹은 `null` 입니다. 이것은 `Draggable` 적용될 필요가 있는 다 수의 스타일을 포함합니다. 이것은 `provided.innerRef`를 적용한 동일한 노드에 적용해야 합니다. 이것은 그것이 드래그 중이나 드래그 중이지 않거나 draggable의 움직임을 제어합니다. 당신의 스타일을 이 오브젝트에 추가하세요. - 그러나 어떤 properties도 삭제하거나 수정하지 마세요.

**Ownership(소유권)**

드래그 엘리먼트의 위치 로직을 얻는 것은 이 라이브러리에 명세되어 있습니다. 이것은 `top`, `right`, `bottom`, `left` 그리고 `transform`과 같은 프로퍼티를 포함합니다. 이 라이브러리는 그것의 위치를 변경할 수 있으며 그것은 주요 버전의 퍼포먼스 문제 없이 사용할 수 있습니다. 그것은 역시 드래그 엘리먼트에 `transition` 프로퍼티를 적용하지 않을 것을 추천합니다.

**Warning: `position: fixed`**
 
 `react-beautiful-dnd`는 드래그 엘리먼트의 포지션에 `position: fixed`를 사용합니다. 이것은 확실히 탄탄하며 당신이 `position: relative | absolute | fixed` 부모를 가지는 것을 허용 합니다. 그러나 불해하게도 `position:fixed` 는 [`transform`에 의해 영향 받습니다](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/) (`transform: rotate(10deg);`와 같이). 이 의미는 당신이 만약 `Draggable`의 부모 중 하나가 `transform: *`를 가진다면 드래그 중에 포지션 로직은 맞지 않을 것입니다. 많은 사용자를 위해 이것은 이슈입니다. 그래도 그것을 제자리에 두기 보다는 포털 솔루션에 드래그 엘리먼트를 붙이는 것이 더 낫습니다. 그러나 제자리에 두는것은 모두를 위해서 좋은 경험(experience)입니다. 지금 우리는 그것 그대로 둘 것입니다. 그러나 이것이 당신에게 중요하다면 마음놓고 이슈를 제기해 주세요.
 
 **Usage of `draggableStyle`**

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
      >
        Drag me!
      </div>
    </div>
  )}
</Draggable>
```

**Extending with your own styles(당신의 스타일을 확장)**

```js
<Draggable draggable="draggable-1">
  {(provided, snapshot) => {
    const style = {
      ...provided.draggableStyle,
      backgroundColor: snapshot.isDragging : 'blue' : 'white',
      fontSize: 18,
    }
    return (
      <div>
        <div
          ref={provided.innerRef}
          style={style}
        >
          Drag me!
        </div>
      </div>
    );
  }}
</Draggable>
```

**Type information(타입 정보)**

```js
type DraggableStyle = DraggingStyle | NotDraggingStyle;

type DraggingStyle = {|
  position: 'fixed',
  boxSizing: 'border-box',
  // allow scrolling of the element behind the dragging element
  pointerEvents: 'none',
  zIndex: ZIndex,
  width: number,
  height: number,
  top: number,
  left: number,
  transform: ?string,
|}

type NotDraggingStyle = {|
  transition: ?string,
  transform: ?string,
  pointerEvents: 'none' | 'auto',
|}
```

- `provided.placeholder (?ReactElement)` The `Draggable` element has `position: fixed` applied to it while it is dragging. The role of the `placeholder` is to sit in the place that the `Draggable` was during a drag. It is needed to stop the `Droppable` list from collapsing when you drag. It is advised to render it as a sibling to the `Draggable` node. When the library moves to `React` 16 the `placeholder` will be removed from api.

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
      >
        Drag me!
      </div>
      {/* Always render me - I will be null if not required */}
      {provided.placeholder}
    </div>
  )}
</Draggable>
```

- `provided.dragHandleProps (?DragHandleProps)` 모든 `Draggable`은 *drag handle*을 가집니다. 이것은 전체 `Draggable`을 드래그 하기 위해 사용됩니다. 가끔 이것은 `Draggable`과 같은 노드일 수 있습니다. 그러나 때때로 그것은 `Draggable`의 자식이 될 수 있습니다. `DragHandleProps`는 당신이 드래그 핸들이 되고자 하는 노드에 적용될 필요가 있습니다. 이것은 `Draggable` 노드에 적용될 필요가 있는 다수의 prop 입니다. 이것은 간단하게 draggable node에 뿌릴(spread) 수 있습니다. (`{...provided.dragHandleProps}`). 그러나 만약 당신이 그것들의 응답이 필요하면 당신은 역시 [monkey patch](https://davidwalsh.name/monkey-patching)의 prop를 환영할 것입니다. DragHandleProps 역시 `isDragDisabled`가 `true`로 설정될 경우 `null`일 것입니다.

**Type information(타입 정보)**

```js
type DragHandleProps = {|
  onMouseDown: (event: MouseEvent) => void,
  onKeyDown: (event: KeyboardEvent) => void,
  onClick: (event: MouseEvent) => void,
  tabIndex: number,
  'aria-grabbed': boolean,
  draggable: boolean,
  onDragStart: () => void,
  onDrop: () => void
|}
```

**Standard example(기본 예제)**

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
        {...provided.dragHandleProps}
      >
        Drag me!
      </div>
      {provided.placeholder}
    </div>
  )}
</Draggable>
```

**Custom drag handle**

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => (
    <div>
      <div
        ref={provided.innerRef}
        style={provided.draggableStyle}
      >
        <h2>Hello there</h2>
        <div {...provided.dragHandleProps}>
          Drag handle
        </div>
      </div>
      {provided.placeholder}
    </div>
  )}
</Draggable>
```

**Monkey patching**

> 만약 당신이 `DragHandleProps`의 prop중 하나가 필요하다면

```js
const myOnClick = (event) => console.log('clicked on', event.target);

<Draggable draggableId="draggable-1">
  {(provided, snapshot) => {
    const onClick = (() => {
      // dragHandleProps might be null(dragHandleProps 는 null 일 것입니다.)
      if(!provided.dragHandleProps) {
        return myOnClick;
      }

      // creating a new onClick function that calls my onClick(my onClick을 호출하는 새로운 onClick 함수 생성)
      // event as well as the provided one.(하나로 제공된 event)
      return (event) => {
        provided.dragHandleProps.onClick(event);
        // You may want to check if event.defaultPrevented(event.defaultPrevented가 true인지 확인하고)
        // is true and optionally fire your handler(선택적으로 핸들러를 시작하는 것이 좋습니다.)
        myOnClick(event);
      }
    })();

    return (
      <div>
        <div
          ref={provided.innerRef}
          style={provided.draggableStyle}
          {...provided.dragHandleProps}
          onClick={onClick}
        >
          Drag me!
        </div>
        {provided.placeholder}
      </div>
    );
  }}
</Draggable>
```

**2. snapshot: (StateSnapshot)**

```js
type StateSnapshot = {|
  isDragging: boolean,
|}
```

`children` 함수 역시 현재 드래그 state와 관련된 몇가지 작은 state와 함께 제공된다. 이것은 선택적으로 당신의 컴포넌트를 향상시킨다. 일반적인 상황은 드래그가 되는 경우 `Draggable`가 보여지는 것을 변경하는 것이다. 주의: 만약 당신이 커서(cursor)를 `grab`과 같은 것으로 변경하고 싶다면 당신은 style을 body에 추가할 필요가 있다. (참고 DragDropContext` > **style** above)

```js
<Draggable draggableId="draggable-1">
  {(provided, snapshot) => {
    const style = {
      ...provided.draggableStyle,
      backgroundColor: snapshot.isDragging ? 'blue' : 'grey',
    };

    return (
      <div>
        <div
          ref={provided.innerRef}
          style={style}
          {...provided.dragHandleProps}
        >
          Drag me!
        </div>
        {provided.placeholder}
      </div>
    );
  }}
</Draggable>
```

## Engineering health

### Typed

이 코드 베이스는 [flowtype](flowtype.org)를 사용하여 내부 일관성을 높이고 탄력적인 코드를 만듭니다.

### Tested

이 코드 베이스는 유닛(unit), 퍼포먼스(performance) 그리고 통합(integration) 테스트를 포함하여 몇가지 다른 테스트 전략을 가지고 있습니다. 테스트는 시스템의 다양한 점에서 퀄리티와 안정성을 높이는 것을 도와줍니다.

코드 커버리지는 [not a guarantee of code health](https://stackoverflow.com/a/90021/1374236) 입니다. 그것은 좋은 지표입니다. 이 코드 베이스는 기존 사이트의 **~95%를 커버리지** 합니다.

### Performance(퍼포먼스)

이 코드베이스는 매위 뛰어난 퍼포먼스를 발휘하도록 설계되어 있습니다 - 그것의 DNA 중 하나입니다. 이것은 `React` 퍼포먼스 성능 조사를 기반으로 합니다. [여기](https://medium.com/@alexandereardon/performance-optimisations-for-react-applications-b453c597b191) 그리고 [여기](https://medium.com/@alexandereardon/performance-optimisations-for-react-applications-round-2-2042e5c9af97). 이것은 각 작업에 필요한 최소 렌더링 수를 수행하도록 설계되었습니다.

**Highlights(하이라이트)**

- using connected-components with memoization to ensure the only components that render are the ones that need to - thanks [`react-redux`](https://github.com/reactjs/react-redux), [`reselect`](https://github.com/reactjs/reselect) and [`memoize-one`](https://github.com/alexreardon/memoize-one)
- all movements are throttled with a [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) - thanks [`raf-schd`](https://github.com/alexreardon/raf-schd)
- memoization is used all over the place - thanks [`memoize-one`](https://github.com/alexreardon/memoize-one)
- conditionally disabling [`pointer-events`](https://developer.mozilla.org/en/docs/Web/CSS/pointer-events) on `Draggable`s while dragging to prevent the browser needing to do redundant work - you can read more about the technique [here](https://www.thecssninja.com/css/pointer-events-60fps)
- Non primary animations are done on the GPU

| Minimal browser paints | Minimal React updates |
|------------------------|-----------------------|
|![minimal-browser-paints](https://github.com/alexreardon/files/blob/master/resources/dnd-browser-paint.gif?raw=true)|![minimal-react-updates](https://github.com/alexreardon/files/blob/master/resources/dnd-react-paint.gif?raw=true)|

## Supported browsers(지원 브라우저)

이 라이브러리는 데스크탑용 표준 [Atlassian 지원 브라우저](https://confluence.atlassian.com/cloud/supported-browsers-744721663.html)을 지원합니다:

| Desktop                             | Version                                              |
|-------------------------------------|------------------------------------------------------|
| Microsoft Internet Explorer(Windows)| Version 11                                           |
| Microsoft Edge                      | Latest stable version supported                      |
| Mozilla Firefox (all platforms)     | Latest stable version supported                      |
| Google Chrome (Windows and Mac)     | Latest stable version supported                      |
| Safari (Mac)                        | Latest stable version on latest OS release supported |

현재 모바일은 지원되지 않습니다. 그러나 터치 지원할 계획을 가지고 있습니다.

## Author / maintainer

Alex Reardon - [@alexandereardon](https://twitter.com/alexandereardon) - areardon@atlassian.com