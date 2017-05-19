# devstack-multinode
devstack安装多节点openstack环境

一个controller,两个compute

# 一些加速配置
1. 配置本地源（系统源和epel）或者使用阿里云的源
2. 跳过安装epel， devstack 每次运行的时候都会安装 epel (卸载已安装的)，因为已经配置了本地源，这一步跳过
```
# vim /opt/devstack/stack.sh

if is_fedora && [[ $DISTRO == "rhel7" ]] && \ 
        [[ ${SKIP_EPEL_INSTALL} != True ]]; then 
    _install_epel_and_rdo                                                                                                                                                                                                                     
fi
改为
if is_fedora && [[ $DISTRO == "rhel7" ]] && \ 
        [[ ${SKIP_EPEL_INSTALL} != True ]]; then 
    #_install_epel_and_rdo  
    echo "skip install epel"
fi
```
3. 修改 git_pip 的初始地址(bootstrap.pypa.io这个地址访问经常超时，建议把文件下载到本地做本地源)
```
# vim /opt/devstack/tools/install_pip.sh

PIP_GET_PIP_URL=${PIP_GET_PIP_URL:-"https://bootstrap.pypa.io/get-pip.py"}
改为
PIP_GET_PIP_URL=${PIP_GET_PIP_URL:-"http://192.168.3.99/get-pip.py"}
```
4. 使用豆瓣的 pypi 源
```
# mkdir -p /root/.pip/

# cat >/root/.pip/pip.conf <<EOF
[global]
index-url = https://pypi.doubanio.com/simple/
[install]
trusted-host = pypi.doubanio.com
[list]
format=columns
EOF
```