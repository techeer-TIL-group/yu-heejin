## fs 모듈 불러오기

- fs 모듈은 Node.js에 내장되어 있기 때문에 별도의 라이브러리 설치없이 바로 불러와서 사용할 수 있다.
- CommonJS 모듈 시스템을 사용하는 프로젝트에서는 `require` 키워드로 불러오고, ES 모듈 시스템을 사용하는 프로젝트에서는 `import` 키워드를 사용한다.

## 비동기 함수 vs 동기 함수

- fs 모듈은 `비동기(asynchronous) API`와 `동기(synchronous) API`를 모두 제공한다.
- 비동기 메서드는 마지막 인자로 콜백 함수를 받고 아무 값도 반환하지 않고, 동기 메서드는 결과값을 반환하며 예외를 일으킬 수 있다.
- 동기 메서드의 이름은 `Sync`로 끝나기 때문에 비동기 메서드와 동기 메서드를 구분할 수 있다.

## 디렉토리 생성하기

- 비동기로 디렉토리를 만들 땐 `mkdir()` 메서드를 사용한다.
    
    ```jsx
    fs.mkdir("our-fs", (err) => console.log(err));
    ```
    
- 같은 작업을 `mkdirSync()`메서드를 사용해서 구현할 수도 있다.
    
    ```jsx
    try {
      fs.mkdirSync("our-fs");
    } catch (err) {
      console.log(err);
    }
    ```
    

## 디렉토리 삭제하기

- 비동기로 디렉토리를 삭제할 땐 `rmdir()` 메서드를 사용한다.
    
    ```jsx
    fs.rmdir("our-fs", (err) => console.log(err));
    ```
    
- 동일한 작업을 `rmdir()` 메서드의 동기 버전인 `rmdirSync()` 메서드를 사용해서 구현할 수 있다.
    
    ```jsx
    try {
      fs.rmdirSync("our-fs");
    } catch (err) {
      console.log(err);
    }
    ```
    

## 파일에 데이터 쓰기

- 비동기로 파일에 데이터를 쓸 수 있도록 `writeFile()` 메서드를 제공한다.
    
    ```jsx
    const file = "test.dat";
    const data = "테스트";
    fs.writeFile(file, data, (err) => console.log(err));
    ```
    
- 마찬가지로 동기 버전인 `writeFileSync()` 메서드도 존재한다.
    
    ```jsx
    try {
      const file = "test.dat";
      const data = "테스트";
      fs.writeFileSync(file, data);
    } catch (err) {
      console.log(err);
    }
    ```
    
- **`writeFile()` 메서드를 사용하면 기존 파일에 있던 데이터를 덮어쓴다.**
    - **기존 파일에 있던 데이터 뒤에 새로운 데이터를 추가**하고 싶다면 `appendFile()` 또는 `appendFileSync()` 메서드를 사용해야 한다.
        
        ```jsx
        try {
          const file = "test.dat";
          const data = "추가분";
          fs.appendFileSync(file, data);
        } catch (err) {
          console.log(err);
        }
        ```
        

## 파일로부터 데이터 읽기

- `readFile()` 메서드를 사용하면 비동기로 파일 데이터를 읽을 수 있다.
    
    ```jsx
    fs.readFile("test.dat", "utf8", (err, data) => {
      if (err) {
        console.error(err);
      } else {
        console.log(data);
      }
    });
    ```
    
    - 주의할 점은 두번째 인자를 `utf8`로 명시하여 인코딩이 되도록 해야한다!
    - 두번째 인자를 생략하면 **콜백 함수의 data 인자로 문자열이 아닌 buffer가 넘어온다.**
- `readFileSync()` 사용 예시
    
    ```jsx
    try {
      const data = fs.readFileSync("test.dat", "utf8");
      console.log(data);
    } catch (err) {
      console.log(err);
    }
    ```
    

## 파일/디렉토리 메타 정보 확인하기

- `stat()` 메서드를 사용하면 파일이나 디렉토리의 메타 정보를 확인할 때 사용한다.
    
    ```jsx
    fs.stat("test.dat", (err, stats) => {
      if (err) {
        console.error(err);
      } else {
        console.log({
          size: stats.size,   // 파일 크기
          mtime: stats.mtime,  // 수정 시간
          isFile: stats.isFile(),  // 파일인지 디렉토리인지 반환
        });
      }
    });
    
    // { size: 18, mtime: 2021-11-13T03:14:37.596Z, isFile: true }
    ```
    
- `statSync()`도 동일한 작업을 구현할 수 있다.
    
    ```jsx
    try {
      const stats = fs.statSync("test.dat");
      console.log({
        size: stats.size,
        mtime: stats.mtime,
        isFile: stats.isFile(),
      });
    } catch (err) {
      console.error(err);
    }
    ```
    

## Stream 사용하기

- 데이터를 클라이언트에 전달하거나 클라이언트로부터 데이터를 받을 때 `ReadableStream/WritableStream`을 사용한다.

### createReadStream

```jsx
fs.createReadStream(path, options);
// fs.ReadStream 객체를 반환한다.
```

- **stream 형태로 파일을 읽어온다.**
    - `readFile`의 경우 파일 전체를 메모리에 올리기 때문에 메모리 사용량이 매우 늘어난다.
- stream을 사용하면 chunk로 파일을 읽어오기 때문에, 메모리에 저장되는 데이터를 최소한으로 유지할 수 있다.
- 찾고자하는 파일을 찾은 뒤에는 `stream.destroy()` 메소드를 사용해 나머지 데이터까지 읽어오는 것을 차단할 수 있기 때문에 파일의 크기가 커진다면 readFile에 비해 처리 속도가 빠르다.
- path: 읽어올 파일의 경로를 포함한다.
    - 문자열, 버퍼 또는 url일 수도 있다.
- options: chunk로 쪼갤 크기를 정한다.

### createWriteStream

```jsx
fs.createWriteStream( path, options )
// path: 읽어올 파일의 경로
// fs.writeStream 객체를 반환한다.
```

```jsx
var wstream = fs.createWriteStream('myOutput.txt');

wstream.write('Hello world!\n');
wstream.write('Another line\n');
wstream.end();
```

- stream 형태로 파일을 쓴다.
- 기본적으로 utf8 인코딩으로 파일을 쓴다.
- 만약 다른 인코딩으로 쓰고 싶으면 옵션을 지정해주거나 각 write에 인코딩 방식을 지정해준다.
    
    ```jsx
    // 이렇게 createWriteStream 에 옵션으로 지정하거나,
    var options = { encoding: 'utf16le' };
    var wstream = fs.createWriteStream('myOutput.txt', options);
    
    // 아니면 이렇게 각 write 에서 인코딩을 지정해줄 수도 있다.
    wstream.write(str, 'utf16le');
    ```
    
- 바이너리 파일을 쓰려면 Buffer를 사용한다.
    
    ```jsx
    var crypto = require('crypto');
    var fs = require('fs');
    var wstream = fs.createWriteStream('myBinaryFile');
    
    // 100 바이트의 랜덤 버퍼를 만든다.
    var buffer = crypto.randomBytes(100);
    wstream.write(buffer);
    // 또다른 100 바이트의 랜덤 버퍼를 만들어서 바로 쓴다.
    wstream.write(crypto.randomBytes(100));
    wstream.end();
    ```
    

## 참고 자료

- https://www.daleseo.com/js-node-fs/
- https://lsy26499.tistory.com/50
- https://www.geeksforgeeks.org/node-js-fs-createreadstream-method/
- [https://velog.io/@zerozoo-front/createReadStream으로-buffer에-스트림-만들기](https://velog.io/@zerozoo-front/createReadStream%EC%9C%BC%EB%A1%9C-buffer%EC%97%90-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- https://basketdeveloper.tistory.com/69
- https://programmingsummaries.tistory.com/362
- https://www.geeksforgeeks.org/node-js-fs-createwritestream-method/