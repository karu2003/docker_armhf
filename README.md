# chroot
image for chrooot

sudo dd bs=4M if=/dev/sda of=bpim2z.img status=progress

# docker install

do not install from Ubuntu’s standard repository

https://docs.docker.com/engine/install/ubuntu/

docker buildx create --name pArmhf --bootstrap --use
docker buildx inspect
docker buildx build --platform linux/arm/v7

# ~/.bashrc

docker-run() {
    docker run --rm -it \
        -v $PWD:/work \
        -v /etc/localtime:/etc/localtime:ro \
        -v $HOME/.ssh:/root/.ssh \
        -v $HOME/.git:/root/.git \
        -v $HOME/.bash_history:/root/.bash_history \
        -v $HOME/.screenrc:/root/.screenrc \
        -v $HOME/.tmux.conf:/root/.tmux.conf \
        -v /tmp/.X11-unix:/tmp/.X11-unix \
        -e DISPLAY=$DISPLAY \
        -v $XAUTHORITY:$XAUTHORITY \
        -e XAUTHORITY \
        -h $HOSTNAME \
        -v $HOME/.Xauthority:/root/.Xauthority \
        -v /dev/dri/renderD128:/dev/dri/renderD128 \
        -v /dev/dri/card0:/dev/dri/card0 \
        -w /work \
        --security-opt seccomp:unconfined $@
}
#--user 1000:1000 \

docker-exec() {
    docker exec -it -w /work $@
}

# docker_armhf

docker-run --platform linux/arm/v7 --name openhd debian:bullseye-slim

docker context ls

docker buildx ls

docker buildx inspect

apt update

apt install sudo

apt install git

git config --global --add safe.directory /work

git config --global --add safe.directory /work/OpenHD/lib/mavlink-headers

git config --global --add safe.directory /work/OpenHD/lib/wifibroadcast

git config --global --add safe.directory /work/OpenHD/ohd_common/lib/json

git config --global --add safe.directory /work/OpenHD/ohd_common/lib/spdlog

git submodule init

git submodule update

git submodule update --init --recursive

git submodule foreach 'git fetch origin; git checkout $(git rev-parse --abbrev-ref HEAD); git reset --hard origin/$(git rev-parse --abbrev-ref HEAD); git submodule update --recursive; git clean -dfx'

./package.sh armhf raspbian && su andrew -Pc "scp openhd pi@192.168.3.97:/home/pi"

ssh -oHostKeyAlgorithms=+ssh-dss pi@192.168.3.97
scp -oHostKeyAlgorithms=+ssh-dss openhd pi@192.168.3.97:/home/pi/.

file openhd
