Spark & HDFS install note 
=========================
xx.xx.7.19: HDFS Namenode, Spark Master
=======================================
xx.xx.7.20-21: HDFS Datanode, Spark Worker
==========================================

Bước 1: Tạo thư mục chứa các bộ cài đặt Spark, Hadoop, Conda, thay đổi quyền trong thư mục cho user bigdata: ::

    $ mkdir -p /opt/bigdata
    $ sudo chown -R bigdata:bigdata /opt/bigdata

Bước 2: Tải Spark, Hadoop, Conda: ::

    /opt/bigdata$ wget "http://mirrors.viethosting.com/apache/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz"
    /opt/bigdata$ wget "http://mirrors.viethosting.com/apache/hadoop/common/hadoop-3.1.1/hadoop-3.1.1.tar.gz"
    /opt/bigdata$ wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh"

Bước 3: Cài Java: ::

    $ sudo add-apt-repository ppa:webupd8team/java
    $ sudo apt update
    $ sudo apt install oracle-java8-installer

Bước 4: Cài Conda, chú ý chọn thư mục cài /opt/bigdata/conda: ::

    /opt/bigdata$ bash Miniconda3-latest-Linux-x86_64.sh

Bước 5: Cài Python (chỉ định phiên bản 3.6 - ổn định): ::

    $conda install python=3.6

.. toctree::
   :maxdepth: 2
   
   



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
