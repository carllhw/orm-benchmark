# ORM Benchmark

A benchmark to compare the performance of golang orm package.

## Results (2018-05-17)

### Environment

* DigitalOcean Intel Xeon E5-2650 v4 2.20GHz (4 vCPUs)
* 8GB Memory
* CentOS 7.5 64位
* go version go1.10.2 linux/amd64
* [docker-ce](https://docs.docker.com/install/linux/docker-ce/centos/) 18.03.1.ce
* [docker-compose](https://github.com/docker/compose) 1.21.2
* [Go MySQL Driver](https://github.com/go-sql-driver/mysql) Latest

### MySQL

* MySQL 5.7.22
* Config in my.cnf

### ORMs

All package run in no-cache mode.

* [Beego ORM](http://beego.me/docs/mvc/model/overview.md) latest in branch [develop](https://github.com/astaxie/beego/tree/develop)
* [xorm](https://github.com/go-xorm/xorm) latest
* [Hood](https://github.com/eaigner/hood) latest
* [Qbs](https://github.com/coocood/qbs) latest (Disabled stmt cache / [patch](https://gist.github.com/slene/8297019) / [full](https://gist.github.com/slene/8297565))
* [gorm](https://github.com/jinzhu/gorm) latest

### Run

get source code and install
```bash
go get -u github.com/carllhw/orm-benchmark
```

start mysql
```bash
cd $GOPATH/src/github.com/carllhw/orm-benchmark
docker-compose up -d
```

run benchmark tests
```bash
orm-benchmark -multi=20 -orm=all
```

### Reports

#### Sample 1

```
 40000 times - Insert
       raw:    31.21s       780288 ns/op     552 B/op     12 allocs/op
       qbs:    32.04s       800915 ns/op    5514 B/op    106 allocs/op
       orm:    42.63s      1065733 ns/op    1936 B/op     40 allocs/op
      xorm:    57.85s      1446261 ns/op    2537 B/op     68 allocs/op
      hood:    58.11s      1452662 ns/op   12477 B/op    299 allocs/op
      gorm:    91.64s      2291015 ns/op    7464 B/op    133 allocs/op

 10000 times - MultiInsert 100 row
       orm:    73.99s      7398529 ns/op  147194 B/op   1534 allocs/op
      xorm:    87.38s      8738152 ns/op  234073 B/op   4751 allocs/op
       raw:    92.13s      9213336 ns/op  110803 B/op    811 allocs/op
       qbs:     Not support multi insert
      gorm:     Not support multi insert
      hood:     Not support multi insert

 40000 times - Update
       qbs:    34.13s       853169 ns/op    5514 B/op    106 allocs/op
       raw:    49.43s      1235746 ns/op     616 B/op     14 allocs/op
      hood:    65.92s      1648112 ns/op   12477 B/op    299 allocs/op
      xorm:    76.06s      1901436 ns/op    2800 B/op     96 allocs/op
       orm:    83.97s      2099248 ns/op    1928 B/op     40 allocs/op
      gorm:   181.11s      4527793 ns/op   19274 B/op    362 allocs/op

 80000 times - Read
       raw:    42.30s       528764 ns/op     1528 B/op     40 allocs/op
       qbs:    50.40s       630015 ns/op     8073 B/op    180 allocs/op
      hood:    99.15s      1239336 ns/op     4482 B/op     93 allocs/op
       orm:   106.38s      1329774 ns/op     2898 B/op    100 allocs/op
      xorm:   125.64s      1570489 ns/op     9615 B/op    245 allocs/op
      gorm:   128.76s      1609464 ns/op    11594 B/op    210 allocs/op

 40000 times - MultiRead limit 100
       raw:    68.72s       1717997 ns/op   34810 B/op   1323 allocs/op
       orm:   148.58s       3714602 ns/op   85253 B/op   4290 allocs/op
       qbs:   158.13s       3953182 ns/op  205390 B/op   6432 allocs/op
      hood:   234.00s       5849922 ns/op  234253 B/op   9611 allocs/op
      gorm:   255.57s       6389258 ns/op  248030 B/op   6228 allocs/op
      xorm:   258.68s       6467108 ns/op  180613 B/op   8188 allocs/op
```

#### Sample 2

```
 40000 times - Insert
       raw:    22.38s       559379 ns/op     552 B/op     12 allocs/op
       qbs:    26.02s       650386 ns/op    5514 B/op    106 allocs/op
      xorm:    46.54s      1163399 ns/op    2537 B/op     68 allocs/op
      hood:    48.59s      1214657 ns/op   12477 B/op    299 allocs/op
       orm:    58.23s      1455697 ns/op    1936 B/op     40 allocs/op
      gorm:    78.92s      1973105 ns/op    7467 B/op    133 allocs/op

 10000 times - MultiInsert 100 row
       raw:    66.85s      6684637 ns/op  110803 B/op    811 allocs/op
       orm:    83.36s      8335880 ns/op  147209 B/op   1534 allocs/op
      xorm:    88.39s      8839102 ns/op  234062 B/op   4751 allocs/op
      gorm:     Not support multi insert
      hood:     Not support multi insert
       qbs:     Not support multi insert

 40000 times - Update
       qbs:    21.82s       545503 ns/op    5514 B/op    106 allocs/op
       raw:    38.95s       973678 ns/op     616 B/op     14 allocs/op
      hood:    47.97s      1199294 ns/op   12477 B/op    299 allocs/op
       orm:    68.57s      1714221 ns/op    1928 B/op     40 allocs/op
      xorm:    84.54s      2113392 ns/op    2801 B/op     96 allocs/op
      gorm:   162.07s      4051683 ns/op   19274 B/op    362 allocs/op

 80000 times - Read
       raw:    38.56s       482014 ns/op    1528 B/op     40 allocs/op
       qbs:    40.45s       505619 ns/op    8072 B/op    180 allocs/op
      hood:    69.99s       874829 ns/op    4482 B/op     93 allocs/op
       orm:    95.80s      1197537 ns/op    2898 B/op    100 allocs/op
      gorm:   112.53s      1406589 ns/op   11594 B/op    210 allocs/op
      xorm:   130.90s      1636220 ns/op    9616 B/op    245 allocs/op

 40000 times - MultiRead limit 100
       raw:    67.22s      1680387 ns/op   34812 B/op   1323 allocs/op
       qbs:   143.19s      3579852 ns/op  205398 B/op   6432 allocs/op
       orm:   161.26s      4031391 ns/op   85262 B/op   4290 allocs/op
      hood:   218.63s      5465857 ns/op  234256 B/op   9611 allocs/op
      gorm:   266.15s      6653779 ns/op  248030 B/op   6228 allocs/op
      xorm:   299.75s      7493741 ns/op  180618 B/op   8188 allocs/op
```

### Contact

Maintain by [carllhw](https://github.com/carllhw)