# Tests on git log:

Here we try to demonstrate some bad cases on git commmits ordering between branche 

## Ordering

In the case of this kind of commit tree:
```
.   A
|\
. | B
| . D
. | C
| . E
|/ 
.   F
```

### Chronological

The default `git log` will return chronological order:

_(Branch) commit_
- (1) A 
- (1) B __Smell 1__
- (2) D __Smell 2__
- (1) C __Smell 1__
- (2) E __Smell 2__
- (1) F __Smell 1__ &&  __Smell 2__

This will create 1 artificial introduction and refactoring each time we go from one branch to the other.

### Topological

We can force topological order by using `git log --topo-order`:

_(Branch) commit_
- (1) A 
- (1) B __Smell 1__
- (1) C __Smell 1__
- (2) D __Smell 2__
- (2) E __Smell 2__
- (1) F __Smell 1__ &&  __Smell 2__

This will reduce the nomber of artificial introduction and refactoring, but not remove them entirely!

## Merge on branches

```
*   2884de7 (HEAD -> master, origin/master) Merge branch 'branch1'
|\
| *   9b97160 (origin/branch1, branch1) Merge branch 'branch2' into branch1
| |\
| | * 96ebd8c (origin/branch2, branch2) C2
| | * c19f3a5 C1
| * | e665998 B3
| |/
| * da6d052 B2
| * 1f1ce09 B1
* | 460acce A2
|/
* cb3df40 A1
```

### Only merge first parent:

Using the command `git log --merges --first-parent` on master will return:

```
commit 2884de7114fcb5217dea4505e7a5fa67b439c7a8 (HEAD -> master, origin/master)
Merge: 460acce 9b97160
Author: Antoine Veuiller <aveuiller@online.net>
Date:   Thu Jul 19 20:57:44 2018 +0200

    Merge branch 'branch1'

commit 82e0f66b784692371f8d79e8adf92f4e0f300c22
Merge: 2b0e6ac ea4cc8d
Author: Antoine Veuiller <aveuiller@online.net>
Date:   Sat Jul 14 16:54:30 2018 +0200

    F: Merge branch 'test'
```

So only the merges onto master