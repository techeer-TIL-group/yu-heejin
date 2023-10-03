## 핵심 컴포넌트

### `<View>`

- flexbox, 스타일, 일부 터치 처리 및 접근성 컨트롤을 사용하여 레이아웃을 지원하는 컨테이너
- 웹 개발에서 `<div>`와 비슷하고, 하위 요소가 여러개일 경우 상위 루트에 해당 컴포넌트가 필요하다.

### `<Text>`

- 텍스트 문자열을 표시하거나 스타일, 중첩 문자열, 터치 이벤트 지원
- 웹 개발에서 `<p>`와 유사하다.

### `<Image>`

- 다양한 유형의 이미지 표시

### `<ScrollView>`

- 여러 구성 요소 및 보기를 포함할 수 있는 일반 스크롤 컨테이너

### `<TextInput>`

- 사용자가 텍스트를 입력할 수 있다.

---

## Props, State

- Props는 ‘Properties’를 줄인 말이다.
- Props는 리액트 컴포넌트를 커스터마이징 할 수 있게 도와준다.
- state는 useState를 사용하여 관리한다.

```tsx
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { View, Text, Button, StyleSheet, ScrollView } from 'react-native';

type CatProps = {
  name: string;
};

const Cat = (props: CatProps) => {
  const [isSleepy, setIsSleepy] = useState(true);
  return (
    <ScrollView>
      <Text>나는 {isSleepy ? '졸린' : '안 졸린' } {props.name}입니다.</Text>
      <Text>아래 버튼을 클릭하시면 저를 재우실 수 있어요.</Text>
      <Button
        onPress={() => {
          setIsSleepy(false);
        }}
        disabled={!isSleepy}
        title={isSleepy ? '희진이를 재워주세요' : '감사합니다.'}
        />
    </ScrollView>
  );
};

export default function App() {
  return (
    <ScrollView>
      <Cat name="유희진"/>
    </ScrollView>
  );
}
```

### useState

- useState로 두 가지 작업을 수행할 수 있다.
    1. **초기 값**으로 ‘state 변수’를 생성한다.
    2. **state 변수의 값을 설정하는 함수** setIsHungry를 생성한다.
- 패턴을 `[<getter>, <setter>] = useState(<initialValue>)`
로 생각하면 편리하다.
- isHungry는 const지만 값 재할당이 가능하다.
    - set~~ 함수가 호출되면 컴포넌트가 리랜더링된다.
    - 이 경우 컴포넌트 함수가 다시 실행되어 useState가 isHungry의 다음 변수를 제공한다.

- button 코어 컴포넌트를 추가하고 onPress Props 전달
    
    ```tsx
    <Button
      onPress={() => {
        setIsHungry(false);
      }}
      //..
    />
    ```
    
    - 위 상태에서 버튼을 누르면 onPress가 발생되며 setIsHungry(false) 호출
    

---

## Text Input

- Text Input은 사용자가 텍스트를 입력할 수 있는 코어 컴포넌트이다.
- 텍스트가 변경될 때마다 호출되는 함수를 인자로 받는 onChangeText prop, 텍스트가 제출될 때마다 호출되는 함수를 인자로 받는 onSubmitEditing prop를 가지고 있다.

```tsx
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { View, Text, Button, StyleSheet, ScrollView, TextInput } from 'react-native';

export default function App() {
  const [text, setText] = useState('');
  return (
    <View>
      <TextInput
        style={{
          height: 40
        }}
        placeholder="글자를 입력해보세요. 토큰 단위로 피자가 생깁니다."
        onChangeText={newText => setText(newText)}
        defaultValue={text}
      />
      <Text style={{padding: 10, fontSize: 42}}>
        { text.split(' ').map(word => word && '🍕').join(' ') }
      </Text>
    </View>
  );
}
```

---

## ScrollView

- 스크롤뷰는 여러 구성 요소와 보기를 포함할 수 있는 일반 스크롤 컨테이너
- horizontal 속성을 설정하여 수직 및 수평으로 스크롤 가능하다.

```tsx
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { View, Text, Button, StyleSheet, ScrollView, TextInput, Image } from 'react-native';

const logo = {
  url: 'https://reactnative.dev/img/tiny_logo.png',
  width: 64,
  height: 64,
};

export default function App() {
  return (
    <ScrollView>
      <Text style={{fontSize: 96}}>Scroll Me Plz</Text>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
      <Image source={logo}/>
    </ScrollView>
  );
}
```

- scrollView는 pagingEnabled props를 사용해 스와이프 제스처로 뷰를 페이징하도록 구성할 수 있다.
    - ViewPager를 사용해 안드로이드에서 뷰 사이를 가로로 스와이프 할 수도 있다.
- ScrollView는 제한된 크기의 적은 양을 표시하는데 가장 적합하다.
    - **스크롤 뷰의 모든 엘리먼트와 뷰는 현재 화면에 보이지 않더라도 전부 렌더링된다.**
    - **만약 화면에 표시할 수 있는 것보다 많은 아이템을 가진 긴 리스트를 가지고 있다면, ScrollView 대신 Flatlist를 사용해야 한다.**

---

## ListViews

- 데이터 리스트를 표시하기 위한 컴포넌트 모음을 제공한다.
- 일반적으로 FlatList, SectionList를 사용한다.

### FlatList

- **비슷한 구조에서 변경**되는 데이터의 리스트를 표시한다.
- 항목의 수가 시간에 따라 변경될 수 있는 긴 데이터 리스트에 적합하다.
- 일반적인 스크롤 뷰와 달리 **FlatList는 모든 엘리먼트를 한 번에 렌더링하지 않고 현재 화면에 보여지는 부분만 렌더링하기 때문이다.**
- FlatList 컴포넌트에는 data, renderItem 두 개의 props가 필요하다.
    - data는 데이터 리스트의 정보 소스
    - renderItem은 데이터 소스에서 하나의 항목을 가져와 렌더링할 컴포넌트를 포맷에 맞게 반환한다.

```tsx
import { StatusBar } from 'expo-status-bar';
import { useState } from 'react';
import { FlatList, StyleSheet, Text, View } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: 22,
  },
  item: {
    padding: 10,
    fontSize: 18,
    height: 44,
  },
});

export default function App() {
  return (
    <View style={styles.container}>
      <FlatList
        data={[
          {key: 'Devin'},
          {key: 'Dan'},
          {key: 'Dominic'},
          {key: 'Jackson'},
          {key: 'James'},
          {key: 'Joel'},
          {key: 'John'},
          {key: 'Jillian'},
          {key: 'Jimmy'},
          {key: 'Julie'},
        ]}
        renderItem={({item}) => <Text style={styles.item}>{item.key}</Text>}
      />
    </View>
  );
}
```

- 만약 데이터를 섹션 헤더와 함께 논리적인 단위로 구분해서 렌더링하고 싶다면 SectionList가 적합하다.
    
    ```tsx
    import { StatusBar } from 'expo-status-bar';
    import { useState } from 'react';
    import { SectionList, StyleSheet, Text, View } from 'react-native';
    
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        paddingTop: 22,
      },
      sectionHeader: {
        paddingTop: 2,
        paddingLeft: 10,
        paddingRight: 10,
        paddingBottom: 2,
        fontSize: 14,
        fontWeight: 'bold',
        backgroundColor: 'pink',
      },
      item: {
        padding: 10,
        fontSize: 18,
        height: 44,
      },
    });
    
    export default function App() {
      return (
        <View style={styles.container}>
          <SectionList
            sections={[
              {
                title: 'D', 
                data: [
                  'Devin', 
                  'Dan', 
                  'Dominic'
                ]
              },
              {
                title: 'J',
                data: [
                  'Jackson',
                  'James',
                  'Jillian',
                  'Jimmy',
                  'Joel',
                  'John',
                  'Julie',
                ],
              },
            ]}
            renderItem={({item}) => <Text style={styles.item}>{item}</Text>}
            renderSectionHeader={({section}) => (
              <Text style={styles.sectionHeader}>{section.title}</Text>
            )}
            keyExtractor={item => `basicListEntry-${item}`}
            />
        </View>
      );
    }
    ```
    
- 리스트 뷰의 가장 일반적인 사용은 서버로부터 받아온 데이터를 보여주는 것이다.

---

## 플랫폼 별 코드(Platform Specific Code)

- 크로스 플랫폼 앱을 만들때 Android, iOS에서 별도의 시각적 컴포넌트를 구현하고자 하는 경우와 같이 코드가 달라야 하는 상황이 발생할 수 있다.
- React Native는 다음과 같은 방법으로 플랫폼을 구분한다.
    1. Platform module
    2. platform-specific file extenstions

### Platform module

- RN은 앱을 실행하고 있는 플랫폼을 감지하는 모듈을 제공한다.
- 컴포넌트의 작은 요소가 플랫폼에 따라 달라지는 경우 다음 옵션을 적용한다.
    
    ```tsx
    import {Platform, StyleSheet} from 'react-native';
    
    const styles = StyleSheet.create({
      height: Platform.OS === 'ios' ? 200 : 100,
    });
    ```
    
- 키가 ‘ios’, ‘android’, ‘native’, ‘default’ 중 하나일 경우 현재 실행중인 플랫폼에 가장 적합한 값을 반환하는 `Platform.select` 메소드도 사용 가능하다.
    
    ```tsx
    import {Platform, StyleSheet} from 'react-native';
    
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        ...Platform.select({
          ios: {
            backgroundColor: 'red',
          },
          android: {
            backgroundColor: 'green',
          },
          default: {
            // other platforms, web for example
            backgroundColor: 'blue',
          },
        }),
      },
    });
    ```
    
    - 즉, 모바일에서 실행하는 경우 ios 혹은 android 키가 우선 적용된다.
    - 만약 키가 지정되어있지 않다면 native 키가 사용된 다음 default 키가 적용된다.
- any 값도 허용되기 때문에 다음과 같은 플랫폼별 컴포넌트를 반환할 때도 사용할 수 있다.
    
    ```tsx
    const Component = Platform.select({
      ios: () => require('ComponentIOS'),
      android: () => require('ComponentAndroid')
    })();
    
    <Component />;
    
    const Component = Platform.select({
      native: () => require('ComponentForNative'),
      default: () => require('ComponentForWeb')
    })();
    
    <Component />;
    ```
    

### Android 버전 인식

- Android에서 Platform 모듈은 앱이 실행중인 안드로이드 플랫폼의 버저을 감지할 때도 사용할 수 있다.
    
    ```tsx
    import { Platform } from 'react-native';
    
    if (Platform.Version === 25) {
      console.log('Running on Nougat!');
    }
    ```
    

### iOS 버전 인식

- iOS에서 version은 `-[UIDevice systemVersion]`의 결과로, **운영체제의 현재 버전을 나타내는 문자열**을 뜻한다.
    - 시스템 버전의 예로는 “10.3”이 있다.
- 예를 들어, iOS에서 주 버전 숫자를 감지하려면 다음과 같이 작성하면 된다.
    
    ```tsx
    import { Platform } from 'react-native';
    
    const majorVersionIOS = parseInt(Platform.Version, 10);
    // 문자열이기 때문에 숫자로 변환해줘야한다!
    if (majorVersionIOS <= 9) {
      console.log('Work around a change in behavior');
    }
    ```
    

## platform-specific file extenstions(플랫폼별 확장 프로그램)

- 플랫폼별 코드가 더 복잡한 경우 코드를 별도의 파일로 분리하는 것을 고려해야 한다.
- React Native는 파일이 `.ios.`, `.android.` 확장자를 가지고 있을 때 인식하고 다른 컴포넌트에서 필요로 할 때 관련된 플랫폼 파일을 로드할 수 있다.

### 사용 예시

- 파일 리스트
    
    ```tsx
    BigButton.ios.js
    BigButton.android.js
    ```
    
- 다음과 같이 구성요소를 가져올 수 있다.
    
    ```tsx
    import BigButton from './BigButton';
    ```
    

## **Native-specific extensions (i.e. sharing code with NodeJS and Web)**

- `.native.js` 는 NodeJS/Web과 RN간의 모듈을 공유하지만 Android/iOS 차이가 없는 경우에도 사용할 수 있다.
- 이 기능은 리액트 네이티브, React 간에 공유되는 공통 코드가 있는 프로젝트에 특히 유용하다.

### 사용 예시

- 프로젝트 파일
    
    ```tsx
    Container.js # picked up by Webpack, Rollup or any other Web bundler
    Container.native.js # picked up by the React Native bundler for both Android and iOS (Metro)
    ```
    
- 다음과 같이 확장 없이 계속 가져올 수 있다.
    
    ```tsx
    import Container from './Container';
    ```
    

## 참고 자료

- [https://reactnative.dev/docs/intro-react](https://reactnative.dev/docs/intro-react)
- https://github.com/dev-seomoon/react-native-docs-ko