# 答案解读：
函数位于lab3\kernel\include\object中
## 前置知识
### vmspace:
是操作系统中用于管理进程虚拟内存的概念。
### cap_group:
![image](https://github.com/litterqi/operating-system/assets/123362884/cd0dab43-cc3f-40bc-94d7-bc84deb69db4)

## cap_group_init()
```
slot_table_init(slot_table, size);
cap_group->pid = pid;
cap_group->thread_cnt = 0;
init_list_head(&(cap_group->thread_list));
```
ppt中说明了该函数需要实现的功能：

![image](https://github.com/litterqi/operating-system/assets/123362884/2ea75366-350c-4196-9993-440576d0b6b7)

`slot_table_init()`函数用于初始化slot_table。

将cap_group结构体中的pid成员设置为变量pid的值。

将cap_group结构体中的thread_cnt成员设置为0，表示线程计数的初始值为0。

通过调用init_list_head函数初始化了cap_group结构体中的thread_list成员，使其成为一个空的链表头节点。
## sys_create_cap_group() 
```
new_cap_group = obj_alloc(TYPE_CAP_GROUP, sizeof(struct cap_group));
```
```
cap_group_init(new_cap_group, BASE_OBJECT_NUM, pid);
```
```
vmspace = obj_alloc(TYPE_VMSPACE, sizeof(struct vmspace));
```
![image](https://github.com/litterqi/operating-system/assets/123362884/54d2369e-481f-47e9-a0dd-5cfa2d15cdab)

第1部分调用`obj_alloc()`函数分配一块内存空间，用来存储一个cap_group结构，并将该内存空间的地址赋值给new_cap_group变量。

第2部分调用`cap_group_init()`函数初始化刚创建的cap_group对象，BASE_OBJECT_NUM是用于初始化cap_group对象的基本对象数量，而pid是与该对象相关的进程ID。

第3部分调用`obj_alloc()`函数分配一块内存空间，用来存储一个vmspace结构，并将该内存空间的地址赋值给vmspace变量。
## create_root_cap_group()
```
cap_group = obj_alloc(TYPE_CAP_GROUP, sizeof(struct cap_group));
```
```
cap_group_init(cap_group, BASE_OBJECT_NUM, ROOT_PID);
slot_id = cap_alloc(cap_group, cap_group, 0);
```
```
vmspace = obj_alloc(TYPE_VMSPACE, sizeof(struct vmspace));
```
```
vmspace->pcid = ROOT_PCID;
vmspace_init(vmspace);
slot_id = cap_alloc(cap_group, vmspace, 0);
```
第1部分与上面相同。

第2部分初始化cap_group对象后，调用`cap_alloc()`函数把cap_group放进slots的第0个位置。

第3部分与上面相同。

第4部分将vmspace对象的pcid属性设置为ROOT_PCID。这可能表示将虚拟内存空间（vmspace）对象关联到根页表标识符（ROOT_PCID），这样就可以使用根页表来管理该虚拟内存空间。

调用`vmspace_init()`的函数，用来初始化vmspace对象。调用`cap_alloc()`函数把vmspace放进slots的第0个位置（这里有一点问题，按照ppt上的图应该把vmspace放进slots第1个位置）。
