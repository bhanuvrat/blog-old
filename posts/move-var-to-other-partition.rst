.. link: 
.. description: 
.. tags: Linux,
.. date: 2014/01/17 07:32:53
.. title: Troubles while moving /var to other partition
.. slug: troubles-while-moving-var-to-other-partition


I am currently running ubuntu 13.10 on my office wala laptop. Since last couple of days the root partition was proving unsufficient. A simple df showed that ``/var`` was the partition consuming most of the space. Little more tinkering revealed that the databases I had on my laptop for development environments were the primary source of trouble. Adding to that were the various snapshots stored by ``docker``. Obviously, the crudest solution, would have been to repartition while reinstalling the os. However, the task of configuring everything again was daunting, and laziness got the best of me. Yesterday, out of the blue, it dawned on me that I had ~30G of unpartitioned space for *future me*'s desire of trying out newer distros. Thence, came the idea of mounting ``/var`` on another partition.

``cfdisk`` and ``mkfs.ext4`` created and formatted the new partition /dev/sda8, but beyond that I was clueless. So naturally, I asked google for the steps. And the results it came up with were:

1. mounting the new partition elsewhere, (say /var2) ::
   
     	mkdir /var2; mount -vt auto /dev/sda8 /var2

2. copying everything from the present /var to /var2, using ``rsync -rvtlD`` for coping the files properly ::

  		rsync -rvtlD /var/* /var2/

3. editing /etc/fstab to mount the new /dev/sda8 as /var automatically upon everyboot, by adding the following line ::

		uuid="long-hexadecimal-string-printed-by-blkid" /var ext4    errors=remount-ro 0       2

4. rebooting the computer ::

		sudo reboot

I followed the guide, blindly, however, it didn't quite work. Ubuntu failed to load the graphics settings. I could see that the new partition was indeed mounted on /var via ::

		mount | grep sda8

executed on tty1 (ctrl+alt+f1). For a second there, I thought that maybe I would have to reinstall the os, after all. I restored the backup of /etc/fstab I had taken earlier and rebooted. My mental peace returned when the system booted properly. I had a problem and I was stuck, so I went to uncle google, again. However, no one had reported having lost their display settings because of mounting /var on other partition. I left it and went to bed. Today, I thought of checking the output of ls -l for /var and /var2. The premissions were identical however the user and groups were not. A bit of googling followed. And I came across some post that suggested using ``rsync -avrtlgoD``. It worked.

References 

[1] http://askubuntu.com/questions/39536/how-can-i-store-var-on-a-separate-partition/

[2] http://www.digizenstudio.com/blog/2007/06/14/watch-out-when-you-move-var-in-ubuntu/

[3] http://explainshell.com/explain?cmd=rsync+-rvtldgoD


