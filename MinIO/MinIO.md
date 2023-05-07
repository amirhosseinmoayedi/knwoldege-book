#minio #object-storage 

What is <font color="#ffff00">object</font> ?
- It's a <font color="#e36c09">BLOB</font> binary large object .
- It can be video, image and ... 
- They have **rich metadata**.

<font color="#366092">Bucket</font> is where we store <u><font color="#ffff00">objects</font> , policies and configurations</u>.
like a disk volume. **they are some sort of DNS.**

> Application interact with buckets to PUT and GET object, they use globally unique DNS ( Buckets ).

<font color="#31859b">Prefixe</font> is a file **path like folder**, it only exists if there are objects with that prefixes <u>so it deletion mean deleting all the object in that prefix</u>, no object can have multiple object but prefix can be combined.
 
What is<font color="#76923c"> Object Storage</font>?
- support buckests and **hirarchy**, and **prefixes**.
- object consist off **unique ID,** data, their full **meta data** 
- <u>access over HTTP</u>
- unlimted **scale** 
 
Minio uses <font color="#00b0f0">S3 objetcs</font>.
Most of serivice use S3 Object to store data lile Apcache spark and kafka<font color="#00b0f0">.</font>

Running Mino need two port<u> 9000 for  api</u> port and <u>9090 consol</u> port.
[[Docker]] running:
```bash
mkdir -p ~/minio/data

docker run \
   -p 9000:9000 \
   -p 9090:9090 \
   --name minio \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=ROOTNAME" \
   -e "MINIO_ROOT_PASSWORD=CHANGEME123" \
   quay.io/minio/minio server /data --console-address ":9090"
```

Minio looks for user and pass of **root** user from the **enviroment variables**.

The difference between sdk and console are that *admin operation* are only done via consloe but S3 operation can be done from both, and they both use [[REST API]].

### S3 Operations:
- GET/POST/HEAD/DELETE
- Retention (Lock/Legal Hold) Lifecycle Management
- Object Encryption
- Bucket Replication
- Bucket Versioning

Minio uses <font color="#e36c09">TLS</font> as defaut and in <u>dev it should be told not to</u>.

Simple example of using SDK:
```python
from minio import Minio
from minio.commonconfig import CopySource

# create client
client = MInio(
    "endpoint", #  "127.0.0.1:9000"
    "access_key", # "user1"
    "secret_key", # "testpass123"
    "session_key",
    "secure_bool", # False
    "region"
)
# get the list of buckets
bucckets = client.list_buckets() # get list of buckets
  
# Get info
result = client.stat_object("learning", "hello.txt") # get stats of an object

# copy to prefix
result = client.copy_object("learning", "new_prefix/hello.txt", CopySource("learning", "hello.txt")) # copy from root to the prefix

# list of all objects
results = client.list_objects("learning", "new_prefix", recursive=True) # source is the prefix

# delete object
result = client.remove_object("learning", "hello.txt") # copy from root to the prefix
```