# instal-istio-1.8

## 使用kind创建两个k8s集群 
```shell
./setup.sh cluster01 6443
./setup.sh cluster02 6444
```

## 安装istio
```shell
./install.sh
```

## 验证跨集群流量

```bash
while true; do
kubectl exec --context=kind-cluster01 -n sample -c sleep \
    "$(kubectl get pod --context=kind-cluster01 -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')" \
    -- curl -s helloworld.sample:5000/hello
done
```

Repeat this request several times and verify that the HelloWorld version should toggle between v1 and v2:

```bash
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v1, instance: helloworld-v1-578dd69f69-sbvc9
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v1, instance: helloworld-v1-578dd69f69-sbvc9
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
```

Now repeat this process from the Sleep pod on cluster2:

```bash
while true; do
kubectl exec --context=kind-cluster02 -n sample -c sleep \
    "$(kubectl get pod --context=kind-cluster02 -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')" \
    -- curl -s helloworld.sample:5000/hello
done

```

Repeat this request several times and verify that the HelloWorld version should toggle between v1 and v2:

```bash
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v1, instance: helloworld-v1-578dd69f69-sbvc9
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v2, instance: helloworld-v2-776f74c475-h7rd9
Hello version: v1, instance: helloworld-v1-578dd69f69-sbvc9
```

## TODO
待验证 

## Reference

[istio-1.8](https://github.com/synuwxy/istio-1.8)

[istio-officials-website-kind](https://istio.io/latest/zh/docs/setup/platform-setup/kind/)

[setupkind.sh](https://raw.githubusercontent.com/istio/istio/release-1.14/samples/kind-lb/setupkind.sh)
