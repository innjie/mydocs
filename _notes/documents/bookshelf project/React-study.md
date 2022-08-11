# 0.Start

2022.05.30 ~ 2022.06.08

리액트 기초 강의
[Lecture Link](https://www.inflearn.com/course/react-%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9/dashboard)


***

# 1. Development Environment
## 디렉토리
>public
index.html이 있는 위치. html 파일에서 `<div id = "root"></div>` 부분에 컴포넌트가 들어가야 한다. <br/>
컴포넌트 파일은 src 내 파일을 수정해서 작동시킨다.

>src
js 파일이 있는 위치. 컴포넌트 구성 파일이 존재한다. 

## 컴포넌트
데이터가 주어지면 UI를 만들고 API로 변환 작업을 수행하는 요소

### 클래스형 컴포넌트
Stateful 컴포넌트라고도 한다. state 기능, 라이프 사이클 이용 가능. 구현 시 render 함수가 필요하다.

### 함수형 컴포넌트
Stateless 컴포넌트라고도 한다. 선언에 용이하고, 비교적 메모리 자원을 적게 활용한다. State를 직접 조작할 수 없어 setter를 사용해야 한다. <br/>
단순하게 데이터를 받아 UI에 뿌려주는데에 사용하면 좋다.

## index.js 파일 살펴보기
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

* `<App />`는 사용자 정의 태그이다. 이 태그는 `import App from './App';`를 참고하고, App.js의 태그들이 실제 구현되는 내용이다.

## 배포
### `npm start`로 실행 시 용량이 꽤 크다.
이유 : `create-react-app`의 개발환경에 필요한 요소들이 포함되어 크기가 크게 측정된다.  <br/>
따라서 배포하는 경우에는 `npm run build`를 활용해서 build 폴더에 만들고, 이 내용들을 배포하는 것이다. <br/>
build 폴더 내부에는 실제 동작에 불필요한 정보 (경고, 에러, 보안문제)를 제거한 내용만 포함한다. <br/>
`npx serve -s build`로 실행시켜 1회성 웹서버로 동작시킬 수 있다.


***

## 컴포넌트 작성
**Component 작성 시 최상위 태그는 한개만 가능하다!! 여러개 작성을 해도 IDE에서 도달할 수 없다는 오류를 반환하기 때문에 잘 확인하자.**
> App.js
```javascript
import React, {Component} from "react";
import './App.css';

//componenet를 만드는 코드
class Subject extends Component {
    render() {
        return (
            <header>
                <h1>WEB</h1>
                word wide web!
            </header>
        );
    }
}

class App extends Component {
    render() {
        return (
            <div className="App">
                <Subject/>
            </div>
        );
    }
}

export default App;
```

## Props
html 태그에 사용자 정의 속성값을 주고 props를 활용하여 값을 주입시키는 방법이다.

```javascript
class Subject extends Component {
    render() {
        return (
            <header>
                <h1>{this.props.title}</h1>
                {this.props.sub}
            </header>
        );
    }
}

class App extends Component {
    render() {
        return (
            <div className="App">
                <Subject title="WEB" sub="word wide web!"></Subject>
                <NSubject/>
                <ASubject/>
            </div>
        );
    }
}
```

## Component split
한 js 파일에 들어있는 컴포넌트들을 외부로 분리하고, import 형태로 가져올 수 있다.

> App.js

```javascript
import React, {Component} from "react";
import './App.css';
import TOC from "./component/TOC";
import Content from "./component/Content";
import Subject from "./component/Subject";

class App extends Component {
    render() {
        return (
            <div className="App">
                <Subject title="WEB" sub="word wide web!"></Subject>
                <Subject title="React" sub="For UI"></Subject>
                <TOC/>
                <Content title="HTML" desc = "HTML is HyperText Markup Language."></Content>
            </div>
        );
    }
}

export default App;
```

> Subject

```javascript
//componenet를 만드는 코드
import React, {Component} from "react";

class Subject extends Component {
    render() {
        return (
            <header>
                <h1>{this.props.title}</h1>
                {this.props.sub}
            </header>
        );
    }
}
export default Subject;
```

**export는 외부 파일에서 이 파일을 사용할 수 있도록 명시하는 역할을 한다.**

## State
> state란?
props에 따라 내부 구현에 필요한 데이터들. 컴포넌트를 조작하는데 필요하며, 사용자는 알 수 없고 내부적으로 사용되는 데이터다. <br/>
prop에 따라 구현하는 state는 철저하게 분리되어야 한다. (좋은 부품의 핵심) <br/>
**props와 state가 변경되면 화면이 다시 그려진다.**

## Key
> Warning: Each child in a list should have a unique "key" prop.
리스트에 키가 없어서 발생하는 문제. 다음과 같이 key 값을 설정하여 해결한다.

```javascript 
lists.push(<li key={data[i].id}><a href = {"/content/" + data[i].id}>{data[i].title}</a></li>);
```

## event
기본적으로 리액트는 함수 실행을 할 때 첫번째 파라미터를 이벤트 객체로 하기로 약속되어 있다. <br/>
그래서 이벤트 핸들링 시 함수의 첫번째 객체를 활용하여 조작하면 된다.

```javascript
                <header>
                    <h1><a href="/" onClick={function(e) {
                        console.log(e);
                        e.preventDefault();
                    }}>{this.state.subject.title}</a></h1>
                    {this.state.subject.sub}
                </header>
```

### bind

강제로 객체를 주입하는 메소드.
이벤트 함수의 this는 아무 값도 가리키고 있지 않다. (컴포넌트가 아님 -> state undefined 오류 발생)<br/>
-> bind를 사용하여 this가 컴포넌트임을 가리킨다.

```javascript
            <div className="App">
                {/*<Subject title={this.state.subject.title} sub={this.state.subject.sub}></Subject>
                <Subject title="React" sub="For UI"></Subject>*/}
                <header>
                    <h1><a href="/" onClick={function(e) {
                        console.log(e);
                        e.preventDefault();
                        // this.state.mode = 'welcome';
                        this.setState({
                            mode : 'welcome'
                        }); //bind : 객체를 강제로 넣는 메소드
                    }.bind(this)}>{this.state.subject.title}</a></h1>
                    {this.state.subject.sub}
                </header>
                <TOC data={this.state.contents}></TOC>
                <Content title={_title} desc={_desc}></Content>
            </div>
```

### setState

컴포넌트 생성 전에는 직접적으로 state의 값들을 수정해도 된다. <br/>
하지만 이미 만들어진 컴포넌트에 대해서 동적으로 수정할 때는 불가능하다. <br/>
이미 만들어진 컴포넌트의 값을 직접 바꾸면 리액트에서 알 수 있는 방법이 없어 렌더링을 할 수 없다. <br/>

```javascript
this.setState({
  mode : 'welcome'
});
```
### 이벤트 함수의 동작

* 컴포넌트에 사용자 정의 이벤트 함수를 정의한다.
* 이벤트 발생 시 props로 전달된 이벤트 함수가 호출된다.

## CRUD

### Create

```javascript
import React, {Component} from "react";

class CreateContent extends Component {
    render() {
        return (
            <article>
                <h2>Create</h2>
                <form action = "/create_process" method = "post"
                onSubmit={function(e){
                    e.preventDefault();
                    alert("Submit");
                }.bind(this)}>
                    <p><input type = "text" name = "title" placeholder="title"/> </p>
                    <p>
                        <textarea name="desc" placeholder="description"></textarea>
                    </p>
                    <p>
                        <input type="submit"></input>
                    </p>
                </form>
            </article>
        );
    }
}
export default CreateContent;
```
form 태그는 리액트가 아니더라도 웹 위에서 동작하는 태그. <br/>
그래서 submit버튼을 누르면 자동으로 동작하도록 설계되어 있는데, form 태그의 onSubmit을 제한해서 페이지 이동을 막을 수 있다.

#### 데이터 변경

```javascript
_article = <CreateContent onSubmit={function (_title, _desc) {
                this.max_content_id = this.max_content_id + 1;
                this.state.contents.push(
                    {id: this.max_content_id, title: _title, desc: _desc}
                );
                this.setState({
                    contents: this.state.contents
                });
                //add content to this.state.contents

                console.log(_title, _desc);
            }.bind(this)}></CreateContent>
```

state의 데이터를 직접 변경할 수 없으므로, 오리지널 데이터를 생성하고
기존 state에 세팅된 데이터를 변경함으로써 데이터 교환 및 create 작업을 완료한다.


```javascript
var _contents = this.state.contents.concat({
                    id : this.max_content_id,
                    title : _title,
                    desc : _desc
                });
```
기존 배열에 추가하는 방식이다. <br/>
새 배열 객체를 만들고, 기존 데이터에 새 데이터를 넣어서 변수에 넣는 방법이다. <br/>
성능 퍼포먼스 개선에도 훨씬 효과적이다. <br/>

### shouldComponenetUpdate
클릭 이벤트에 따라 관련되지 않은 컴포넌트도 계속 렌더링을 하고 있다. <br/>
큰 프로그램에서 중요한 이슈가 될 수 있다. 성능 향상을 위해서 `render()`의 실행여부를 결정하는 함수를 사용한다. <br/>

```javascript
shouldComponentUpdate() {
        console.log('TOC shouldComponentUpdate')
        return false;
    }
```

부모 컴포넌트가 변경되어도 자식 컴포넌트의 `render()`가 호출되지 않는다. 

### Immutable
javascript에서 배열을 조작할 때, push와 concat의 방법을 사용할 수 있다.

>push
원본이 변경됨

>concat
복사본이 변경됨

immutable을 사용하면, 불변 속성을 갖는 유사 객체를 제어할 수 있다. <br/>
immutable은 원본 객체의 복사본을 바꾼 결과를 return한다.

변하면 안되는 객체를 변경하면 여러 이슈가 생길 수 있다. 그래서 객체를 immutable하게 다루기 위한 라이브러리가 제공된다. 
>immutable.js 
배열과 객체의 대체재로 사용할 수 있다. 원본을 변경하지 않고, 복제된 원본을 변경한 새 객체를 return한다.

```javascript
<script src = "immutable.min.js"></script>
<script>
    var map1 = Immutable.Map({a : 1, b : 2, c : 3});
    var map2 = map1.set('b', 50);
    map1.get('b'); // 2
    map2.get('b'); // 50
</script>
```

### Update

`onChange`를 사용해서 변경값을 실시간으로 받아온다.

```javacript 
<input
    type = "text"
    name = "title"
    placeholder="title"
    value = {this.state.title}
    onChange={this.inputFormHandler.bind(this)}
></input>
```

#### javascript :: []

최신 javascript 문법으로, []가 있다.

```javascript
    inputFormHandler(e) {
        this.setState({
            [e.target.name] : e.target.value
        });
    }
```
[] 안의 값을 가져와 세팅해준다. </br>
이렇게 사용해서 같은 동작을 하는 다른 id값을 갖는 요소의 코드 중복을 해결할 수 있다.


### Delete

DB를 사용하지 않으므로 `Arrays.splice`를 사용하여 삭제해준다. <br/>

#### splice
i번째 원소부터 몇개를 삭제할 지 지정할 수 있다.
예제에 사용된 `_contents.splice(i, 1);`의 경우에는 _contents의 i번째를 삭제하는 코드이다.

```javascript
<Control onChangeMode={function (_mode) {
    if (_mode === 'delete') {
        if (window.confirm('delete')) {
            var i = 0;
            while (i < this.state.contents.length) {
                var _contents = Array.from(this.state.contents);
                if (_contents[i].id === this.state.selected_content_id) {
                    //i 부터 1개 삭제
                    _contents.splice(i, 1);
                    break;
                }
                i = i + 1;
            }
            this.setState({
                mode: 'welcome',
                contents: _contents
            })
            alert("deleted");
        }
    } else {
        this.setState({
            mode: _mode
        });
    }


}.bind(this)}></Control>
```

### Router
웹 애플리케이션은 한개의 URL로 모든 페이지를 다루고 있다. 페이지 전환 시 네트워크를 로드하지 않는 것은 장점이지만 URL만으로 페이지를 찾아올 수 없다.Router를 사용하면 URL에 따라 컴포넌트를 호출할 수 있다. (UI 제공) npm을 통해 설치하는 플러그인이다.

### create-react-app
`npm run eject` 을 실행하면 react에 숨겨진 설정들을 변경할 수 있다.
하지만 eject를 실행하면 실행 전으로 돌아갈 수 없으니 주의한다.

### Redux
컴포넌트 수의 증가에 따라 컴포넌트를 다루기가 까다로워진다.
부모 -> 자식은 props, 자식 -> 부모는 event를 호출하고 있다. 

**Redux**는 중앙에 데이터 저장소를 만들고, 모든 컴포넌트는 중앙 저장소에 직접 연결된다. 저장소에 저장된 데이터가 변경되면 관련된 모든 컴포넌트가 변경되어 다시 저장된다. 

### React server side rendering
서버쪽에서 웹 어플리케이션을 만들고, 클라이언트로 완성된 HTML을 보내서 구동할 수 있다.
초기 구동시간을 단축할 수 있고, javascript의 로딩이 필요없는 구현이 가능하다. 검색엔진들이 HTML 태그를 읽을 수 있기 때문에 웹 페이지 분석에 친화적인 기술이다. 

### React Native
IOS, Android에서도 동작할 수 있는 애플리케이션 개발이 가능하다.
한개의 앱을 다양한 환경에서 동작할 수 있는 하이브리드 기술이다.
