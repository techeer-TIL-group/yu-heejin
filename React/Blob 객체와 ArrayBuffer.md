## ArrayBuffer

```jsx
let arrBuffer = new ArrayBuffer(16);
```

- 레퍼런스 타입으로 되어있으며, **고정된 길이의 연속된 메모리 공간을 할당해 사용하겠다고 알려주는 역할**을 한다.
- 연속된 공간의 메모리에 할당되는 고정 길이에 대한 참조 객체
- 오디오 전용인 `AudioBuffer`, 미디어 전용인 `SourceBuffer`도 있다.
- **이름에 array가 있다고 이것이 배열이라고 생각하면 안된다!**
    - 배열과 달리 `ArrayBuffer`는 그 길이가 변동되지 않고 고정되어 있다.
    - `ArrayBuffer`는 정확하게 **메모리의 어느 바이트만큼 사용하겠다고 명시하는 역할**을 하고 있으며, 해당 버퍼의 내용에 접근하기 위해 **배열처럼 `array[0]`과 같은 행위의 get은 불가능하다.**
- 직접 콘텐츠를 조작할 순 없지만, **특정 형식으로 버퍼를 나타내는 DataView를 만들어 버퍼의 콘텐츠를 읽고 쓰기위해 사용한다.**
    - 이 때 해당 DataView 중 하나가 uint8Array(8비트 부호 없는 정수)이고, 해당 8비트 이터러블한 정수 배열을 블롭 안에 넣어주어 사용해야한다.
- ArrayBuffer의 공간 안에 있는 내용을 조작하려면 특수한 `view` 역할을 하는 객체들을 사용해야한다. (`TypedArray`)
    - view 객체는 자기 스스로는 어떠한 데이터도 저장하고 있지 않고, 그저 ArrayBuffer의 내부를 들여다보기 위한 수단으로 사용한다.
    
    ![Untitled](https://user-images.githubusercontent.com/58500558/160519525-7dd5f99c-1678-429c-8b43-085e0c544205.png)
    
    ```jsx
    let buffer = new ArrayBuffer(16); // 16바이트(128비트) 메모리공간 할당할게요
    let view = new Uint32Array(buffer) // 4바이트(32비트) 꼴로 버퍼에 있는 내용 읽어들일게요
    
    //즉, buffer은 [00000000 00000000 ...... (총 16개)] 로 형성되어있고
    // view가 이것을 인식할 때 4바이트씩, 즉 8개짜리 4개씩 단위로 끊어 읽는다는 소리다.
    // [00000000 00000000 00000000 00000000, 00000000 00000000 ......]
    
    console.log(view.length) // 분할해서 읽는 공간의 갯수가 총 4개네요 (16바이트 / 4바이트)
    console.log(view.byteLength) // 총 바이트크기는 16바이트구요
    
    view[0] = 123456; // 분할공간 중 첫번째에 123456이라는 숫자데이터를 넣어주세요
    console.log(view) // 0:123456, 1:0, 2:0, 3:0
    ```
    
    - `Uint8Array`: 매 바이트마다 독자적인 정수로 읽어들여 다루겠다는 의미
    - `Uint16Array`: 매 2바이트마다 독자적인 정수로 읽어들여 다루겠다는 의미
    - `Uint32Array`: 매 4바이트마다 독자적인 정수로 읽어들여 다루겠다는 의미
    - `Uint64Array`: 매 8바이트마다 float값으로 읽어들여 다루겠다는 의미로, 기능값에 소숫점 이하가 포함될 수 있음을 의미

## Stream, Buffer

![Untitled](https://user-images.githubusercontent.com/58500558/160525707-7729ccde-8474-4fbc-a625-24f7923600b9.png)

예를 들어, 큰 동영상 파일이 CDN 서버에 캐싱되어 저장되어 있고, 유저가 이를 요청했다고 가정하자.

- 일반적으로 동영상 재생이라고 하는 것은 이 영상 파일을 통째로 내려받은 후, 재생하는 것을 의미한다.
- 또는 실시간 방송과 같이 영상 내용을 송출할 때에는 영상이라는 실시간 데이터를 계속 전달해줘야 유저들이 이를 볼 수 있다.
    - 즉, 어떤 식으로든 데이터를 잘게 쪼개서 전송해야하는 상황이 발생한다는 것이다.
- Buffer란 일정 구획만큼의 데이터를 쪼개서 전달되는 stream을 저장한 후 일정 크기가 도달하면 출력 장치나 동영상 플레이어로 전달해주는 중개자 역할을 하는 객체이다.
    - binary 데이터를 임시로 받아 저장할 수 있는 기능을 가지고 있다.
- 기존에는 자바스크립트에서 메모리에 할당되는 데이터의 크기를 관여할 수 없었으나, 자바스크립트의 용도가 다양해지면서 오디오나 비디오와 같은 이진 데이터 역시 다룰 필요성이 생겼다.

## Blob

```jsx
const blob = new Blob(new ArrayBuffer(바이트), {type: "video/mp4"})
//참고로 Blob 생성자의 첫번째 인자로는 ArrayBuffer, ArrayBufferView, Blob, DOM string 단독 혹은 배열이 올 수 있다고 한다.

new Blob([new ArrayBuffer(8)], { type: 'video/mp4' });
new Blob(new Uint8Array(buffer), { type: 'image/png' });  
new Blob(['<div>Hello Blob!</div>'], {
  type: 'text/html',
  endings: 'native'
});
```

- Blob은 Binary Large Object의 줄임말로, 보통 **이미지/사운드/비디오와 같이 매우 큰 이진 데이터들을 손쉽게 처리할 수 있도록 만들어진 생성자이다.**
    - 멀티미디어 파일 바이너리를 객체 형태로 저장한 것을 의미한다.
- 멀티미디어 파일들은 대다수 용량이 큰 경우가 많기 떄문에, 이를 데이터베이스에 효과적으로 저장하기 위해 고안된 자료형이다.
- 자바스크립트에서 **텍스트, 이미지, 사운드, 비디오와 같은 멀티미디어 데이터를 다룰 때 사용한다.**
    - 비동기적으로 데이터를 읽기 위해서 파일 리더 객체를 사용하게 되는데, 이때 블롭 객체를 이용하게 된다.
    - 즉, **블롭 객체로 변경한 후 파일 리더 객체를 사용하게 되면 파일을 읽고 컴퓨터에 저장도 가능하다.**
- 객체 프로퍼티인 `size`는 바이트 크기를 의미하며, `type`은 MIME 타입을 의미한다.
    - MIME 타입이란 전송 데이터의 타입으로, 보통 데이터 응답자가 리소스를 내려받았을 떄 어떤 동작을 기본적으로 해야 하는지 결정하도록 만들어주는 지표
- 만약 해당 Blob에 들어가는 데이터의 크기가 매우 큰 경우, 한번에 Blob 객체를 보내는 것은 부담스러울 수 있다.
    - 이 때 Blob 객체 내에 내장되어 있는 `slice` 메소드를 이용하면 큰 객체를 작은 단위로 쪼갤 수 있다.
        
        ```jsx
        const blob = new Blob(buffer, {type: "image/jpeg");
        const chunk = blob.slice(0, 1024, "image/jpeg") // 0바이트부터 1KB까지 짜르고, 타입은 jpeg로
        ```
        

### base64와의 차이점

- base64는 바이너리 데이터를 다루기 위해 텍스트(문자열) 형태로 저장한 포맷
- blob이란 바이너리 데이터를 다루기 위해 객체(object)로 저장하는 것

## blob 이미지 다루기

### Blob → ObjectURL

- Blob 객체는 곧바로 사용할 수 없고, url 타입으로 따로 변환을 해서 사용한다.
    
    ```jsx
    fetch('https://www.business2community.com/wp-content/uploads/2014/04/Free.jpg')
    	.then((response) => response.blob())
    	.then((blob) => {
    	  const url = URL.createObjectURL(blob);
    	  document.querySelector('img').src = url;
    	  document.querySelector('a').href = url;
    	});
    ```
    
    - `URL.createObjectURL` 메서드를 활용하면 Blob 객체를 가지고 고유한 URL을 생성할 수 있다.
    - 이 때 생성되는 URL은 blob:/ 의 형태를 띄며, 변환된 URL은 source를 속성으로 가지는 모든 HTML 태그와 CSS 속성에서 사용 가능하다.
- Blob 객체는 별도로 type을 명시하기 때문에 Blob 객체를 다운로드/업로드 하는 과정에서 네트워크 요청에서의 Content-Type은 자연스럽게 명시된 type으로 매칭된다.
- 변환된 URL은 현재 탭의 브라우저 메모리에 저장되고, 저장된 URL은 매핑된 Blob 객체를 참고하고 있는 형태이다.
    - 따라서 변환된 URL은 항상 현재 문서에만 유효하다. (현재 브라우저 메모리에 적재된 상태)
    - 변환된 URL을 새로고침하거나 다른 페이지에서 사용하려고 하면 제대로 사용할 수 없다.
- 하지만 이와 관련해서 **메모리 이슈가 존재**한다.
    - 명시적으로 해당 URL이 해제되기 전까지 브라우저는 해당 URL이 유효하다고 판단하기 때문에 자바스크립트 엔진에서 가비지 컬렉션이 이루어지지 않는다.
    - 따라서 blob URL을 더이상 사용하지 않을 시점이라고 판단되면 명시적으로 해제해주는 것이 좋다.
        
        ```jsx
        // Create Blob URL
        const objectURL = window.URL.createObjectURL(blob);
        
        // Revoke Blob URL after DOM updates..
        window.URL.revokeObjectURL(objectURL);
        ```
        

### Blob → Base64

```jsx
fetch('https://play-lh.googleusercontent.com/hYdIazwJBlPhmN74Yz3m_jU9nA6t02U7ZARfKunt6dauUAB6O3nLHp0v5ypisNt9OJk')
    .then((res) => res.blob())
    .then((blob) => {
        const reader = new FileReader();
        reader.onload = () => {
            const base64data = reader.result;
            console.log(base64data)
        }
        reader.readAsDataURL(blob);
    })
```

- Blob 객체로 base64의 형태로 변환 가능하고, 변환된 문자열을 바로 src로써 사용할 수 있다.
    - 이러한 URL은 보통 `data url`이라고 부르는데, 이렇게 변환된 base64 형태의 URL은 `URL.createObjectURL` 메서드를 통해 변환된 것과 달리 어디에서나 사용 가능하다. (문자열 자체가 이미지 데이터이기 때문)

### Base64 → ArrayBuffer → Blob

```jsx
// bas64를 blob으로 변환해주는 함수
function b64toBlob(b64Data, contentType = '', sliceSize = 512) {
   const image_data = atob(b64Data.split(',')[1]); // data:image/gif;base64 필요없으니 떼주고, base64 인코딩을 풀어준다

   const arraybuffer = new ArrayBuffer(image_data.length);
   const view = new Uint8Array(arraybuffer);

   for (let i = 0; i < image_data.length; i++) {
      view[i] = image_data.charCodeAt(i) & 0xff;
      // charCodeAt() 메서드는 주어진 인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환
      // 비트연산자 & 와 0xff(255) 값은 숫자를 양수로 표현하기 위한 설정
   }

   return new Blob([arraybuffer], { type: contentType });
}

const contentType = 'image/png';
const b64Data =
   'data:image/gif;base64,R0lGODlhAAEAAcQAALe9v9ve3/b393mDiJScoO3u74KMkMnNz4uUmKatsOTm552kqK+1uNLW18DFx3B7gP///wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH/C1hNUCBEYXRhWE1QPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS4wLWMwNjAgNjEuMTM0Nzc3LCAyMDEwLzAyLzEyLTE3OjMyOjAwICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtbG5zOnhtcD0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wLyIgeG1wTU06T3JpZ2luYWxEb2N1bWVudElEPSJ4bXAuZGlkOjAxODAxMTc0MDcyMDY4MTE5QjEwQjYyNTc4MkUxRURBIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjEzN0VEMDZBQjMyNzExRTE4REMzRUZGMkFCOTM1NkZBIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjEzN0VEMDY5QjMyNzExRTE4REMzRUZGMkFCOTM1NkZBIiB4bXA6Q3JlYXRvclRvb2w9IkFkb2JlIFBob3Rvc2hvcCBDUzUgTWFjaW50b3NoIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MDI4MDExNzQwNzIwNjgxMTlCMTBCNjI1NzgyRTFFREEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MDE4MDExNzQwNzIwNjgxMTlCMTBCNjI1Nzgy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';

const blob = b64toBlob(b64Data, contentType); // base64 -> blob
const blobUrl = URL.createObjectURL(blob); // object url 생성

const img = document.createElement('img');
img.src = blobUrl;
document.body.appendChild(img);
```

- base64 데이터를 blob으로 바꾸기 위한 중간 과정으로 arrayBuffer가 사용된다.

### Blob → ArrayBuffer

- 정통적인 FileReader 객체로 변환
    
    ```jsx
    let fileReader = new FileReader();
    
    fileReader.readAsArrayBuffer(blob); // ArrauBuffer 형태로 데이터를 읽어 변환
    
    fileReader.onload = function(event) {
      let arrayBuffer = fileReader.result;
    };
    ```
    
- 대용량 blob의 경우 promise를 사용한 비동기 처리
    
    ```jsx
    const blob = new Blob(...);
    const buf = await blob.arrayBuffer(); // 비동기로 arrayBuffer로 변환 (대용량 일경우)
    ```
    

## 참고 자료

- [https://velog.io/@chltjdrhd777/ArrayBuffer-와-Blob](https://velog.io/@chltjdrhd777/ArrayBuffer-%EC%99%80-Blob)
- [https://velog.io/@joy37/BLOB](https://velog.io/@joy37/BLOB)
- [https://inpa.tistory.com/entry/JS-📚-Base64-Blob-ArrayBuffer-File-다루기-정말-이해하기-쉽게-설명](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Base64-Blob-ArrayBuffer-File-%EB%8B%A4%EB%A3%A8%EA%B8%B0-%EC%A0%95%EB%A7%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85)