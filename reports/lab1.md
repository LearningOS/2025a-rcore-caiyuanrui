# chapter3练习

## 编程作业

trace 的读写内存地址的功能很直接，没有考虑地址非法的情况，也没有处理任何其他异常，不过测例并不涉及此情况。
统计当前任务的 syscall 调用情况是直接在 `TaskManagerInner` 中新增了一个字段 `syscall_counter` 用于记录每种 syscall 在每个 task 的调用数量。
当 Trap 发生时，如果 scause 是 `UserEnvCall`, 则调用增加计数器，然后再调用后续的 syscall，这样做刚好也符合 *本次调用也计入统计* 的实验要求。

## 简答题

### Q1
三个 bad 测例运行结果如下
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003a4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.

### Q2
1. L40：刚进入 __restore 时，sp 指向 kernel stack，__restore 实现用户态和内核态的切换，也可以作为第一次运行用户程序时的引导程序。
2. L43-L48：特殊处理了 sp、sstatus 和 sepc。把 sp 存放在 sscratch，为后续交换内核与用户栈指针做准备；sstatus 和 sepc 分别是 trap 发生时的特权级和正在执行指令的地址，是能够让用户程序从发生 trap 的地址继续执行下去的必须设置的寄存器。
3. L50-L56：x2(sp) 已经被保存在 sscratch 了，x4(tp) 用不到
4. L60：此后 sp 指向用户栈，sscratch 指向内核栈
5. 状态切换发生在 sret，执行 sret 后会跳转到 sepc 中设置的地址，sepc 在 L47 已经设置成用户程序发生 trap 时的 pc 值了
6. 此后 sscratch 和 sp 还分别指向用户栈和内核栈
7. ecall

## 荣誉准则
在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

无

此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

无

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。
