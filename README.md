# Movie App 2020

React JS Fundamentals Course 2020

# 08 영화 앱에 여러 기능 추가하기
## 08-1 react-router-dom 설치하고 프로젝트 폴더 정리하기
가장 처음으로 '내비게이션' 기능을 만들어볼텐데 화면 이동을 시켜주는 장치인 라우터를 이용해 영화 앱 화면으로 이동시켜주는 Home, 개발자 자기 소개 화면으로 이동시켜주는 About을 만들 것이다.
1. react-router-dom 설치하기
```
gimhyoeun:movie_app_2020 gimhyoeun$ npm install react-router-dom
```
2. components 폴더에 Movie 컴포넌트 옮기기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
3. routes 폴더에 라우터가 보여줄 화면 만들기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
4. Home.js 수정하기
```
import React from 'react';
import axios from 'axios';
import Movie from '.../components/Movie';
import './Home.css';


class Home extends React.Component {
  state = {
    isLoading: true,
    movies: [],
  };
  getMovies = async () => {
    const {
      data: {
        data: { movies },
      },
    } = await axios.get("https://yts-proxy.now.sh/list_movies.json?sort_by=rating");
    this.setState({ movies, isLoading: false });
  };
  componentDidMount() {
    this.getMovies();
  }
  render() {
    const { isLoading, movies } = this.state;
    return (
      <section className="container">
        {isLoading ? (
          <div className="loader">
            <span class="loader__text">Loading...</span>
          </div>
        ) : (
          <div className="movies">
            {movies.map(movie => (
              <Movie
                key={movie.id}
                id={movie.id}
                year={movie.year}
                title={movie.title}
                summary={movie.summary}
                poster={movie.medium_cover_image}
                genres={movie.genres}
              />
            ))}
          </div>
        )}
      </section>
    );
  }
}
export default Home;
```
5. Home.css 만들기
routes 폴더에 Home.css를 만들고 다음과 같이 입력하면 영화  카드가 브라우저 폭에 맞게 1줄 또는 2줄로 출력 된다.
```
.container {
    height: 100%;
    display: flex;
    justify-content: center;
}


.loader {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: 300;
}
.movies {
    display: grid;
    grid-template-columns: repeat(2, minmax(400px, 1fr));
    grid-gap: 100px;
    padding: 40px;
    width: 80%;
    padding-top: 70px;
}


@media screen and (max-width: 1090px) {
    .movies {
        grid-template-columns: 1fr;
        width: 100%;
    }
}
```

6. App.js 수정하기
```
import React from 'react';
import Home from './routes/Home';
import './App.css';

function App() {
  return <Home />;
}


export default App;
```
진짜 그런지 보기 위해 App.js 코드를 이렇게 수정하고 영화 앱을 확인하면 진짜 브라우저 폭에 맞게 영화 카드가 1줄 또는 2줄로 출력되고 맨 위에 있는 영화 포스터 이미지가 잘리지 않게 바뀐다.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
## 08-2 라우터 만들어 보기
이제 App.js를 수정해서 라우터를 만들어 볼 것이다. 라우터는 사용자가 입력한 URL을 통해 특정 컴포넌트를 불러주는 기능을 하고 우리는 여러 종류의 라우터 중에서 HashRouter와 Route 컴포넌트를 사용할 것이다.
1. HashRouter와 Route 컴포넌트 사용하기
HashRouter와 Route 컴포넌트를 임포트하고, HashRouter 컴포넌트가 Route 컴포넌트를 감싸 반환하도록 App.js를 수정해 보자.
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';


function App() {
  return (
    <HashRouter>
      <Route />
    </HashRouter>
  );
}

export default App;
```
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
앱이 실행되는 주소에 #/이 붙을텐데 이건 HashRouter 때문이다.
Route에는 2가지 props를 전달할 수 있는데 하나는 URL을 위한 path props, 하나는 URL에 맞는 컴포넌트를 불러 주기 위한 component props다. 이 props를 통해 사용자가 접속한 URL을 보고, 그에 맞는 컴포넌트를 화면에 그릴 수 있게 되는 것이다,

2. Route 컴포넌트에 path, component props 추가하기
About 컴포넌트를 임포트하고 path, component props에 URL과 About 컴포넌트를 전달하자.
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';

function App() {
  return (
    <HashRouter>
      <Route path="/about" component={About} />
    </HashRouter>
  );
}

export default App;
```
3. About.js 수정하기
```
import React from 'react';


function About() {
    return <span>About this page: I built it because I love movies.</span>;
}

export default About;
```
HashRouter에 Route를 넣었고, Route가 About 컴포넌트를 불러오도록 만들었다. 

4. 라우터 테스트해 보기
무비앱 주소창에 /about을 붙여서 접속해보자.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
접속해보면 About 컴포넌트에 썼던 내용이 나온다. 아까 2번에서 추가한 path props를 보고 component props에 지정한 About 컴포넌트를 그려준 거기 때문이다.
이제 Home 컴포넌트도 보여줄 수 있도록 App.js를 수정해보자.

5. Home 컴포넌트를 위한 Route 컴포넌트 추가하기
App 컴포넌트에 Home 컴포넌트를 임포트하고, 또 다른 Route 컴포넌트를 추가하자.
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';

function App() {
  return (
    <HashRouter>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </HashRouter>
  );
}

export default App;
```

6. 라우터 테스트하고 문제 찾아보기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
아까와 같이 주소창에 /about을 입력하고 접속하면 이상하게 About 컴포넌트와 함께 Movie 컴포넌트가 출력된다.
/about에 접속하면 About 컴포넌트만 보여야 하는데 Movie 컴포넌트도 같이 보이는 문제가 있는 것이다.

7. 라우터 자세히 살펴보기
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';

function App() {
  return (
    <HashRouter>
      <Route path="/home">
        <h1>Home</h1>
      </Route>
      <Route path="/home/introduction">
        <h1>Introduction</h1>
      </Route>
      <Route path="/about">
        <h1>About</h1>
      </Route>
    </HashRouter>
  );
}

export default App;
```
8. 라우터 다시 테스트해 보기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
/home에 접속하면 별 문제 없이 출력된다.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
/home/introduction에 접속하면 이렇게 아까랑 같은 또 똑같은 문제가 발생한다. 왜냐면 라우터는 사용자가 /home/introduction에 접속하면 /, /home, /home/introduction 순서로 path props가 있는지 찾는다. 그런데 path props에는 /home과 /home/introduction이 모두 있으니까 /home/introduction으로 접속한 경우 Home, Introduction 컴포넌트가 모두 그려지는 것이다.

9. App.js 다시 원래대로 돌리기
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';

function App() {
  return (
    <HashRouter>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </HashRouter>
  );
}

export default App;
```
이제 다시 Home, About 컴포넌트를 그려주었던 상태로 코드를 돌려 놓자. 그런 다음 /about에 다시 접속해보면
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
여전히 Home, About 컴포넌트가 모두 그려진다, 이제 진짜 이 현상을 고치기 위해 Route 컴포넌트에 exact props를 추가하자. 이 때, exact props는 Route 컴포넌트가 path props와 정확하게 일치하는 URL에만 반응하도록 만들어 준다.

10. Route 컴포넌트에 exact props 추가해 보기
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';

function App() {
  return (
    <HashRouter>
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
    </HashRouter>
  );
}

export default App;
```
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
exact={true}를 추가하고 실행하면 이제 About 컴포넌트만 출력되는 걸 볼 수 있다.

11. About.css 작성하기
```
.about__container {
    box-shadow: 0 13px 27px -5px rgba(50, 50, 93, 0.25), 0 8px 16px -8px
rgba(0, 0, 0, 0.3),
    0 -6px 16px -6px rgba(0, 0, 0, 0.025);
padding: 20px;
border-radius: 5px;
background-color: whote;
margin: 0auto;
margin-top: 100px;
width: 100%;
max-width: 400px;
font-weight: 300;
}


.about__container span:first-child {
    font-size: 20px;
}
.about__container span:last-child {
    display: block;
    margin-top: 10px;
}
```
routes 폴더에 About.css 파일을 생성한 다음 위와 같이 입력하자.
```
import React from 'react';
import './About.css';

function About() {
    return (
        <div className="about__container">
            <span>
                "Freedom is the freedom to say that two plus two make four. If that is granted, all else follows."
            </span>
            <span>- George Orwell, 1984</span>
        </div>
    );
}

export default About;
```
그 다음 About.js에 About.css를 임포트하고 임포트한 About.css를 적용할 수 있도록 위와 같이 수정하자,
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
이제 앱을 실행해서 /about으로 접속하면 위와 같은 화면이 나타나니까 내비게이션 메뉴를 만들자.

## 08-3 내비게이션 만들어 보기
<Home>과 <About>이라는 2개의 버튼을 만들고, 각각의 버튼을 눌렀을 때 적절한 화면을 보여주도록 하자.

1. Navigation 컴포넌트 만들기
```
import React from 'react';

function Navigation() {
    return (
        <div>
            <a href="/">Home</a>
            <a href="/about">About</a>
        </div>
    );
}

export default Navigation;
```
components 폴더에 Navigation.js 파일을 만들고 2개의 a 엘리먼트를 반환하도록 JSX를 작성하자.

2. Navigation 컴포넌트 App 컴포넌트에 포함시키기
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';
import Navigation from './components/Navigation';

function App() {
  return (
    <HashRouter>
      <Navigation />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
    </HashRouter>
  );
}

export default App;
```
App 컴포넌트에 Navigation 컴포넌트를 포함시켜보자.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
영화 앱을 실행시켜 보면 왼쪽 위에 Home, About 링크가 생긴 걸 확인할 수 있다.

3. Home 링크 눌러 보기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
Home 링크를 눌러보면 겉으로 보기에는 잘 동작하는 것 같아 보이지만 링크를 누를 때마다 리액트가 죽고, 새 페이지가 열리는 문제가 있다.
이것은 a 엘리먼트의 href 속성이 페이지 전체를 다시 그리기 때문이다. 
문제를 해결하기 위해 react-router-dom의 Link 컴포넌트를 사용하자.

4. a 엘리먼트 Link 컴포넌트로 바꾸기

```
import React from 'react';
import { Link } from 'react-router-dom';


function Navigation() {
    return (
        <div>
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
        </div>
    );
}

export default Navigation;
```
Navigation 컴포넌트에 Link 컴포넌트를 임포트한 다음 a 엘리먼트를 Link 컴포넌트로 바꾸자. 그리고 href 속성은 to로 바꾸자.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
이제 Home과 About 링크를 눌러도 페이지 전체가 다시 새로 고침되지 않는다.
<여기서 Link, Router 컴포넌트는 반드시 HashRouter 안에 포함되어야 한다!!>

5. Navigation 컴포넌트 위치 다시 확인하기
혹시 실수로 Navigation 컴포넌트를 HashRouter 밖에 위치시켰다면 컴포넌트의 위치를 다시 확인해 보자.

6. Navigation 컴포넌트 스타일링하기

components 폴더에 Navigation.css 파일을 만들고 아래와 같이 작성한 다음 Navigation 컴포넌트에 임포트 시키자. 또 Navigation.css 파일을 Navigation 컴포넌트에 적용시키기 위해 Navigation 컴포넌트의 JSX를 수정하자.
```
.nav {
    z-index: 1;
    position: fixed;
    top: 50px;
    left: 10px;
    display: flex;
    flex-direction: column;
    background-color: white;
    padding: 10px 20px;
    box-shadow: 0 13px 27px -5px rgba(50, 50, 93, 0.25), 0 8px 16px -8px
    rgba(0, 0, 0, 0.3),
        0 -6px 16px -6px rgba(0, 0, 0, 0.025);
    border-radius: 5px;
}

@media screen and (max-width: 1090px) {
    .nav {
        left: initial;
        top: initial;
        bottom: 0px;
        width: 100%;
    }
}

.nav a {
    text-decoration: none;
    color: #0008fc;
    text-transform: uppercase;
    font-size: 12px;
    text-align: center;
    font-weight: 600;
}

.nav a:not(:last-child) {
    margin-bottom: 20px;
}
```


```
import React from 'react';
import { Link } from 'react-router-dom';
import './Navigation.css';

function Navigation() {
    return (
        <div className="nav">
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
        </div>
    );
}

export default Navigation;
```
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
화면 폭이 넓어지면 왼쪽에 화면 폭이 좁아지면 아래쪽에 내비게이션이 생긴다.

## 08-4 영화 상세 정보 기능 만들어 보기
이 기능을 만들기 위해서는 route props를 이해해야 한다.
route props란 라우팅 대상이 되는 컴포넌트에 넘겨주는 기본 props를 말한다.
다시 말하자면 우리가 직접 넘겨주지 않아도 기본으로 넘어가는 route props가 있고, 이것을 이용해야 영화 데이터를 상세 정보 컴포넌트에 전달할 수 있는 것이다.

1. route props 살펴보기
우선 console.log()를 통해 About으로 어떤 props가 넘어오는지 살펴보자.
```
import React from 'react';
import './About.css';

function About(props) {
    console.log(props);
    return (
        <div className="about__container">
            <span>
                "Freedom is the freedom to say that two plus two make four. If that is granted, all else follows."
            </span>
            <span>- George Orwell, 1984</span>
        </div>
    );
}

export default About;
```
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
About으로 이동하고 [console] 탭을 확인해보면 다음과 같이 보일 것이다. 이게 바로 react-router-dom에서 Route 컴포넌트가 그려줄 컴포넌트에 전달한 props다.
이 때 우리가 주목해야 할 점은 Route 컴포넌트가 그려줄 컴포넌트에는 항상 이 props가 전달되고, 이 props에 우리 마음대로 데이터를 담아 보내줄 수 있다는 사실이다.

2. route props에 데이터 담아 보내기
일단 아래와 같이 Navigation 컴포넌트 /about으로 보내주는 Link 컴포넌트의 to props를 수정해 보자.
```
import React from 'react';
import { Link } from 'react-router-dom';
import './Navigation.css';

function Navigation() {
    return (
        <div className="nav">
            <Link to="/">Home</Link>
            <Link to={{ pathname: '/about', state: { fromNavigation: true }
            }}>About</Link>
        </div>
    );
}

export default Navigation;
```
코드에서 보듯 to props에 객체를 전달했다. pathname은 URL을 의미하고, state는 우리가 route props에 보내줄 데이터를 의미한다.

3. route props 다시 살펴보기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
/about으로 이동한 다음 [console] 탭에서 [location]을 펼쳐 보면 state 키에 우리가 보내준 데이터를 확인할 수 있다. (((근데 그렇게 안됨)))

4. Navigation 컴포넌트 정리하기
```
import React from 'react';
import { Link } from 'react-router-dom';
import './Navigation.css';

function Navigation() {
    return (
        <div className="nav">
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
        </div>
    );
}

export default Navigation;
```
위와 같이 Navigation 컴포넌트를 원래대로 돌려놓고 본격적으로 영화 상세 정보 기능을 만들어 보자.

5. Movie 컴포넌트에 Link 컴포넌트 추가하기
Movie 컴포넌트에 Link 컴포넌트를 임포트하고, Link 컴포넌트에 to props를 작성하면 된다. 이 때 Link 컴포넌트의 위치에 주의하자. 안 그러면 영화 카드 모양이 이상해진다!
```
import React from 'react';
import PropTypes from 'prop-types';
import './Movie.css';
import { Link } from 'react-router-dom';
function Movie({ title, year, summary, poster, genres}) {
    return (
        <div className="movie">
            <Link
                to={{
                    pathname: '/movie-detail',
                    state: { year, title, summary, poster, genres },
                }}
            >
            <img src={poster} alt={title} title={title} />
            <div className="movie__data">
                <h3 className="movie__title">{title}</h3>
                <h5 className="movie__year">{year}</h5>
                <ul className="movie__genres">
                    {genres.map((genre, index) => (
                        <li key={index} className="genres__genre">
                            {genre}
                        </li>
                    ))}
                </ul>
                <p className="movie__summary">{summary.slice(0, 180)}...</p>
            </div>
        </Link>
        </div>
    );
}
Movie.propTypes = {
    year: PropTypes.number.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    poster: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string).isRequired,
};
export default Movie;
```
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
이제 영화카드를 누르면 /movie-detail로 이동하게 된다.

6. Detail 컴포넌트 만들기
Detail 컴포넌트를 routes 폴더에 추가하자. 그리고 Detail 컴포넌트에서 Movie 컴포넌트의 Link 컴포넌트가 보내준 영화 데이터를 확인헤 볼 수 있도록 console.log()도 작성하자.
```
import React from 'react';

function Detail(props) {
    console.log(props);
    return <span>hello</span>;
}

export default Detail;
```

7. Route 컴포넌트 추가하기

App.js를 연 다음 Detail 컴포넌트를 임포트하고 Route 컴포넌트에서 Detail 컴포넌트를 그려주도록 코드를 작성하자.
```
import React from 'react';
import './App.css';
import { HashRouter, Route } from 'react-router-dom';
import About from './routes/About';
import Home from './routes/Home';
import Navigation from './components/Navigation';
import Detail from './routes/Detail';

function App() {
  return (
    <HashRouter>
      <Navigation />
      <Route path="/" exact={true} component={Home} />
      <Route path="/about" component={About} />
      <Route path="/movie-detail" component={Detail} />
    </HashRouter>
  );
}

export default App;
```

8. 영화 카드를 눌러 /movie-detail로 이동한 다음 영화 데이터 확인하기
영화 카드를 눌러 /movie-detail로 이동해 보면 hello라는 문장이 보일 것이다.
그리고 [Console] 탭을 보면 [location->state]에 Movie 컴포넌트에서 Link 컴포넌트를 통해 보내준 데이터가 있을 것이다.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기

9. /movie-detail로 바로 이동하기
URL에 /movie-detail을 입력해서 바로 이동해 보자. 그 다음 [Console] 탭에 영화 데이터가 있는지 확인해 보자.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
Hello는 출력하고 있지만 state 값이 undefined이다. Detail 컴포넌트로 영화 데이터가 넘어오지 못했기 때문이다.
이런 경우 사용자를 강제로 Home으로 돌려보내야 한다. 이 때 그 기능을 '리다이렉트 기능'이라고 한다.

## 08-5 리다이렉트 기능 만들어보기
리다이렉트 기능을 위해서는 route props의 history 키를 활용해야 한다. history 키에는 push, go, goBack, goForward와 같은 키가 있는데 그 키에는 URL을 변경해 주는 함수들이 들어 있다.

1. history 키 살펴보기

주소창에 localhost:3000을 입력해서 이동한 뒤에 아무 영화 카드나 눌러서 이동하자.
그 다음 [Console] 탭에서 [history]에 출력된 값을 살펴보자.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
push, go, goBack, goForward와 같은 키가 보이는데 이게 다 URL을 변경해 주는 함수다.
이 중에서 지정한 URL로 보내주는 push() 함수를 사용해보자.
그 전에 Detail 컴포넌트를 클래스형 컴포넌트로 변경할 것이다. 그래야 componentDidMount() 생명주기 함수를 사용해 Detail 컴포넌트가 마운트될 때 push() 함수를 실행할 거기 때문이다.

2. Detail 컴포넌트 클래스형 컴포넌트로 변경ㅇ하기
```
import React from 'react';

class Detail extends React.Component {
    componentDidMount() {
        const { location, history } = this.props;
    }


    render() {
        return <span>hello</span>;
    }
}

export default Detail;
```
Detail 컴포넌트를 함수형에서 클래스형 컴포넌트로 변경한 다음 location, history 키를 구조 분해 할당하자.

3. push() 함수 사용하기
```
import React from 'react';

class Detail extends React.Component {
    componentDidMount() {
        const { location, history } = this.props;
        if (location.state === undefined) {
            history.push('/');
        }
    }


    render() {
        return <span>hello</span>;
    }
}

export default Detail;
```
location.state가 undefined인 경우 history.push("/")를 실행하도록 코드를 작성하자.

4. 리다이렉트 기능 확인해 보기

영화 앱을 실행한 다음 직접 주소를 입력해서 /movie-detial으로 이동해 보자. 그러면 다시 Home으로 돌아오게 될 것이다.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
이제 영화 카드를 눌러 영화 상세 정보 페이지로 이동하지 않으면 다시 첫화면으로 돌아갈 것이다.
하지만 아직 영화 상세 정보 페이지로 이동하면 hello만 출력되니까 영화 상세 정보 페이지를 만들어 보자.

5. 영화 제목 출력하기
Movie 컴포넌트로부터 전달받은 영화 데이터는 location.state에 들어 있었으니깐 location.state.title을 이제 출력해보자.
```
import React from 'react';

class Detail extends React.Component {
    componentDidMount() {
        const { location, history } = this.props;
        if (location.state === undefined) {
            history.push('/');
        }
    }


    render() {
        const { location } = this.props;
        return <span>{location.state.title}</span>;
    }
}

export default Detail;
```

6. /movie-detail로 바로 이동하기
하지만 또 다시 /movie-detail로 이동하면 오류가 발생한다.
![Alt text](/path/to/img.jpg) 이거 사진 바꾸기
componentDidMount() 생명주기 함수에 작성한 리다이렉트 기능이 동작하지 않아서 오류가 발생하는데 그 이유는 Detail 컴포넌트는 [render() -> componentDidMount()]의 순서로 함수를 실행하기 때문이다. render() 함수 내에서 location.state.title을 사용하려 하는데, location.state가 아까와 마찬가지로 undefined이기 때문이다.
그러므로 render() 함수에도 componentDidMount() 생명주기 함수에 작성한 리다이렉트 코드를 추가해주어야 한다.

7. location.state 확인하기
location.state가 없으면 render() 함수가 null을 반환한도록 수정하자.
```
import React from 'react';

class Detail extends React.Component {
    componentDidMount() {
        const { location, history } = this.props;
        if (location.state === undefined) {
            history.push('/');
        }
    }


    render() {
        const { location } = this.props;
        if (location.state) {
        return <span>{location.state.title}</span>;
        } else {
            return null;
        }
    }
}

export default Detail;
```

location.state가 없으면 render() 함수가 null을 반환하도록 만들어서 문제없이 실행되도록 만들었다. 그러면 이어서 componentDidMount() 생명주기 함수가 실행되면서 리다이렉트 기능이 동작할 것이다.