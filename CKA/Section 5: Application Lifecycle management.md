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

<details>
<summary> Configuring ConfigMaps in Applications</summary>

We store environment data into various pod definition files, and it is difficult to manage. We can take this information out of the Pod definition file and manage it centrally using ConfigMaps. `ConfigMaps are used to pass configuration data in the form of key value pairs in Kubernetes.` 

When a pod is created, injrect the configmap into the pod so the key value pairs are avaialble as environment variables for the application hosted inside the container in the pod.

__How to inject the configMap into the pod?__
> 👉 There are three ways to inject configMap.
>
>> 1️⃣ inject configMap as a single environment variable, using `env` in pod definition file
>>
>> 2️⃣ inject configMap as an environment variable, using `envFrom` in pod definition file
>>
>> 3️⃣ inject configMap as files in a volume, using `volume` in pod definition file

</details>

<details>
<summary> Secrets </summary>

__How to inject the secret into the pod?__
> 👉 There are three ways to inject secret.
>
>> 1️⃣ inject secret as a single environment variable, using `env` in pod definition file
>>
>> 2️⃣ inject secret as an environment variable, using `envFrom` in pod definition file
>>
>> 3️⃣ inject secret as files in a volume, using `volume` in pod definition file
>>
>>> If we mount the secret as a volume in the pod, each attribute in the secret is created as a file with the value of the secret as its content inside the container.

```
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bX1zcWw=
  DB_User: cm9vdA==
  DB_Password: cGFzd3JK
```

Mount the above secret
```
volumes:
- name: app-secret-volume
  secret:
    secretName: app-secret
```

we can see there are three files created inside the container
```
$ ls /opt/app-secret-volumes
  DB_host      DB_Password      DB_User

$ cat /opt/app-secret-volumes/DB_Password
  password
```

</details>