 第一步先去 /Users/loongtao/kernel 对应的 GitHub 仓库，跑这个 Action：

  compile-rockchip-rk3588-kernel

  建议参数这样填：

  - kernel_source: armbian/linux-rockchip@rk-6.1-rkr5.1
  - kernel_version: 6.1.y
  - kernel_auto: true
  - kernel_package: all
  - kernel_toolchain: gcc-15.2
  - ccache_clear: false
  - docker_image: trixie
  - kernel_sign: -rk3588-ophub
  - kernel_config: kernel-config/release/rk3588



 第二个 action 选：

  Build OpenWrt using Unifreq scripts

  这是你这个仓库里真正把 OpenWrt rootfs 和外部内核拼起来的那条。

  建议参数这样填：

  - openwrt_soc: h88k
  - customize_rk3399: 保持默认
  - openwrt_ip: 192.168.1.1
  - kernel_repo: 填你放了刚才内核改动的 GitHub 仓库
    例子：如果你的 /Users/loongtao/kernel 对应远端是 cathaysia/kernel，这里就填 cathaysia/kernel
  - openwrt_kernel: 6.1.y
  - auto_kernel: true
  - script_diy_path: 留空
  - openwrt_storage: save
  - builder_name: ophub 或你自己的名字

  最关键的是这两个：

  - openwrt_soc = h88k
  - kernel_repo = 你的 kernel 仓库
  - openwrt_kernel = 6.1.y

  这样它才会去你刚改过的 kernel 仓库里拿 rk3588/6.1.y 内核产物，并且生成 H88K 镜像。

  你的两步顺序应该是：

  1. 在 kernel 仓库跑 compile-rockchip-rk3588-kernel
  2. 等内核 release 产物出来后，在 amlogic-s9xxx-openwrt 仓库跑 Build OpenWrt using Unifreq scripts

  如果第二步你还是填 ophub/kernel，那就会拿官方现成内核，不会包含你刚加的 LCD 支持。
