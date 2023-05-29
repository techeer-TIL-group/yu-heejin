```tsx
var mysql = require('mysql');
var conn = mysql.createConnection({
    ...
});

var sql = "INSERT INTO Test (name, email, n) VALUES ?";
var values = [
    ['demian', 'demian@gmail.com', 1],
    ['john', 'john@gmail.com', 2],
    ['mark', 'mark@gmail.com', 3],
    ['pete', 'pete@gmail.com', 4]
];
conn.query(sql, [values], function(err) {
    if (err) throw err;
    conn.end();
});
```

```tsx
let reviewImgList = [];
  
for (i in reviewImg) {
    reviewImgList.push([getReviewIdx[0].reviewIdx,reviewImg[i]])
}

if (reviewImgList.length > 0) {
    await hospitalDao.postReviewImg(connection, [reviewImgList]);
}

async function postReviewImg(connection, [values]) {
    const postReviewImgQuery = `
    INSERT INTO ReviewImg(reviewIdx, reviewImg) VALUES ?;
 
    `;
    const [postReviewImgRow] = await connection.query(
        postReviewImgQuery,
        [values]
    );

    return postReviewImgRow;
}
```

- 2차원 배열의 형태로 파라미터를 전달하면 된다.

## 참고 자료

- [https://yusang.tistory.com/108](https://yusang.tistory.com/108)