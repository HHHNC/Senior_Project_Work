# 关于sdk的使用方法：

## 1、使用git clone 将sdk 克隆到客户端

可以`mkdir workpace`

`cd workpace`

(先要安装好git，ubuntu平台下是 sudo apt install git)

`git clone https://github.com/HHHNC/Senior_Project_Work.git`

## 2、quickly start：

1. 先运行：`source install.sh`

- 这个会更改项目的权限、安装相关库、修复 toolchain

2. 运行`source env.sh`：

- 这个会配置环境变量

## build

- makefile，使用`make all`可以编译 bootcode、runner，添加如`make all APPNAME=vendor`可以编译 APP，并且无需修改`RISCV_RCP_PATH`
- 直接`make all`后默认是`APPNAME=vendor`，现在只会编译 app，如果你想编译 pulp-builder，就输入`make pulp_build`
- 用`make run APPNAME=<比如vendor>`可以直接生成 flash 比特流
-  vendor 目录，所有的代码都可以在这个目录下写，入口函数应该就是 main.c 里的 main 函数，我们可以在 main.c 里调用 src 目录的函数，添加功能就行了，比如我就添加了一个 gpio
- 在 sdk外部 执行 make gbd_flash 就可以使用gbd进行烧录，此时程序可以进行断点调试，不过暂时是在sram中运行。
- 如果不需要调试 可以直接执行 `make load_sram` 就可以运行了

### 3、常见问题

## pulp_build issue：

- 如果你的环境配置不同，可能无法正确编译 pulp，这时：
  - 进入这些目录：`sdk/pulp-builder/json-tools`、`sdk/pulp-builder/pulp-configs`
  - 分别执行`cmake CMakeLists.txt`，这样就可以生成对应的 makefile，之后再进行`make pulp_build`就行了
- 如果发现有某个 config 找不到，比如:`Didn't find JSON file from any specified path ......`可以修改`set_env.sh`