# 背景

/root的.bashrc追加了一行 docker exec docker exec -it 09d228aee2f2 /bin/bash
不论是通过ssh还是物理终端root登陆都会直接进度docker内部, 执行登出会直接结束掉会话

exec的作用是将exec后启动的进程替换到当前进程空间

# 目标

恢复root登陆, 且不重启主机
其他信息: 已知root用户密码

# 思路

1. 关掉docker后台进程, 避免exec 进入容器
2. 获取root身份(不能加载.bashrc)


# 试错

1. su 到root, 然后修改.bashrc
    失败, su 到 root 依然读取了.bashrc 进入了容器内部
2. sudo vim /root/.bashrc
    失败, 机器默认禁用了sudo, 需要root权限才能修改
3. 使用ansible进行修改
    失败, ansible建立连接过程会先执行.bashrc

# 解法
su root -c '/bin/bash --norc'
