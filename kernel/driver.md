# 字符驱动模型

	1. __init
		1. 设备号(主, 次设备号)
			1. 设备号是由主,次设备号拼接而成.
			2. 主,次设备号的拼接(高12位为主设备号, 低20位为次设备号)
			3. register_chrdev_region().
			4. 字符设备号存储在哈希表中.
			5. 主设备1~255, 次设备0~255.
		2. 注册设备
			1. 初始化cdev结构体.
				1. 给.owner = THIS_MODULE
				2. 给.fops = file_operations
				3. 给.name = 设备字
				4. 给.count = 设备数量
				5. 给.dev = 设备号
			2. 将cdev加入由系统维护的字符设备双向循环链表中.
	2. __exit
		1. device_destroy();
		2. class_destroy();  udev
		3. cdev_del()
		4. unregister_chrdev_region()
	3. 模块宏-> license, 作者, 描述.
	4. file_operations结体
	5. open()函数方法 -> 自旋锁()
	6. read()函数方法 -> 返回值为0表示EOF. 标记读的偏移量.
	7. write()函数方法 -> 标记写的偏移量.
	8. ioctl()函数方法 -> _IO: 1.nr(0~7) 2:type(8~15) 3.size(16~29) 4.dir(30~31)
	                      _IOR
	                      _IOW
	                      _IORW
	9. release()函数方法 -> 应用close()
	10. 自动创建设备节点.
	    1. class_create(THIS_MODULE, "hello") -> /sys/class/hello
	    2. device_create("led")   -> /dev/led

## misc:
	1. misc_register() -> 注册misc设备到miscedevice结构体中
	2. misc_unregister() -> 注销misc设备
	3. misc_init()  -> misc设备初始化
		1. 申请设备号, 主10, 次0, 数量 256
		2. 注册misc设备到字符设备双向循环链表中.
		3. file_operations -> open()
	4. misce_open()
		1. 使用次设备号,在miscdevice结构体中, 查找misc设备.
		2. 用misc设备中的fops替换file结体中的fops方法.
		3. 调用misc设备中实现的file_operations中open方法.

## 数据结构:
	1. 单向链表 (设备号)
	2. 双向循环链表
		1. container_of()
		2. list_for_each()

## 锁操作

### 原子操作
	1. atomic_t v = ATOMIC_INIT(1) 初始化原子操作变量为1
	2. atomic_inc(&v) 对变量加1操作
	3. atomic_dec_and_test(&v) 先对变量减一操作,再测试变量是否等于0.
		 如果等于返回真.否则返回假.注: 变量值不会自动加1.

### 自旋锁
	0. spinlock_t lock;      定义自旋锁结构体变量
	1. spin_lock_init(&lock); 初始化自旋锁结构体.
	2. spin_lock(&lock);      加锁
	3. spin_unlock(&lock);    解锁
	注意: 自旋锁不能进行休眠操作.

### 信号量
	0. semaphore sem      定义信号量变量
	1. sema_init(&sem, 1) 初始化信号量
	2. down(&sem);        对信号量进行P操作, 减1操作
	3. up(&sem);          对信息量进行V操作, 加1操作
	注意: 信号量可以进行休眠操作.

### 等待队列
	0. work_struct queue;  定义等待队列变量
	1. init_waitqueue_head(&queue); 初始化等待队列
	2. wait_event_interruptible(&queue, 条件); 进行等待队列,等待唤醒. 唤醒后检查条件是否满足.
	3. wake_up(&queue);    唤醒等待队列
	注意: 可以进行休眠操作.

## 中断
### tasklet
	0. tasklet_struct task;  定义tasklet变量
	1. tasklet_init(&task, func, arg);
		1. tasklet变量
		2. 中断处理函数
		3. 中断处理函数参数
	2. tasklet_schedule(&task); 将中断加入到中断下半部处理的tasklet处理列表中.
	3. void func(unsigned long arg); 中断处理函数
	注意: 不能进行休眠操作. 软件中断也不能进行休眠操作.

### 工作队列
	0. work_struct work; 定义工作队列变量
	1. INIT_WORK(&work, (void *)func); 初始化工作队列
		1. 工作队列变量
		2. 处理函数
	2. schedule_work(&work); 将中断加入到中断下半部处理的工作队列处理列表中.
	3. void func(struct work_struct work); 工作队列处理函数
	注意：可以进行休眠操作.

### 中断注册操作
	0. request_irq(irq_no, func, flag, name, NULL);
		1. 中断号
		2. 中断处理函数
		3. 中断标记位.IRQF_DISABLE: 当处理中断时,不能被其它中断打断.
		4. 中断名子.
		5. NULL.
	1. irqreturn_t func(int irq_no, void * devid);
		1. 返回值 IRQ_HANDLED  中断成功
		2. 返回值 IRQ_NONE     中断失败
		3. ?还有一个返回值, 需要自己补充上.
	2. free_irq(irq_no, NULL); 释放申请的中断号.

## platform 总线
	1. platform_device_register
	2. platform_drvice_register
	3. platform_match_device
	通用总线, 是虚构出来的.

### 注册driver和device
	1. 这块使用name进行匹配
	2. platform_get_ioresourec
		1. IORESOUREC_MEM
		2. IORESOUREC_IRQ

### 设备树
	1. .compatible进行匹配
	2. platform_get_ioresourec
		1. IORESOUREC_MEM
		2. IORESOUREC_IRQ

## I2C总线
	1. i2c_match_device()
	2. i2c_init()
	3. i2c_probe()
	4. i2c_open()
	5. i2c_read()
	6. i2c_write()
	7. i2c_relsease()
	8. i2c_ioctl()

### i2c-dev
	1. .of_match_table -> .compatible
	2. algos
		1. strcut i2c_msg
			1. .addr
			2. .flags
			3. .len
			4. .*buf
		2. i2c_transfer(adapter, msg, ARRAY_SIZE(smg))



# 附录:

## 读设备文件
```
$ sudo insmod hello.ko
$ cat /dev/hello
$ sudo rmmod hello
```

## 写设备文件
```
$ sudo insmod hello.ko
$ sudo chown linux:linux /dev/hello
$ echo "Welcome to kernel" > /dev/hello
$ sudo rmmod hello
```

## 查看kernel连接符号表
```
$ cat /proc/kallsyms
```

## 查看设备号
查看当前系统设备已使用的设备号, 包括字符设备和块设备.
```
$ cat /proc/devices
```
## 查看中断
第一列是中断号, 第二列是接收中断数目的计数器. 第三列是中断的中断控制器.第四列是中断相关的设备名子.
```
$ cat /proc/interrupts
```

## 管道操作

需要保证在同一个目录下.

### 窗口-1
1. 创建管道文件.
```
$ mkfifo ps
```
2. 用cat读ps
```
$ cat ps
```
### 窗口-2
1. 向ps写数据
```
$ cat > ps
```





### 查看内核log信息

```
$ dmesg
```

