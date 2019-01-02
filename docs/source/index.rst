Spark & HDFS install note 
=========================
xx.xx.7.19:     (MASTER) HDFS Namenode, Spark Master
--------------------------------------------------------
xx.xx.7.20-21:  (WORKER) HDFS Datanode, Spark Worker
-----------------------------------------------------

Bước 1: Cài Java, Python, tải Spark, Hadoop:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1.1: Tạo thư mục chứa các bộ cài đặt Spark, Hadoop, Conda, thay đổi quyền trong thư mục cho user bigdata: ::

    $ mkdir -p /opt/bigdata
    $ sudo chown -R bigdata:bigdata /opt/bigdata

1.2: Tải Spark, Hadoop, Conda: ::

    /opt/bigdata$ wget "http://mirrors.viethosting.com/apache/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz"
    /opt/bigdata$ wget "http://mirrors.viethosting.com/apache/hadoop/common/hadoop-3.1.1/hadoop-3.1.1.tar.gz"
    /opt/bigdata$ wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh"

1.3: Cài Java: ::

    $ sudo add-apt-repository ppa:webupd8team/java
    $ sudo apt update
    $ sudo apt install oracle-java8-installer

1.4: Cài Conda, thuận tiện trong việc quản lý python và python package, chú ý chọn thư mục cài /opt/bigdata/conda: ::

    /opt/bigdata$ bash Miniconda3-latest-Linux-x86_64.sh

1.5: Cài Python (chỉ định phiên bản 3.6 - ổn định): ::

    $ conda install python=3.6


Bước 2: Giải nén, thiết lập cấu hình Spark, Hadoop:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2.0: Lưu ý về việc chọn spark bin hadoop hay spark with out hadoop, nếu chọn spark with out hadoop thì cần phải thêm cấu hình trong spark đường dẫn đến thư viện của hadoop: ::

    SPARK_DIST_CLASSPATH="$(${HADOOP_HOME}/bin/hadoop classpath)"

2.1: Giải nén Spark, Hadoop: ::

    /opt/bigdata$ tar -xzf spark-2.x.x.tgz
    /opt/bigdata$ tar -xzf hadoop-3.x.x.tar.gz

2.2: Cấu hình Spark: ::

    $ cd $SPARK_HOME/conf
    
        * spark-env.sh          : Cấu hình các biến môi trường: JAVA_HOME, PYSPARK_DRIVER_PYTHON
        * spark-defaults.conf   : Cấu hình resouce (số lượng core, ram ...) : spark.driver.cores, spark.driver.memory, spark.executor.cores, spark.executor.memory, spark.deploy.defaultCores
        * slaves                : Cấu hình hostname (ip các node worker)

2.3: Cấu hình Hadoop: ::

    $ cd $HADOOP_HOME/etc/hadoop

        * hadoop-env.sh         : Cấu hình các biến môi trường: JAVA_HOME
        * core-site.xml         : 
        * hdfs-site.xml         : Thiết lập đường dẫn trỏ đến nơi lưu trữ dữ liệu (nanmenode: schema, datanode: data)
        * workers               : Cấu hình hostname (ip các datanode worker)

Bước 3: Thiết lập hostname, add ssh key giữa các node master <---> worker, khới động Spark, HDFS:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3.1: Cập nhật hostname: ::
    
    $ sudo nano /etc/hosts
    xx.xx.7.18      jupyterhub
    xx.xx.7.19      bigdata.master
    xx.xx.7.20      bigdata.worker1
    xx.xx.7.21      bigdata.worker2

3.2: Tạo key ssh, add key chéo giữa các node master <---> worker  (lưu ý cần add public key của node master vào authorized_keys của chính nó): ::
    
    Bước này để phục vụ script start tất cả các node từ node master
    $ SPARK_HOME/sbin/start-all.sh
    $ HADOOP_HOME/sbin/start-dfs.sh

3.3: Chạy các script trên master: ::

    $ SPARK_HOME/sbin/start-all.sh
    $ HADOOP_HOME/sbin/start-dfs.sh

3.4: Kiểm tra ứng tính khả dụng:
    * Kiểm tra Web UI của Spark và Hadoop
    * Kiểm tra các port (defaut) 7077, 8020 của spark và hadoop đã binding trên ip public chưa (nếu binding trên IP 127.0.0.1 --> Các ứng dụng bên ngoài không gọi vào được)


.. toctree::
   :maxdepth: 2
   
   



