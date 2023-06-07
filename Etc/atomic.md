## 디자인 시스템과 아토믹 디자인

- brad frost에 따르면, **디자인 시스템은 어떤 조직이 디지털 인터페이스를 디자인하고 구축하는 방식에 대한 이야기라고 말한다.**
- 디자인 시스템은 여러가지 하위 시스템을 포함하는데, UI 컴포넌트와 variants, 타이포그래피 시스템, 컬러 팔레트 시스템, 레이아웃 / 그리드 시스템 등이 있다.
- 아토믹 디자인은 **디자인 시스템을 만드는 방법론이다.**

## Atomic design

![Untitled](https://fe-developers.kakaoent.com/static/34afd4d0a47ff85c8f34295c18c2e374/78612/atomic-design-flow.png)

- brad frost의 아토믹 디자인은 화학적 관점에서 영감을 얻은 디자인 시스템이다.
- 모든 것은 atom(원자)으로 구성되어 있고, atom들이 서로 결합하여 molecule(분자)이 되고, molecule은 더 복잡한 organism(유기체)으로 결합하여 궁극적으로 모든 물질을 생성한다.
- 아토믹 디자인에서는 이러한 개념을 차용해서 **컴포넌트를 atom, molecule, organism,template, page의 5가지 레벨로 나눈다.**

### Atom

![Untitled](https://fe-developers.kakaoent.com/static/f8077fd0ee7a9a9cc39932e5b24d0177/d30ee/atom.png)

- **더 이상 분해할 수 없는 기본 컴포넌트**이다.
- label, input, button과 같은 기본 html element 태그, 글꼴, 애니메이션, 컬러 팔레트, 레이아웃과 같이 추상적인 요소도 포함될 수 있다.
- atom은 모든 기본 스타일을 한 눈에 보여주므로 디자인 시스템을 개발할 때 유용하게 사용된다.
- atom은 추상적인 개념을 표현할 수 있는데, 이것을 단일 컴포넌트로 사용하기엔 어려운 경우가 있다.
    - 예를 들어 레이아웃과 같은 atom 그 자체로는 실제 페이지에서 바로 사용하기에 유용하지 않을 수 있다.
    - 또한, atom을 다른 atom과 결합한 molecule 혹은 organism 단위에서 여러 단위와 결합하여 유용하게 사용될 수 있다.

### Molecule

![Untitled](https://fe-developers.kakaoent.com/static/cbb4f064ddfaedb6153527517c852c4a/afa26/molecule.png)

- 여러 개의 atom을 결합하여 자신의 고유한 특성을 가진다.
- 예를 들어, atom들을 결합할 경우, button atom을 클릭하여 form을 전송하는 molecule로 정의할 수 있다.
- **molecule의 중요한 점은 한 가지 일을 하는 것이다!**
    - SRP 원칙으로 인해 키워드 전송 기능이 필요한 곳에서 재사용될 수 있다.
- molecule의 SRP는 재사용성과 UI에서의 일관성, 테스트하기 쉬운 조건이라는 이점을 가진다.

### Organism

![Untitled](https://fe-developers.kakaoent.com/static/0e6c7ceae4d3379fdce1569a4791e2e0/78612/organism.png)

- 앞 단계보다 더 복잡하고 서비스에서 표현될 수 있는 명확한 영역과 특정 컨텍스트를 가진다.
- atom, molecule, organism으로 구성할 수 있다.
- 예를 들어 header 컨텍스트에 logo(atom), navigation(molecule), search from(molecule)을 포함할 수 있다.
- atom, molecule에 비해 좀 더 구체적으로 표현되고 컨텍스트를 가지기 때문에 상대적으로 재사용성이 낮아지는 특성을 가진다.

### Template

![Untitled](https://fe-developers.kakaoent.com/static/3ec46b462b280c889ab317611aaa404a/78612/template.png)

- 템플릿은 page를 만들 수 있도록 여러 개의 organism, molecule로 구성할 수 있다.
- **실제 컴포넌트를 레이아웃에 배치하고 구조를 잡는 와이어 프레임**
- 즉, 실제 콘텐츠가 없는 page 수준의 스켈레톤이다.

### Page

- page는 유저가 볼 수 있는 실제 콘텐츠를 담고 있다.
- template의 인스턴스

## Molecule, Organism을 나누는 기준

- molecule은 atom으로 구성되어 SRP에 따라 한 가지 책임을 진다.
- organism은 atom, molecule, organism으로 구성되어 서비스에서 Layout을 기준으로 나눌 수 있는 영역을 갖는다.
- 작성한 컴포넌트에 컨텍스트가 있는 경우 organism, 컨텍스트 없이 UI적인 요소로 SRP를 지킬 수 있다면 재사용하기 쉬운 molcule로 작성한다.
    - 따라서 molecule의 컴포넌트 네이밍은 컨텍스트 없이 주로 UI적인 네이밍이 포함되며, organism 네이밍은 컨텍스트가 포함된다.
        
        ![Untitled](https://fe-developers.kakaoent.com/static/4ae46e986375f8a2372a0608415d6aa7/73caa/molecule-organism-ex.png)
        

## Organism을 나누는 기준

![Untitled](https://fe-developers.kakaoent.com/static/2dc4d9084d080e7b075f8d0121ed7af5/78612/organism-split-problem.png)

- organism은 UI에서 명확한 영역을 갖는다.
- A처럼 큰 영역으로 나누거나, B처럼 비슷한 유형의 책임을 그루핑하여 구분할 수 있다.
- B와 같이 공통 컨텍스트를 묶어 organism 컴포넌트로 표현하면 적당한 책임을 가진 컴포넌트를 작성할 수 있다.

## 참고 자료

- https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/
- https://yozm.wishket.com/magazine/detail/1531/