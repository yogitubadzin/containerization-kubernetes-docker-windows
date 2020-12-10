1.Create folder, i.e.: "d:\kubernetes".
2.Install kubernetes from following link:
  https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows
-If you have git installed, you can find curl here: 
 "C:\Program Files\Git\mingw64\bin\curl.exe"
-To run curl from the command line
a) Right-hand-click on "My Computer" icon
b) Select Properties
c) Click 'Advanced system settings' link
d) Go to tab [Advanced] - 'Environment Variables' button
e) Under System variable select 'Path' and Edit button
f) Add a semicolon followed by the path to where you placed your curl.exe (e.g. ;D:\software\curl),
   in our case "C:\Program Files\Git\mingw64\bin\curl.exe"
-another option is to install kubertness via choco:
 "choco install kubernetes-cli"

Please remember about creatig config in .kube directory.
3.Install minikube
-choco install minikube
-minikube start
-minikube start --driver=docker
-minikube config set driver docker

Now, run any kubectl command, if you have the deployment yaml file then,...
-kubectl apply -f <file path with name>
4.Run command:
-kubectl apply -f kubernetes-conf
5.You can open kubernetes dashboard in web browser by command:
-minikube dashboard
On my local machine is under dashboard:
-http://127.0.0.1:64098/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/overview?namespace=default
6.Commands:
a)$ kubectl get po -A
NAMESPACE              NAME                                         READY   STATUS             RESTARTS   AGE
default                client-react-deployment-79df79769b-4pjsx     0/1     ImagePullBackOff   0          4m31s
default                client-react-deployment-79df79769b-p7hsr     0/1     ImagePullBackOff   0          4m31s
default                client-react-deployment-79df79769b-wsxf5     0/1     ImagePullBackOff   0          4m31s
default                nodejs-server-deployment-86cd544669-4vtd8    0/1     ImagePullBackOff   0          4m31s
default                nodejs-server-deployment-86cd544669-7xknn    0/1     ImagePullBackOff   0          4m31s
default                nodejs-server-deployment-86cd544669-t5qdp    0/1     ImagePullBackOff   0          4m31s
default                postgres-deployment-7476bc47bd-85jzv         0/1     CrashLoopBackOff   5          4m31s
default                redis-deployment-58c4799ccc-hfv7k            1/1     Running            0          4m30s
default                worker-service-deployment-8487d8b97f-2kcs5   0/1     ImagePullBackOff   0          4m30s
kube-system            coredns-f9fd979d6-tnk9j                      1/1     Running            0          6m47s
kube-system            etcd-minikube                                1/1     Running            0          6m51s
kube-system            kube-apiserver-minikube                      1/1     Running            0          6m51s
kube-system            kube-controller-manager-minikube             1/1     Running            0          6m51s
kube-system            kube-proxy-fdm62                             1/1     Running            0          6m47s
kube-system            kube-scheduler-minikube                      1/1     Running            0          6m50s
kube-system            storage-provisioner                          1/1     Running            1          6m45s
kubernetes-dashboard   dashboard-metrics-scraper-c95fcf479-blp2r    1/1     Running            0          3m16s
kubernetes-dashboard   kubernetes-dashboard-584f46694c-qwrzx        1/1     Running            0          3m16s

b)Deploy on port 8080:
$ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
deployment.apps/hello-minikube created

$ kubectl expose deployment hello-minikube --type=NodePort --port=8080
service/hello-minikube exposed

$ kubectl get services hello-minikube
NAME             TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
hello-minikube   NodePort   10.102.166.133   <none>        8080:31040/TCP   60s

c)Deploy on Load Balancer:
$ kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4
deployment.apps/balanced created

$ kubectl expose deployment balanced --type=LoadBalancer --port=8080
service/balanced exposed

In other command window:
$ minikube tunnel
* Starting tunnel for service balanced.

$ kubectl get services balanced
NAME       TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
balanced   LoadBalancer   10.103.241.114   127.0.0.1     8080:32711/TCP   3m8s
----------------------------------
