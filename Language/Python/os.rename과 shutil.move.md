파이썬에서 `os.rename`은 rename의 대상이 동일한 파일 시스템에 있는 경우에만 정상적으로 동작한다.

만약, 서로 다른 파일 시스템으로 rename을 시도하면 `OSError(18, ‘Invalid cross-device link’)` 오류가 발생하면서 rename에 실패한다.

반면, `shutil.move`는 파일 시스템이 다르더라도 파일을 복사할 수 있다.

# Docker에서 마운트된 디렉토리의 파일 시스템

- Docker 컨테이너에서 volume으로 마운트되지 않은 파일의 파일 시스템은 overlay를 따른다.
- 반면, 마운트된 파일 시스템은 Host OS의 파일 시스템을 따른다.
- 마운트되지 않은 파일 시스템에서 마운트된 파일 시스템으로 파일을 이동할 때, os.rename으로 파일을 이동시키면 서로 다른 파일 시스템이므로 실패하게 된다.

# 참고 자료

- https://stackoverflow.com/questions/42392600/oserror-errno-18-invalid-cross-device-link