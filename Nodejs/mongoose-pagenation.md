```tsx
const results = await User.find({ deletedAt: null })
	.limit(pageSize)  // 가져올 데이터의 수
	.skip((page - 1) * pageSize)  // offset
	.sort({ createdAt: -1 });  // 생성일 기준 내림차순 정렬
```

## 참고 자료

- https://dev.to/hakimraissi/pagination-with-express-and-mongoose-pnh