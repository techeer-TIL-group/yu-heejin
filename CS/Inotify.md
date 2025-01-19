리눅스 커널 2.6.13 버전부터 파일 시스템 이벤트를 모니터링할 수 있는 `Inotify`라는 파일 시스템 모니터링 기능이 제공된다.

# Inotify

파일이나 디렉토리를 개별적으로 모니터링 할 수 있도록 해준다.

리눅스 커널 기능으로, 파일 시스템을 감시하고 있다가 삭제, 읽기, 쓰기, `umount` 연산까지 모두 인식할 수 있고, 세부 기능으로 파일 시작과 목적지를 파악해 이동까지도 추적이 가능하다.

재귀적으로 모니터링 되는 것은 아니기 때문에, 모든 하위 디렉토리를 모니터링하려면 각 디렉토리에 대해 모니터링하는 것이 필요하다.

# Inotify API

```c
#include <sys/inotify.h>

int inotify_init(void);

/**
	return: 초기화된 inotify instance를 가리키는 파일 디스크립터
**/
```

- inotify instance를 초기화하기 위한 함수이다.
    - 성공 시 파일 디스크립터를 리턴한다.
    - 오류 발생 시 -1을 리턴하고, errno가 설정된다.

```c
int inotify_add_watch(int fd, const char *pathname, uint32_t mask);

/**
	parameter:
	 - fd: inotify_init()에서 리턴받은 file descriptor
	 - pathname: 모니터링 리스트에 생성되거나 수정되는 파일
	 - mask: pathname에 모니터링할 이벤트 지정 마스크
	 
	return: 추가 또는 수정된 inotify instance를 가리키는 file descriptor
**/
```

- file descriptor가 가리키는 inotify instance의 모니터링 리스트에 새로운 파일 또는 디렉토리를 추가하거나, 기존에 추가되어 있던 항목을 수정하기 위해 사용된다.
    - 기존에 추가된 파일이나 디렉토리인 경우, mask만 수정된다.

```c
int inotify_rm_watch(int fd, int wd);

/**
	parameter:
		- fd: inotify_init()에서 리턴받은 file descriptor
		- wd: inotify_add_watch()의 이전 호출이 리턴한 file descriptor
		
	return: 수정된 inotify instance를 가리키는 file descriptor
**/
```

- file descriptor fd가 가리키는 inotify instance로부터 wd로 지정한 모니터링 리스트를 제거한다.

# Inotify Event Mask

inotify_add_watch의 mask 인자와 inotify 파일 디스크립터에서 read할 때 반환되는 inotify_event 구조체의 mask 필드로, inotify 이벤트들을 나타내는 비트 마스크이다.

| Mask | 입력 | 출력 | 설명 |
| --- | --- | --- | --- |
| IN_ACCESS | O | O | 파일에 접근했음 |
| IN_ATTRIB | O | O | 파일의 메타데이터가 변경되었음 (`chmod`, `link`, `chown` 등) |
| IN_CLOSE_WRITE | O | O | Write 위해 열린 파일이 닫힘 |
| IN_CLOSE_NOWRITE | O | O | 쓰기용으로 열리지 않은 파일 내지 디렉토리가 닫힘 |
| IN_CLOSE | O |  | IN_CLOSE_WRITE | IN_CLOSE_NOWRITE |
| IN_CREATE | O | O | 모니터링 디렉토리 내의 파일 or 디렉토리 생성됨 |
| IN_DELETE | O | O | **모니터링 디렉토리 내의 파일** or 디렉토리 삭제됨 (모니터링 대상 내의 파일이나 디렉토리) |
| IN_DELETE_SELF | O | O | **모니터링 중인 디렉토리** or 파일이 삭제됨 (모니터링 파일이나 폴더 자체)
 - `mv`와 같이 다른 파일 시스템으로 객체를 옮기는 경우에도 발생한다. |
| IN_MODIFY | O | O | 파일이 수정됨 |
| IN_MOVE_SELF | O | O | 모니터링 중인 디렉토리 or 파일이 이동됨 |
| IN_MOVE_FROM | O | O | 파일이 모니터링 중인 디렉토리 밖으로 이동됨 |
| IN_MOVE_TO | O | O | 파일이 모니터링 중인 디렉토리로 이동됨 |
| IN_MOVE | O | O | IN_MOVED_FROM | IN_MOVED_TO |
| IN_OPEN | O | O | 파일이 열림 |
| IN_ALL_EVENTS | O |  | 위의 모든 이벤트를 포함함 |
| IN_DONT_FOLLOW | O |  | Symbolic Link를 역참조하지 않음 |

# 참고 자료

- https://blog.naver.com/gaeean/60112843607
- https://sonseungha.tistory.com/436
- [https://wariua.github.io/man-pages-ko/inotify(7)/](https://wariua.github.io/man-pages-ko/inotify%287%29/)