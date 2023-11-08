# 答案解读：
## do_page_fault()
pgfault.c位于lab3\kernel\arch\aarch64\irq中
```
ret = handle_trans_fault(current_thread->vmspace, fault_addr);
```
将缺页异常转发给`handle_trans_fault()`函数，将当前线程的虚拟内存空间和传输错误的地址作为参数传递给该函数，以便处理传输错误并进行相应的后续操作。
## handle_trans_fault()
pgfault_handler.c位于lab3\kernel\mm中
```
pa = get_page_from_pmo(pmo, index);
```
```
pa = virt_to_phys(get_pages(0));
memset(phys_to_virt(pa), 0, PAGE_SIZE);
commit_page_to_pmo(pmo, index, pa);

#ifdef ENABLE_MEMORY_PAGE_SWITCH
  alloc_page_in_memory(vmspace->pgtbl, fault_addr, pa, perm);
#else
  map_range_in_pgtbl(vmspace->pgtbl, fault_addr, pa, PAGE_SIZE, perm);
#endif
```
```
#ifdef ENABLE_MEMORY_PAGE_SWITCH
  alloc_page_in_memory(vmspace->pgtbl, fault_addr, pa, perm);
#else
  map_range_in_pgtbl(vmspace->pgtbl, fault_addr, pa, PAGE_SIZE, perm);
#endif
```
![image](https://github.com/litterqi/operating-system/assets/123362884/15d27be6-d6a1-46bf-a9ff-fe160ec2ce9d)

第1部分通过`get_page_from_pmo()`函数获取PMO中offset对应的物理页（检查PMO中当前的fault地址对应的物理页是否存在）。

第2部分：
![image](https://github.com/litterqi/operating-system/assets/123362884/2455a9af-e71d-47c8-af00-29b698c1b729)

第3部分与第2部分后半部分相同。
## sys_putc() sys_getc()
syscall.c位于lab3\kernel\syscall中
```
uart_send(ch);
```
```
return uart_recv();
```

## __chcore_sys_putc() u32 __chcore_sys_getc()
raw_syscall.h位于lab3\libchcore\include\chcore\internal中
```
__chcore_syscall1(__CHCORE_SYS_putc, ch);
```
```
ret = (u32) __chcore_syscall0(__CHCORE_SYS_getc);
```
