### Jupyter with Fusion on Linux

Use the script `fusion-jupyter.sh` to connect fusion and jupyter _(fusion-jupyter-virtualenv.sh for virtualenv support)_
### Requirements:
0. Copy the file on the machine where Fusion is installed. Verify the python environment you want to use. You can configure the python environment by changing this line in the script: `export PYSPARK_PYTHON=python3`
1. Have python environment ready and managed (3.x preferred 2.x will also work)
2. Install Jupyter and all the packages you would like to use e.g. sklearn,tensorflow,keras etc.
3. Open a new terminal:
- 3.1 `export FUSION_HOME=/root/path/to/fusion/4.x`
- 3.2 `sh ./fusion-jupyter.sh` or `bash ./fusion-jupyter.sh`
4.  You should have jupyter running on port 8888
5. Navigate to port localhost:8888, if you have jupyter running create a notebook and test the following lines of code:  
  ```python
  df = spark.read.format("solr").load(format='solr',
                         collection="system_logs",
                         zkhost="localhost:9983/lwfusion/4.2.0-SNAPSHOT/solr",
                         flatten_multivalued='false',
                         request_handler='/select',
                         query="*:*"
                         )
  df.printSchema()
  df.show(3)
  ```
6. If you see the spark df schema and some documents you are good to go!

### Notes:
1. If the collection is empty you might get an error but that's ok
2. If you are on a remote machine you will need to tunnel in to access jupyter. Even if port 8888 is opened to public ip's but jupyter has a setting which blocks it. You can google the instructions to allow jupter to accept connections from public ip's. You will have to generate jupyter config and change *ip* and *allow_origin* properties.
3. Moving forward you may not have to specify the zkhost. 
4. For full range of query support check out the Spark-Solr github project (https://github.com/lucidworks/spark-solr)
