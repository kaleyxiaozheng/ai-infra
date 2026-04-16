<details>
<summary> Commands and Arguments in Docker</summary>

Containers are not meant to host an operating system, they are meant to run a specific task or process, such as to host an instance of a web server or application server, or a databas, or simply to carry out some kind of computation or analysis. Once the task is complete, the container exists.  

A container only lives as long as the process inside it is alive.

__Who defines what process is run within the container?__
> &#x1F449; CMD stands for command that defines the program that will be run within the container when ti starts.

__To build a custom Ubuntu image__
```
# new image will run Ubuntu container and sleep 5 seconds

FROM Ubuntu
CMD sleep 5
```

```shell
$ docker build -t ubuntu-sleeper .
$ docker run ubuntu-sleeper
```

__Two ways to add parameters to CMD__
| command and param | json pattern |
| :--- | :--- |
| `CMD` command param1 | `CMD` ["command", "param1"]
| `CDM` sleep 5 | `CMD` ["sleep", "5"]

However, if we want to just input the number of seconds the container should sleep, and sleep command should be invoked automatically, for example:
```
docker run ubuntu-sleeper 10
```
We define `ENTRYPOINT` in the DOCKERFILE
```
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```
```
$ docker run --name ubuntu-sleeper ubuntu-sleep 10
```
ENTRYPOINT defines the executable command, CMD provides default args as in.

![image](./img/pod%20coresponding%20fileds%20in%20docker%20file.png)

</details>