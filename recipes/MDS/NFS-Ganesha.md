# Quickinstall �� ������������� NFS-Ganesha

## ��������� NFS Ganesha, ������� ��������� �������������� CephFS
## ��� ������������, �������� ����� libcephfs. � ������� ������� 
## ������ ����� �������� ���������������� �����, ����� ��������� ��� 
## �� ������ ����� � ���������� ������������� NFS ���� ����� �localhost�


## ��������� ceph �� ������ �����

	yum -y install https://download.ceph.com/rpm-luminous/el7/noarch/ceph-release-1-1.el7.noarch.rpm
	yum -y install ceph

## ��������� nfs-ganesha

	yum -y install nfs-ganesha nfs-ganesha-ceph nfs-ganesha-utils

## ������ ���������������� ����
## ( ������� ./cfg/ganesha.conf )

## ���������� � ��������� ������

	systemctl enable nfs-ganesha.service
	systemctl start nfs-ganesha.service

## ������������ � �������� �����������������

	mkdir -p /mnt/ganesha
	mount.nfs localhost:/ceph /mnt/ganesha
	df -h /mnt/ganesha

## ��� ������ � oVirt ������������� �����

	chown -R 36.36 /mnt/ganesha

## ������������, ���� ���������

	umount /mnt/ganesha