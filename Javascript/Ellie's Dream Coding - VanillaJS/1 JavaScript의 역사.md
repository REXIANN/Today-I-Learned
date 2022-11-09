# 1. JavaScript의 역사

## JavaScript의 역사

1994년 Marc Andreessen은 Netscape를 설립한다. 그리고 HTML과 CSS를 사용하여 웹 페이지를 꾸미는 것이 가능했었다. 그러나 동적으로 DOM을 조작하기 위한 언어가 새로 필요하다고 판단하여 개발에 착수하였고 1994년 9월, Mocha라는 스크립트 언어를 만들고 Netscape navigator에 이 언어를 해석할 수 있는 엔진(interpreter)을 탑재하였다. Mocha는 다시 LiveScript로 이름을 바꾸고 다시 한번 자바의 인기에 편승하기 위해 JavaScript로 다시 한번 이름을 변경했다. 이것이 오늘날 우리가 알고있는 JavaScript이며 비슷한 이름 때문에 종종 Java와 헷갈리게 만든다.

1995년 마이크로소프트는 Netscape 사의 브라우저를 Reverse engineering하여 JavaScript를 똑같이 구현해내고 이름을 JScript라고 붙였다. 그렇게 JScript를 탑재한 브라우저 Internet Explorer를 만들었다. 이는 웹사이트는 표준의 파편화가 되는 시초가 되게 된다. 브라우저 엔진의 통일을 위해 1997년 Netscape사는 ECMAScript1 language specification라는 표준안을 제정하였다. 그 후 ES는 꾸준히 발전을 거듭하였으며 ECMAScript4에서는 `class`, `optional type annotation`, `Enterprise scale` 등이 추가되기도 했다.

## 마이크로소프트, 너 그렇게 안봤는데…

그동안 IE의 점유율이 95퍼센트에 달하며 마이크로소프트는 점점 ECMAScript의 표준안에 반하는 의사를 보이고, 더 이상 이 표준안에 참여하지 않겠다고 선언하였다. 압도적인 시장점유율을 바탕으로 지배력을 행사하고 있었기에 별다른 수가 없었다.

그렇게 시간은 흘러 2004년, mozilla에서 만든 firefox라는 브라우저도 등장했다. 이 당시의 개발자들은 IE, Netscape Navigator, Firefox 세 개의 브라우저에서 자신들의 웹 사이트가 잘 돌게 만들기 위해 여러번 일을 해야 했었다고 한다… 당시 jQuery, dojo, moo tools 등의 라이브러리가 등장하였는데, 이는 JVM과 비슷한 역할로 개발자가 작성한 하나의 코드가 다른 여러 브라우저에서 공통적으로 잘 동작하게 도와주었다. 개발자는 해당 인터페이스를 사용하면 어느 브라우저에서 구동되는지는 상관 없었기 때문에 각광받는 프레임워크였다. 아마 개발을 하는 사람들이라면 다들 jQuery정도는 들어봤을 거라고 생각한다.

2004년은 자바스크립트에 의미가 있는 해로, Jesse James Garrett이라는 사람이 `AJAX(Asynchronous JavaScript and XML)` 이라는 기술명세서를 발표하였기 때문이다. AJAX는 비동기적으로 데이터를 서버에서 받아오고 처리할 수 있도록 도와주어 기존에 동기적으로 처리해야 했던 모든 데이터 전송을 비동기적으로 요청하고 받아올 수 있게 되었다.

## 게임체인저의 등장

2008년 구글에서 Chrome이라는 브라우저가 등장했다. 당시 크롬은 `JIT(just-in-time compilation)`이라는 강력한 엔진을 탑재했는데 자바스크립트를 실행하는 속도가 엄청나게 빨랐다! 사람들은 앞다투어 이 새로운 브라우저로 옮기게 되었다. 2020년 현재 크롬 브라우저의 인터넷 점유율은 66.31%로, 2위인 Safari 점유율인 16%을 훌쩍 뛰어넘는다. Safari가 모든 애플기기에 기본으로 설치되어 있다는 점을 고려해볼 때 엄청난 수치가 아닐 수 없다.

2009년 ECMAS5이 출시, 2015년에 드디어 ECMAScript6가 출시된다. ES6에는 우리가 사용하는 class, arrow function, const, let 등이 정의되었다. 이제는 모든 브라우저들이 ECMAScript 표준을 잘 따르고 있으므로 더 이상 라이브러리의 도움없이 모든 브라우저에서 잘 동작하는 웹사이트를 만들 수 있다!

각 브라우저는 자바스크립트를 해석하고 실행하는 엔진들이 존재한다

<table>
    <thead>
        <td>브라우저</td>
        <td>엔진</td>
    </thead>
    <tbody>
        <tr>
            <td>CHROME</td>
            <td>V8</td>
        </tr>
        <tr>
            <td>Firefox</td>
            <td>Spider Monkey</td>
        </tr>
        <tr>
            <td>Safari</td>
            <td>JSCore</td>
        </tr>
        <tr>
            <td>MS Edge</td>
            <td>V8</td>
        </tr>
        <tr>
            <td>Opera</td>
            <td>Caravan</td>
        </tr>
    </tbody>
</table>

크롬 브라우저에서 사용하는 V8엔진은 node.js와 ELECTRON에서도 사용한다. 우리가 개발할때는 최신버전의 JS를 쓰고 사용자가 쓰는 브라우저의 버전이 이전 버전이라면 그것을 도와주는 BABEL이라는 녀석이 존재한다.

요즘 뜨고 있는 SPA는 대표적으로 React, Angular, Vue, Backbone 등의 프레임워크를 사용하여 쉽게 만들 수 있다. 또한 강력한 V8엔진을 이용해서 node.js가 등장한다. node.js는 강력한 자바스크립트 엔진을 이용한 백엔드 프레임워크이다. React Native나 coldova를 이용할 수도 있고 ELECTRON을 이용해서 데스크탑 앱을 만들 수 있다.

브라우저에서 동작하는 유일한 언어는 자바스크립트 였다. 요즘은 Web Assembly가 등장해서 Rust, C, C#, Java, Go등 다양한 언어를 사용해서 웹 어플리케이션을 만드는 것이 가능해졌다.

자바스크립트는 가장 많이 사용되는 언어에서 7년연속 1위, 개발자들이 배우고 싶은 언어 2위를 이어오고 있다.

우선 먼저 client 자바스크립트를 먼저 배우고 server-side를 나중에 배우자!