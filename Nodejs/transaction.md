## 트랜잭션(Transaction)

- 쪼개지면 안 되는 업무 처리의 단위
- 여러 개의 연속된 쿼리로 구성되어 있지만 하나의 작업처럼 동작해야 한다.
- 결과값으로는 롤백과 커밋이 있다.
    - **롤백**은 부분 작업이 실패했을 때 DB를 트랜잭션 실행 전으로 되돌린다.
    - **커밋**은 정상적으로 완료된 경우 변경사항을 한꺼번에 DB에 반영한다.

## async/await 패턴

```jsx
var express = require('express')
var router = express.Router()
const pool = require('../database/pool')

router.post('/:boardId/comment', async (req, res, next) => {
  const { boardId } = req.params
  const { content } = req.body

  const conn = await pool.getConnection()
  try {
    await conn.beginTransaction() // 트랜잭션 적용 시작

    const ins = await conn.query('insert into board_comment set ?', { board_id: boardId, content: content })
    const upd = await conn.query('update board set comment_cnt = comment_cnt + 1 where board_id = ?', [boardId])

    await conn.commit() // 커밋
    return res.json(ins)
  } catch (err) {
    console.log(err)
    await conn.rollback() // 롤백
    return res.status(500).json(err)
  } finally {
    conn.release() // conn 회수
})
```

- `await conn.beginTransaction()` 을 통해 트랜잭션을 실행한다.
- 트랜잭션을 실행하면, **중간에 에러가 발생한 경우 `rollback()`을 실행해 변경 사항을 반영하지 않는다.**
- `conn.commit()`으로 트랜잭션의 결과를 반영한다.

## 참고 자료

- [https://gofnrk.tistory.com/64](https://gofnrk.tistory.com/64)
- [https://balmostory.tistory.com/48](https://balmostory.tistory.com/48)
- [https://yusang.tistory.com/93](https://yusang.tistory.com/93)