# 答案解读：
irq_entry.S位于lab3\kernel\arch\aarch64\irq中

```
exception_entry sync_el1t
exception_entry irq_el1t
exception_entry fiq_el1t
exception_entry error_el1t

exception_entry sync_el1h
exception_entry irq_el1h
exception_entry fiq_el1h
exception_entry error_el1h
	
exception_entry sync_el0_64
exception_entry irq_el0_64
exception_entry fiq_el0_64
exception_entry error_el0_64

exception_entry sync_el0_32
exception_entry irq_el0_32
exception_entry fiq_el0_32
exception_entry error_el0_32	
```
```
bl unexpected_handler
```
```
bl handle_entry_c
```
第1部分：
![image](https://github.com/litterqi/operating-system/assets/123362884/2bf859ba-ba57-4575-8efb-56c44bbbda7c)

![image](https://github.com/litterqi/operating-system/assets/123362884/70515860-63fa-4d23-9708-7136aca84132)

根据ppt提示，第2部分和第3部分即是分别跳转到`unexpected_handler()`函数和`handle_entry_c()`函数处理异常。第2部分位于sync_el1t中，第3部分位于sync_el1h中。
