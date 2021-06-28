# hw04
1. zfs list выдал, что отсутсвует zfs, установил используя предустановленный скрипт через команду vagrant provision
2. zfs list выдал сообщение о пустом списке. сначала создал зеркало из шести дисков zpool create poolmirror sd{b..g},
 удалил zpool destroy poolmirror.
3. Создал стрип zpool create stripe sd{b..g}, добавил в него диски  с разными видами сжатия: gzip, gzip-N (9), on, lzjb, zle, lz4.
4. Скачал файл с войной и миром, распаковал его gunzip War_and_Peace.txt.utf8.gz.
5. Скопировал через 'echo папки в которые выкладываем | xargs -n 1 cp файл который копируем', в семь созданных фс
6. zfs list показал таблицу где видно, что наилучшее сжатие даёт gzip (gzip9).

7. Загрузил архив через vagrant upload C:\VM\zfs_task1.tar.gz загруженный предварительно через десктоп,
8. Удалил предудыщий стрип ZFS zpool destroy stripe.
9. Распаковал скаченный архив tar -xvf zfs_task1.tar.gz
zpoolexport/
zpoolexport/filea
zpoolexport/fileb
10. Импортировал пул zpool import -d ./zpoolexport/[root@localhost tmp]# zpool import -d ./zpoolexport/
   pool: otus
     id: 6554193320433390805
  state: ONLINE
 action: The pool can be imported using its name or numeric identifier.
 config:

        otus                        ONLINE
          mirror-0                  ONLINE
            /tmp/zpoolexport/filea  ONLINE
            /tmp/zpoolexport/fileb  ONLINE
11. Подставляем имя пула и проверяем вывод 
[root@localhost tmp]# zpool import -d ./zpoolexport/ otus
[root@localhost tmp]# zfs list
NAME             USED  AVAIL     REFER  MOUNTPOINT
otus            2.04M   350M       24K  /otus
otus/hometask2  1.88M   350M     1.88M  /otus/hometask2
12. Вывод команд. Размер хранилища
[root@localhost tmp]# zpool get size otus
NAME  PROPERTY  VALUE  SOURCE
otus  size      480M   -
                 Тип pool
zfs get type otus
NAME  PROPERTY  VALUE       SOURCE
otus  type      filesystem  -
                Значение recordsize
[root@localhost tmp]# zfs get recordsize otus
NAME  PROPERTY    VALUE    SOURCE
otus  recordsize  128K     local
                Используется сжатие
[root@localhost tmp]# zfs get compression otus
NAME  PROPERTY     VALUE     SOURCE
otus  compression  zle       local
                Какая контрольная сумма используется
 zfs get checksum otus
NAME  PROPERTY  VALUE      SOURCE
otus  checksum  sha256     local
              общий вывод по ключам
[root@localhost tmp]# zfs get type,recordsize,compression,checksum otus
NAME  PROPERTY     VALUE       SOURCE
otus  type         filesystem  -
otus  recordsize   128K        local
otus  compression  zle         local
otus  checksum     sha256      local
	        Общий вывод 
[root@localhost tmp]# zfs get all otus
NAME  PROPERTY              VALUE                  SOURCE
otus  type                  filesystem             -
otus  creation              Fri May 15  4:00 2020  -
otus  used                  2.04M                  -
otus  available             350M                   -
otus  referenced            24K                    -
otus  compressratio         1.00x                  -
otus  mounted               yes                    -
otus  quota                 none                   default
otus  reservation           none                   default
otus  recordsize            128K                   local
otus  mountpoint            /otus                  default
otus  sharenfs              off                    default
otus  checksum              sha256                 local
otus  compression           zle                    local
otus  atime                 on                     default
otus  devices               on                     default
otus  exec                  on                     default
otus  setuid                on                     default
otus  readonly              off                    default
otus  zoned                 off                    default
otus  snapdir               hidden                 default
otus  aclinherit            restricted             default
otus  createtxg             1                      -
otus  canmount              on                     default
otus  xattr                 on                     default
otus  copies                1                      default
otus  version               5                      -
otus  utf8only              off                    -
otus  normalization         none                   -
otus  casesensitivity       sensitive              -
otus  vscan                 off                    default
otus  nbmand                off                    default
otus  sharesmb              off                    default
otus  refquota              none                   default
otus  refreservation        none                   default
otus  guid                  14592242904030363272   -
otus  primarycache          all                    default
otus  secondarycache        all                    default
otus  usedbysnapshots       0B                     -
otus  usedbydataset         24K                    -
otus  usedbychildren        2.01M                  -
otus  usedbyrefreservation  0B                     -
otus  logbias               latency                default
otus  objsetid              54                     -
otus  dedup                 off                    default
otus  mlslabel              none                   default
otus  sync                  standard               default
otus  dnodesize             legacy                 default
otus  refcompressratio      1.00x                  -
otus  written               24K                    -
otus  logicalused           1020K                  -
otus  logicalreferenced     12K                    -
otus  volmode               default                default
otus  filesystem_limit      none                   default
otus  snapshot_limit        none                   default
otus  filesystem_count      none                   default
otus  snapshot_count        none                   default
otus  snapdev               hidden                 default
otus  acltype               off                    default
otus  context               none                   default
otus  fscontext             none                   default
otus  defcontext            none                   default
otus  rootcontext           none                   default
otus  relatime              off                    default
otus  redundant_metadata    all                    default
otus  overlay               off                    default
otus  encryption            off                    default
otus  keylocation           none                   default
otus  keyformat             none                   default
otus  pbkdf2iters           0                      default
otus  special_small_blocks  0                      default
		Общий вывод по пулу
[root@localhost tmp]# zpool get all otus
NAME  PROPERTY                       VALUE                          SOURCE
otus  size                           480M                           -
otus  capacity                       0%                             -
otus  altroot                        -                              default
otus  health                         ONLINE                         -
otus  guid                           6554193320433390805            -
otus  version                        -                              default
otus  bootfs                         -                              default
otus  delegation                     on                             default
otus  autoreplace                    off                            default
otus  cachefile                      -                              default
otus  failmode                       wait                           default
otus  listsnapshots                  off                            default
otus  autoexpand                     off                            default
otus  dedupditto                     0                              default
otus  dedupratio                     1.00x                          -
otus  free                           478M                           -
otus  allocated                      2.09M                          -
otus  readonly                       off                            -
otus  ashift                         0                              default
otus  comment                        -                              default
otus  expandsize                     -                              -
otus  freeing                        0                              -
otus  fragmentation                  0%                             -
otus  leaked                         0                              -
otus  multihost                      off                            default
otus  checkpoint                     -                              -
otus  load_guid                      5102501821984428654            -
otus  autotrim                       off                            default
otus  feature@async_destroy          enabled                        local
otus  feature@empty_bpobj            active                         local
otus  feature@lz4_compress           active                         local
otus  feature@multi_vdev_crash_dump  enabled                        local
otus  feature@spacemap_histogram     active                         local
otus  feature@enabled_txg            active                         local
otus  feature@hole_birth             active                         local
otus  feature@extensible_dataset     active                         local
otus  feature@embedded_data          active                         local
otus  feature@bookmarks              enabled                        local
otus  feature@filesystem_limits      enabled                        local
otus  feature@large_blocks           enabled                        local
otus  feature@large_dnode            enabled                        local
otus  feature@sha512                 enabled                        local
otus  feature@skein                  enabled                        local
otus  feature@edonr                  enabled                        local
otus  feature@userobj_accounting     active                         local
otus  feature@encryption             enabled                        local
otus  feature@project_quota          active                         local
otus  feature@device_removal         enabled                        local
otus  feature@obsolete_counts        enabled                        local
otus  feature@zpool_checkpoint       enabled                        local
otus  feature@spacemap_v2            active                         local
otus  feature@allocation_classes     enabled                        local
otus  feature@resilver_defer         enabled                        local
otus  feature@bookmark_v2            enabled                        local

13. Восстанавливаю снапшот из скачанного файла
[root@localhost tmp]# zfs receive otus/snap < otus_task2.file
[root@localhost tmp]# zfs list
NAME             USED  AVAIL     REFER  MOUNTPOINT
otus            4.93M   347M       25K  /otus
otus/hometask2  1.88M   347M     1.88M  /otus/hometask2
otus/snap       2.83M   347M     2.83M  /otus/snap
14. Ищу secret_message ll otus/snap и т.д.

[root@localhost tmp]# vi /otus/snap/task1/file_mess/secret_message

15. https://github.com/sindresorhus/awesome

