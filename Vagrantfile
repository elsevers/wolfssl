# -*- mode: ruby -*-
# vi: set ft=ruby :

$setup = <<SCRIPT


apt-get update
apt-get install -y git autoconf libtool make valgrind libpq-dev


URL=https://sourceforge.net/projects/levent/files/libevent
LIB=libevent-2.0
VER=22-stable

wget -q $URL/$LIB/$LIB.$VER.tar.gz && tar -zxf $LIB.$VER.tar.gz
cd $LIB.$VER/ && ./autogen.sh && ./configure -q && make -s
sudo make install && cd .. && rm -rf $LIB.$VER*


SRC=vagrant
DST=wolfssl

cp -rp /$SRC/ $DST/

echo "cd $DST"                                         >> .bashrc
echo "read -p 'Sync $DST? (y/n) ' -n 1 -r"             >> .bashrc
echo "echo # new line"                                 >> .bashrc
echo 'if [[ \$REPLY =~ ^[Yy]$ ]]; then'                >> .bashrc
echo "    echo -e '\e[0;32mRunning $DST sync\e[0m'"    >> .bashrc
echo "    ./pull_to_vagrant.sh"                        >> .bashrc
echo "fi"                                              >> .bashrc

cd $DST
./autogen.sh
./configure
make check

cd ..
chown -hR vagrant:vagrant $DST/ /tmp/output

SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.provision "shell", inline: $setup
end
