#nginx #web-server #architecture
**worker-master** proccess in nginx:
![worker-master-nginx](../../../_resources/Screenshot_from_2022-05-06_11-34-15.png)

**Master proccess** **manages** the<font color="#ffff00"> shutdown</font> and<font color="#ffff00"> startup</font> of the process, evaluating the configs and etc.

Its the job of the worker proccess to serve the **incoming requests** and keep the **conections**.

***Os*** will disterbiute the requests to the worker processes, The proccess algorithm is **not OS dependent**.

When the nginx is running you don't have to start the worker proccesses manually, they will be started automatically.


nginx configuration file:
> /etc/nginx/nginx.conf


in the event section you can modify the number of connection **per secound per worker process**.
`events { worker_connections 1024; }`

The number of worker processes is **depended** by the number of CPU cores(*it's good practice*)
`worker_processes auto;`

The HTTP block in the  `etc/nginx/nginx.conf` <font color="#e36c09">incluedes</font> the `/etc/nginx/conf.d/default.conf` which we add our <u>nginx config for serving the pages or cahce or proxy and etc.</u>