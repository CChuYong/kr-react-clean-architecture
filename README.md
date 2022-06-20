## 조언의 말
이 레포지토리는 예시이며, 필자는 프로덕션 레벨에서 이것과 완전히 같게 구현하지 않는다. 필자는 이렇게도 할 수 있다는 한가지 방법을 공유할뿐, 최선의 방법은 아닐수도 있으며, 하지만 실제적인 방법이다. 필자는 독자들이 연구를 완료하기 위해 다른 구현 방법들도 둘러보길 권장한다. 이 레포지토리는 또한 조금 오래되었고, 시간이 지남에 따라 많은 전통을 가지고 있다.  

### 예시 (PR을 제출해 또다른 예시를 제출해보라)
- https://github.com/bespoyasov/frontend-clean-architecture

# 리액트 클린 아키텍쳐
리액트 코드베이스에 클린 아키텍쳐를 적용하는것은 많은 이점을 가져오는데, 그 이점들의 대부분은 독자들이 구글에 "클린 아키텍쳐의 개념"과 "어떤 아키텍쳐 패턴을 결합해야하는가"를 검색함으로써 찾을 수 있다. 하나의 이점 중 필자를 감명깊게 한 부분은 "비즈니스 규칙을 프레임워크-특정적인것에서 격리시킬 수 있다"는 것이다. 이는 우리의 코어 로직이 리액트 프레임워크에 종속적이지 않은것을 의미한다. (리엑트 네이티브나, 익스프레스,, 기타등등)  
이는 독자들에게 충분한 유연성을 주는데, 예를 들어 어플리케이션의 특정 부분을 백엔드 파트로 이전한다거나, 라이브러리 교체를 큰 고통 없이 해내거나, 테스트를 한번 해보고 독자들이 원하는 만큼 재사용하거나, 코드를 리액트뿐만 아니라 리액트 네이티브 사이에서 공유한다거나.. 등등  
이것은 현실적인 접근이다. 무슨말이냐면, 이는 프로덕션 레벨에서 적용할 수 있을정도로 단순하면서도 견고하다.  
물론 필자는 교육 목적을 위해 최대한 단순화했기에, 이 글이 독자들에게 아키텍쳐 패턴을 적용하고 독자들의 필요에 맞게 적용하는데 큰 역할을 했으면 한다.  

## 구체적인 설명 (TMI)
필자가 최근 바빴으나, 최대한 많이 써볼 것이다.
필자는 클린 아키텍쳐가 무엇인지, 왜 써야하고 어떻게 써야하는것인지에 대한 세개의 글을 작성하려고 한다.
포르투갈어의 리엑트 코드베이스에 클린아키텍쳐를 적용하는 글은 [여기](https://medium.com/@eduardomoroni/arquitetura-limpa-para-bases-de-c%C3%B3digo-react-df0f78d2b42e)

## Philosophy
![high-level-diagram](https://github.com/eduardomoroni/react-clean-architecture/blob/master/docs/images/high-level-diagram.jpg)  

The nomenclature may vary, but the concept behind this architectural pattern is: the domain dictates how tools should be organized and not the other way around.
What I mean by that is that we should organize our codebase around the business rules and not around the frameworks we use to achieve business rules.
The diagram above shows how the dependency rule works, the inner circles must not know about the outer circles. That is, there cannot be an import of a use case within an entity, or import of a framework within a use case.
Another important rule is: entities and use cases should not rely on external libraries. The explanation is simple, the core of our application must be robust enough and malleable enough to meet the demands of the business without needing any external intervention.
If by chance, an essential part of the application core MUST BE an external dependency. Dependency needs to be modeled following [dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle).

## Communication flow
![communication-flow-diagram](https://github.com/eduardomoroni/react-clean-architecture/blob/master/docs/images/communication-flow.jpg)  

### A brief explanation of each responsibility
- **Entity**: Application independent business rules
- **Interactor**: Application-specific business rules
- **Adapter**: Glue code from/to *Interactors* and *Presenter*, most of the time implementing a framework-specific behaviour.
  e. g.: We have to connect *Interactor* with react container, to do so, we have to connect *Interactor* with redux (framework) and then connect redux to container components.
- **Presenter**: Maps data from/to *Adapter* to/from *Components*.
- **Components**: Simplest possible unit of presentation. Any mapping, conversion, MUST be done by the *Presenter*.

## Sample apps DEMO
Talk is cheap, don't you think? That's why I'm sharing two sample apps to facilitate your digestion.  
A great advantage of following clean architecture is having all business logic self-contained and closer, in a readable way.  
Take a look at `core/entities/` and `core/useCases/` folders and see for yourself.
### Counter
The counter app is a simple example of how to apply clean architecture to react world, it uses only synchronous actions and has no external dependencies.  
It contains 2 use case rules:
- The count must not be negative.
- The count must not be greater than 10.  

![counter-gif](https://github.com/eduardomoroni/react-clean-architecture/blob/master/docs/images/counter.gif)  

---  

### Authentication
An authentication app is a simple example, but not that simple, of how to apply clean architecture to a realistic scenario.
It contains some shared business rules:
- Users must have a valid email.
- Users password must comprises only numbers and/or letters.
- Users name must have a full name, and it must to be lowercased.
- The App cannot sign up two users with the same email address.
- The App must use an external dependency to persist user register.  

![authentication-gif](https://github.com/eduardomoroni/react-clean-architecture/blob/master/docs/images/authentication.gif)  

---  

## Folder Structure
This repository contains 2 examples of how to implement react following clean architecture, represented by the [diagram](#philosophy) above, and both follow the same folder structure:
```
./counter
├── core
│   └── lib
│       ├── adapters
│       │   └── redux
│       ├── entities
│       ├── frameworks
│       └── useCases
├── native
│   └── src
│       ├── components
│       └── stylesheets
└── web
    └── src
        ├── assets
        ├── components
        └── stylesheets
```
*Note:* the `frameworks` folder comprises framework-specific setups to have it available to the adapters.  

## Running the apps
run `npm install` under the project you'd like to run, and then run `npm start`.

### Running on Windows

There's an [issue](https://github.com/eduardomoroni/react-clean-architecture/issues/2) related to how yarn/npm symlink file dependencies on windows. Due to this issue, you should first go under the `core` module and run `npm install` and `npm run build`. This will make the `core` module ready to be installed on the other modules.

## References
- [Clean Architecture: a craftsman's guide to software structure and design](https://goo.gl/2h3fsD)
- [The clean architecture](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Agility and Architecture](https://www.youtube.com/watch?v=0oGpWmS0aYQ)
- [Github](https://github.com/topics/clean-architecture?o=desc&s=stars)
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

## Thanks
- Microsoft: for providing a [typescript react native starter kit](https://github.com/Microsoft/TypeScript-React-Native-Starter).
- [Will Monk](https://github.com/wmonk): for providing a [react typescript starter kit](https://github.com/wmonk/create-react-app-typescript) .

## Feedback
If something looks odd, don't hesitate to reach me out or opening an issue.
