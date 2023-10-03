## Multer

- 파일 업로드를 위해 사용되는 multipart/form-data를 다루기 위한 node.js의 미들웨어
- **효율성을 최대화하기 위해 busboy를 기반**으로 하고 있다.
- Multer는 multipart/form-data가 아닌 폼에서는 동작하지 않는다.
- 중간 파일을 인메모리 방식이나 하드디스크에 저장한다.
- **req.files에 들어오는 객체를 사용**한다.

## Formidable

- Express에서만 사용되는 것은 아니고, **일시적인 하드디스크의 공간에 파일을 저장한다.**

## Busboy

- 이벤트 기반/스트림 파서
- Express에만 국한되어 사용되지 않고, 중간 파일을 저장하는 것 대신에 **입력되는 파일을 스트림으로 제공한다.**

## Multiparty

- formidable을 쉽게 사용할 수 있게 감싼 것
- 파일을 스트림 단위로 읽을 수 있는 기능도 제공한다.

## 작은 용량을 원한다면 Formidable or Multer

- **프로토타입을 작성하거나 적은 어드민 유저가 사용하는 경우 중간 파일을 저장**해도 된다.
- 서버가 잠재적으로 메모리 누수나 하드디스크가 꽉 차서 서버가 잠재적으로 중단되는 것을 걱정하지 않아도 된다.
- 파일이 메모리를 이용해도 된다면 사용할 수 있다.

## Express가 없이는 Formidable

- Express 외부에서 파일 업로드 처리 시에는 formidable or Multiparty
- 이 경우 파일을 메모리에 저장하는 옵션은 없고 디스크의 임시 파일에만 저장하는 옵션이 있다.

## Express + In memory로는 Multer

## Express + 하드디스크 임시 저장을 원한다면 Multer or Formidable

## production level의 대용량을 컨트롤 해야한다면 Busboy

- 대용량을 처리해야 한다면 Nodejs의 서버에 중간 파일을 저장하지 않는 것이 가장 좋다.
- **받자마자 분리하여 파일을 서버로 저장하는 것이 좋다.**
    - 파일 서버는 AWS S3 or BLOB 저장을 지원하는 클라우드 스토리지 서비스

## 참고 자료

- https://bedevelopers.tistory.com/232
- https://gareen.tistory.com/78