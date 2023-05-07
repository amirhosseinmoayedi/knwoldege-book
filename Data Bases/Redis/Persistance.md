#redis #nosql 
| mode | description |
| --- | --- |
| **RDB** | The RDB persistence performs point-in-time snapshots of your dataset at specified intervals. |
| **AOF** | The AOF persistence logs every write operation received by the server, that will be played again at server startup, reconstructing the original dataset.<br><br>Commands are logged using the same format as the Redis protocol itself, in an append-only fashion |
| **No persistence** | If you wish, you can disable persistence completely |
| **RDB + AOF** | It is possible to combine both AOF and RDB in the same instance. Notice that, in this case, <u>when Redis restarts the AOF file will be used</u> to reconstruct the original dataset since it is guaranteed to be the most complete. |

## <font color="#b2a2c7">RDB</font>:
- good for **schedule backups**
- good for disaster recovery
- maximizes Redis performances since the only work the Redis parent process needs to do in order to persist is forking a child that will do all the rest
- *faster restarts with big datasets compared to AOF*

<u>AOF gives better full recovery than RDB</u>, because it **save data periodically**.

> creating a child process can have a overhead on cup, and depend on size of data set it is time consuming.

## <font color="#76923c">AOF</font>:
- **more durable**
- is append only so **no corruption and no overwrite**
- make a log file of all commands it's easy to read and parse 

AOF file are bigger than `rdb` files and is slower than RDB.

Redis dumps data in file called `dump.rdb` in if RDB is enabled.

## <font color="#fac08f">snapshot</font>:
redis save files in `n` minute if `m` changes are made.
save every 60 second if 1000 changes are made to dataset:
```bash
SAVE 60 1000
```

Snapshotting is <u>not very durable</u>, if data is changed in that 1 minute that you specified to take snapshot and redis instance crashes data in that 1 minute is lost.

```bash
CONFIG set appendonly yes
```
So Redis supports an interesting feature: it is able to rebuild the AOF in the background without interrupting service to clients.Redis will write the shortest sequence of commands needed to rebuild the current dataset in memory.

## AOF options
| command | description |
| --- | --- |
| appendfsync always | `fsync` every time new commands are appended to the AOF. Very very slow, very safe.<br><br>Note that the commands are apended to the AOF after a batch of commands from multiple clients or a pipeline are executed |
| appendfsync everysec | `fsync` every second, fast enough Default|
| appendfsync no | Never `fsync`, just put your data in the hands of the Operating System. The faster and less safe method. Normally Linux will flush data every 30 seconds with this configuration, but it's up to the kernel exact tuning. |

> for backing up redis data you can create a cronjob that copy the rdb file created by redis.