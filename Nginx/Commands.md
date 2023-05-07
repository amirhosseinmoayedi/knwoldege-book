#nginx #commands 
| <font color="#ffc000">Command</font> | <font color="#00b050">Action</font> |
| --- | --- |
| nginx -h | see list of commands |
| nginx -v or -V | see the version | 
| nginx start | start the web server|
| service nginx stop | stop the web server|
| nginx -t | check the config file if it is ok|
| nginx -s | send the signal to process|

```bash
# will send stop signal
nginx -s stop 
# reload the configs
nginx -s reload 
```

