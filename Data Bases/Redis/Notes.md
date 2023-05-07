#redis #notes #nosql 

commands are **Atomic**.

# Optimistic concurrency control
**Optimistic concurrency control** (**OCC**), also known as **optimistic locking**, is a concurrency control method applied to transactional systems such as relational database management systems and software transactional memory. OCC assumes that multiple transactions can frequently complete without interfering with each other. While running, transactions use data resources without acquiring locks on those resources. Before committing, each transaction verifies that no other transaction has modified the data it has read. If the check reveals conflicting modifications, the committing transaction rolls back and can be restarted.

<font color="#ff0000">Redis</font> <u>does not requires root privileges to run</u>. It is recommended to run it as an unprivileged *redis* user that is only used for this purpose.  

Redis **single threades** the executed command, so<u> two commands sent to redis will be executed after their order in queue</u>.
<font color="#ff0000">Redis</font> is<u> mostly single threaded</u>, however there are certain threaded operations such as `UNLINK`, **slow I/O accesses** and other things that are performed on <u>side threads</u>.