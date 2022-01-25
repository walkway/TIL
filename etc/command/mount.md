# mount

- 장치를 특정 파일/폴더에 할당
- 예를 들어 CD-ROM을 마운트 한다는 것은, 특정 폴더에 들어가면 해당 CD-ROM의 내용을 볼 수 있도록 한다는 것.
````
mount [option] [device] [directory]
````

````
option

-a : /etc/fstab 에 있는 파일 시스템을 모두 마운트 한다.
-t : 파일시스템을 지정한다. -t ext2 와 같이 파일시스템 이름과 같이 쓰인다.
-o : 다른 옵션들을 명시할 수 있다.
````
````
device

FDD : /dev/fd0
CD/DVD : /dev/cdrom, /dev/dvd
IDE HDD : dev/hda
USB, SCSI HDD, SATA HDD : /dev/sda, /sdb
````