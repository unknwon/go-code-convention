# 声明语句

### 函数或方法

- 函数或方法的参数排列顺序遵循以下几点原则：
	1. 参数的重要程度与逻辑顺序
	2. 简单类型优先于复杂类型
	3. 尽可能将同种类型的参数放在相邻位置，则只需写一次类型
	
#### 示例

以下声明语句，`User` 类型要复杂于 `string` 类型，但由于 `Repository` 是 `User` 的附属品，首先确定 `User` 才能继而确定 `Repository`。因此，`User` 的顺序要优先于 `repoName`。

	func IsRepositoryExist(user *User, repoName string) (bool, error) { ...