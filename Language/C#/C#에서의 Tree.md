## Tree

- 계층적인 자료를 나타내는데 자주 사용되는 자료구조
- 하나 이하의 부모노드와 복수 개의 자식 노드들을 가질 수 있다.
- 트리는 하나의 루트 노드에서 출발하여 자식 노드들을 갖게되며, 각각의 자식 노드는 또한 자신의 자식 노드들을 가질 수 있다.
- 트리 구조는 한 노드에서 출발하여 다시 자기 자신의 노드로 돌아오는 순환 구조를 가질 수 없다.

## 트리의 특징

- 트리에는 사이클이 존재할 수 없다.
    - 만약 사이클이 만들어진다면 트리가 아니라 그래프이다.
    - 하지만 사이클이 존재하지 않는다고 무조건 트리는 아니고, 트리의 경우 사이클이 존재하지 않고 모든 노드가 간선으로 연결되어 있어야한다.
- 모든 노드는 자료형으로 표현이 가능하다.
- 루트에서 한 노드로 가는 경로는 유일한 경로 뿐이다.
- 노드의 개수가 N개이면, 간선은 N-1개를 가진다.

## 이진 트리(Binary Tree)

- 이진 트리는 자식 노드가 0개~2개인 트리를 말한다.
- 따라서 이진 트리 노드는 데이터 필드와 왼쪽 노드 및 오른쪽 노드를 갖는 자료 구조로 되어있다.
- 이진 트리는 루트 노드로부터 출발하여 원하는 특정 노드에 도달할 수 있는데, 이 때의 검색 시간은 O(n)이 소요된다.

## 트리 순회

### 전위 순회(preorder)

- 뿌리를 먼저 방문

### 중위 순회(inorder)

- 왼쪽 하위 트리를 먼저 방문한 후 root 방문

### 후위 순회(postorder)

- 왼쪽, 오른쪽 하위 트리 모두 방문 후 뿌리 방문

### 층별 순회(levelorder)

- 위쪽 node로부터 아래 방향으로 차례대로 방문

## C#에서의 이진트리

```csharp
// 이진 트리 노드 클래스
public class BinaryTreeNode<T>
{
	public T Data {get; set;}
	public BinaryTreeNode<T> Left {get; set;}
	public BinaryTreeNode<T> right {get; set;}

	public BinaryTreeNode(T data)
	{
		this.Data = data;
	}
}

// 이진 트리 클래스
public class BinaryTree<T>
{
	public BinaryTreeNode<T> Root {get; set;}
	private Comparer<T> comparer = Comparer<T>.Default;

	// 전위 순회
	public void PreOrderTraversal(BinaryTreeNode<T> node)
	{
		if (node == null) return;

		Console.WriteLine(node.Data);
		PreOrderTraversal(node.Left);
		PreOrderTraversal(node.Right);
	}

	public void insert(T val)
	{
		BinaryTreeNode<T> node = root;
	
		if (node == null)
		{
			root = new BinaryTreeNode<T>(val);
			return;
		}

		while (node != null)
		{
			int result = comparer.Compare(node.tData, val);    // 두 값의 크기 비교

			if (result == 0)      // 두 값이 같은 경우
			{
				throw new InvalidDataException("Duplicate value");
				return;
			}
			else if (result > 0)      // 첫번째 값이 두번째 값보다 큰 경우, 즉, 삽입하고자 하는 값이 더 작은 경우
			{
				if (node.btnLeft == null)
				{
					node.btnLeft = new BinaryTreeNode<T>(val);
					return;
				}

				node = node.btnLeft;
			}
			else    // 삽입하고자 하는 값이 더 큰 경우
			{
				if (node.btnRight == null)
				{
					node.btnRight = new BinaryTreeNode<T>(val);
					return;
				}
	
				node = node.btnRight;
			}
	}
}
```

## C# 이진 트리 순회

```csharp
public class TreeNode {
	public int val;
	public TreeNode left;
	public TreeNode right;

	// 생성자
}

public class BinaryTreeTraversal {
	public void Preorder(TreeNode node) {
		if (node == null) return;
		Console.Write(node.val + " ");
		Preorder(node.left);
		Preorder(node.right);
	}

	public void Inorder(TreeNode node) {
		if (node == null) return;
		Inorder(node.left);
		Console.Write(node.val + " ");
		Inorder(node.right);
	}

	public void Postorder(TreeNode node) {
		if (node == null) return;
		PostOrder(node.left);
		PostOrder(node.right);
		Console.Write(node.val + " ");
	}
}
```

## 참고 자료

- https://m.blog.naver.com/rlakk11/60159303809
- https://www.csharpstudy.com/DS/tree.aspx
- https://gamemakerslab.tistory.com/33
- https://da-new.tistory.com/186
- [https://thepathfinder.co.kr/entry/C-비선형-자료구조-트리](https://thepathfinder.co.kr/entry/C-%EB%B9%84%EC%84%A0%ED%98%95-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%A6%AC)