# service-mesh-demo

```
kubectl apply -f webapp-color
```
```
$ kubectl get pod,svc -o wide
NAME                                   READY   STATUS    RESTARTS   AGE   IP          NODE                                     NOMINATED NODE   READINESS GATES
pod/webapp-color-v1-67865b64c9-gvjpc   2/2     Running   0          34m   10.4.3.8    gke-central-default-pool-6490da94-r9g4   <none>           <none>
pod/webapp-color-v2-776b7b5757-h4nww   2/2     Running   0          33m   10.4.0.12   gke-central-default-pool-6490da94-5dp2   <none>           <none>

NAME                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE   SELECTOR
service/kubernetes         ClusterIP   34.118.224.1    <none>        443/TCP    58m   <none>
service/webapp-color-svc   ClusterIP   34.118.239.77   <none>        8080/TCP   36m   app=webapp-color

$ kubectl get ep webapp-color-svc
NAME               ENDPOINTS                      AGE
webapp-color-svc   10.4.0.12:8080,10.4.3.8:8080   35m
```
```
$ kubectl get gateway,virtualservice,destinationrule -A
NAME                                               AGE
gateway.networking.istio.io/webapp-color-gateway   21m

NAME                                                    GATEWAYS                   HOSTS   AGE
virtualservice.networking.istio.io/webapp-color-route   ["webapp-color-gateway"]   ["*"]   21m

NAME                                                           HOST   AGE
destinationrule.networking.istio.io/webapp-color-destination   *      21m
```
```
$ kubectl get svc istio-ingressgateway -n istio-system
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                                                                      AGE
istio-ingressgateway   LoadBalancer   34.118.239.135   35.204.33.90   15021:31215/TCP,80:32149/TCP,443:31760/TCP,15012:31691/TCP,15443:30857/TCP   36m
```
```
# Test:
export GATEWAY_URL=35.204.33.90
$ curl http://{GATEWAY_URL}
```
