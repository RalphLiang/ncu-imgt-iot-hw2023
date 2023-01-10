## Raspberry Pi4 安裝pyspark心得
1. 首先一定要是安裝 64位元作業系統，Ex: raspbian 64 
2. 可以的話,最好使用SSD,買256G以內只需幾百元，搭配SATA轉 usb3.0 的線
3. 安裝docker
```
sudo apt-get install docker.io
```
4. 可另外拉image portainer 方便管理 
```
docker run -d -p 9000:9000 -v //var/run/docker.sock:/var/run/docker.sock --restart=always --name portainer -v portainer_data:/data portainer/portainer-ce
```
5. 拉pyspark image 來用
```
docker run -d -p 8888:8888 --name pyspark3.2.1 -v /home/ycliang/pyspark:/home/jovyan -e SPARK_OPTS="--packages io.delta:delta-core_2.12:2.0.0 --driver-java-options=-Xms1024M --driver-java-options=-Xmx6144M --driver-java-options=-Dlog4j.logLevel=info --ResourceUseDisplay.mem_limit" jupyter/pyspark-notebook:aarch64-spark-3.2.1
```
6. 在container裡裝預計會使用到的python套件
```
pip install msteams delta-spark  tableauserverclient dash jupyter-git delta-spark==2.0.0
pip install --upgrade jupyterlab jupyterlab-git
pip install psycopg2-binary
```
7. 下列指令可以產生可使用的delta lake jar file under 目錄下 (做 data lake 用)
```
pyspark --packages io.delta:delta-core_2.12:2.0.0 \
  --conf "spark.sql.extensions=io.delta.sql.DeltaSparkSessionExtension" \
  --conf "spark.sql.catalog.spark_catalog=org.apache.spark.sql.delta.catalog.DeltaCatalog"
  ```
8. 安裝 delta-spark 套件
```
pip install delta-spark==2.0.0
```
程式開發部分
## [票房查詢](https://github.com/RalphLiang/pyspark-testing-suite/blob/master/testShowMovieApi.ipynb)


