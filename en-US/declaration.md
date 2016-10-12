# Declaration

### Functions or Methods

Order of arguments of functions or methods should generally apply following rules (from left to right):

1. More important to less important.
2. Simple types to complicated types.
3. Same types should be put together whenever possible.

#### Example

In the following declaration, type `User` is more complicated than type `string`, but `Repository` belongs to `User`, so `User` is more left than `Repository`.

```Go
func IsRepositoryExist(user *User, repoName string) (bool, error) { ...
```
