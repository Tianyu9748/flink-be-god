目前Flink 官方提供的sql-client只能一条一条SQL执行，不能满足实际的生产应用。
期望是可以执行一个xxx.sql文件，然后通过flink的解析优化之后将任务提交到Yarn或K8s。
本项目是搬运的Jark大佬的[SQL-Submit](https://github.com/wuchong/flink-sql-submit),我们也是基于这个项目做的改造。
附上一条最终提交任务的命令：
> flink run -d -m yarn-cluster -p 5 -yjm 2048 -ytm 2560 -ys 5 -yqu queue -ynm xxx -yD env.java.opts="-server -XX:+UseConcMarkSweepGC -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=75 -XX:ParallelGCThreads=4 -XX:+AlwaysPreTouch -XX:NewRatio=1 -DjobName=jobName" -c xxxx.SqlSubmit sql-submit.jar -job_name job -sql H4sIAAAAAAAAAJVU23LbNhB9rr4Cb7RmRFiXNm3sMjOMosac0SUj05PMZDIMRC4lVCBAA6At9+u7BE1dHNdO+SCS2LO7h3uO1oAlGoxl2vrGamZh/RDkfAeZn4FgD5cd8wyCHiEosxaK0ppg0O//BNz9IpaYyw4hDg87SCvLlaTpBtJtqbi0XK4p7CxoyQT/B7MPIV+DBVnjg+UkDqN5spgn43A+nkynYRwt5pcvVsUb6DsmghEpuHwZW6gMgjBOppPwOsY248kreLbzUyXTSmtkeMQZp/NKJpd+ySoDwRs3mpewlhegKhsMhg7aSTXgnIllKwE4/FJpm5hNYTaqTIyqdApnOGpChFondS6qU5TkffQxmsc9F+EluY6X0fxj85pzEBmZhZ/+fDw9emxu7951up+j+Kop7JValaAtB0NXStla+JIaHDNo4wVexn0upfK3LN+y/oBuKFsN6K21hlugEuzF2/7bodf7odhaq6qkPMMaueBy65tb4afbFmlSJqnzGqJqrRAncBTG+irPcYQt0KqSpxg8HU4bRcUkpFZpRDiKbSBXumAWT5GSVasqP6Lo3ultZVEJzpSPYzVsDX79ySgZJu1PBj5Y8UMqX0ulAUXXBnzQWrlR5UwY8DpdlHW8nITxhMTh++mEpIKn241CgyQGB0GeUzSOZpPrOJx9Oht1m247uEMfJjwj7rq2Gv3ThvDfJbOEy1w9jSHHFMm70s9UzSw5XB+QY3P8Henp7/tA469Ol9Q+Qb6/nMz572yVtiOptPBIQNzZxeFDL87PB78P6eAN/XVEhxd/DIajc+cCr0ceZa0d70tWgMt/Yn0J922HTHPUpQFV9IHJDHb00ImO68er+vFDg2yZmXoBPZbPIGeVsPvuJTPmXunMBfeWRH4U5c1B+7mozMbtBK3ujYP91u/3/xvariYHHZoToCsDKBI0lUZ7QzHNhADBTdHkNe7hErlbgiXVU/PgchGoA4kXyUHcv5aLWXIzj77UR2cnxjrHzd7vdntuLXz19qbyvn31kG0F3rdeZx882OokPl2Mw+m+Xa9uXlvn9b497wEvfzbzs8zr9q4WN8uz/8m828m1Kkjn2d1I7jeggbT8M7jjKSQrjSY5+oDAu7oJP08i7/JfeDgWPzMHAAA=