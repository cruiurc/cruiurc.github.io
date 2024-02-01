docker 中的jellyfin使用硬件加速
===============================

通过硬件加速
若要使用硬件加速，需要允许容器访问渲染设备。如果您使用的是 container-selinux-2.226 或更高版本，则必须在 selinux 中设置标志，否则容器将无法使用它：container_use_dri_device

sudo setsebool -P container_use_dri_device 1

在旧版本的 container-selinux 上，您必须通过向 podman 命令添加命令来禁用容器的 selinux 限制。--security-opt label=disable

然后，您需要将渲染设备挂载到容器内：

--device /dev/dri/:/dev/dri/

最后，您需要为容器设置标志以使用渲染设备：--device

--device /dev/dri/

Podman 运行
   podman run \
    --detach \
    --label "io.containers.autoupdate=registry" \
    --name myjellyfin \
    --publish 8096:8096/tcp \
    --device /dev/dri/:/dev/dri/ \
    # --security-opt label=disable # Only needed for older versions of container-selinux < 2.226
    --rm \
    --user $(id -u):$(id -g) \
    --userns keep-id \
    --volume jellyfin-cache:/cache:Z \
    --volume jellyfin-config:/config:Z \
    --mount type=bind,source=/path/to/media,destination=/media,ro=true,relabel=private \
    docker.io/jellyfin/jellyfin:latest

systemd的
[Unit]
Description=jellyfin

[Container]
Image=docker.io/jellyfin/jellyfin:latest
AutoUpdate=registry
PublishPort=8096:8096/tcp
UserNS=keep-id
#SecurityLabelDisable=true # Only needed for older versions of container-selinux < 2.226
AddDevice=/dev/dri/:/dev/dri/
Volume=jellyfin-config:/config:Z
Volume=jellyfin-cache:/cache:Z
Volume=jellyfin-media:/media:Z

[Service]
# Inform systemd of additional exit status
SuccessExitStatus=0 143

[Install]
# Start by default on boot
WantedBy=default.target
